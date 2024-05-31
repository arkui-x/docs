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
    ace create project
   ? Please enter the project name: HelloWorld
   ? Please enter the bundle name (com.example.helloworld):com.example.helloworld
   ? Please enter the system (1: OpenHarmony, 2: HarmonyOS): 1
   ? Please enter the project type (1: Application, 2: Library): 1
   ? Please enter the template (1: Empty Ability, 2: Native C++): 1 // Create an empty Ability or native C++ project.
   ```
* The structure of the created project is as follows:
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android         // Android application project
    |   └── ios             // iOS application project            
    ├── AppScope                             
    ├── build-profile.json5
    ├── entry               // Module
    ├── hvigor
    ├── hvigorw
    ├── hvigorw.bat
    └── oh-package.json5
    ```
* The project structure is the same as the structure of a cross-platform project built using DevEco Studio.
* Run the **ace build** command to build and pack the ArkTS source code. In this way, a cross-platform application is built.
## Native C++ Development
With ACE Tools, you can use native C++ APIs in cross-platform development.
* When creating a project, select **Native C++** for the template type.

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
    ├── hvigorw
    ├── hvigorw.bat
    └── oh-package.json5
    ```
* Run the **ace build** command to build the source code. In this way, a cross-platform application is built with native C++ APIs.

## AAR/Framework Development
With ACE Tools, you can develop Android Archive (AAR) plug-ins.
* When creating a project, select **Library** for the project type. The project structure is the same as the structure of a simple project. The only difference is that the project you create is an Android directory project.
* Run the **ace build** command to build the library product of the target platform.
    ```shell
    ace build aar        // Build the AAR for the Android platform.
    ace build framework  // Build the framework of the iOS platform.
    ```
## Multi-Ability Development
With ACE Tools, you can develop a cross-platform application with multiple abilities.
* Run the following command and enter the ability name as prompted to create an Ability module:
    ```
    cd entry
    ace create ability
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
* Run the **ace build app/apk** command to build and pack the ArkTS source code. In this way, an iOS or Android application in the multi-ability scenario is built.
## Multi-Module Development
With ACE Tools, you can develop a cross-platform application with multiple modules.
* Run the following command and enter the module name as prompted:
    ```
    cd entry
    ace create ability
    ? Please enter the module name: feature1
    ```
* Run the **ace create module** command and enter the module name as prompted. In this example, the module name is **feature1**. This command adds the corresponding ability adaptation code to the **entry**, **android**, and **ios** directories.
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
* Run the **ace build app/apk** command to build and pack the ArkTS source code. In this way, an iOS or Android application in the multi-module scenario is built.
