# 术语

## A

- ### ArkUI

  方舟开发框架（简称：ArkUI），是一套构建OpenHarmony应用界面的UI开发框架，它提供了简洁自然的UI语法与包括UI组件、动画机制、事件交互等在内的UI开发基础设施，以满足应用开发者的可视化界面开发需求。更多ArkUI内容，[请参考](https://developer.harmonyos.com/cn/develop/arkUI/)。

- ### ArkUI-X

  ArkUI-X，是扩展ArkUI开发框架到多个OS平台，目前支持OpenHarmony、HarmonyOS、Android、 iOS，后续会逐步增加更多平台支持。开发者基于一套主代码，就可以构建支持多平台的精美、高性能应用。

- ### ACE

  ArkUI跨平台开发环境（简称：ACE，ArkUI Cross-Platform Environment）是一套为ArkUI-X应用开发者提供的命令行工具包（ACE Tools），包括开发环境检查，新建项目，编译打包，安装调试等功能。

## N

- ### Node-API

  Node-API是用于封装JavaScript能力为Native插件的API，独立于底层JavaScript，并作为Node.js的一部分。ArkUI-X的N-API组件对Node-API的接口进行了重新实现，当前支持Node-API标准库中的[部分接口](./application-dev/reference/native-lib/third_party_napi/napi.md)。

## P

- ### Platform Bridge

  平台桥接(@arkui-x.bridge)用于客户端（ArkUI）和平台（Android或iOS）之间传递消息，主要用于ArkUI与平台双向传递消息、ArkTS调用平台API、平台调用ArkTS的方法。