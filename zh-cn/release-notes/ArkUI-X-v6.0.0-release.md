# ArkUI-X 6.0.0 Release

## 版本概述

ArkUI-X 6.0.0 Release版本配套OpenHarmony 6.0.0 API 20 Release版本。框架跟随OpenHarmony同源演进，在ArkUI组件、图形引擎、语言运行时等底座能力进一步完善，ArkUI 高级组件、Web组件、网络、文件管理、数据库、graphic2D等平台基础API能力完善，支持跨平台使用。欢迎大家关注并使用ArkUI-X，如有疑问可以通过issues交流，期待您的宝贵建议！

## 特性说明

### 应用框架
* Android窗口支持TextureView，页面支持设置圆角场景；
* 无障碍大字体；
* SymbolGlyph；

### 组件适配
* SwipeRefresher
* ComposeListItem
* ComposeTitleBar
* SelectTitleBar
* SelectionMenu
* SplitLayout
* Filter
* EditableTitleBar
* TabTitleBar
* ProgressButton
* Counter
* Chip
* TreeView
* ChipGroup
* GridObjectSortComponent
* FoldSplitContainer
* ExceptionPrompt
详情参见：[组件跨平台列表](../application-dev/reference/arkui-ts/README.md)。

### API适配

主要新增以下接口跨平台适配：
* ohos.arkui.theme
* ohos.arkui.StateManagement
* ohos.arkui.Prefetcher
* ohos.arkui.modifier
* ohos.arkui.observer
* ohos.arkui.UIContext OverlayManager
* ohos.file.fs;
* ohos.data.relationalStore;
* ohos.data.preferences;
* ohos.graphic.drawing;
* ohos.graphic.common2D;
* ohos.hiTraceMeter;
* ohos.bundle.bundleManager
* ohos.net.http requestInStream;
* ohos.i18 getAppPreferredLanguage、SetAppPreferredLanguage;
  详情参见：[ArkTS接口跨平台列表](../application-dev/reference/apis/README.md)。

### 社区
* 新增[FAQ专区](../application-dev/tutorial/faq/README.md)，针对开发者应用跨平台开发过程中问题，进行汇总和总结，提供解决方案。
* samples仓新增[应用开发案例集](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/Cases)、[ArkUI组件集合](https://gitcode.com/arkui-x/samples/tree/master/CodeLab/ArkTSComponentCollection)、[Rust示例应用](https://gitcode.com/arkui-x/samples/tree/master/RustFeature/Rust)、[平行视界应用](https://gitcode.com/arkui-x/samples/tree/master/SuperFeature/SplitScreenView)，为应用开发提供开箱即用案例集合。


## 配套关系

**表1** 版本软件和平台配套关系

| 目标平台     | 兼容OS版本                                        | 获取方式                                                     |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------ |
| OpenHarmony | 6.0.0 Release (API Version 20)               | HUAWEI DevEco Studio 上动态下载，获取方式：<br />[请点击这里获取HUAWEI DevEco Studio](https://developer.huawei.com/consumer/cn/download/deveco-studio)                  |
| HarmonyOS   | 6.0.0 Release (API Version 20)               | HUAWEI DevEco Studio 内置集成，获取方式：<br />[请点击这里获取HUAWEI DevEco Studio](https://developer.huawei.com/consumer/cn/download/deveco-studio) |
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

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 6.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/darwin/arkui-x-darwin-x64-6.0.0.103-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/darwin/arkui-x-darwin-x64-6.0.0.103-Release.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 6.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/darwin/arkui-x-darwin-arm64-6.0.0.103-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/darwin/arkui-x-darwin-arm64-6.0.0.103-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 6.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/windows/arkui-x-windows-x64-6.0.0.103-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/windows/arkui-x-windows-x64-6.0.0.103-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 6.0.0 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/linux/arkui-x-linux-x64-6.0.0.103-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.0.0.103/linux/arkui-x-linux-x64-6.0.0.103-Release.zip.sha256) |