# ArkUI-X 5.0.1 Release

## 版本概述

ArkUI-X 5.0.1 Release版配套OpenHarmony 5.0.1 Rlease，API 13，新增适配部分API 13接口支持跨平台；框架能力进一步完善，支持Android应用非压缩模式，支持Android Fragment对接跨平台。ACE Tools工具易用性提升，支持创建module时选择module类型、config提示优化和联动编译。组件跨平台能力进一步增强，新增XComponent组件支持跨平台。欢迎大家关注并使用ArkUI-X，如有疑问可以通过issues交流，期待您的宝贵建议！


## 特性说明

### 应用框架

* 新增支持Android平台Fragment对接跨平台，参见[ Fragment跨平台开发指南](../application-dev/tutorial/how-to-use-fragment-on-android.md)；
* 支持Android应用非压缩模式（useLegacyPacking等于false 或 android:extractNativeLibs等于false场景）；
* 支持Activity和ViewController销毁时，框架自动对API插件进行内存回收；
* 新增支持设置沉浸式及获取状态栏等避让区域信息；

### ACE Tools

* 支持创建module时选择module类型；
* 支持多hap/hsp同时安装到OpenHarmony终端设备；
* 支持设置ArkUI-X框架源码目录，配置后自动关联源码编译产物（--source-dir）；
* 支持联动编译，方便开发者在Android、iOS工程中触发ArkTS编译，参见[联动编译开发指南（Android）](../application-dev/tutorial/how-to-linkage-compilation-on-android.md)、[联动编译开发指南（iOS）](../application-dev/tutorial/how-to-linkage-compilation-on-ios.md)；


### 组件适配

* 支持XComponent组件跨平台适配，参考[XComponent](../application-dev/reference/arkui-ts/ts-basic-components-xcomponent.md)；
* Dialog、Toast、contextMenu、Popup适配子窗口；

   详情参见：[组件跨平台列表](../application-dev/reference/arkui-ts/README.md)。

### API适配

主要新增以下接口跨平台适配：
* ohos.events.emitter;
* EventHub;
* window.setWindowLayoutFullScreen、window.setWindowSystemBarEnable、 window.getWindowAvoidArea;
* ohos.promptAction;
* ohos.security.cryptoFramework;
* ohos.measure;
* ohos.wifiManager;
* ohos.file.picker;
* ohos.file.photoAccessHelper;
* ohos.multimedia.audio;
* ohos.multimedia.media;
* ohos.notificationManager;

以下接口API 12 新增接口跨平台适配：
* ohos.net.socket;
* ohos.net.webSocket;
* ohos.resourceManager;
* ohos.multimedia.image;
* ohos.data.relationalStore;
* ohos.taskpool;
* ohos.file.fs;
* ohos.font;

  详情参见：[ArkTS接口跨平台列表](../application-dev/reference/apis/README.md)。


## 配套关系

**表1** 版本软件和平台配套关系

| 目标平台     | 兼容OS版本                                        | 获取方式                                                     |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------ |
| OpenHarmony | 5.0.1 Release (API Version 13)                   | 后续提供                                                     |
| HarmonyOS   | 5.0.1 Release (API Version 13)                   | HUAWEI DevEco Studio获取方式：<br />[请点击这里获取](https://developer.huawei.com/consumer/cn/download/) |
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
   repo init -u git@gitcode.com:arkui-x/manifest.git -b ArkUI-X-5.0.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u git@gitcode.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v5.0.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b ArkUI-X-5.0.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v5.0.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 5.0.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/darwin/arkui-x-darwin-x64-5.0.1.110-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/darwin/arkui-x-darwin-x64-5.0.1.110-Release.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 5.0.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/darwin/arkui-x-darwin-arm64-5.0.1.110-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/darwin/arkui-x-darwin-arm64-5.0.1.110-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 5.0.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/windows/arkui-x-windows-x64-5.0.1.110-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/windows/arkui-x-windows-x64-5.0.1.110-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 5.0.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/linux/arkui-x-linux-x64-5.0.1.110-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.0.1.110/linux/arkui-x-linux-x64-5.0.1.110-Release.zip.sha256) |