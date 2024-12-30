# Using ACE Tools to Build a Cross-Platform Application

This tutorial walks you through on how to use ACE Tools to extend the OpenHarmony development paradigm to cross-platform development, including:

* Basic project development
* Native C++ development
* AAR/Framework development
* Multi-ability development
* Multi-module development

## Basic Project Development
* Run the **ace create** command to create a simple project and enter the project name and related information as prompted.
    ```shell
    ohos@user Desktop % ace create demo
   ? Please enter the project name(demo): # Enter the project name. If no project name is entered, the folder name is used by default.
   ? Please enter the bundleName (com.example.demo): # Enter the bundle name. If no bundle name is entered, com.example.projectName is used by default.
   ? Please enter the runtimeOS (1: OpenHarmony, 2: HarmonyOS): 1 # Enter the runtime OS.
   ? The project already exists. Do you want to delete the directory (y / n): y
   Delete directory successfully, creating new project.
   
   Project created successfully! Target directory: ${current directory}/demo.
   
   In order to run your application, type:
   
     $ cd demo
     $ ace run
   
   Your application code is in demo/entry.
   ```
* The structure of the created project is as follows:
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android                   // Android application project
    |   └── ios                       // iOS application project
    ├── AppScope
    ├── build-profile.json5
    ├── entry                         // Application entry module
    ├── hvigor
    ├── hvigorfile.ts
    ├── hvigorw
    ├── hvigorw.bat
    └── oh-package.json5
    ```
* The project structure is the same as the structure of a cross-platform sample project imported via DevEco Studio.
* Run the **ace build** command to build and pack the ArkTS source code. In this way, a cross-platform application is built.
## Native C++ Development
With ACE Tools, you can use native C++ APIs in cross-platform development.
* When creating a project, select **plugin_napi** for the template type.

  **Note**: The iOS project structure remains unchanged. After the iOS project is configured, the **hello.cpp** file in the **source** directory is compiled to the application.

* The structure of the created project is as follows:

    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android/app/src/main  
    |   |   └── cpp
    |   |       └── CmakeLists.txt
    |   └── ios             
    ├── AppScope                             
    ├── build-profile.json5
    ├── entry/src/main
    |   └── cpp                 // Native C++
    |       ├── types/libentry
    |       |   ├── index.d.ts 
    |       |   └── oh-package.json5
    |       ├── CMakeLists.txt
    |       └── hello.cpp 
    ├── hvigor
    ├── hvigorfile.ts
    ├── hvigorw
    ├── hvigorw.bat
    └── oh-package.json5
    ```
* Run the **ace build** command to build the source code. In this way, a cross-platform application is built with native C++ APIs.

## AAR/Framework Development
With ACE Tools, you can develop AAR for the Android platform and Framework for the iOS platform.
* When creating a project, select **Library** for the project type.
* Run the **ace build** command to build the library product of the target platform.
    ```shell
    ace build aar              // Build AAR for the Android platform.
    ace build ios-framework    // Build Framework for the iOS platform.
    ace build ios-xcframework  // Build XCFramework for the iOS platform.
    ```
## Multi-Ability Development
With ACE Tools, you can develop a cross-platform application with multiple abilities.
* Run the following command and enter the ability name as prompted to create an Ability module:
    ```
    cd entry
    ace new ability
    ? Please enter the ability name:Second
    ```
* The preceding command adds the corresponding ability adaptation code to the **entry**, **android**, and **ios** directories. The following shows only the new code.
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android/app/src/main/java/com/example/demo
    |   |   └── EntrySecondAbilityActivity.java         // Android code for ability adaptation                  
    |   └── ios/app
    |       ├── EntrySecondAbilityViewController.h      // iOS code for ability adaptation
    |       └── EntrySecondAbilityViewController.m      // iOS code for ability adaptation                       
    |               
    └── entry/src/main/ets              
        └── SecondAbility           // SecondAbility
            └── SecondAbility.ets             
    ```
* Run the **ace build ios/apk** command to build and pack the ArkTS source code. In this way, an iOS or Android application in the multi-ability scenario is built.
## Multi-Module Development
With ACE Tools, you can develop a cross-platform application with multiple modules.
* Run the following command and enter the module name as prompted:
    ```
    cd <Root directory of the cross-platform application project>
    ace new module
    ? Please enter the module name: feature1
    ```
* Run the **ace new module** command and enter the module name as prompted. In this example, the module name is **feature1**. This command adds the corresponding ability adaptation code to the **entry**, **android**, and **ios** directories.
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android/app/src/main/java/com/example/demo
    |   |   └── Feature1Feature1AbilityActivity.java         // Android code for ability adaptation
    |   └── ios/app
    |       ├── Feature1Feature1AbilityViewController.h      // iOS code for ability adaptation
    |       └── Feature1Feature1AbilityViewController.m      // iOS code for ability adaptation                       
    ├── entry              
    └── feature1        // New module: feature1
             
    ```
* Run the **ace build ios/apk** command to build and pack the ArkTS source code. In this way, an iOS or Android application in the multi-module scenario is built.

##  References

- [ACE Tools](https://gitcode.com/arkui-x/cli/blob/ArkUI-X-5.0.2-Release/README-EN.md)
