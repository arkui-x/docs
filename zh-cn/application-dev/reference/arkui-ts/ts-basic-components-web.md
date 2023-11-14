# Web

提供具有网页显示能力的Web组件，[@ohos.web.webview](../apis/js-apis-webview.md)提供web控制能力。

> **说明：**
>
> - 该组件从API Version 8开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
> - 示例效果请以真机运行为准，当前IDE预览器不支持。

## 需要权限
#### ArkUI:
访问在线网页时需添加网络权限：ohos.permission.INTERNET。
#### Android:
访问在线网页时需添加网络权限：android.permission.INTERNET

``` <uses-permission android:name="android.permission.INTERNET" />```

#### iOS:
无需设置，应用通过询问用户获取网络权限。

## 跨平台相关设置
### 无法访问Http的设置
#### Android
targetSdkVersion版本升级到28之后，默认拒绝应用程序使用明文流量的请求，如http协议不再支持访问。
需要在AndroidManifest.xml中开启明文网络流量解决此问题
> android:usesCleartextTraffic="true"

如需详细网络安全设置也可以通过配置android:networkSecurityConfig属性来进行详细设置。
#### iOS
App Transport Security (ATS) 不支持访问http服务。通过修改info.plist可以做到。

在源代码模式修改比较方便。
在Xcode中右击项目中的info.plist，选择Open As > Source Code，在plist标签中加入NSAppTransportSecurity。

示例如下：

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


## 子组件

无

## 接口

Web(options: { src: ResourceStr, controller: WebviewController})

> **说明：**
>
> 不支持转场动画。
> 同一页面的多个web组件，必须绑定不同的WebviewController。

**参数：**

| 参数名        | 参数类型                                     | 必填   | 参数描述    |
| ---------- | ---------------------------------------- | ---- | ------- |
| src        | [ResourceStr](ts-types.md)               | 是    | 网页资源地址。如果访问本地资源文件，请使用$rawfile。 |
| controller | [WebviewController<sup>9+</sup>](../apis/js-apis-webview.md#webviewcontroller)| 是    | 控制器。 |

**示例：**

  加载在线网页

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

  加载本地网页

  ```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview'

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    build() {
      Column() {
        // 通过$rawfile加载本地资源文件。
        Web({ src: $rawfile("index.html"), controller: this.controller })
      }
    }
  }
  ```

  加载的index.html文件，位于resources目录下rawfile子目录中

  ```html
  <!-- index.html -->
  <!DOCTYPE html>
  <html>
      <body>
          <p>Hello World</p>
      </body>
  </html>
  ```

## 属性

### javaScriptAccess

javaScriptAccess(javaScriptAccess: boolean)

设置是否允许执行JavaScript脚本，默认允许执行。

**参数：**

| 参数名              | 参数类型    | 必填   | 默认值  | 参数描述                |
| ---------------- | ------- | ---- | ---- | ------------------- |
| javaScriptAccess | boolean | 是    | true | 是否允许执行JavaScript脚本。 |

**示例：**

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

设置是否支持手势进行缩放，默认允许执行缩放。

**参数：**

| 参数名        | 参数类型    | 必填   | 默认值  | 参数描述          |
| ---------- | ------- | ---- | ---- | ------------- |
| zoomAccess | boolean | 是    | true | 设置是否支持手势进行缩放。 |

**示例：**

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

网页开始加载时触发该回调，且只在主frame触发，iframe或者frameset的内容加载时不会触发此回调。  
Android和iOS的触发时机与OpenHarmony不完全相同，以各平台行为为准。

**参数：**

| 参数名  | 参数类型   | 参数描述      |
| ---- | ------ | --------- |
| url  | string | 页面的URL地址。 |

**示例：**

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

网页加载完成时触发该回调，且只在主frame触发。

**参数：**

| 参数名  | 参数类型   | 参数描述      |
| ---- | ------ | --------- |
| url  | string | 页面的URL地址。 |

**示例：**

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

网页加载遇到错误时触发该回调。出于性能考虑，建议此回调中尽量执行简单逻辑。


**参数：**

| 参数名     | 参数类型                                     | 参数描述            |
| ------- | ---------------------------------------- | --------------- |
| request | [WebResourceRequest](#webresourcerequest) | 网页请求的封装信息。      |
| error   | [WebResourceError](#webresourceerror)    | 网页加载资源错误的封装信息 。 |

**示例：**

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

设置网页字体大小最小值。

**参数：**

| 参数名 | 参数类型 | 必填 | 默认值 | 参数描述                                                     |
| ------ | -------- | ---- | ------ | ------------------------------------------------------------ |
| size   | number   | 是   | 8      | 设置网页字体大小最小值，单位px。输入值的范围为-2^31到2^31-1，实际渲染时超过72的值按照72进行渲染，低于1的值按照1进行渲染。 |

**示例：**

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

设置是否显示横向滚动条，包括系统默认滚动条和用户自定义滚动条。默认显示。

**参数：**

| 参数名              | 参数类型 | 必填 | 默认值 | 参数描述                 |
| ------------------- | -------- | ---- | ------ | ------------------------ |
| horizontalScrollBar | boolean  | 是   | true   | 设置是否显示横向滚动条。 |

**示例：**

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

加载的html文件。

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

设置是否显示纵向滚动条，包括系统默认滚动条和用户自定义滚动条。默认显示。

**参数：**

| 参数名                  | 参数类型 | 必填 | 默认值 | 参数描述                 |
| ----------------------- | -------- | ---- | ------ | ------------------------ |
| verticalScrollBarAccess | boolean  | 是   | true   | 设置是否显示纵向滚动条。 |

**示例：**

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

 加载的html文件。

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

设置web组件背景颜色

**参数：**

**ResourceColor**

| 类型                                                         | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Color](https://gitee.com/arkui-x2/docs/blob/master/zh-cn/application-dev/reference/arkui-ts/ts-appendix-enums.md) | 颜色枚举值。                                                 |
| number                                                       | HEX格式颜色，支持rgb。示例：0xffffff。                       |
| string                                                       | rgb或者argb格式颜色。示例：'#ffffff', '#ff000000', 'rgb(255, 100, 255)', 'rgba(255, 100, 255, 0.5)'。 |

**示例：**

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
      Text('设置backgroundColor为Color.Blue').height(50)
      Web({ src: 'https://www.example.com', controller: this.controller1 })
        .backgroundColor(Color.Blue).height(200)
        
      Text('设置backgroundColor为0x00ffff').height(50)
      Web({ src: 'https://www.example.com', controller: this.controller2 })
        .backgroundColor(0x00ffff).height(200)
        
      Text('设置backgroundColor为"#00FF00"').height(50)
      Web({ src: 'https://www.example.com', controller: this.controller3 })
        .backgroundColor('#00FF00').height(200)
        
      Text('设置backgroundColor为"rgb(255, 100, 255)"').height(50)
      Web({ src: 'https://www.example.com', controller: this.controller4 })
        .backgroundColor('rgb(255, 100, 255)').height(200)
    }
  }
}
  ```

### mediaPlayGestureAccess

mediaPlayGestureAccess(access: boolean)

设置有声视频播放是否需要用户手动点击，静音视频播放不受该接口管控，默认需要。
Android：该属性只对网页内嵌视频播放生效。
iOS：不支持。

**参数：**

| 参数名 | 参数类型 | 必填 | 默认值 | 参数描述                               |
| ------ | -------- | ---- | ------ | -------------------------------------- |
| access | boolean  | 是   | true   | 设置有声视频播放是否需要用户手动点击。 |

**示例：**

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

网页加载资源遇到的HTTP错误（响应码>=400）时触发该回调。

**参数：**

| 参数名   | 参数类型                                    | 参数描述                                                     |
| -------- | ------------------------------------------- | ------------------------------------------------------------ |
| request  | [WebResourceRequest](#webresourcerequest)   | 网页请求的封装信息。                    |
| response | [WebResourceResponse](#webresourceresponse) | 资源响应的封装信息。 |

**示例：**

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

网页加载进度变化时触发该回调。

**参数：**

| 参数名      | 参数类型 | 参数描述                               |
| ----------- | -------- | -------------------------------------- |
| newProgress | number   | 新的加载进度，取值范围为0到100的整数。 |

**示例：**

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

通知网页滚动条滚动位置。

**参数：**

| 参数名  | 参数类型 | 参数描述                                     |
| ------- | -------- | -------------------------------------------- |
| xOffset | number   | 以网页最左端为基准，水平滚动条滚动所在位置。 |
| yOffset | number   | 以网页最上端为基准，竖直滚动条滚动所在位置。 |

**示例：**

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

网页document标题更改时触发该回调。
Android和iOS的返回值与OpenHarmony不完全相同，以各平台行为为准。

**参数：**

| 参数名 | 参数类型 | 参数描述           |
| ------ | -------- | ------------------ |
| title  | string   | document标题内容。 |

**示例：**

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

通知宿主应用JavaScript console消息。

**参数：**

| 参数名  | 参数类型                          | 参数描述                                                    |
| ------- | --------------------------------- | ----------------------------------------------------------- |
| message | [ConsoleMessage](#consolemessage) | 触发的控制台信息。 |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 当返回true时，该条消息将不会再打印至控制台，反之仍会打印至控制台。iOS不支持。 |

**示例：**

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

当前页面显示比例的变化时触发该回调。
Android和iOS的页面显示比例与OpenHarmony不完全相同，以各平台行为为准。

**参数：**

| 参数名   | 参数类型 | 参数描述                 |
| -------- | -------- | ------------------------ |
| oldScale | number   | 变化前的显示比例百分比。 |
| newScale | number   | 变化后的显示比例百分比。 |

**示例：**

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

当Web组件加载url之前触发该回调，用于判断是否阻止此次访问。默认允许加载。
在Android平台，此接口在重定向时触发。

**参数：**

| 参数名  | 参数类型                                  | 参数描述            |
| ------- | ----------------------------------------- | ------------------- |
| request | [WebResourceRequest](#webresourcerequest) | url请求的相关信息。 |

**返回值：**

| 类型    | 说明                                         |
| ------- | -------------------------------------------- |
| boolean | 返回true表示阻止此次加载，否则允许此次加载。 |

**示例：**

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

当Controller成功绑定到Web组件时触发该回调，并且该Controller必须为WebviewController，因该回调调用时网页还未加载，无法在回调中使用有关操作网页的接口，可以使用[loadUrl](../apis/js-apis-webview.md#loadurl)等操作网页不相关的接口。

**示例：**

在该回调中使用loadUrl加载网页

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

 加载的html文件。

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

web组件资源管理错误信息对象。示例代码参考[onErrorReceive事件](#onerrorreceive)。

### getErrorCode

getErrorCode(): number

获取加载资源的错误码。

**错误码：**

Android和iOS的错误码与OpenHarmony不完全相同，以各平台错误码为准。

**返回值：**

| 类型     | 说明          |
| ------ | ----------- |
| number | 返回加载资源的错误码。 |

### getErrorInfo

getErrorInfo(): string

获取加载资源的错误信息。  
Android和iOS的错误信息与OpenHarmony不完全相同，以各平台错误信息为准。

**返回值：**

| 类型     | 说明           |
| ------ | ------------ |
| string | 返回加载资源的错误信息。 |

## WebResourceRequest

web组件获取资源请求对象。示例代码参考[onErrorReceive事件](#onerrorreceive)。


### getRequestUrl

getRequestUrl(): string

获取资源请求的URL信息。

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| string | 返回资源请求的URL信息。 |

## WebResourceResponse

web组件资源响应对象。示例代码参考[onHttpErrorReceive事件](#onhttperrorreceive)。

###  getResponseMimeType

getResponseMimeType(): string

获取资源响应的媒体（MIME）类型。

**返回值：**

| 类型   | 说明                             |
| ------ | -------------------------------- |
| string | 返回资源响应的媒体（MIME）类型。 |

###  getResponseEncoding

getResponseEncoding(): string

获取资源响应的编码。

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| string | 返回资源响应的编码。 |

###  getResponseCode

getResponseCode(): number

获取资源响应的状态码。

**返回值：**

| 类型   | 说明                   |
| ------ | ---------------------- |
| number | 返回资源响应的状态码。 |

## ConsoleMessage

Web组件获取控制台信息对象。示例代码参考[onConsole事件](#onconsole)。

### getMessage

getMessage(): string

获取ConsoleMessage的日志信息。

**返回值：**

| 类型   | 说明                           |
| ------ | ------------------------------ |
| string | 返回ConsoleMessage的日志信息。 |

### getMessageLevel

getMessageLevel(): MessageLevel

获取ConsoleMessage的信息级别。

**返回值：**

| 类型                                  | 说明                           |
| ------------------------------------- | ------------------------------ |
| [MessageLevel](#messagelevel枚举说明) | 返回ConsoleMessage的信息级别。 |

##  MessageLevel枚举说明

| 名称  | 描述       |
| ----- | ---------- |
| Debug | 调试级别。 |
| Error | 错误级别。 |
| Info  | 消息级别。 |
| Log   | 日志级别。 |
| Warn  | 警告级别。 |