# ArkUI-X 1.0.0 Alpha

## 版本概述

首次发布ArkUI-X 1.0.0 Alpha版本，主要能力范围包括：

- 应用开发范式：支持基于ArkTS的声明式开发范式。
- 应用开发模型：支持Stage开发模型。
- 开发者工具：提供DevEco Studio(IDE)和ACE Tools(命令行)两种ArkUI-X应用构建工具。
- 混合开发能力：ArkTS声明式开发范式和Stage模型支持集成在现有iOS/Android应用中，通过现有应用加载，解析和运行。
- 跨语言调用能力：提供[FFI(Node-API)](../application-dev/quick-start/ffi-napi-introduction.md)和[平台桥接](../application-dev/quick-start/platform-bridge-introduction.md)两种机制，用于API扩展和平台插件开发。
- 基础测试调试：支持单元/UI/XTS集成测试和ArkTS断点调试。

### 接口范围

ArkUI跨平台接口包含OpenHarmony接口和自定义扩展接口，OpenHarmony接口以Public接口为基础，接口范围为API10<sup>+</sup>，具体支持列表详见[API参考](../application-dev/reference/README.md)。

>说明：ArkUI-X 1.0.0 Alpha版本为ArkUI-X首次发布的预览版本，除提供[ArkUI控件](../application-dev/reference/arkui-ts/README.md)和部分[@ohos接口](../application-dev/reference/apis/README.md)之外，暂不提供其它OpenHarmony接口定义的跨平台实现。

### 应用开发工具

- ACE Tools，是一套为ArkUI-X开发者提供的命令行工具，包括开发环境检查，新建项目，编译打包，安装调试。[详情参见ACE Tools快速入门](../application-dev/quick-start/start-with-ace-tools.md)。
- DevEco Studio，是OpenHarmony和HarmonyOS默认的应用程序IDE开发工具，同时支持ArkUI-X应用创建和构建等功能。[详情参见DevEco Studio使用说明](../application-dev/tools/how-to-use-deveco-studio.md)。

## 配套关系

  **表1** 版本软件和平台配套关系

| 目标平台    | 项目编译使用OS SDK版本              | 备注 |
| ----------- | ----------------------------------- | ---- |
| OpenHarmony | 4.0 (API Version 10) |  Beta2  |
| HarmonyOS   | 4.0.0 (API 10) | NA  |
| Android     | Quince Tart 8<sup>+</sup> (API level 26<sup>+</sup>)       | NA   |
| iOS         | iOS 10<sup>+</sup>                             | NA   |

>说明：Alpha版本为面向特定开发者发布的早期预览版本，不承诺UI和API稳定性。由于HarmonyOS 4.0.0未发布，暂不支持HarmonyOS设备体验。

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

通过repo + ssh 下载（需注册公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)）。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```
   repo init -u git@gitee.com:arkui-x/manifest.git -b ArkUI-X-v1.0.0-Alpha --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```
   repo init -u git@gitee.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Alpha --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https 下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```
   repo init -u https://gitee.com/arkui-x/manifest.git -b ArkUI-X-v1.0.0-Alpha --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```
   repo init -u https://gitee.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Alpha --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

### 从镜像站点获取

**表2** 获取源码和SDK路径

| 版本源码                                  | **版本信息** | **下载站点** | **SHA256校验码** | **软件包容量**|
| -----------------------------------------| ------------ | ------------ | ---------------- | ---------------- |
| ArkUI-X全量代码 | 1.0.0 Alpha    | [站点]()     | [SHA256校验码]() |            |
| ArkUI-X SDK包（Windows）  | 1.0.0 Alpha | [站点]()     | [SHA256校验码]() |               |
| ArkUI-X SDK包（macOS）    | 1.0.0 Alpha | [站点]()     | [SHA256校验码]() |               |
| ArkUI-X SDK包（Linux）    | 1.0.0 Alpha | [站点]()     | [SHA256校验码]() |               |

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
