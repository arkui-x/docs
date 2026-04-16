## Project Overview

ArkUI-X is a cross-platform UI framework that enables development of applications running on HarmonyOS, Android, and iOS from a single codebase. It provides a declarative UI framework using ArkTS (TypeScript-based) with native performance through C++ rendering engines.

## Knowledge Base

This project maintains a comprehensive knowledge base system for in-depth component analysis and development guidance.

**Location**: `knowledge/`

### Quick Reference


| Topic               | Document                                        |
| ------------------- | ----------------------------------------------- |
| System Architecture | `knowledge/architecture/system_architecture.md` |
| Plugin Adaptation   | `knowledge/plugin_adaptation/adapter_guide.md`  |
| Development Guide   | `knowledge/best_practices/development_guide.md` |

### Usage Guidelines

When answering questions or providing guidance:

1. **Check for relevant knowledge base documents** before diving into code analysis
2. **Search the knowledge base** using Grep tools to find component-specific information
3. **Reference knowledge base content** to provide comprehensive, context-aware answers
4. **Cross-reference with actual code** using the file paths and line numbers cited in knowledge base documents

> **Complete structure**: See [Knowledge Base README](knowledge/knowledge_base_README.md) for full documentation structure.

## Core Working Principles

### 1. Code Verification: Actual Code Only

When answering questions about ArkUI-X Crossplatform code:

- **Always provide actual code from the repository**
  - Use Read/Grep tools to locate and read actual source code
  - Reference complete file paths when mentioning code (e.g., `frameworks/core/xxx/yyy.cpp:123`)
  - Never guess or fabricate code implementations
- **Missing information triggers user feedback**
  - If required code is not found in current project, explicitly state: "此代码在当前工程中未找到"
  - Do not make assumptions or write hypothetical code
  - Ask user to provide the missing implementation

**Example**:

```cpp
// ✅ Correct: Read actual source
// Source: frameworks/core/components_ng/pattern/menu/menu_pattern.cpp:123
Size GetSubWindowSize(int32_t parentContainerId, uint32_t displayId)
{
    auto finalDisplayId = displayId;
    auto defaultDisplay = Rosen::DisplayManager::GetInstance().GetDisplayById(displayId);
    // ... actual implementation
}

// ❌ Wrong: Fabricated code
// Do not write hypothetical implementations without verification
```

### 2. Speculation Management

When dealing with uncertain or incomplete information:

- **Explicitly label speculation**
  - Clearly mark any unverified content as: "推测" (speculation)
  - Provide reasoning for the speculation when possible
  - Request user confirmation for speculative statements
- **Verify before implementation**
  - If speculating about behavior, first use Grep/Read to verify
  - If implementation is not found in ace_engine, ask user to provide it
  - Never implement based on speculation alone

**Example**:

```markdown
✅ Correct:
基于 menu_pattern.cpp:123 的分析，推测 OnModifyDone 在以下情况下会被调用...（推测）

❌ Wrong:
OnModifyDone 会在以下情况下调用...（未标注推测）
```

### 3. Code Logic Verification: Code Over Suggestions

When receiving suggestions or corrections:

- **Verify suggestions against actual code**
  - User suggestions about code logic may be incorrect
  - Always verify with Read/Grep tools before accepting suggestions
  - Base conclusions on actual code behavior, not assumptions
- **Evidence-based reasoning**
  - When user questions code behavior, analyze actual implementation
  - Use "代码为准原则" (Code-first principle)
  - Provide evidence from source code to support or refute suggestions

**Example**:

```
User: "这个函数应该在初始化时调用，对吗？"
Claude: 让我先查看源码验证...
[Read source file]
根据 frameworks/xxx/yyy.cpp:456，该函数实际上是在 OnDirtyLayoutWrapperSwap 时调用，而非初始化时。
```

### 4. Error Learning: Knowledge Base Updates

When errors are corrected by users:

- **Learn from corrections**
  - Document the error and correction in knowledge base
  - Identify root cause of the misunderstanding
  - Add preventive measures to avoid similar errors
- **Update documentation**
  - Create or update knowledge base entries with correct information
  - Reference actual code locations (file:line)
  - Share lessons learned across sessions

**Example**:

```markdown
## Learned Lessons

### Error: Incorrect GetSubWindowSize Branch Coverage
**Date**: 2025-01-27
**Issue**: Assumed defaultDisplay is always available
**Root Cause**: Did not verify Rosen::DisplayManager dependency
**Correction**: User clarified that mock infrastructure is required
**Prevention**: Always verify external dependencies with user before assuming availability
**Reference**: adapter/ohos/entrance/subwindow/subwindow_ohos.cpp:199-243
```

### 5. Knowledge Base Maintenance

When discovering discrepancies between documentation and actual code:

- **Verify actual code first**
  - Use Read/Grep to confirm current implementation
  - Compare documented behavior with actual code
- **Update documentation**
  - Fix incorrect information in knowledge base files
  - Update code references (file paths, line numbers)
  - Ensure all examples match actual code
- **Notify user**
  - Report discovered discrepancies
  - Propose corrections for approval
  - Document the correction after update

**Example**:

```
Discrepancy Found:
Documented: frameworks/xxx/yyy.cpp:299 calls UpdateBorderRadius
Actual: frameworks/xxx/yyy.cpp:303 calls UpdateBorderRadius (after code refactoring)

Action: Updating knowledge base to reflect current code location...
```

### 6. Code Editing: API Call Validation (Critical)

When adding or modifying code that calls existing framework APIs:

- **Validate every API call for duplicate call scenarios**

  - Before adding any API call, search for ALL existing call sites of that API
  - Analyze if the API is called from multiple branches/entry points
  - Determine if the API has internal protection against duplicate calls (static flags, etc.)
  - If no protection exists, evaluate whether duplicate calls would cause issues
- **Mandatory validation steps**:

  1. **Search all call sites**: Use Grep to find all places where the API is called
  2. **Check internal protection**: Read the API implementation to verify if it has duplicate call protection
  3. **Analyze call paths**: Map out all entry points that could lead to calling this API
  4. **Evaluate impact**: If duplicate calls occur, determine if it causes:
     - Memory leaks or resource exhaustion
     - Data corruption or inconsistent state
     - Performance degradation
     - JS object property overwriting
- **Decision criteria**:

  - If API has internal protection: Safe to call from multiple places
  - If API has no protection but is idempotent: Generally safe
  - If API has no protection and is NOT idempotent: **Must add protection or avoid duplicate call paths**

**Example**:

```cpp
// ❌ Wrong: Adding API call without validation
void PreloadAceModuleEssential(void* runtime) {
    // ...
    PreloadAceConsole(runtime, global);  // Called without checking if PreloadAceModule also calls this
    PreloadStateManagement(runtime);      // May execute JS code twice!
}

// ✅ Correct: Validate first, then decide approach
// Step 1: Grep for PreloadAceConsole calls - found in PreloadAceModule, InitJsObject
// Step 2: Check implementation - no static protection
// Step 3: Analyze - if PreloadAceModuleEssential runs first, then PreloadAceModule,
//         PreloadAceConsole would be called twice
// Step 4: Solution - Only call PreloadView (has static protection) in PreloadAceModuleEssential
void PreloadAceModuleEssential(void* runtime) {
    // Only preload essential views - other modules loaded by PreloadAceModule
    PreloadView(JSNApi::GetGlobalObject(vm));  // PreloadView has static bool isPreloaded protection
}
```

## Build System

This project uses **GN (Generate Ninja)** as the meta-build system with Python-based build tooling.

### Main Build Commands

```bash
# Build for Android
./build.sh --product-name arkui-x --target-os android

# Build for iOS
./build.sh --product-name arkui-x --target-os ios
```

## Architecture

详见：[系统架构文档](docs/knowledge/architecture/system_architecture.md)

### 分层架构设计

```
┌─────────────────────────────────────────────────────────────────┐
│                      应用层 (Application Layer)                  │
│                    ArkTS/TypeScript Applications                 │
└─────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────┐
│                     前端桥接层 (Frontend Layer)                   │
│   declarative_frontend | arkts_frontend | js_frontend           │
│   - ArkTS 编译器      - ETS Runtime    - V8 Runtime              │
└─────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────┐
│                   组件框架层 (Component Framework)                │
│   Components NG (Pattern | Layout | Render | Gesture)           │
│   - 声明式组件系统   - 状态管理      - 动画框架                   │
└─────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────┐
│                   图形引擎层 (Graphics Engine)                    │
│   Rosen 2D | Skia | OpenGL ES                                   │
│   - 2D 渲染       - 绘制 API      - Surface 管理                  │
└─────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────┐
│                   平台适配层 (Platform Adapter)                   │
│   Android Adapter | iOS Adapter                                 │
│   - Window         - Input       - Font      - Keyboard          │
│   - JNI Bridge     - ObjC++ Bridge                               │
└─────────────────────────────────────────────────────────────────┘
                               ↓
┌─────────────────────────────────────────────────────────────────┐
│                  原生平台层 (Native Platform)                     │
│   Android (AOSP) | iOS (Apple Frameworks)                        │
└─────────────────────────────────────────────────────────────────┘
```

### 各层职责

| 层级 | 职责 | 关键技术 | 代码位置 |
|-----|-----|---------|---------|
| **应用层** | 用户应用代码 | ArkTS/TypeScript | `samples/`, 应用工程 |
| **前端桥接层** | 代码解析、运行时 | ArkTS编译器、ETS Runtime、V8 | `arkcompiler/`, `foundation/arkui/ace_engine/frameworks/bridge/` |
| **组件框架层** | UI组件、状态管理 | Components NG | `foundation/arkui/ace_engine/frameworks/core/components_ng/` |
| **图形引擎层** | 渲染、绘制 | Rosen 2D、Skia | `foundation/graphic/graphic_2d/` |
| **平台适配层** | 平台能力抽象 | JNI、ObjC++ | `foundation/arkui/ace_engine/adapter/` |
| **原生平台层** | 系统API调用 | AOSP、Apple Frameworks | Android SDK, iOS SDK |

### 目录结构映射

```
arkuix-master/
│
├── arkcompiler/                       # 编译工具链
│   ├── ets_frontend/                  # ArkTS/TypeScript 前端编译器
│   ├── ets_runtime/                   # ETS 运行时执行引擎
│   ├── runtime_core/                  # 核心运行时支持
│   └── toolchain/                     # 编译工具链
│
├── foundation/                        # 基础框架层
│   ├── arkui/                         # UI 框架
│   │   ├── ace_engine/                # ACE 引擎核心
│   │   │   ├── frameworks/            # 框架实现
│   │   │   │   ├── bridge/            # 前端桥接
│   │   │   │   │   ├── declarative_frontend/  # 声明式前端
│   │   │   │   │   └── js_frontend/           # JS 前端
│   │   │   │   └── core/              # 核心组件
│   │   │   │       ├── components_ng/      # NG 组件系统
│   │   │   │       │   ├── pattern/        # Pattern 模式
│   │   │   │       │   ├── layout/         # 布局算法
│   │   │   │       │   ├── render/         # 渲染处理
│   │   │   │       │   └── gesture/        # 手势处理
│   │   │   │       └── pipeline/       # 渲染管道
│   │   │   └── adapter/               # 平台适配
│   │   │       ├── android/           # Android 适配
│   │   │       └── ios/               # iOS 适配
│   │   └── napi/                     # NAPI 绑定层
│   │
│   ├── graphic/                      # 图形引擎
│   │   ├── graphic_2d/               # Rosen 2D 图形引擎
│   │   ├── graphic_surface/          # Surface 管理
│   │   └── graphics_effect/          # 图形特效
│   │
│   ├── appframework/                 # 应用框架
│   ├── multimedia/                   # 多媒体框架
│   ├── communication/                # 通信框架
│   ├── distributeddatamgr/           # 分布式数据管理
│   ├── filemanagement/               # 文件管理
│   └── multimodalinput/              # 多模态输入
│
├── plugins/                          # 平台插件 (50+ 模块)
│   ├── [module_name]/                # 各子系统插件
│   │   ├── interface/                # 平台无关接口
│   │   ├── napi/                     # NAPI 绑定
│   │   ├── common/                   # 共享 C++ 代码
│   │   ├── android/                  # Android 实现
│   │   └── ios/                      # iOS 实现
│   └── plugin_lib.gni                # 插件注册配置
│
├── interface/                        # API 定义
│   ├── sdk/                          # Native SDK
│   └── sdk-js/                       # JavaScript SDK
│       └── api/                      # @ohos.* API 类型定义
│
├── build/                            # OpenHarmony 构建系统
│   └── core/gn/                      # GN 构建配置
│
├── build_plugins/                    # ArkUI-X 构建系统
│   └── build_scripts/                # 构建脚本
│
├── base/                             # 基础服务
│   ├── hiviewdfx/                    # 日志和追踪
│   ├── security/                     # 安全框架
│   └── notification/                 # 事件处理
│
├── commonlibrary/                    # 公共库
│   ├── c_utils/                      # C 工具函数
│   ├── ets_utils/                    # ETS 工具函数
│   └── memory_utils/                 # 内存管理
│
├── developtools/                     # 开发工具
│   ├── ace_ets2bundle/               # ETS 编译器
│   ├── ace_js2bundle/                # JS 编译器
│   └── profiler/                     # 性能分析器
│
├── test/                             # 测试框架
│   ├── testfwk/arkxtest/             # 测试框架
│   └── xts/                          # XTS 兼容性测试
│
├── third_party/                      # 第三方依赖
│   ├── skia/                         # 2D 图形
│   ├── icu/                          # 国际化
│   └── openssl/                      # SSL/TLS
│
└── samples/                          # 示例应用
```

### 组件化架构 (Components NG)

采用 Pattern-Model-Property 分离的现代组件架构：

```
FrameNode (组件节点)
    ├── Pattern (行为模式)
    │   ├── OnModifyDone()           # 属性修改回调
    │   ├── OnDirtyLayoutWrapperSwap()  # 布局交换
    │   └── ...
    ├── LayoutAlgorithm (布局算法)
    │   ├── Measure()                # 测量
    │   └── Layout()                 # 布局
    ├── RenderNode (渲染节点)
    │   ├── Paint()                  # 绘制
    │   └── ...
    └── EventHub (事件中心)
        ├── AddOnTouchEvent()        # 触摸事件
        └── ...
```

### 跨平台适配架构

```
┌─────────────────────────────────────────────────────────────────┐
│           统一 @ohos.* API 层 (TypeScript .d.ts)                │
│                  interface/sdk-js/api/                          │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│                 NAPI 绑定层 (C++)                                │
│        参数解析、类型转换、异步处理                              │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│            业务逻辑层 (平台无关)                                 │
│              核心算法、状态管理                                  │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌──────────────────────┬────────────────────────────────────────┐
│  平台接口 (C++ 头)    │  纯虚 C++ 接口                         │
│                      │  - IFeatureStrategy                    │
└──────────────────────┴────────────────────────────────────────┘
                           ↓
        ┌──────────────────┴──────────────────┐
        ↓                                     ↓
┌─────────────────────┐          ┌─────────────────────┐
│  Android 平台       │          │   iOS 平台          │
│  - JNI Bridge        │          │  - ObjC++ Bridge     │
│  - Java/Kotlin Impl  │          │  - ObjC/Swift Impl   │
│  - AOSP APIs         │          │  - Apple Frameworks  │
└─────────────────────┘          └─────────────────────┘
```

**两种适配路径**：

| 能力类型 | 适配位置 | 示例 |
|---------|---------|-----|
| **框架能力** | `foundation/arkui/ace_engine/adapter/` | Window, Input, Font, Keyboard |
| **子系统 API** | `plugins/` | HTTP, FS, Data, BLE |

### 架构原则

1. **Layer Separation**: 清晰的层次边界，单向依赖
2. **Platform Abstraction**: 纯虚接口隔离平台差异
3. **Cross-Platform Sharing**: 最大化代码复用
4. **Minimal Platform APIs**: 仅必要的平台服务
5. **Declarative UI**: ArkTS/TypeScript 响应式状态
6. **Component-Based**: Pattern-Model-Property 分离
7. **Performance First**: Native 渲染 (Rosen/Skia)
8. **Gradual Migration**: 支持遗留 JavaScript

### Key Components


| 组件              | 路径                             | 说明                  |
| ----------------- | -------------------------------- | --------------------- |
| **ACE Engine**    | `foundation/arkui/ace_engine/`   | 核心 UI 框架引擎      |
| **App Framework** | `foundation/appframework/`       | 应用框架              |
| **NAPI**          | `foundation/arkui/napi`          | NAPI 绑定层           |
| **Graphic 2D**    | `foundation/graphic/graphic_2d/` | Rosen 2D 图形引擎     |
| **Plugins**       | `plugins/`                       | 平台插件 (50+ 适配器) |
| **Build System**  | `build/`, `build_plugins/`       | 构建系统              |

### Key Code Locations


| Functionality         | Location                                                              |
| --------------------- | --------------------------------------------------------------------- |
| **Component Pattern** | `foundation/arkui/ace_engine/frameworks/core/components_ng/pattern/`  |
| **Component Render**  | `foundation/arkui/ace_engine/frameworks/core/components_ng/render/`   |
| **Layout Algorithms** | `foundation/arkui/ace_engine/frameworks/core/components_ng/layout/`   |
| **Gesture Handling**  | `foundation/arkui/ace_engine/frameworks/core/components_ng/gestures/` |
| **Pipeline Core**     | `foundation/arkui/ace_engine/frameworks/core/pipeline/`               |
| **Platform Adapter**  | `foundation/arkui/ace_engine/adapter/android\|ios/`                   |
| **Plugin Interfaces** | `plugins/[module]/[module]_exec_interface.h`                          |
| **API Definitions**   | `interface/sdk-js/api/@ohos.*.d.ts`                                   |
| **NAPI Bindings**     | `plugins/[module]/[module]_module.cpp`                                |

## Cross-Platform Development Guide

### Overview

ArkUI-X 跨平台框架用于将 OpenHarmony (OHOS) API 适配到 Android 和 iOS 平台，采用分层架构实现最大化的代码复用。

### Recommended Tool: arkuix-module-adapter Skill

**推荐使用 `arkuix-module-adapter` Skill 进行 OHOS API 跨平台适配**

```bash
# 使用 skill 进行适配
skill(name="arkuix-module-adapter")
```
**获取方式**：

- 如果本地没有此 skill，可从 [Matrix 技能广场](https://matrix.openharmony.cn/#/skillsquare) 下载
- 该 skill 提供完整的适配流程指导和代码生成能力

## Documentation

Comprehensive documentation is available in the `docs/` directory:

- `docs/en/` - English documentation
- `docs/zh-cn/` - Chinese documentation

## License

Apache License 2.0 - See LICENSE file in community/ directory

## Critical Include Patterns

```cpp
// For ACE framework APIs
#include "core/pipeline_ng/ui_task_scheduler.h"
#include "interfaces/inner_api/ace/ace_view.h"

// For platform adapters
#ifdef ANDROID_PLATFORM
#include "adapter/android/entrance/java/jni/ace_view_android.h"
#elif defined IOS_PLATFORM
#include "adapter/ios/entrance/ace_view_ios.h"
#endif

// For graphics
#include "roseEngine/modules/rosen/modules/render_service_client/core/..."
```
## Conditional Compilation Patterns

### Platform Guards

```cpp
#ifdef IS_ARKUI_X_TARGET
    #ifdef ANDROID_PLATFORM
        // Android-specific code
    #elif defined(IOS_PLATFORM)
        // iOS-specific code
    #endif
#endif
```
