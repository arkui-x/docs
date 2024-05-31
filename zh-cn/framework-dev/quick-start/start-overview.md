# 开发准备

本文档适用于ArkUI-X框架开发的初学者。通过环境搭建、代码下载、代码编译、API扩展和使用，快速了解跨平台项目开发流程。


## 基本概念

### ArkUI框架

ArkUI是一套构建分布式应用的声明式UI开发框架。它具备简洁自然的UI信息语法、丰富的UI组件、多维的状态管理，以及实时界面预览工具，帮助您提升应用开发效率，并能在多种设备上实现生动而流畅的用户体验。详情可参考[ArkUI框架介绍](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/ui/arkui-overview.md)

### ArkUI-X

ArkUI-X进一步将ArkUI扩展到了多个OS平台：目前支持OpenHarmony、Android、 iOS，后续会逐步增加更多平台支持。开发者基于一套主代码，就可以构建支持多平台的精美、高性能应用。

### API扩展

API扩展包括两部分内容：一是复用OpenHarmony NAPI机制，在Android和iOS平台实现OpenHarmony的接口定义；二是支持开发者基于Android和iOS平台接口能力或三方库能力扩展业务插件。

## 环境准备

- 编译环境需要Ubuntu18.04及以上版本，macOS需要11.6.2及以上版本。

- 安装编译所需的程序包。

  [Linux]

  ```shell
  sudo apt-get install binutils git-core gnupg flex bison gperf build-essential zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev ccache libgl1-mesa-dev libxml2-utils xsltproc unzip m4
  ```

  [macOS]

  ```shell
  brew install wget coreutils
  ```

### 配置Java环境
**说明：** 建议下载JDK17.0.10版本，下载请点击[此处](https://repo.huaweicloud.com/openjdk/)。

  [Linux]

  ```shell
  // 配置环境变量
  export JAVA_HOME=/home/usrername/path-to-java-sdk
  export PATH=${JAVA_HOME}/bin:${PATH}
  ```

  [macOS]

  ```shell
  // 配置环境变量
  export JAVA_HOME=/Users/usrername/path-to-java-sdk
  export PATH=$JAVA_HOME/bin:$PATH
  ```

### 配置Android SDK环境

  [Linux]

  通过[命令行工具](https://developer.android.google.cn/studio#command-line-tools-only)下载和管理Android SDK，命令行工具使用说明详见[sdkmanager](https://developer.android.google.cn/studio/command-line/sdkmanager)官方指导。SDK版本下载要求如下：

  ```shell
  ./sdkmanager --install "ndk;21.3.6528147" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "platforms;android-26" --sdk_root=/home/usrername/path-to-android-sdk
  ./sdkmanager --install "build-tools;28.0.3" --sdk_root=/home/usrername/path-to-android-sdk
  ```

  ```shell
  // 配置环境变量
  export ANDROID_HOME=/home/usrername/path-to-android-sdk
  export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
  ```

  [macOS]

  通过IDE [SDK管理器](https://developer.android.google.cn/studio/intro/update#sdk-manager)下载和管理Android SDK，NDK版本要求为：21.3.6528147，SDK Platform版本为：26。

  ```shell
  // 配置环境变量
  export ANDROID_HOME=/Users/usrername/path-to-android-sdk
  export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/build-tools/28.0.3:$ANDROID_HOME/platform-tools:$PATH
  ```

### 配置iOS SDK环境

  - Xcode和Command Line Tools for Xcode应用可前往Mac App Store应用商店下载安装。
  - Command Line Tools也可使用命令方式安装:

    ```shell
    xcode-select --install
    ```

完成上述操作及基本概念的理解后，即可参照[下载代码](./start-with-download.md)和[编译代码](./start-with-build.md)章节内容进一步体验跨平台框架开发和学习。