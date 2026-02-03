# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ArkUI-X is a cross-platform UI framework that enables development of applications running on HarmonyOS, Android, and iOS from a single codebase. It provides a declarative UI framework using ArkTS (TypeScript-based) with native performance through C++ rendering engines.

## Knowledge Base

This project maintains a comprehensive knowledge base system for in-depth component analysis and development guidance.

**Location**: `knowledge/`

### Quick Reference

| Topic | Document |
|-------|----------|
| System Architecture | `knowledge/architecture/system_architecture.md` |
| Plugin Adaptation | `knowledge/plugin_adaptation/adapter_guide.md` |
| Development Guide | `knowledge/best_practices/development_guide.md` |

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

## Environment Setup

Quick setup guide - see [最佳实践 - 环境配置](knowledge/best_practices/development_guide.md#环境配置) for details.

### Prerequisites

**System Requirements:**
- Linux: Ubuntu 18.04+ 64-bit (recommended) or macOS 11.6.2+
- Python 3.7.4+
- JDK 17.0.10 or later
- bash shell (not dash/sh)

### Quick Start

```bash
# Linux dependencies
sudo apt-get update
sudo apt-get install binutils git-core gnupg flex bison gperf build-essential \
    zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
    lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev \
    ccache libgl1-mesa-dev libxml2-utils xsltproc unzip m4

# Set JAVA_HOME
export JAVA_HOME=/path/to/java-sdk
export PATH=${JAVA_HOME}/bin:${PATH}

# Download prebuilts
./build/prebuilts_download.sh --build-arkuix --skip-ssl
```

For detailed setup instructions, see [最佳实践文档](knowledge/best_practices/development_guide.md)。

## Build System

This project uses **GN (Generate Ninja)** as the meta-build system with Python-based build tooling.

### Main Build Commands

```bash
# Build for Android
./build.sh --product-name arkui-x --target-os android

# Build for iOS
./build.sh --product-name arkui-x --target-os ios

# Build specific component
./build.sh --product-name arkui-x --target-os android --build-target ace_engine

# Debug build
./build.sh --product-name arkui-x --target-os android --log-level debug
```

### Build Artifacts

Build outputs are placed in `out/[product]/` directory.

For detailed build instructions, see [最佳实践 - 构建系统](knowledge/best_practices/development_guide.md#构建系统)。

## Architecture

详见：[系统架构文档](knowledge/architecture/system_architecture.md)

### 快速参考

**核心层次**：
- 应用层 → 前端桥接层 → 组件框架层 → 图形引擎层 → 平台适配层 → 原生平台层

**关键组件**：
- **Frontend**: declarative_frontend, arkts_frontend, js_frontend
- **Components NG**: Pattern, Layout, Render, Gesture
- **Graphics**: Rosen 2D, Skia, OpenGL ES
- **Adapter**: Android, iOS 平台适配

**主要目录**：
```
arkuix-master/
├── arkcompiler/              # 编译工具链
│   ├── ets_frontend/         # ArkTS 前端编译器
│   ├── ets_runtime/          # ETS 运行时
│   └── runtime_core/         # 核心运行时
├── foundation/               # 基础框架
│   ├── arkui/ace_engine/     # UI 框架引擎
│   ├── graphic/              # 图形引擎
│   └── appframework/         # 应用框架
├── plugins/                  # 平台插件 (50+ 模块)
├── interface/sdk-js/         # JavaScript SDK
├── build/                    # OpenHarmony 构建系统
├── build_plugins/            # ArkUI-X 构建系统
├── base/                     # 基础服务
├── commonlibrary/            # 公共库
├── developtools/             # 开发工具
├── test/                     # 测试框架
└── third_party/              # 第三方依赖
```

### Component Lifecycle

```
Application Start → Ability Initialization → Stage/Page Initialization
→ Page Loading → Component Tree Building → Layout & Rendering
```

详见：[系统架构 - 组件生命周期](docs/knowledge/architecture/system_architecture.md#组件生命周期)

### Cross-Platform Adaptation Architecture

```
@ohos.* API Layer → NAPI Binding → Business Logic → Platform Interface
→ Platform Implementation (Android/iOS)
```

详见：[系统架构 - 跨平台适配架构](docs/knowledge/architecture/system_architecture.md#跨平台适配架构)

### Key Architecture Principles

1. **Layer Separation**: Clear boundaries between layers
2. **Platform Abstraction**: Pure virtual interfaces
3. **Cross-Platform Sharing**: Maximum code reuse
4. **Minimal Platform APIs**: Essential platform services only
5. **Declarative UI**: ArkTS/TypeScript with reactive state
6. **Component-Based**: Pattern-Model-Property separation
7. **Performance First**: Native rendering with Rosen/Skia
8. **Gradual Migration**: Support for legacy JavaScript

### Key Components

**IMPORTANT FOR CLAUDE**: This section lists all major repositories and their CLAUDE.md documentation paths.

| 组件 | 路径 | 说明 |
|------|-----|-----|
| **ACE Engine** | `foundation/arkui/ace_engine/` | 核心 UI 框架引擎 |
| **App Framework** | `foundation/appframework/` | 应用框架 |
| **NAPI** | `foundation/arkui/napi` | NAPI 绑定层 |
| **Graphic 2D** | `foundation/graphic/graphic_2d/` | Rosen 2D 图形引擎 |
| **Plugins** | `plugins/` | 平台插件 (50+ 适配器) |
| **Build System** | `build/`, `build_plugins/` | 构建系统 |

## Cross-Platform Development Guide

详见完整指南：
- [插件子系统适配指南](docs/knowledge/plugin_adaptation/README.md)
- [开发最佳实践](docs/knowledge/best_practices/README.md)

### Core Principles

ArkUI-X cross-platform architecture follows three fundamental principles:

1. **Maximize OHOS Reuse**: Reuse OpenHarmony's ArkTS frontend, graphics engine, and runtime
2. **Adapter-Based Architecture**: Use adapter layers for framework capabilities (window, input, font, etc.)
3. **Plugin-Based Subsystem APIs**: Use plugin architecture for subsystem APIs (HTTP, FS, data, BLE, etc.)

### Development Workflow

```
1. Identify Feature Type → 2. Choose Adaptation Strategy → 3. Implement Code
→ 4. Configure Build System → 5. Add Tests & Documentation
```

### Quick Reference

**Framework Capabilities (Adapter Layer)**:
- Location: `foundation/arkui/ace_engine/adapter/android|ios/`
- Examples: Window, Input, Font, Keyboard management

**Subsystem APIs (Plugin Layer)**:
- Location: `plugins/[module_name]/`
- Examples: HTTP, File System, Data Storage, Sensors
- 完整指南：[插件适配指南](docs/knowledge/plugin_adaptation/README.md)

### Adaptation Checklist

For subsystem API adaptation, see the complete checklist in [插件适配指南](docs/knowledge/plugin_adaptation/README.md#检查清单)。

## Testing

### XTS Test Suite

```bash
# Android XTS tests
cd test/xts/android/xts_2.0
./run.sh {Module name}

# Build specific test
./build.sh --product-name arkui-x --target-os android --build-target {test_target}
```

**Test Location:** `test/xts/` - Contains platform-specific test suites

详见：[最佳实践 - 测试与调试](docs/knowledge/best_practices/README.md#测试与调试)

## Documentation

Comprehensive documentation is available in the `docs/` directory:
- `docs/en/` - English documentation
- `docs/zh-cn/` - Chinese documentation

## Important File Locations

- `BUILD.gn` files - Build definitions (throughout source tree)
- `.gn` - GN configuration (symlink to `build/core/gn/dotfile.gn`)
- `bundle.json` - Component/package metadata
- `build_plugins/build_scripts/build.sh` - Main build script
- `build/hb/main.py` - HB build tool entry point

## License

Apache License 2.0 - See LICENSE file in community/ directory

## Compilation Macros and Build Flags

### Platform Detection Macros

```cpp
IS_ARKUI_X_TARGET        // Cross-platform build target
ANDROID_PLATFORM         // Android-specific adaptations
CROSS_PLATFORM           // Cross-platform compatibility layer
ROSEN_ANDROID           // Rosen graphics backend on Android
```

### Feature Toggle Macros

| Macro | Purpose | Usage Frequency |
|-------|---------|-----------------|
| `ENABLE_ROSEN_BACKEND` | Rosen graphics rendering backend | 1360+ files |
| `ENABLE_DRAG_FRAMEWORK` | Drag-and-drop support | All platforms |
| `PLATFORM_VIEW_SUPPORTED` | Platform view component | 1360+ files |
| `VIDEO_SUPPORTED` | Video playback support | All platforms |

### Compiler Optimization Flags

```bash
# Size optimization (already enabled)
-Os                      # Optimize for size (2190 files)
-ffunction-sections      # Separate functions (5131 files)
-fdata-sections          # Separate data (3131 files)
-fvisibility=hidden      # Hide symbols by default
-Wl,--gc-sections        # Linker dead code elimination
```

### Critical Include Patterns

```cpp
// For ACE framework APIs
#include "core/pipeline_ng/ui_task_scheduler.h"
#include "interfaces/inner_api/ace/ace_view.h"

// For platform adapters
#ifdef ANDROID
#include "adapter/android/entrance/java/jni/ace_view_android.h"
#elif defined(IOS)
#include "adapter/ios/entrance/ace_view_ios.h"
#endif

// For graphics
#include "roseEngine/modules/rosen/modules/render_service_client/core/..."
```

## Symbol Visibility and Export Strategy

The framework uses **default-hidden** symbol visibility:

```cpp
// Export macros
#define ACE_EXPORT __attribute__((visibility("default")))

// Public API
ACE_EXPORT void PublicFunction();
```

**Rule:** Only symbols explicitly marked `visibility("default")` are exported.

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

### Feature Guards

```cpp
#ifdef ENABLE_ROSEN_BACKEND
    // Rosen rendering backend
#endif

#ifdef QRCODEGEN_SUPPORT
    // QR code functionality
#endif
```

## SDK Size Optimization Guidelines

### Current Optimization State

✅ **Already Enabled:**
- `-Os` flag (size optimization)
- `-ffunction-sections` + `-fdata-sections`
- `-fvisibility=hidden` (symbol hiding)
- `-Wl,--gc-sections` (linker dead code elimination)
- `NDEBUG` defined (disables assert)

⚠️ **Potential Improvements:**
- Disable `ACE_INSTANCE_LOG` (adds 2-5% size)
- Strip debug symbols (10-20% reduction)
- Ensure `NDEBUG` everywhere

详见：[最佳实践 - 性能优化](docs/knowledge/best_practices/README.md#性能优化)

## Cross-Platform API Adaptation Rules

详见完整指南：[插件子系统适配指南](docs/knowledge/plugin_adaptation/README.md)

### Quick Reference

**13 Core Rules**:
1. **Layered Architecture** - 5-layer architecture
2. **Platform Isolation** - Directory separation
3. **Minimal JNI Calls** - Cache references
4. **Unified Async Model** - Promise/Deferred pattern
5. **Template-Based GN Build** - Multi-platform templates
6. **Configuration Separation** - Separate .gni files
7. **Clear Dependencies** - Explicit dependency declaration
8. **Interface Abstraction** - Pure virtual interfaces
9. **Minimal Conditional Compilation** - Prefer separate files
10. **Unified Error Handling** - Platform-independent codes
11. **Zero-Copy Data Transfer** - std::string_view and move semantics
12. **Smart Pointer Lifecycle** - shared_ptr/unique_ptr
13. **Thread Safety** - Thread-safe containers and mutexes

### Key Checklist

When adapting new @ohos APIs:

- [ ] 5-layer architecture implemented
- [ ] Platform isolation via directory structure
- [ ] JNI minimized with cached references
- [ ] Async model: Promise/Deferred pattern
- [ ] GN template-based build
- [ ] Tested on Android and iOS

### Reference Implementation

**Best reference**: `plugins/net/http`

| Aspect | File |
|--------|------|
| API Definition | `interface/sdk-js/api/@ohos.net.http.d.ts` |
| NAPI Binding | `plugins/net/http/http_module.cpp` |
| Platform Interface | `plugins/net/http/http_exec_interface.h` |
| Android Implementation | `plugins/net/http/android/http_exec.cpp` |
| iOS Implementation | `plugins/net/http/ios/http_exec.cpp` |

### Applicable Modules

- **Network**: @ohos.net.http, @ohos.net.socket, @ohos.net.webSocket
- **Storage**: @ohos.data.preferences, @ohos.data.relationalStore
- **System**: @ohos.file.fs, @ohos.sensor, @ohos.device.info
- **Multimedia**: @ohos.multimedia.image, @ohos.multimedia.player

详见：[插件适配指南](docs/knowledge/plugin_adaptation/README.md)
