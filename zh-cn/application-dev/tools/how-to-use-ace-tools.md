# ACE Tools构建跨平台应用

本教程主要讲述如何通过ACE Tools将OpenHarmony开发范式扩展到跨平台进行开发，包括：

* 基本工程开发
* Native C++开发
* AAR/Framework开发
* 多Ability开发
* 多Module开发

## 基本工程开发
* 执行 ace create 命令创建一个简单工程，按照提示填写工程名和相关信息：
    ```shell
    ace create project
   ? Please enter the project name: HelloWorld
   ? Please enter the bundle name (com.example.helloworld):com.example.helloworld
   ? Please enter the system (1: OpenHarmony, 2: HarmonyOS): 1
   ? Please enter the project type (1: Application, 2: Library): 1
   ? Please enter the template (1: Empty Ability, 2: Native C++): 1   //选择创建Empty Ability或者Native C++项目
   ```
* 创建的工程结构如下：
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android         //Android应用工程
    |   └── ios             //iOS应用工程             
    ├── AppScope                             
    ├── build-profile.json5
    ├── entry               //Module模块
    ├── hvigor
    ├── hvigorw
    ├── hvigorw.bat
    └── oh-package.json5
    ```
* 工程结构与通过DevEco Studio构建的跨平台工程一致。
* 执行ace build命令，即可完成对ArkTS源码编译和应用打包，构建出相应的跨平台应用。
## Native C++开发
ACE Tools支持开发者进行Native C++开发跨平台。
* 需要在创建工程时，指定template类型为Native C++。

**注：** iOS工程结构没有变化，通过iOS工程配置，便会将source目录下的hello.cpp文件编译到应用当中。 
* 新增工程结构如下:

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
    |   └── cpp                 //Native C++
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
* 执行ace build命令，即可构建出带有Native C++的跨平台应用。
## AAR/Framework开发
ACE Tools支持开发者针对Android平台开发AAR。
* 需要在创建工程时，选择工程类型为library。工程结构和简单工程类型一致，区别为Android目录工程
* 执行ace build命令即可构建相应目标平台的library产物：
    ```shell
    ace build aar        //构建Android平台的AAR产物
    ace build framework  //构建iOS平台的Framework产物
    ```
## 多Ability开发
ACE Tools支持开发者针跨平台进行多Ability开发。
* 执行如下命令，按提示填写Ability名称即可创建Ability模块
    ```
    cd entry
    ace create ability
    ? Please enter the ability name:Second
    ```
* 该命令会在entry、android和ios目录下增加相应的ability适配代码，如下只展示新增部分
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android/app/src/main/java/com/example/demo
    |   |   └── EntrySecondAbilityActivity.java         //Android侧适配Ability代码                   
    |   └── ios/app
    |       ├── EntrySecondAbilityViewController.h      //iOS侧适配Ability代码
    |       └── EntrySecondAbilityViewController.m      //iOS侧适配Ability代码                        
    |               
    └── entry/src/main/ets              
        └── SecondAbility           //SecondAbility
            └── SecondAbility.ets             
    ```
* 执行ace build app/apk 命令，即可完成对多Ability场景下ArkTS源码编译和打包，构建出相应的iOS或Android应用。
## 多Module开发
ACE Tools支持开发者针对跨平台进行多Module范式开发。
* 执行如下命令，按提示填写Module名称:
    ```
    cd entry
    ace create ability
    ? Please enter the module name: feature1
    ```
* 执行ace create module 命令，按照提示输入module name，示例名称为feature1。该命令会在entry、android和ios目录下增加相应的ability代码
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android/app/src/main/java/com/example/demo
    |   |   └── Feature1Feature1AbilityActivity.java         //Android侧适配Ability代码
    |   └── ios/app
    |       ├── Feature1Feature1AbilityViewController.h      //iOS侧适配Ability代码
    |       └── Feature1Feature1AbilityViewController.m      //iOS侧适配Ability代码                        
    ├── entry              
    └── feature1        //新增的feature1模块
             
    ```
* 执行ace build app/apk 命令，即可完成对多module场景下ArkTS源码编译和打包，构建出相应的iOS或Android应用。