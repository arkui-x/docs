# 框架开发导读

框架开发文档用于指导开发者通过ArkUI-X项目主库提供的框架代码，完成ArkUI-X项目编译环境搭建，代码下载、代码编译、代码贡献等操作。目前，ArkUI-X项目已完成ArkUI开发框架在Android/iOS/OpenHarmony平台上的基础能力适配。

在框架开发的文档中，您可以获取到如下几方面的内容：

## 快速开始

[快速开始](README.md#快速开始)可以帮助开发者快速了解跨平台框架开发的基本流程，主要包含ArkUI-X项目编译环境搭建，代码下载，代码编译，以及开发跨平台框架所必备的基础知识。

## 框架开发

统一的API扩展机制复用OpenHarmony的[NAPI机制](../application-dev/quick-start/ffi-napi-introduction.md)和ArkTS接口定义，通过提供JS->C++/Java/ObjectC互调机制，即可在Android和iOS平台上扩展JS接口和实现OpenHarmony接口定义，并且支持开发者开发三方插件。

## API参考

API参考仅提供ArkUI-X项目已支持的组件列表、接口列表和平台集成接口列表，可以帮助开发者快速查找到支持的指定接口的详细描述和调用方法。

内容包括：

- [组件参考（基于ArkTS的声明式开发范式）](../application-dev/reference/arkui-ts/README.md)
- [接口参考（ArkTS及JS API）](../application-dev/reference/apis/README.md)
- [接口参考（Native API）](../application-dev/reference/native-apis/README.md)
- 平台集成
  - [Android](../application-dev/reference/arkui-for-android/README.md)
  - [iOS](../application-dev/reference/arkui-for-ios/README.md)
