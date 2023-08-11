# Before You Start

This document is intended for beginners who use the ArkUI-X framework for development. It helps you quickly understand the ArkUI-X project development process.


## Basic Concepts

### ArkUI Framework

ArkUI is a declarative UI development framework for building distributed applications. It provides simple and natural UI syntax, diverse array of UI components, multi-dimensional status management, and real-time UI preview tools, helping you improve application development efficiency and achieve vivid and smooth user experience across devices. For details, see [ArkUI Overview](https://gitee.com/openharmony/docs/blob/master/en/application-dev/ui/arkui-overview.md).

### ArkUI-X

ArkUI-X extends the ArkUI framework to multiple OS platforms. Currently, it supports OpenHarmony, Android, and iOS. More OS platforms will be supported in the future. With ArkUI-X, you can use a set of main code to develop high-performance applications supporting multiple platforms.

### API Extension

API extension involves the following: You can use the OpenHarmony NAPI mechanism to define OpenHarmony APIs on Android and iOS platforms. You can also develop service plug-ins based on the Android and iOS APIs or third-party libraries.

## Setting Up the Environment

- Install Ubuntu 18.04 or later, or macOS 11.6.2 or later.

- Install the application packages required for build.

  [Linux]

  ```shell
  sudo apt-get install binutils git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip m4
  ```

  [macOS]

  ```shell
  brew install wget coreutils
  ```

### Configuring the Java Environment
**NOTE**:<br>You are advised to [download JDK11.0.2 or later](https://repo.huaweicloud.com/openjdk/).

  [Linux]

  ```shell
  // Configure environment variables.
  export JAVA_HOME=/home/usrername/path-to-java-sdk
  export PATH=${JAVA_HOME}/bin:${PATH}
  ```

  [macOS]

  ```shell
  // Configure environment variables.
  export JAVA_HOME=/Users/usrername/path-to-java-sdk
  export PATH=$JAVA_HOME/bin:$PATH
  ```

### Configuring Android SDK Environment

  [Linux]

  Use the [command line tools](https://developer.android.google.cn/studio#command-line-tools-only) to download and manage the Android SDK. For details about how to use the command line tools, see [sdkmanager](https://developer.android.com/tools/sdkmanager). The SDK must meet the following version requirements:

  ```shell
  ./sdkmanager --install "ndk;21.3.6528147" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "platforms;android-26" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "build-tools;28.0.3" --sdk_root=/home/usrername/path-to-android-sdk
  ```

  ```shell
  // Configure environment variables.
  export ANDROID_HOME=/home/usrername/path-to-android-sdk
  export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
  ```

  [macOS]

  Download and manage the Android SDK through IDE [SDK Manager](https://developer.android.google.cn/studio/intro/update#sdk-manager). The NDK version must be 21.3.6528147, and the SDK Platform version must be 26.

  ```shell
  // Configure environment variables.
  export ANDROID_HOME=/Users/usrername/path-to-android-sdk
  export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/build-tools/28.0.3:$ANDROID_HOME/platform-tools:$PATH
  ```

### Configuring the iOS SDK Environment

  - You can download and install Xcode and Command Line Tools for Xcode from Mac App Store.
  - You can also run the following command to install the Command Line Tools for Xcode:

    ```shell
    xcode-select --install
    ```

Then, you can [download code](./start-with-download.md) and [build an ArkUI-X project](./start-with-build.md) with the ArkUI framework.
