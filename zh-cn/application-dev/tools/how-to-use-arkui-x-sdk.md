# ArkUI-X简介

本文档配套ArkUI-X项目，将OpenHarmony ArkUI开发框架扩展到不同的OS平台，比如Android和iOS平台， 让开发者基于ArkUI，可复用大部分的应用代码（UI以及主要应用逻辑）并可以部署到相应的OS平台，降低跨平台应用开发成本。

ArkUI-SDK包含ArkUI跨平台运行时，组件和接口插件包，以及ACE Tools命令行工具，你可以用来开发ArkUI-X应用，并布置到Android和iOS平台。ArkUI-X SDK获取，请[点击](../../release-notes/ArkUI-X-v0.1.0-beta.md#从镜像站点获取)。

## ArkUI-X SDK基础内容

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

>说明：ArkUI-X SDK内部详细结构请参考[ArkUI-X目录结构](../quick-start/sdk-structure-guide.md)。

## ArkUI-X命令行工具

ACE Tools，是一套为ArkUI-X应用开发者提供的命令行工具，包括开发环境检查，新建项目，编译打包，安装调试等功能。[详情参见](../quick-start/start-with-ace-tools.md)。