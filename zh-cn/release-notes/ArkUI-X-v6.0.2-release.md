# ArkUI-X 6.0.2 Release

## 版本概述

当前版本在ArkUI-X 6.0.0版本基础上，完善平台桥接的能力、新增ohos.accessibility、ohos.vibrator、ohos.geoLocationManager API支持跨平台使用、修复了部分功能和系统稳定性的问题，增强了系统稳定性。欢迎大家更新使用，如有疑问可以通过在社区提交issues交流，期待您的宝贵建议！

## 特性说明

### 应用框架

* 平台桥接Bridge实例和Ability实例解耦，支持应用内全局调用、Bridge子线程运行、同时新增同步接口调用能力。

### API适配

主要新增以下接口跨平台适配：

* ohos.accessibility
* ohos.vibrator
* ohos.geoLocationManager
* 详情参见：[ArkTS接口跨平台列表](../application-dev/reference/apis/README.md)。

## 配套关系

**表1** 版本软件和平台配套关系

| 目标平台    | 兼容OS版本                                       | 获取方式                                                                                                                                            |
| ----------- | ------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------- |
| OpenHarmony | 6.0.2 Release (API Version 22)                   | HUAWEI DevEco Studio 上动态下载，获取方式：[请点击这里获取HUAWEI DevEco Studio](https://developer.huawei.com/consumer/cn/download/deveco-studio) |
| HarmonyOS   | 6.0.2 Release (API Version 22)                   | HUAWEI DevEco Studio 内置集成，获取方式：[请点击这里获取HUAWEI DevEco Studio](https://developer.huawei.com/consumer/cn/download/deveco-studio)   |
| Android     | Android 8+ (API level 26+) | NA                                                                                                                                                  |
| iOS         | iOS 10+                               | NA                                                                                                                                                  |

## 源码获取

### 前提条件

1. 注册GitCode帐号。
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

通过repo + ssh下载（需注册公钥，请参考[GitCode帮助中心](https://docs.gitcode.com/docs/users/ssh-key/)）。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
  
  ```shell
  repo init -u git@gitcode.com:arkui-x/manifest.git -b ArkUI-X-6.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
  
  ```shell
  repo init -u git@gitcode.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v6.0.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```

**方式二**

通过repo + https下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
  
  ```shell
  repo init -u https://gitcode.com/arkui-x/manifest.git -b ArkUI-X-6.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
  
  ```shell
  repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v6.0.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                   | **版本信息** | **下载站点**                                                                                                    | **SHA256校验码**                                                                                                               |
| ------------------------- | ------------------ | --------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| ArkUI-X SDK包（macOS）    | 6.0.2 Release      | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/darwin/arkui-x-darwin-x64-6.0.2.118-Release.zip)   | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/darwin/arkui-x-darwin-x646.0.2.118-Release.zip.sha256)   |
| ArkUI-X SDK包（macOS-M1） | 6.0.2 Release      | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/darwin/arkui-x-darwin-arm64-6.0.2.118-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/darwin/arkui-x-darwin-arm64-6.0.2.118-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）  | 6.0.2 Release      | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/windows/arkui-x-windows-x64-6.0.2.118-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/windows/arkui-x-windows-x64-6.0.2.118-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 6.0.2 Release      | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/linux/arkui-x-linux-x64-6.0.2.118-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.2.118/linux/arkui-x-linux-x64-6.0.2.118-Release.zip.sha256)     |

