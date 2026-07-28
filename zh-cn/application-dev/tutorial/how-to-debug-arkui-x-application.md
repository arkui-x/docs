# 跨平台方舟Debug应用堆栈查看开发指导

## 简介

本文档用于指导在 ArkUI-X 工程中编译、安装和调试 Debug 应用，并查看应用运行日志。Debug 应用支持 Source Map 加载，可将 TS 堆栈还原为 ETS 源文件行号，便于定位问题。

## 能力说明

Debug 应用构建流程会携带 Source Map 资源，支持以下能力：

- TS 堆栈到 ETS 源文件行号的还原
- 运行时日志输出与查看
- Source Map 资源的加载

TS 堆栈还原为 ETS 源文件行号的功能在 Debug 应用中自动生效，当应用启动时，会自动加载 Source Map 资源；发生异常时，会自动将 TS 堆栈中的行号映射到 ETS 源文件行号并输出到应用日志中。

## 前置条件

- macOS 需要 11.6.2 及以上版本，Windows 需要 Windows 10 及以上版本。
- 需配置 ACE Tools 环境，配置可参考：[ACE Tools环境配置指导](./how-to-configure-dev-environment.md)
- 如果使用 DevEco Studio，需安装 Android Studio 或 Xcode，并配置好对应的 ArkUI-X SDK 及工程。

**使用前请确认：**

1. 当前工程为 ArkUI-X 工程。
2. 工程中已经正确安装并配置 ArkUI-X SDK。

## 使用 ACE Tools 编译、安装和查看日志

ACE Tools 提供了一套完整的命令行工具，支持编译、安装和日志查看。

### 编译 Debug 应用

在工程根目录下执行以下命令，指定 Debug 模式构建：

```bash
ace build apk --debug
```

或构建 iOS 应用：

```bash
ace build ios --debug
```
* 补充说明：iOS 构建需先在 Xcode 工程中配置开发者签名。

### 安装应用到设备

构建完成后，在工程根目录下执行安装命令(无需指定安装路径)：

```bash
ace install apk
```

或安装 iOS 应用：

```bash
ace install ios
```

在多设备的场景下，可以按照如下命令指定安装设备：

* Android 平台
```bash
ace install apk -d <deviceId>
```
* iOS 平台
```bash
ace install ios -d <deviceId>
```

deviceId 可以通过如下的命令去获取：

```bash
ace devices

Tools info :[√] OpenHarmony hdc installed
            [√] HarmonyOS hdc installed
            [√] adb installed

[√] Connected device (2 available)
  •  BKQ_AN00 (AS6VVB5A2300XXXX) [Android]
  •  YAL_AL50 (NDYDU1981901XXXX) [Android]
```
上述的 AS6VVB5A2300XXXX 就是 deviceId。

Ace Tools 的详细使用说明请参考：[ACE Tools 使用说明](https://gitcode.com/arkui-x/cli/blob/master/README.md)

### 查看应用日志

推荐使用 ACE Tools 提供的 `ace log` 命令查看应用日志：

```bash
ace log
```
输出的堆栈日志的格式如下：

```bash
Error: Nested function error
    at nestedFunctionC entry (entry/src/main/ets/pages/StackTestEntry.ets:21:11)
    at nestedFunctionB entry (entry/src/main/ets/pages/StackTestEntry.ets:17:10)
    at nestedFunctionA entry (entry/src/main/ets/pages/StackTestEntry.ets:13:10)
    at handleUncaughtNested entry (entry/src/main/ets/pages/StackTestEntry.ets:31:10)
    at anonymous entry (entry/src/main/ets/pages/StackTestEntry.ets:99:35)
```

或指定平台：

```bash
ace log apk
ace log ios
```

指定设备：

```bash
ace log -d <deviceId>
```

`ace log` 命令会自动获取应用进程 ID，并输出该进程的相关日志。

### 构建输出目录

以 entry 模块为例，Debug 构建输出目录如下：

#### Android

- APK 输出路径：`.arkui-x/android/app/build/outputs/apk/debug/app-debug.apk`
- Source Map 资源路径：`.arkui-x/android/app/src/main/assets/arkui-x/entry/ets/sourceMaps.map`

#### iOS

- APP 输出路径：`.arkui-x/ios/build/outputs/app/`
- Source Map 资源路径：`.arkui-x/ios/arkui-x/entry/ets/sourceMaps.map`

## 使用 DevEco Studio 编译、安装和查看日志

DevEco Studio 主要用于编译 OpenHarmony/HarmonyOS 的 HAP/HSP 包。对于 Android 和 iOS 平台的应用，DevEco Studio 可以编译生成对应平台的工程，但无法自动安装和拉起应用，因此需要配合 Android Studio 或 Xcode 使用。

### 编译 Android APK

DevEco Studio 可以编译生成 Android 工程，但无法自动安装和拉起应用，需要使用 Android Studio 配合完成。

#### 1. 使用 DevEco Studio 编译 Android 工程

- 使用 DevEco Studio 将工程修改为 debug 模式：点击 DevEco Studio 右上角的 **选择产品** 图标，点击 **构建模式** 下拉框，选择 **debug** 模式。
- 点击菜单栏的 **构建 > 编译 Hap(s)/APP(s) > 编译 APP(s)** 进行编译。

#### 2. 使用 Android Studio 进行安装和拉起应用

使用 Android Studio 打开 `.arkui-x/android` 目录下的工程，点击右上角的 **Debug 'app'** 按钮，即可编译 Debug APK 并安装到 Android 设备。

#### 3. 查看日志

使用 Android Studio 中的 logcat 面板或 `adb logcat` 命令查看应用日志。

### 编译 iOS APP

DevEco Studio 可以编译生成 iOS 工程，但无法自动安装和拉起应用，且 iOS 工程需要开发者手动进行签名，因此需要使用 Xcode 配合完成。

#### 1. 使用 DevEco Studio 编译 iOS 工程

- 使用 DevEco Studio 将工程修改为 debug 模式：点击 DevEco Studio 右上角的 **选择产品** 图标，点击 **构建模式** 下拉框，选择 **debug** 模式。
- 点击菜单栏的 **构建 > 编译 Hap(s)/APP(s) > 编译 APP(s)** 进行编译（编译前需使用 Xcode 进行签名）。

#### 2. 使用 Xcode 进行安装和拉起应用

使用 Xcode 打开 `.arkui-x/ios` 目录下的工程，使用快捷键 **Cmd + Shift + ,** 打开面板，选择 **Run**，修改 **Build Configuration** 为 **Debug**。

> iOS 工程必须手动配置开发者签名才能编译成功，DevEco Studio 不会自动处理 iOS 签名。

#### 3. 查看日志

使用 Xcode 中的 Console 面板查看应用日志，Xcode 会自动获取应用日志。

## 注意事项

- Release 模式下不会打包 Source Map 文件，堆栈将显示TS中间代码行号，无法还原到 ETS 源文件行号。
- iOS 设备查看日志时，需确保设备已信任开发证书，并在设置中开启开发者模式。
- Android 设备查看日志时，需确保 USB 调试已开启，并授权 adb 调试权限。
- 确保使用的 SDK 版本支持 debug 构建（API 10 及以上 Stage 模型均支持）。
