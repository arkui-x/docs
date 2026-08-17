# ArkUI-X 6.1.1 Release

## 版本概述

ArkUI-X 6.1.1 Release版本配套OpenHarmony 6.1.1 API 24 Release版本。框架跟随OpenHarmony同源演进，在ArkUI组件、图形引擎、语言运行时等底座能力进一步完善，平台视图、AceTools等特性能力进一步增强，XComponent组件、Image组件、字体引擎、剪贴板、数据库、网络通信等平台基础API能力完善，支持跨平台使用。欢迎大家关注并使用ArkUI-X，如有疑问可以通过issues交流，期待您的宝贵建议！

## 特性说明

### 应用框架
* ArkUI支持UI和逻辑解耦，支持不带UI的逻辑跨平台，支撑应用的ArkTS逻辑跨平台

### 特性能力
* 平台视图组件适配SurfaceView模型
* AceTools创建跨平台工程时，支持动态选择平台（Android、iOS、Android+iOS）

### 组件适配
* Hyperlink
* Image组件支持DrawableDescriptor输入
* XComponent组件支持模式Texture模式
详情参见：[组件跨平台列表](https://gitcode.com/arkui-x/docs/blob/ArkUI-X-6.1-Release/zh-cn/application-dev/reference/arkui-ts/README.md)。

### API适配

主要新增以下接口跨平台适配：
* i18n（TimeZone、BreakIterator等）
* ohos.graphics.text
* ohos.net.socket/ohos.net.webSocket
* ohos.data.distributedKVStore
* ohos.pasteboard
  详情参见：[ArkTS接口跨平台列表](https://gitcode.com/arkui-x/docs/blob/ArkUI-X-6.1-Release/zh-cn/application-dev/reference/apis/README.md)。

### 社区
* samples仓新增[UI和逻辑解耦示例应用](https://gitcode.com/arkui-x/samples/tree/ArkUI-X-6.1-Release/SuperFeature/DecoupledUIAndLogic), 为应用开发提供开箱即用案例集合。


## 配套关系

**表1** 版本软件和平台配套关系

| 目标平台     | 兼容OS版本                                        | 获取方式                                                     |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------ |
| OpenHarmony | 6.1.1 Release (API Version 24)               | HUAWEI DevEco Studio 上动态下载，获取方式：<br />[请点击这里获取HUAWEI DevEco Studio](https://developer.huawei.com/consumer/cn/download/deveco-studio)                  |
| HarmonyOS   | 6.1.1 Release (API Version 24)               | HUAWEI DevEco Studio 内置集成，获取方式：<br />[请点击这里获取HUAWEI DevEco Studio](https://developer.huawei.com/consumer/cn/download/deveco-studio) |
| Android     | Android 8<sup>+</sup> (API level 26<sup>+</sup>) | NA                                                           |
| iOS         | iOS 10<sup>+</sup>                               | NA                                                           |



## 源码获取

### 前提条件

1. 注册GitCode帐号。

2. 注册GitCode SSH公钥，请参考[GitCode帮助中心](https://docs.gitcode.com/docs/help/home/user_center/security_management/ssh)。

3. 安装[git客户端](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)和[git-lfs](https://docs.gitcode.com/docs/help/home/org_project/project_manage/file_operations/lfs/)并配置用户信息。
  
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

通过repo + ssh下载（需注册公钥，请参考[GitCode帮助中心](https://docs.gitcode.com/docs/help/home/user_center/security_management/ssh)）。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u git@gitcode.com:arkui-x/manifest.git -b ArkUI-X-6.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u git@gitcode.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-6.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

**方式二**

通过repo + https下载。

- 从版本分支获取源码。可获取该版本分支的最新源码，包括版本发布后在该分支的合入。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b ArkUI-X-6.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```
   
- 从版本发布Tag节点获取源码。可获取与版本发布时完全一致的源码。
   ```shell
   repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-6.1-Release --no-repo-verify
   repo sync -c
   repo forall -c 'git lfs pull'
   ```

## SDK获取

**表2** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 6.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/darwin/arkui-x-darwin-x64-6.1.1.111-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/darwin/arkui-x-darwin-x64-6.1.1.111-Release.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 6.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/darwin/arkui-x-darwin-arm64-6.1.1.111-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/darwin/arkui-x-darwin-arm64-6.1.1.111-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 6.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/windows/arkui-x-windows-x64-6.1.1.111-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/windows/arkui-x-windows-x64-6.1.1.111-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 6.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/linux/arkui-x-linux-x64-6.1.1.111-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/6.1.1.111/linux/arkui-x-linux-x64-6.1.1.111-Release.zip.sha256) |