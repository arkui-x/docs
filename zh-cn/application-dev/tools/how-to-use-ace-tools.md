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
    ohos@user Desktop % ace create demo
   ? Please enter the project name(demo): # 输入工程名称，不输入默认为文件夹名称
   ? Please enter the bundleName (com.example.demo):  # 输入包名，不输入默认为com.example.工程名
   ? Please enter the runtimeOS (1: OpenHarmony, 2: HarmonyOS): 1 # 输入RuntimeOS系统
   ? The project already exists. Do you want to delete the directory (y / n): y
   Delete directory successfully, creating new project.
   
   Project created successfully! Target directory:  ${当前目录}/demo.
   
   In order to run your application, type:
   
     $ cd demo
     $ ace run
   
   Your application code is in demo/entry.
   ```
* 创建的工程结构如下：
    ```
    HelloWorld
    ├── .arkui-x
    |   ├── android                   //Android应用工程
    |   └── ios                       //iOS应用工程
    ├── AppScope
    ├── build-profile.json5
    ├── entry                         //应用入口模块
    ├── hvigor
    ├── hvigorfile.ts
    ├── hvigorw
    ├── hvigorw.bat
    └── oh-package.json5
    ```
* 工程结构与通过DevEco Studio导入的跨平台Sample工程一致。
* 执行ace build命令，即可完成对ArkTS源码编译和应用打包，构建出相应的跨平台应用。
## Native C++开发
ACE Tools支持开发者进行Native C++开发跨平台。
* 需要在创建工程时，指定template类型为plugin_napi。

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
    ├── hvigorfile.ts
    ├── hvigorw
    ├── hvigorw.bat
    └── oh-package.json5
    ```
* 执行ace build命令，即可构建出带有Native C++的跨平台应用。
## AAR/Framework开发
ACE Tools支持开发者针对Android平台开发AAR，针对iOS平台开发Framework。
* 需要在创建工程时，选择工程类型为library。
* 执行ace build命令即可构建相应目标平台的library产物：
    ```shell
    ace build aar              //构建Android平台的AAR产物
    ace build ios-framework    //构建iOS平台的framework产物
    ace build ios-xcframework  //构建iOS平台的xcframework产物
    ```
## 多Ability开发
ACE Tools支持开发者针跨平台进行多Ability开发。
* 执行如下命令，按提示填写Ability名称即可创建Ability模块
    ```
    cd entry
    ace new ability
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
* 执行ace build ios/apk 命令，即可完成对多Ability场景下ArkTS源码编译和打包，构建出相应的iOS或Android应用。
## 多Module开发
ACE Tools支持开发者针对跨平台进行多Module范式开发。
* 执行如下命令，按提示填写Module名称:
    ```
    cd <跨平台工程根目录>
    ace new module
    ? Please enter the module name: feature1
    ```
* 执行ace new module 命令，按照提示输入module name，示例名称为feature1。该命令会在entry、android和ios目录下增加相应的ability代码
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
* 执行ace build ios/apk 命令，即可完成对多module场景下ArkTS源码编译和打包，构建出相应的iOS或Android应用。

##  参考

- [ACE Tools命令行详情说明](https://gitee.com/arkui-x/cli/blob/master/README.md)
- [ACE Tools环境配置详细说明](../tutorial/how-to-configure-dev-environment.md)