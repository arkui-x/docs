# 框架开发导读

框架开发文档用于指导开发者通过ArkUI-X项目主库提供的框架代码，完成ArkUI-X项目编译环境搭建，代码下载、代码编译、代码贡献等操作。目前，ArkUI-X项目已完成ArkUI开发框架在Android/iOS/OpenHarmony平台上的基础能力适配。

在框架开发的文档中，您可以获取到如下几方面的内容：

## 快速开始

[快速开始](quick-start/start-overview.md)可以帮助开发者了解跨平台框架开发的基本流程。

这一部分包含了ArkUI-X项目编译环境搭建，代码下载、代码编译、代码贡献，以及开发跨平台框架所必备的基础知识。

开发的基础知识包含了ArkUI-X项目总体设计、框架代码工程及构建介绍。

## 开发

统一的API扩展机制复用OpenHarmony的[NAPI机制](../framework-dev/napi/napi-guidelines.md)和JS接口定义，通过提供的JS->C++/Java/ObjectC的互调机制，即可在Android和iOS平台上扩展JS接口和实现OpenHarmony接口，并且支持开发者开发三方插件。

## 示例教程

我们提供了[sample工程](https://gitee.com/arkui-x/samples)，为开发者提供更丰富的开发参考，辅助开发者理解功能逻辑，提升开发效率。并且，示例工程可以通过CLI命令行工具直接运行。

通过示例教程开发者可在sample工程中加入开发者自己扩展的JS接口调用。

## API参考

API参考提供了ArkUI-X项目已支持的组件列表、接口列表和平台集成接口的文档说明，可以帮助开发者快速查找到支持的指定接口的详细描述和调用方法。

内容包括：

- [组件参考（基于ArkTS的声明式开发范式）](../application-dev/reference/arkui-ts/readme.md)
- [接口参考（ArkTS及JS API）](../application-dev/reference/apis/readme.md)
- 平台集成
  - [Android](../application-dev/reference/arkui-for-android/readme.md)
  - [iOS](../application-dev/reference/arkui-for-ios/readme.md)
