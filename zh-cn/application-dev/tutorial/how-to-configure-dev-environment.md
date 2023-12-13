# ACE Tools环境配置指导

## 简介

ACE Tools是一套为ArkUI-X应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等，以下是详细的环境配置指导。

## 环境准备

**前置条件：** macOS需要11.6.2及以上版本，Windows需要Windows 10版本，Ubuntu需要18.04及以上版本。

**配置ohpm环境**

   OHPM CLI作为鸿蒙生态三方库的包管理工具，支持OpenHarmony共享包的发布、安装和依赖管理，配置方法请参见：[ohpm使用指导](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/ide-command-line-ohpm-0000001490235312-V3)。

**配置Java环境**

   Android和OpenHarmony/HarmonyOS应用打包需Java环境支持，建议下载JDK11.0.2版本，下载请点击[此处](https://repo.huaweicloud.com/openjdk/)。推荐环境变量配置方法：

   [Ubuntu]

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

**配置ArkUI-X SDK环境**

   ArkUI-SDK获取和配置目录要求，请参考[ArkUI-X SDK介绍](../tools/how-to-use-arkui-x-sdk.md)。推荐环境变量配置方法：

   [Ubuntu]

   ```shell
   // 配置环境变量
   export ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   export PATH=${ARKUIX_SDK_HOME}/10/arkui-x/toolchains/bin:${PATH}
   ```
   [macOS]

   ```shell
   // 配置环境变量
   export ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   export PATH=${ARKUIX_SDK_HOME}/10/arkui-x/toolchains/bin:$PATH
   ```

   [Windows]

   可在桌面工具栏**搜索框**键入"环境变量"，然后选择**编辑系统环境变量**，进行环境变量配置。另外，也可在控制台通过如下命令进行配置。

   ```shell
   // 配置环境变量
   set ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   set PATH=%PATH%;%ARKUIX_SDK_HOME%/10/arkui-x/toolchains/bin
   ```
   > **说明**：配置环境变量时，由于ARKUIX_SDK_HOME是ACE Tools要求的固定变量名，不允许自定义。

## 环境检查

   执行 `ace check` 命令可以检查ArkUI-X应用开发环境依赖配置情况。对于必选项，需要检查通过，否则无法继续接下来的操作。

   ```shell
   ace check
   ```
   环境配置完成后，即可使用[ACE Tools](../quick-start/start-with-ace-tools.md#创建应用)进行跨平台应用开发调试。

  > **说明**：开发环境检查只识别IDE和SDK默认的安装路径，以及下文提到的环境变量配置。如果提示结果与实际不符，请您通过[ace config命令](https://gitee.com/arkui-x/cli#ace-config)指定实际的IDE安装和SDK下载路径。

## 配置目标平台应用开发环境

**OpenHarmony平台**

   SDK下载方法：如果您使用DevEco Studio，可直接通过[IDE管理SDK软件包](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427)进行下载，否则需通过OpenHarmony [SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli_openharmony)下载OpenHarmony SDK，命令行工具使用说明详见[ohsdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ohos-sdk-command-line-tool-0000001263280431)官方指导。

   SDK下载完成后，按照如下方法配置环境变量：

   [Ubuntu]

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

**HarmonyOS平台**

   SDK下载方法：如果您使用DevEco Studio，可直接通过[IDE管理SDK软件包](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427)进行下载，否则需通过HarmonyOS [SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli)下载HarmonyOS SDK，命令行工具使用说明详见[sdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ide-command-line-sdkmgr-0000001110390078)官方指导。

   SDK下载完成后，按照如下方法配置环境变量：

   [Ubuntu]

   ```shell
   // 配置环境变量
   export HarmonyOS_HOME=/path-to-harmonyos-sdk
   export PATH=${HarmonyOS_HOME}/openharmony/versioncode/toolchains:${PATH}
   ```

   [macOS]

   ```shell
   // 配置环境变量
   export HarmonyOS_HOME=/path-to-harmonyos-sdk
   export PATH=$HarmonyOS_HOME/openharmony/versioncode/toolchains:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set HarmonyOS_HOME=/path-to-harmonyos-sdk
   set PATH=%PATH%;%HarmonyOS_HOME%/openharmony/versioncode/toolchains
   ```

**Android平台**

   SDK下载方法：如果您使用Android Studio，可直接通过[IDE管理SDK软件包](https://developer.android.google.cn/studio/intro/update#sdk-manager)进行下载，否则需通过[Android SDK命令行工具](https://developer.android.google.cn/studio#command-line-tools-only)下载Android SDK，命令行工具使用说明详见[sdkmanager](https://developer.android.google.cn/studio/command-line/sdkmanager)官方指导。

   SDK下载完成后，按照如下方法配置环境变量：

   [Ubuntu]

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

**iOS平台**

   Xcode和Command Line Tools for Xcode应用可前往Mac App Store应用商店下载安装。Command Line Tools也可使用命令方式安装:

   ```shell
   xcode-select --install
   ```

   libimobiledevice，详细信息[参照](https://libimobiledevice.org)。

   ```shell
   brew install libimobiledevice
   ```

   ios-deploy安装，详细信息[参照](https://github.com/ios-control/ios-deploy)。

   ```shell
   brew install ios-deploy
   ```
