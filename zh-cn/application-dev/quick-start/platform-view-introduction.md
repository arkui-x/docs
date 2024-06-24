# 平台视图

## 简介

平台视图，为了利用原生组件，减少开发成本，提供了ArkUI-X与原生view进行混合显示。

以Android为例，ArkUI-X框架支持将原生View和ArkUI组件混合显示的能力，用户可以编写ArkTS代码，java原生代码，就可以在ArkUI-X框架上显示原生组件的视图。

本文介绍如何通过编写自定义的Android、iOS平台的代码。具体用法，Android侧参考[PlatformView](../tutorial/how-to-use-platformview-on-android.md)，iOS侧参考[PlatformView](../tutorial/how-to-use-platformview-on-ios.md)。


## 使用场景及能力

### 使用场景

平台视图主要用于这样的场景：应用需要显示原生组件的视图，如地图，而在OpenHarmony中没有对应的跨平台的类似组件的实现。

具体可用于以下场景：

1、利用Android的地图原生组件，如显示地图等；<br/>
2、利用iOS的地图原生组件，如显示地图等；<br/>

> **说明**
>
>  开发者能够在XComponent的PLATFORMVIEW模式下，能够混合显示ArkUI组件与原生view，并支持用户操作的事件处理。


### 接口支持

平台视图提供的接口包括，IPlatformView接口，PlatformViewFactory接口，支持Android、iOS。具体如下表：

IPlatformView接口:

| 函数名称           | 类型     | 描述                                   |
| ------------------ | -------- | -------------------------------------- |
| getView() | View | 获得原生组件View           |
| onDispose()      | void | 资源销毁的处置       |
| getXComponentID()          | String | 返回原生组件的ID |

PlatformViewFactory接口:

| 函数名称           | 类型     | 描述                                   |
| ------------------ | -------- | -------------------------------------- |
| getPlatformView(String) | IPlatformView | 获得IPlatformView接口           |



> **说明**
> 
> 接口的详细声明，参见接口介绍，Android侧参考[PlatformView](../reference/arkui-for-android/platformview-interface-android.md)，iOS侧参考[PlatformView](../reference/arkui-for-ios/platformview-interface-ios.md)。


## 开发指南

[平台视图开发指南（Android）](../tutorial/how-to-use-platformview-on-android.md)<br />
[平台视图开发指南（iOS）](../tutorial/how-to-use-platformview-on-ios.md)