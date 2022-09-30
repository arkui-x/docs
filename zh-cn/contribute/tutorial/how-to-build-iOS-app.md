## iOS平台构建ArkUI应用

#### 总览

本教程主要讲述如何利用ArkUI-X SDK完成iOS应用开发，实现ArkUI开发范式在iOS平台显示。包括：

* ArkUI-X SDK获取方法
* ArkUI-X SDK内容组成
* iOS应用工程集成ArkUI-X SDK
* iOS应用工程集成ArkUI JSBundle实例

##### 开发准备

**方式一**

* 通过镜像站点获取ArkUI-X SDK，适合ArkUI跨平台应用初学者。

| 版本源码                             | **版本信息** | **下载站点** | **SHA256校验码** |
| ------------------------------------ | ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（iOS） | 0.1.0 Beta    | [站点]()     | [SHA256校验码]() |

**方式二**

* 通过ArkUI-X跨平台项目源码编译出SDK，适用于ArkUI跨平台应用高阶开发者。完成跨平台项目[代码下载](../../application-dev/quick-start/README.md)后，执行如下命令编译ArkUI-X iOS平台SDK。

```
./build.sh --product-name arkui-cross --target-os ios
```

##### SDK组成清单

ArkUI跨平台iOS侧SDK提供`libace_ios.framework`和`libace_ios.xcframework`两种Framework类型；Framework文件的Header包含`Ace.h`、`AceViewController.h`、`FlutterPlugin.h`，其中`AceViewController`实现了ArkUI启动和JSBundle加载等功能。

```
ArkUI_iOS_SDK
    ├── libace_ios.framework                 // ArkUI iOS framework
    │   ├── Headers                          // ArkUI framework头文件
    │   │   ├── Ace.h                          
    │   │   ├── AceViewController.h   
    │   │   └── FlutterPlugin.h        
    |   ├── info.plist
    │   ├── libace_ios                       // 二进制可执行文件
    |   ├── libace_ios.podspec               // CocoPods配置文件
    |   ├── LICENSE
    │   └── Modules
    │       └── module.modulemap            
    └── libace_ios.xcframework/ios-arm64/libace_ios.framework    // ArkUI iOS xcframework           
        ├── Headers                          // ArkUI framework头文件
        │   ├── Ace.h            
        │   ├── AceViewController.h      
        │   └── FlutterPlugin.h            
        ├── info.plist
        ├── libace_ios                       // 二进制可执行文件
        ├── libace_ios.podspec               // CocoPods配置文件
        ├── LICENSE
        └── Modules
            └── module.modulemap  
```

>![](../public_sys-resources/icon-note.gif) **说明：**
>
>-   ArkUI跨平台提供的Native层接口，比如：napi、libuv和NativeXComponent接口，不在这里列举和描述。
>-   [Native层接口说明](../../application-dev/reference/README.md)

##### iOS工程集成ArkUI跨平台SDK

iOS工程集成ArkUI跨平台SDK遵循iOS应用工程集成Framework规则，即将SDK中libace_ios.xcframework包拷贝到工程目录下，并引入到工程目录。

* AppDelegate.mm中实例化AceViewController，并加载ArkUI页面

```
 #import <libace_ios/Ace.h>

 AceViewController *controller = [[AceViewController alloc] initWithVersion:(ACE_VERSION_ETS) instanceName:@"MainAbility"];
 controller.view.frame = [UIScreen mainScreen].bounds;
 [self.view addSubview:controller.view];

```

其中参数`version`为ArkUI开发范式，1:表示ArkUI类Web范式，2:ArkUI声明式范式；`instanceName`为ArkUI范式代码编译为JSBundle后在应用工程中存放的目录名。

##### iOS工程集成ArkUI JSBundle实例

ArkUI JSBundle生成后，拷贝到iOS应用工程assets/js目录下，比如：js/jsdemo。这里“js”目录名称是固定的，不能更改。

```
iOS应用工程
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


##### 注意事项
**工程修改**
XCode11之后，新建iOS工程为SceneDelegate类型工程，需删除SceneDelegate相关内容：
1. 删除Info.plist中的Applicaton Scene Manifest选项。
2. SceneDelegate代码文件删除，或注释掉文件中的所有代码。
3. 删除或注释掉AppDelegate中关于Scene的方法。

**真机调试**
1. Target signing & capabilities选项中Signing配置签名信息。
2. Target General选项中Frameworks，Libraries，and Embedded Content配置Embed类型为Embed & Sign。
3. Target Building Settings选项中Build Options Enable Bitcode设置为No。

**模拟器调试**
1. 添加模拟器版本的ArkUI跨平台Framework。
2. Target General选项中Frameworks，Libraries，and Embedded Content配置Embed类型为Do Not Embed。

按照注意事项配置后，即可按照iOS应用构建流程，构建ArkUI iOS应用。


##### 参考

【1】[ArkUI JSBundle获取指南]()

