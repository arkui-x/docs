# ACE Tools快速指南

## 简介

ACE Tools是一套为ArkUI-X项目跨平台应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等。

## 环境准备

**前置条件：** Ubuntu需要18.04以上版本，macOS需要11.6.2及以上版本，Windows需要Windows 10版本。

**1. 配置Node.js环境**

   运行ACE Tools和OpenHarmony SDK需Node.js环境支持，建议下载14.19.1 - 16.19.1版本。可命令行运行 `node -v` 查看本地Node.js版本，如不存在或版本不符合要求，请自行下载安装稳定版本：[Node.js下载地址](https://nodejs.org/en/download/)，并配置到环境变量。

**2. 配置Java环境**

   Android和OpenHarmony/HarmonyOS应用打包需Java环境支持，建议下载JDK11.0.2以上版本，下载请点击[此处](https://repo.huaweicloud.com/openjdk/)。推荐环境变量配置如下：

   [Mac]

   ```shell
   // 配置环境变量
   export JAVA_HOME=/Users/usrername/path-to-java-sdk
   export PATH=$JAVA_HOME/bin:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set JAVA_HOME=/Users/usrername/path-to-java-sdk
   set PATH=%PATH%;%JAVA_HOME%/bin
   ```

**3. 配置ohpm环境**

   OHPM CLI（OpenHarmony Package Manager Command-line Interface）是OpenHarmony应用工程的三方库的包管理工具，可通过DevEco Studio > File > Settings > Build, Execution, Deployment > Ohpm 查看ohpm home的安装路径，并配置到环境变量中。

**4. 配置ArkUI-X SDK环境**

   ArkUI-X SDK下载路径，可通过DevEco Studio > File > Settings > ArkUI-X（macOS为DevEco Studio > Preferences > ArkUI-X）查看ArkUI-X的下载路径，并配置到环境变量中。推荐环境变量配置如下：

   [Mac]

   ```shell
   // 配置环境变量
   export ARKUIX_SDK_HOME=/Users/usrername/path-to-arkui-x-sdk
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set ARKUIX_SDK_HOME=/Users/usrername/path-to-arkui-x-sdk
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
   cd arkui-x/toolchains/ace_tools    // 根据ArkUI-X SDK下载路径，按实进入ACE Tools所在目录。
   npm install
   npm install . -g
   ```

## 使用说明

### 开发环境检查

   ```shell
   ace check
   ```

执行 `ace check` 命令可以检查ArkUI-X应用本地开发环境是否完备。对于必选项，需要检查通过，否则无法继续接下来的操作。

*注：开发环境检查主要针对Android/iOS/OpenHarmony/HarmonyOS IDE以及对应SDK的默认安装和下载路径进行检查。如果提示结果与实际不符，请您通过ace config命令指定实际的IDE安装和SDK下载路径。*

### 创建应用

   以创建一个 Stage模型‘demo’项目为例：

   ```shell
   ace create project
   ? Please enter the project name: demo
   ? Please enter the bundle name (com.example.demo):com.example.demo
   ? Please enter the system (1: OpenHarmony, 2: HarmonyOS): 2
   ? Please enter the template (1: Empty Ability, 2: Native C++): 1   //选择创建Empty Ability或者Native C++项目
   ```

执行 `ace create project` 命令，接着输入项目名 demo ，包名直接回车默认即可。输入“2”代表创建HarmonyOS，再次输入“1”代表Empty Ability，再次输入“1”代表stage模型的应用项目。至此，一个名为 ‘demo’ 的项目就创建成功了。

### 编写代码

在上述工程创建完成后，开发者可在项目中的source目录下进行代码开发。

### 项目编译

开始对 'demo' 项目进行编译。编译目标产物为hap 、apk和app，分别对应OpenHarmony/HarmonyOS、Android和iOS应用工程，下述命令以hap为例，并且可将hap参数替换为apk或app，其分别对应Android应用和iOS应用，功能一致：

```shell
cd demo
```

 编译hap，默认编译所有Module

   ```shell
   ace build hap
   ```

   最终会生成一个hap应用文件，默认路径为 demo/ohos/entry/build/default/outputs/default/。

**注：** apk文件的默认路径为demo/android/app/build/outputs/apk/debug/。app文件的默认路径为demo/ios/build/outputs/app/。

### 应用安装和卸载

开始对编译出的hap应用包进行安装，先进入到demo工程目录下

```shell
cd demo
```
1. 安装hap应用包

   ```shell
   ace install hap
   ```

2. 安装hap应用到指定的设备上

   ```shell
   ace install hap -d deviceId
   ```
3. 卸载hap应用包

   ```shell
   ace uninstall hap --bundle bundleName
   ```

4. 卸载指定设备上的hap应用安装包

   ```shell
   ace uninstall hap --bundle bundleName -d deviceId
   ```

###  运行应用

1. 运行hap应用

   ```shell
   ace run hap
   ```

2. 在指定的设备上运行hap应用

   ```shell
   ace run hap -d deviceId
   ```

### 清理编译结果

清除所有编译结果(hap、apk、app)

```shell
ace clean
```

### 输出日志文件

滚动输出正在运行的应用日志信息

1. 输出hap应用日志

   ```shell
   ace log hap
   ```

2. 输出指定的设备上运行hap应用日志

   ```shell
   ace log hap -d deviceId
   ```

### 帮助工具

展示可以支持的命令信息

```shell
ace help
```

支持单个指令支持查询

```shell
ace build --help
```

## 参考

- [命令行详情说明](https://gitee.com/arkui-x/cli/blob/master/README.md)

<!--no_check-->
