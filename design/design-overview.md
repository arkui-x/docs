# ArkUI跨平台设计总体说明

## 1.简介

**1.1** **范围**

本文档描述ArkUI（即Openharmony下的ACE子系统）跨平台运行能力相关的总体技术方案。

ArkUI是面向全设备的UI开发框架. 已通过OpenHarmony代码仓开放，其关键组成包括：1.开发模型; 2.应用界面&交互; 3.扩展机制-使能三方组件&平台API扩展机制。

ArkUI跨平台项目则是扩展ArkUI开发框架到其他OS平台-Android/iOS/Windows等，让开发者基于ArkUI，复用绝大部分的应用代码（UI以及主要应用逻辑），并可以部署到相应的OS平台上。

 **1.2** **假设和约束**

本文提到的跨平台是指UI部分的跨平台，其UI代码可以重用，其它应用对平台依赖的能力，需要应用层面进行适配，或通过JS API封装机制（NAPI），进行实现暴露到JS层。

涉及平台能力的JS API（比如网络、存储等）请参考openharmony中的定义，需要通过在不同平台的封装实现达到复用的目的。

本文主要是通用方案设计说明，并以Android平台做为示例说明，其他平台的基本设计思路类似，但相关设计需进一步细化补充。

## 2.总体视图 

 ![](.\png\arkui-overview.png)

 

从设计之初，**跨平台**就作为ArkUI最基本的设计目标之一，当前已支持基础的跨平台架构.相关的设计思路如下：

1. 采用**C++**编写整体后端引擎代码，保持在多平台的可移植性，最小化平台依赖，降低平台移植成本
2. 整体绘制采用自渲染机制，降低平台依赖，同时进一步提升绘制效果的一致性
3. 抽象出平台适配层以及平台桥接层，以便不同平台的适配

## 3. 模块功能介绍 

ArkUI主要包括以下几个模块：

1. 前端框架层， 包括JavaScript前端框架，JS UI组件等，可跨平台
2. 声明式UI后端引擎，包括布局，渲染，C++ UI组件，事件机制等，可跨平台
3. API扩展机制，基于NAPI机制，可跨平台。 不同平台需要各自扩展具体的API实现
4. 工具链/SDK,  工具链可跨平台，SDK需基于不同平台构建

另外，ArkUI依赖的JS/TS引擎以及图形引擎，也可跨平台。

ArkUI声明式UI后端引擎，主要完成整体pipeline流程控制、视图更新、布局系统、多页面管理、事件分发和回调、焦点管理、动画机制、主题机制、资源管理/缓存/provider等。 其中的UI组件，主要通过显示相关组件细粒度化，动画、事件、焦点等机制组件化，满足适配不同前端所需要的灵活性。

整体的跨平台需求，就是扩展ArkUI开发框架到其他OS平台，帮助开发者降低多平台应用开发成本。

以Android平台为例，提供Android平台的SDK及工具，可以让开发者同时构建出Openharmony版本hap及Android版本的apk。

![](.\png\arkui-deploy.png)



## 4.方案设计

 **4.1** **跨平台应用结构设计**

跨平台应用目录结构，包含一套为 ArkUI 开发者提供的工程应用模板，提供打包openharmony hap和以及其他OS应用包的能力。

├── android // Android平台相关代码

│  ├── entry

│  ├── module1

│  └── module2

└── source // OHOS以及应用公共代码

  ├── entry

├── module1

└── module2

以Android跨平台工程为例，首层结构如下，设计为：项目根目录下放android、source目录，分别对应安卓应用、源代码的资源文件：

每个目录下的entry、module目录表示创建的模块（entry为默认创建的模块），每个模块是最终打包的单位。

首先介绍Android平台目录的结构，整体如下：

```
android
├── build.gradle
├── entry
│   ├── build.gradle
│   ├── libs
│   │   ├── ace_android_adapter.jar   // ArkUI的跨平台适配库，在SDK中发布
│   │   └── arm64─v8a
│   │       │── libace_android.z.so   // ArkUI的引擎库，在SDK中发布
│   │       └── libace_napi_android.z.so      // ArkUI的NAPI接口扩展库，在SDK中发布
│   └── src
│       └── main
│           ├── AndroidManifest.xml
│           ├── assets
│           ├── java
│           │   └── com
│           │       └── example
│           │           └── myapp
│           │               ├── LoginActivity.java
│           │               └── MyApplication.java
│           └── res
├── gradle
├── gradle.properties
├── gradlew
├── module1
└── module2
```

 外部的gradle相关文件用于配置编译Android平台所需的配置文件。以entry为例介绍每个module下的结构：entry下的build.gradle用于配置app的编译，并配置Android SDK的路径，libs下存放使用的三方库以及ACE的跨平台相关的库，src是一些代码及资源文件，包括assets、java代码文件、资源res和配置文件AndroidManifest.xml。应用生成的bundle文件编译后会拷贝到assets/js目录下。

source目录是OpenHarmony默认的结构，存放公共的代码，可以独立构建出OpenHarmony的应用，也能配合上述平台的代码构建出对应平台的应用。可以使用基于TS扩展的声明式范式或基于JS扩展的类Web范式进行开发。



基于TS扩展的声明式范式的entry模块的工程目录结构设计如下：

```
source
├── build.gradle
├── entry // 模块入口
│   └── src
│       └── main
│           ├── config.json  // OHOS配置文件
│           ├── cpp   // C++接口扩展
│           │   └── CMakeLists.txt
│           ├── ets   // UI逻辑核心代码
│           │   ├── Login
│           │   │   ├── android   // Android平台TS入口
│           │   │   │   ├── LoginActivity.ts
│           │   │   │   └── LogoutActivity.ts
│           │   │   ├── ohos   // openharmony平台TS入口
│           │   │   │   ├── LoginAbility.ts
│           │   │   │   └── LogoutAbility.ts
│           │   │   └── pages
│           │   │       ├── index.ets
│           │   │       └── second.ets
│           │   └── MyAbilityStage.ts
│           └── resources
│               ├── base
│               │   ├── element
│               │   │   └── string.json
│               │   └── media
│               │       └── icon.png
│               └── rawfile
├── gradle
├── gradle.properties
├── gradlew
├── module1
└── module2
```



基于JS扩展的类Web范式的跨平台应用的工程目录结构设计如下：

```
 source
├── build.gradle
├── entry
│   └── src
│       └── main
│           ├── config.json
│           ├── cpp
│           │   └── CMakeLists.txt
│           ├── js  // UI逻辑核心代码
│           │   └── Login
│           │       ├── app.js  // js模块入口
│           │       ├── common  // 页面共享资源
│           │       │   ├── logo.png
│           │       │   └── style.css
│           │       ├── i18n   // 国际化字符资源
│           │       │   ├── en─US.json
│           │       │   └── zh─CN.json
│           │       └── pages  // 页面代码
│           │           └── index
│           │               ├── index.css
│           │               ├── index.hml
│           │               └── index.js
│           └── resources
│               ├── base
│               │   ├── element
│               │   │   └── string.json
│               │   └── media
│               │       └── icon.png
│               └── rawfile
├── gradle
├── gradle.properties
├── gradlew
├── module1
└── module2
```

 

**4.2** 跨平台SDK构建

跨平台SDK的编译：

OHOS的SDK由OpenHarmony代码可直接构建出。

ArkUI for Android的SDK由OpenHarmony源码 + 用户下载的Android SDK + NDK构建。(依赖编译构建子系统提供java编译以及android_dpes的能力)

ArkUI for Android的SDK中包含的内容有：

**ace_android_adapter.jar :** 包含安卓系统能力适配层的代码以及安卓运行环境的适配

**libace_android.z.so** ：ArkUI核心引擎库

**libace_napi_android.z.so**：ArkUI NAPI桥接机制库

按照最新的分仓架构，adapter/aosp作为独立仓，在Android版本上使用aosp和ArkUI核心仓即可构建上述内容

![](.\png\arkui-repo-structure.png) 

**4.3** **操作系统抽象层**

基于C++实现的OS Abstract Layer，屏蔽不同平台的OS相关的实现，主要包含功能列表：

- 日志、Trace抽象层
- 网络接口抽象层
- 文件/资源读写抽象层
- 基础线程抽象层
- 系统资源管理抽象及实现
- 系统Prop配置读取抽象层
- 打点能力抽象层

以Log调用流程为例，整体交互流程如下：

![](.\png\arkui-log-sequence.png)

 

如上述流程，Core模块直接使用Base提供的接口，Base模块对接口进行定义，对于依赖平台的能力，在编译期就选择了对应OS的平台抽象层OSAL。运行时，直接通过OSAL的实现，调用到具体平台相关的库中。

 

**4.4** **跨平台启动入口**

对应平台语言实现的Entrance，提供不同平台的基础入口环境，功能列表：

提供多个平台的加载入口，如OpenHarmony侧为一个Ability，Android侧为一个Activity；

声明式范式抽象Android的TS入口接口

对接不同平台的生命周期、事件输入、Vsync；

对接不同平台的窗口系统、硬件渲染加速；

对接不同平台的应用信息

不同平台的原生语言转换到统一的C++后端，公共代码复用；

以Android的启动流程为例，整体交互流程如下：

![](.\png\arkui-startup-android.png)

 

 

**4.5** **跨平台能力桥接**

跨平台能力桥接包含框架内部需要用到的不同的平台能力模块，如：剪切板、输入法、视频等，提供基础能力模块的定义，不同平台按照定义实现对应的功能模块。功能列表：

- 剪切板抽象接口，及不同平台的实现；
- 输入法抽象接口，及不同平台的实现；
- 视频媒体、Camera抽象接口，及不同平台的实现；
- WebView抽象接口，及不同平台的实现；
- RichText抽象接口，及不同平台的实现；
- XComponent不同平台的实现；
- 其它框架内部需要用到的不同的平台能力模块

以剪切板ClipBoard为例，整体交互流程如下：

![](.\png\arkui-clipboard.png)



如上图，例如在框架核心层的TextField组件中，需要用到剪切板的能力，通过Proxy创建具体的ClipBoard实现，返回抽象的ClipBoard接口。在组件实现层即可实现平台无关的调用。以Android平台为例，GetData的调用会通过JNI调用到平台实现的Plugin中，然后Plugin通过访问剪切板服务实现对应的功能。

 

**4.6 API扩展机制**



1、JS API的通用封装机制，提供多JS引擎抽象封装，用于Native接口能力暴露到JS层的统一封装机制该能力直接复用OHOS上的统一封装机制，扩展API （C++实现），并实现部分内置API，NAPI的整体结构如下图。

对于不同平台，**JS API需要遵循OHOS的API定义**，在不同平台上通过NAPI的扩展机制进行扩展。

![](.\png\arkui-api-extension.png) 

NAPI需求列表如下：

NAPI的Android平台编译

支持应用通过NAPI扩展接口，并通过apk运行时动态加载

扩展部分内置API，支持apk运行时动态加载 (按照OHOS的API定义实现)

NAPI快速扩展Android平台系统API的能力（JS-&gt;C++-&gt;Java）

 

**4.7** **跨平台命令行工具**

ArkUI CLI，是一套为ArkUI开发者提供的命令行工具，包括开发环境检查，新建项目，编译打包，安装调试

目录结构介绍：

```
.
│  .eslintrc            // eslint 插件配置文件
│  LICENSE
│  NOTICE
│  package.json         // ace_tools 配置文件
│  README.md
│  rollup.config.js     // rollup 打包插件配置文件
│
│
├─dist    // 使用 rollup 打包后生成
|      ace
│      ace.cmd
│      ace.ps1
│      ace_tools.js    // 项目入口文件
│      template        // 工程模板
│      ace_tools.js    // 打包后的 ace_tools.js
│      ace_tools.js.map
│
└─src    // 功能实现的代码目录
    ├─ace-build    // 实现 ace build 命令，编译打包项目
    │  │  ace-build.js
    │  │  index.js
    │  │
    │  ├─ace-compiler
    │  │      index.js
    │  │
    │  └─ace-packager
    │          index.js
    │
    ├─ace-check    // 实现 ace check 命令，检查 ace 应用需要依赖的库和工具链
    │      checkDevices.js
    │      checkJavaSdk.js
    │      checkNodejs.js
    │      configs.js
    │      getTool.js
    │      Ide.js
    │      index.js
    │      Info.js
    │      platform.js
    │      Sdk.js
    │      util.js
    │
    ├─ace-config    // 实现 ace config 命令，配置 ace 的设置，包括openharmony sdk 的路径，编译输出的路径，开发证书路径等
    │      index.js
    │
    ├─ace-create    // 实现 ace create 命令，创建一个 ace 应用工程，或 module，或 component
    │  ├─component
    │  │      index.js
    │  │
    │  ├─module
    │  │      index.js
    │  │
    │  ├─project
    │  │      index.js
    │  │
    │  └─template    // 模板
    │      │  template.json
    │      │
    │      ├─app    // 1.0 工程模板：android, ohos, source
    │      │  ├─android
    │      │  │
    │      │  ├─ohos
    │      │  │
    │      │  └─source
    │      │
    │      ├─appv2    // 2.0 工程模板：android, ohos, source
    │      │  ├─android
    │      │  │
    │      │  ├─ohos
    │      │  │
    │      │  └─source
    │      │
    │      ├─module_android   // android 的 module 模板
    │      │
    │      └─module_ohos    // ohos 的 module 模板
    │
    ├─ace-devices    // 实现 ace devices 命令，列出所有连接的设备
    │      index.js
    │
    ├─ace-install    // 实现 ace install 命令，将 ace 应用安装到连接的设备上
    │      index.js
    │
    ├─ace-launch    // 实现 ace launch 命令，在设备上启动 ace 应用
    │      index.js
    │
    ├─ace-log       // 实现 ace log 命令，滚动展示正在运行的 ace 应用的 log
    │      index.js
    │
    ├─ace-run       // 实现 ace run 命令，编译并在设备上运行 ace 应用
    │      index.js
    │
    ├─ace-uninstall    // 实现 ace uninstall 命令，将 ace 应用卸载
    │       index.js
    │
    └─bin
           ace      // 脚本文件，通过 ace 命令来执行框架内业务逻辑
           ace.cmd
           ace.ps1
           ace_tools.js    // 项目入口文件
```

 

**5.** **任务列表：**



| 编号 | 描述                                                         | 状态 |
| ---- | ------------------------------------------------------------ | ---- |
| 1    | 【开发环境】明确ArkUI的跨平台运作方式，开源社区建立相关的仓：ace_build、ace_android_adapter、ace_command_tool |      |
| 2    | 【开发环境】新建Ace的Manifest.xml配置，配置依赖的代码仓，可以拉取跨平台完整代码 |      |
| 3    | 【开发环境】跨平台编译构建环境，可编译android平台jar包及so   |      |
| 4    | 【构建工具】命令行工具最小原型(Android + OHOS)，包含工具链配置、创建工程、构建工程 |      |
| 5    | 【启动入口】提供Android平台的加载入口，Android侧为一个Activity，加载ACE的Native环境； |      |
| 6    | 【启动入口】对接Android平台的生命周期、事件输入、Vsync       |      |
| 7    | 【启动入口】对接Android平台的窗口系统、硬件渲染加速          |      |
| 8    | 【启动入口】对接Android平台的应用信息                        |      |
| 9    | 【NAPI机制】NAPI的Android平台编译                            |      |
| 10   | 【启动入口】声明式范式抽象Android的TS入口接口                |      |
| 11   | 【系统抽象层】Android平台日志、Trace、打点、线程抽象层       |      |
| 12   | 【系统抽象层】Android平台系统Prop配置对接                    |      |
| 13   | 【系统抽象层】Android平台文件/资源读写抽象层                 |      |
| 14   | 【系统抽象层】Android平台网络接口抽象层                      |      |
| 15   | 【系统抽象层】Android平台系统资源管理抽象及实现              |      |
| 16   | 【NAPI机制】扩展部分内置API，支持apk运行时动态加载           |      |
| 17   | 【能力桥接】Android平台剪切板能力桥接                        |      |
| 18   | 【能力桥接】Android平台输入法能力桥接                        |      |
| 19   | 【能力桥接】Android平台Video能力桥接                         |      |
| 20   | 【能力桥接】Android平台Camera能力桥接                        |      |
| 21   | 【能力桥接】Android平台WebView能力桥接                       |      |
| 22   | 【能力桥接】Android平台RichText能力桥接                      |      |
| 23   | 【能力桥接】Android平台XComponent能力桥接                    |      |
| 24   | 【NAPI机制】支持应用通过NAPI扩展接口，并通过apk运行时动态加载 |      |
| 25   | 【NAPI机制】NAPI快速扩展Android平台系统API的能力（JS-&gt;C++-&gt;Java） |      |
| 26   | 【构建工具】命令行工具-config                                |      |
| 27   | 【构建工具】命令行工具-check                                 |      |
| 28   | 【构建工具】命令行工具-devices                               |      |
| 29   | 【构建工具】命令行工具-create                                |      |
| 30   | 【构建工具】命令行工具-build                                 |      |
| 31   | 【构建工具】命令行工具-install                               |      |
| 32   | 【构建工具】命令行工具-uninstall                             |      |
| 33   | 【构建工具】命令行工具-launch                                |      |
| 34   | 【构建工具】命令行工具-log                                   |      |
| 35   | 【构建工具】命令行工具-run                                   |      |
| 60   | 【iOS平台适配】                                              |      |
| 70   | 【Windows平台适配】                                          |      |
| 80   | 【Mac平台适配】                                              |      |
| 90   | 【Linux平台适配】                                            |      |
| 100  | 【OHOS JS API能力构建】ohos中各子系统的JS API在不同OS平台上的适配实现 |      |
| 200  | 【性能、内存、BinarySize优化】                               |      |

