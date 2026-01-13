# iOS平台集成ArkUI-X接口类

## 概述

实现OpenHarmony平台与iOS平台的对接，可以帮助开发者将基于ArkUI开发的应用运行在标准的iOS设备上。

## 类概述

| 类    | 描述               |
| ----------- | ---------------------------------- |
| [BridgePlugin](BridgePlugin.md) | 平台Bridge |
| [StageViewController](StageViewController.md) | Stage模型UIViewController，将iOS中UIViewController的生命周期与OpenHarmony中Ability的生命周期进行映射 |
| [StageApplication](StageApplication.md) | 负责iOS应用初始化流程 |
| [IArkUIXPlugin](IArkUIXPlugin.md) | ArkUI-X框架平台端插件能力开发 |
| [ILogger](ILogger.md) | ArkUI-X框架支持日志拦截能力，支持在iOS平台拦截并输出框架日志及ArkTS日志 |
| [AbilityLoader](AbilityLoader.md) | Stage模型AbilityLoader，支持在iOS平台主动加载指定UIAbility |
