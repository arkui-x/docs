# Web

The **<Web\>** component can be used to display web pages. It can be used with the [@ohos.web.webview](../apis/js-apis-webview.md) module, which provides APIs for web control.

> **NOTE**
>
> - This component is supported since API version 8. Updates will be marked with a superscript to indicate their earliest API version.
> - You can preview how this component looks on a real device. The preview is not yet available in the DevEco Studio Previewer.

## Required Permissions
#### ArkUI:
ohos.permission.INTERNET, required only for accessing online web pages
#### Android:
android.permission.INTERNET, required only for accessing online web pages.

` <uses-permission android:name="android.permission.INTERNET" />`

#### iOS:
No setting is required. The application needs to prompt the user to obtain the Internet access permission.

## Cross-Platform Settings
### Settings for Fixing the HTTP Access Denied Issue
#### Android
After the target SDK version is upgraded to 28, cleartext traffic in applications is restricted by default. In this case, HTTP access is not allowed.
To use HTTP for access, enable plaintext network traffic in the AndroidManifest.xml file.
> android:usesCleartextTraffic="true"

You can also configure more network security settings through **android:networkSecurityConfig**.
#### iOS
Due to the App Transport Security (ATS) feature in iOS, applications cannot use secure HTTPS connections by default. If access via HTTP is needed, exceptions must be defined in the **Info.plist** file.

It is convenient to modify the file in source code mode.
In Xcode, right-click the **info.plist** file of the application project, choose **Open As** > **Source Code**, and add **NSAppTransportSecurity** under **plist**.

Example:

```
<plist version="1.0">
<dict>
	<key>NSAppTransportSecurity</key>
	<dict>
		<key>NSAllowsArbitraryLoads</key>
		<true/>
	</dict>
</dict>
</plist>
```


## Child Components

Not supported

## APIs

Web(options: { src: ResourceStr, controller: WebviewController})

> **NOTE**
>
> Transition animation is not supported.
> **\<Web>** components on a page must be bound to different **WebviewController**s.

**Parameters**

| Name       | Type                                    | Mandatory  | Description   |
| ---------- | ---------------------------------------- | ---- | ------- |
| src        | [ResourceStr](ts-types.md)               | Yes   | Address of a web page resource. To access local resource files, use $rawfile.|
| controller | [WebviewController<sup>9+</sup>](../apis/js-apis-webview.md#webviewcontroller)| Yes   | Controller.|

**Example**

  Example of loading online web pages:

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
      }
    }
  }
  ```

  Example of loading local web pages:

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    build() {
      Column() {
        // Load a local resource file through $rawfile.
        Web({ src: $rawfile("index.html"), controller: this.controller })
      }
    }
  }
  ```

  The loaded **index.html** file is stored in the **rawfile** folder under **resources**.

  ```html
  <!-- index.html -->
  <!DOCTYPE html>
  <html>
      <body>
          <p>Hello World</p>
      </body>
  </html>
  ```

## Attributes

### javaScriptAccess

javaScriptAccess(javaScriptAccess: boolean)

Sets whether JavaScript scripts can be executed. By default, JavaScript scripts can be executed.

**Parameters**

| Name             | Type   | Mandatory  | Default Value | Description               |
| ---------------- | ------- | ---- | ---- | ------------------- |
| javaScriptAccess | boolean | Yes   | true | Whether JavaScript scripts can be executed.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .javaScriptAccess(true)
      }
    }
  }
  ```

### zoomAccess

zoomAccess(zoomAccess: boolean)

Sets whether to enable zoom gestures. By default, this feature is enabled.

**Parameters**

| Name       | Type   | Mandatory  | Default Value | Description         |
| ---------- | ------- | ---- | ---- | ------------- |
| zoomAccess | boolean | Yes   | true | Whether to enable zoom gestures.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .zoomAccess(true)
      }
    }
  }
  ```

### onPageBegin

onPageBegin(callback: (event?: { url: string }) => void)

Called when the web page starts to be loaded. This API is called only for the main frame content, and not for the iframe or frameset content. 

When this API is called varies by platform.

**Parameters**

| Name | Type  | Description     |
| ---- | ------ | --------- |
| url  | string | URL of the page.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onPageBegin((event) => {
            if (event) {
              console.log('url:' + event.url)
            }
          })
      }
    }
  }
  ```

### onPageEnd

onPageEnd(callback: (event?: { url: string }) => void)

Called when the web page loading is complete. This API takes effect only for the main frame content.

**Parameters**

| Name | Type  | Description     |
| ---- | ------ | --------- |
| url  | string | URL of the page.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onPageEnd((event) => {
            if (event) {
              console.log('url:' + event.url)
            }
          })
      }
    }
  }
  ```

### onErrorReceive

onErrorReceive(callback: (event?: { request: WebResourceRequest, error: WebResourceError }) => void)

Called when an error occurs during web page loading. For better results, simplify the implementation logic in the callback.


**Parameters**

| Name    | Type                                    | Description           |
| ------- | ---------------------------------------- | --------------- |
| request | [WebResourceRequest](#webresourcerequest) | Encapsulation of a web page request.     |
| error   | [WebResourceError](#webresourceerror)    | Encapsulation of a web page resource loading error.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onErrorReceive((event) => {
            if (event) {
              console.log('getErrorInfo:' + event.error.getErrorInfo())
              console.log('getErrorCode:' + event.error.getErrorCode())
              console.log('url:' + event.request.getRequestUrl())
            }
          })
      }
    }
  }
  ```

### minFontSize<sup>9+</sup>

minFontSize(size: number)

Sets the minimum font size for the web page.

**Parameters**

| Name| Type| Mandatory| Default Value| Description                                                    |
| ------ | -------- | ---- | ------ | ------------------------------------------------------------ |
| size   | number   | Yes  | 8      | Minimum font size to set, in px. The value ranges from -2^31 to 2^31-1. In actual rendering, values greater than 72 are handled as 72, and values less than 1 are handled as 1.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    @State fontSize: number = 30
    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .minFontSize(this.fontSize)
      }
    }
  }
  ```

### horizontalScrollBarAccess<sup>9+</sup>

horizontalScrollBarAccess(horizontalScrollBar: boolean)

Sets whether to display the horizontal scrollbar, including the default system scrollbar and custom scrollbar. By default, the horizontal scrollbar is displayed.

**Parameters**

| Name             | Type| Mandatory| Default Value| Description                |
| ------------------- | -------- | ---- | ------ | ------------------------ |
| horizontalScrollBar | boolean  | Yes  | true   | Whether to display the horizontal scrollbar.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
      
    build() {
      Column() {
        Web({ src: $rawfile('index.html'), controller: this.controller })
        .horizontalScrollBarAccess(true).height(150).width(200)
      }
    }
  }
  ```

HTML file to be loaded:

```html
<!--index.html-->
<!DOCTYPE html>
<html>
  <head>
    <title>Demo</title>
    <style>
      body {
        width:3000px;
        height:3000px;
        padding-right:170px;
        padding-left:170px;
        border:5px solid blueviolet
      }
    </style>
  </head>
  <body>
    Scroll Test
  </body>
</html>
```

### verticalScrollBarAccess<sup>9+</sup>

verticalScrollBarAccess(verticalScrollBar: boolean)

Sets whether to display the vertical scrollbar, including the default system scrollbar and custom scrollbar. By default, the vertical scrollbar is displayed.

**Parameters**

| Name                 | Type| Mandatory| Default Value| Description                |
| ----------------------- | -------- | ---- | ------ | ------------------------ |
| verticalScrollBarAccess | boolean  | Yes  | true   | Whether to display the vertical scrollbar.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new w eb_webview.WebviewController()

    build() {
      Column() {
        Web({ src: $rawfile('index.html'), controller: this.controller })
        .verticalScrollBarAccess(true).height(150).width(200)
      }
    }
  }
  ```

 HTML file to be loaded:

```html
<!--index.html-->
<!DOCTYPE html>
<html>
  <head>
    <title>Demo</title>
    <style>
      body {
        width:3000px;
        height:3000px;
        padding-right:170px;
        padding-left:170px;
        border:5px solid blueviolet
      }
    </style>
  </head>
  <body>
    Scroll Test
  </body>
</html>
```

### backgroundColor

backgroundColor(ResourceColor:Color | number | string)

Sets the background color for the **\<Web>** component.

**Parameters**

**ResourceColor**

| Type                                                        | Description                                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Color](https://gitee.com/arkui-x/docs/blob/ArkUI-X-5.0-Beta1/zh-cn/application-dev/reference/arkui-ts/ts-appendix-enums.md) | Color enums.                                                |
| number                                                       | Color in hexadecimal format. RGB is supported. Example: **0xffffff**                      |
| string                                                       | Color in RGB or ARGB notation. Example: **'#ffffff', '#ff000000', 'rgb(255, 100, 255)', 'rgba(255, 100, 255, 0.5)'**|

**Example**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';

@Entry
@Component
struct WebComponent {
  controller1: web_webview.WebviewController = new web_webview.WebviewController();
  controller2: web_webview.WebviewController = new web_webview.WebviewController();
  controller3: web_webview.WebviewController = new web_webview.WebviewController();
  controller4: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Text ('Set backgroundColor to Color.Blue').height (50)
      Web({ src: 'https://www.example.com', controller: this.controller1 })
        .backgroundColor(Color.Blue).height(200)
        
      Text ('Set backgroundColor to 0x00ffff').height (50)
      Web({ src: 'https://www.example.com', controller: this.controller2 })
        .backgroundColor(0x00ffff).height(200)
        
      Text ('Set backgroundColor to "#00FF00"').height (50)
      Web({ src: 'https://www.example.com', controller: this.controller3 })
        .backgroundColor('#00FF00').height(200)
        
      Text('Set backgroundColor to "rgb(255, 100, 255)"').height(50)
      Web({ src: 'https://www.example.com', controller: this.controller4 })
        .backgroundColor('rgb(255, 100, 255)').height(200)
    }
  }
}
  ```

### mediaPlayGestureAccess

mediaPlayGestureAccess(access: boolean)

Sets whether video playback must be started by user gestures. This API is not applicable to videos that do not have an audio track or whose audio track is muted.

Android: This API takes effect only for videos embedded in web pages.

iOS: This API is not supported.

**Parameters**

| Name| Type| Mandatory| Default Value| Description                              |
| ------ | -------- | ---- | ------ | -------------------------------------- |
| access | boolean  | Yes  | true   | Whether video playback must be started by user gestures.|

**Example**

```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    @State access: boolean = true
    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .mediaPlayGestureAccess(this.access)
      }
    }
  }
```

### onHttpErrorReceive

onHttpErrorReceive(callback: (event?: { request: WebResourceRequest, response: WebResourceResponse }) => void)

Called when an HTTP error (the response code is greater than or equal to 400) occurs during web page resource loading.

**Parameters**

| Name  | Type                                   | Description                                                    |
| -------- | ------------------------------------------- | ------------------------------------------------------------ |
| request  | [WebResourceRequest](#webresourcerequest)   | Encapsulation of a web page request.                   |
| response | [WebResourceResponse](#webresourceresponse) | Encapsulation of a resource response.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onHttpErrorReceive((event) => {
            if (event) {
              console.log('url:' + event.request.getRequestUrl())
              console.log('getResponseEncoding:' + event.response.getResponseEncoding())
              console.log('getResponseMimeType:' + event.response.getResponseMimeType())
              console.log('getResponseCode:' + event.response.getResponseCode())
              let result = event.request.getRequestHeader()
              console.log('The request header result size is ' + result.length)
              for (let i of result) {
                console.log('The request header key is : ' + i.headerKey + ' , value is : ' + i.headerValue)
              }
              let resph = event.response.getResponseHeader()
              console.log('The response header result size is ' + resph.length)
              for (let i of resph) {
                console.log('The response header key is : ' + i.headerKey + ' , value is : ' + i.headerValue)
              }
            }
          })
      }
    }
  }
  ```

### onProgressChange

onProgressChange(callback: (event?: { newProgress: number }) => void)

Called when the web page loading progress changes.

**Parameters**

| Name     | Type| Description                              |
| ----------- | -------- | -------------------------------------- |
| newProgress | number   | New loading progress. The value is an integer ranging from 0 to 100.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onProgressChange((event) => {
            if (event) {
              console.log('newProgress:' + event.newProgress)
            }
          })
      }
    }
  }
  ```

### onScroll<sup>9+</sup>

onScroll(callback: (event: {xOffset: number, yOffset: number}) => void)

Called when the scrollbar of the page scrolls.

**Parameters**

| Name | Type| Description                                    |
| ------- | -------- | -------------------------------------------- |
| xOffset | number   | Position of the scrollbar on the x-axis relative to the leftmost of the web page.|
| yOffset | number   | Position of the scrollbar on the y-axis relative to the top of the web page.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
        .onScroll((event) => {
            console.info("x = " + event.xOffset)
            console.info("y = " + event.yOffset)
        })
      }
    }
  }
  ```

### onTitleReceive

onTitleReceive(callback: (event?: { title: string }) => void)

Called when the document title of the web page is changed.

The return value varies by platform.

**Parameters**

| Name| Type| Description          |
| ------ | -------- | ------------------ |
| title  | string   | Document title.|

**Example**

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onTitleReceive((event) => {
            if (event) {
              console.log('title:' + event.title)
            }
          })
      }
    }
  }
  ```

### onConsole

onConsole(callback: (event?: { message: ConsoleMessage }) => boolean)

Called to notify the host application of a JavaScript console message.

**Parameters**

| Name | Type                         | Description                                                   |
| ------- | --------------------------------- | ----------------------------------------------------------- |
| message | [ConsoleMessage](#consolemessage) | Console message.|

**Return value**

| Type   | Description                                                        |
| ------- | ------------------------------------------------------------ |
| boolean | Returns **true** if the message will not be printed to the console; returns **false** otherwise.<br>This API is not supported on iOS. |

**Example**

```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onConsole((event) => {
            if (event) {
              console.log('getMessage:' + event.message.getMessage())
              console.log('getMessageLevel:' + event.message.getMessageLevel())
            }
            return false
          })
      }
    }
  }
```

### onScaleChange<sup>9+</sup>

onScaleChange(callback: (event: {oldScale: number, newScale: number}) => void)

Called when the display ratio of this page changes.

The page display ratio varies by platform.

**Parameters**

| Name  | Type| Description                |
| -------- | -------- | ------------------------ |
| oldScale | number   | Display ratio of the page before the change.|
| newScale | number   | Display ratio of the page after the change.|

**Example**

```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onScaleChange((event) => {
            console.log('onScaleChange changed from ' + event.oldScale + ' to ' + event.newScale)
          })
      }
    }
  }
```

### onLoadIntercept<sup>10+</sup>

onLoadIntercept(callback: (event?: { data: WebResourceRequest }) => boolean)

Called when the **\<Web>** component is about to access a URL. This API is used to determine whether to block the access, which is allowed by default.
On Android, this API is called when redirection occurs.

**Parameters**

| Name | Type                                 | Description           |
| ------- | ----------------------------------------- | ------------------- |
| request | [WebResourceRequest](#webresourcerequest) | Information about the URL request.|

**Return value**

| Type   | Description                                        |
| ------- | -------------------------------------------- |
| boolean | Returns **true** if the access is blocked; returns **false** otherwise.|

**Example**

```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: 'https://www.example.com', controller: this.controller })
          .onLoadIntercept((event) => {
            console.log('url:' + event.data.getRequestUrl())
            return true
          })
      }
    }
  }
```

### onControllerAttached<sup>10+</sup>

onControllerAttached(callback: () => void)

Called when the controller is successfully bound to the **\<Web>** component. The controller must be WebviewController. As the web page is not yet loaded when this callback is called, APIs for operating the web page cannot be used in the callback. Other APIs, such as [loadUrl](../apis/js-apis-webview.md#loadurl), which do not involve web page operations, can be used properly.

**Example**

The following example uses **loadUrl** in the callback to load the web page.

```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()

    build() {
      Column() {
        Web({ src: '', controller: this.controller })
          .onControllerAttached(() => {
            this.controller.loadUrl($rawfile("index.html"));
          })
      }
    }
  }
```

 HTML file to be loaded:

```html
  <!-- index.html -->
  <!DOCTYPE html>
  <html>
      <body>
          <p>Hello World</p>
      </body>
  </html>
```

## WebResourceError

Implements the **WebResourceError** object. For the sample code, see [onErrorReceive](#onerrorreceive).

### getErrorCode

getErrorCode(): number

Obtains the error code for resource loading.

**Error codes**

The error codes vary by platform.

**Return value**

| Type    | Description         |
| ------ | ----------- |
| number | Error code for resource loading.|

### getErrorInfo

getErrorInfo(): string

Obtains error information about resource loading. 

The error information varies by platform.

**Return value**

| Type    | Description          |
| ------ | ------------ |
| string | Error information about resource loading.|

## WebResourceRequest

Implements the **WebResourceRequest** object. For the sample code, see [onErrorReceive](#onerrorreceive).


### getRequestUrl

getRequestUrl(): string

Obtains the URL of the resource request.

**Return value**

| Type    | Description           |
| ------ | ------------- |
| string | URL of the resource request.|

## WebResourceResponse

Implements the **WebResourceResponse** object. For the sample code, see [onHttpErrorReceive](#onhttperrorreceive).

###  getResponseMimeType

getResponseMimeType(): string

Obtains the MIME type of the resource response.

**Return value**

| Type  | Description                            |
| ------ | -------------------------------- |
| string | MIME type of the resource response.|

###  getResponseEncoding

getResponseEncoding(): string

Obtains the encoding string of the resource response.

**Return value**

| Type  | Description                |
| ------ | -------------------- |
| string | Encoding string of the resource response.|

###  getResponseCode

getResponseCode(): number

Obtains the status code of the resource response.

**Return value**

| Type  | Description                  |
| ------ | ---------------------- |
| number | Status code of the resource response.|

## ConsoleMessage

Implements the **ConsoleMessage** object. For the sample code, see [onConsole](#onconsole).

### getMessage

getMessage(): string

Obtains the log information of this console message.

**Return value**

| Type  | Description                          |
| ------ | ------------------------------ |
| string | Log information of the console message.|

### getMessageLevel

getMessageLevel(): MessageLevel

Obtains the level of this console message.

**Return value**

| Type                                 | Description                          |
| ------------------------------------- | ------------------------------ |
| [MessageLevel](#messagelevel)| Level of the console message.|

##  MessageLevel

| Name | Description      |
| ----- | ---------- |
| Debug | Debug level.|
| Error | Error level.|
| Info  | Information level.|
| Log   | Log level.|
| Warn  | Warning level. |
