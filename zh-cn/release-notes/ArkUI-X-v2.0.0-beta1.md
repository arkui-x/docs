# ArkUI-X 2.0.0 Beta1

## 版本概述

ArkUI-X 2.0.0 Beta1版配套OpenHarmony API 12 Beta1，新增适配部分API 12接口支持跨平台；框架能力进一步完善，支持Android应用非压缩模式，支持Android Fragment对接跨平台。ACE Tools工具易用性提升，支持创建module时选择module类型、config提示优化和联动编译。组件跨平台能力进一步增强，新增XComponent组件支持跨平台。欢迎大家关注并使用ArkUI-X，如有疑问可以通过issues交流，期待您的宝贵建议！


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
* ohos.measure；
* ohos.events.emitter；
*  EventHub；
* window.setWindowLayoutFullScreen、window.setWindowSystemBarEnable、 window.getWindowAvoidArea；
* ohos.arkui.UIContext
* ohos.promptAction
* ohos.notificationManager
* ohos.file.picker
* ohos.security.cryptoFramework
* ohos.font
* ohos.arkui.drawableDescriptor
* ohos.measure
  详情参见：[ArkTS接口跨平台列表](../application-dev/reference/apis/README.md)。


## 配套关系

**表1** 版本软件和平台配套关系

| 目标平台    | 兼容OS版本                                       | 获取方式                               |
| ----------- | ------------------------------------------------ | -------------------------------------- |
| OpenHarmony | 5.0.0 Beta1 (API Version 12)                     | 后续提供                               |
| HarmonyOS   | 5.0.0 Beta1(API Version 12)                      | 当前仅面向HarmonyOS Next合作企业开发者 |
| Android     | Android 8<sup>+</sup> (API level 26<sup>+</sup>) | NA                                     |
| iOS         | iOS 10<sup>+</sup>                               | NA                                     |



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
   repo init -u git@gitee.com:arkui-x/manifest.git -b ArkUI-X-5.0-Beta1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u git@gitee.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v5.0-Beta1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u https://gitee.com/arkui-x/manifest.git -b ArkUI-X-5.0-Beta1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u https://gitee.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v5.0-Beta1 --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 2.0.0 Beta1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/darwin/arkui-x-darwin-x64-2.0.0.27-Beta1.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/darwin/arkui-x-darwin-x64-2.0.0.27-Beta1.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 2.0.0 Beta1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/darwin/arkui-x-darwin-arm64-2.0.0.27-Beta1.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/darwin/arkui-x-darwin-arm64-2.0.0.27-Beta1.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 2.0.0 Beta1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/windows/arkui-x-windows-x64-2.0.0.27-Beta1.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/windows/arkui-x-windows-x64-2.0.0.27-Beta1.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 2.0.0 Beta1 | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/linux/arkui-x-linux-x64-2.0.0.27-Beta1.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/2.0.0.27/linux/arkui-x-linux-x64-2.0.0.27-Beta1.zip.sha256) |

## Samples

**表3** Samples新增列表

| 项目名称      | 简介                                                         |
| ------------- | ------------------------------------------------------------ |
| PlatformNAPI | C++跨平台应用示例 |
| XcomponentNative | XComponent组件跨平台示例 |

请访问[Samples](https://gitee.com/arkui-x/samples)仓了解更多消息。

## 修复缺陷列表

**表4** 修复缺陷ISSUE列表

| ISSUE单 | 问题描述 |
| -------- | -------- |
| I94ZJT | 无法支持全屏，沉浸式状态栏 |
| I8RMYW | 适配获取Android、iOS状态栏高度 |
| I8ZIH | 键盘回收之后回弹影响体验 |
| I9593W | 日期组件iOS端显示不正常 |
| I9VUS0 | Android应用退到后台bridge通信失败 |

### 遗留缺陷列表

**表5** 遗留缺陷列表

| ISSUE单 | 问题描述                                   | 影响                                                         | 计划解决日期 |
| ------- | ------------------------------------------ | ------------------------------------------------------------ | ------------ |
| IA728D  | 界面中有Web对象bindMenu弹出菜单无法交互    | 使用web组件跨平台弹出菜单不可用。<br />                      | 2024.12.30   |
| IA728I  | iOS无法通过setPreferredOrientation强制横屏 | 该API无法横屏。<br />规避措施：使用bridge桥接机制调用原生接口横屏。 | 2024.9.30    |
