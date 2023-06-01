# 快速指南

## 简介

ACE Tools是一套为ArkUI-X项目跨平台应用开发者提供的命令行工具，支持在Windows/Ubuntu/macOS平台运行，用于构建OpenHarmony、HarmonyOS、Android和iOS平台的应用程序， 其功能包括开发环境检查，新建项目，编译打包，安装调试等。

## 环境准备

**前置条件：** Ubuntu需要18.04以上版本，macOS需要11.6.2及以上版本，Windows需要Windows 10版本。

**1. 配置Node.js环境**

   运行ACE Tools和OpenHarmony SDK需Node.js环境支持，建议下载14.19.1 - 16.19.1版本。可命令行运行 `node -v` 查看本地Node.js版本，如不存在或版本不符合要求，请自行下载安装稳定版本：[Node.js下载地址](https://nodejs.org/en/download/)。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export NodeJS_HOME=/home/usrername/path-to-nodejs-sdk
   export PATH=${NodeJS_HOME}/bin:${PATH}
   ```

   [Mac]

   ```shell
   // 配置环境变量
   export NodeJS_HOME=/Users/usrername/path-to-nodejs-sdk
   export PATH=$NodeJS_HOME/bin:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set NodeJS_HOME=/Users/usrername/path-to-nodejs-sdk
   set PATH=%PATH%;%NodeJS_HOME%/bin
   ```

**2. 配置Java环境**

   Android和OpenHarmony/HarmonyOS应用打包需Java环境支持，建议下载JDK11.0.2以上版本，下载请点击[此处](https://repo.huaweicloud.com/openjdk/)。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export JAVA_HOME=/home/usrername/path-to-java-sdk
   export PATH=${JAVA_HOME}/bin:${PATH}
   ```

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

  
**3. 配置OpenHarmony SDK环境**

   **SDK下载：** 通过[OpenHarmony SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli_openharmony)下载OpenHarmony SDK，命令行工具使用说明详见[ohsdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ohos-sdk-command-line-tool-0000001263280431)官方指导。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export OpenHarmony_HOME=/home/usrername/path-to-ohsdk
   export PATH=${OpenHarmony_HOME}/versioncode/toolchains:${PATH}
   ```

   [Mac]

   ```shell
   // 配置环境变量
   export OpenHarmony_HOME=/Users/usrername/path-to-ohsdk
   export PATH=$OpenHarmony_HOME/versioncode/toolchains:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set OpenHarmony_HOME=/Users/usrername/path-to-ohsdk
   set PATH=%PATH%;%OpenHarmony_HOME%/versioncode/toolchains
   ```

   **说明：** 如果您使用DevEco Studio，则无需使用此命令行工具，可直接通过[IDE管理SDK软件包](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427)。

**4. 配置HarmonyOS SDK环境**

   **SDK下载：** 通过[HarmonyOS SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli)下载HarmonyOS SDK，命令行工具使用说明详见[sdkmgr](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/ide-command-line-sdkmgr-0000001110390078)官方指导。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export HarmonyOS_HOME=/home/usrername/path-to-hossdk
   export PATH=${HarmonyOS_HOME}/hmscore/versioncode/toolchains:${PATH}
   ```

   [Mac]

   ```shell
   // 配置环境变量
   export HarmonyOS_HOME=/Users/usrername/path-to-hossdk
   export PATH=$HarmonyOS_HOME/hmscore/versioncode/toolchains:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set HarmonyOS_HOME=/Users/usrername/path-to-hossdk
   set PATH=%PATH%;%HarmonyOS_HOME%/hmscore/versioncode/toolchains
   ```

   **说明：** 如果您使用DevEco Studio，则无需使用此命令行工具，可直接通过[IDE管理SDK软件包](https://developer.harmonyos.com/cn/docs/documentation/doc-guides/environment_config-0000001052902427)。


**5. 配置Android SDK环境**

   **SDK下载：** 通过[Android SDK命令行工具](https://developer.android.google.cn/studio#command-line-tools-only)下载Android SDK，命令行工具使用说明详见[sdkmanager](https://developer.android.google.cn/studio/command-line/sdkmanager)官方指导。推荐环境变量配置如下：

   [Linux]

   ```shell
   // 配置环境变量
   export ANDROID_HOME=/home/usrername/path-to-android-sdk
   export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
   ```

   [Mac]

   ```shell
   // 配置环境变量
   export ANDROID_HOME=/Users/usrername/path-to-android-sdk
   export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/build-tools/28.0.3:$ANDROID_HOME/platform-tools:$PATH
   ```

   [Windows]

   ```shell
   // 配置环境变量
   set ANDROID_HOME=/home/usrername/path-to-android-sdk
   set PATH=%PATH%;%ANDROID_HOME%/tools;%ANDROID_HOME%/tools/bin;%ANDROID_HOME%/build-tools/28.0.3;%ANDROID_HOME%/platform-tools
   ```

   **说明：** 如果您使用Android Studio，则无需使用此命令行工具，可直接通过[IDE管理SDK软件包](https://developer.android.google.cn/studio/intro/update#sdk-manager)。


**6. iOS应用开发环境**

   6.1 Xcode和Command Line Tools for Xcode应用可前往Mac App Store应用商店下载安装。Command Line Tools也可使用命令方式安装:

   ```shell
   xcode-select --install
   ```

   6.2 libimobiledevice，详细信息[参照](https://libimobiledevice.org)。

   ```shell
   brew install libimobiledevice
   ```

   6.3 ios-deploy安装，详细信息[参照](https://github.com/ios-control/ios-deploy)。

   ```shell
   brew install ios-deploy
   ```

## 命令安装
### 安装ohpm命令
   OpenHarmony [SDK命令行工具](https://developer.harmonyos.com/cn/develop/deveco-studio#download_cli_openharmony)自带ohpm，下载后进入到命令行目录后，执行：
   ```shell
   cd command-line-tools/ohpm    //按实进入package.json所在目录
   npm install
   npm install . -g
   ```
   **说明：** 如果您使用DevEco Studio，则无需额外下载ohpm，可在DevEco Studio设置中查看其安装路径，并配置到环境变量中。
### 安装ace命令
   - 修改npm源，前往用户目录，在.npmrc文件中添加如下内容：

   ```shell
   @ohos:registry=https://repo.harmonyos.com/npm/
   registry=https://repo.huaweicloud.com/repository/npm/
   ```

   - 全局安装ACE命令

   ```shell
   cd ace_tools/cli    //按实进入package.json所在目录
   npm install
   npm install . -g
   ```

## 使用说明

### 创建应用

#### 1. 检查开发环境

   ```shell
   ace check
   ```

执行 `ace check` 命令可以检查上述的本地开发环境。对于必选项，需要检查通过，否则无法继续接下来的操作。

*注：开发环境检查主要针对SDK和IDE的默认安装和下载路径；如果通过SDK Manager下载SDK，会检查默认环境变量：ANDROID_HOME、OpenHarmony_HOME和HarmonyOS_HOME是否配置。*

#### 2. 检查设备连接

   ```shell
   ace devices
   ```

获得当前连接的设备devices 及 deviceID。后续命令的参数需要加 deviceID，可随时执行查看。

*注：该命令已经集成在 ` ace check` 中，可跳过。*

#### 3. 开发环境路径配置

   ```shell
   ace config
   ```

如果开发者没有按照IDE和SDK默认路径进行安装和下载，可通过此命令进行自定义路径配置。

#### 4. 创建project

   以创建一个 ‘demo’  项目为例：

   ```shell
   ace create project
   ? Please enter the project name: demo
   ? Please enter the bundle name (com.example.demo):com.example.demo
   ? Please enter the system (1: OpenHarmony, 2: HarmonyOS): 1
   ? Please enter the template (1: Empty Ability, 2: Native C++): 1   //选择创建Empty Ability或者Native C++项目
   ? Please enter the Ability Model Type (1: Stage, 2: FA):
   ```

执行 `ace create project` 命令（project 可省略），接着输入项目名 demo ，包名直接回车默认即可。输入“1”代表创建OpenHarmony，再次输入“1”代表Empty Ability，再次输入“1”代表stage模型的应用项目。

一个名为 ‘demo’ 的项目就创建成功了。

项目关键结构如下：

```shell
demo/
├── android      //用于编译跨平台应用Android工程
│   ├── app
│   │   ├── build.gradle
│   │   ├── libs      //跨平台SDK中提供的Android jar包放置于此
│   │   │   └── arm64-v8a      //跨平台SDK中提供的Android so库放置于此
│   │   ├── proguard-rules.pro
│   │   └── src
│   │       ├── androidTest
│   │       ├── main
│   │       │   ├── AndroidManifest.xml
│   │       │   ├── assets	//用于存放跨平台应用编译生成的资源文件
│   │       │   ├── java
│   │       │   │   └── com
│   │       │   │       └── example
│   │       │   │           └── demo
│   │       │   │               ├── EntryMainActivity.java  //继承自ArkUI提供的StageActivity基类
│   │       │   │               └── MyApplication.java     //继承自ArkUI提供的StageApplication基类
│   │       │   └── res
│   │       └── test
│   ├── build.gradle
│   ├── gradle
│   ├── gradle.properties
│   ├── gradlew
│   ├── gradlew.bat
│   └── settings.gradle
├── ios     //用于编译跨平台应用ios工程
│   ├── app
│   │   ├── AppDelegate.h
│   │   ├── AppDelegate.m
│   │   ├── Assets.xcassets
│   │   ├── Base.lproj
│   │   ├── EntryMainViewController.h   //继承自ArkUI提供的StageViewController基类
│   │   ├── EntryMainViewController.m
│   │   ├── Info.plist
│   │   └── main.m
│   ├── app.xcodeproj
│   ├── arkui-x
│   └── frameworks
│       └── libarkui_ios.xcframework         //跨平台SDK中提供的iOS库放置于此
├── ohos    //用于编译跨平台应用ohos工程
│   ├── AppScope
│   ├── build-profile.json5
│   ├── hvigorfile.ts
│   └── package.json
└── source   //用于编写跨平台应用源码
    └── entry
        ├── build-profile.json5
        ├── hvigorfile.ts
        ├── package.json
        └── src
            ├── main
            │   ├── ets
            │   │   ├── Application
            │   │   │   └── AbilityStage.ts
            │   │   ├── mainability
            │   │   │   └── MainAbility.ts
            │   │   └── pages
            │   │       └── Index.ets
            │   ├── module.json5
            │   └── resources
            └── ohosTest

```

### 编写代码

在上述工程创建完成后，开发者可在项目中的source目录下进行代码开发。

### 项目编译

开始对 'demo' 项目进行编译。编译分为hap 、apk和app：

   ```shell
cd demo
   ```

1. 编译hap，默认编译所有Module

```shell
ace build hap
```

   每个Module生成一个hap应用文件，默认路径为 demo/ohos/entry/build/default/outputs/default/。

2. 编译hap，只编译指定的Module

```shell
ace build hap --target moduleName
ace build hap --target "moduleName1 moduleName2 ..."
```

   最终各module会在对应目录下生成一个hap应用文件。默认路径为 demo/ohos/moduleName/build/default/outputs/default/。

   *注：当前版本指定Module编译时，需要先完成entry模块编译，多个Module可分别加在 --target 参数后，使用引号包括目标Module名并使用空格分开。*

3. 编译apk，默认编译Module为app的模块

```shell
ace build apk
```

   最终会生成一个apk应用文件，默认路径为：demo/android/app/build/outputs/apk/debug/。

4. 编译apk，编译指定的Module

```shell
ace build apk --target moduleName
```

   最终会生成一个apk应用文件。默认路径为：demo/android/app/build/outputs/apk/debug/。

5. 编译app，默认编译Module为app的模块

```shell
ace build app
```
   
   最终生成一个app应用文件，默认路径为：demo/ios/build/outputs/app/
   
6. 编译app，编译指定的Module

```shell
ace build app --target moduleName
```

   最终会生成一个app应用文件。默认路径为：demo/ios/build/outputs/app/

### 应用安装和卸载

开始对编译出的应用包进行安装，先进入到demo工程目录下

   ```shell
cd demo
   ```

1. 安装hap应用安装包

```shell
ace install hap
```

2. 安装hap应用到指定的设备上

```shell
ace install hap -d deviceId
```

3. 安装apk应用安装包

```shell
ace install apk
```

4. 安装apk应用安装包到指定的设备上

```shell
ace install apk -d deviceId
```

5. 安装app应用安装包

```shell
ace install app
```

6. 安装app应用安装包到指定的设备上

```shell
ace install app -d deviceId
```

7. 卸载hap应用安装包

```shell
ace uninstall hap --bundle bundleName
```

8. 卸载指定设备上的hap应用安装包

```shell
ace uninstall hap --bundle bundleName -d deviceId
```

9. 卸载apk应用安装包

```shell
ace uninstall apk --bundle bundleName
```

10. 卸载指定设备上的apk应用安装包

```shell
ace uninstall apk --bundle bundleName -d deviceId
```

11. 卸载app应用安装包

```shell
ace uninstall app --bundle bundleName
```

12. 卸载指定设备上的app应用安装包

```shell
ace uninstall app --bundle bundleName -d deviceId
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

3. 运行apk应用

```shell
ace run apk
```

4. 在指定的设备上运行apk应用

```shell
ace run apk -d deviceId
```

5. 运行app应用

```shell
ace run app
```

6. 在指定的设备上运行app应用

```shell
ace run app -d deviceId
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

3. 输出apk应用日志

  ```shell
ace log apk
  ```

4. 输出指定的设备上运行apk应用日志

  ```shell
ace log apk -d deviceId
  ```

5. 输出app应用日志

  ```shell
ace log app
  ```

6. 输出指定的设备上运行app应用日志

  ```shell
ace log app -d deviceId
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
