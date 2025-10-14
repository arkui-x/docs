# Class (WebResourceHandler)
通过WebResourceHandler，可以提供自定义的返回头以及返回体给Web组件。

> **说明：**
>
> - 本Class首批接口从API version 22开始支持。
>
> - 示例效果请以真机运行为准，当前DevEco Studio预览器不支持。

## 导入模块

```ts
import { webview } from '@kit.ArkWeb';
```

## didReceiveResponse

didReceiveResponse(response: WebSchemeHandlerResponse): void

将构造的响应头传递给被拦截的请求。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名          | 类型    |  必填  | 说明                                            |
| ---------------| ------- | ---- | ------------- |
| response      | [WebSchemeHandlerResponse](./arkts-apis-webview-WebSchemeHandlerResponse.md)  | 是   | 该拦截请求的响应。 |

**错误码：**

以下错误码的详细介绍请参见[Webview错误码](../errorcodes/errorcode-webview.md)、[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
|  401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified.    |
| 17100021 | The resource handler is invalid. |

**示例：**

示例请参考[OnRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## didReceiveResponseBody

didReceiveResponseBody(data: ArrayBuffer): void

将构造的响应体传递给被拦截的请求。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名          | 类型    |  必填  | 说明                                            |
| ---------------| ------- | ---- | ------------- |
| data      | ArrayBuffer  | 是   | 响应体数据。 |

**错误码：**

以下错误码的详细介绍请参见[Webview错误码](../errorcodes/errorcode-webview.md)、[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
|  401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified.    |
| 17100021 | The resource handler is invalid. |

**示例：**

示例请参考[OnRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## didFinish

didFinish(): void

通知Web组件被拦截的请求已经完成，并且没有更多的数据可用，调用前需要优先调用[didReceiveResponse](#didreceiveresponse)将构造的响应头传递给被拦截的请求。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**错误码：**

以下错误码的详细介绍请参见[Webview错误码](../errorcodes/errorcode-webview.md)。

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 17100021 | The resource handler is invalid. |

**示例：**

示例请参考[OnRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## didFail

didFail(code: WebNetErrorList): void

通知ArkWeb内核被拦截请求应该返回失败，调用前需要优先调用[didReceiveResponse](#didreceiveresponse)将构造的响应头传递给被拦截的请求。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型    |  必填  | 说明                       |
| --------| ------- | ---- | ---------------------------|
|  code | [WebNetErrorList](arkts-apis-netErrorList.md#webneterrorlist) | 是   | 网络错误码。 |

**错误码：**

以下错误码的详细介绍请参见[Webview错误码](../errorcodes/errorcode-webview.md)、[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Incorrect parameter types. |
| 17100021 | The resource handler is invalid. |


**示例：**

示例请参考[OnRequestStart](./arkts-apis-webview-WebSchemeHandler.md#onrequeststart)。

## didFail

didFail(code: WebNetErrorList, completeIfNoResponse: boolean): void

通知ArkWeb内核，被拦截请求应返回失败。若completeIfNoResponse为false，调用前需优先调用[didReceiveResponse](#didreceiveresponse)，将构造的响应头传递给被拦截的请求。若completeIfNoResponse为true，且调用前未调用[didReceiveResponse](#didreceiveresponse)，则自动生成一个响应头，网络错误码为-104，详情参见[WebNetErrorList](arkts-apis-netErrorList.md#webneterrorlist)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型    |  必填  | 说明                       |
| --------| ------- | ---- | ---------------------------|
|  code | [WebNetErrorList](arkts-apis-netErrorList.md#webneterrorlist) | 是   | 网络错误码。 |
|  completeIfNoResponse | boolean | 是   | 调用当前接口时，若之前未调用过[didReceiveResponse](#didreceiveresponse)，是否完成此次网络请求；值为true时，若之前未调用过[didReceiveResponse](#didreceiveresponse)，则会自动生成一个response以完成此次网络请求，网络错误码为-104；值为false时，构建一个空白response以完成此次网络请求，网络错误码为该接口传入的code值。 |

**错误码：**

以下错误码的详细介绍请参见[Webview错误码](../errorcodes/errorcode-webview.md)。

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 17100101 | The errorCode is either ARKWEB_NET_OK or outside the range of error codes in WebNetErrorList. |
| 17100021 | The resource handler is invalid. |

**示例：**

```ts
// xxx.ets
import { webview, WebNetErrorList } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  schemeHandler: webview.WebSchemeHandler = new webview.WebSchemeHandler();

  build() {
    Column() {
      Web({ src: 'https://www.example.com', controller: this.controller })
        .onControllerAttached(() => {
          try {
            this.schemeHandler.onRequestStart((request: webview.WebSchemeHandlerRequest, resourceHandler: webview.WebResourceHandler) => {
              console.info("[schemeHandler] onRequestStart");
              try {
                console.info("[schemeHandler] onRequestStart url:" + request.getRequestUrl());
                console.info("[schemeHandler] onRequestStart method:" + request.getRequestMethod());
                console.info("[schemeHandler] onRequestStart isMainFrame:" + request.isMainFrame());
                console.info("[schemeHandler] onRequestStart hasGesture:" + request.hasGesture());
                console.info("[schemeHandler] onRequestStart header size:" + request.getHeader().length);
                let header = request.getHeader();
                for (let i = 0; i < header.length; i++) {
                  console.info("[schemeHandler] onRequestStart header:" + header[i].headerKey + " " + header[i].headerValue);
                }
              } catch (error) {
                console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
              }

              if (request.getRequestUrl().endsWith("example.com")) {
                return false;
              }

              try {
                // 直接调用didFail(WebNetErrorList.ERR_FAILED, true)，自动构造一个网络请求错误ERR_CONNECTION_FAILED
                resourceHandler.didFail(WebNetErrorList.ERR_FAILED, true);
              } catch (error) {
                // 当error.code为17100101(The errorCode is either ARKWEB_NET_OK or outside the range of error codes in WebNetErrorList)
                // 且didFail(code: WebNetErrorList, completeIfNoResponse: boolean)的code值不为null时，接口会继续调用不会中断。
                console.error(`[schemeHandler] ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
              }
              return true;
            })

            this.schemeHandler.onRequestStop((request: webview.WebSchemeHandlerRequest) => {
              console.info("[schemeHandler] onRequestStop");
            });

            this.controller.setWebSchemeHandler('https', this.schemeHandler);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
        .javaScriptAccess(true)
        .domStorageAccess(true)
    }
  }
}
```