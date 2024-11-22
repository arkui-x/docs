# ArkUI-X Application Project Structure

# Overview

ArkUI-X extends the OpenHarmony ArkUI development framework to different OS platforms like Android and iOS. This way, you can reuse most of the application code (UI and main application logic) based on ArkUI and deploy the code on the corresponding OS platform.

# Application Project Directory Structure

The ArkUI-X application project directory contains a set of application project templates, which you can leverage to build OpenHarmony, HarmonyOS, Android, and iOS applications.

```
Directory structure of the ArkUI-X application project
  ├── .arkui-x
  │   ├── android                 // Android-related code
  │   └── ios                     // iOS-related code
  ├── AppScope
  ├── entry
  ├── hvigor
  ├── build-profile.json5
  ├── hvigorfile.ts
  ├── hvigorw
  ├── hvigorw.bat
  ├── local.properties
  └── oh-package.json5
```

As can be seen from the preceding structure, the ArkUI-X application directory has platform-specific application projects added to the OpenHarmony application project. You build ArkTS code and resources on the OpenHarmony side, and build native code in the respective Android or iOS application project.

# Compilation and Building

* ArkTS source code

Use the OpenHarmony SDK toolchain to generate Ark Byte Code (abc) from the ArkTS source code. Then, copy the code to the Android and iOS application projects and manage it as platform application resources.

* ArkUI application resources

Use the OpenHarmony SDK toolchain to compile ArkUI resources. Then, copy the compiled ArkUI resources to the Android and iOS application projects and manage them as platform application resources.

* ArkUI Framework Resources

The ArkUI framework resources are released with the ArkUI-X SDK. During application building, the resources are automatically packaged into the ArkUI-X application, which ensures the UX rendering consistency of the ArkUI-X application across platforms.

To sum up, ArkTS compilation products, ArkUI application resources, and ArkUI framework resources are managed in **assets** on the Android platform and in **Bundle Resources** on the iOS platform.

## Android Application Project Structure

```
ArkUI-X Android application project
├── app
│   ├── libs
│   │   ├── arkui_android_adapter.jar                   // ArkUI-X cross-platform adaptation layer, which is released in the SDK
│   │   └── arm64-v8a
│   │       └── libarkui_android.so                     // ArkUI-X cross-platform engine library, which is released in the SDK
│   │       └── libhilog.so                             // ArkUI-X log library, which is released in the SDK
│   │       └── libresourcemanager.so                   // ArkUI-X resource management library, which is released in the SDK
│   ├── src
│   │   ├── androidTest
│   │   ├── main
│   │   │   ├── assets
│   │   │   │   └── arkui-x                             // Byte code file and resources generated after compilation
│   │   │   │       ├── entry                           // Compilation result of a single ArkUI module
│   │   │   │       │   ├── ets                         // Compilation result of a single ArkUI module: byte code and sourceMap files
│   │   │   │       │   │   ├── sourceMaps.map
│   │   │   │       │   │   └── modules.abc
│   │   │   │       │   ├── resources.index             // Compilation result of a single ArkUI module: resources
│   │   │   │       │   ├── resources                   // rawfile resources, which are not compiled
│   │   │   │       │   └── module.json
│   │   │   │       ├── entry_test                      // ohosTest, included only in the debug mode
│   │   │   │       └── systemres                       // System resources provided by the ArkUI framework
│   │   │   ├── java/com/example/mayapp
│   │   │   │   ├── MyApplication.java                  // MyApplication extended based on the StageApplication
│   │   │   │   └── EntryEntryAbilityActivity.java      // EntryEntryAbilityActivity extended based on the StageActivity
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

## iOS Application Project Structure

```
ArkUI-X iOS application project
├── app.xcodeproj
│   ├── project.xcworkspace
│   ├── xcuserdata
│   └── project.pbxproj
├── app
│   ├── Assets.xcassets
│   ├── Base.Iproj
│   ├── AppDelegate.h
│   ├── AppDelegate.m                               // Application entry that drives the lifecycle of the StageApplication
│   ├── EntryEntryAbilityViewController.h           
│   ├── EntryEntryAbilityViewController.m           // EntryEntryViewController extended based on the StageViewController
│   ├── Info.plist
│   └── main.m
├── arkui-x                                         // Byte code file and resources generated after compilation
│   ├── entry                                       // Compilation result of a single ArkUI module
│   │   ├── ets                                     // Compilation result of a single ArkUI module: byte code and sourceMap files
│   │   │   ├── sourceMaps.map
│   │   │   └── modules.abc
│   │   ├── resources.index                         // Compilation result of a single ArkUI module: resources
│   │   ├── resources                               // rawfile resources, which are not compiled
│   │   └── module.json
│   ├── entry_test                                  // ohosTest, included only in the debug mode
│   └── systemres                                   // System resources provided by the ArkUI framework
└── frameworks                                      // ArkUI cross-platform framework dynamic libraries, including the ArkUI-X framework and plug-ins
```
