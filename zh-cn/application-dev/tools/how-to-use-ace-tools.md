# ACE Tools构建跨平台应用

## 总览

本教程主要讲述如何通过ACE Tools将OpenHarmony开发范式扩展到跨平台进行开发，包括：

* 基本工程开发
* Native C++开发
* AAR/Framework开发
* 多Ability开发
* 多Module开发

### 基本工程开发
* 执行 ace create [project] 命令 ，按照提示填写：
1. project name：示例名称为demo
2. bundle name：选择默认包名
3. system选项： 选择OpenHarmony或HarmonyOS均可，只区分hap应用的安装设备平台
4. template类型： 选择 Empty Ability
```
demo
├── android             //android应用工程            
├── ios                 //iOS应用工程                              
├── ohos                //OpenHarmony/HarmonyOS应用工程
└── source              //保存ArkTS源码
    └── entry 
```
* 工程创建时，source目录下默认生成一个Module，名称为entry。与通过DevEco Studio构建的项目中的module一致
* 执行ace build命令，即可完成对ArkTS源码编译和打包，构建出相应的跨平台应用。
### Native C++开发
ACE Tools支持开发者进行Native C++开发跨平台。
* 需要在创建工程时，指定template类型为Native C++，也就是在基本工程开发中的第4步选择Native C++，其他选项保持一致。此操作会生成相应的c++模块。
**注：** iOS工程结构没有变化，通过iOS工程配置，便会将source目录下的hello.cpp文件编译到应用当中。 
```
demo
├── android/app/src/main
|   └── cpp                         //cpp部分
|       └──CmakeLists.txt                       
├── ios                                             
├── ohos                
└── source/entry/src/main              
    └── cpp                         //cpp部分
        ├── types/libentry
        |   ├── index.d.ts 
        |   └── oh-package.json5
        ├── CMakeLists.txt
        └── hello.cpp 
```
* 执行ace build命令，即可构建出带有Native C++的跨平台应用。
### AAR开发
ACE tools支持开发者针对Android平台开发AAR。
* 进入ace工程目录下，执行 ace create aar，按照提示填写AAR name，执行该命令后会在android目录下创建一个AAR工程，示例名称为myaar。
```
demo
├── android                        
|   └── myaar           //AAR工程目录
├── ios                                               
├── ohos                
└── source              
    └── entry 
```
* 执行ace build aar，即可完成对ArkTS源码编译和打包，构建出相应的AAR产物。
### Framework开发
ACE tools支持开发者针iOS平台开发Framework。
* 进入ace工程目录下，执行 ace create framework，按照提示填写framework name，执行该命令后会在ios目录下创建一个Framework工程，示例名称为myframework。
* 执行ace build framework，即可完成对ArkTS源码编译和打包，构建出相应的framework产物。
```
demo
├── android                        
├── ios
|   └── myframework    //Framework工程目录                                             
├── ohos                
└── source              
    └── entry 
```
### 多Ability开发
ACE tools支持开发者针跨平台进行多Ability开发。
* 按照上述demo为例，执行 cd demo/source/entry 命令，进入工程的entry目录。
* 执行ace create ability 目录，按照提示输入ability name，示例名称为Second。该命令会在entry、android和ios目录下增加相应的ability代码
```
demo
├── android/app/src/main/java/com/example/demo
|   └── EntrySecondAbilityActivity.java  //java侧适配ability代码                   
├── ios/app
|   ├── EntrySecondViewController.h      //ios侧适配ability代码
|   └── EntrySecondViewController.m      //ios侧适配ability代码                        
├── ohos                
└── source/entry/src/main/ets              
    └── SecondAbility
        └── SecondAbility.ts             //SecondAbility
```
* 执行ace build app/apk 命令，即可完成对多Ability场景下ArkTS源码编译和打包，构建出相应的iOS或Android应用。
### 多Module开发
ACE tools支持开发者针跨平台进行多Module开发。
* 按照上述demo为例，执行 cd demo/source 命令，进入工程的source目录。
* 执行ace create module 命令，按照提示输入module name，示例名称为feature1。该命令会在entry、android和ios目录下增加相应的ability代码
```
demo
├── android/app/src/main/java/com/example/demo
|   └── Feature1MainAbilityActivity.java  //java侧适配ability代码                   
├── ios/app
|   ├── Feature1MainViewController.h      //ios侧适配ability代码
|   └── Feature1MainViewController.m      //ios侧适配ability代码                        
├── ohos                
└── source
    ├── entry      
    └── feature1                        //feature1模块
```
* 执行ace build app/apk 命令，即可完成对多module场景下ArkTS源码编译和打包，构建出相应的iOS或Android应用。