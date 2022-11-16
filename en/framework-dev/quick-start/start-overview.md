# Before You Start

This document is intended for the beginners who use the ArkUI-X project framework. It helps you quickly understand the ArkUI-X project development process.

Before you start, you need to understand the basic concepts about the ArkUI framework and the ArkUI-X project.

## Basic Concepts

### ArkUI framework

ArkUI is a declarative UI development framework for building distributed applications. It provides simple and natural UI syntax, diverse array of UI components, multi-dimensional status management, and real-time UI preview tools, helping you improve application development efficiency and achieve vivid and smooth user experience across devices.

### ArkUI-X Project

The ArkUI-X project further extends the ArkUI framework to multiple OS platforms. Currently, it supports OpenHarmony, Android, and iOS. More OS platforms will be supported in the future. The ArkUI-X project allows you to develop high-performance applications supporting multiple platforms by using a set of main code.

### API Extension

API extension involves the following: You can use the OpenHarmony NAPI mechanism to implement the OpenHarmony interface definition on the Android and iOS platforms. You can develop service plug-ins based on the Android and iOS platform APIs or third-party libraries.

## Setting Up the Environment

- Install Ubuntu 18.04 or later, or macOS 11.6.2 or later.

- Install the application packages required for build.

  [Linux]

  ```shell
  sudo apt-get install binutils git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip m4
  ```

  [Mac]

  ```shell
  brew install wget coreutils
  ```

### Configuring the Java Environment
**NOTE**:<br>You are advised to download [JDK11.0.2 or later](https://repo.huaweicloud.com/openjdk/).

  [Linux]

  ```shell
  // Configure environment variables.
  export JAVA_HOME=/home/usrername/path-to-java-sdk
  export PATH=${JAVA_HOME}/bin:${PATH}
  ```

  [Mac]

  ```shell
  // Configure environment variables.
  export JAVA_HOME=/Users/usrername/path-to-java-sdk
  export PATH=$JAVA_HOME/bin:$PATH
  ```

### Configuring Android SDK Environment

Download the Android SDK from [ANDROID STUDIO](https://developer.android.google.cn/studio). For details about how to use the SDK, see [sdkmanager](https://developer.android.google.cn/studio/command-line/sdkmanager). The SDK must meet the following version requirements:

  ```shell
  ./sdkmanager --install "ndk-bundle" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "ndk;21.3.6528147" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "cmake;3.10.2.4988404" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "platforms;android-29" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "build-tools;28.0.3" --sdk_root=/home/usrername/path-to-android-sdk
  ```
**NOTE**<br>If you use Android Studio, you do not need to use this tool. You can [use the IDE to manage SDK software packages](https://developer.android.google.cn/studio/intro/update#sdk-manager).

  [Linux]

  ```shell
  // Configure environment variables.
  export ANDROID_HOME=/home/usrername/path-to-android-sdk
  export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
  ```

  [Mac]

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
