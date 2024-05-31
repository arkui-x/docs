# Platform Bridge (@arkui-x.bridge)

## Overview

Platform bridge is used for interaction between the client (ArkUI) and the platform (Android or iOS). It allows you to exchange messages between ArkUI and the platform, call platform APIs on ArkUI, and call ArkUI APIs on the platform.

For example, ArkTS and Java code do not support bidirectional invoking. To implement interaction between ArkTS and Java code, ArkTS code must interact with C++ code, and then the C++ code interacts with Java code, and vice versa. However, this looks like direct interaction between ArkTS and Java code from the perspective of developers.

This top describes how to compile custom Android and iOS platform code through platform bridge. For details about the usage on ArkUI, Android, and iOS, see [Bridge API](../reference/apis/js-apis-bridge.md), [BridgePlugin](../reference/arkui-for-android/BridgePlugin.md), and [BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md), respectively.


## Use Cases and Capabilities

### Use Cases

Platform bridge is mainly used in the following scenario: An application need to reuse platform code, but OpenHarmony does not provide corresponding cross-platform APIs (excluding UI-related APIs).

Platform bridge can be used in the following scenarios:

- Transmitting data such as JSON data and images between ArkUI and the platform.
- Calling platform APIs on ArkUI, for example, calling APIs to obtain the battery level of an Android or iOS device or reusing third-party libraries on the platform.
- Calling ArkUI APIs on the platform, for example, reusing third-party libraries of JavaScript.

> **NOTE**
>
>  Platform bridge allows ArkUI to call Android Java APIs and iOS Objective-C APIs. OpenHarmony also provides [APIs with cross-platform support](../reference/apis/README.md) to implement some platform-related features.


### Data Types Supported

Platform bridge transfers data through serialization, encoding, and decoding in JSON format. Basic data types, array types, and structured data are supported, as described in the table below.

| ArkTS                 | Java                | Objective-C               |
| --------------------- | ------------------- | ------------------------- |
| string                | java.lang.String    | NSString                  |
| number(32bit integer) | java.lang.Integer   | NSNumber numberWithInt    |
| number(double)        | java.lang.Double    | NSNumber numberWithDouble |
| boolean               | java.lang.Boolean   | NSNumber numberWithBool   |
| null                  | null                | NSNull                    |
| Array\<S\>            | java.util.ArrayList | NSArray                   |
| Map\<string, T\>      | java.util.HashMap   | NSDictionary              |

> **NOTE**
>
> S indicates the string, number, or boolean type. T indicates S and its corresponding array type.
> The Map type supports only keys of the string type and is used only in return values.


## Developer Guide

[Platform Bridge Development (Android)](../tutorial/how-to-use-bridge-on-android.md)

[Platform Bridge Development (iOS)](../tutorial/how-to-use-bridge-on-ios.md)