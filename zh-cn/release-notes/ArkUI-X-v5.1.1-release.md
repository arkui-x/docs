# ArkUI-X 5.1.1 Release

## 版本概述

当前版本在ArkUI-X 5.1.0版本基础上，修复了iOS平台深色模式、无障碍朗读、滑动手势、模糊效果等已知问题。欢迎大家关注并使用ArkUI-X，如有疑问可以通过issues交流，期待您的宝贵建议！

## 修复缺陷

**表1** 修复缺陷列表

| 序号 | 问题描述 |
| ---- | -------- |
| 1 | 修复了iOS深色模式异常的问题 |
| 2 | 修复了SegmentButton无边框的渲染异常问题 |
| 3 | 修复了iOS无障碍部分场景朗读异常的问题 |
| 4 | 修复了iOS上Scroll内部嵌套List时，低概率触发滑动变拖拽的问题 |
| 5 | 修复了iOS上backgroundBlurStyleOptions设置CustomDialogController，弹框模糊效果不生效的问题 |


## 配套关系

**表2** 版本软件和平台配套关系

| 目标平台     | 兼容OS版本                                        | 获取方式                                                     |
| ----------- | ------------------------------------------------ | ------------------------------------------------------------ |
| OpenHarmony | 5.1.0 Release (API Version 18)                   | HUAWEI DevEco Studio支持下载                                                     |
| HarmonyOS   | 5.1.0 Release (API Version 18)                   | HUAWEI DevEco Studio获取方式：<br />[请点击这里获取](https://developer.huawei.com/consumer/cn/download/) |
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

**表3** 获取SDK路径列表

| SDK版本                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| -----------------------------------------| ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（macOS）  | 5.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/darwin/arkui-x-darwin-x64-5.1.1.112-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/darwin/arkui-x-darwin-x64-5.1.1.112-Release.zip.sha256) |
| ArkUI-X SDK包（macOS-M1）    | 5.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/darwin/arkui-x-darwin-arm64-5.1.1.112-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/darwin/arkui-x-darwin-arm64-5.1.1.112-Release.zip.sha256) |
| ArkUI-X SDK包（Windows）    | 5.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/windows/arkui-x-windows-x64-5.1.1.112-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/windows/arkui-x-windows-x64-5.1.1.112-Release.zip.sha256) |
| ArkUI-X SDK包（Linux）    | 5.1.1 Release | [站点](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/linux/arkui-x-linux-x64-5.1.1.112-Release.zip) | [SHA256校验码](https://repo.huaweicloud.com/arkui-crossplatform/sdk/5.1.1.112/linux/arkui-x-linux-x64-5.1.1.112-Release.zip.sha256) |
