# 使用DevEco Studio开发ArkUI-X项目
## 配置开发环境


### 准备工作

请参考[搭建开发环境](https://developer.harmonyos.com/cn/docs/documentation/doc-guides-V3/installation_process-0000001071425528-V3)完成DevEco Studio的下载与安装。


### 使用DevEco Studio开发ArkUI-X约束说明

- DevEco Studio仅支持ArkUI-X源码开发和调试，各平台Native代码请使用对应平台的IDE编辑器进行开发和调试；

- ArkUI-X支持在Android/iOS平台真机和模拟器上运行调试；

- 平台版本及构建工具要求：
  - OpenHarmony平台：支持API 10+；
  - Android平台：Android 8+，Level-26，version code: O，Codename: Oreo；
  - iOS平台：iOS 10+


### 依赖环境准备

在开发应用/服务前，请先完成依赖环境准备。

  **表1** Windows平台环境要求

| 组件包名 | 说明 | 
| -------- | -------- |
| HarmonyOS&nbsp;SDK | HarmonyOS的SDK包。 | 
| ArkUI-X&nbsp;SDK | ArkUI-X的SDK包。 | 
| Android&nbsp;SDK | Android的SDK包。 | 
| Android&nbsp;Studio | Android应用开发环境，请参考官方环境搭建指导。 | 

  **表2** macOS平台环境要求

| 组件包名 | 说明 | 
| -------- | -------- |
| HarmonyOS&nbsp;SDK | HarmonyOS的SDK包。 | 
| ArkUI-X&nbsp;SDK | ArkUI-X的SDK包。 | 
| Android&nbsp;SDK | Android的SDK包。 | 
| Android&nbsp;Studio | Android应用开发环境。 | 
| Xcode | iOS应用开发环境，请参考官方环境搭建指导。 | 


### 安装HarmonyOS SDK

1. 在**File &gt; Settings &gt; SDK**（macOS为**DevEco Studio &gt; Preferences &gt; SDK**）下，点击Location右侧的**Edit**，为SDK选择存储路径。

2. 在弹出的SDK Setup页面选择存储路径，一直点击**Next**，直到完成SDK的安装后，点击**Finish**。![zh-cn_image_0000001579836532](figures/zh-cn_image_0000001579836532.png)


### 安装ArkUI-X SDK

1. 在**File &gt; Settings &gt; ArkUI-X**（macOS为**DevEco Studio &gt; Preferences &gt; ArkUI-X**）下，点击Location右侧的**Edit**，为SDK选择存储路径。

2. 在弹出的SDK Setup页面选择存储路径，一直点击**Next**，直到完成SDK的安装后，点击**Finish**。![zh-cn_image_0000001579996808](figures/zh-cn_image_0000001579996808.png)


### 配置Android SDK安装目录环境变量

配置环境变量ANDROID_HOME，设置Android SDK安装目录。

- Windows环境变量设置方法：
  在**此电脑 &gt; 属性 &gt; 高级系统设置 &gt; 高级 &gt; 环境变量**中，新建系统变量。变量名为ANDROID_HOME，变量值为Android SDK安装目录。

  ![zh-cn_image_0000001578322442](figures/zh-cn_image_0000001578322442.png)

  环境变量配置完成后，关闭并重启DevEco Studio。

- macOS环境变量设置方法：
  1. 打开终端工具，执行以下命令，打开.bash_profile文件。
        
      ```
      vi ~/.bash_profile
      ```
  2. 单击字母“i”，进入**Insert**模式。
  3. 输入以下内容，配置Android SDK安装目录。
        
      ```
      export ANDROID_HOME=/Users/xxx/Library/Android/sdk
      ```
  4. 编辑完成后，单击**Esc**键，退出编辑模式，然后输入“:wq”，单击**Enter**键保存。
  5. 执行以下命令，使配置的环境变量生效。
        
      ```
      source ~/.bash_profile
      ```
  6. 环境变量配置完成后，关闭并重启DevEco Studio。
## 开发跨平台应用


### 创建工程

Deveco Studio提供ArkUI-X模板快速创建跨平台工程，支持基于Stage模型HarmonyOS应用生成跨平台Android和iOS应用。

1. 根据工程创建向导，选择创建Application，选择ArkUI-X模板**[ArkUI-X] Empty Ability**，点击**Next**。
   ![zh-cn_image_0000001580417686](figures/zh-cn_image_0000001580417686.png)

2. 在工程配置页面，填写工程的基本信息，点击**Finish**。
   ![zh-cn_image_0000001630496961](figures/zh-cn_image_0000001630496961.png)

为了您开发应用的良好体验，DevEco Studio提供了ArkUI-X开发环境诊断的功能，帮助您识别开发环境是否完备。创建工程完成后，会进行环境检查，如果环境配置有问题，会弹窗提示环境检查不通过，请根据具体的错误项进行检查并完成相应的配置。

如弹窗提示Android SDK检查结果报错，请确认环境变量是否已完成配置。![zh-cn_image_0000001642806197](figures/zh-cn_image_0000001642806197.png)


### 工程目录结构

  
```
MyApplication 
├── .arkui-x
│   ├── android
│   │   ├── app
│   │   │   ├── libs
│   │   │   │   ├── ace_android.jar                       // ArkUI-X跨平台适配层，在SDK中发布
│   │   │   │   ├── ace_ohos_${module-name}.jar           // API扩展库，在SDK中发布
│   │   │   │   └── arm64-v8a
│   │   │   │       ├── libace_android.so                 // ArkUI-X跨平台引擎库，在SDK中发布
│   │   │   │       └── libace_${module-name}_${submodule-name}.so     // API扩展库，在SDK中发布
│   │   │   ├── src
│   │   │   │   ├── androidTest
│   │   │   │   ├── main
│   │   │   │   │   ├── assets                            // 源码和资源编译后的JSBundle和资源文件，作为平台资源存放在assets中
│   │   │   │   │   │   └── arkui-x
│   │   │   │   │   │       ├── entry                     // JSBundle&resources资源
│   │   │   │   │   │       │   ├── ets
│   │   │   │   │   │       │   │   ├── Application
│   │   │   │   │   │       │   │   ├── MainAbility
│   │   │   │   │   │       │   │   └── pages
│   │   │   │   │   │       │   ├── resources.index       // 应用资源
│   │   │   │   │   │       │   ├── resources
│   │   │   │   │   │       │   └── module.json
│   │   │   │   │   │       ├── feature1
│   │   │   │   │   │       ├── feature2
│   │   │   │   │   │       ├── entryTest                 // ohosTest，仅Debug模式构建包含
│   │   │   │   │   │       └── systemres                 // OpenHarmony系统资源
│   │   │   │   │   ├── java/com/example/myapplication
│   │   │   │   │   │   ├── EntryEntyAbilityActibity.java // 基于AceActivity扩展EntryActivity
│   │   │   │   │   │   └── MyApplication.java            // 基于AceApplication扩展MainActivity
│   │   │   │   │   ├── res
│   │   │   │   │   └── AndroidManifest.xml
│   │   │   │   └── test
│   │   │   ├── build.gradle
│   │   │   └── proguard-rules.pro
│   │   ├── gradle
│   │   ├── build.gradle
│   │   ├── gradle.properties
│   │   ├── gradlew
│   │   ├── gradlew.bat
│   │   └── settings.gradle
│   └── ios
├── .hvigor
├── .idea
├── AppScope                                              // 应用的全局配置信息
├── entry[src/main]                                       // HarmonyOS应用的源码
│   └── src
│       └── main
│           ├── ets
│           │   ├── entryability                          // 应用入口
│           │   │   └── EntryAbility.ets
│           │   └── pages                                 // 应用包括的页面
│           │       └── Index.ets
│           ├── resources                                 // 用于存放应用所用到的资源文件
│           └── module.json5
├── hvigor
├── oh_modules
├── build-profile.json5                                   // 应用级配置信息，包括签名、产品配置等
├── hvigorfile.ts                                         // 应用级编译构建任务脚本
├── hvigorw
├── hvigorw.bat
├── local.properties
└── oh-package.json5
```


### 编译构建生成跨平台应用

DevEco Studio可打包生成不同平台的应用包，在Windows平台上同时生成可运行在HarmonyOS设备上的app包和运行在Andorid设备上的apk包，在macOS平台上还将生成可运行在iOS设备上的app包。

1. 在主菜单栏，单击**Build &gt; Build Hap(s)/APP(s) &gt; Build APP(s)**。
   ![zh-cn_image_0000001580152768](figures/zh-cn_image_0000001580152768.png)

   构建完成后，在 **.arkui-x &gt; android &gt; app &gt; build &gt; outputs** 目录下生成可运行在Andorid设备上的apk包。如果是使用macOS平台，还将在ios目录下构建出可运行在iOS设备上的app包。

   ![zh-cn_image_0000001580312688](figures/zh-cn_image_0000001580312688.png)

   在**build**目录下生成可运行在HarmonyOS设备上的app包。

   ![zh-cn_image_0000001630032705](figures/zh-cn_image_0000001630032705.png)

2. 在Android Studio打开android目录，通过Android Studio将构建产物推包到Android真机设备上，查看应用运行效果。
## 开发跨平台依赖包


基于HarmonyOS应用，构建Android和iOS应用的依赖包。依赖包支持添加到已有的Android和iOS应用中。


### 创建Library

1. 根据工程创建向导，选择创建Application，选择所需要的ArkUI-X模板 **[ArkUI-X] Library**，点击**Next**。
   ![zh-cn_image_0000001624998937](figures/zh-cn_image_0000001624998937.png)

2. 在工程配置页面，填写工程的基本信息，点击**Finish**。
   ![arkUI-x-属性配置](figures/arkUI-x-属性配置.PNG)


### 编译构建生成跨平台依赖包

基于HarmonyOS应用，DevEco Studio可打包生成不同平台的依赖包，在Windows平台上同时生成集成Andorid应用的aar包，在macOS平台上还将生成集成在iOS应用的.xcframwork包。

在主菜单栏，单击**Build &gt; Build Hap(s)/APP(s) &gt; Build APP(s)**。

![zh-cn_image_0000001580313452](figures/zh-cn_image_0000001580313452.png)

构建完成后，在 **.arkui-x &gt; android &gt; library &gt; build &gt; outputs**目录下生成可集成在Andorid应用上的aar包。如果是使用macOS平台，还将ios目录下构建出可集成在iOS应用上的.xcframwork包。

![zh-cn_image_0000001579994524](figures/zh-cn_image_0000001579994524.png)

