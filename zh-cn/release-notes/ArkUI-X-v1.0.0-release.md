# ArkUI-X 1.0.0 Release

## 版本概述

ArkUI-X 1.0.0正式版本如期而至，配套OpenHarmony SDK API 10。相比1.0.0预览版，新增API 10接口覆盖范围，使应用开发能力更加丰富；ArkUI组件基本覆盖API 10接口范围；共享包能力覆盖OpenHarmony提供的HAR（Harmony Archive）静态共享包和HSP（Harmony Shared Package）动态共享包；ACE Tools命令行工具使用体验优化，简化命令使用方式；动态化能力支持ArkTS页面动态下发和加载显示。另外，该版本在稳定性和一次开发多平台部署等方面也得到了进一步增强，欢迎开发者踊跃使用并给我们提出宝贵意见。

您可以阅读本文档了解更多关键特性及能力。

## 特性说明

### 应用框架

* 完善Stage模型跨平台能力，Ability与Activity和ViewController生命周期对接适配，StageApplication初始化时机增强，详情参见：[Android平台Ability开发说明](../application-dev/quick-start/start-with-ability-on-android.md)和[iOS平台Ability开发说明](../application-dev/quick-start/start-with-ability-on-ios.md)。

### 应用包管理

这里，应用包管理特指OpenHarmony应用工程结构跨平台编译适配，不包括OpenHarmony包管理运行时，编译后的应用程序包内容在Android和iOS平台仍然有原生包管理进行管理。

* 支持OpenHarmony应用程序结构和编译打包后的应用程序包结构：Module分为“Ability”和“Library”两种类型，“Ability”类型的Module对应于编译后的HAP（Harmony Ability Package）；“Library”类型的Module对应于HAR（Harmony Archive），或者HSP（Harmony Shared Package）。

* 支持多module和UIAbility开发：在开发态，可以在跨平台工程中创建一个或者多个Module。一个Module可以包含一个或多个UIAbility组件。

### 动态化能力

* 支持业务内容快速部署和更新，即ArkTS编译产物和资源支持动态下发和加载。另外，Android平台支持ArkUI-X基础库和插件库动态下发。
* 动态化内容(Bundle)涵盖HAP（Harmony Ability Package）和HSP（Harmony Shared Package）两种应用程序包结构内容。

### 接口范围

* 完善ArkUI API 10组件跨平台适配，包括：组件通用信息(事件/属性/手势)，基础组件，容器组件，媒体组件，绘制组件，画布组件，动画，全局UI和页面方法，以及状态管理和渲染控制等能力。详情参见：[组件参考](../application-dev/reference/arkui-ts/README.md)。

* 提供Web组件基础能力跨平台适配，包括网页显示、手势操作和输入法等基础能力，更多接口能力后续版本逐步开放。

* 完善ArkTS接口跨平台适配，基础API初步完成跨平台适配，主要包括：Ability框架、包管理（静态包信息）、图形图像（窗口/屏幕属性）、媒体（图片处理）、资源管理、全球化、网络管理（WebSocket连接/Socket连接/Http数据请求/网络连接管理/上传下载）、文件IO、设备信息、数据管理（用户首选项/关系型数据库）、程序访问控制管理、语言基础库和日志打印等接口。详情参见：[ArkTS接口参考](../application-dev/reference/apis/README.md)。

> 说明：跨平台接口以OpenHarmony Public接口为基础，接口范围为API 10，支持列表详见：[API参考](../application-dev/reference/README.md)，列表之外的接口会在后续ArkUI-X版本逐步开放。


### 应用开发工具

- ACE Tools，是一套为ArkUI-X开发者提供的命令行工具，包括开发环境检查，新建项目，编译打包，安装调试。相比ArkUI-X 1.0.0 Canary版本，用户体验全面升级，比如：新增了模拟器管理、工程创建增强、辅助命令优化、环境检查优化、安装运行优化等，并提供运行命令Shell脚本，省略NPM依赖安装，支持直接运行或环境变量配置。[详情参见ACE Tools快速入门](../application-dev/quick-start/start-with-ace-tools.md)。
- DevEco Studio，是HarmonyOS默认的应用程序开发集成工具，当前仅支持ArkUI-X应用工程导入和编译构建。跨平台工程创建、安装运行和调试等功能在后续版本逐步开放。[详情参见DevEco Studio使用说明](../application-dev/tools/how-to-use-deveco-studio.md)。

## 配套关系

  **表1** 版本软件和平台配套关系

| 目标平台     | 兼容OS版本              | 备注 |
| ----------- | ----------------------------------- | ---- |
| OpenHarmony | 4.0 Release (API Version 10) |  HUAWEI DevEco Studio获取方式：<br />[Windows(64-bit)](https://contentcenter-vali-drcn.dbankcdn.cn/pvt_2/DeveloperAlliance_package_901_9/9a/v3/HBD3TfhiT_GFqeX44Qcwtg/devecostudio-windows-4.0.0.600.zip?HW-CC-KV=V1&HW-CC-Date=20231027T004333Z&HW-CC-Expire=315360000&HW-CC-Sign=279824A013505EFC063997614DC1B6AB1C3A2EE5AC48CEF15DDB3E1F79DA435A)  <br />SHA256校验码：2c88cf43e1ef6ba722aac31eccc8ef92f07a9b72e43a9c1df127017828a22137<br />[Mac(X86)](https://contentcenter-vali-drcn.dbankcdn.cn/pvt_2/DeveloperAlliance_package_901_9/e0/v3/y3Qc4UHsTn6i1M7yr3hVYg/devecostudio-mac-4.0.0.600.zip?HW-CC-KV=V1&HW-CC-Date=20231027T004531Z&HW-CC-Expire=315360000&HW-CC-Sign=07F14E7173D87ABF73777BA0CF7ADF1C1264A7D94909976471AC420C1932E8A6)  <br />SHA256校验码：25e491458eec50b4abddf5bed6aa85893801d70afbce02958f17bd904619405a<br />[Mac(ARM)](https://contentcenter-vali-drcn.dbankcdn.cn/pvt_2/DeveloperAlliance_package_901_9/94/v3/b0yynFMFSGGvbe--AQQR9w/devecostudio-mac-arm-4.0.0.600.zip?HW-CC-KV=V1&HW-CC-Date=20231027T004429Z&HW-CC-Expire=315360000&HW-CC-Sign=451E5B5C6B6E721A6C35E96FD67791D50ADEC56E987D87CD61E9E5152F8D6626)  <br />SHA256校验码：284cb01f7b819e0da1d4fcacbbbbe8017ba220b5e3b9b1d5e4cc59ea30456acc |
| Android     | Android 8<sup>+</sup> (API level 26<sup>+</sup>)       | NA   |
| iOS         | iOS 10<sup>+</sup>                             | NA   |


## 源码获取

### 前提条件

1. 注册码云gitee帐号。

2. 注册码云SSH公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)。

3. 安装[git客户端](https://gitee.com/link?target=https%3A%2F%2Fgit-scm.com%2Fbook%2Fzh%2Fv2%2F%25E8%25B5%25B7%25E6%25AD%25A5-%25E5%25AE%2589%25E8%25A3%2585-Git)和[git-lfs](https://gitee.com/vcs-all-in-one/git-lfs?_from=gitee_search#downloading)并配置用户信息。
  
   ```
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. 安装码云repo工具，可以执行如下命令。
  
   ```
   curl -s https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 > /usr/local/bin/repo  #如果没有权限，可下载至其他目录，并将其配置到环境变量中chmod a+x /usr/local/bin/repo
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```


### 通过repo获取

**方式一（推荐）**

通过repo + ssh下载（需注册公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)）。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u git@gitee.com:arkui-x/manifest.git -b ArkUI-X-1.0.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u git@gitee.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u https://gitee.com/arkui-x/manifest.git -b ArkUI-X-1.0.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u https://gitee.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 1.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/darwin/arkui-x-darwin-x64-1.0.0.0-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/darwin/arkui-x-darwin-x64-1.0.0.0-Release.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 1.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/darwin/arkui-x-darwin-arm64-1.0.0.0-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/darwin/arkui-x-darwin-arm64-1.0.0.0-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 1.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/windows/arkui-x-windows-x64-1.0.0.0-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/windows/arkui-x-windows-x64-1.0.0.0-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 1.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/linux/arkui-x-linux-x64-1.0.0.0-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.0.0.0/linux/arkui-x-linux-x64-1.0.0.0-Release.zip.sha256) |

## Samples

**表3** Samples列表

| 项目名称      | 简介                                                         |
| ------------- | ------------------------------------------------------------ |
| HelloWorld | HellWorld应用工程示例，支持Android、iOS和OpenHarmony应用构建。 |
| Shopping | 仿购物应用工程示例，支持Android、iOS和OpenHarmony应用构建。   |
| HealthyDiet | 健康饮食应用工程示例，支持Android、iOS和OpenHarmony应用构建。|
| Native | NAPI应用工程示例，支持Android、iOS和OpenHarmony应用构建。|
| Library | 平台库应用工程示例，支持Android、iOS和OpenHarmony应用构建。|

请访问[Samples](https://gitee.com/arkui-x/samples)仓了解更多消息。
