# Android平台集成ArkUI-X接口类

## 概述

实现OpenHarmony平台与Android平台的对接，可以帮助开发者将基于ArkUI开发的应用运行在标准的Android设备上。

## 类概述

| 类    | 描述               |
| ----------- | ---------------------------------- |
| [BridgePlugin](BridgePlugin.md) | 平台Bridge |
| [StageApplication](StageApplication.md) | Stage模型Application，用于初始化资源路径以及加载配置信息 |
| [StageApplicationDelegate](StageApplicationDelegate.md) | Stage模型Application代理类，将来自StageApplication的数据经过处理后传递给OpenHarmony框架层 |
| [StageActivity](StageActivity.md) | Stage模型Activity，将Android中Activity的生命周期与OpenHarmony中Ability的生命周期进行映射 |
| [IArkUIXPlugin](IArkUIXPlugin.md) | ArkUI-X框架平台端插件能力开发 |
| [ILogger](ILogger.md) | ArkUI-X框架支持日志拦截能力，支持在Android平台拦截并输出框架日志及ArkTS日志 |
| [AbilityLoader](AbilityLoader.md) | Stage模型AbilityLoader，支持在Android平台主动加载指定UIAbility |