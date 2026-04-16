# ArkUI-X 系统架构

本文档详细说明 ArkUI-X 的系统架构设计，包括分层架构、目录结构、核心组件及其交互关系。

## 目录

- [分层架构设计](#分层架构设计)
- [目录结构映射](#目录结构映射)
- [核心技术特性](#核心技术特性)
- [组件生命周期](#组件生命周期)
- [跨平台适配架构](#跨平台适配架构)
- [构建系统架构](#构建系统架构)
- [架构原则](#架构原则)

## 分层架构设计

ArkUI-X 采用清晰的分层架构，从上到下依次为：

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

| 层级 | 职责 | 关键技术 |
|-----|-----|---------|
| **应用层** | 用户应用代码 | ArkTS/TypeScript |
| **前端桥接层** | 代码解析、运行时 | ArkTS编译器、ETS Runtime、V8 |
| **组件框架层** | UI组件、状态管理 | Components NG |
| **图形引擎层** | 渲染、绘制 | Rosen 2D、Skia |
| **平台适配层** | 平台能力抽象 | JNI、ObjC++ |
| **原生平台层** | 系统API调用 | AOSP、Apple Frameworks |

## 目录结构映射

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
│   ├── multimedi*/                  # 多媒体框架
│   ├── communicati*/                 # 通信框架
│   ├── distributeddatamgr/           # 分布式数据管理
│   ├── filemanagement/              # 文件管理
│   └── multimodalinput/             # 多模态输入
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
│       └── dotfile.gn                # GN 配置 (链接为 .gn)
│
├── build_plugins/                    # ArkUI-X 构建系统
│   └── build_scripts/                # 构建脚本
│       └── build.sh                  # 主构建脚本 (链接为 build.sh)
│
├── base/                             # 基础服务
│   ├── hiviewdfx/                    # 日志和追踪
│   │   ├── hilog/                    # 系统日志
│   │   └── hitrace/                  # 性能追踪
│   ├── security/                     # 安全框架
│   ├── notification/                 # 事件处理
│   └── global/                       # 全局资源
│
├── commonlibrary/                    # 公共库
│   ├── c_utils/                      # C 工具函数
│   ├── ets_utils/                    # ETS 工具函数
│   └── memory_utils/                 # 内存管理
│
├── developtools/                     # 开发工具
│   ├── ace_ets2bundle/               # ETS 编译器
│   ├── ace_js2bundle/                # JS 编译器
│   ├── profiler/                     # 性能分析器
│   └── hapsigner/                    # HAP 签名工具
│
├── test/                             # 测试框架
│   ├── testfwk/arkxtest/             # 测试框架
│   └── xts/                          # XTS 兼容性测试
│
├── third_party/                      # 第三方依赖
│   ├── skia/                         # 2D 图形
│   ├── icu/                          # 国际化
│   ├── openssl/                      # SSL/TLS
│   └── ...                           # 30+ 其他库
│
├── samples/                          # 示例应用
├── docs/                             # 文档
└── productdefine/                    # 产品定义
```

## 核心技术特性

### 1. 多前端支持

ArkUI-X 支持三种前端技术：

| 前端类型 | 说明 | 适用场景 |
|---------|-----|---------|
| **declarative_frontend** | ArkTS 声明式前端 | 推荐，性能最优 |
| **arkts_frontend** | ArkTS 编译型前端 | 新项目 |
| **js_frontend** | JavaScript 前端 | 兼容遗留项目 |

### 2. 组件化架构 (Components NG)

采用 Pattern-Model-Property 分离的现代组件架构：

```
FrameNode (组件节点)
    ├── Pattern (行为模式)
    │   ├── OnModifyDone()    # 属性修改回调
    │   ├── OnDirtyLayoutWrapperSwap()  # 布局交换
    │   └── ...
    ├── LayoutAlgorithm (布局算法)
    │   ├── Measure()         # 测量
    │   └── Layout()          # 布局
    ├── RenderNode (渲染节点)
    │   ├── Paint()           # 绘制
    │   └── ...
    └── EventHub (事件中心)
        ├── AddOnTouchEvent() # 触摸事件
        └── ...
```

### 3. 状态管理

```
@State  - 组件内部状态
@Prop   - 父组件传递的属性
@Link   - 双向绑定
@Provide/@Inject - 跨层级状态共享
@Observed/@ObjectLink - 对象嵌套观察
```

### 4. 渲染管线

```
VSync信号
    ↓
Measure (测量)
    ↓
Layout (布局)
    ↓
Render (渲染)
    ↓
Present (呈现)
```

## 组件生命周期

```
Application Start
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 1. Ability 初始化                                               │
│    • StageApplication.onCreate()                               │
│    • AceEnv::getInstance() - 加载 native 库                    │
│    • CreateRuntime() - 初始化 JS 运行时 (ArkJsRuntime)          │
│    • PreloadAceModule() - 预加载核心模块                        │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 2. Stage/Page 初始化                                            │
│    • StageActivity.onCreate() (Android) / iOS 等价实现          │
│    • Attach stage activity, 设置 window view                   │
│    • 创建 ResourceManager (app + system resources)             │
│    • 创建 JsAbilityStage                                        │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 3. Page 加载 (loadContent)                                      │
│    • UIContent::Create()                                        │
│    • AceContainer 创建 (以 instanceId 为键)                    │
│    • PipelineContext 初始化                                     │
│    • Frontend 附加 (DeclarativeFrontendNG)                     │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 4. 组件树构建                                                   │
│    • 解析 ArkTS 代码 → 组件描述                                 │
│    • 创建 FrameNode 实例                                        │
│    • 建立父子关系                                               │
│    • 初始化 Pattern, Properties, EventHub                      │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 5. 布局与渲染                                                   │
│    • Measure: 计算组件尺寸                                      │
│    • Layout: 定位组件                                           │
│    • Render: 使用 Rosen/Skia 图形引擎绘制                       │
│    • VSync: 显示帧同步                                          │
└─────────────────────────────────────────────────────────────────┘
```

### 关键生命周期方法

| 方法 | 触发时机 | 用途 |
|------|---------|-----|
| `OnAttachToMainTree()` | 组件挂载到树 | 初始化 |
| `OnModifyDone()` | 属性修改完成 | 应用新属性 |
| `OnDirtyLayoutWrapperSwap()` | 布局包装器交换 | 更新布局 |
| `OnDetachFromMainTree()` | 组件从树卸载 | 清理资源 |

## 跨平台适配架构

### 架构分层

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
│    - Activity        │          │    - UIKit           │
│    - SurfaceView     │          │    - UIView          │
│    - AudioManager    │          │    - AVFoundation    │
└─────────────────────┘          └─────────────────────┘
```

### 两种适配路径

| 能力类型 | 适配位置 | 示例 |
|---------|---------|-----|
| **框架能力** | `foundation/arkui/ace_engine/adapter/` | Window, Input, Font, Keyboard |
| **子系统 API** | `plugins/` | HTTP, FS, Data, BLE |

详细适配指南参考：
- 框架能力适配：见 `foundation/arkui/ace_engine/adapter/` 目录下的实现
- 子系统 API 适配：见 [插件子系统适配指南](../plugin_adaptation/README.md)

## 构建系统架构

```
build.sh (主入口)
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  HB 构建工具 (build/hb/main.py)                                  │
│  • 解析命令行参数                                                │
│  • 加载产品配置                                                  │
│  • 执行构建流程                                                  │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  GN (Generate Ninja)                                            │
│  • 解析 .gn 配置                                                │
│  • 处理 BUILD.gn 文件                                            │
│  • 生成 ninja.build 文件                                         │
│  • 解析依赖关系                                                  │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  Ninja 构建执行器                                                │
│  • 执行构建任务                                                  │
│  • 并行编译                                                      │
│  • 链接库文件                                                    │
│  • 生成输出文件                                                  │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  构建产物 (out/[product]/)                                       │
│  • libace*.z.so - ACE 引擎库                                    │
│  • libarkui_*.z.so - 组件库                                     │
│  • *.abc - ArkTS 字节码文件                                     │
│  • APK/IPA - 平台特定包                                         │
└─────────────────────────────────────────────────────────────────┘
```

### 多模块编译流程

```
For each module in ace_platforms (android, ios):
  ┌────────────────────────────────────────┐
  │ 1. 解析模块的 BUILD.gn                │
  │    - platform_sources (common)        │
  │    - platform_sources (android)       │
  │    - platform_sources (ios)           │
  └────────────────────────────────────────┘
              ↓
  ┌────────────────────────────────────────┐
  │ 2. 添加条件编译                        │
  │    - ANDROID_PLATFORM 宏              │
  │    - IOS_PLATFORM 宏                  │
  └────────────────────────────────────────┘
              ↓
  ┌────────────────────────────────────────┐
  │ 3. 链接平台特定库                      │
  │    - Android: JNI, Java 库           │
  │    - iOS: Objective-C 框架            │
  └────────────────────────────────────────┘
              ↓
  ┌────────────────────────────────────────┐
  │ 4. 打包输出                            │
  │    - Android: .so + .jar              │
  │    - iOS: .framework / .dylib         │
  └────────────────────────────────────────┘
```

## 架构原则

### 1. 层次分离

- 清晰的边界：前端、框架、图形、平台层之间职责明确
- 单向依赖：上层依赖下层，下层不感知上层
- 接口稳定：通过抽象接口隔离变化

### 2. 平台抽象

- **纯虚接口**：平台差异通过纯虚 C++ 接口隔离
- **最小平台 API**：JNI/ObjC++ 桥接仅用于必要的平台服务
- **最大化复用**：通用 C++ 层最大程度共享代码

### 3. 声明式 UI

- ArkTS/TypeScript 声明式语法
- 响应式状态管理
- 自动 diff 和更新

### 4. 组件化

- Pattern-Model-Property 分离
- 组合优于继承
- 生命周期管理

### 5. 性能优先

- Native 渲染：Rosen/Skia
- 优化布局算法
- VSync 同步

### 6. 渐进式迁移

- 支持遗留 JavaScript（V8 运行时）
- 平滑升级路径
- 向后兼容

## 参考文档

- [插件子系统适配指南](../plugin_adaptation/README.md)
- [开发最佳实践](../best_practices/README.md)
- [主 CLAUDE.md](../../CLAUDE.md)
