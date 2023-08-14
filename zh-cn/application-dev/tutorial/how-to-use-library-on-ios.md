# iOS平台基于ArkUI-X的Framework的构建与使用

## 简介
本教程主要讲述如何利用ArkUI-X SDK完成iOS framework开发与使用，实现基于ArkTS的声明式开发范式在iOS平台显示。包括：

* 跨平台Library工程开发介绍
* Framework在iOS应用工程的使用

### 使用ACE Tools和DevEco Studio集成ArkUI-X SDK进行Android AAR开发
可以通过通过ACE Tools或DevEco Studio完成
* ACE Tools
1. ace create 命令创建一个跨平台应用工程，示例工程名为demo,选择工程类型为library：
    ```
    ace create [project]
    ```
2. 执行ace build framework命令，构建得到iOS Framework包。
    ```
    ace build framework
    ```
* DevEco Studio
1. 创建跨平台library工程
2. 通过执行build app选项，构建得到iOS Framework包

### Framework在应用工程的使用

通过xcode创建一个应用工程，删除其他xcode自动生成的的代码头文件和源文件(参考[ios工程集成 ArkUI sdk](../tutorial/how-to-integrate-arkui-into-ios.md))。将上述myframework.framework与libarkui_ios.framework引入到工程中。
**AppDelegate部分**
* AppDelegate.h
```objective-c
#import <UIKit/UIKit.h>

 #import <myframework/ArkUIAppDelegate.h>
 @interface AppDelegate : ArkUIAppDelegate


 @end
```
* AppDelegate.m
```objective-c
 #import "AppDelegate.h"

 @interface AppDelegate ()

 @end

 @implementation AppDelegate

 @end
```

完成上述步骤后即可按照iOS应用构建流程，构建ArkUI iOS应用。