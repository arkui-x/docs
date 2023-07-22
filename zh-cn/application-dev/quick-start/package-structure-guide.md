# ArkUI-X应用工程结构说明

# 简介

本文档配套ArkUI-X项目，将OpenHarmony ArkUI开发框架扩展到不同的OS平台，比如Android和iOS平台， 让开发者基于ArkUI，可复用大部分的应用代码（UI以及主要应用逻辑）并可以部署到相应的OS平台，降低跨平台应用开发成本。

# 应用工程目录结构介绍

跨平台应用工程目录结构，包含一套为ArkUI开发者提供的应用工程模板，提供构建OpenHarmony应用，HarmonyOS应用，Android应用，iOS应用的能力。

```
ArkUI-X应用工程目录结构
  ├── .arkui-x
  │   ├── android                 // Android平台相关代码
  │   └── ios                     // iOS平台相关代码
  ├── .hvigor
  ├── .idea
  ├── AppScope
  ├── entry
  ├── hvigor
  ├── oh_modules
  ├── build-profile.json5
  ├── hvigorfile.ts
  ├── local.properties
  └── oh-package.json5
```

此应用目录结构设计思想是从OpenHarmony应用工程原生支持跨平台角度出发，在OpenHarmony应用工程之上叠加Android和iOS应用工程，ArkUI代码和resources资源编辑在OpenHarmony侧完成，Native代码仍在各自平台应用工程中完成。

# 编译构建说明

* JSBundle编译和拷贝方案

ArkUI源码通过OpenHarmony应用工程的Bundle Task编译，生成的JSBundle分别拷贝到Android和iOS应用工程中，作为应用资源的一部分。由于ArkUI源码编译依赖OpenHarmony应用工程，那么Android和iOS的应用构建会依赖OpenHarmony应用工程编译生成的中间件。

* 应用resources资源编译和拷贝方案

ArkUI的resources资源编译，同JSBundle编译一样，也依赖OpenHarmony应用工程。编译后的resources资源，在Android和iOS平台上由原生平台的资源管理进行管理。例如：

Android应用通过assets和res进行资源管理，ArkUI编译后的resources资源直接作为assets资源的一部。

iOS应用通过Bundle Resources进行资源管理，ArkUI编译后的resources资源直接作为Resources资源项进行管理。

* 系统资源

OpenHarmony的系统资源采用分层管理，并预置到OpenHarmony系统中，应用运行时直接读取系统资源。ArkUI-X为了确保各平台渲染一致性，需要把OpenHarmony的系统资源打包Android和iOS应用中。ArkUI-X跨平台设计时，使系统资源存于ArkUI-X跨平台SDK中，在Android和iOS应用构建时打包到应用中。

## Android应用工程结构

```
ArkUI-X Android应用工程
├── app
│   ├── libs
│   │   ├── arkui_android_adapter.jar                   // ArkUI-X跨平台适配层，在SDK中发布
│   │   └── arm64-v8a
│   │       └── libarkui_android.so                     // ArkUI-X跨平台引擎库，在SDK中发布
│   ├── src
│   │   ├── androidTest
│   │   ├── main
│   │   │   ├── assets
│   │   │   │   └── arkui-x                             // ArkUI应用编译后的字节码文件和Resources，作为资源文件存放在assets/arkui-x中
│   │   │   │       ├── entry                           // ArkUI单个模块的编译结果
│   │   │   │       │   ├── ets                         // ArkUI单个模块代码的编译结果：包括字节码文件以及sourceMap文件
│   │   │   │       │   │   ├── sourceMaps.map
│   │   │   │       │   │   └── modules.abc
│   │   │   │       │   ├── resources.index             // ArkUI单个模块资源的编译结果：resources资源的编译结果
│   │   │   │       │   ├── resources                   // resources资源中的rawfile资源，不会进行编译。
│   │   │   │       │   └── module.json
│   │   │   │       ├── entry_test                      // ohosTest，仅仅Debug模式构建包含。
│   │   │   │       └── systemres                       // ArkUI框架自带的系统资源
│   │   │   ├── java/com/example/mayapp
│   │   │   │   ├── MyApplication.java                  // 基于StageApplication扩展MyApplication
│   │   │   │   └── EntryEntryAbilityActivity.java      // 基于StageActivity扩展EntryEntryAbilityActivity
│   │   │   ├── res
│   │   │   └── AndroidManifest.xml
│   │   └── test
│   ├── build.gradle
│   └── proguard-rules.pro
├── gradle/wrapper
├── build.gradle
├── gradle.properties
├── gradlew
├── gradlew.bat
└── settings.gradle
```

## iOS应用工程结构

```
ArkUI-X iOS应用工程
├── app.xcodeproj
│   ├── project.xcworkspace
│   └── project.pbxproj
├── app
│   ├── Assets.xcassets
│   ├── Base.Iproj
│   ├── AppDelegate.h
│   ├── AppDelegate.m                               // 应用入口, 驱动StageApplication的生命周期
│   ├── EntryEntryAbilityViewController.m           // 基于StageViewController扩展EntryEntryViewController
│   ├── Info.plist
│   └── main.m
├── arkui-x                                         // ArkUI应用编译后的字节码文件和Resources，作为资源文件存放在assets/arkui-x中
│   ├── entry                                       // ArkUI单个模块的编译结果
│   │   ├── ets                                     // ArkUI单个模块代码的编译结果：包括字节码文件以及sourceMap文件
│   │   │   ├── sourceMaps.map
│   │   │   └── modules.abc
│   │   ├── resources.index                         // ArkUI单个模块资源的编译结果：resources资源的编译结果
│   │   ├── resources                               // resources资源中的rawfile资源，不会进行编译。
│   │   └── module.json
│   ├── entry_test                                  // ohosTest，仅仅Debug模式构建包含。
│   └── systemres                                   // ArkUI框架自带的系统资源
└── frameworks                                      // ArkUI跨平台Framework动态库：包含ArkUI-X的框架以及插件
```