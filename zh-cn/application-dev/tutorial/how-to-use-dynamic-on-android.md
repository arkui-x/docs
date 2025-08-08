# Android平台动态化开发指南

## 简介

本文介绍如何在Android平台进行ArkUI-X动态化开发，包括动态化目录规则及约束。

## 适用场景

动态化主要包括两个典型场景：

+ 场景1：框架动态化，为了降低应用ROM体积占用，及满足动态升级框架目的；
+ 场景2：特性Bundle动态化，特性和宿主应用发布解耦；

## 开发指南

### 目录说明

动态加载时要求应用沙箱内目录架构如下所示：

```
/data/data/应用/files/arkui-x
├── arkui-x.json                # so库所关联的版本号
├── feature1                    # 跨平台特性1
│   ├── ets                     # ets目录
│   │   ├──sourceMaps.map
│   │   └──modules.abc
│   ├── resources.index         
│   ├── resources              
│   ├── module.json             # modules.abc以及resource相关版本号
│   └── libs                    # 特性bundle带的so库
│       ├── arm64-v8a
│       ├── armeabi-v7a
│       └── x86_64  
├── systemres                   # ArkUI公共资源
└── libs                        # 根目录下libs库
    ├── arm64-v8a               
    │    └──libarkui_android.so    # ArkUI-X引擎
    ├── armeabi-v7a           
    └── x86_64
```

1. `/data/data/应用/files/arkui-x `可以视为ArkUI-X动态加载的沙箱根目录，框架和特性Bundle均需要放在这个目录下；

2. 根目录下的libs文件夹放置引擎（libarkui_android.so），及其他公共库；

3. 根目录下要求按照module级别组织，**不可以重名**；

   ### 加载优先级

+ 引擎so库：存在arkui-x.json时，根据其中的version字段选择优先加载的路径，不存在时，优先加载应用lib目录，如果未找到则去应用沙箱目录加载；

+ 插件so库：存在arkui-x.json时，根据其中的version字段选择优先加载的路径，不存在时，优先加载应用lib目录，如果未找到则去应用沙箱目录加载；

+ module加载：根据module.json中的versionCode字段判断加载的路径；

+ systemres加载：加载沙箱路径；

  **注意**：.so以及hsp类型的module只支持冷启动，不支持热重载，module支持冷启动以及热重载。

  ### 框架初始化

  如果应用使用了框架引擎动态化，首次下载引擎库后将其放置`/data/data/应用/files/arkui-x/libs/arm64-v8a`

 目录，之后再打开对应跨平台界面时初始化框架：

  **注意**：初始化必须在主线程调用。

```ts
appDelegate = new StageApplicationDelegate();
appDelegate.initApplication(this)
```

### 加载流程
主库与插件库加载流程:

<img src=".\figures\how-to-use-dynamic-on-android_001.png" />

modules.abc以及resource相关

<img src=".\figures\how-to-use-dynamic-on-android_002.png" />

后续再打开应用，建议按照正常流程在Application里初始化框架，提前完全引擎库加载，提高跨平台模块加载速度；

