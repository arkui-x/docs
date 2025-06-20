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
├── feature1                    # 跨平台特性1
│   ├── ets                     # ets目录
│   │   ├──sourceMaps.map
│   │   └──modules.abc
│   ├── resources.index         
│   ├── resources              
│   ├── module.json
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

+ 引擎so库：优先加载应用lib目录下，如果未找到则去应用沙箱根目录加载；

+ 插件so库：优先加载应用lib目录下，如果未找到则去应用沙箱根目录尝试加载，最后去插件自身的libs目录加载；

+ module加载：优先从应用assets目录下寻找，如果找不到则去沙箱内尝试加载；

+ systemres加载：同上，优先加载应用assets目录，找不到则去沙箱内加载；

  **注意**：不建议应用同一个module，即预制到应用assets内又在沙箱同时部署。

  ### 框架初始化

  如果应用使用了框架引擎动态化，首次下载引擎库后将其放置`/data/data/应用/files/arkui-x/libs/arm64-v8a`

 目录，之后再打开对应跨平台界面时初始化框架：

  **注意**：初始化必须在主线程调用。

```ts
appDelegate = new StageApplicationDelegate();
appDelegate.initApplication(this)
```

后续再打开应用，建议按照正常流程在Application里初始化框架，提前完全引擎库加载，提高跨平台模块加载速度；

