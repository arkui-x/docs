# ArkUI-X SDK目录结构介绍

# 简介

本文档配套ArkUI-X项目，将OpenHarmony ArkUI开发框架扩展到不同的OS平台，比如Android和iOS平台，让开发者基于ArkUI，可复用大部分的应用代码（UI以及主要应用逻辑）并可以部署到相应的OS平台，降低跨平台应用开发成本。

# SDK获取

* SDK获取可参见[版本说明](../../release-notes/README.md)。

# 开发工具集成

* ACE Tools命令行集成
  ACE Tools作为ArkUI跨平台应用构建的命令行工具，通过集成ArkUI-X SDK具有创建、编译构建OpenHarmony/HarmonyOS、Android和iOS应用的能力。
* DevEco Studio集成
  DecEco Studio作为ArkUI跨平台应用构建的IDE工具，通过集成ArkUI-X SDK支持一键构建出OpenHarmony/HarmonyOS应用、Android应用、iOS应用的能力。

# ArkUI-X SDK构建规则

## SDK压缩包命名规则

ArkUI-X项目编译构建流水线出包时，需按照SDK命名规则进行打包，命名规则如下：

```
path_操作系统类型_CPU架构类型_版本号_releaseType.zip
```

表1 SDK规则字段说明

|  字段 | 描述 |
| --- | --- |
| path | 取值为SDK根目录元数据arkui-x.json文件中的path标签内容。 |
| 操作系统类型 | 可选值：windows，darwin，linux。 |
| CPU架构类型 | 可选值："x64"-x86架构，"arm64"-arm架构。 |
| 版本号 | 构建版本号与OpenHarmony SDK版本号规则保持一致。 |
| releaseType | 可选值：Alpha，Beta，Release三种可选取值。releaseType后面加数字，标识迭代次数，比如：Beta1。|

**示例：**
arkui-x_windows_x64_1.0.0.0_Alpha.zip

## SDK压缩包内部结构
这里，以macOS平台上的ArkUI-X SDK包为例，对SDK目录结构和内容规格进行说明。更详细的ArkUI-X SDK内容规格会在第五节进行介绍。

```
arkui-x_darwin_x64_1.0.0.0_Alpha.zip
└── arkui-x
    ├── engine                   // ArkUI-X的引擎库
    │   ├── lib                  // ArkUI-X的引擎库：包括Android平台及架构的动态库
    │   ├── framework            // ArkUI-X的引擎库：包括iOS平台及架构的Framework库
    │   ├── xcframework          // ArkUI-X的引擎库：包括iOS平台及架构的XCFramework库
    │   ├── ets                  // ArkUI-X增量接口，比如：@arkui-x.bridge
    │   ├── apiConfig.json       // engine库配置文件，用于IDE和ACE Tools解析，以支持应用构建按需打包。
    │   └── systemres            // ArkUI-X框架自带的资源
    ├── plugins                  // ArkUI-X官方提供的插件库
    │   ├── component            // ArkUI组件插件库
    │   └── api                  // @ohos接口插件库，apiConfig.json
    ├── toolchains               // ArkUI-X应用开发工具，比如：ACE Tools。
    ├── sdkConfig.json           // 增量d.ts路径和接口前缀配置
    ├── arkui-x.json             // SDK管理配置，流水线自动生成
    └── NOTICE.txt
```

### ArkUI-X SDK引擎目录结构

ArkUI-X应用构建最小依赖集合，位于arkui-x_darwin_x64_1.0.0.0_Alpha.zip/arkui-x/engine目录：

```
engine
├── lib
│   ├── include
│   ├── arkui
│   │   ├── arkui_android_adapter.jar
│   │   ├── android-arm
│   │   ├── android-arm-profile
│   │   ├── android-arm-release
│   │   ├── android-arm64
│   │   ├── android-arm64-profile
│   │   ├── android-arm64-release
│   │   └── android-x86_64
│   ├── third_party                        // 内部目录同arkui
│   └── utils                              // 内部目录同arkui
├── framework
│   ├── arkui
│   │   ├── ios-arm64
│   │   ├── ios-arm64-profile
│   │   ├── ios-arm64-release
│   │   ├── ios-arm64-simulator
│   │   └── ios-x86_64-simulator
│   ├── third_party
│   └── utils
├── xcframework
│   ├── arkui
│   │   ├── ios
│   │   │   └── libarkui_ios.xcframework
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       └── ios-arm64_x86_64-simulator
│   │   ├── ios-profile
│   │   │   └── libarkui_ios.xcframework
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       └── ios-arm64_x86_64-simulator
│   │   └── ios-release
│   │       └── libarkui_ios.xcframework
│   │           ├── Info.plist
│   │           ├── ios-arm64
│   │           └── ios-arm64_x86_64-simulator
│   ├── third_party                          // 内部目录同arkui
│   └── utils                                // 内部目录同arkui
├── ets
│   └── @arkui-x.bridge.d.ts
├── apiConfig.json
└── systemres
```

### ArkUI-X SDK插件目录结构

ArkUI-X应用按需打包插件库集合，位于arkui-x_darwin_x64_1.0.0.0_Alpha.zip/arkui-x/plugins目录：

```
plugins
├── component
│   ├── lib
│   │   ├── include
│   │   └── ${ui-name}                                        // 一个UI组件一个目录
│   │       ├── ${ui-name}.jar
│   │       ├── android-arm
│   │       ├── android-arm-profile
│   │       ├── android-arm-release
│   │       ├── android-arm64
│   │       ├── android-arm64-profile
│   │       ├── android-arm64-release
│   │       │   └── lib${ui-name}.so
│   │       └── android-x86_64
│   ├── framework
│   │   ├── ios-arm64
│   │   ├── ios-arm64-profile
│   │   │   └── lib${ui-name}.framework
│   │   ├── ios-arm64-release
│   │   ├── ios-arm64-simulator
│   │   └── ios-x86_64-simulator
│   ├── xcframework
│   │   ├── ios
│   │   │   └── lib${ui-name}.xcframework
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       └── ios-arm64_x86_64-simulator
│   │   ├── ios-profile
│   │   │   └── lib${ui-name}.xcframework
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       └── ios-arm64_x86_64-simulator
│   │   └── ios-release
│   │       └── lib${ui-name}.xcframework
│   │           ├── Info.plist
│   │           ├── ios-arm64
│   │           └── ios-arm64_x86_64-simulator
│   └── apiConfig.json
└── api
    ├── lib
    │   ├── include
    │   └── ${module-name}_${submodule-name}                  // 一个API模块一个目录
    │       ├── ${module-name}_${submodule-name}.jar
    │       ├── android-arm
    │       ├── android-arm-profile
    │       ├── android-arm-release
    │       ├── android-arm64
    │       ├── android-arm64-profile
    │       ├── android-arm64-release
    │       │   └── lib${module-name}_${submodule-name}.so
    │       └── android-x86_64
    ├── framework
    │   ├── ios-arm64
    │   ├── ios-arm64-profile
    │   │   └── lib${module-name}_${submodule-name}.framework
    │   ├── ios-arm64-release
    │   ├── ios-arm64-simulator
    │   └── ios-x86_64-simulator
    ├── xcframework
    │   ├── ios
    │   │   └── lib${module-name}_${submodule-name}.xcframework
    │   │       ├── Info.plist
    │   │       ├── ios-arm64
    │   │       └── ios-arm64_x86_64-simulator
    │   ├── ios-profile
    │   │   └── lib${module-name}_${submodule-name}.xcframework
    │   │       ├── Info.plist
    │   │       ├── ios-arm64
    │   │       └── ios-arm64_x86_64-simulator
    │   └── ios-release
    │       └── lib${module-name}_${submodule-name}.xcframework
    │           ├── Info.plist
    │           ├── ios-arm64
    │           └── ios-arm64_x86_64-simulator
    └── apiConfig.json
```

### **arkui-x.json**配置说明

```json
{
  "apiVersion": "10",
  "displayName": "ArkUI-X",
  "meta": {
    "metaVersion": "1.0.0"
  },
  "path": "arkui-x",
  "releaseType": "Beta",
  "version": "1.0.0.0"
}
```

字段解释如下：

- **apiVersion:** ArkUI-X SDK依赖OpenHarmony SDK的版本。
- **displayName:** ArkUI-X SDK在DevEco Studio的显示名称。
- **path:** ArkUI-X SDK下载的后的路径名称。
- **version:** ArkUI-X SDK编译构建版本号，用于转测试。

# ArkUI-X SDK内容详细规格

## Windows平台

### ArkUI-X SDK引擎目录结构

* lib目录：ArkUI-X基础框架跨平台实现。
* ets目录：ArkUI-X独有接口定义和ArkUI跨平台Stage模型相关基础接口配置说明。
* systemres目录：ArkUI渲染一致性资源主题包。

```
arkui-x_windows_x64_1.0.0.0_Alpha.zip/arkui-x/engine
├── lib                                           // ArkUI跨平台引擎及平台适配层
│   ├── include                                   // NAPI和相关辅助C接口
│   ├── arkui
│   │   ├── arkui_android_adapter.jar
│   │   ├── android-arm
│   │   ├── android-arm-profile
│   │   ├── android-arm-release
│   │   ├── android-arm64
│   │   ├── android-arm64-profile
│   │   ├── android-arm64-release
│   │   │   └── libarkui_android.so               // ArkUI跨平台引擎，包含：ArkUI\NAPI\ARk三部分。
│   │   └── android-x86_64
│   ├── third_party
│   └── utils
├── ets                                           // ArkUI-X独有接口定义和ArkUI跨平台Stage模型相关基础接口配置说明。
│   └── @arkui-x.bridge.d.ts
├── apiConfig.json
└── systemres                                     // ArkUI组件渲染一致性系统资源包
```

### ArkUI-X SDK插件目录结构

* component目录：ArkUI组件插件化动态库。
* api目录：ArkTS接口插件化动态库。

```
arkui-x_windows_x64_1.0.0.0_Alpha.zip/arkui-x/plugins
├── component                                                   // ArkUI组件插件化动态库。
│   ├── lib
│   │   ├── include
│   │   └── ${ui-name}
│   │       ├── ${ui-name}_android_adapter.jar                    // 部分组件实现依赖的Android接口。
│   │       ├── android-arm
│   │       ├── android-arm-profile
│   │       ├── android-arm-release
│   │       ├── android-arm64
│   │       ├── android-arm64-profile
│   │       ├── android-arm64-release
│   │       │   └── lib${ui-name}.so                            // ArkUI组件实现。
│   │       └── android-x86_64
│   └── apiconfig.json                                          // ArkUI组件跨平台实现配置说明。
└── api                                                         // ArkTS接口插件化动态库。
    ├── lib
    │   ├── include
    │   └── ${module-name}_${submodule-name}
    │       ├── ${module-name}_${submodule-name}.jar            // ArkTS接口实现依赖的Android接口。
    │       ├── android-arm
    │       ├── android-arm-profile
    │       ├── android-arm-release
    │       ├── android-arm64
    │       ├── android-arm64-profile
    │       ├── android-arm64-release
    │       │   └── lib${module-name}_${submodule-name}.so      // ArkTS接口实现。
    │       └── android-x86_64
    └── apiConfig.json                                          // ArkTS @ohos接口跨平台实现配置说明。
```

## Linux平台

### ArkUI-X SDK引擎目录结构

* lib目录：ArkUI-X基础框架跨平台实现。
* ets目录：ArkUI-X独有接口定义和ArkUI跨平台Stage模型相关基础接口配置说明。
* systemres目录：ArkUI渲染一致性资源主题包。

```
arkui-x_linux_x64_1.0.0.0_Alpha.zip/arkui-x/engine
├── lib                                           // ArkUI跨平台引擎及平台适配层
│   ├── include                                   // NAPI和相关辅助C接口
│   ├── arkui
│   │   ├── arkui_android_adapter.jar            // ArkUI Android平台适配层
│   │   ├── android-arm
│   │   ├── android-arm-profile
│   │   ├── android-arm-release
│   │   ├── android-arm64
│   │   ├── android-arm64-profile
│   │   ├── android-arm64-release
│   │   │   └── libarkui_android.so               // ArkUI跨平台引擎，包含：ArkUI\NAPI\ARk三部分。
│   │   └── android-x86_64
│   ├── third_party
│   └── utils
├── ets                                           // ArkUI-X独有接口定义和ArkUI跨平台Stage模型相关基础接口配置说明。
│   └── @arkui-x.bridge.d.ts
├── apiConfig.json
└── systemres                                     // ArkUI组件渲染一致性系统资源包
```

### ArkUI-X SDK插件目录结构

* component目录：ArkUI组件插件化动态库。
* api目录：ArkTS接口插件化动态库。

```
arkui-x_linux_x64_1.0.0.0_Alpha.zip/arkui-x/plugins
├── component                                                   // ArkUI组件插件化动态库。
│   ├── lib
│   │   ├── include
│   │   └── ${ui-name}
│   │       ├── ${ui-name}_android_adapter.jar                    // 部分组件实现依赖的Android接口。
│   │       ├── android-arm
│   │       ├── android-arm-profile
│   │       ├── android-arm-release
│   │       ├── android-arm64
│   │       ├── android-arm64-profile
│   │       ├── android-arm64-release
│   │       │   └── lib${ui-name}.so                            // ArkUI组件实现。
│   │       └── android-x86_64
│   └── apiconfig.json                                          // ArkUI组件跨平台实现配置说明。
└── api                                                         // ArkTS接口插件化动态库。
    ├── lib
    │   ├── include
    │   └── ${module-name}_${submodule-name}
    │       ├── ${module-name}_${submodule-name}.jar            // ArkTS接口实现依赖的Android接口。
    │       ├── android-arm
    │       ├── android-arm-profile
    │       ├── android-arm-release
    │       ├── android-arm64
    │       ├── android-arm64-profile
    │       ├── android-arm64-release
    │       │   └── lib${module-name}_${submodule-name}.so      // ArkTS接口实现。
    │       └── android-x86_64
    └── apiConfig.json                                          // ArkTS @ohos接口跨平台实现配置说明。
```

## macOS平台

### ArkUI-X SDK引擎目录结构

* lib、framework、xcframework目录：ArkUI-X基础框架跨平台实现。
* ets目录：ArkUI-X独有接口定义和ArkUI跨平台Stage模型相关基础接口配置说明。
* systemres目录：ArkUI渲染一致性资源主题包。

```
arkui-x_darwin_x64_1.0.0.0_Alpha.zip/arkui-x/engine
├── lib                                                 // ArkUI跨平台引擎及平台适配层
│   ├── include                                         // NAPI和相关辅助C接口
│   ├── arkui
│   │   ├── arkui_android_adapter.jar                     // ArkUI Android平台适配层
│   │   ├── android-arm
│   │   ├── android-arm-profile
│   │   ├── android-arm-release
│   │   ├── android-arm64
│   │   ├── android-arm64-profile
│   │   ├── android-arm64-release
│   │   │   └── libarkui_android.so                     // ArkUI跨平台引擎，包含：ArkUI\NAPI\ARk\Ability等部分。
│   │   └── android-x86_64
│   ├── third_party
│   └── utils
├── framework
│   ├── arkui
│   │   ├── ios-arm64
│   │   ├── ios-arm64-profile
│   │   │   └── libarkui_ios.framework                    // ArkUI跨平台引擎及平台适配层
│   │   │       ├── Headers
│   │   │       │   ├── Ace.h
│   │   │       │   ├── AceViewController.h
│   │   │       │   └── include
│   │   │       ├── Info.plist
│   │   │       ├── libarkui_ios
│   │   │       ├── libarkui_ios.podspec
│   │   │       └── Modules
│   │   │           └── module.modulemap
│   │   ├── ios-arm64-release
│   │   ├── ios-arm64-simulator
│   │   └── ios-x86_64-simulator
│   ├── third_party
│   └── utils
├── xcframework
│   ├── arkui
│   │   ├── ios
│   │   │   └── libarkui_ios.xcframework                     // ArkUI跨平台引擎及平台适配层
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       │   └── libarkui_ios.framework
│   │   │       │       ├── Headers
│   │   │       │       │   ├── Ace.h
│   │   │       │       │   ├── AceViewController.h
│   │   │       │       │   └── include
│   │   │       │       ├── Info.plist
│   │   │       │       ├── libarkui_ios
│   │   │       │       ├── libarkui_ios.podspec
│   │   │       │       └── Modules
│   │   │       │           └── module.modulemap
│   │   │       └── ios-arm64_x86_64-simulator
│   │   ├── ios-profile
│   │   │   └── libarkui_ios.xcframework                     // ArkUI跨平台引擎及平台适配层
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       │   └── libarkui_ios.framework
│   │   │       │       ├── Headers
│   │   │       │       │   ├── Ace.h
│   │   │       │       │   ├── AceViewController.h
│   │   │       │       │   └── include
│   │   │       │       ├── Info.plist
│   │   │       │       ├── libarkui_ios
│   │   │       │       ├── libarkui_ios.podspec
│   │   │       │       └── Modules
│   │   │       │           └── module.modulemap
│   │   │       └── ios-arm64_x86_64-simulator
│   │   └── ios-release
│   │       └── libarkui_ios.xcframework                     // ArkUI跨平台引擎及平台适配层
│   │           ├── Info.plist
│   │           ├── ios-arm64
│   │           │   └── libarkui_ios.framework
│   │           │       ├── Headers
│   │           │       │   ├── Ace.h
│   │           │       │   ├── AceViewController.h
│   │           │       │   └── include
│   │           │       ├── Info.plist
│   │           │       ├── libarkui_ios
│   │           │       ├── libarkui_ios.podspec
│   │           │       └── Modules
│   │           │           └── module.modulemap
│   │           └── ios-arm64_x86_64-simulator
│   ├── third_party
│   └── utils
├── ets                                                 // ArkUI-X独有接口定义和ArkUI跨平台Stage模型相关基础接口配置说明。
│   └── @arkui-x.bridge.d.ts
├── apiConfig.json
└── systemres                                           // ArkUI组件渲染一致性系统资源包
```

### ArkUI-X SDK插件目录结构

* component目录：ArkUI组件插件化动态库。
* api目录：ArkTS接口插件化动态库。

```
arkui-x_darwin_x64_1.0.0.0_Alpha.zip/arkui-x/plugins
├── component                                                  // ArkUI组件插件化动态库。
│   ├── lib
│   │   ├── include
│   │   └── ${ui-name}
│   │       ├── ${ui-name}_android_adapter.jar                 // 部分组件实现依赖的Android接口。
│   │       ├── android-arm
│   │       ├── android-arm-profile
│   │       ├── android-arm-release
│   │       ├── android-arm64
│   │       ├── android-arm64-profile
│   │       ├── android-arm64-release
│   │       │   └── lib${ui-name}.so                           // ArkUI组件实现。
│   │       └── android-x86_64
│   ├── framework
│   │   ├── ios-arm64
│   │   ├── ios-arm64-profile
│   │   ├── ios-arm64-release
│   │   │   └── lib${ui-name}.framework                        // ArkUI组件实现。
│   │   │       ├── Headers
│   │   │       ├── Info.plist
│   │   │       ├── lib${ui-name}
│   │   │       ├── lib${ui-name}.podspec
│   │   │       └── Modules
│   │   │           └── module.modulemap
│   │   ├── ios-arm64-simulator
│   │   └── ios-x86_64-simulator
│   ├── xcframework
│   │   ├── ios
│   │   │   └── lib${ui-name}.xcframework                          // ArkUI组件实现。
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       │   └── lib${ui-name}.framework
│   │   │       │       ├── Headers
│   │   │       │       ├── Info.plist
│   │   │       │       ├── lib${ui-name}
│   │   │       │       ├── lib${ui-name}.podspec
│   │   │       │       └── Modules
│   │   │       │           └── module.modulemap
│   │   │       └── ios-arm64_x86_64-simulator
│   │   ├── ios-profile
│   │   │   └── lib${ui-name}.xcframework                          // ArkUI组件实现。
│   │   │       ├── Info.plist
│   │   │       ├── ios-arm64
│   │   │       │   └── lib${ui-name}.framework
│   │   │       │       ├── Headers
│   │   │       │       ├── Info.plist
│   │   │       │       ├── lib${ui-name}
│   │   │       │       ├── lib${ui-name}.podspec
│   │   │       │       └── Modules
│   │   │       │           └── module.modulemap
│   │   │       └── ios-arm64_x86_64-simulator
│   │   └── ios-release
│   │       └── lib${ui-name}.xcframework                          // ArkUI组件实现。
│   │           ├── Info.plist
│   │           ├── ios-arm64
│   │           │   └── lib${ui-name}.framework
│   │           │       ├── Headers
│   │           │       ├── Info.plist
│   │           │       ├── lib${ui-name}
│   │           │       ├── lib${ui-name}.podspec
│   │           │       └── Modules
│   │           │           └── module.modulemap
│   │           └── ios-arm64_x86_64-simulator
│   └── apiConfig.json                                         // ArkTS UI组件跨平台实现配置说明。
└── api                                                        // ArkTS接口插件化动态库。
    ├── lib
    │   ├── include
    │   └── ${module-name}_${submodule-name}
    │       ├── ${module-name}_${submodule-name}_android_adapter.jar       // ArkTS接口实现依赖的Android接口。
    │       ├── android-arm
    │       ├── android-arm-profile
    │       ├── android-arm-release
    │       ├── android-arm64
    │       ├── android-arm64-profile
    │       ├── android-arm64-release
    │       │   └── lib${module-name}_${submodule-name}.so     // ArkTS接口实现。
    │       └── android-x86_64
    ├── framework
    │   ├── ios-arm64
    │   ├── ios-arm64-profile
    │   ├── ios-arm64-release
    │   │   └── lib${module-name}_${submodule-name}.framework  // ArkTS接口实现。
    │   │       ├── Headers
    │   │       ├── Info.plist
    │   │       ├── lib${module-name}_${submodule-name}
    │   │       ├── lib${module-name}_${submodule-name}.podspec
    │   │       └── Modules
    │   │           └── module.modulemap
    │   ├── ios-arm64-simulator
    │   └── ios-x86_64-simulator
    ├── xcframework
    │   ├── ios
    │   │   └── lib${module-name}_${submodule-name}.xcframework    // ArkTS接口实现。
    │   │       ├── Info.plist
    │   │       ├── ios-arm64
    │   │       │   └── lib${module-name}_${submodule-name}.framework
    │   │       │       ├── Headers
    │   │       │       ├── Info.plist
    │   │       │       ├── lib${module-name}_${submodule-name}
    │   │       │       ├── lib${module-name}_${submodule-name}.podspec
    │   │       │       └── Modules
    │   │       │           └── module.modulemap
    │   │       └── ios-arm64_x86_64-simulator
    │   ├── ios-profile
    │   │   └── lib${module-name}_${submodule-name}.xcframework    // ArkTS接口实现。
    │   │       ├── Info.plist
    │   │       ├── ios-arm64
    │   │       │   └── lib${module-name}_${submodule-name}.framework
    │   │       │       ├── Headers
    │   │       │       ├── Info.plist
    │   │       │       ├── lib${module-name}_${submodule-name}
    │   │       │       ├── lib${module-name}_${submodule-name}.podspec
    │   │       │       └── Modules
    │   │       │           └── module.modulemap
    │   │       └── ios-arm64_x86_64-simulator
    │   └── ios-release
    │       └── lib${module-name}_${submodule-name}.xcframework    // ArkTS接口实现。
    │           ├── Info.plist
    │           ├── ios-arm64
    │           │   └── lib${module-name}_${submodule-name}.framework
    │           │       ├── Headers
    │           │       ├── Info.plist
    │           │       ├── lib${module-name}_${submodule-name}
    │           │       ├── lib${module-name}_${submodule-name}.podspec
    │           │       └── Modules
    │           │           └── module.modulemap
    │           └── ios-arm64_x86_64-simulator
    └── apiConfig.json                                         // ArkTS @ohos接口跨平台实现配置说明。
```