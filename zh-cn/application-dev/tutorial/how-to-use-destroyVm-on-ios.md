# 特性级跨平台场景，退出跨平台页面，主动释放ArkTS运行时占用开发指导

## 简介

本文介绍如何在iOS平台调用`destroyVm`接口，主动销毁ArkTS运行时及关联资源。在应用从ArkUI页面返回原生页面且不再需要ArkTS运行时的场景（低内存场景），开发者可以调用该接口释放运行时占用的内存资源，但注意，运行时释放以后，开发者重新进入跨平台页面，需要重新创建运行时，会影响启动时延。<br>

## 适用场景

`destroyVm`接口适用于一级页面为iOS原生页面，二级页面为ArkUI-X技术实现的页面的应用，如[华为运动健康](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/tutorial/how-to-use-arkuix-on-huaweiHealth.md)应用。<br>

当用户从ArkUI二级页面返回iOS原生页面，在返回时机调用`destroyVm`接口可主动释放运行时，减少内存占用。<br>

## 接口说明

关于接口`destroyVm`的详细说明见[destroyVm](../reference/arkui-for-ios/StageApplication.md#destroyvm)。<br>

## 约束

> 1. 该接口最早使用时机在 `[StageApplication launchApplication]` 调用之后。<br>
> 2. 若当前存在活跃的 Worker 线程，接口将返回 NO，Worker 完成后可再次调用。<br>
> 3. 接口作为同步接口调用后会立即返回，但运行时的实际销毁是在事件循环中异步执行。<br>
> 4. 销毁后若再次需要进入 ArkUI 页面，引擎会自动重新初始化，无需额外操作。<br>
