# 平台桥接(@arkui-x.bridge)

## 简介

平台桥接用于客户端（ArkUI）和平台（Android或iOS）之间传递消息，即用于ArkUI与平台双向数据传递、ArkUI侧调用平台的方法、平台调用ArkUI侧的方法。

以Android平台为例，ArkTS和Java没有相互调用的能力，为了实现ArkTS和Java交互，需要ArkTS与C++交互，C++再与Java交互，反之亦然。但是对于开发者，就像是ArkTS和Java直接交互。

本文介绍如何通过平台桥接编写自定义的Android、iOS平台代码。ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，Android侧参考[BridgePlugin](../reference/arkui-for-android/BridgePlugin.md)，iOS侧参考[BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md)。


## 使用场景及能力

### 使用场景

平台桥接主要用于这样的场景：应用需要复用平台的代码，而在OpenHarmony中没有对应的跨平台API（不包括UI相关）实现。

具体可用于以下场景：

1、ArkUI与平台双向传递数据，如传递JSON数据、图片等；<br/>
2、ArkUI侧调用平台的API，如获取Android或iOS平台上的电池电量、复用平台上的三方库等；<br/>
3、平台调用ArkUI侧的方法，如复用JavaScript的三方库等。

> **说明**
>
>  平台桥接支持ArkUI调用Android Java API和iOS Objective-C API。此外，一些平台相关功能可直接通过已有的[OpenHarmony跨平台API](../reference/apis/README.md)实现。


### 数据类型支持

平台桥接通过JSON格式序列化编解码传递数据，支持基础数据类型、数组类型和结构化数据。具体支持类型如下表：

| ArkTS                 | Java                | Objective-C               |
| --------------------- | ------------------- | ------------------------- |
| string                | java.lang.String    | NSString                  |
| number(32bit integer) | java.lang.Integer   | NSNumber numberWithInt    |
| number(double)        | java.lang.Double    | NSNumber numberWithDouble |
| boolean               | java.lang.Boolean   | NSNumber numberWithBool   |
| null                  | null                | NSNull                    |
| Array\<S\>            | java.util.ArrayList | NSArray                   |
| Map\<string, T\>      | java.util.HashMap   | NSDictionary              |

> **说明**
>
> S表示stirng、number、boolean类型，T表示S及其对应的数组类型；
> Map类型仅支持string类型的key，且仅用于方法返回。


## 开发指南

[平台桥接开发指南（Android）](../tutorial/how-to-use-bridge-on-android.md)<br />
[平台桥接开发指南（iOS）](../tutorial/how-to-use-bridge-on-ios.md)
