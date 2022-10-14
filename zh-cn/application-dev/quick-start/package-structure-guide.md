# ArkUI跨平台应用工程结构

ArkUI跨平台应用目录结构，包含一套为ArkUI-X开发者提供的应用工程模板，提供构建OpenHarmony应用、Android应用、iOS应用的能力。跨平台应用工程0层结构设计如下：

```
ArkUI-X AppProject
  ├── ohos              // OpenHarmony平台相关代码 0-1
  │   └── entry
  ├── android           // Android平台相关代码 0-2
  │   └── app
  ├── ios               // iOS平台相关代码 0-3
  │   └── app
  └── source            // ArkUI页面源码 0-4
      └── entry
```

项目根目录包含：ohos、android、ios、source四个目录，分别对应OpenHarmony应用、Android应用、iOS应用，ArkUI源码模块。每个目录下的entry和app目录表示创建的模块（entry/app为默认创建的模块名），每个模块对应一个编译单元（hap/apk/app）。其中，source目录是OpenHarmony默认的结构，存放公共的代码，配合上述平台的代码构建出对应平台的应用。可以使用基于eTS的声明式范式或兼容JS的类Web范式进行开发。

* OpenHarmony平台工程结构（0-1）

```
OpenHarmony平台代码
  ├── .hvigor
  ├── entry
  │   ├── src
  │   │   ├── main
  │   │   │   ├── ets
  │   │   │   ├── resources
  │   │   │   └── config.json
  │   │   └── ohosTest
  │   ├── build-profile.json5
  │   ├── hvigorfile.js
  │   └── package.json
  ├── node_modules
  ├── build-profile.json5
  ├── hvigorfile.js
  ├── local.properties
  └── package.json
```

* Android平台工程结构（0-2）

```
Android平台代码
  ├── app
  │   ├── libs
  │   │   ├── ace_android_adapter.jar               // ArkUI跨平台适配层，在SDK中发布
  │   │   └── arm64-v8a
  │   │       ├── libace_android.so                 // ArkUI引擎库，在SDK中发布
  │   │       ├── libace_napi.so                    // API接口扩展库，在SDK中发布
  │   │       ├── libace_napi_quickjs.so            // JS引擎库，在SDK中发布
  │   │       └── libxxx.so                         // 其它功能模块库
  │   ├── src
  │   │   ├── androidTest
  │   │   ├── main
  │   │   │   ├── assets                            // ArkUI编译后的JSBundle和Resources，作为资源文件存放在assets
  │   │   │   │   ├── js
  │   │   │   │   │   └── entry_MainAbility         // JSBundle，来自source目录ArkUI源码编译结果。
  │   │   │   │   └── res                           // resources资源
  │   │   │   │       ├── appres                    // 应用资源，来自source目录resources资源编译结果。
  │   │   │   │       └── systemres                 // 系统资源
  │   │   │   ├── java/com/example/myapp
  │   │   │   │   ├── MyApplication.java            // 基于AceApplication扩展MyApplication
  │   │   │   │   └── MainActivity.java             // 基于AceActivity扩展MainActivity
  │   │   │   ├── res
  │   │   │   └── AndroidManifest.xml
  │   │   └── test
  │   ├── build.gradle
  │   └── proguard-rules.pro
  ├── gradle/wrapper
  ├── build.gradle
  ├── gradle.properties
  ├── gradlew
  ├── gradlew.bat
  └── settings.gradle
```

* iOS平台工程结构（0-3）

```
iOS平台代码
  ├── myapp.xcodeproj
  │   ├── project.xcworkspace
  │   └── project.pbxproj
  ├── myapp
  │   ├── Assets.xcassets
  │   ├── base.Iproj
  │   ├── AppDelegate.h
  │   ├── AppDelegate.mm              // 实例化AceViewController, 并加载JSBundle和Resources资源。
  │   ├── Info.plist
  │   └── main.m
  ├── js                              // ArkUI JSBundle，来自source目录ArkUI源码编译结果。
  │   └── entry_MainAbility
  ├── res                             // Resources资源
  │   ├── appres                      // 应用资源，来自source目录resources资源编译结果。
  │   └── systemres                   // 系统资源
  └── framework                       // ArkUI跨平台Framework动态库
      └── libace_ios.xcframework
```

* ArkUI源码目录（0-4）

```
source
  └── entry/src
      ├── main
      │   ├── ets
      │   │   └── MainAbility
      │   │       ├── app.ets
      │   │       ├── manifest.json
      │   │       └── pages
      │   └── resources
      └── ohosTest
``` 