# 简介

本文档配套ArkUI-X项目，将OpenHarmony ArkUI开发框架扩展到不同的OS平台，比如Android和iOS平台， 让开发者基于ArkUI，可复用大部分的应用代码（UI以及主要应用逻辑）并可以部署到相应的OS平台，降低跨平台应用开发成本。

ArkUI-SDK包含ArkUI跨平台运行时，组件和接口插件包，以及ACE Tools命令行工具，你可以用来开发ArkUI-X应用，并布置到Android和iOS平台。详细内容如下：

```
ArkUI-X SDK
├── engine                   // ArkUI-X SDK引擎部分
│   ├── lib                  // ArkUI-X Android平台应用集成依赖库。
│   ├── framework            // ArkUI-X iOS平台应用集成依赖库。
│   ├── xcframework          // ArkUI-X iOS平台应用集成依赖库。
│   ├── ets                  // ArkUI-X增量接口，比如：@arkui-x.bridge
│   ├── apiConfig.json       // engine库配置文件，用于IDE和ACE Tools解析，以支持应用构建按需打包。
│   └── systemres            // OpenHarmony/HarmonyOS应用系统资源，支持ArkUI-X跨平台UX一致性。
├── plugins                  // ArkUI-X SDK插件部分
│   ├── component            // ArkUI组件插件库，apiConfig.json
│   └── api                  // @ohos接口插件库，apiConfig.json
├── toolchains               // 工具链，比如ACE Tools
├── sdkConfig.json           // 增量d.ts路径和接口前缀配置
├── arkui-x.json             // SDK管理配置，流水线自动生成
└── NOTICE.txt
```

>说明：ArkUI-X SDK内部详细结构请参考[ArkUI-X目录结构](../../application-dev/quick-start/sdk-structure-guide.md)。

下面将分别讲述：如何配置ArkUI-X SDK内容白名单，如何编译生成ArkUI-X SDK包，以及如何验证调试生成的ArkUI-X SDK包。

## ArkUI-SDK配置说明

这里，ArkUI-SDK白名单配置说明以@ohos接口跨平台实现为例，通过[@ohos.i18n (国际化-I18n)](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-i18n.md)进行讲述。

### @ohos.i18n接口跨平台实现

@ohos接口定义跨平台实现主要涉及的ArkUI-X社区代码仓为：https://gitee.com/arkui-x/plugins
@ohos.i18n主体开发目录为：https://gitee.com/arkui-x/plugins/tree/master/i18n
这里，对于@ohos.i18n接口定义如何具体实现跨平台不做详细描述，请参考：[Android平台扩展@ohos接口](how-to-archieve-arkts-interface-on-android.md)和[iOS平台扩展@ohos接口](how-to-archieve-arkts-interface-on-ios.md)。

- @ohos.i18n接口定义跨平台实现完成后，提供[i18n_static_android和i18n_static_ios](https://gitee.com/arkui-x/plugins/blob/master/i18n/BUILD.gn)两个静态链接GN Targets。
- 根据模块名规则，@ohos.i18n的模块名为i18n，需配置在插件列表中[common_plugin_libs](https://gitee.com/arkui-x/plugins/blob/master/plugin_lib.gni)，作为GN插件模板(plugin_lib)的输入。
- plugin_lib模板会在[arkui_for_android仓](https://gitee.com/arkui-x/arkui_for_android/blob/master/build/BUILD.gn)和[arkui_for_ios仓](https://gitee.com/arkui-x/arkui_for_ios/blob/master/build/BUILD.gn)进行调用。分别生成动态链接Targets：
  - //foundation/arkui/ace_engine/adapter/android/build:i18n
  - //foundation/arkui/ace_engine/adapter/ios/build:libi18n
- 由于i18n涉及Android平台接口调用，还会提供Java Library GN Targets：//plugins/i18n/android/java:i18n_plugin_java

### @ohos.i18n接口SDK白名单配置

@ohos.i18n接口定义跨平台实现后，需通过白名单配置输出到ArkUI-X SDK包中，涉及代码仓为：
- https://gitee.com/arkui-x/build_plugins
- https://gitee.com/arkui-x/interface_sdk

[Android平台白名单配置](https://gitee.com/arkui-x/build_plugins/blob/master/sdk/arkui_cross_sdk_description_std.json)
```json
   ...
    {
        "install_dir": "arkui-x/plugins/api/lib/i18n/arch_type",                     // 用于指定输出到ArkUI-X SDK哪个目录下。
        "module_label": "//foundation/arkui/ace_engine/adapter/android/build:i18n",  // 需要打包到ArkUI-X SDK的内容(SO动态库)
        "target_os": [
            "linux",
            "windows",
            "darwin"
        ]
    },
    {
        "install_dir": "arkui-x/plugins/api/lib/i18n",                     // 用于指定输出到ArkUI-X SDK哪个目录下。
        "module_label": "//plugins/i18n/android/java:i18n_plugin_java",    // 需要打包到ArkUI-X SDK的内容(Jar包)
        "target_os": [
            "linux",
            "windows",
            "darwin"
        ]
    },
    ...
```

[iOS平台白名单配置](https://gitee.com/arkui-x/build_plugins/blob/master/sdk/arkui_cross_sdk_description_std.json)

```json
    ...
    {
        "install_dir": "arkui-x/plugins/api/framework/arch_type/libi18n.framework",   // 用于指定输出到ArkUI-X SDK哪个目录下。
        "module_label": "//foundation/arkui/ace_engine/adapter/ios/build:libi18n",    // 需要打包到ArkUI-X SDK的内容(Framework动态库)
        "target_os": [
            "darwin"
            ]
    },
    {
        "install_dir": "arkui-x/plugins/api/xcframework/platform_type/libi18n.xcframework",  // 用于指定输出到ArkUI-X SDK哪个目录下。
        "module_label": "//interface/sdk:copy_libi18n_xcframework",                          // 需要打包到ArkUI-X SDK的内容(XCFramework动态库)
        "target_os": [
            "darwin"
            ]
    },
    ...
```

其中，libi18n.xcframework需要在[interface_sdk仓](https://gitee.com/arkui-x/interface_sdk/blob/master/BUILD.gn)完成GN Targets编写。

```GN
  copy_xcframework("copy_libi18n_xcframework") {
    deps = [ "//foundation/arkui/ace_engine/adapter/ios/build:libi18n" ]
    sources = [
      "${root_out_dir}/lib.ios.framework/arkui/plugins/libi18n.xcframework",
    ]
    outputs = [ "${root_out_dir}/lib.xcframework/${target_name}" ]
  }
```

### @ohos.i18n接口调用解析

ArkUI-X SDK中engine和plugins目录都会包含apiConfig.json配置文件，用于DevEco Studio和ACE Tools解析，可使开发者只关注ArkTS代码开发，无需关注引用的ArkUI控件和@ohos接口插件。这里，对于如何解析apiConfig.json不做描述，只讲述如何配置apiConfig.json文件，涉及代码仓为：https://gitee.com/arkui-x/interface_sdk

```json
    ...
    {
        "module": "ohos.i18n",                                           // 表示OpenHarmony中的i18n接口模块：@ohos.i18n
        "library": {
            "android": [                                                 // 表示i18n在Android平台进行应用开发时，哪些库需打包到Android安装包中。
                "lib/i18n/ace_i18n_plugin_android.jar",
                "lib/i18n/arch_type/libi18n.so"
            ],
            "ios":[ "xcframework/build_modes/libintl.xcframework" ]      // 表示i18n在iOS平台进行应用开发时，哪些库需打包到iOS安装包中。
        },
        "deps": {
            "android": [ "engine/utils/plugin_util" ],                   // 表示i18n在Android平台进行应用开发时，哪些依赖库需打包到Android安装包中。
            "ios":[]                                                     // 表示i18n在iOS平台进行应用开发时，哪些依赖库需打包到iOS安装包中，空代表没有依赖。
        }
    },
    ...
```

## ArkUI-SDK构建说明

ArkU-SDK构建在ArkUI-X框架[基础构建](../quick-start/start-with-build.md)上新增了ArkUI-X SDK包构建指令，详细如下：

### Linux平台编译

- 构建ArkUI-X Debug，Release和Profile全量版本，仅用于Android平台。
```
./build.sh --product-name arkui-cross --target-os android --gn-args enable_auto_pack=true
```

- 构建ArkUI-X Debug和Release版本，仅用于Android平台。
```
./build.sh --product-name arkui-cross --target-os android --gn-args enable_auto_pack=true build_type=no_profile
```

- 构建ArkUI-X Release版本，仅用于Android平台。
```
./build.sh --product-name arkui-cross --target-os android --gn-args enable_auto_pack=true build_type=release
```

### Mac平台编译

#### Android和iOS联合打包

- 构建ArkUI-X Debug，Release和Profile全量版本，可用于Android和iOS平台。
```
./build.sh --product-name arkui-cross --target-os ios --gn-args enable_auto_pack=true build_android=true
```

- 构建ArkUI-X Debug和Release版本，可用于Android和iOS平台。
```
./build.sh --product-name arkui-cross --target-os ios --gn-args enable_auto_pack=true build_type=no_profile build_android=true
```

- 构建ArkUI-X Release版本，可用于Android和iOS平台。
```
./build.sh --product-name arkui-cross --target-os ios --gn-args enable_auto_pack=true build_type=release build_android=true
```

#### iOS

- 构建ArkUI-X Debug，Profile和Release全量版本，仅用于iOS平台。
```
./build.sh --product-name arkui-cross --target-os ios --gn-args enable_auto_pack=true
```

- 构建ArkUI-X Debug和Release版本，仅用于iOS平台。
```
./build.sh --product-name arkui-cross --target-os ios --gn-args enable_auto_pack=true build_type=no_profile
```

- 构建ArkUI-X Release版本，仅用于iOS平台。
```
./build.sh --product-name arkui-cross --target-os ios --gn-args enable_auto_pack=true build_type=release
```

#### Android

- 构建ArkUI-X Debug，Profile和Release全量版本，仅用于Android平台。
```
./build.sh --product-name arkui-cross --target-os android --gn-args enable_auto_pack=true
```

- 构建ArkUI-X Debug和Release版本，仅用于Android平台。
```
./build.sh --product-name arkui-cross --target-os android --gn-args enable_auto_pack=true build_type=no_profile
```

- 构建ArkUI-X Release版本，仅用于Android平台。
```
./build.sh --product-name arkui-cross --target-os android --gn-args enable_auto_pack=true build_type=release
```

## ArkUI-SDK调试说明

- ArkUI-X SDK编译输出目录为：out/arkui-cross/packages/arkui-cross

- 替换当前PC上已安装的ArkUI-X SDK。注意：如果使用ACE Tools进行应用工程构建和调试，需重新下载ACE命令依赖，可[参考](../../application-dev/quick-start/start-with-ace-tools.md#安装ace命令)。


