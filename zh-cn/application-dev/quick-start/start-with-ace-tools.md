# ACE Tools快速指南

## 简介

ACE Tools是一套为ArkUI-X应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等。

## 使用说明

针对Windows和macOS的平台环境，使用ACE Tools前，建议优先下载DevEco Studio，请参考[社区版本软件和工具配套关系](../../release-notes/ArkUI-X-v1.0.0-release.md#配套关系)完成DevEco Studio的下载和安装。Ubuntu环境用户请参考[Ubuntu环境配置说明](../tutorial/how-to-configure-dev-environment.md)。

### 环境准备

**前置条件：** Ubuntu需要18.04以上版本，macOS需要11.6.2及以上版本，Windows需要Windows 10版本。

**1. 配置ohpm环境**

   OHPM CLI作为鸿蒙生态三方库的包管理工具，支持OpenHarmony共享包的发布、安装和依赖管理。可通过DevEco Studio > File > Settings > Build, Execution, Deployment > Ohpm 查看ohpm home的安装路径，并配置到环境变量中（macOS为DevEco Studio > Preferences > Build, Execution, Deployment > Ohpm）。

**2. 配置ArkUI-X SDK环境**

   ArkUI-X SDK下载路径，可通过DevEco Studio > File > Settings > ArkUI-X查看ArkUI-X的安装路径，并配置到环境变量中（macOS为DevEco Studio > Preferences > ArkUI-X）。推荐如下配置方法：

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

### 开发环境检查

   ```shell
   ace check
   ```

执行 `ace check` 命令可以检查ArkUI-X应用开发环境是否完备。

> **说明**：开发环境检查只识别IDE和SDK默认的安装路径，如果提示结果与实际不符，请您通过[ace config命令](https://gitee.com/arkui-x/cli#ace-config)指定实际的IDE安装和SDK下载路径。

### 创建应用

   以创建一个 Stage模型‘demo’项目为例：

 ```shell
 ohos@user Desktop % ace create demo
 ? Enter the project name(demo): # 输入工程名称，不输入默认为文件夹名称
 ? Enter the bundleName (com.example.demo):  # 输入包名，不输入默认为com.example.工程名
 ? Enter the runtimeOS (1: OpenHarmony, 2: HarmonyOS): 1 # 输入RuntimeOS系统

 Project created. Target directory:  ${当前目录}/demo.

 In order to run your app, type:

    $ cd demo
    $ ace run

 Your app code is in demo/entry.
 ```

### 应用运行

* 安装运行到Android/iOS/OpenHarmony设备（注：iOS设备执行ace run前请先打开Xcode完成应用签名）

```shell
cd demo
ace run
```

上述命令会完成应用构建打包，并安装到目标平台设备运行。

## 常用命令参考

- [ACE Tools使用说明](https://gitee.com/arkui-x/cli/blob/master/README.md)