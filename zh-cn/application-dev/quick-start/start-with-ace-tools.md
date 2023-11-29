# ACE Tools快速指南

## 简介

ACE Tools是一套为ArkUI-X应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等。

## 常用命令参考

- [ACE Tools命令行详情说明](https://gitee.com/arkui-x/cli/blob/master/README.md)

## 环境搭建指南

- [ACE Tools环境配置详细说明](../tutorial/how-to-configure-dev-environment.md)

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

   Project created successfully! Target directory:  ${当前目录}/demo.

   In order to run your application, type:

      $ cd demo
      $ ace run
   
   Your application code is in demo/entry.
   ```

### 应用运行

* 安装运行到Android设备

```shell
cd demo
ace run apk
```

* 安装运行到iOS设备（注：执行ace run ios前请先打开Xcode完成应用签名）

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

<!--no_check-->