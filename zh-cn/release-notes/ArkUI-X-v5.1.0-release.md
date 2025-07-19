# ArkUI-X 5.1.0 Release

## 版本概述

ArkUI-X 5.1.0 Release版配套OpenHarmony 5.1.0 Rlease，API 18，新增适配部分API 18接口支持跨平台；框架能力持续完善，新增平台视图、无障碍屏幕朗读、动态路由等功能，Web组件功能、动态化能力进一步增强。欢迎大家关注并使用ArkUI-X，如有疑问可以通过issues交流，期待您的宝贵建议！


## 特性说明

### 应用框架

* 新增平台视图功能，支持ArkUI视图中嵌套Android、iOS平台的视图，参见[ 平台视图开发指南 ](../application-dev/tutorial/how-to-use-platformview-on-android.md)；
* 支持无障碍屏幕朗读；
* 动态化能力进一步增强，同时支持预置和动态化，让特性和宿主应用发布解耦；
* 支持日志拦截能力，捕获ArkUI-X框架以及ArkTS业务日志。

### 组件适配

* 组件 ohos.arkui.advanced.Dialog、ohos.arkui.advanced.TooBar、ohos.arkui.advanced.SegmentButton、ohos.arkui.advanced.SubHeader适配跨平台；

   详情参见：[组件跨平台列表](../application-dev/reference/arkui-ts/README.md)。

### API适配

主要新增以下接口跨平台适配：
* arkts.collections (ArkTS容器集)
* arkts.lang (ArkTS语言基础能力)
* arkts.math.Decimal (高精度数学库Decimal)
* arkts.utils (ArkTS工具库)
* ohos.systemDateTime(系统时间、时区)
* ohos.hiviewdfx.hiAppEvent (应用事件打点);
* ohos.arkui.dragController (DragController)
* ohos.web.webview 新增常用属性、时间、地理位置等API适配;

  详情参见：[ArkTS接口跨平台列表](../application-dev/reference/apis/README.md)。


## 配套关系

**表1** 版本软件和平台配套关系

| 目标平台     | 兼容OS版本                                        | 获取方式                                                     |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------ |
| OpenHarmony | 5.1.0 Release (API Version 18)                   | 后续提供                                                     |
| HarmonyOS   | 5.1.0 Release (API Version 18)                   | HUAWEI DevEco Studio获取方式：<br />[请点击这里获取](https://developer.huawei.com/consumer/cn/download/) |
| Android     | Android 8<sup>+</sup> (API level 26<sup>+</sup>) | NA                                                           |
| iOS         | iOS 10<sup>+</sup>                               | NA                                                           |



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
   repo init -u git@gitcode.com:arkui-x/manifest.git -b ArkUI-X-5.1.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u git@gitcode.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v5.1.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b ArkUI-X-5.1.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v5.1.0-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 5.1.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/darwin/arkui-x-darwin-x64-5.1.0.104-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/darwin/arkui-x-darwin-x64-5.1.0.104-Release.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 5.1.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/darwin/arkui-x-darwin-arm64-5.1.0.104-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/darwin/arkui-x-darwin-arm64-5.1.0.104-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 5.1.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/windows/arkui-x-windows-x64-5.1.0.104-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/windows/arkui-x-windows-x64-5.1.0.104-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 5.1.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/linux/arkui-x-linux-x64-5.1.0.104-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.0.104/linux/arkui-x-linux-x64-5.1.0.104-Release.zip.sha256) |