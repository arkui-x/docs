# iOS平台动态化开发指南

## 简介

本文介绍如何在iOS平台进行ArkUI-X动态化开发，包括动态化目录规则及约束。

## 适用场景

由于iOS平台AppStore不允许应用动态更新so库，因此ArkUI-X在iOS上动态化只能更新界面及业务逻辑构成的abc，不支持so库动态加载。应用需要将ArkUI-X框架、插件及业务so库提前打包到应用内上架。

## 开发指南

### 目录结构

iOS平台ArkUI-X沙箱内目录结构如下所示：

```
/Data/Application/应用/Documents/files/arkui-x    
├── feature1                    # 跨平台特性1
│   ├── ets                     # ets目录
│   │   ├──sourceMaps.map
│   │   └──modules.abc
│   ├── resources.index         
│   ├── resources              
│   └── module.json
└── systemres                   # ArkUI公共资源
```

1. `/Data/Application/应用/Documents/files/arkui-x`可以视为ArkUI-X动态加载的沙箱根目录，特性Bundle需要放在这个目录下；
2. 根目录下要求按照module级别组织，**不可以重名**；

### 加载优先级

+ module加载：优先从应用根目录下寻找，如果找不到则去沙箱内尝试加载；

+ systemres加载：同上，优先加载应用根目录下的资源，找不到则去沙箱内加载；

  **注意**：不建议应用同一个module，即预制到应用内又在沙箱同时部署。
