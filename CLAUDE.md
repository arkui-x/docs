# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

ArkUI-X is a cross-platform UI framework that enables development of applications running on HarmonyOS, Android, and iOS from a single codebase. It provides a declarative UI framework using ArkTS (TypeScript-based) with native performance through C++ rendering engines.

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

### Prerequisites

**System Requirements:**

- Linux: Ubuntu 18.04+ 64-bit (recommended) or macOS 11.6.2+
- Python 3.7.4+
- JDK 17.0.10 or later
- bash shell (not dash/sh) - run `sudo dpkg-reconfigure dash` and select "no" if needed

**Linux Dependencies:**

```bash
sudo apt-get update
sudo apt-get install binutils git-core gnupg flex bison gperf build-essential \
    zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
    lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev \
    ccache libgl1-mesa-dev libxml2-utils xsltproc unzip m4
```

**macOS Dependencies:**

```bash
brew install wget coreutils
xcode-select --install  # For Command Line Tools
```

### Java Environment

```bash
# Download JDK 17.0.10 or later from:
# https://repo.huaweicloud.com/openjdk/

# Set JAVA_HOME (Linux)
export JAVA_HOME=/path/to/java-sdk
export PATH=${JAVA_HOME}/bin:${PATH}

# Set JAVA_HOME (macOS)
export JAVA_HOME=/Users/username/path-to-java-sdk
export PATH=$JAVA_HOME/bin:$PATH
```

### Android SDK (for Android builds)

```bash
# Download Android SDK command line tools
# Use sdkmanager to install required packages:

./sdkmanager --install "ndk;21.3.6528147" --sdk_root=/path/to-android-sdk
./sdkmanager --install "platforms;android-26" --sdk_root=/path/to-android-sdk
./sdkmanager --install "build-tools;28.0.3" --sdk_root=/path/to-android-sdk

# Set environment variables
export ANDROID_HOME=/path/to-android-sdk
export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
```

### Download Prebuilts

After code checkout or update, download prebuilt toolchains:

```bash
./build/prebuilts_download.sh --build-arkuix --skip-ssl
```

## Build System

This project uses **GN (Generate Ninja)** as the meta-build system with Python-based build tooling.

### Main Build Commands

```bash
# Main entry point - builds the entire project
./build.sh [options]

# Key build options:
#   --product-name <product>    # Specific product to build (default: arkui-x)
#   --target-os <os>            # Target OS (android, ios)
#   --build-target <target>     # Specific build target
#   --target-cpu <arch>         # CPU architecture (arm, arm64, x86_64)
#   --build-type <type>         # release or debug
#   --gn-args                  # Additional GN parameters
#   --ninja-args               # Additional Ninja parameters (e.g., -dkeeprsp for deps)
#   --log-level                # Log level (info, debug)
#   --help, -h                 # Help command

# Example: Build for Android with debug info
./build.sh --product-name arkui-x --target-os android 

# Example: Build for iOS
./build.sh --product-name arkui-x --target-os ios

# Example: Build specific component
./build.sh --product-name arkui-x --target-os android --build-target ace_engine

# Example: Build with custom GN args
./build.sh --product-name arkui-x --target-os android --gn-args="enable_profiler=true"
```

The main `build.sh` is a symlink to `build_plugins/build_scripts/build.sh`, which wraps the HB build tool.

### Build Artifacts

Build outputs are placed in `out/` directory with the product name.

### Available Products

The default product is `arkui-x` (defined in `productdefine/common/products/arkui-x.json`).

## Architecture

### Overall System Architecture

ArkUI-X implements a **cross-platform UI framework** with a layered architecture that enables application development in ArkTS/TypeScript with native performance on Android, iOS, and HarmonyOS platforms.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                        Application Layer                                    │
│                     ArkTS/TypeScript Applications                           │
│  • Declarative UI (@Component, @Builder, @State)                           │
│  • State Management (@Watch, @Link, @Prop)                                │
│  • Application Lifecycle (onCreate, onDestroy, onShow, onHide)             │
└─────────────────────────────────────────────────────────────────────────────┘
                                        ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                         Frontend Bridge Layer                              │
│                   (foundation/arkui/ace_engine/frameworks/bridge)          │
│  • Declarative Frontend: ArkTS/TS parser → Component tree                 │
│  • ArkTS Frontend: Static ArkTS compilation                              │
│  • JavaScript Frontend: Legacy JS support (V8)                           │
│  • State Management: AppStorage, LocalStorage, @Watch/@Link/@Prop        │
└─────────────────────────────────────────────────────────────────────────────┘
                                        ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                       Component Framework Layer                           │
│                 (foundation/arkui/ace_engine/frameworks/core)             │
│  • Components NG: Modern component architecture                          │
│    - Pattern Layer: Business logic & lifecycle                           │
│    - Layout Algorithm: Flexbox, Grid, Absolute                           │
│    - Render Pipeline: Drawing and display                                │
│    - Gesture Recognition: Touch, keyboard, mouse                         │
│  • ViewAbstract: Base view with common properties                       │
│  • FrameNode: Component container (Pattern + Element + RenderNode)       │
└─────────────────────────────────────────────────────────────────────────────┘
                                        ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                        Graphics Engine Layer                              │
│                  (foundation/graphic/ + third_party/)                    │
│  • Rosen 2D: 2D graphics rendering engine                                │
│  • Rosen Surface: Display buffer management                              │
│  • Skia: 2D graphics library (fallback)                                  │
│  • OpenGL ES: 3D graphics support                                        │
│  • VSync Sync: Display frame synchronization                             │
└─────────────────────────────────────────────────────────────────────────────┘
                                        ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                      Platform Adapter Layer                               │
│            (foundation/arkui/ace_engine/adapter/ + plugins/)             │
│  • Android Adapter: JNI bridge + Java/Kotlin implementation             │
│  • iOS Adapter: Objective-C++ bridge + Objective-C/Swift implementation  │
│  • Native Platform APIs:                                                 │
│    - Android: Activity, SurfaceView, AudioManager, etc.                 │
│    - iOS: UIViewController, UIView, AVAudioSession, etc.                │
└─────────────────────────────────────────────────────────────────────────────┘
                                        ↓
┌─────────────────────────────────────────────────────────────────────────────┐
│                       Native Platform Layer                               │
│                Android (AOSP) / iOS (Apple) / HarmonyOS                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

### Directory Structure Mapping

```
arkuix6.0/
├── foundation/                          # Foundation layer modules
│   ├── arkui/ace_engine/               # ✅ Core UI framework engine
│   │   ├── frameworks/
│   │   │   ├── bridge/                 # Frontend bridge layer
│   │   │   │   ├── declarative_frontend/  # ArkTS/TS declarative UI
│   │   │   │   ├── arkts_frontend/        # ArkTS static support
│   │   │   │   └── js_frontend/           # Legacy JS support
│   │   │   └── core/                   # Component framework layer
│   │   │       ├── components_ng/        # Modern components
│   │   │       ├── pipeline/             # Rendering pipeline
│   │   │       └── gesture/             # Gesture recognition
│   │   ├── adapter/                    # Platform adapters
│   │   │   ├── android/                 # Android platform
│   │   │   └── ios/                     # iOS platform
│   │   └── interfaces/                 # Public APIs
│   ├── appframework/                   # Application lifecycle
│   ├── graphic/                        # Graphics rendering
│   │   ├── graphic_2d/                 # Rosen 2D engine
│   │   ├── graphic_surface/            # Surface management
│   │   └── graphics_effect/            # Graphics effects
│   ├── multimedia/                     # Media frameworks
│   │   ├── audio_framework/            # Audio capture/playback
│   │   ├── av_codec/                   # Audio/video codecs
│   │   ├── image_framework/            # Image processing
│   │   └── player_framework/           # Media player
│   ├── communication/                  # Network & communication
│   │   ├── bluetooth/                  # Bluetooth
│   │   ├── netmanager_base/            # Network management
│   │   └── netstack/                   # Network stack
│   ├── distributeddatamgr/             # Data management
│   │   ├── kv_store/                   # Key-value storage
│   │   ├── preferences/                # Preferences
│   │   ├── relational_store/           # RDB database
│   │   └── pasteboard/                 # Clipboard
│   ├── multimodalinput/               # Input handling
│   └── filemanagement/                # File system API
│
├── arkcompiler/                        # Compiler toolchain
│   ├── ets_frontend/                   # ArkTS frontend compiler
│   ├── ets_runtime/                    # ETS runtime execution
│   ├── runtime_core/                   # Core runtime support
│   └── toolchain/                      # Compiler utilities
│
├── plugins/                            # Platform-specific adapters (50+ modules)
│   ├── android/                        # Android implementations
│   │   ├── *.java                      # Java/Kotlin code
│   │   └── jni/                        # JNI bridge
│   ├── ios/                            # iOS implementations
│   │   └── *.mm                        # Objective-C++ code
│   ├── common/                         # Shared C++ code
│   └── [module_name]/                  # Per-module adapters
│       ├── bluetooth/                  # Bluetooth adapter
│       ├── data/                       # Data adapters (kv_store, etc.)
│       ├── file/                       # File adapter
│       └── ...                         # 50+ adapters
│
├── interface/                          # API definitions
│   ├── sdk/                            # Native SDK interfaces
│   └── sdk-js/                         # JavaScript SDK (@ohos.* APIs)
│       └── api/                        # API type definitions
│
├── build/                              # OpenHarmony build system
│   └── core/gn/                        # GN build configuration
│       └── dotfile.gn                  # GN configuration (linked as .gn)
│
├── build_plugins/                      # ArkUI-X build system
│   └── build_scripts/                  # Build scripts
│       └── build.sh                    # Main build script (linked as build.sh)
│
├── base/                               # Base services
│   ├── hiviewdfx/                      # Logging & tracing
│   │   ├── hilog/                      # System logging
│   │   └── hitrace/                    # Performance tracing
│   ├── security/                       # Security frameworks
│   ├── notification/                   # Event handling
│   └── global/                         # Global resources
│
├── commonlibrary/                      # Common libraries
│   ├── c_utils/                        # C utilities
│   ├── ets_utils/                      # ETS utilities
│   └── memory_utils/                   # Memory utilities
│
├── developtools/                       # Development tools
│   ├── ace_ets2bundle/                 # ETS compiler
│   ├── ace_js2bundle/                  # JS compiler
│   ├── profiler/                       # Performance profiler
│   └── hapsigner/                      # HAP signing tool
│
├── test/                               # Test frameworks
│   ├── testfwk/arkxtest/               # Test framework
│   └── xts/                            # XTS compatibility tests
│
├── third_party/                        # Third-party dependencies
│   ├── skia/                           # 2D graphics
│   ├── icu/                            # Internationalization
│   ├── openssl/                        # SSL/TLS
│   ├── curl/                           # HTTP client
│   └── ...                             # 30+ libraries
│
├── samples/                            # Sample applications
├── docs/                               # Documentation
└── productdefine/                      # Product definitions
```

### Component Lifecycle Architecture

```
Application Start
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 1. Ability Initialization                                       │
│    • StageApplication.onCreate()                               │
│    • AceEnv::getInstance() - Load native libraries              │
│    • CreateRuntime() - Initialize JS runtime (ArkJsRuntime)     │
│    • PreloadAceModule() - Preload core modules                  │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 2. Stage/Page Initialization                                    │
│    • StageActivity.onCreate() (Android) / iOS equivalent       │
│    • Attach stage activity, set window view                     │
│    • Create ResourceManager (app + system resources)            │
│    • Create JsAbilityStage                                      │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 3. Page Loading (loadContent)                                   │
│    • UIContent::Create()                                        │
│    • AceContainer creation (keyed by instanceId)               │
│    • PipelineContext initialization                             │
│    • Frontend attachment (DeclarativeFrontendNG)               │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 4. Component Tree Building                                      │
│    • Parse ArkTS code → Component description                   │
│    • Create FrameNode instances                                 │
│    • Establish parent-child relationships                       │
│    • Initialize Pattern, Properties, EventHub                  │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│ 5. Layout & Rendering                                           │
│    • Measure: Calculate component sizes                         │
│    • Layout: Position components                                │
│    • Render: Draw using Rosen/Skia graphics engine             │
│    • VSync: Display frame synchronization                       │
└─────────────────────────────────────────────────────────────────┘
```

### Cross-Platform Adaptation Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│           Unified @ohos.* API Layer (TypeScript .d.ts)          │
│                  interface/sdk-js/api/                         │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│                 NAPI Binding Layer (C++)                        │
│        Parameter parsing, type conversion, async handling       │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│            Business Logic Layer (Platform-Independent)          │
│              Core algorithms, state management                  │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌──────────────────────┬────────────────────────────────────────┐
│  Platform Interface  │  Pure virtual C++ interface             │
│     (C++ Header)     │  - IFeatureStrategy                    │
└──────────────────────┴────────────────────────────────────────┘
                           ↓
        ┌──────────────────┴──────────────────┐
        ↓                                     ↓
┌─────────────────────┐          ┌─────────────────────┐
│  Android Platform   │          │   iOS Platform      │
│     Implementation  │          │   Implementation    │
├─────────────────────┤          ├─────────────────────┤
│ • JNI Bridge        │          │ • ObjC++ Bridge      │
│ • Java/Kotlin Impl  │          │ • ObjC/Swift Impl   │
│ • AOSP APIs         │          │ • Apple Frameworks  │
│   - Activity        │          │   - UIKit           │
│   - SurfaceView     │          │   - UIView          │
│   - AudioManager    │          │   - AVFoundation    │
└─────────────────────┘          └─────────────────────┘
```

### Build System Architecture

```
build.sh (Main Entry Point)
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  HB Build Tool (build/hb/main.py)                               │
│  • Parse command line arguments                                │
│  • Load product configuration                                  │
│  • Execute build workflow                                      │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  GN (Generate Ninja)                                            │
│  • Parse .gn configuration                                     │
│  • Process BUILD.gn files                                       │
│  • Generate ninja.build files                                  │
│  • Resolve dependencies                                        │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  Ninja Build Executor                                           │
│  • Execute build tasks                                          │
│  • Parallel compilation                                         │
│  • Link libraries                                               │
│  • Generate outputs                                            │
└─────────────────────────────────────────────────────────────────┘
      ↓
┌─────────────────────────────────────────────────────────────────┐
│  Build Outputs (out/[product]/)                                │
│  • libace*.z.so - ACE engine libraries                         │
│  • libarkui_*.z.so - Component libraries                       │
│  • *.abc - ArkTS bytecode files                                 │
│  • APK/IPA - Platform-specific packages                         │
└─────────────────────────────────────────────────────────────────┘
```

### Multi-Module Compilation Flow

```
For each module in ace_platforms (android, ios):
  ┌────────────────────────────────────────┐
  │ 1. Parse module's BUILD.gn            │
  │    - platform_sources (common)        │
  │    - platform_sources (android)       │
  │    - platform_sources (ios)           │
  └────────────────────────────────────────┘
              ↓
  ┌────────────────────────────────────────┐
  │ 2. Add conditional compilation         │
  │    - ANDROID_PLATFORM macro            │
  │    - IOS_PLATFORM macro               │
  └────────────────────────────────────────┘
              ↓
  ┌────────────────────────────────────────┐
  │ 3. Link platform-specific libs        │
  │    - Android: JNI, Java libs          │
  │    - iOS: Objective-C frameworks      │
  └────────────────────────────────────────┘
              ↓
  ┌────────────────────────────────────────┐
  │ 4. Package output                     │
  │    - Android: .so + .jar              │
  │    - iOS: .framework / .dylib         │
  └────────────────────────────────────────┘
```

### Key Architecture Principles

1. **Layer Separation**: Clear boundaries between frontend, framework, graphics, and platform layers
2. **Platform Abstraction**: Pure virtual interfaces isolate platform-specific implementations
3. **Cross-Platform Sharing**: Maximum code reuse in common C++ layers
4. **Minimal Platform APIs**: JNI/ObjC++ bridges only for essential platform services
5. **Declarative UI**: ArkTS/TypeScript declarative syntax with reactive state management
6. **Component-Based**: Modern component architecture with Pattern-Model-Property separation
7. **Performance First**: Native rendering with Rosen/Skia, optimized layout algorithms
8. **Gradual Migration**: Support for legacy JavaScript with V8 runtime

### Key Components

**IMPORTANT FOR CLAUDE**: This section lists all major repositories and their CLAUDE.md documentation paths. During initialization, read the CLAUDE.md files from these paths to get detailed context for each sub-project.

#### Core Framework Layer

**foundation/arkui/ace_engine/** - Core UI framework engine

- Declarative UI runtime, component system, state management, animation framework
- See [CLAUDE.md](foundation/arkui/ace_engine/CLAUDE.md) for detailed implementation

**foundation/appframework/** - Application framework

- Ability lifecycle management, application startup and initialization, resource management
- See [CLAUDE.md](foundation/appframework/CLAUDE.md) for detailed implementation

**foundation/arkui/napi** - NAPI binding layer

- Native API integration layer
- See [CLAUDE.md](foundation/arkui/napi/CLAUDE.md) for detailed implementation

#### Compiler Toolchain

**arkcompiler/ets_frontend/** - ArkTS/TypeScript frontend compiler

- ArkTS to bytecode compilation
- See [CLAUDE.md](arkcompiler/ets_frontend/CLAUDE.md) for detailed implementation

**arkcompiler/ets_runtime/** - ETS runtime execution engine

- Runtime execution engine for ETS bytecode
- See [CLAUDE.md](arkcompiler/ets_runtime/CLAUDE.md) for detailed implementation

**arkcompiler/runtime_core/** - Core runtime support

- Core runtime infrastructure
- See [CLAUDE.md](arkcompiler/runtime_core/CLAUDE.md) for detailed implementation

**arkcompiler/toolchain/** - Compiler toolchain utilities

- Compiler tools and utilities
- See [CLAUDE.md](arkcompiler/toolchain/CLAUDE.md) for detailed implementation

#### Graphics & Rendering

**foundation/graphic/graphic_2d/** - 2D graphics rendering engine

- Rosen 2D graphics rendering, drawing APIs
- See [CLAUDE.md](foundation/graphic/graphic_2d/CLAUDE.md) for detailed implementation

**foundation/graphic/graphic_surface/** - Surface management

- Display buffers and surface management
- See [CLAUDE.md](foundation/graphic/graphic_surface/CLAUDE.md) for detailed implementation

**foundation/graphic/graphics_effect/** - Graphics effects

- Graphics effects and filters
- See [CLAUDE.md](foundation/graphic/graphics_effect/CLAUDE.md) for detailed implementation

#### Multimedia Framework

**foundation/multimedia/audio_framework/** - Audio framework

- Audio capture and playback
- See [CLAUDE.md](foundation/multimedia/audio_framework/CLAUDE.md) for detailed implementation

**foundation/multimedia/av_codec/** - Audio/video codec

- Audio/video codec support
- See [CLAUDE.md](foundation/multimedia/av_codec/CLAUDE.md) for detailed implementation

**foundation/multimedia/av_session/** - AV session management

- Audio/video session management
- See [CLAUDE.md](foundation/multimedia/av_session/CLAUDE.md) for detailed implementation

**foundation/multimedia/image_framework/** - Image framework

- Image encoding/decoding and processing
- See [CLAUDE.md](foundation/multimedia/image_framework/CLAUDE.md) for detailed implementation

**foundation/multimedia/media_foundation/** - Media foundation

- Multimedia foundation libraries
- See [CLAUDE.md](foundation/multimedia/media_foundation/CLAUDE.md) for detailed implementation

**foundation/multimedia/player_framework/** - Player framework

- Media player implementation
- See [CLAUDE.md](foundation/multimedia/player_framework/CLAUDE.md) for detailed implementation

**foundation/multimedia/drm_framework/** - DRM framework

- Digital Rights Management support
- See [CLAUDE.md](foundation/multimedia/drm_framework/CLAUDE.md) for detailed implementation

#### Communication & Networking

**foundation/communication/bluetooth/** - Bluetooth

- Bluetooth connectivity and communication
- See [CLAUDE.md](foundation/communication/bluetooth/CLAUDE.md) for detailed implementation

**foundation/communication/netmanager_base/** - Network manager base

- Network management base framework
- See [CLAUDE.md](foundation/communication/netmanager_base/CLAUDE.md) for detailed implementation

**foundation/communication/netstack/** - Network stack

- Network protocol stack
- See [CLAUDE.md](foundation/communication/netstack/CLAUDE.md) for detailed implementation

#### Data Management

**foundation/distributeddatamgr/kv_store/** - KV Store

- Key-value storage (cross-platform)
- See [CLAUDE.md](foundation/distributeddatamgr/kv_store/CLAUDE.md) for detailed implementation

**foundation/distributeddatamgr/preferences/** - Preferences

- Preferences data storage
- See [CLAUDE.md](foundation/distributeddatamgr/preferences/CLAUDE.md) for detailed implementation

**foundation/distributeddatamgr/relational_store/** - Relational Store

- Relational database (RDB) support
- See [CLAUDE.md](foundation/distributeddatamgr/relational_store/CLAUDE.md) for detailed implementation

**foundation/distributeddatamgr/udmf/** - UDMF

- Unified data management format
- See [CLAUDE.md](foundation/distributeddatamgr/udmf/CLAUDE.md) for detailed implementation

**foundation/distributeddatamgr/pasteboard/** - Pasteboard

- Clipboard/copy-paste functionality
- See [CLAUDE.md](foundation/distributeddatamgr/pasteboard/CLAUDE.md) for detailed implementation

#### File Management

**foundation/filemanagement/file_api/** - File API

- File system API and operations
- See [CLAUDE.md](foundation/filemanagement/file_api/CLAUDE.md) for detailed implementation

#### Platform Abstraction Layer (CRITICAL)

**plugins/** - Platform-specific implementations (50+ adapters)

- Internal platform adapters for Android/iOS
- Contains: `plugins/android/` (Android bridge), `plugins/ios/` (iOS bridge)
- Each adapter implements @ohos APIs using native platform capabilities
- **IMPORTANT**: NOT third-party plugins (those are via OHPM)
- Build rules: `plugins/plugin_lib.gni`
- See [CLAUDE.md](plugins/CLAUDE.md) for detailed implementation

**foundation/arkui/ace_engine/adapter/android/** - Android adapter

- Android-specific adaptations for ACE engine
- See [CLAUDE.md](foundation/arkui/ace_engine/adapter/android/CLAUDE.md) for detailed implementation

**foundation/arkui/ace_engine/adapter/ios/** - iOS adapter

- iOS-specific adaptations for ACE engine
- See [CLAUDE.md](foundation/arkui/ace_engine/adapter/ios/CLAUDE.md) for detailed implementation

#### Input & Interaction

**foundation/multimodalinput/input/** - Input handling

- Multi-modal input (touch, keyboard, mouse)
- See [CLAUDE.md](foundation/multimodalinput/input/CLAUDE.md) for detailed implementation

**base/msdp/device_status/** - Device status

- Device status monitoring
- See [CLAUDE.md](base/msdp/device_status/CLAUDE.md) for detailed implementation

#### Development Tools

**developtools/ace_ets2bundle/** - ETS to bundle compiler

- ETS bytecode compiler
- See [CLAUDE.md](developtools/ace_ets2bundle/CLAUDE.md) for detailed implementation

**developtools/ace_js2bundle/** - JS to bundle compiler

- JavaScript bundle compiler
- See [CLAUDE.md](developtools/ace_js2bundle/CLAUDE.md) for detailed implementation

**developtools/global_resource_tool/** - Resource tool

- Global resource management tool
- See [CLAUDE.md](developtools/global_resource_tool/CLAUDE.md) for detailed implementation

**developtools/hapsigner/** - HAP signer

- HAP signing tool
- See [CLAUDE.md](developtools/hapsigner/CLAUDE.md) for detailed implementation

**developtools/packing_tool/** - Packing tool

- Application packaging tool
- See [CLAUDE.md](developtools/packing_tool/CLAUDE.md) for detailed implementation

**developtools/profiler/** - Profiler

- Performance profiling tools
- See [CLAUDE.md](developtools/profiler/CLAUDE.md) for detailed implementation

**developtools/integration_verification/** - Integration verification

- Integration verification tools
- See [CLAUDE.md](developtools/integration_verification/CLAUDE.md) for detailed implementation

**developtools/ace_tools/** - CLI tools

- Command-line interface tools
- See [CLAUDE.md](developtools/ace_tools/CLAUDE.md) for detailed implementation

#### Build System

**build/** - OpenHarmony build system

- GN templates, toolchains, product configurations
- See [CLAUDE.md](build/CLAUDE.md) for detailed implementation

**build_plugins/** - ArkUI-X plugin build system

- Build scripts, cross-platform SDK configuration
- See [CLAUDE.md](build_plugins/CLAUDE.md) for detailed implementation

#### Testing Framework

**test/testfwk/arkxtest/** - Test framework

- ArkUI-X test framework (xdevice)
- See [CLAUDE.md](test/testfwk/arkxtest/CLAUDE.md) for detailed implementation

**test/xts/** - XTS tests

- Cross-platform compatibility tests
- See [CLAUDE.md](test/xts/CLAUDE.md) for detailed implementation

#### SDK & API Definitions

**interface/sdk/** - Native SDK

- Native SDK interfaces
- See [CLAUDE.md](interface/sdk/CLAUDE.md) for detailed implementation

**interface/sdk-js/** - JavaScript SDK

- JavaScript SDK and public APIs (@ohos.* modules)
- See [CLAUDE.md](interface/sdk-js/CLAUDE.md) for detailed implementation

#### Common Libraries

**commonlibrary/c_utils/** - C utilities

- C utility functions
- See [CLAUDE.md](commonlibrary/c_utils/CLAUDE.md) for detailed implementation

**commonlibrary/ets_utils/** - ETS utilities

- ETS utility functions
- See [CLAUDE.md](commonlibrary/ets_utils/CLAUDE.md) for detailed implementation

**commonlibrary/memory_utils/** - Memory utilities

- Memory management utilities
- See [CLAUDE.md](commonlibrary/memory_utils/CLAUDE.md) for detailed implementation

**commonlibrary/utils_lite/** - Lite utilities

- Lightweight utility functions
- See [CLAUDE.md](commonlibrary/utils_lite/CLAUDE.md) for detailed implementation

#### Base Services

**base/hiviewdfx/hilog/** - Logging system

- System logging (HiLog)
- See [CLAUDE.md](base/hiviewdfx/hilog/CLAUDE.md) for detailed implementation

**base/hiviewdfx/hitrace/** - Trace

- Trace and performance analysis
- See [CLAUDE.md](base/hiviewdfx/hitrace/CLAUDE.md) for detailed implementation

**base/global/resource_management/** - Resource management

- Global resource management
- See [CLAUDE.md](base/global/resource_management/CLAUDE.md) for detailed implementation

**base/global/system_resources/** - System resources

- System resource definitions
- See [CLAUDE.md](base/global/system_resources/CLAUDE.md) for detailed implementation

**base/notification/eventhandler/** - Event handler

- Event handling framework
- See [CLAUDE.md](base/notification/eventhandler/CLAUDE.md) for detailed implementation

**base/security/certificate_framework/** - Certificate framework

- Certificate management
- See [CLAUDE.md](base/security/certificate_framework/CLAUDE.md) for detailed implementation

**base/security/crypto_framework/** - Crypto framework

- Cryptographic operations
- See [CLAUDE.md](base/security/crypto_framework/CLAUDE.md) for detailed implementation

**base/time/time_service/** - Time service

- Time and timezone services
- See [CLAUDE.md](base/time/time_service/CLAUDE.md) for detailed implementation

**base/web/webview/** - WebView

- WebView component
- See [CLAUDE.md](base/web/webview/CLAUDE.md) for detailed implementation

**foundation/resourceschedule/qos_manager/** - QoS manager

- Quality of Service management
- See [CLAUDE.md](foundation/resourceschedule/qos_manager/CLAUDE.md) for detailed implementation

#### Third-Party Dependencies

**third_party/skia/** - Skia graphics library

- See [CLAUDE.md](third_party/skia/CLAUDE.md) for detailed implementation

**third_party/icu/** - ICU (Internationalization)

- See [CLAUDE.md](third_party/icu/CLAUDE.md) for detailed implementation

**third_party/openssl/** - OpenSSL (SSL/TLS)

- See [CLAUDE.md](third_party/openssl/CLAUDE.md) for detailed implementation

**third_party/curl/** - cURL (HTTP client)

- See [CLAUDE.md](third_party/curl/CLAUDE.md) for detailed implementation

**third_party/zlib/** - Zlib (Compression)

- See [CLAUDE.md](third_party/zlib/CLAUDE.md) for detailed implementation

**third_party/freetype/** - FreeType (Font rendering)

- See [CLAUDE.md](third_party/freetype/CLAUDE.md) for detailed implementation

**third_party/sqlite/** - SQLite (Database)

- See [CLAUDE.md](third_party/sqlite/CLAUDE.md) for detailed implementation

**third_party/libpng/** - libpng (PNG images)

- See [CLAUDE.md](third_party/libpng/CLAUDE.md) for detailed implementation

**third_party/ffmpeg/** - FFmpeg (Audio/video codecs)

- See [CLAUDE.md](third_party/ffmpeg/CLAUDE.md) for detailed implementation

**third_party/node/** - Node.js runtime

- See [CLAUDE.md](third_party/node/CLAUDE.md) for detailed implementation

**third_party/typescript/** - TypeScript compiler

- See [CLAUDE.md](third_party/typescript/CLAUDE.md) for detailed implementation

**third_party/jsoncpp/** - JSON parsing

- See [CLAUDE.md](third_party/jsoncpp/CLAUDE.md) for detailed implementation

**third_party/libxml2/** - XML parsing

- See [CLAUDE.md](third_party/libxml2/CLAUDE.md) for detailed implementation

**third_party/protobuf/** - Protocol buffers

- See [CLAUDE.md](third_party/protobuf/CLAUDE.md) for detailed implementation

**third_party/googletest/** - Testing framework

- See [CLAUDE.md](third_party/googletest/CLAUDE.md) for detailed implementation

**third_party/bounds_checking_function/** - Security bounds checking

- See [CLAUDE.md](third_party/bounds_checking_function/CLAUDE.md) for detailed implementation

**third_party/** - 30+ additional libraries (libuv, libwebsockets, pcre2, etc.)

- See [CLAUDE.md](third_party/CLAUDE.md) for detailed implementation

#### Documentation & Resources

**docs/** - Project documentation (English and Chinese)

- See [CLAUDE.md](docs/CLAUDE.md) for detailed implementation

**samples/** - Sample applications for testing and learning

- See [CLAUDE.md](samples/CLAUDE.md) for detailed implementation

**community/** - Community resources and license information

- See [CLAUDE.md](community/CLAUDE.md) for detailed implementation

**productdefine/common/** - Product configuration definitions

- See [CLAUDE.md](productdefine/common/CLAUDE.md) for detailed implementation

## Cross-Platform Development Guide

This section provides comprehensive guidance for cross-platform development in ArkUI-X, covering common operations and workflows for adapting features from OpenHarmony (OHOS) to Android/iOS platforms.

### Core Principles

ArkUI-X cross-platform architecture follows three fundamental principles:

1. **Maximize OHOS Reuse**: Reuse OpenHarmony's ArkTS frontend, graphics rendering engine, and runtime wherever possible
2. **Adapter-Based Architecture**: Use adapter layers for platform-specific implementations (window, input, font, keyboard, view, etc.)
3. **Plugin-Based Subsystem APIs**: Use plugin architecture for subsystem APIs (HTTP, FS, data management, BLE, etc.)

### Development Workflow Overview

```
┌─────────────────────────────────────────────────────────────────┐
│  1. Identify Feature Type                                       │
│     ├─ Core Framework? → Adapter Layer                          │
│     └─ Subsystem API? → Plugin Layer                            │
└─────────────────────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────────┐
│  2. Choose Adaptation Strategy                                  │
│     ├─ Reuse OHOS Module (if available in repo)                │
│     ├─ Implement Adapter (framework capabilities)              │
│     └─ Implement Plugin (subsystem APIs)                        │
└─────────────────────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────────┐
│  3. Implement Cross-Platform Code                               │
│     ├─ Common C++ code (shared)                                │
│     ├─ Android implementation (JNI + Java/Kotlin)              │
│     └─ iOS implementation (ObjC++ + Objective-C/Swift)          │
└─────────────────────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────────┐
│  4. Configure Build System                                     │
│     ├─ Update BUILD.gn files                                    │
│     ├─ Register in plugin_lib.gni (if plugin)                  │
│     └─ Add to arkui_cross_sdk_description_std.json             │
└─────────────────────────────────────────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────────────────┐
│  5. Add Tests & Documentation                                   │
│     ├─ XTS tests (test/xts/)                                   │
│     ├─ API documentation (docs/zh-cn/application-dev/reference/apis/)
│     └─ Sample code (samples/CodeLab/EnjoyArkUIX/)               │
└─────────────────────────────────────────────────────────────────┘
```

### Principle 1: Maximize OHOS Module Reuse

**Objective**: Reuse OpenHarmony's ArkTS frontend, graphics engine, and runtime to minimize code duplication and ensure consistency.

#### What Can Be Reused

The following OHOS modules are directly reused (referenced from OpenHarmony repo):

| Module | Path | Purpose |
|--------|------|---------|
| **arkui_ace_engine** | `foundation/arkui/ace_engine/` | Core UI framework (frontend, components, rendering) |
| **arkcompiler_ets_frontend** | `arkcompiler/ets_frontend/` | ArkTS/TypeScript frontend compiler |
| **arkcompiler_ets_runtime** | `arkcompiler/ets_runtime/` | ETS runtime execution engine |
| **arkcompiler_runtime_core** | `arkcompiler/runtime_core/` | Core runtime infrastructure |
| **arkcompiler_toolchain** | `arkcompiler/toolchain/` | Compiler toolchain |
| **arkui_napi** | `foundation/arkui/napi` | NAPI binding layer |
| **graphic_graphic_2d** | `foundation/graphic/graphic_2d/` | Rosen 2D graphics engine |
| **graphic_graphic_surface** | `foundation/graphic/graphic_surface/` | Surface management |
| **third_party_skia** | `third_party/skia/` | 2D graphics library |
| **third_party_icu** | `third_party/icu/` | Internationalization |
| ... and 100+ other modules | | |

#### How to Check Reusability

**1. Check .repo/manifests/default.xml**:
```bash
grep "remote=\"openharmony\"" .repo/manifests/default.xml
```

If the project has `remote="openharmony"`, it's being reused from OpenHarmony.

**2. Check Revision**:
```xml
<project name="arkui_ace_engine" path="foundation/arkui/ace_engine"
         remote="openharmony" revision="OpenHarmony-6.0-Release"
         upstream="OpenHarmony-6.0-Release" />
```

This means the code is directly pulled from OpenHarmony, not forked.

**3. Reuse Strategy**:
- ✅ **DO**: Use OHOS modules as-is when available
- ✅ **DO**: Report issues to OpenHarmony if bugs found
- ❌ **DON'T**: Fork OHOS modules unless absolutely necessary
- ❌ **DON'T**: Modify OHOS module code in ArkUI-X repo

### Principle 2: Framework Capabilities Adaptation (Adapter Layer)

**Objective**: Adapt framework-level capabilities (window, input method, font, keyboard, view, etc.) using the adapter architecture.

#### Adapter Directory Structure

Framework capabilities are adapted in `foundation/arkui/ace_engine/adapter/`:

```
foundation/arkui/ace_engine/adapter/
├── android/                          # Android platform adapter
│   ├── build/                         # Build configuration
│   ├── capability/                    # Platform capability implementations
│   │   ├── window/                    # Window management
│   │   ├── input/                     # Input handling
│   │   ├── font/                      # Font rendering
│   │   ├── keyboard/                   # Keyboard management
│   │   ├── drag/                      # Drag-and-drop
│   │   └── ...                        # Other capabilities
│   ├── entrance/                      # Platform entry points
│   │   ├── ace_ability_entry.cpp      # Ability lifecycle
│   │   ├── ace_application_entry.cpp  # Application initialization
│   │   └── ...
│   ├── osal/                          # OS Abstraction Layer
│   │   ├── memory/                    # Memory management
│   │   ├── thread/                    # Thread utilities
│   │   └── ...
│   └── stage/                         # Stage management
│       ├── stage_activity.cpp         # Android Activity
│       └── ...
│
└── ios/                              # iOS platform adapter
    ├── build/                         # Build configuration
    ├── capability/                    # Platform capability implementations
    │   ├── window/                    # Window management
    │   ├── input/                     # Input handling
    │   ├── font/                      # Font rendering
    │   ├── keyboard/                   # Keyboard management
    │   ├── drag/                      # Drag-and-drop
    │   └── ...                        # Other capabilities
    ├── entrance/                      # Platform entry points
    │   ├── ace_ability_entry.mm      # Ability lifecycle
    │   ├── ace_application_entry.mm  # Application initialization
    │   └── ...
    ├── osal/                          # OS Abstraction Layer
    │   ├── memory/                    # Memory management
    │   ├── thread/                    # Thread utilities
    │   └── ...
    └── stage/                         # Stage management
        └── stage_view_controller.mm  # iOS UIViewController
```

#### Adapter Subdirectory Purposes

| Subdirectory | Purpose | Examples |
|--------------|---------|----------|
| **build/** | GN build configuration, framework linking | BUILD.gn, cmake config |
| **capability/** | Platform-specific feature implementations | Window, input, font, keyboard |
| **entrance/** | Application lifecycle entry points | Ability creation, stage initialization |
| **osal/** | OS Abstraction Layer (memory, thread, file) | Platform-independent utilities |
| **stage/** | Stage/window management | Activity (Android), ViewController (iOS) |

#### Adding New Framework Capability

**When to Use**: For window management, input handling, font rendering, keyboard, view system, etc.

**Step-by-Step Workflow**:

1. **Create Capability Implementation**:
   ```bash
   # Android
   mkdir -p foundation/arkui/ace_engine/adapter/android/capability/new_feature
   touch foundation/arkui/ace_engine/adapter/android/capability/new_feature/new_feature_impl.cpp

   # iOS
   mkdir -p foundation/arkui/ace_engine/adapter/ios/capability/new_feature
   touch foundation/arkui/ace_engine/adapter/ios/capability/new_feature/new_feature_impl.mm
   ```

2. **Update BUILD.gn**:
   ```gni
   # foundation/arkui/ace_engine/adapter/android/BUILD.gn
   ohos_source_set("ace_android_adapter") {
     sources = [
       "capability/new_feature/new_feature_impl.cpp",
       # ... other sources
     ]
     # ... other configs
   }
   ```

3. **Use OHOS Reuse First**:
   - Check if OHOS has similar implementation in `foundation/arkui/ace_engine/`
   - Reuse core logic, adapt platform-specific parts only

4. **Add OSAL Abstraction** (if needed):
   ```cpp
   // foundation/arkui/ace_engine/adapter/osal/new_feature_osal.h
   #ifdef ANDROID_PLATFORM
       // Android implementation
   #elif defined(IOS_PLATFORM)
       // iOS implementation
   #endif
   ```

### Principle 3: Subsystem API Adaptation (Plugin Architecture)

**Objective**: Adapt subsystem APIs (HTTP, filesystem, data management, BLE, etc.) using plugin architecture.

#### Plugin Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│  Application Layer (@ohos.* API calls)                          │
└─────────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────────┐
│  NAPI Binding Layer (C++)                                        │
│  - Parse parameters                                              │
│  - Convert types                                                 │
│  - Call plugin interface                                         │
└─────────────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────────────┐
│  Plugin Interface (Pure Virtual C++)                            │
│  - IFeatureStrategy                                             │
│  - Platform-independent business logic                          │
└─────────────────────────────────────────────────────────────────┘
                          ↓
        ┌─────────────────┴─────────────────┐
        ↓                                   ↓
┌─────────────────────┐         ┌─────────────────────┐
│  Android Plugin     │         │  iOS Plugin          │
│  - JNI Bridge        │         │  - ObjC++ Bridge     │
│  - Java/Kotlin Impl  │         │  - ObjC/Swift Impl   │
└─────────────────────┘         └─────────────────────┘
```

#### Plugin Directory Structure

```
plugins/
├── plugin_lib.gni                    # Plugin registration (MUST UPDATE)
├── http/                             # Example: HTTP plugin
│   ├── BUILD.gn                      # Build template
│   ├── http.gni                      # Module configuration
│   ├── interface/                    # Platform-independent interfaces
│   │   └── http_interface.h          # Pure virtual interface
│   ├── napi/                         # NAPI binding layer
│   │   └── http_module.cpp           # NAPI implementation
│   ├── common/                       # Shared C++ code
│   │   └── http_core.cpp             # Business logic
│   ├── android/                      # Android platform
│   │   ├── BUILD.gn
│   │   ├── http_strategy_android.cpp  # Android strategy
│   │   └── java/
│   │       └── jni/
│   │           └── http_jni.cpp     # JNI bridge
│   └── ios/                          # iOS platform
│       ├── BUILD.gn
│       ├── http_strategy_ios.mm      # iOS strategy
│       └── http_impl.mm              # iOS implementation
```

#### Plugin Adaptation Checklist

**CRITICAL**: Complete all 4 build system configurations when adding new plugin:

**Step 1: Create Plugin Directory Structure**
```bash
# 1. Create plugin directory
mkdir -p plugins/new_feature

# 2. Create subdirectories
cd plugins/new_feature
mkdir -p interface      # Platform-independent interfaces
mkdir -p napi          # NAPI binding
mkdir -p common        # Shared business logic
mkdir -p android       # Android implementation
mkdir -p ios          # iOS implementation
```

**Step 2: Add to plugins/plugin_lib.gni** (MANDATORY)
```gni
# plugins/plugin_lib.gni (around line 120)
common_plugin_libs = [
  # ... existing plugins ...
  "pasteboard",
  "new_feature",  # ← Add this line in alphabetical order
  "vibrator",
]
```

**Why**: Without this, the plugin won't be compiled into the plugin library.

**Verification**:
```bash
grep "new_feature" plugins/plugin_lib.gni
```

**Step 3: Configure API Toolchain (interface/sdk/plugins/api/apiConfig.json)**

Add module configuration at end of JSON array:
```json
{
    "module": "ohos.new_feature",
    "library": {
        "android": [
            "lib/new_feature/ace_new_feature_android.jar",
            "lib/new_feature/arch_type/libnew_feature.so"
        ],
        "ios": [ "xcframework/build_modes/libnew_feature.xcframework" ]
    },
    "deps": {
        "android": [],
        "ios": []
    }
}
```

**Where**: Insert before closing `]` of the array.

**Verification**:
```bash
# Validate JSON
python3 -m json.tool interface/sdk/plugins/api/apiConfig.json > /dev/null && echo "Valid" || echo "Invalid"

# Check module is configured
grep "ohos.new_feature" interface/sdk/plugins/api/apiConfig.json
```

**Step 4: Update Cross-Platform SDK Description (build_plugins/sdk/arkui_cross_sdk_description_std.json)**

This is the **cross-platform compilation entry point** - without this, the module won't be included in the SDK.

**Android SO Configuration** (add to `"android"` array after vibrator):
```json
{
    "install_dir": "arkui-x/plugins/api/lib/new_feature/arch_type",
    "module_label": "//plugins/new_feature/android:new_feature_adapter_android",
    "target_os": ["linux", "windows", "darwin"]
}
```

**If has Java adapter, Android Java Configuration** (add to `"android"` array):
```json
{
    "install_dir": "arkui-x/plugins/api/lib/new_feature",
    "module_label": "//plugins/new_feature/frameworks/android/java:new_feature_plugin_java",
    "target_os": ["linux", "windows", "darwin"]
}
```

**iOS Configuration** (add to `"ios"` array after vibrator):
```json
{
    "install_dir": "arkui-x/plugins/api/framework/arch_type/libnew_feature.framework",
    "module_label": "//plugins/new_feature/ios:new_feature_adapter_ios",
    "target_os": ["darwin"]
}
```

**Verification**:
```bash
# Validate JSON
python3 -m json.tool build_plugins/sdk/arkui_cross_sdk_description_std.json > /dev/null && echo "Valid" || echo "Invalid"

# Check module is configured
grep "new_feature" build_plugins/sdk/arkui_cross_sdk_description_std.json
```

#### Implementation Guidelines

**Detailed Reference**: See [skill-platform-api.md](.claude/skills/skill-platform-api.md) for complete implementation patterns.

**Quick Reference**:

1. **Architecture**: Use 5-layer architecture (API → NAPI → Business Logic → Platform Interface → Platform Implementation)
2. **Platform Isolation**: Separate Android/iOS code via directory structure or `#ifdef`
3. **Minimal JNI**: Cache JNI references, minimize cross-language calls
4. **Unified Async**: Always use Promise/Deferred pattern
5. **GN Template**: Use template-based multi-platform build
6. **Error Handling**: Unified error codes across platforms
7. **Smart Pointers**: Use shared_ptr/unique_ptr for lifecycle
8. **Thread Safety**: Use mutexes and thread-safe containers

### Principle 4: Add XTS Tests (test/xts/)

**Objective**: Add XTS (X Test Suite) tests to protect subsystem API functionality after adaptation.

#### What is XTS?

XTS is OpenHarmony's test suite framework for cross-platform compatibility testing. ArkUI-X migrates and extends XTS tests for Android/iOS platforms.

#### XTS Directory Structure

```
test/xts/
├── android/                          # Android XTS tests
│   └── xts_2.0/
│       └── new_feature/             # Feature-specific tests
│           ├── BUILD.gn
│           ├── src/
│           │   ├── main/
│           │   │   ├── resources/
│           │   │   │   └── base/
│           │   │   │       ├── ohos/
│           │   │   │       │   └── global/
│           │   │   │       │       └── i18n/
│           │   │   │       │           ├── media/
│           │   │   │       │           │   └── test.json
│           │   │   │       │           └── string/
│           │   │   │       │               └── en_US/
│           │   │   │       │                   └── element/
│           │   │   │       │                       └── default.json
│           │   │   ├── ets/
│           │   │   │   ├── test/
│           │   │   │   │   ├── list/
│           │   │   │   │   │   ├── NewFeature.test.ets
│           │   │   │   │   │   └── ...
│           │   │   │   │   └── module/
│           │   │   │   │       └── NewFeatureModule.test.ets
│           │   │   │   └── module.json
│           │   │   └── ...
│           │   └── ...
│           └── ...
│
├── ios/                              # iOS XTS tests (similar structure)
│   └── xts_2.0/
│       └── new_feature/
│           └── ...
│
└── acts/                             # ACTS (Act) tests
    └── new_feature/
        └── ...
```

#### Adding XTS Tests

**Step 1: Create Test Directory**
```bash
# For Android
mkdir -p test/xts/android/xts_2.0/new_feature/src/main/ets/test/list
mkdir -p test/xts/android/xts_2.0/new_feature/src/main/resources
```

**Step 2: Create Test Module**
```typescript
// test/xts/android/xts_2.0/new_feature/src/main/ets/test/list/NewFeature.test.ets
import { describe, beforeAll, beforeEach, afterEach, afterAll, it, expect } from '@ohos/hypium'

export default function newFeatureTest() {
  describe('NewFeature', () => {
    it('should pass basic test', 0, () => {
      // Test implementation
      expect(true).assertEqual(true)
    })
  })
}
```

**Step 3: Create module.json**
```json
{
  "module": {
    "name": "new_feature",
    "type": "entry",
    "srcEntrance": "./test/list/NewFeature.test.ets",
    "description": "$string:module_desc"
  },
  "deviceType": [
    "default",
    "tablet"
  ]
}
```

**Step 4: Create BUILD.gn**
```gni
import("../../../../../build/xts_aaf_actd.gni")

xts_ets_test_unittest_case("new_feature") {
  module_name = "new_feature"
  module_source_dir = "src/main/ets"

  test_suites = [
    "test/list/NewFeature.test.ets"
  ]
}
```

**Step 5: Run Tests**
```bash
cd test/xts/android/xts_2.0
./run.sh new_feature
```

#### XTS Test Best Practices

1. **Test Coverage**: Cover main functionality, edge cases, error handling
2. **Async Tests**: Use async/await for Promise-based APIs
3. **Platform-Specific**: Add platform-specific tests if needed
4. **Naming**: Follow `NewFeature.test.ets` naming convention
5. **Documentation**: Add comments explaining test purpose

### Principle 5: Add API Documentation (docs/zh-cn/application-dev/reference/apis/)

**Objective**: Add API documentation in Chinese for new subsystem APIs.

#### Documentation Directory Structure

```
docs/zh-cn/application-dev/reference/apis/
├── arkui-ts/                         # ArkTS APIs
│   └── @ohos.new_feature.d.ts      # API type definition
│       └── README.md                # API documentation
│           ├── overview.md          # API overview
│           ├── apis/                # API methods
│           │   └── new_feature_api.md
│           └── ...
├── js-apis-archive/                 # Legacy JS APIs (if applicable)
└── ...
```

#### Adding API Documentation

**Step 1: Create Type Definition**
```typescript
// docs/zh-cn/application-dev/reference/apis/arkui-ts/@ohos.new_feature.d.ts
/**
 * @fileoverview NewFeature API Type Definitions
 * @syscap SystemCapability.ArkUI.CrossPlatform
 * @since 10
 */

/**
 * Initializes new feature.
 *
 * @permission N/A
 * @param {NewFeatureOptions} options - Options for initialization
 * @returns {Promise<void>} Promise that resolves when initialization completes
 * @throws {BusinessError} 401 if parameter is invalid
 * @syscap SystemCapability.ArkUI.CrossPlatform
 * @since 10
 */
declare function initNewFeature(options: NewFeatureOptions): Promise<void>;

/**
 * NewFeature options.
 *
 * @syscap SystemCapability.ArkUI.CrossPlatform
 * @since 10
 */
interface NewFeatureOptions {
  /**
   * Enable debug mode.
   *
   * @syscap SystemCapability.ArkUI.CrossPlatform
   * @since 10
   */
  debug?: boolean;

  /**
   * Timeout in milliseconds.
   *
   * @syscap SystemCapability.ArkUI.CrossPlatform
   * @since 10
   */
  timeout?: number;
}
```

**Step 2: Create README**
```markdown
# NewFeature API

## Overview

The NewFeature module provides cross-platform support for ...

## Module Summary

| Import | API |
|--------|-----|
| import { newFeature } from '@ohos.new_feature' | newFeature |

## Usage

### Initialization

```typescript
import newFeature from '@ohos.new_feature'

async function example() {
  try {
    await newFeature.init({ debug: true })
    console.log('NewFeature initialized')
  } catch (error) {
    console.error('Failed to initialize:', error)
  }
}
```

## APIs

### initNewFeature

initNewFeature(options: NewFeatureOptions): Promise<void>

Initializes the new feature module.

**Parameters**

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| options | [NewFeatureOptions](#newfeatureoptions) | Yes | Options for initialization |

**Return Value**

Promise\<void\>

**Error Codes**

| ID | Error Message |
|----|--------------|
| 401 | Parameter error. |
| 200 | Permission error. |
```

**Step 3: Create API Details**
```markdown
## newFeature

### Modules to Import

```typescript
import newFeature from '@ohos.new_feature'
```

## newFeature.init

init(options: NewFeatureOptions): Promise\<void\>

Initializes the new feature module with specified options.

**Parameters**

| Name | Type | Mandatory | Description |
|------|------|-----------|-------------|
| options | [NewFeatureOptions](#newfeatureoptions) | Yes | Options for initialization |

**Return Value**

Promise\<void\>

**Errors**

| ID | Error Message |
|----|--------------|
| 401 | Parameter error. |
| 200 | No permission. |

**Example**

```typescript
import newFeature from '@ohos.new_feature'

async function example() {
  try {
    await newFeature.init({ debug: true, timeout: 5000 })
    console.log('Success')
  } catch (err) {
    console.error('Error:', err.code, err.message)
  }
}
```
```

#### Documentation Best Practices

1. **Type Safety**: Always provide TypeScript type definitions
2. **Examples**: Include complete, runnable code examples
3. **Error Codes**: Document all possible error codes
4. **Permissions**: Specify required permissions
5. **System Capabilities**: Indicate required system capabilities
6. **Platform Notes**: Document platform-specific behavior
7. **Chinese Language**: Primary documentation in Chinese, English as reference

### Principle 6: Add Sample Test Cases (samples/CodeLab/EnjoyArkUIX/)

**Objective**: Add test cases in sample application to protect and demonstrate functionality.

#### Sample Application Structure

```
samples/
└── CodeLab/
    └── EnjoyArkUIX/               # Main sample application
        ├── entry/
        │   └── src/
        │       └── main/
        │           ├── pages/
        │           │   ├── Index.ets           # Main page
        │           │   ├── NewFeaturePage.ets  # New feature demo
        │           │   └── ...
        │           └── resources/
        │               └── base/
        │                   ├── element/
        │                   ├── media/
        │                   └── profile/
        └── ...
```

#### Adding Test Cases

**Step 1: Create Feature Page**
```typescript
// samples/CodeLab/EnjoyArkUIX/entry/src/main/pages/NewFeaturePage.ets
import newFeature from '@ohos.new_feature'
import prompt from '@ohos.prompt'

@Entry
@Component
struct NewFeaturePage {
  @State message: string = 'Click to test NewFeature'
  @State result: string = ''

  async testNewFeature() {
    try {
      // Test initialization
      await newFeature.init({ debug: true })

      // Test main functionality
      const result = await newFeature.execute({ param: 'test' })

      this.result = `Success: ${JSON.stringify(result)}`
      this.message = 'NewFeature test passed!'
    } catch (error) {
      this.result = `Error: ${error.code} - ${error.message}`
      this.message = 'NewFeature test failed!'
    }
  }

  build() {
    Column() {
      Text(this.message)
        .fontSize(20)
        .margin(20)

      Button('Test NewFeature')
        .onClick(() => this.testNewFeature())
        .margin(20)

      Text(this.result)
        .fontSize(14)
        .margin(20)
    }
    .width('100%')
    .height('100%')
  }
}
```

**Step 2: Add Navigation Entry**
```typescript
// In main index page or menu
router.pushUrl({ url: 'pages/NewFeaturePage' })
```

**Step 3: Test on Platforms**
```bash
# Android
./build.sh --product-name arkui-x --target-os android
# Install APK and run NewFeaturePage

# iOS
./build.sh --product-name arkui-x --target-os ios
# Install app and run NewFeaturePage
```

#### Sample Code Best Practices

1. **Complete Examples**: Provide full, runnable examples
2. **Error Handling**: Demonstrate proper error handling
3. **User Feedback**: Show loading states and results
4. **Multiple Scenarios**: Cover typical use cases
5. **Comments**: Explain key implementation details
6. **Platform Testing**: Test on both Android and iOS

### Complete Development Workflow Example

**Scenario**: Adding new subsystem API (e.g., `@ohos.wifi`)

```
1. ✅ Identify Type: Subsystem API → Plugin Architecture

2. ✅ Create Plugin Structure:
   mkdir -p plugins/wifi/{interface,napi,common,android,ios}

3. ✅ Update Build System:
   - Add "wifi" to plugins/plugin_lib.gni
   - Configure apiConfig.json
   - Update arkui_cross_sdk_description_std.json

4. ✅ Implement Plugin:
   - Define IWifiStrategy interface
   - Implement AndroidWifiStrategy (JNI + Java)
   - Implement IOSWifiStrategy (ObjC++ + Objective-C)
   - Add NAPI bindings

5. ✅ Add XTS Tests:
   - Create test/xts/android/xts_2.0/wifi/
   - Add Wifi.test.ets test cases
   - Run tests to verify

6. ✅ Add Documentation:
   - Create @ohos.wifi.d.ts in docs/
   - Write API reference in Chinese
   - Include code examples

7. ✅ Add Sample Code:
   - Create WifiPage.ets in samples/
   - Test on Android and iOS
   - Verify functionality

8. ✅ Build & Verify:
   - Build Android APK
   - Build iOS app
   - Run all tests
   - Deploy samples
```

### Quick Reference Checklist

For each cross-platform adaptation task:

**Framework Capabilities (Adapter Layer)**:
- [ ] Identify capability type (window, input, font, etc.)
- [ ] Create implementation in adapter/android/capability/
- [ ] Create implementation in adapter/ios/capability/
- [ ] Update BUILD.gn files
- [ ] Test on Android and iOS
- [ ] Add XTS tests
- [ ] Add documentation
- [ ] Add sample code

**Subsystem APIs (Plugin Layer)**:
- [ ] Create plugin directory structure
- [ ] Add to plugins/plugin_lib.gni
- [ ] Update interface/sdk/plugins/api/apiConfig.json
- [ ] Update build_plugins/sdk/arkui_cross_sdk_description_std.json
- [ ] Implement NAPI bindings
- [ ] Implement Android plugin (JNI + Java)
- [ ] Implement iOS plugin (ObjC++ + ObjC/Swift)
- [ ] Add XTS tests (test/xts/)
- [ ] Add API documentation (docs/zh-cn/)
- [ ] Add sample code (samples/CodeLab/EnjoyArkUIX/)
- [ ] Build and test on both platforms

### Troubleshooting

**Common Issues**:

1. **Plugin not compiled**: Check `plugins/plugin_lib.gni` registration
2. **SDK missing module**: Check `arkui_cross_sdk_description_std.json`
3. **API not found**: Check `apiConfig.json` configuration
4. **XTS tests fail**: Verify test module.json and imports
5. **Sample app crashes**: Check API signatures and error handling

**Verification Commands**:
```bash
# Check plugin registration
grep "plugin_name" plugins/plugin_lib.gni

# Check SDK description
grep "plugin_name" build_plugins/sdk/arkui_cross_sdk_description_std.json

# Check API config
grep "ohos.plugin_name" interface/sdk/plugins/api/apiConfig.json

# Validate JSON files
python3 -m json.tool build_plugins/sdk/arkui_cross_sdk_description_std.json
python3 -m json.tool interface/sdk/plugins/api/apiConfig.json

# Run XTS tests
cd test/xts/android/xts_2.0 && ./run.sh plugin_name
```

### Related Documentation

- **Cross-Platform API Adaptation Rules**: See detailed rules in [skill-platform-api.md](.claude/skills/skill-platform-api.md)
- **Build System**: See Build System chapter for GN configuration
- **Testing**: See Testing chapter for XTS framework details
- **Architecture**: See Architecture chapter for system design

## Sample Applications

Reference applications are available in `samples/`:

- `samples/BasicFeature/HelloWorld` - Minimal ArkTS application
- `samples/BasicFeature/Library` - Library usage example
- `samples/BasicFeature/MultiAbility` - Multi-ability application
- `samples/BasicFeature/Native` - C++ native integration example

Use these samples to test framework changes and learn best practices.

## Testing

### XTS Test Suite

The project uses the XTS (X Test Suite) for testing cross-platform functionality:

```bash
# Android XTS tests
cd test/xts/android/xts_2.0
./run.sh {Module name}    # Run specific test module

# The XTS tests are built as JSBundles and placed in platform-specific directories
# For Android: assets/ directory
# Tests execute automatically on app startup via aboutToAppear callback
```

**Test Location:** `test/xts/` - Contains platform-specific test suites migrated from OpenHarmony

### Test Framework

The project uses a Python-based test framework located in `test/testfwk/`:

```bash
# Run tests using xdevice test framework
python3 test/testfwk/arkxtest/xdevice/src/xdevice/__main__.py [options]

# Common test options:
#   -product <product_name>     # Product to test
#   -testtype <type>            # Test type (unit, integration, etc.)
#   -report <report_dir>        # Report output directory
```

**Test Types:**

- Unit tests - Component-level testing
- Integration tests - Cross-component testing
- Platform compatibility tests - Verify Android/iOS/HarmonyOS parity
- Performance tests - Framework performance benchmarks

### Running Specific Tests

To run tests for specific components:

```bash
# Build test target for specific component
./build.sh --product-name arkui-x --target-os android --build-target {test_target}

# Example: Build and run ACE engine tests
./build.sh --product-name arkui-x --target-os android --build-target ace_engine/test:unittest
```

**Note:** Test execution results are currently visible only in logs. Visual reporting is planned for future releases.

## Documentation

Comprehensive documentation is available in the `docs/` directory:

- `docs/en/` - English documentation
- `docs/zh-cn/` - Chinese documentation

Key sections:

- Application development guides
- Framework development tutorials
- API references
- Platform-specific notes

## Important File Locations

- `BUILD.gn` files - Build definitions (located throughout source tree)
- `.gn` - GN configuration (symlink to `build/core/gn/dotfile.gn`)
- `bundle.json` - Component/package metadata
- `build_plugins/build_scripts/build.sh` - Main build script
- `build/hb/main.py` - HB build tool entry point

## License

Apache License 2.0 - See LICENSE file in community/ directory

## Compilation Macros and Build Flags

This section documents key compilation macros and build flags extracted from `compile_commands.json` (7202 compilation entries analyzed).

### Platform Detection Macros

```cpp
IS_ARKUI_X_TARGET        // Cross-platform build target (vs native HarmonyOS)
ANDROID                  // Building for Android platform
ANDROID_PLATFORM         // Android-specific adaptations
CROSS_PLATFORM           // Cross-platform compatibility layer
ROSEN_ANDROID           // Rosen graphics backend on Android
ROSEN_CROSS_PLATFORM    // Cross-platform Rosen implementation
```

### Feature Toggle Macros

Control major features at compile time:


| Macro                     | Purpose                          | Usage Frequency |
| ------------------------- | -------------------------------- | --------------- |
| `ENABLE_ROSEN_BACKEND`    | Rosen graphics rendering backend | 1360+ files     |
| `ENABLE_DRAG_FRAMEWORK`   | Drag-and-drop support            | All platforms   |
| `PLATFORM_VIEW_SUPPORTED` | Platform view component          | 1360+ files     |
| `PIXEL_MAP_SUPPORTED`     | Image pixel map operations       | All platforms   |
| `VIDEO_SUPPORTED`         | Video playback support           | All platforms   |
| `QRCODEGEN_SUPPORT`       | QR code generation               | 1095+ files     |
| `USE_NEW_SKIA`            | Use newer Skia version (m133)    | Most files      |
| `ACE_ENABLE_GL`           | OpenGL rendering in ACE          | 1217 files      |
| `ARKUI_CIRCLE_FEATURE`    | Circular screen support          | 1095+ files     |

### Build Configuration Macros


| Macro                    | Purpose                           | Impact                             |
| ------------------------ | --------------------------------- | ---------------------------------- |
| `NDEBUG`                 | Release build (disables assert()) | **Defined in all analyzed builds** |
| `ACE_INSTANCE_LOG`       | Enable instance ID logging        | Adds 2-5% code size                |
| `ACE_LOG_TAG="Ace"`      | Log tag prefix                    | Logging system identifier          |
| `ROSEN_DISABLE_DEBUGLOG` | Disable Rosen debug logs          | Reduces logging overhead           |
| `RUNTIME_MODE_RELEASE`   | Runtime release mode              | Disables debug checks              |
| `USE_ROSEN_DRAWING`      | Use Rosen drawing APIs            | Graphics backend selection         |

**Key Finding:** `NDEBUG` is defined across all analyzed builds (2000/2000 samples), indicating release configuration is the default.

### Graphics & Rendering Macros

```cpp
USE_ROSEN_DRAWING         // Use Rosen 2D drawing APIs
ROSEN_ARKUI_X            // Rosen for ArkUI-X
USE_M133_SKIA            // Skia version m133
SK_GL                    // OpenGL support in Skia
RS_ENABLE_GL             // OpenGL in RenderService
RS_ENABLE_EGLIMAGE       // EGL image support
RS_ENABLE_PARALLEL_RENDER // Parallel rendering
```

### Compiler Optimization Flags

Current optimization flags from compile_commands.json analysis (5000+ files sampled):

```bash
# Size optimization flags
-Os                       # Optimize for size (2190 files)
-O2                       # General optimization (2000 files)
-ffunction-sections       # Separate functions (5131 files)
-fdata-sections           # Separate data (3131 files)
-fvisibility-inlines-hidden # Hide inline symbols (3280 files)
-fvisibility=hidden       # Hide symbols by default (all)
-fmerge-all-constants     # Merge duplicate constants (2000 files)
-fno-ident                # Remove compiler version (2000 files)

# Security and stability
-fstack-protector-strong  # Stack protection (2012 files)
-funwind-tables           # Exception unwinding (2000 files)

# Platform-specific
-mno-outline-atomics      # Disable outline atomics (2000 files, ARM)
```

**Important:** `-Wl,--gc-sections` (linker dead code elimination) works with `-ffunction-sections`/`-fdata-sections`.

### Critical Include Patterns

When adding new includes, follow these patterns:

```cpp
// For ACE framework APIs
#include "core/pipeline_ng/ui_task_scheduler.h"           // NG pipeline
#include "interfaces/inner_api/ace/ace_view.h"             // Public API

// For platform adapters
#ifdef ANDROID
#include "adapter/android/entrance/java/jni/ace_view_android.h"
#elif defined(IOS)
#include "adapter/ios/entrance/ace_view_ios.h"
#endif

// For graphics
#include "roseEngine/modules/rosen/modules/render_service_client/core/..." // Rosen APIs
```

## Symbol Visibility and Export Strategy

The framework uses **default-hidden** symbol visibility:

```cpp
// BUILD.gn configuration
cflags = [ "-fvisibility=hidden" ]
cflags_cc = [ "-fvisibility-inlines-hidden" ]

// Export macros (65 files use visibility attributes)
#define ACE_EXPORT __attribute__((visibility("default")))

// Public API
ACE_EXPORT void PublicFunction();
```

**Rule:** Only symbols explicitly marked `visibility("default")` are exported.

Files with visibility exports (total 72 occurrences across 65 files):

- `interfaces/inner_api/ace_kit/include/*.h` - Public ACE kit
- `interfaces/napi/kits/*` - NAPI bindings
- `frameworks/bridge/*/interfaces/*` - Bridge interfaces

## Conditional Compilation Patterns

### Platform Guards

```cpp
// Cross-platform file structure
#ifdef IS_ARKUI_X_TARGET
    #ifdef ANDROID
        // Android-specific code
        #include <android/log.h>
    #elif defined(IOS)
        // iOS-specific code
        #include <CoreFoundation/CoreFoundation.h>
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

### Debug/Release Guards

```cpp
#ifdef NDEBUG
    // Release build - optimized
    #define DCHECK(x) (void)0
#else
    // Debug build - with checks
    #define DCHECK(x) assert(x)
#endif

#ifdef ACE_INSTANCE_LOG
    HILOG_INFO("Instance %{public}d", instance_id_);
#endif
```

## SDK Size Optimization Guidelines

### Current Optimization State

Based on compile_commands.json analysis:

✅ **Already Enabled:**

- `-Os` flag (size optimization) - 2190 files
- `-ffunction-sections` + `-fdata-sections` - enabled
- `-fvisibility=hidden` - symbol hiding
- `-Wl,--gc-sections` - linker dead code elimination
- `-Wl,--icf=safe` or `-Wl,--icf=all` - identical code folding
- `NDEBUG` defined - disables assert()

⚠️ **Potential Improvements:**

- `ACE_INSTANCE_LOG` enabled (adds 2-5% size)
- Debug symbols may not be fully stripped
- Some inline functions not marked hidden

### Size Optimization Impact


| Optimization              | Estimated Size Reduction | Risk                                     |
| ------------------------- | ------------------------ | ---------------------------------------- |
| Disable`ACE_INSTANCE_LOG` | 2-5%                     | Low (logging only)                       |
| Strip debug symbols       | 10-20%                   | Medium (keep symbols for crash analysis) |
| Ensure`NDEBUG` everywhere | 2-5%                     | Low (already mostly done)                |
| ICF optimization          | 3-8%                     | Low (may already be enabled)             |
| Remove unused exports     | 5-10%                    | Low (audit visibility attributes)        |

**Combined Potential: 22-48% size reduction**

### Optimization Checklist

When working on SDK size:

- [ ]  Verify `NDEBUG` is defined (check compile_commands.json)
- [ ]  Disable `ACE_INSTANCE_LOG` in release builds
- [ ]  Use `-fvisibility=hidden` (already configured)
- [ ]  Mark internal functions `static` where possible
- [ ]  Use `const`/`constexpr` to enable optimizations
- [ ]  Audit `__attribute__((visibility("default")))` usage
- [ ]  Check for duplicate code that ICF can merge
- [ ]  Profile with `nm -C --size-sort lib.so` to find large symbols

## Cross-Platform API Adaptation Rules

**Complete Guide**: See [skill-platform-api.md](.claude/skills/skill-platform-api.md) for detailed implementation rules and patterns.

### Quick Reference

**13 Core Rules**:

1. **Layered Architecture** - 5-layer architecture (ArkTS API → NAPI → Business Logic → Platform Abstraction → Platform Implementation)
2. **Platform Isolation** - Directory separation or conditional compilation
3. **Minimal JNI Calls** - Cache references, minimize overhead
4. **Unified Async Model** - Promise/Deferred pattern
5. **Template-Based GN Build** - Multi-platform build templates
6. **Configuration Separation** - Separate .gni, BUILD.gn files
7. **Clear Dependencies** - Explicit dependency declaration
8. **Interface Abstraction** - Pure virtual interfaces
9. **Minimal Conditional Compilation** - Prefer separate files over large #ifdef blocks
10. **Unified Error Handling** - Platform-independent error codes
11. **Zero-Copy Data Transfer** - Use std::string_view and move semantics
12. **Smart Pointer Lifecycle** - shared_ptr/unique_ptr for memory management
13. **Thread Safety** - Thread-safe containers and mutexes

### Key Checklist

When adapting new @ohos APIs:

- [ ]  5-layer architecture implemented
- [ ]  NAPI layer: parameter parsing only
- [ ]  Business logic: platform-independent C++
- [ ]  Platform interface: pure virtual
- [ ]  Platform isolation: directory separation
- [ ]  JNI minimized: cached references
- [ ]  Async model: Promise/Deferred pattern
- [ ]  GN template-based build
- [ ]  Config separation: .gni files
- [ ]  Dependencies: explicitly declared
- [ ]  Error handling: unified codes
- [ ]  Smart pointers: proper lifecycle
- [ ]  Thread safety: mutexes/containers
- [ ]  Tested on Android and iOS

### Reference Implementation

**Best reference**: `plugins/net/http`


| Aspect                 | File                                       |
| ---------------------- | ------------------------------------------ |
| API Definition         | `interface/sdk-js/api/@ohos.net.http.d.ts` |
| NAPI Binding           | `plugins/net/http/http_module.cpp`         |
| Platform Interface     | `plugins/net/http/http_exec_interface.h`   |
| Android Implementation | `plugins/net/http/android/http_exec.cpp`   |
| iOS Implementation     | `plugins/net/http/ios/http_exec.cpp`       |

### Applicable Modules

- **Network**: `@ohos.net.http`, `@ohos.net.socket`, `@ohos.net.webSocket`
- **Storage**: `@ohos.data.preferences`, `@ohos.data.relationalStore`
- **System**: `@ohos.file.fs`, `@ohos.sensor`, `@ohos.device.info`
- **Multimedia**: `@ohos.multimedia.image`, `@ohos.multimedia.player`

**Note**: For detailed rules, implementation patterns, and best practices, refer to the complete guide in [skill-platform-api.md](.claude/skills/skill-platform-api.md).
