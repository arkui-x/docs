# ArkUI-X 1.0.0 Canary1

## 版本概述

首次发布ArkUI-X 1.0.0 Canary1版本，主要能力范围包括：

- 应用开发范式：支持基于ArkTS的声明式开发范式。
- 应用开发模型：支持Stage开发模型。
- 开发者工具：提供DevEco Studio(IDE)和ACE Tools(命令行)两种ArkUI-X应用构建工具。
- 混合开发能力：ArkTS声明式开发范式和Stage模型支持集成在现有iOS/Android应用中，通过现有应用加载，解析和运行。
- 跨语言调用能力：提供[FFI(Node-API)](../application-dev/quick-start/ffi-napi-introduction.md)和[平台桥接](../application-dev/quick-start/platform-bridge-introduction.md)两种机制，用于API扩展和平台插件开发。
- 基础测试调试：支持单元/UI/XTS集成测试和ArkTS断点调试。

### 接口范围

ArkUI跨平台接口包含OpenHarmony接口和自定义扩展接口，OpenHarmony接口以Public接口为基础，接口范围为API10<sup>+</sup>，具体支持列表详见[API参考](../application-dev/reference/README.md)。

>说明：ArkUI-X 1.0.0 Canary1版本为ArkUI-X首次发布的预览版本，除提供[ArkUI控件](../application-dev/reference/arkui-ts/README.md)和部分[@ohos接口](../application-dev/reference/apis/README.md)之外，暂不提供其它OpenHarmony接口定义的跨平台实现。

### 应用开发工具

- ACE Tools，是一套为ArkUI-X开发者提供的命令行工具，包括开发环境检查，新建项目，编译打包，安装调试。[详情参见ACE Tools快速入门](../application-dev/quick-start/start-with-ace-tools.md)。
- DevEco Studio，是OpenHarmony和HarmonyOS默认的应用程序IDE开发工具，同时支持ArkUI-X应用创建和构建等功能。[详情参见DevEco Studio使用说明](../application-dev/tools/how-to-use-deveco-studio.md)。

## 配套关系

  **表1** 版本软件和平台配套关系

| 目标平台    | 项目编译使用OS SDK版本              | 备注 |
| ----------- | ----------------------------------- | ---- |
| OpenHarmony | 4.0 (API Version 10) |  Beta2  |
| Android     | Android 8<sup>+</sup> (API level 26<sup>+</sup>)       | NA   |
| iOS         | iOS 10<sup>+</sup>                             | NA   |

>说明：Canary1版本为面向特定开发者发布的早期预览版本，不承诺UI和API稳定性。

## 源码获取

### 前提条件

1. 注册gitcode帐号。

2. 注册GitCode SSH公钥，请参考[GitCode帮助中心](https://docs.gitcode.com/docs/users/ssh-key/)。

3. 安装[git客户端](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)和[git-lfs](https://docs.gitcode.com/docs/repo/code/lfs/)并配置用户信息。
  
   ```
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. 安装GitCode repo工具，可以执行如下命令。
  
   ```
   curl -s https://gitcode.com/gitcode-dev/repo/blob/main/repo-py3 > /usr/local/bin/repo  #如果没有权限，可下载至其他目录，并将其配置到环境变量中chmod a+x /usr/local/bin/repo
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```


### 通过repo获取

**方式一（推荐）**

通过repo + ssh 下载（需注册公钥，请参考[GitCode帮助中心](https://docs.gitcode.com/docs/users/ssh-key/)）。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```
   repo init -u git@gitcode.com:arkui-x/manifest.git -b ArkUI-X-1.0.0-Canary1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```
   repo init -u git@gitcode.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Canary1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https 下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```
   repo init -u https://gitcode.com/arkui-x/manifest.git -b ArkUI-X-1.0.0-Canary1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```
   repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Canary1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 1.0.0 Canary1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-x64-0.0.9.6-Canary1.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-x64-0.0.9.6-Canary1.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 1.0.0 Canary1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-arm64-0.0.9.6-Canary1.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-arm64-0.0.9.6-Canary1.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 1.0.0 Canary1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/windows/arkui-x-windows-x64-0.0.9.6-Canary1.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/windows/arkui-x-windows-x64-0.0.9.6-Canary1.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 1.0.0 Canary1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/linux/arkui-x-linux-x64-0.0.9.6-Canary1.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/linux/arkui-x-linux-x64-0.0.9.6-Canary1.zip.sha256) |

## Samples

**表3** Samples列表

| 项目名称      | 简介                                                         |
| ------------- | ------------------------------------------------------------ |
| HelloWorld | HellWorld应用工程示例，支持Android、iOS和OpenHarmony应用构建。 |
| Shopping | 仿购物应用工程示例，支持Android、iOS和OpenHarmony应用构建。   |
| HealthyDiet | 健康饮食应用工程示例，支持Android、iOS和OpenHarmony应用构建。|
| Native | NAPI应用工程示例，支持Android、iOS和OpenHarmony应用构建。|
| Library | 平台库应用工程示例，支持Android、iOS和OpenHarmony应用构建。|

请访问[Samples](https://gitcode.com/arkui-x/samples)仓了解更多消息。
