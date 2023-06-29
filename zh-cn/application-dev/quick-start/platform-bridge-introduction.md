# 平台桥接(@arkui-x.bridge)

平台桥接用于客户端（ArkUI）和平台（Android或iOS）之间传递消息，主要用于ArkUI与平台双向传递消息、ArkTS调用平台API、平台调用ArkTS的方法。

以Android平台为例，ArkTS和Java没有相互调用的能力，为了实现ArkTS和Java交互，需要ArkTS与C++交互，C++再与Java交互，反之亦然。但是对于开发者，就像是ArkTS和Java直接交互。

本文介绍如何通过平台桥接编写自定义的Android、iOS平台代码。ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，Android侧参考[BridgePlugin](../reference/arkui-for-android/BridgePlugin.md)，iOS侧参考[BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md)。

> **说明**
>
>  平台桥接支持ArkUI调用Android Java API和iOS Objective-C API。此外，一些平台相关功能可以通过已有的[OpenHarmony跨平台API](../reference/apis/README.md)实现。

## 平台桥接数据类型

平台桥接通过JSON格式序列化编解码传递数据，支持基础数据类型、数组类型和结构化数据。具体支持类型如下表：

| ArkTS                 | Java                | Objective-C               |
| --------------------- | ------------------- | ------------------------- |
| string                | java.lang.String    | NSString                  |
| number(32bit integer) | java.lang.Integer   | NSNumber numberWithInt    |
| number(double)        | java.lang.Double    | NSNumber numberWithDouble |
| boolean               | java.lang.Boolean   | NSNumber numberWithBool   |
| null                  | null                | nil                       |
| Array\<S\>            | java.util.ArrayList | NSArray                   |
| Map\<string, T\>      | java.util.HashMap   | NSDictionary              |

> **说明**
>
> S表示stirng、number、boolean类型，T表示S及其对应的数组类型；
> Map类型仅支持string类型的key，且仅用于方法返回。

## 指南

具体平台桥接的用法参考[Android示例](../tutorial/how-to-use-bridge-on-android.md)和[iOS示例](../tutorial/how-to-use-bridge-on-ios.md)。
