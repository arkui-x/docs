# ArkUI-X SDK介绍

ArkUI-X SDK是ArkUI-X开源项目的编译产物，可将ArkUI-X SDK集成到现有Android和iOS应用工程中，使开发者基于一套ArkTS主代码，就可以构建支持多平台的精美、高性能应用。获取渠道如下：

* 通过DevEco Studio下载ArkUI-X SDK，Windows平台进入DevEco Studio > File > Settings > ArkUI-X选项进行下载（macOS平台为DevEco Studio > Preferences > ArkUI-X）。
* 通过开源社区Release Notes下载ArkUI-X SDK，参见：[ArkUI-X Release Notes](../../release-notes/README.md)
* 通过ArkUI-X源码自行编译ArkUI-X SDK，编译方法参见：[ArkUI-X SDK构建说明](../../framework-dev/tutorial/how-to-package-arkui-x-sdk.md#arkui-sdk构建说明)。

## ArkUI-X SDK内容组成

ArkUI-X SDK提供的内容，主要包括ArkUI-X SDK基础引擎库、插件库、工具链和SDK配置说明等文件。通过DevEco Studio或ACE Tools命令行工具集成使用时，需按照特定目录结构进行配置，如下示例为macOS平台上DevEco Studio中ArkUI-X SDK的默认下载路径。

```
/Users/用户名/Library/ArkUI-X/Sdk
├── {versioncode}                    // arkui-x.json中的apiVersion字段值，文件位于ArkUI-X SDK根目录中，ArkUI-X 1.0.0 Release版本versioncode取值为：10。
│   └── arkui-x
│       ├── engine                   // ArkUI-X SDK引擎部分
│       │   ├── lib                  // ArkUI-X Android平台应用集成依赖库。
│       │   ├── framework            // ArkUI-X iOS平台应用集成依赖库。
│       │   ├── xcframework          // ArkUI-X iOS平台应用集成依赖库。
│       │   ├── ets                  // ArkUI-X增量接口，比如：@arkui-x.bridge
│       │   ├── apiConfig.json       // engine库配置文件，用于IDE和ACE Tools解析，以支持应用构建按需打包。
│       │   └── systemres            // OpenHarmony/HarmonyOS应用系统资源，支持ArkUI-X跨平台UX一致性。
│       ├── plugins                  // ArkUI-X SDK插件部分
│       │   ├── component            // ArkUI组件插件库，apiConfig.json
│       │   └── api                  // @ohos接口插件库，apiConfig.json
│       ├── toolchains               // 工具链，比如ACE Tools
│       ├── sdkConfig.json           // 增量d.ts路径和接口前缀配置
│       ├── arkui-x.json             // SDK管理配置，流水线自动生成。
│       └── NOTICE.txt
└── licenses                         // DevEco Studio下载ArkUI-X SDK时，用户同意的ArkUI-X SDK协议。
    ├── ArkUI-X-SDK
    └── ArkUI-X-SDK.sha256
```

>说明：ArkUI-X SDK内部详细文件目录结构请参考[ArkUI-X SDK目录结构介绍](../quick-start/sdk-structure-guide.md)。

## ArkUI-X命令行工具

[ACE Tools](../quick-start/start-with-ace-tools.md)，是一套为ArkUI-X应用开发者提供的命令行工具，包括开发环境检查，新建项目，编译打包，安装调试等功能。