## iOS平台构建ArkUI应用

#### 总览

本教程主要讲述如何利用ArkUI-CrossPlatform SDK完成iOS应用开发，实现ArkUI开发范式在iOS平台显示。包括：

* ArkUI-CrossPlatform SDK获取方法
* ArkUI-CrossPlatform SDK内容组成
* iOS应用工程集成ArkUI-CrossPlatform SDK
* iOS应用工程集成ArkUI JSBundle实例

##### 开发准备

**方式一**

* 通过镜像站点获取ArkUI-CrossPlatform SDK，适合ArkUI跨平台应用初学者。

| 版本源码                             | **版本信息** | **下载站点** | **SHA256校验码** |
| ------------------------------------ | ------------ | ------------ | ---------------- |
| ArkUI-CrossPlatform SDK包（iOS） | 1.0 Alpha    | [站点]()     | [SHA256校验码]() |

**方式二**

* 通过ArkUI-CrossPlatform跨平台项目源码编译出SDK，适用于ArkUI跨平台应用高阶开发者。完成跨平台项目[代码下载](https://gitee.com/arkui-crossplatform/doc/blob/master/application-dev/quick-start/README.md)后，执行如下命令编译ArkUI-CrossPlatform iOS平台SDK。

```
./build.sh --product-name arkui-cross --target-os ios
```

##### SDK组成清单

ArkUI跨平台iOS侧SDK主要有`libace_ios.framework`和`libace_ios.xcframework`组成；Framework文件的Header包含`Ace.h`、`AceViewController.h`、`FlutterPlugin.h`，其中`AceViewController`实现了ArkUI启动和JSBundle加载等功能。

```
out/arkui-cross_ios_arm64/lib.ios.framework/arkui/ace_engine_cross
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
>-   [Native层接口说明](https://gitee.com/arkui-crossplatform/doc/blob/master/application-dev/reference/README.md)

##### iOS工程集成ArkUI跨平台SDK

iOS工程集成ArkUI跨平台SDK遵循iOS应用工程集成动态库规则，即将SDK中libace_ios.xcframework包拷贝到工程目录下，并引入到工程目录。

* ArkUI demo页面创建

```
 #import <libace_ios/Ace.h>

 AceViewController *controller = [[AceViewController alloc] initWithVersion:1 bundleDirectory:@"jsdemo"];
 controller.view.frame = [UIScreen mainScreen].bounds;
 [self.view addSubview:controller.view];

```

其中参数`version`为ArkUI开发范式，1:表示ArkUI类Web范式，2:ArkUI声明式范式；`bundleDirectory`为ArkUI范式代码编译为JSBundle后在应用工程中存放的目录名。

##### iOS工程集成ArkUI JSBundle实例

ArkUI JSBundle生成后，拷贝到iOS应用工程assets/js目录下，比如：assets/js/jsdemo。这里“js”目录名称是固定的，不能更改。

```
/assets/js/jsdemo
    ├── app.js
    ├── manifest.json
    └── pages
        └── index
            └── index.js
```

手动修改signing & capabilities配置之后，即可按照iOS应用构建流程，构建ArkUI iOS应用。


##### 参考

【1】[ArkUI JSBundle获取指南]()

