# ACE Tools快速指南

## 简介

ACE Tools是一套为ArkUI-X应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等。

## 使用说明

针对Windows和macOS的平台环境，使用ACE Tools前，建议优先下载DevEco Studio，请参考[社区版本软件和工具配套关系](https://gitee.com/openharmony/docs/blob/master/zh-cn/release-notes/OpenHarmony-v4.0-release.md#配套关系)完成DevEco Studio的下载和安装。Linux环境用户请参考[Ubuntu环境配置说明](../tutorial/how-to-configure-dev-environment.md)。

### 环境准备

**前置条件：** Ubuntu需要18.04以上版本，macOS需要11.6.2及以上版本，Windows需要Windows 10版本。

**1. 配置ohpm环境**

   OHPM CLI（OpenHarmony Package Manager Command-line Interface）是OpenHarmony应用工程的三方库的包管理工具，可通过DevEco Studio > File > Settings > Build, Execution, Deployment > Ohpm 查看ohpm home的安装路径，并配置到环境变量中。

**2. 配置ArkUI-X SDK环境**

   ArkUI-X SDK下载路径，可通过DevEco Studio > File > Settings > ArkUI-X（macOS为DevEco Studio > Preferences > ArkUI-X）查看ArkUI-X的下载路径，并配置到环境变量中。

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

### 命令安装

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

### 开发环境检查

   ```shell
   ace check -v
   ```

执行 `ace check -v` 命令可以检查ArkUI-X应用本地开发环境是否完备。

*注：开发环境检查主要针对Android/iOS/OpenHarmony/HarmonyOS IDE和对应SDK，以及ArkUI-X SDK的默认安装和下载路径进行检查。如果提示结果与实际不符，请您通过[ace config命令](https://gitee.com/arkui-x/cli#ace-config)指定实际的IDE安装和SDK下载路径。*

### 创建应用

   以创建一个 Stage模型‘demo’项目为例：

 ```shell
 ohos@user Desktop % ace create demo
 ? Please enter the project name(demo): # 输入工程名称，不输入默认为文件夹名称
 ? Please enter the bundleName (com.example.demo):  # 输入包名，不输入默认为com.example.工程名
 ? Please enter the runtimeOS (1: OpenHarmony, 2: HarmonyOS): 1 # 输入RuntimeOS系统

 Project created successfully! Target directory:  ${当前目录}/demo.

 In order to run your application, type:

    $ cd demo
    $ ace run

 Your application code is in demo/entry.
 ```

### 应用运行

* 安装运行到Android/iOS/OpenHarmony设备（注：iOS设备执行ace run前请先打开Xcode完成应用签名）

```shell
cd demo
ace run
```

上述命令会完成应用构建打包，并安装到目标平台设备运行。

## 常用命令参考

- [ACE Tools命令行详情说明](https://gitee.com/arkui-x/cli/blob/master/README.md)

<!--no_check-->