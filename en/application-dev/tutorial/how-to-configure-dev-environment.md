# How to Configure the ACE Tools Environment

## Overview

ACE Tools is a command line (CLI) tool that allows you to build ArkUI-X applications on the OpenHarmony, HarmonyOS, Android, and iOS platforms. Its functions include development environment check, project creation, building and packaging, and installation and debugging.

## Setting Up the Environment

Prerequisites: macOS 11.6.2 or later, Windows 10, or Ubuntu 18.04 or later

**Configuring the ohpm Environment**

OpenHarmony Package Manager Command Line Interface (OHPM CLI) is the package management tool of third-party libraries in OpenHarmony. It supports the release, installation, and dependency management of OpenHarmony shared packages. For details about how to configure ohpm, see [ohpm Usage Guide](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/ide-command-line-ohpm-0000001490235312-V3).

**Configuring the Java Environment**

The Java environment is required for packaging Android, OpenHarmony, and HarmonyOS applications. You are advised to download JDK 11.0.2. You can download it [here](https://repo.huaweicloud.com/openjdk/).

The following are recommended environment variable configurations:

[Ubuntu]

   ```shell
   // Configure environment variables.
   export JAVA_HOME=/path-to-java-sdk
   export PATH=${JAVA_HOME}/bin:${PATH}
   ```

[macOS]

   ```shell
   // Configure environment variables.
   export JAVA_HOME=/path-to-java-sdk
   export PATH=$JAVA_HOME/bin:$PATH
   ```

[Windows]

   ```shell
   // Configure environment variables.
   set JAVA_HOME=/path-to-java-sdk
   set PATH=%PATH%;%JAVA_HOME%/bin
   ```

**Configuring the ArkUI-X SDK Environment**

For details about how to obtain and configure the ArkUI-X SDK, see [ArkUI-X SDK](../tools/how-to-use-arkui-x-sdk.md).

The following are recommended environment variable configurations:

[Ubuntu]

   ```shell
   // Configure environment variables.
   export ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   export PATH=${ARKUIX_SDK_HOME}/10/arkui-x/toolchains/bin:${PATH}
   ```
[macOS]

   ```shell
   // Configure environment variables.
   export ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   export PATH=${ARKUIX_SDK_HOME}/10/arkui-x/toolchains/bin:$PATH
   ```

[Windows]

You can enter **Environment Variables** in the search box on the toolbar of the home screen and choose **Edit the system environment variables** to configure environment variables. You can also run the following commands on the console:

   ```shell
   // Configure environment variables.
   set ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   set PATH=%PATH%;%ARKUIX_SDK_HOME%/10/arkui-x/toolchains/bin
   ```
   > **NOTE**: During environment variable configuration, do not change **ARKUIX_SDK_HOME**, which is a fixed variable name required by ACE Tools.

## Checking the Environment

Run the **ace check** command to check the dependency configuration of the ArkUI-X application development environment. Mandatory items must pass the check. Otherwise, you are not allowed to continue the subsequent operations.

   ```shell
   ace check
   ```
After the environment is configured, you can use [ACE Tools](../quick-start/start-with-ace-tools.md#creating-an-application) to develop and debug an ArkUI-X application.

  > **NOTE**: Only the default installation paths of DevEco Studio and the SDK and the environment variable configurations mentioned below are checked. If the check result is inconsistent with the actual situation, run the [ace config command](https://gitcode.com/arkui-x/cli#ace-config) to specify the actual DevEco Studio installation path and SDK download path.

## Configuring the Environment for the Target Platform

**OpenHarmony Platform**

If you use DevEco Studio, download the SDK via DevEco Studio by following the instructions provided in [SDK Management](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427). If you do not use DevEco Studio, download the SDK via the [OpenHarmony SDK CLI](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli_openharmony). For details about how to use the CLI, see [ohsdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ohos-sdk-command-line-tool-0000001263280431).

After the SDK is downloaded, configure environment variables as follows:

[Ubuntu]

   ```shell
   // Configure environment variables.
   export OpenHarmony_HOME=/path-to-openharmony-sdk
   export PATH=${OpenHarmony_HOME}/versioncode/toolchains:${PATH}
   ```

[macOS]

   ```shell
   // Configure environment variables.
   export OpenHarmony_HOME=/path-to-openharmony-sdk
   export PATH=$OpenHarmony_HOME/versioncode/toolchains:$PATH
   ```

[Windows]

   ```shell
   // Configure environment variables.
   set OpenHarmony_HOME=/path-to-openharmony-sdk
   set PATH=%PATH%;%OpenHarmony_HOME%/versioncode/toolchains
   ```

**HarmonyOS Platform**

If you use DevEco Studio, download the SDK via DevEco Studio by following the instructions provided in [SDK Management](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427). If you do not use DevEco Studio, download the SDK via the [HarmonyOS SDK CLI](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli). For details about how to use the CLI, see [sdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ide-command-line-sdkmgr-0000001110390078).

After the SDK is downloaded, configure environment variables as follows:

   [Ubuntu]

   ```shell
   // Configure environment variables.
   export HarmonyOS_HOME=/path-to-harmonyos-sdk
   export PATH=${HarmonyOS_HOME}/openharmony/versioncode/toolchains:${PATH}
   ```

[macOS]

   ```shell
   // Configure environment variables.
   export HarmonyOS_HOME=/path-to-harmonyos-sdk
   export PATH=$HarmonyOS_HOME/openharmony/versioncode/toolchains:$PATH
   ```

[Windows]

   ```shell
   // Configure environment variables.
   set HarmonyOS_HOME=/path-to-harmonyos-sdk
   set PATH=%PATH%;%HarmonyOS_HOME%/openharmony/versioncode/toolchains
   ```

**Android Platform**

If you use Android Studio, download the SDK via Android Studio by following the instructions provided in [Update your tools with the SDK Manager](https://developer.android.google.cn/studio/intro/update#sdk-manager). If you do not use DevEco Studio, download the SDK via the [Android SDK CLI](https://developer.android.google.cn/studio#command-line-tools-only). For details about how to use the CLI, see [sdkmanager](https://developer.android.google.cn/studio/command-line/sdkmanager).

After the SDK is downloaded, configure environment variables as follows:

[Ubuntu]

   ```shell
   // Configure environment variables.
   export ANDROID_HOME=/path-to-android-sdk
   export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
   ```

[macOS]

   ```shell
   // Configure environment variables.
   export ANDROID_HOME=/path-to-android-sdk
   export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/build-tools/28.0.3:$ANDROID_HOME/platform-tools:$PATH
   ```

[Windows]

   ```shell
   // Configure environment variables.
   set ANDROID_HOME=/path-to-android-sdk
   set PATH=%PATH%;%ANDROID_HOME%/tools;%ANDROID_HOME%/tools/bin;%ANDROID_HOME%/build-tools/28.0.3;%ANDROID_HOME%/platform-tools
   ```

**iOS Platform**

You can download and install Xcode and Command Line Tools for Xcode from Mac App Store. You can also run the following command to install the Command Line Tools for Xcode:

   ```shell
   xcode-select --install
   ```

Install libimobiledevice. For details, see [libimobiledevice](https://libimobiledevice.org).

   ```shell
   brew install libimobiledevice
   ```

Install ios-deploy. For details, see [ios-deploy](https://github.com/ios-control/ios-deploy).

   ```shell
   brew install ios-deploy
   ```
