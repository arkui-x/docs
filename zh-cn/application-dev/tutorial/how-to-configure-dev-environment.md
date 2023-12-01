# ACE Tools环境配置指导

## 简介

ACE Tools是一套为ArkUI-X应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等，以下是详细的环境配置指导。

## 环境准备

**前置条件：** Ubuntu需要18.04以上版本，macOS需要11.6.2及以上版本，Windows需要Windows 10版本。

**1. 配置Node.js环境**

   运行ACE Tools需Node.js环境支持，建议下载18.x版本。可命令行运行 `node -v` 查看本地Node.js版本，如不存在或版本不符合要求，请自行下载安装稳定版本：[Node.js下载地址](https://nodejs.org/en/download/)。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export NODE_HOME=/path-to-nodejs-sdk
   export PATH=${NODE_HOME}/bin:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export NODE_HOME=/path-to-nodejs-sdk
   export PATH=$NODE_HOME/bin:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set NODE_HOME=/path-to-nodejs-sdk
   ```

**2. 配置ohpm环境**

   OHPM CLI（OpenHarmony Package Manager Command-line Interface）OpenHarmony生态三方库的包管理工具，位于OpenHarmony [SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli_openharmony)中。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export OHPM_HOME=/path-to-ohpm-sdk
   export PATH=${OHPM_HOME}/bin:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export OHPM_HOME=/path-to-ohpm-sdk
   export PATH=$OHPM_HOME/bin:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set OHPM_HOME=/path-to-ohpm-sdk
   set PATH=%PATH%;%OHPM_HOME%/bin
   ```

   环境变量配置完成后，执行ohpm初始化。
   ```shell
   cd command-line-tools/ohpm    //按实进入package.json所在目录
   ./bin/init                    //执行init命令
   ```

**3. 配置Java环境**

   Android和OpenHarmony/HarmonyOS应用打包需Java环境支持，建议下载JDK11.0.2以上版本，下载请点击[此处](https://repo.huaweicloud.com/openjdk/)。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export JAVA_HOME=/path-to-java-sdk
   export PATH=${JAVA_HOME}/bin:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export JAVA_HOME=/path-to-java-sdk
   export PATH=$JAVA_HOME/bin:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set JAVA_HOME=/path-to-java-sdk
   set PATH=%PATH%;%JAVA_HOME%/bin
   ```

**4. 配置OpenHarmony SDK环境**

   **SDK下载：** 通过[OpenHarmony SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli_openharmony)下载OpenHarmony SDK，命令行工具使用说明详见[ohsdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ohos-sdk-command-line-tool-0000001263280431)官方指导。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export OpenHarmony_HOME=/path-to-openharmony-sdk
   export PATH=${OpenHarmony_HOME}/versioncode/toolchains:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export OpenHarmony_HOME=/path-to-openharmony-sdk
   export PATH=$OpenHarmony_HOME/versioncode/toolchains:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set OpenHarmony_HOME=/path-to-openharmony-sdk
   set PATH=%PATH%;%OpenHarmony_HOME%/versioncode/toolchains
   ```

   **说明：** 如果您使用DevEco Studio，则无需使用此命令行工具，可直接通过[IDE管理SDK软件包](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427)。

**5. 配置HarmonyOS SDK环境**

   **SDK下载：** 通过[HarmonyOS SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli)下载HarmonyOS SDK，命令行工具使用说明详见[sdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ide-command-line-sdkmgr-0000001110390078)官方指导。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export HarmonyOS_HOME=/path-to-harmonyos-sdk
   export PATH=${HarmonyOS_HOME}/hmscore/versioncode/toolchains:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export HarmonyOS_HOME=/path-to-harmonyos-sdk
   export PATH=$HarmonyOS_HOME/hmscore/versioncode/toolchains:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set HarmonyOS_HOME=/path-to-harmonyos-sdk
   set PATH=%PATH%;%HarmonyOS_HOME%/hmscore/versioncode/toolchains
   ```

   **说明：** 如果您使用DevEco Studio，则无需使用此命令行工具，可直接通过[IDE管理SDK软件包](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427)。


**6. 配置Android SDK环境**

   **SDK下载：** 通过[Android SDK命令行工具](https://developer.android.google.cn/studio#command-line-tools-only)下载Android SDK，命令行工具使用说明详见[sdkmanager](https://developer.android.google.cn/studio/command-line/sdkmanager)官方指导。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export ANDROID_HOME=/path-to-android-sdk
   export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export ANDROID_HOME=/path-to-android-sdk
   export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/build-tools/28.0.3:$ANDROID_HOME/platform-tools:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set ANDROID_HOME=/path-to-android-sdk
   set PATH=%PATH%;%ANDROID_HOME%/tools;%ANDROID_HOME%/tools/bin;%ANDROID_HOME%/build-tools/28.0.3;%ANDROID_HOME%/platform-tools
   ```

   **说明：** 如果您使用Android Studio，则无需使用此命令行工具，可直接通过[IDE管理SDK软件包](https://developer.android.google.cn/studio/intro/update#sdk-manager)。


**7. iOS应用开发环境**

   7.1 Xcode和Command Line Tools for Xcode应用可前往Mac App Store应用商店下载安装。Command Line Tools也可使用命令方式安装:

   ```shell
   xcode-select --install
   ```

   7.2 libimobiledevice，详细信息[参照](https://libimobiledevice.org)。

   ```shell
   brew install libimobiledevice
   ```

   7.3 ios-deploy安装，详细信息[参照](https://github.com/ios-control/ios-deploy)。

   ```shell
   brew install ios-deploy
   ```

**8. 配置ArkUI-X SDK环境**

   ArkUI-SDK获取和配置目录要求，请参考[ArkUI-X SDK介绍](../tools/how-to-use-arkui-x-sdk.md)。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   ```

## 命令安装

### 安装ace命令
   [Linux]

   ```shell
   // 配置环境变量
   export PATH=/path-to-arkui-x-sdk/10/arkui-x/toolchains/bin:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export PATH=/path-to-arkui-x-sdk/10/arkui-x/toolchains/bin:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set PATH=%PATH%;/path-to-arkui-x-sdk/10/arkui-x/toolchains/bin
   ```

## 环境检查

   ```shell
   ace check -v
   ```

执行 `ace check -v` 命令可以检查上述的本地开发环境配置情况。对于必选项，需要检查通过，否则无法继续接下来的操作。
