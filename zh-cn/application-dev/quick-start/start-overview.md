# 开发准备

本文档适用于ArkUI跨平台应用开发的初学者。通过开发环境搭建、应用工程创建、编译和运行，熟悉ArkUI跨平台应用开发基本流程。

在开始之前，您需要了解有关跨平台应用的一些基本概念：ArkUI框架的简单说明、ArkUI-X的基本概念。

## 基本概念

### ArkUI

ArkUI是一套构建分布式应用的声明式UI开发框架。它具备简洁自然的UI信息语法、丰富的UI组件、多维的状态管理，以及实时界面预览工具，帮助您提升应用开发效率，并能在多种设备上实现生动而流畅的用户体验。详情可参考[ArkUI框架介绍](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/ui/arkui-overview.md)

### ArkUI-X

ArkUI跨平台框架(ArkUI-X)进一步将ArkUI开发框架扩展到了多个OS平台：目前支持OpenHarmony、Android、 iOS，后续会逐步增加更多平台支持。开发者基于一套主代码，就可以构建支持多平台的精美、高性能应用。


## 开发工具

您可以通过自己偏好的文本编辑器，结合ACE Tools命令行工具来开发ArkUI-X应用。然而，我们推荐您使用DevEco Studio以获取更好的开发体验，除提供代码智能编辑和双向预览功能外，还会对ArkTS接口进行跨平台过滤和使用提示。

### IDE工具（DevEco Studio）

1. DevEco Studio为ArkUI-X应用构建提供了简单的集成开发环境，版本要求：V4.0 Beta2。请参考[社区版本软件和工具配套关系](https://gitee.com/openharmony/docs/blob/master/zh-cn/release-notes/OpenHarmony-v4.0-beta2.md#配套关系)完成DevEco Studio下载和安装。

2. 请参考[环境配置](./start-with-dev-environment.md)，完成基于**DevEco Studio**的ArkUI-X开发环境配置。

### 命令行工具（ACE Tools）

1. ACE Tools默认随ArkUI-X SDK发布，获取渠道请参见[SDK介绍](../tools/how-to-use-arkui-x-sdk.md)。

2. 请参考[环境配置](./start-with-ace-tools.md#环境准备)和[命令安装](./start-with-ace-tools.md#命令安装)，完成基于命令行的ArkUI-X开发环境配置。

完成上述操作和基本概念的理解后，即可参照[DevEco Studio使用说明](./start-with-deveco-studio.md)或[ACE Tools使用说明](./start-with-ace-tools.md#使用说明)，以及[使用ArkTS语言开发](./start-with-ets-stage.md)中的章节进行下一步ArkUI-X应用开发体验和学习。
