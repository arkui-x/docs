# 插件子系统适配指南

本文档提供 ArkUI-X 插件子系统适配的完整指南，包括架构原则、实现规则、检查清单和参考实现。

## 目录

- [概述](#概述)
- [适配模式对比](#适配模式对比)
- [插件架构](#插件架构)
- [核心规则](#核心规则)
- [实现步骤](#实现步骤)
- [构建系统配置](#构建系统配置)
- [参考实现](#参考实现)
- [检查清单](#检查清单)
- [常见问题](#常见问题)

## 概述

### 什么是插件适配

插件适配是指将 OpenHarmony 的子系统 API（如 `@ohos.net.http`、`@ohos.data.preferences` 等）适配到 Android 和 iOS 平台的过程。

### 适配范围

| 模块类型 | 示例 API | 适配方式 |
|---------|---------|---------|
| **网络** | @ohos.net.http, @ohos.net.socket | 插件适配 |
| **存储** | @ohos.data.preferences, @ohos.data.relationalStore | 插件适配 |
| **文件** | @ohos.file.fs | 插件适配 |
| **多媒体** | @ohos.multimedia.image, @ohos.multimedia.player | 插件适配 |
| **传感器** | @ohos.sensor | 插件适配 |
| **设备** | @ohos.device.info | 插件适配 |

### 适用条件

**需要插件适配**：
- 子系统级别的功能模块
- 暴露 @ohos.* API 给应用层
- 需要调用平台原生能力（网络、存储、传感器等）

**需要框架适配**（不使用插件）：
- 窗口管理
- 输入处理
- 字体渲染
- 键盘管理

## 适配模式对比

ArkUI-X 插件适配支持两种架构模式，根据具体场景选择：

### 模式 1: 独立实现模式 (Independent Implementation)

**理念**: 完全独立实现，零 OHOS 依赖

**架构**:
```
ArkTS API → NAPI (plugins/ 独立) → 接口 (plugins/) → 平台适配器
```

**特点**:
- ✅ **零 OHOS 依赖** - 完全独立
- ✅ **最小代码体积** - 仅包含必要功能
- ✅ **平台优化** - 直接调用原生 API
- ❌ **API 偏差风险** - 可能与 OHOS API 不一致
- ❌ **重复维护** - 独立 NAPI 层
- ❌ **需手动同步** - OHOS 更改不会自动传播

**代码量分布**:
- NAPI 绑定: ~2000 行（自定义）
- 平台适配器: ~500 行/平台
- 总计: ~3000 行（100% 自定义）

**适用场景**:
1. OHOS 模块**重度依赖平台特定功能**（如 `@ohos.telephony`）
2. 仅需**最小功能集**（降级场景）
3. 平台 API 与 OHOS **显著不同**
4. 二进制体积是**关键约束**

**参考**: `plugins/net/http` (独立实现)

### 模式 2: OHOS 代码复用模式 (OHOS Reuse) ⭐ 推荐

**理念**: 最大化复用 OHOS 代码，仅实现最小平台适配器

**架构**:
```
ArkTS API → NAPI (OHOS 复用) → 数据结构 (OHOS 复用) → 接口 → 平台适配器
```

**特点**:
- ✅ **~95% 代码复用** - 仅适配器是自定义的
- ✅ **API 兼容性** - 100% 与 OHOS 一致
- ✅ **自动同步** - OHOS 更改自动传播
- ✅ **最小维护** - 仅 ~500 行/平台
- ⚠️ **OHOS 依赖** - 需要 foundation 代码
- ⚠️ **构建复杂度** - 更多 GN 配置

**代码量分布**:
- OHOS NAPI 绑定: ~2000 行（复用）
- OHOS 数据结构: ~1000 行（复用）
- 平台适配器: ~500 行/平台
- 总计: ~3000 行（5% 自定义，95% 复用）

**适用场景**:
1. OHOS 模块**最小平台特定逻辑**
2. 需要 **API 兼容性**
3. 需要**自动更新**
4. 大多数标准数据结构模块

**参考**: `plugins/pasteboard` (OHOS 复用)

### 详细对比表

| 对比项 | 模式 1: 独立实现 | 模式 2: OHOS 复用 |
|-------|-----------------|-----------------|
| **代码量** | 100% 自定义 (~3000 行) | 5% 自定义，95% 复用 (~150 行新增) |
| **二进制大小** | 较大（重复 NAPI） | 较小（共享 NAPI） |
| **API 兼容性** | ⚠️ 可能偏离 | ✅ 100% 兼容 |
| **OHOS 更新** | ❌ 手动同步 | ✅ 自动 |
| **维护成本** | 高（完整 NAPI + 适配器） | 低（仅适配器） |
| **构建复杂度** | 简单 | 复杂（OHOS 依赖） |
| **平台优化** | ✅ 完全控制 | ⚠️ 受 OHOS 接口限制 |
| **学习曲线** | 中等 | 低（遵循 OHOS 模式） |
| **最适合** | 平台重度模块 | 数据重度模块 |

### 决策流程图

```
START: 适配新的 @ohos API
   ↓
OHOS 实现是否主要是平台无关的业务逻辑？
   ↓
   YES → 模块是否重度依赖 OHOS 特定服务？
           ↓
           NO → 使用模式 2: OHOS 代码复用 ⭐
           ↓
           YES → 能否提取平台无关部分？
                    ↓
                    YES → 模式 2 + 服务抽象
                    NO → 使用模式 1: 独立实现
   ↓
   NO → 使用模式 1: 独立实现
```

### 插件模式 (Plugin Pattern)

**架构**：
```
ArkTS API → NAPI (独立) → 业务逻辑 → 平台接口 → 平台实现
```

**特点**：
- ✅ 完全独立实现，不依赖 OHOS 代码
- ✅ 适合新 API，从零开始
- ✅ 平台隔离清晰（目录分离）
- ❌ NAPI 代码与 OHOS 重复
- ❌ 维护成本较高（两套 NAPI）

**适用场景**：
- 全新 API，OHOS 无对应实现
- 需要完全不同的实现逻辑
- 插件式架构，松耦合

### 适配器模式 (Adapter Pattern)

**架构**：
```
ArkTS API → NAPI (OHOS 共享) → 纯虚接口 → 平台适配器 (条件编译)
```

**特点**：
- ✅ 单一 NAPI 层，无代码重复
- ✅ OHOS 适配器是薄包装（100% 转发）
- ✅ 编译时多态，无运行时开销
- ✅ 最小化 OHOS 代码改动
- ❌ 依赖 OHOS 仓库代码结构
- ❌ 需要 OHOS 仓库协作

**适用场景**：
- 已有 OHOS 模块需要迁移
- OHOS 和 ArkUI-X 逻辑相似
- 需要最小化维护成本

### 选择决策树

```
是否有 OHOS 实现？
├─ 否 → 使用插件模式（模式 1）
└─ 是 → OHOS 逻辑是否可复用？
    ├─ 是 → 使用适配器模式（模式 2）⭐
    └─ 否 → 使用插件模式（模式 1）
```

### 模式切换示例

**场景**：适配 `@ohos.pasteboard`

**插件模式** (如果选择)：
```
plugins/pasteboard/
├── napi/pasteboard_module.cpp      # 独立 NAPI
├── common/pasteboard_core.cpp      # 业务逻辑
├── android/pasteboard_android.cpp  # Android 实现
└── ios/pasteboard_ios.mm           # iOS 实现
```

**适配器模式** (推荐)：
```
OHOS 仓库:
foundation/distributeddatamgr/pasteboard/
├── interfaces/kits/napi/pasteboard_module.cpp  # NAPI (共享)
├── interfaces/kits/napi/pasteboard_adapter.h   # 纯虚接口
└── services/adapter/pasteboard_adapter_ohos.cpp # OHOS 薄包装

Plugin 仓库:
plugins/pasteboard/
├── adapter/android/pasteboard_adapter_android.cpp  # JNI 调用
└── adapter/ios/pasteboard_adapter_ios.mm           # ObjC++ 调用
```

### 适配器模式详细说明

#### 架构层次

```
┌─────────────────────────────────────────────────────────────────┐
│  第 1 层: ArkTS API 层                                           │
│  interface/sdk-js/api/@ohos.xxx.d.ts                            │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 2 层: NAPI 层 (OHOS 仓库维护，跨平台共享)                     │
│  foundation/xxx/yyy/interfaces/kits/napi/xxx_module.cpp        │
│  - 参数解析                                                      │
│  - 类型转换                                                      │
│  - 调用适配器接口                                                │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 3 层: 平台接口层 (纯虚 C++ 接口)                              │
│  foundation/xxx/yyy/interfaces/kits/napi/xxx_adapter.h         │
│  - 纯虚接口定义                                                  │
│  - 工厂函数声明                                                  │
└─────────────────────────────────────────────────────────────────┘
                           ↓
        ┌─────────────────┴─────────────────┐
        ↓                                   ↓
┌─────────────────────┐         ┌─────────────────────┐
│  第 4 层: OHOS 实现   │         │  第 4 层: Android   │
│  (薄包装，100%转发)  │         │  - JNI Bridge       │
│  xxx_adapter_ohos.cpp│         │  - Java/Kotlin Impl │
└─────────────────────┘         └─────────────────────┘
                                          ↓
                              ┌─────────────────────┐
                              │  第 4 层: iOS 实现   │
                              │  - ObjC++ Bridge     │
                              │  - ObjC/Swift Impl   │
                              └─────────────────────┘
```

#### 关键原则

1. **单一 NAPI 层**：NAPI 代码只在 OHOS 仓库维护，Android/iOS 共享
2. **OHOS 薄包装**：OHOS 适配器必须是 100% 转发，零逻辑改动
3. **编译时多态**：使用条件编译选择平台实现，无虚函数表开销
4. **Helper 宏**：提供迁移辅助宏，简化代码替换

#### Helper 宏示例

```cpp
// xxx_adapter_helper.h
#ifdef OHOS_PLATFORM
    #define XXX_ADAPTER() (GetPasteboardClient())
#elif defined(ANDROID_PLATFORM)
    #define XXX_ADAPTER() (GetPasteboardAdapterAndroid())
#elif defined(IOS_PLATFORM)
    #define XXX_ADAPTER() (GetPasteboardAdapterIOS())
#endif
```

#### 迁移步骤

1. **定义纯虚接口**：创建 `xxx_adapter.h`
2. **实现 OHOS 薄包装**：创建 `xxx_adapter_ohos.cpp`
3. **实现平台适配器**：在 Plugin 仓库创建 Android/iOS 实现
4. **修改 NAPI 层**：使用 Helper 宏替换直接调用
5. **配置构建系统**：添加条件编译规则

#### 参考

- **完整示例**：`plugins/pasteboard/ADAPTER_ARCHITECTURE.md`
- **迁移指南**：`foundation/distributeddatamgr/pasteboard/interfaces/kits/napi/ADAPTER_MIGRATION.md`
- **Skill 工具**：使用 `/skill-platform-api` 自动生成适配器代码

### GN 构建配置规则

根据选择的架构模式，GN 配置有显著差异。

#### 模式 1: 独立实现 GN 配置

**plugins/{module}/{module}.gni**:
```gni
# 无 OHOS 源文件，仅自定义代码
{module}_sources = [
  "napi/{module}_napi.cpp",           # 自定义 NAPI 绑定
  "common/{module}_data.cpp",         # 自定义数据结构
]

{module}_android_sources = [
  "android/{module}_adapter_android.cpp",
]

{module}_ios_sources = [
  "ios/{module}_adapter_ios.mm",
]

# 无 OHOS 包含路径
{module}_include_dirs = [
  "plugins/{module}",
  "plugins/{module}/napi",
  "plugins/interfaces/native",
]

# 无 OHOS 依赖
{module}_android_deps = [
  "//plugins/interfaces/native:ace_plugin_util_android",
]
```

**关键规则**:
- ✅ 零 OHOS 依赖
- ✅ 自定义 NAPI 源文件
- ✅ 简单的依赖关系

#### 模式 2: OHOS 复用 GN 配置

**plugins/{module}/{module}.gni**:
```gni
# OHOS 模块根目录（用于代码复用）
{MODULE}_OHOS_ROOT = "//foundation/{ohos_module_path}"

# OHOS NAPI 绑定（100% 复用）
{module}_sources = [
  # OHOS NAPI 绑定 - 直接复用
  "${MODULE}_OHOS_ROOT/interfaces/kits/napi/src/napi_init.cpp",
  "${MODULE}_OHOS_ROOT/interfaces/kits/napi/src/napi_{module}.cpp",
  "${MODULE}_OHOS_ROOT/interfaces/kits/napi/src/napi_{module}_common.cpp",
  # ... 所有 OHOS NAPI 文件（11+ 个文件）

  # OHOS innerkits（数据结构实现）
  "${MODULE}_OHOS_ROOT/frameworks/innerkits/src/{module}_client.cpp",
  "${MODULE}_OHOS_ROOT/frameworks/innerkits/src/{module}_data.cpp",
  # ... 所有 OHOS 数据结构文件（6+ 个文件）
]

# 平台特定源文件（仅平台适配层）
{module}_android_sources = [
  "android/{module}_adapter_android.cpp",  # ~500 行
]

{module}_ios_sources = [
  "ios/{module}_adapter_ios.mm",  # ~500 行
]

# 包含 OHOS 头文件
{module}_include_dirs = [
  "plugins/{module}",
  # OHOS 包含路径（复用的关键）
  "${MODULE}_OHOS_ROOT/interfaces/kits/napi/include",
  "${MODULE}_OHOS_ROOT/interfaces/kits/napi/src",
  "${MODULE}_OHOS_ROOT/frameworks/innerkits/include",
  # Plugin 接口
  "plugins/interfaces/native",
]

# 依赖必须包含 OHOS
{module}_android_deps = [
  "plugins/interfaces/native:ace_plugin_util_android",
  "${MODULE}_OHOS_ROOT/interfaces/kits/napi:{module}_client",  # 关键
]

{module}_ios_deps = [
  "plugins/interfaces/native:ace_plugin_util_ios",
  "${MODULE}_OHOS_ROOT/interfaces/kits/napi:{module}_client",  # 关键
]

# iOS 框架
{module}_ios_frameworks = [
  "Foundation.framework",
  "UIKit.framework",
  "MobileCoreServices.framework",
]
```

**关键规则**:
- ✅ OHOS 源文件列在 `{module}_sources`
- ✅ OHOS 包含路径在 `{module}_include_dirs`
- ✅ OHOS 依赖在 `{module}_android_deps` 和 `{module}_ios_deps`
- ✅ 平台适配器仅 ~500 行
- ✅ 实现 ~95% 代码复用

### GN 构建配置最佳实践

#### 规则 1: 包含路径必须匹配依赖

```gni
# ✅ 正确：包含路径与依赖匹配
include_dirs = [
  "plugins/{module}",
  "{MODULE}_OHOS_ROOT/interfaces/kits/napi/include",  # 有对应依赖
]
deps = [
  "{MODULE}_OHOS_ROOT/interfaces/kits/napi:{module}_client",  # 匹配包含
]

# ❌ 错误：包含路径无对应依赖
include_dirs = [
  "{MODULE}_OHOS_ROOT/.../include",  # 无对应依赖
]
deps = []  # 缺少依赖！
```

#### 规则 2: 使用 external_deps 引用系统库

```gni
# ✅ 正确：使用 external_deps 引用系统级库
external_deps = [
  "hilog:libhilog",
  "napi:ace_napi",
]

# ❌ 错误：不要使用 deps 引用外部库
deps = [
  "//foundation/hiviewdfx/hilog:libhilog",  # 错误路径
]
```

#### 规则 3: iOS 框架单独配置

```gni
# ✅ 正确：iOS 框架在独立配置中
{module}_ios_frameworks = [
  "Foundation.framework",
  "UIKit.framework",
  "MobileCoreServices.framework",
]

# 在 iOS BUILD.gn 中
ohos_source_set("{module}_ios") {
  frameworks = {module}_ios_frameworks
}
```

#### 规则 4: 条件编译选择平台适配器

```gni
# OHOS 仓库 BUILD.gn
ohos_shared_library("{module}_napi") {
  sources = [
    "napi/src/napi_{module}.cpp",
    # ... 其他 NAPI 源文件
  ]

  # 条件编译适配器
  if (!is_arkuix_target) {
    # OHOS: 使用 OHOS 适配器
    sources += [ "../../services/adapter/{module}_adapter_ohos.cpp" ]
  } else {
    # ArkUI-X: 使用 Plugin 仓库适配器
    if (is_android) {
      sources += [ "//plugins/{module}/adapter/android:{module}_adapter_android.cpp" ]
    } else if (is_ios) {
      sources += [ "//plugins/{module}/adapter/ios:{module}_adapter_ios.mm" ]
    }
  }
}
```

### 完整示例：Pasteboard (模式 2)

**plugins/pasteboard/pasteboard.gni**:
```gni
import("//build/ohos.gni")

# 模块版本
pasteboard_version = {
  major = 2
  minor = 0
  patch = 0
}

# 模块根目录
PASTEBOARD_ROOT = "//plugins/pasteboard"

# OHOS pasteboard 根目录（用于代码复用）
PASTEBOARD_OHOS_ROOT = "//foundation/distributeddatamgr/pasteboard"

# 公共包含目录
pasteboard_include = [
  "$PASTEBOARD_ROOT",
  "$PASTEBOARD_ROOT/android",
  "$PASTEBOARD_ROOT/ios",
  # OHOS pasteboard 接口（复用）
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/include",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src",
  "$PASTEBOARD_OHOS_ROOT/frameworks/innerkits/include",
  # Plugin 接口
  "//plugins/interfaces",
  "//plugins/interfaces/native",
  # Third party
  "//third_party/bounds_checking_function/include",
  # Logging
  "//foundation/hiviewdfx/hilog/interfaces/native/innerkits/include",
]

# 公共源文件（复用 OHOS NAPI 实现）
pasteboard_sources = [
  # OHOS NAPI 绑定（直接复用）
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_init.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_pasteboard.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_pasteboard_common.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_pastedata.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_pastedata_record.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_pasteboard_observer.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_pasteboard_progress_signal.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_systempasteboard.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/async_call.cpp",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi/src/napi_data_utils.cpp",
  # OHOS innerkits（数据结构实现）
  "$PASTEBOARD_OHOS_ROOT/frameworks/innerkits/src/pasteboard_client.cpp",
  "$PASTEBOARD_OHOS_ROOT/frameworks/innerkits/src/paste_data.cpp",
  "$PASTEBOARD_OHOS_ROOT/frameworks/innerkits/src/paste_data_record.cpp",
  "$PASTEBOARD_OHOS_ROOT/frameworks/innerkits/src/pasteboard_utils.cpp",
  "$PASTEBOARD_OHOS_ROOT/frameworks/innerkits/src/pasteboard_copy.cpp",
  "$PASTEBOARD_OHOS_ROOT/frameworks/innerkits/src/convert_utils.cpp",
]

# 平台特定源文件（仅平台适配层）
pasteboard_android_sources = [
  "android/pasteboard_adapter_android.cpp",  # Android 平台适配器
]

pasteboard_ios_sources = [
  "ios/pasteboard_adapter_ios.mm",  # iOS 平台适配器
]

# Android 依赖
pasteboard_android_deps = [
  "//plugins/interfaces/native:ace_plugin_util_android",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi:pasteboard_client",  # OHOS 依赖
]

# iOS 依赖
pasteboard_ios_deps = [
  "//plugins/interfaces/native:ace_plugin_util_ios",
  "$PASTEBOARD_OHOS_ROOT/interfaces/kits/napi:pasteboard_client",  # OHOS 依赖
]

# iOS 框架
pasteboard_ios_frameworks = [
  "Foundation.framework",
  "UIKit.framework",
  "MobileCoreServices.framework",
]
```

**关键要点**:
1. ✅ OHOS 源文件列在 `pasteboard_sources`
2. ✅ OHOS 包含路径在 `pasteboard_include`
3. ✅ OHOS 依赖在 `pasteboard_android_deps` 和 `pasteboard_ios_deps`
4. ✅ 平台适配器仅 ~500 行
5. ✅ 实现 ~95% 代码复用

## 插件架构

### 5 层架构

```
┌─────────────────────────────────────────────────────────────────┐
│  第 1 层: ArkTS API 层                                           │
│  interface/sdk-js/api/@ohos.xxx.d.ts                            │
│  - TypeScript 类型定义                                           │
│  - 应用开发者使用                                                │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 2 层: NAPI 绑定层                                            │
│  plugins/xxx/napi/xxx_module.cpp                                │
│  - 参数解析                                                      │
│  - 类型转换                                                      │
│  - 异步处理                                                      │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 3 层: 业务逻辑层 (平台无关 C++)                               │
│  plugins/xxx/common/xxx_core.cpp                               │
│  - 核心算法                                                      │
│  - 状态管理                                                      │
│  - 平台无关逻辑                                                  │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 4 层: 平台接口层 (纯虚 C++ 接口)                              │
│  plugins/xxx/interface/xxx_interface.h                         │
│  - 纯虚接口定义                                                  │
│  - 平台抽象                                                      │
└─────────────────────────────────────────────────────────────────┘
                           ↓
        ┌─────────────────┴─────────────────┐
        ↓                                   ↓
┌─────────────────────┐         ┌─────────────────────┐
│  第 5 层: Android 实现 │         │  第 5 层: iOS 实现   │
│  - JNI Bridge        │         │  - ObjC++ Bridge     │
│  - Java/Kotlin Impl  │         │  - ObjC/Swift Impl   │
│  - AOSP APIs         │         │  - Apple Frameworks  │
└─────────────────────┘         └─────────────────────┘
```

### 目录结构

```
plugins/
├── plugin_lib.gni                    # 插件注册（必须更新）
├── http/                             # 示例：HTTP 插件
│   ├── BUILD.gn                      # 构建模板
│   ├── http.gni                      # 模块配置
│   ├── interface/                    # 平台无关接口
│   │   └── http_interface.h          # 纯虚接口
│   ├── napi/                         # NAPI 绑定层
│   │   └── http_module.cpp           # NAPI 实现
│   ├── common/                       # 共享 C++ 代码
│   │   └── http_core.cpp             # 业务逻辑
│   ├── android/                      # Android 平台
│   │   ├── BUILD.gn
│   │   ├── http_strategy_android.cpp # Android 策略
│   │   └── java/
│   │       └── jni/
│   │           └── http_jni.cpp      # JNI 桥接
│   └── ios/                          # iOS 平台
│       ├── BUILD.gn
│       ├── http_strategy_ios.mm      # iOS 策略
│       └── http_impl.mm              # iOS 实现
```

## 核心规则

### 核心实现规则（16 条）

#### 1. 分层架构

严格遵循 5 层架构，每层职责单一：

```
API 层 → NAPI 层 → 业务逻辑层 → 平台接口层 → 平台实现层
```

**正确示例**：
```cpp
// interface/http_interface.h - 平台接口层
class IHttpStrategy {
public:
    virtual void ExecuteRequest(const HttpRequest& request, Callback callback) = 0;
};

// android/http_strategy_android.cpp - Android 实现层
class AndroidHttpStrategy : public IHttpStrategy {
    void ExecuteRequest(const HttpRequest& request, Callback callback) override {
        // 调用 Java 实现
    }
};
```

**错误示例**：
```cpp
// ❌ NAPI 层直接调用 Java，跳过业务逻辑层
static napi_value Execute(napi_env env, napi_callback_info info) {
    // 直接调用 JNI，没有平台抽象
    jni_call_java();
}
```

#### 2. 平台隔离

**方式一：目录分离**（推荐）

```
plugins/xxx/
├── common/     # 平台无关代码
├── android/    # Android 特定代码
└── ios/        # iOS 特定代码
```

**方式二：条件编译**（用于小型差异）

```cpp
// common/xxx_core.cpp
#ifdef ANDROID_PLATFORM
    // Android 特定代码
#elif defined(IOS_PLATFORM)
    // iOS 特定代码
#endif
```

#### 3. 最小化 JNI 调用

**缓存 JNI 引用**：

```cpp
// ✅ 正确：缓存 JNI 引用
class JniCache {
private:
    static jmethodID midExecute_;
    static jobject httpService_;

public:
    static void Init(JNIEnv* env) {
        jclass clazz = env->FindClass("com/ohos/HttpService");
        midExecute_ = env->GetMethodID(clazz, "execute", "..."));
        httpService_ = env->NewGlobalRef(...);
    }
};

// ❌ 错误：每次都查找
void Execute() {
    jclass clazz = env->FindClass("com/ohos/HttpService");  // 性能差
    jmethodID mid = env->GetMethodID(clazz, "execute", "...");
}
```

**批量传递数据**：

```cpp
// ✅ 正确：一次性传递所有参数
struct RequestParams {
    std::string url;
    std::string method;
    std::map<std::string, std::string> headers;
    std::string body;
};
jni_execute(env, requestParams);

// ❌ 错误：多次 JNI 调用
jni_set_url(env, url);
jni_set_method(env, method);
jni_set_headers(env, headers);  // 多次跨语言调用
```

#### 4. 统一异步模型

**使用 Promise/Deferred 模式**：

```cpp
// ✅ 正确：统一异步处理
napi_value ExecuteAsync(napi_env env, napi_callback_info info) {
    napi_value promise;
    napi_deferred deferred;
    napi_create_promise(env, &promise, &deferred);

    // 异步执行
    auto callback = [env, deferred](Result result) {
        napi_value result_value;
        // 创建结果对象
        napi_resolve_deferred(env, deferred, result_value);
    };

    strategy_->Execute(request, callback);
    return promise;
}

// ❌ 错误：同步阻塞
napi_value Execute(napi_env env, napi_callback_info info) {
    Result result = strategy_->ExecuteSync(request);  // 阻塞线程
    return convert_to_napi(result);
}
```

#### 5. 模板化 GN 构建

**使用构建模板**：

```gni
# plugins/xxx/xxx.gni
template("xxx_plugin") {
  if (target_os == "android") {
    # Android 特定配置
  } else if (target_os == "ios") {
    # iOS 特定配置
  }

  # 通用配置
  sources = [
    "interface/xxx_interface.h",
    "common/xxx_core.cpp",
  ]
}
```

#### 6. 配置分离

**分离 .gni 和 BUILD.gn**：

```gni
# xxx.gni - 可重用配置
declare_args() {
  xxx_enable_feature = true
}

xxx_sources = [
  "interface/xxx_interface.h",
  "common/xxx_core.cpp",
]
```

```gni
# BUILD.gn - 构建规则
import("xxx.gni")

ohos_shared_library("xxx") {
  sources = xxx_sources
  deps = []
}
```

#### 7. 明确依赖声明

```gni
# BUILD.gn
ohos_shared_library("xxx") {
  deps = [
    "//foundation/arkui/napi:napi",           # 明确声明
    "//plugins/net/http:http_common",         # 明确声明
    "//third_party/curl:curl_shared",         # 明确声明
  ]

  external_deps = [
    "hilog_native:libhilog",                 # 外部依赖
  ]
}
```

#### 8. 接口抽象

**使用纯虚接口**：

```cpp
// ✅ 正确：纯虚接口
class IHttpStrategy {
public:
    virtual ~IHttpStrategy() = default;
    virtual void Execute(const Request& req, Callback cb) = 0;
    virtual void Cancel() = 0;
};

// ❌ 错误：具体实现作为接口
class HttpStrategy {
public:
    void Execute(const Request& req, Callback cb) {
        // 具体实现
    }
};
```

#### 9. 最小化条件编译

**优先使用文件分离**：

```
✅ 推荐：
plugins/xxx/
├── common/xxx_core.cpp      # 平台无关
├── android/xxx_android.cpp  # Android 特定
└── ios/xxx_ios.mm           # iOS 特定

❌ 避免：
plugins/xxx/common/xxx_core.cpp  # 包含大量 #ifdef ANDROID
```

#### 10. 统一错误处理

**定义平台无关错误码**：

```cpp
// common/error_codes.h
enum class ErrorCode {
    OK = 0,
    INVALID_PARAM = 401,
    PERMISSION_DENIED = 200,
    NETWORK_ERROR = 300,
};

// Android 实现
int AndroidMapToErrorCode(int javaError) {
    switch (javaError) {
        case JAVA_INVALID_PARAM: return static_cast<int>(ErrorCode::INVALID_PARAM);
        // ...
    }
}

// iOS 实现
int iOSMapToErrorCode(NSInteger nsError) {
    switch (nsError) {
        case NSURLErrorBadURL: return static_cast<int>(ErrorCode::INVALID_PARAM);
        // ...
    }
}
```

#### 11. 零拷贝数据传输

**使用 std::string_view 和移动语义**：

```cpp
// ✅ 正确：避免拷贝
void ProcessUrl(std::string_view url) {  // 不拷贝
    // 处理 URL
}

std::string BuildRequest() {
    Request req;
    // ...
    return req;  // RVO/移动语义
}

// ❌ 错误：不必要的拷贝
void ProcessUrl(std::string url) {  // 按值传递，拷贝
    // 处理 URL
}
```

#### 12. 智能指针生命周期

**使用 shared_ptr/unique_ptr**：

```cpp
// ✅ 正确：智能指针管理
class HttpClient {
    std::shared_ptr<IHttpStrategy> strategy_;
    std::unique_ptr<RequestContext> context_;
};

// ❌ 错误：手动管理
class HttpClient {
    IHttpStrategy* strategy_;  // 谁负责释放？
    RequestContext* context_;  // 内存泄漏风险
};
```

#### 13. 线程安全

**使用线程安全容器和互斥锁**：

```cpp
// ✅ 正确：线程安全
class RequestManager {
private:
    std::mutex mutex_;
    std::unordered_map<int, std::shared_ptr<Request>> requests_;

public:
    void AddRequest(int id, std::shared_ptr<Request> req) {
        std::lock_guard<std::mutex> lock(mutex_);
        requests_[id] = req;
    }
};

// ❌ 错误：非线程安全
class RequestManager {
private:
    std::unordered_map<int, Request*> requests_;  // 竞态条件
};
```

#### 14. 符号可见性控制

**只导出必要的符号**：

```cpp
// ✅ 正确：明确导出
#define PLUGIN_EXPORT __attribute__((visibility("default")))

PLUGIN_EXPORT void* CreateModule() {
    return new MyModule();
}

// 内部函数隐藏
namespace {
    void InternalHelper() {  // 匿名命名空间，默认隐藏
        // ...
    }
}

// ❌ 错误：所有符号都导出
__attribute__((visibility("default"))) void EverythingExported();
```

**BUILD.gn 配置**：
```gni
ohos_shared_library("my_plugin") {
  # 默认隐藏所有符号
  cflags = [ "-fvisibility=hidden" ]

  # 只明确标记的符号才导出
  cflags_cc = [ "-fvisibility-inlines-hidden" ]
}
```

#### 15. JNI 线程附加

**确保 JNI 调用前线程已附加**：

```cpp
// ✅ 正确：自动线程附加
class JniThreadAttacher {
private:
    JNIEnv* env_;
    bool attached_;

public:
    JniThreadAttacher(JavaVM* vm) : attached_(false) {
        if (vm->GetEnv(reinterpret_cast<void**>(&env_), JNI_VERSION_1_6) == JNI_EDETACHED) {
            vm->AttachCurrentThread(&env_, nullptr);
            attached_ = true;
        }
    }

    ~JniThreadAttacher() {
        if (attached_) {
            vm_->DetachCurrentThread();
        }
    }

    JNIEnv* GetEnv() { return env_; }
};

void AndroidCallback() {
    JniThreadAttacher attacher(vm_);
    JNIEnv* env = attacher.GetEnv();
    // 安全使用 env
}

// ❌ 错误：未检查线程附加
void AndroidCallbackUnsafe() {
    JNIEnv* env;  // 可能是无效指针
    vm->GetEnv(reinterpret_cast<void**>(&env_), JNI_VERSION_1_6);
    // 直接使用可能崩溃
}
```

#### 16. 平台特定错误映射

**统一错误码，平台特定映射**：

```cpp
// common/error_codes.h
enum class PluginErrorCode {
    OK = 0,
    INVALID_PARAM = 401,
    IO_ERROR = 300,
    PERMISSION_DENIED = 201,
};

// Android 错误映射
PluginErrorCode MapAndroidError(int javaError) {
    switch (javaError) {
        case EINVAL: return PluginErrorCode::INVALID_PARAM;
        case EPERM: return PluginErrorCode::PERMISSION_DENIED;
        case EIO: return PluginErrorCode::IO_ERROR;
        default: return PluginErrorCode::IO_ERROR;
    }
}

// iOS 错误映射
PluginErrorCode MapIOSError(NSError* nsError) {
    if ([nsError.domain isEqualToString:NSURLErrorDomain]) {
        switch (nsError.code) {
            case NSURLErrorBadURL:
                return PluginErrorCode::INVALID_PARAM;
            case NSURLErrorNoPermissionsToReadFile:
                return PluginErrorCode::PERMISSION_DENIED;
            default:
                return PluginErrorCode::IO_ERROR;
        }
    }
    return PluginErrorCode::IO_ERROR;
}
```

## 实现步骤

### 步骤 1: 创建插件目录结构

```bash
# 1. 创建插件目录
mkdir -p plugins/new_feature

# 2. 创建子目录
cd plugins/new_feature
mkdir -p interface      # 平台无关接口
mkdir -p napi          # NAPI 绑定
mkdir -p common        # 共享业务逻辑
mkdir -p android       # Android 实现
mkdir -p ios          # iOS 实现
```

### 步骤 2: 定义平台接口

```cpp
// plugins/new_feature/interface/new_feature_interface.h
#ifndef PLUGIN_NEW_FEATURE_INTERFACE_H
#define PLUGIN_NEW_FEATURE_INTERFACE_H

#include <functional>
#include <string>

struct Request {
    std::string data;
    int timeout;
};

struct Response {
    int code;
    std::string result;
};

using Callback = std::function<void(const Response&)>;

class INewFeatureStrategy {
public:
    virtual ~INewFeatureStrategy() = default;
    virtual void Execute(const Request& req, Callback cb) = 0;
    virtual void Cancel() = 0;
};

#endif  // PLUGIN_NEW_FEATURE_INTERFACE_H
```

### 步骤 3: 实现业务逻辑

```cpp
// plugins/new_feature/common/new_feature_core.cpp
#include "interface/new_feature_interface.h"
#include <memory>

class NewFeatureCore {
private:
    std::shared_ptr<INewFeatureStrategy> strategy_;

public:
    void SetStrategy(std::shared_ptr<INewFeatureStrategy> strategy) {
        strategy_ = strategy;
    }

    void Execute(const Request& req, Callback cb) {
        if (strategy_) {
            strategy_->Execute(req, cb);
        }
    }
};
```

### 步骤 4: 实现 Android 平台

```cpp
// plugins/new_feature/android/new_feature_android.cpp
#include "interface/new_feature_interface.h"
#include <jni.h>

class AndroidNewFeatureStrategy : public INewFeatureStrategy {
private:
    JNIEnv* env_;
    jobject java_obj_;

public:
    void Execute(const Request& req, Callback cb) override {
        // 调用 Java 实现
        jmethodID mid = /* 获取方法 ID */;
        jobject result = env_->CallObjectMethod(java_obj_, mid, /* 参数 */);

        // 转换结果并回调
        Response resp;
        cb(resp);
    }

    void Cancel() override {
        // 取消操作
    }
};
```

### 步骤 5: 实现 iOS 平台

```objc
// plugins/new_feature/ios/new_feature_ios.mm
#import "interface/new_feature_interface.h"
#import <Foundation/Foundation.h>

class IOSNewFeatureStrategy : public INewFeatureStrategy {
public:
    void Execute(const Request& req, Callback cb) override {
        // 调用 Objective-C 实现
        NSURLSession* session = [NSURLSession sharedSession];
        // ...

        dispatch_async(dispatch_get_main_queue(), ^{
            Response resp;
            cb(resp);
        });
    }

    void Cancel() override {
        // 取消操作
    }
};
```

### 步骤 6: 添加 NAPI 绑定

```cpp
// plugins/new_feature/napi/new_feature_module.cpp
#include "napi/native_api.h"
#include "interface/new_feature_interface.h"

static napi_value Execute(napi_env env, napi_callback_info info) {
    // 解析参数
    size_t argc = 1;
    napi_value args[1];
    napi_get_cb_info(env, info, &argc, args, nullptr, nullptr);

    // 创建 Promise
    napi_value promise;
    napi_deferred deferred;
    napi_create_promise(env, &promise, &deferred);

    // 调用业务逻辑
    Request req;
    auto callback = [env, deferred](const Response& resp) {
        napi_value result;
        // 转换为 NAPI value
        napi_resolve_deferred(env, deferred, result);
    };

    // 执行请求
    core_->Execute(req, callback);

    return promise;
}

static napi_value Init(napi_env env, napi_value exports) {
    napi_property_descriptor props[] = {
        {"execute", nullptr, Execute, nullptr, nullptr, nullptr, napi_default, nullptr}
    };
    napi_define_properties(env, exports, sizeof(props)/sizeof(props[0]), props);
    return exports;
}

NAPI_MODULE(addon, Init)
```

## 构建系统配置

### ⚠️ 强制性验证清单

**关键**: 生成适配器代码后，必须完成以下 4 个构建系统配置。没有这些，模块将**无法编译**。

### 配置文件清单

添加新插件时，必须更新以下 4 个配置文件：

| 配置文件 | 作用 | 状态 | 影响评估 |
|---------|-----|------|---------|
| `plugins/plugin_lib.gni` | 插件注册 | ⚠️ **手动** | 不更新则模块被排除编译 |
| `interface/sdk/plugins/api/apiConfig.json` | API 工具链配置 | ⚠️ **手动** | 不更新则构建系统找不到库 |
| `build_plugins/sdk/arkui_cross_sdk_description_std.json` | 跨平台 SDK 描述 | ⚠️ **手动** | 不更新则不会包含在 SDK 中 |
| `plugins/new_feature/BUILD.gn` | 构建规则 | ✅ **自动** | 自动生成 |

### ✅ 检查项 1: GN 构建配置

**状态**: Skill 工具**自动生成**

**内容**: 在所有必需位置创建 BUILD.gn 文件

**OHOS 仓库文件**:
- [ ] `foundation/{module}/services/adapter/BUILD.gn`
- [ ] `foundation/{module}/interfaces/jskits/{module}/BUILD.gn` (已更新)

**Plugin 仓库文件**:
- [ ] `plugins/{module}/adapter/android/BUILD.gn`
- [ ] `plugins/{module}/adapter/ios/BUILD.gn`
- [ ] `plugins/{module}/BUILD.gn`
- [ ] `plugins/{module}/{module}.gni`

**验证命令**:
```bash
# 列出模块的所有 BUILD.gn 文件
find foundation/{module} -name "BUILD.gn"
find plugins/{module} -name "BUILD.gn" -o -name "*.gni"
```

**常见问题**:
- ❌ 缺少 OHOS 实现的 `deps`（如 `distributeddata_inner`）
- ❌ 缺少适配器头文件的 `include_dirs`
- ❌ 忘记 `if (!is_arkuix_target)` 条件编译

---

### ✅ 检查项 2: 插件库注册 (plugin_lib.gni)

**状态**: ⚠️ **手动** - 必须更新此文件

**文件**: `plugins/plugin_lib.gni`

**作用**: 将模块名称添加到 `common_plugin_libs` 数组

**步骤**:
1. 打开 `plugins/plugin_lib.gni`
2. 找到 `common_plugin_libs` 数组（约第 120 行）
3. 按字母顺序添加模块名称

**示例**:
```gni
# plugins/plugin_lib.gni (约第 120 行)
common_plugin_libs = [
  # ... 现有模块 ...
  "notification_manager",
  "pasteboard",
  "{your_module}",  # ← 添加这一行（按字母顺序）
  "vibrator",
]
```

**验证命令**:
```bash
# 检查模块是否已注册
grep "\"{your_module}\"" plugins/plugin_lib.gni
```

**影响**: 没有此配置，模块将被排除在编译之外

---

### ✅ 检查项 3: API 工具链配置 (apiConfig.json)

**状态**: ⚠️ **手动** - 必须更新此文件

**文件**: `interface/sdk/plugins/api/apiConfig.json`

**作用**: 添加模块工具链配置

**步骤**:
1. 打开 `interface/sdk/plugins/api/apiConfig.json`
2. 滚动到 JSON 数组末尾
3. 在闭合 `]` 前添加模块配置

**模板**:
```json
{
    "module": "ohos.{your_module}",
    "library": {
        "android": [
            "lib/{your_module}/ace_{your_module}_android.jar",
            "lib/{your_module}/arch_type/lib{your_module}.so"
        ],
        "ios": [ "xcframework/build_modes/lib{your_module}.xcframework" ]
    },
    "deps": {
        "android": [],
        "ios": []
    }
}
```

**重要**: 保持有效的 JSON 语法（逗号、引号、括号）

**验证命令**:
```bash
# 检查 JSON 语法是否有效
python3 -m json.tool interface/sdk/plugins/api/apiConfig.json > /dev/null && echo "✅ Valid JSON" || echo "❌ Invalid JSON"

# 检查模块是否已配置
grep "ohos.{your_module}" interface/sdk/plugins/api/apiConfig.json
```

**影响**: 没有此配置，构建系统不知道在哪里找到输出库

---

### ✅ 检查项 4: 跨平台 SDK 描述 (MANDATORY)

**状态**: ⚠️ **手动** - 必须更新此文件

**文件**: `build_plugins/sdk/arkui_cross_sdk_description_std.json`

**作用**: 将模块添加到跨平台 SDK 描述（android 和 ios 数组）

**原因**: 这是**跨平台编译入口点** - 没有这个，模块不会包含在跨平台 SDK 中

**步骤**:
1. 打开 `build_plugins/sdk/arkui_cross_sdk_description_std.json`
2. 找到 `"android"` 数组（约第 67 行）
3. 在 vibrator 模块后添加 Android 配置（约第 1650 行）
4. 找到 `"ios"` 数组（约第 1688 行）
5. 在 vibrator 模块后添加 iOS 配置（约第 2660 行）

**Android 模板**:
```json
{
    "install_dir": "arkui-x/plugins/api/lib/{your_module}/arch_type",
    "module_label": "//plugins/{your_module}/adapter/android:{your_module}_adapter_android",
    "target_os": ["linux", "windows", "darwin"]
}
```

**Android Java 模板** (如果有 Java 适配器):
```json
{
    "install_dir": "arkui-x/plugins/api/lib/{your_module}",
    "module_label": "//plugins/{your_module}/frameworks/android/java:{your_module}_plugin_java",
    "target_os": ["linux", "windows", "darwin"]
}
```

**iOS 模板**:
```json
{
    "install_dir": "arkui-x/plugins/api/framework/arch_type/lib{your_module}.framework",
    "module_label": "//plugins/{your_module}/ios:{your_module}_adapter_ios",
    "target_os": ["darwin"]
}
```

**验证命令**:
```bash
# 验证 JSON 格式
python3 -m json.tool build_plugins/sdk/arkui_cross_sdk_description_std.json > /dev/null && echo "✅ Valid" || echo "❌ Invalid"

# 检查模块是否已配置
grep "{your_module}" build_plugins/sdk/arkui_cross_sdk_description_std.json
```

**影响**: ⚠️ **关键** - 没有这个配置，模块不会包含在跨平台 SDK 中

---

## 参考实现

### 最佳参考: plugins/net/http

| 方面 | 文件位置 |
|------|---------|
| API 定义 | `interface/sdk-js/api/@ohos.net.http.d.ts` |
| NAPI 绑定 | `plugins/net/http/http_module.cpp` |
| 平台接口 | `plugins/net/http/interface/http_exec_interface.h` |
| 业务逻辑 | `plugins/net/http/common/http_request_group.cpp` |
| Android 实现 | `plugins/net/http/android/http_exec.cpp` |
| iOS 实现 | `plugins/net/http/ios/http_exec.mm` |
| Java 实现 | `plugins/net/http/frameworks/android/java/` |
| 构建配置 | `plugins/net/http/BUILD.gn` |

### 代码示例

#### HTTP 请求流程

```cpp
// 1. NAPI 层接收请求
napi_value CreateHttpRequest(napi_env env, napi_callback_info info) {
    // 解析参数
    // 创建 HttpRequest 对象
}

// 2. 业务逻辑层处理
void HttpCore::ExecuteRequest(const HttpRequest& req, Callback cb) {
    strategy_->Execute(req, cb);
}

// 3. 平台接口层调度
class IHttpExec {
    virtual void Execute(const HttpRequest& req, Callback cb) = 0;
};

// 4. Android 实现
class AndroidHttpExec : public IHttpExec {
    void Execute(const HttpRequest& req, Callback cb) override {
        // 调用 Java HttpURLConnection
    }
};

// 5. iOS 实现
class IOSHttpExec : public IHttpExec {
    void Execute(const HttpRequest& req, Callback cb) override {
        // 调用 NSURLSession
    }
};
```

## 检查清单

### 实现前

- [ ] 确认需要插件适配（非框架能力）
- [ ] 查看 OHOS 是否有可复用的实现
- [ ] 确定适用模块列表

### 实现中

- [ ] 创建目录结构 (interface, napi, common, android, ios)
- [ ] 定义纯虚接口 (interface/)
- [ ] 实现业务逻辑 (common/)
- [ ] 实现 Android 平台 (android/)
- [ ] 实现 iOS 平台 (ios/)
- [ ] 添加 NAPI 绑定 (napi/)

### 构建配置

- [ ] 添加到 `plugins/plugin_lib.gni`
- [ ] 配置 `interface/sdk/plugins/api/apiConfig.json`
- [ ] 配置 `build_plugins/sdk/arkui_cross_sdk_description_std.json`
- [ ] 创建 `BUILD.gn`

### 测试

- [ ] 添加 XTS 测试 (test/xts/)
- [ ] 添加 API 文档 (docs/zh-cn/)
- [ ] 添加示例代码 (samples/CodeLab/)
- [ ] 在 Android 上测试
- [ ] 在 iOS 上测试

### 验证

- [ ] 编译通过
- [ ] 测试通过
- [ ] 文档完整
- [ ] 符合 16 条核心规则

## 常见问题

### Q1: 插件编译不通过？

**A**: 检查以下配置：
1. 是否已添加到 `plugins/plugin_lib.gni`
2. `BUILD.gn` 中的 `deps` 是否正确
3. `include_dirs` 是否包含头文件路径

### Q2: SDK 中找不到插件模块？

**A**: 检查：
1. `build_plugins/sdk/arkui_cross_sdk_description_std.json` 是否配置
2. `interface/sdk/plugins/api/apiConfig.json` 是否配置
3. 重新执行构建

### Q3: 运行时 API 调用失败？

**A**: 检查：
1. NAPI 绑定是否正确导出
2. JNI 引用是否已缓存
3. 异步回调是否在正确的线程执行

### Q4: 如何调试插件？

**A**:
```bash
# 1. 查看构建日志
./build.sh --product-name arkui-x --target-os android --log-level debug

# 2. 查看 NAPI 日志
hilog -T ace

# 3. 查看 JNI 日志
adb logcat | grep -i "new_feature"

# 4. 在代码中添加日志
HILOG_INFO("Tag", "Debug info: %{public}s", data);
```

### Q5: 何时使用目录分离 vs 条件编译？

**A**:
- **目录分离**: 平台逻辑差异大，代码超过 50 行
- **条件编译**: 小型差异，代码少于 20 行

### Q6: 如何处理平台特有功能？

**A**:
```cpp
// 在平台实现层处理，不影响业务逻辑
class AndroidStrategy : public IStrategy {
    void Execute() override {
        #ifdef ANDROID_ENABLE_FEATURE_X
            // Android 特有功能
        #endif
    }
};
```

### Q6: 如何处理平台特有功能？

**A**:
```cpp
// 在平台实现层处理，不影响业务逻辑
class AndroidStrategy : public IStrategy {
    void Execute() override {
        #ifdef ANDROID_ENABLE_FEATURE_X
            // Android 特有功能
        #endif
    }
};
```

## 自动化工具

### Skill 工具：Platform API Adapter

Claude Code 提供了一个 Skill 工具，可以自动生成适配器模式的代码框架。

#### 使用方法

在 Claude Code 中调用：

```
/skill-platform-api
```

#### 功能

- 自动生成纯虚接口定义
- 生成 OHOS 薄包装代码
- 生成 Android/iOS 存根实现
- 生成构建配置文件
- 生成迁移指南文档

#### 示例会话

```
You: /skill-platform-api

Skill: Platform API Adapter Generator
=====================================

Module name (e.g., 'pasteboard'): preferences
OHOS client class name (e.g., 'PasteboardClient'): PreferencesClient

Enter methods to adapt (one per line, empty line to finish):
Format: return_type method_name param1_type param1_name ...
Method> void PutString const std::string& key const std::string& value
Method> std::string GetString const std::string& key const std::string& defValue
Method> void Clear
Method>

Generating adapter architecture...
✓ Adapter interface generated
✓ OHOS adapter generated
✓ Android adapter stub generated
✓ iOS adapter stub generated
✓ Build configurations generated
✓ Documentation generated

Next steps:
1. Implement Android/iOS adapter methods (marked with TODO)
2. Apply NAPI layer migration (see ADAPTER_MIGRATION.md)
3. Update plugin_lib.gni
4. Update apiConfig.json
5. Compile and test
```

#### 生成的文件

**OHOS 仓库**：
- `interfaces/kits/napi/preferences_adapter.h`
- `interfaces/kits/napi/preferences_adapter_helper.h`
- `services/adapter/preferences_adapter_ohos.cpp`
- `ADAPTER_MIGRATION.md`

**Plugin 仓库**：
- `adapter/android/preferences_adapter_android.cpp`
- `adapter/ios/preferences_adapter_ios.mm`
- `BUILD.gn`
- `ADAPTER_ARCHITECTURE.md`

### 自动化迁移脚本

#### sed 脚本示例

```bash
#!/bin/bash
# migration_script.sh - 自动替换 NAPI 层调用

# 1. 替换直接调用为 Helper 宏调用
find . -name "*.cpp" -exec sed -i 's/PasteboardClient::GetInstance()/PASTEBOARD_ADAPTER()/g' {} \;

# 2. 添加 Helper 宏头文件
find . -name "*.cpp" -exec sed -i '1i #include "pasteboard_adapter_helper.h"' {} \;

# 3. 验证替换结果
grep -r "PASTEBOARD_ADAPTER()" . --include="*.cpp"
```

#### Python 迁移脚本

```python
#!/usr/bin/env python3
# migrate.py - 自动化迁移工具

import re
import sys
from pathlib import Path

# 映射表：原始调用 → Helper 宏
REPLACEMENTS = {
    r'PasteboardClient::GetInstance()': 'PASTEBOARD_ADAPTER()',
    r'PreferencesClient::GetInstance()': 'PREFERENCES_ADAPTER()',
    r'KVStoreClient::GetInstance()': 'KVS_ADAPTER()',
}

def migrate_file(file_path):
    """迁移单个文件"""
    content = file_path.read_text()
    modified = False

    for pattern, replacement in REPLACEMENTS.items():
        if re.search(pattern, content):
            content = re.sub(pattern, replacement, content)
            modified = True

    if modified:
        file_path.write_text(content)
        print(f"Migrated: {file_path}")

def main():
    """主函数"""
    root = Path(sys.argv[1]) if len(sys.argv) > 1 else Path(".")
    cpp_files = root.rglob("*.cpp")

    for file_path in cpp_files:
        migrate_file(file_path)

if __name__ == "__main__":
    main()
```

#### 使用方法

```bash
# 1. 备份代码
git stash

# 2. 运行迁移脚本
bash migration_script.sh
# 或
python3 migrate.py

# 3. 验证编译
./build.sh --product-name arkui-x --target-os android --build-target pasteboard_napi

# 4. 运行测试
# 如果失败，回滚
git stash pop
```

## 高级调试技巧

### JNI 调试

```bash
# 1. 启用 JNI 调试日志
adb shell setprop debug.checkjni 1

# 2. 查看 JNI 调用
adb logcat | grep -i "checkjni"

# 3. 检测 JNI 引用泄漏
adb logcat | grep -i " JNI"
```

### 符号查看

```bash
# 1. 查看导出的符号
nm -D --defined-only libplugin.so | grep PLUGIN_EXPORT

# 2. 查看所有符号（包括隐藏）
nm -a libplugin.so | grep -i "function_name"

# 3. 检查符号可见性
readelf -s libplugin.so | grep NOT_GLOBAL
```

### 内存泄漏检测

```bash
# Android - 使用 AddressSanitizer
./build.sh --product-name arkui-x --target-os android \
    --gn-args="is_asan=true use_clang_coverage=true"

# 运行应用后查看报告
adb logcat | grep -i "asan"

# iOS - 使用 Instruments
# 1. Xcode → Product → Profile
# 2. 选择 "Leaks" 模板
# 3. 运行应用并分析
```

### 性能分析

```bash
# Android - Simpleperf
# 1. 采集性能数据
adb shell simpleperf record -p <pid> -f 99 -a sleep 30

# 2. 导出报告
adb pull /data/local/tmp/perf.data perf.data

# 3. 分析报告
simpleperf report perf.data

# iOS - Instruments Time Profiler
# Xcode → Product → Profile → Time Profiler
```

## 最佳实践总结

### DO (推荐做法)

✅ **架构设计**：
- 使用明确的分层架构
- 平台隔离通过目录或条件编译
- 接口抽象使用纯虚类

✅ **性能优化**：
- 缓存 JNI 引用和方法 ID
- 批量传递数据，减少跨语言调用
- 使用 `std::string_view` 和移动语义

✅ **错误处理**：
- 定义统一的错误码
- 平台特定错误映射
- 详细的错误日志

✅ **线程安全**：
- 使用互斥锁保护共享数据
- JNI 调用前确保线程已附加
- 使用线程安全容器

✅ **构建系统**：
- 明确声明依赖
- 配置符号可见性
- 使用条件编译选择平台

### DON'T (避免做法)

❌ **架构设计**：
- 不要在 NAPI 层直接调用平台 API
- 不要过度使用条件编译（优先目录分离）
- 不要在接口层包含平台特定代码

❌ **性能优化**：
- 不要频繁 JNI 调用（未缓存）
- 不要不必要的数据拷贝
- 不要在热路径中使用虚函数表

❌ **错误处理**：
- 不要直接返回平台错误码
- 不要忽略错误检查
- 不要使用通用错误码掩盖具体问题

❌ **线程安全**：
- 不要假设线程已附加到 JVM
- 不要在未加锁的情况下访问共享数据
- 不要在回调中阻塞主线程

❌ **构建系统**：
- 不要忘记更新 `plugin_lib.gni`
- 不要遗漏 `apiConfig.json` 配置
- 不要忘记 SDK 描述配置

## 参考文档

### 架构与设计
- [系统架构](../architecture/system_architecture.md) - ArkUI-X 系统架构
- [开发最佳实践](../best_practices/development_guide.md) - 开发指南
- [主 CLAUDE.md](../../CLAUDE.md) - 项目主文档

### 适配器模式
- [Platform API Adapter Skill](.claude/skills/platform-api-adapter/README.md) - 自动化工具
- [Pasteboard Adapter Example](../../plugins/pasteboard/ADAPTER_ARCHITECTURE.md) - 完整示例
- [Preferences Adapter Guide](../../foundation/distributeddatamgr/preferences/ADAPTER_MIGRATION.md) - 迁移指南

### 外部资源
- [OpenHarmony API 文档](https://docs.openharmony.cn/)
- [JNI 规范](https://docs.oracle.com/javase/8/docs/technotes/guides/jni/)
- [NAPI 指南](https://docs.openharmony.cn/en/v3.2/zh-cn/application-dev/napi/)
