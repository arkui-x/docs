# ArkUI-X Application Project Structure

The ArkUI-X application directory provides a set of application project templates for ArkUI-X developers to build OpenHarmony, Android, and iOS applications. The directory structure of cross-platform application project layer 0 is as follows:

```
ArkUI-X AppProject
  ├── ohos              // OpenHarmony code 0-1
  │   └── entry
  ├── android           // Android code 0-2
  │   └── app
  ├── ios               // iOS code 0-3
  │   └── app
  └── source            // ArkUI source code 0-4
      └── entry
```

The project root directory contains the **ohos**, **android**, **ios**, and **source** directories for OpenHarmony applications, Android applications, iOS applications, and ArkUI source code, respectively. The **entry** and **app** directories in each directory are directories for created modules (**entry** and **app** are default module names). Each module corresponds to a build unit (HAP, APK, or app). **source** is the default directory of OpenHarmony. It contains common code in the ArkTS-based declarative development paradigm. The command code and the OS platform code build applications for the target platform. You can use the ArkTS-based declarative development paradigm or JS-compatible web-like development paradigm.

* OpenHarmony platform project structure (0-1)

```
OpenHarmony platform code
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

* Android platform project structure (0-2)

```
Android platform code
  ├── app
  │   ├── libs
  │   │   ├── ace_android_adapter.jar               // ArkUI cross-platform adaptation layer, published in the SDK
  │   │   └── arm64-v8a
  │   │       ├── libace_android.so                 // ArkUI engine library, published in the SDK
  │   │       ├── libace_napi.so                    // API extension library, published in the SDK
  │   │       ├── libace_napi_ark.so            // JS engine library, published in the SDK
  │   │       └── libxxx.so                         // Other functional module library
  │   ├── src
  │   │   ├── androidTest
  │   │   ├── main
  │   │   │   ├── assets                            // Resource files, including the JS bundle and resources generated after ArkUI compilation
  │   │   │   │   ├── js
  │   │   │   │   │   └── entry_MainAbility         // JS bundle, generated after the compilation of the code in the source directory
  │   │   │   │   └── res                           // Resources
   │   │   │   │       ├── appres                    // Application resources, generated after the compilation of the code in the source/resources directory
  │   │   │   │       └── systemres                 // System resources
  │   │   │   ├── java/com/example/myapp
  │   │   │   │   ├── MyApplication.java            // MyApplication extended from AceApplication
  │   │   │   │   └── MainActivity.java             // MainActivity extended from AceActivity
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

* iOS platform project structure (0-3)

```
iOS platform code
  ├── myapp.xcodeproj
  │   ├── project.xcworkspace
  │   └── project.pbxproj
  ├── myapp
  │   ├── Assets.xcassets
  │   ├── base.Iproj
  │   ├── AppDelegate.h
  │   ├── AppDelegate.mm              // AceViewController instance, loaded with the JS bundle and resources
  │   ├── Info.plist
  │   └── main.m
  ├── js                              // ArkUI JS bundle, generated after compilation of the ArkUI source code in the source directory
  │   └── entry_MainAbility
  ├── res                             // Resources
  │   ├── appres                      // Application resources, generated after the compilation of the code in the source/resources directory.
  │   └── systemres                   // System resources
  └── framework                       // ArkUI cross-platform framework dynamic library
      └── libace_ios.xcframework
```

* ArkUI source code directory (0-4)

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
