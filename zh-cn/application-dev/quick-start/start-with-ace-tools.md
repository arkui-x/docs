# ACE Tools快速指南

## 简介

ACE Tools是一套为ArkUI-X应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等。

## 环境准备

**前置条件：** Ubuntu需要18.04以上版本，macOS需要11.6.2及以上版本，Windows需要Windows 10版本。

**1. 配置Node.js环境**

   运行ACE Tools需Node.js环境支持，建议下载18.x版本。可命令行运行 `node -v` 查看本地Node.js版本，如不存在或版本不符合要求，请自行下载安装稳定版本：[Node.js下载地址](https://nodejs.org/en/download/)，并配置到环境变量。

**2. 配置Java环境**

   Android和OpenHarmony/HarmonyOS应用打包需Java环境支持，建议下载JDK11.0.2以上版本，下载请点击[此处](https://repo.huaweicloud.com/openjdk/)。推荐环境变量配置如下：

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

**3. 配置ohpm环境**

   OHPM CLI（OpenHarmony Package Manager Command-line Interface）是OpenHarmony应用工程的三方库的包管理工具，可通过DevEco Studio > File > Settings > Build, Execution, Deployment > Ohpm 查看ohpm home的安装路径，并配置到环境变量中。

**4. 配置ArkUI-X SDK环境**

   ArkUI-X SDK下载路径，可通过DevEco Studio > File > Settings > ArkUI-X（macOS为DevEco Studio > Preferences > ArkUI-X）查看ArkUI-X的下载路径，并配置到环境变量中。推荐环境变量配置如下：

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
   - 修改npm源，前往用户目录，在.npmrc文件中添加如下内容：

   ```shell
   @ohos:registry=https://repo.harmonyos.com/npm/
   registry=https://repo.huaweicloud.com/repository/npm/
   ```

   - 全局安装ACE命令

   ```shell
   cd arkui-x/toolchains/ace_tools    // 根据ArkUI-X SDK下载路径，进入ACE Tools实际所在目录。
   npm install
   npm install . -g
   ```

## 使用说明

### 开发环境检查

   ```shell
   ace check
   ```

执行 `ace check` 命令可以检查ArkUI-X应用本地开发环境是否完备。

*注：开发环境检查主要针对Android/iOS/OpenHarmony/HarmonyOS IDE以及对应SDK的默认安装和下载路径进行检查。如果提示结果与实际不符，请您通过ace config命令指定实际的IDE安装和SDK下载路径。*

### 创建应用

   以创建一个 Stage模型‘demo’项目为例：

   ```shell
   ohos@user Desktop % ace create demo
   ? Please enter the project name(demo): # 输入工程名称，不输入默认为文件夹名称
   ? Please enter the bundleName (com.example.demo):  # 输入包名，不输入默认为com.example.工程名
   ? Please enter the runtimeOS (1: OpenHarmony, 2: HarmonyOS): 1 # 输入RuntimeOS系统
   Check summary (to see all details, run ace check -v)
   [√] ArkUI-X (ArkUI-X SDK version 0.0.9.6)
   [√] OpenHarmony toolchains - develop for OpenHarmony devices (OpenHarmony SDK version 4.0.9.6)
   [√] HarmonyOS toolchains - develop for HarmonyOS devices (HarmonyOS SDK version 3.1.0)
   [√] Android toolchains - develop for Android devices (Android SDK version 34.0.0)
   [√] DevEco Studio (version 4.0.3)
   [√] Android Studio (version 2022.3)
   [√] Xcode - develop for iOS (Xcode 14.3.1)
   Tools info :[√] OpenHarmony hdc installed
               [√] HarmonyOS hdc installed
               [√] adb installed
               [√] ios-deploy installed
   [√] Connected device (1 available)
   •  iPhone 14 Pro Max (1058643C-A725-4E19-AA62-781588C94A7F) [iOS Simulator]
   √ ACE Tools found no issues.
   Project created successfully!
   ```

### 应用运行

* 安装运行到Android设备

```shell
cd demo
ace run apk
```

* 安装运行到iOS设备

```shell
cd demo
ace run ios
```

* 安装运行到OpenHarmony设备

```shell
cd demo
ace run hap
```

上述命令会完成应用构建打包，并安装到目标平台设备运行。

## 参考

- [ACE Tools命令行详情说明](https://gitee.com/arkui-x/cli/blob/master/README.md)
- [Ubuntu环境配置说明](../tutorial/how-to-configure-dev-environment.md)

<!--no_check-->