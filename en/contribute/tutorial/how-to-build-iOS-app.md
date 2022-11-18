## Building ArkUI Applications on iOS

#### Introduction

This tutorial describes how to use the ArkUI-X SDK to develop iOS applications, thereby implementing the display of the ArkTS-based declarative development paradigm on iOS. It covers the following contents:

* Obtaining the ArkUI-X SDK
* ArkUI-X SDK Components
* Integrating the ArkUI-X SDK into an iOS Project
* Integrating an ArkUI JS Bundle Instance into an iOS Project

##### Obtaining the ArkUI-X SDK

**Method 1**

* Obtain the ArkUI-X SDK from the mirror site. This method is suitable for beginners of ArkUI cross-platform applications.

| Source Code            | Version| Mirror| SHA-256 Checksum|
| -------------------- | ------------ | ------------ | ---------------- |
| ArkUI-X SDK (iOS)| 0.1.0 Beta   | [Download]()    | [Download]()|

**Method 2**

* Build the SDK using the source code of the ArkUI-X project. This method is applicable to high-level developers of ArkUI cross-platform applications. After [downloading the source code](../../application-dev/quick-start/README.md), run the following command to build the iOS-based ArkUI-X SDK:

```
./build.sh --product-name arkui-cross --target-os ios
```

##### ArkUI-X SDK Components

The iOS-based ArkUI-X SDK provides two frameworks: **libace_ios.framework** and **libace_ios.xcframework**. The header of the framework files contains **Ace.h**, **AceViewController.h**, and **FlutterPlugin.h**, where **AceViewController** implements ArkUI startup and JS bundle loading.

```
ArkUI_iOS_SDK
    ├── libace_ios.framework                 // ArkUI iOS framework
    │   ├── Headers                          // ArkUI framework header files
    │   │   ├── Ace.h                          
    │   │   ├── AceViewController.h   
    │   │   └── FlutterPlugin.h        
    |   ├── info.plist
    │   ├── libace_ios                       // Binary executable file
    |   ├── libace_ios.podspec               // CocoPods configuration file
    |   ├── LICENSE
    │   └── Modules
    │       └── module.modulemap            
    └── libace_ios.xcframework/ios-arm64/libace_ios.framework    // ArkUI iOS xcframework           
        ├── Headers                          // ArkUI framework header files
        │   ├── Ace.h            
        │   ├── AceViewController.h      
        │   └── FlutterPlugin.h            
        ├── info.plist
        ├── libace_ios                       // Binary executable file
        ├── libace_ios.podspec               // CocoPods configuration file
        ├── LICENSE
        └── Modules
            └── module.modulemap  
```

##### Integrating the ArkUI-X SDK into an iOS Project

The integration complies with the integration framework rules of iOS application projects. Specifically, you need to copy and import the **libace_ios.xcframework** package in the SDK to the project directory.

* Instantiate **AceViewController** in **AppDelegate.mm** and load the ArkUI page.

```
 #import <libace_ios/Ace.h>

 AceViewController *controller = [[AceViewController alloc] initWithVersion:(ACE_VERSION_ETS) instanceName:@"MainAbility"];
 controller.view.frame = [UIScreen mainScreen].bounds;
 [self.view addSubview:controller.view];

```

The **version** parameter specifies the ArkUI development paradigm, which can be **ACE_VERSION_JS** (JS-compatible web-like development paradigm) or **ACE_VERSION_ETS** (ArkTS-based declarative development paradigm). **instanceName** specifies the name of the directory where the ArkUI paradigm code is stored in the application project after being built into the JS bundle.

##### Integrating an ArkUI JS Bundle Instance into an iOS Project

Copy the generated ArkUI JS bundle to the **assets/js** directory of the iOS project, for example, **js/jsdemo**. The **js** directory name is fixed and cannot be changed.

```
iOS application project
├── myapp.xcodeproj
├── myapp
│   ├── Assets.xcassets
│   ├── Base.Iproj
│   ├── AppDelegate.h
│   ├── AppDelegate.mm
│   ├── Info.plist
│   └── main.m
├── js
│   └── MainAbility
│       ├── app.js
│       ├── manifest.json
│       └── pages
└── frameworks
    └── libace_ios.xcframework
```


##### Special Notes
**Modifying a Project**
In versions later than Xcode 11, when an iOS project of the **SceneDelegate** type is created, you must delete the SceneDelegate-related content.
1. Delete the **Application Scene Manifest** option from **Info.plist**.
2. Delete the **SceneDelegate** code file or comment out all code in the file.
3. Delete or comment out the **Scene** method in **AppDelegate**.

**Debugging Using a Real Device**
1. Configure **Signing** in **Target signing & capabilities**.
2. In **Target General**, set **Frameworks, Libraries, and Embedded Content** to **Embed &Sign**.
3. In **Target Building Settings**, set **Build Options Enable Bitcode** to **No**.

**Debugging Using an Emulator**
1. Add the ArkUI cross-platform framework corresponding to the emulator version.
2. In **Target General**, set **Frameworks, Libraries, and Embedded Content** to **Do Not Embed**.

Now you can build an ArkUI iOS application according to the iOS application building process.


##### References

[1] [Obtaining ArkUI JS Bundle]()