# ArkUI-X 1.1.6 Release

## 版本概述

ArkUI-X 1.1.6正式版配套OpenHarmony API 11 Release，新增适配部分API 11接口支持跨平台；组件跨平台能力进一步增强，新增按键、触摸和焦点事件适配，完善web组件跨平台能力，优化video组件跨平台效果。动态化能力增强，支持应用指定时机加载引擎库，沙箱目录新增支持加载systemres、hsp和业务动态库；API拓展机制能力提升，新增API插件生命周期管理，方便应用自己开发、加载API插件。框架Bridge通信能力进一步完善，新增支持传递二进制格式参数，支持bridge通信并发及callback回调能力。欢迎大家关注并使用ArkUI-X，如有疑问可以通过issues交流，期待您的宝贵建议！


## 特性说明

### 应用框架

* 完善拓展机制，新增API插件生命周期管理机制，方便应用开发、加载自己拓展的API插件，参见[ArkUI-X插件开发指南（Android）](../application-dev/tutorial/how-to-use-arkui-x-plugin-on-android.md)、[ArkUI-X插件开发指南(iOS)](../application-dev/tutorial/how-to-use-arkui-x-plugin-on-ios.md)


### 动态化

* 支持在应用沙箱目录加载systemres、hsp及业务动态库；
* 支持应用指定时机加载引擎库；


### Bridge通信

* 支持跨语言通信使用二进制格式编解码参数;
* Bridge通信支持传递callback参数，参见[平台桥接开发指南（Android）](../application-dev/tutorial/how-to-use-bridge-on-android.md)、[平台桥接开发指南（iOS）](../application-dev/tutorial/how-to-use-bridge-on-ios.md)；
* Bridge通信在原生侧支持设置并发模式，如果不传表示主线程，设置TaskOption(false)表示在原生侧启用单线程有序并发模式，设置TaskOption(true)表示在原生侧启用多线程无序并发模式，参见[BridgePlugin (Android)](../application-dev/reference/arkui-for-android/BridgePlugin.md)、[BridgePlugin(iOS)](../application-dev/reference/arkui-for-ios/BridgePlugin.md);


### 组件适配

* 优化Video组件跨平台渲染方案，底层使用同层渲染提升性能；
* 完善Web组件跨平台能力，支持WebviewController、WebDatabase、cookieManager等接口；
* 组件事件跨平台适配，包括按键、触摸、焦点事件，
详情参见：[组件跨平台列表](../application-dev/reference/arkui-ts/README.md)。

### API适配

主要新增以下接口跨平台适配：
* ohos.wifiManager；
* ohos.security.cert；
* ohos.request；
* ohos.multimedia.image；
* ohos.window；
详情参见：[ArkTS接口跨平台列表](../application-dev/reference/apis/README.md)。


## 配套关系

**表1** 版本软件和平台配套关系

| 目标平台    | 兼容OS版本                                       | 获取方式                                                         |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------ |
| OpenHarmony | 4.1 Release (API Version 11)                     | HUAWEI DevEco Studio获取方式：<br />[Windows(64-bit)](https://contentcenter-vali-drcn.dbankcdn.cn/pvt_2/DeveloperAlliance_package_901_9/ee/v3/HqJ-6O2FQny86xtk_dg9HQ/devecostudio-windows-4.1.0.400.zip?HW-CC-KV=V1&HW-CC-Date=20240409T033730Z&HW-CC-Expire=315360000&HW-CC-Sign=BFA444BC43A041331E695AE2CFA9035A957AF107E06C97E793FD3D31D7096A0D)  <br />SHA256校验码：c46be4f3cfde27af1806cfc9860d9c366e66a20e31e15180cf3a90ab05464650<br />[Mac(X86)](https://contentcenter-vali-drcn.dbankcdn.cn/pvt_2/DeveloperAlliance_package_901_9/3b/v3/JgGp8n0bShOkm1MpBFJ73w/devecostudio-mac-4.1.0.400.zip?HW-CC-KV=V1&HW-CC-Date=20240409T034037Z&HW-CC-Expire=315360000&HW-CC-Sign=35C1F8B3FC19325EBBC32D8E11106DDB074A8ECC6BB3A77FF2EADBA2A8A223DA)  <br />SHA256校验码：15d6136959b715e4bb2160c41d405b889820ea26ceadbb416509a43e59ed7f09<br />[Mac(ARM)](https://contentcenter-vali-drcn.dbankcdn.cn/pvt_2/DeveloperAlliance_package_901_9/21/v3/D7Jy1StbTwSLUXaA20VrAw/devecostudio-mac-arm-4.1.0.400.zip?HW-CC-KV=V1&HW-CC-Date=20240409T034235Z&HW-CC-Expire=315360000&HW-CC-Sign=19598AAC650D2AB24CAC6DFDF0DBD312188FB0438A8233B7687E6ACDC43A51F8)  <br />SHA256校验码：ac04ca7c2344ec8f27531d5a59261ff037deed2c5a3d42ef88e6f90f4ed45484 |
| HarmonyOS   | 4.1.0 Release (API Version 11)                   | 当前仅面向HarmonyOS Next合作企业开发者                       |
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
   repo init -u git@gitcode.com:arkui-x/manifest.git -b ArkUI-X-1.1.6-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u git@gitcode.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.1.6-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b ArkUI-X-1.1.6-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.1.6-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 1.1.6 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/darwin/arkui-x-darwin-x64-1.1.6.12-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/darwin/arkui-x-darwin-x64-1.1.6.12-Release.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 1.1.6 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/darwin/arkui-x-darwin-arm64-1.1.6.12-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/darwin/arkui-x-darwin-arm64-1.1.6.12-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 1.1.6 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/windows/arkui-x-windows-x64-1.1.6.12-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/windows/arkui-x-windows-x64-1.1.6.12-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 1.1.6 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/linux/arkui-x-linux-x64-1.1.6.12-Release.zip)     | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/1.1.6.12/linux/arkui-x-linux-x64-1.1.6.12-Release.zip.sha256) |

## Samples

**表3** Samples新增列表

| 项目名称      | 简介                                                         |
| ------------- | ------------------------------------------------------------ |
| Animation | 动画samples跨平台应用示例 |
| Files | 读写文件跨平台应用示例   |
| InfiniteList | 长列表跨平台示例|
| JsonExample | 反序列化JSON跨平台应用示例|
| News | 新闻跨平台应用示例|
| PlatformBridge | 平台桥接TS和原生通信示例|
| RichText | 富文本跨平台应用示例|
| Router | 路由跳转跨平台应用示例|
| WebExample | Web组件跨平台应用示例|
| FauxNativeAlbum | 混合开发图库跨平台应用示例|

请访问[Samples](https://gitcode.com/arkui-x/samples)仓了解更多消息。

## 修复缺陷列表

**表4** 修复缺陷ISSUE列表

| ISSUE单 | 问题描述 |
| -------- | -------- |
| I8SZSR | 如何在跨平台场景使用web组件|
| I8W63Y | PersistentStorage保存数据在Android平台重启后丢失问题，重复ISSUE有：I91KEY |
| I8XHFG | touch事件无法识别到多个触摸点 |
| I8ZADY | 'Bridge' can't support crossplatform application |
| I8ZX4O | iOS应用使用ArkUI-X上架时提示使用私有API问题 |
| I91FF7 | Image组件设置fillColor在安卓上无效 |
| I93AK8 | iOS应用使用ArkUI-X侧滑返回失效问题 |
| I9410O | ArkUI-X转场动画/模态转场存在编译警告 |
| I9599Z | iOS端软键盘无法回收问题 |
