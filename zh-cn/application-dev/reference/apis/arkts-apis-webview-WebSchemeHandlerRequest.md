# Class (WebSchemeHandlerRequest)
通过WebSchemeHandler拦截到的请求。

> **说明：**
>
> - 本Class首批接口从API version 22开始支持。
>
> - 示例效果请以真机运行为准，当前DevEco Studio预览器不支持。

## getHeader

getHeader(): Array\<WebHeader\>

获取资源请求头信息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型               | 说明                 |
| ------------------ | -------------------- |
| Array\<WebHeader\> | 返回资源请求头信息。 |

**示例：**

完整示例代码参考[onRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## getRequestUrl

getRequestUrl(): string

获取资源请求的URL信息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| string | 返回资源请求的URL信息。 |

**示例：**

完整示例代码参考[onRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## getRequestMethod

getRequestMethod(): string

获取请求方法。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| string | 返回请求方法。 |

**示例：**

完整示例代码参考[onRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## isMainFrame

isMainFrame(): boolean

判断资源请求是否为主frame。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| boolean | 判断资源请求是否为主frame，如果资源请求是主frame则返回true，否则返回false。 |

**示例：**

完整示例代码参考[onRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## hasGesture

hasGesture(): boolean

获取资源请求是否与手势（如点击）相关联。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| boolean | 返回资源请求是否与手势（如点击）相关联，如果返回资源请求与手势相关联则返回true，否则返回false。 |

**示例：**

完整示例代码参考[onRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

