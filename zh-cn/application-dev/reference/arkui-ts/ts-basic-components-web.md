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
| [Color](./ts-appendix-enums.md)                              | 颜色枚举值。                                                  |
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

### onAlert

onAlert(callback: (event?: { url: string; message: string; result: JsResult }) => boolean)

网页触发alert()告警弹窗时触发回调。

**参数：**

| 参数名  | 参数类型              | 参数描述                                                     |
| ------- | --------------------- | ------------------------------------------------------------ |
| url     | string                | 当前显示弹窗所在网页的URL。                                  |
| message | string                | 弹窗中显示的信息。                                           |
| result  | [JsResult](#jsresult) | 通知Web组件用户操作行为，iOS端时result.handleCancel行为和result.handleConfirm一致。 |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 当回调返回true时，应用可以调用系统弹窗能力（包括确认和取消），并且需要根据用户的确认或取消操作调用JsResult通知Web组件最终是否离开当前页面。当回调返回false时，web组件暂不支持触发默认弹窗。 |

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
        Web({ src: $rawfile("index.html"), controller: this.controller })
          .onAlert((event) => {
            if (event) {
              console.log("event.url:" + event.url)
              console.log("event.message:" + event.message)
              AlertDialog.show({
                title: 'onAlert',
                message: 'text',
                primaryButton: {
                  value: 'cancel',
                  action: () => {
                    event.result.handleCancel()
                  }
                },
                secondaryButton: {
                  value: 'ok',
                  action: () => {
                    event.result.handleConfirm()
                  }
                },
                cancel: () => {
                  event.result.handleCancel()
                }
              })
            }
            return true
          })
      }
    }
  }
```

### onConfirm

onConfirm(callback: (event?: { url: string; message: string; result: JsResult }) => boolean)

网页调用confirm()告警时触发此回调。

**参数：**

| 参数名  | 参数类型              | 参数描述                    |
| ------- | --------------------- | --------------------------- |
| url     | string                | 当前显示弹窗所在网页的URL。 |
| message | string                | 弹窗中显示的信息。          |
| result  | [JsResult](#jsresult) | 通知Web组件用户操作行为。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 当回调返回true时，应用可以调用系统弹窗能力（包括确认和取消），并且需要根据用户的确认或取消操作调用JsResult通知Web组件。当回调返回false时，web组件暂不支持触发默认弹窗，跨平台目前这个返回值没有作用。 |

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
        Web({ src: $rawfile("index.html"), controller: this.controller })
          .onConfirm((event) => {
            if (event) {
              console.log("event.url:" + event.url)
              console.log("event.message:" + event.message)
              AlertDialog.show({
                title: 'onConfirm',
                message: 'text',
                primaryButton: {
                  value: 'cancel',
                  action: () => {
                    event.result.handleCancel()
                  }
                },
                secondaryButton: {
                  value: 'ok',
                  action: () => {
                    event.result.handleConfirm()
                  }
                },
                cancel: () => {
                  event.result.handleCancel()
                }
              })
            }
            return true
          })
      }
    }
  }
```

### onPrompt<sup>9+</sup>

onPrompt(callback: (event?: { url: string; message: string; value: string; result: JsResult }) => boolean)

**参数：**

| 参数名  | 参数类型              | 参数描述                    |
| ------- | --------------------- | --------------------------- |
| url     | string                | 当前显示弹窗所在网页的URL。 |
| message | string                | 弹窗中显示的信息。          |
| result  | [JsResult](#jsresult) | 通知Web组件用户操作行为。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 当回调返回true时，应用可以调用系统弹窗能力（包括确认和取消），并且需要根据用户的确认或取消操作调用JsResult通知Web组件。当回调返回false时，web组件暂不支持触发默认弹窗，跨平台目前这个返回值没有作用。 |

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
        Web({ src: $rawfile("index.html"), controller: this.controller })
          .onPrompt((event) => {
            if (event) {
              console.log("url:" + event.url)
              console.log("message:" + event.message)
              console.log("value:" + event.value)
              AlertDialog.show({
                title: 'onPrompt',
                message: 'text',
                primaryButton: {
                  value: 'cancel',
                  action: () => {
                    event.result.handleCancel()
                  }
                },
                secondaryButton: {
                  value: 'ok',
                  action: () => {
                    event.result.handlePromptConfirm(event.value)
                  }
                },
                cancel: () => {
                  event.result.handleCancel()
                }
              })
            }
            return true
          })
      }
    }
  }
```

### onHttpAuthRequest<sup>9+</sup>

onHttpAuthRequest(callback: (event?: { handler: HttpAuthHandler, host: string, realm: string}) => boolean)

通知收到http auth认证请求。

Android加载页面不会直接触发该回调，iOS加载页面会直接触发该回调。

**参数：**

| 参数名  | 参数类型                             | 参数描述                     |
| ------- | ------------------------------------ | ---------------------------- |
| handler | [HttpAuthHandler](#httpauthhandler9) | 通知Web组件用户操作行为。    |
| host    | string                               | HTTP身份验证凭据应用的主机。 |
| realm   | string                               | HTTP身份验证凭据应用的域。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 返回false表示此次认证失败，否则成功，跨平台目前这个返回值没有作用。 |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview'

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController()
  httpAuth: boolean = false

  build() {
    Column() {
      Web({ src: 'https://www.example.com', controller: this.controller })
        .onHttpAuthRequest((event) => {
          if (event) {
            AlertDialog.show({
              title: 'onHttpAuthRequest',
              message: 'text',
              primaryButton: {
                value: 'cancel',
                action: () => {
                  event.handler.cancel()
                }
              },
              secondaryButton: {
                value: 'ok',
                action: () => {
                  event.handler.confirm("2222", "2222");
                }
              },
              cancel: () => {
                event.handler.cancel()
              }
            })
          }
          return true
        })
    }
  }
}
```

### onGeolocationShow

onGeolocationShow(callback: (event?: { origin: string, geolocation: JsGeolocation }) => void)

通知用户收到地理位置信息获取请求。

目前iOS不支持。

**参数：**

| 参数名      | 参数类型                        | 参数描述                  |
| ----------- | ------------------------------- | ------------------------- |
| origin      | string                          | 指定源的字符串索引。      |
| geolocation | [JsGeolocation](#jsgeolocation) | 通知Web组件用户操作行为。 |

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
        Web({ src:'https://www.example.com', controller:this.controller })
        .geolocationAccess(true)
        .onGeolocationShow((event) => {
          if (event) {
            AlertDialog.show({
              title: 'title',
              message: 'text',
              confirm: {
                value: 'onConfirm',
                action: () => {
                  event.geolocation.invoke(event.origin, true, true)
                }
              },
              cancel: () => {
                event.geolocation.invoke(event.origin, false, true)
              }
            })
          }
        })
      }
    }
  }
```

### onGeolocationHide

onGeolocationHide(callback: () => void)

通知用户先前被调用[onGeolocationShow](#ongeolocationshow)时收到地理位置信息获取请求已被取消。

目前iOS不支持。

**参数：**

| 参数名   | 参数类型   | 参数描述                                 |
| -------- | ---------- | ---------------------------------------- |
| callback | () => void | 地理位置信息获取请求已被取消的回调函数。 |

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
        Web({ src:'https://www.example.com', controller:this.controller })
        .geolocationAccess(true)
        .onGeolocationHide(() => {
          console.log("onGeolocationHide...")
        })
      }
    }
  }
```

### onPermissionRequest<sup>9+</sup>

onPermissionRequest(callback: (event?: { request: PermissionRequest }) => void)

通知收到获取权限请求。

iOS监听到webview权限申请的前提是要在plist设置app获取权限选项，并且在首次打开应用，系统弹出获取权限窗口时选择授予。

Android监听到webview权限申请的前提是要在Manifest中静态配置。

getOrigin返回值以各平台行为为准。

**参数：**

| 参数名  | 参数类型                                 | 参数描述                  |
| ------- | ---------------------------------------- | ------------------------- |
| request | [PermissionRequest](#permissionrequest9) | 通知Web组件用户操作行为。 |

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
          .onPermissionRequest((event) => {
            if (event) {
              AlertDialog.show({
                title: 'title',
                message: 'text',
                primaryButton: {
                  value: 'deny',
                  action: () => {
                    event.request.deny()
                  }
                },
                secondaryButton: {
                  value: 'onConfirm',
                  action: () => {
                    event.request.grant(event.request.getAccessibleResource())
                  }
                },
                cancel: () => {
                  event.request.deny()
                }
              })
            }
          })
      }
    }
  }
```

### onPageVisible<sup>9+</sup>

onPageVisible(callback: (event: {url: string}) => void)

设置新页面内容即将可见时触发的回调函数。

获取的url以各平台行为为准。

**参数：**

| 参数名 | 参数类型 | 参数描述                              |
| ------ | -------- | ------------------------------------- |
| url    | string   | 新页面内容即将可见时新页面的url地址。 |

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
        Web({ src:'https://www.example.com', controller: this.controller })
         .onPageVisible((event) => {
          console.log('onPageVisible url:' + event.url)
        })
      }
    }
  }
```

### onDownloadStart

onDownloadStart(callback: (event?: { url: string, userAgent: string, contentDisposition: string, mimetype: string, contentLength: number }) => void)

通知主应用开始下载一个文件

返回信息以各平台行为为准，跨平台目前只支持获取url, userAgent, mimetype, contentLength。

**参数：**

| 参数名        | 参数类型      | 参数描述                             |
| ------------- | ------------- | ------------------------------------ |
| url           | string        | 文件下载的URL。                      |
| userAgent     | string        | 用于下载的用户代理。                 |
| mimetype      | string        | 服务器返回内容媒体类型（MIME）信息。 |
| contentLength | contentLength | 服务器返回文件的长度。               |

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
          .onDownloadStart((event) => {
            if (event) {
              console.log('url:' + event.url)
              console.log('userAgent:' + event.userAgent)
              console.log('mimetype:' + event.mimetype)
              console.log('contentLength:' + event.contentLength)
            }
          })
      }
    }
  }
```

### onShowFileSelector<sup>9+</sup>

onShowFileSelector(callback: (event?: { result: FileSelectorResult, fileSelector: FileSelectorParam }) => boolean)

调用此函数以处理具有“文件”输入类型的HTML表单，以响应用户按下的“选择文件”按钮。

目前iOS不支持。

**参数：**

| 参数名       | 参数类型                                   | 参数描述                        |
| ------------ | ------------------------------------------ | ------------------------------- |
| result       | [FileSelectorResult](#fileselectorresult9) | 用于通知Web组件文件选择的结果。 |
| fileSelector | [FileSelectorParam](#fileselectorparam9)   | 文件选择器的相关信息。          |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 当返回值为true时，用户可以调用系统提供的弹窗能力。当回调返回false时，web组件暂不支持触发默认弹窗。 |

**示例：**

```ts
  // xxx.ets
  import web_webview from '@ohos.web.webview';
  import picker from '@ohos.file.picker';
  import { BusinessError } from '@ohos.base';

  @Entry
  @Component
  struct WebComponent {
    controller: web_webview.WebviewController = new web_webview.WebviewController()
    @State uri: string = "file:///data/user/0/com.example.helloworld";

    build() {
      Column() {
        Web({ src: $rawfile('index.html'), controller: this.controller })
          .onShowFileSelector((event) => {
            console.log('MyFileUploader onShowFileSelector invoked')
            event.result.handleFileList([this.uri]);
            return true
          })
      }
    }
  }
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

## JsResult

Web组件返回的弹窗确认或弹窗取消功能对象。示例代码参考[onAlert事件](#onalert)。

### handleCancel

handleCancel(): void

通知Web组件用户取消弹窗操作。

### handleConfirm

handleConfirm(): void

通知Web组件用户确认弹窗操作。

### handlePromptConfirm<sup>9+</sup>

handlePromptConfirm(result: string): void

通知Web组件用户确认弹窗操作及对话框内容。

**参数：**

| 参数名 | 参数类型 | 必填 | 默认值 | 参数描述               |
| ------ | -------- | ---- | ------ | ---------------------- |
| result | string   | 是   | -      | 用户输入的对话框内容。 |

## HttpAuthHandler<sup>9+</sup>

Web组件返回的http auth认证请求确认或取消和使用缓存密码认证功能对象。示例代码参考[onHttpAuthRequest事件](#onhttpauthrequest9)。

### cancel<sup>9+</sup>

cancel(): void

通知Web组件用户取消HTTP认证操作。

### confirm<sup>9+</sup>

confirm(userName: string, pwd: string): boolean

使用用户名和密码进行HTTP认证操作。

**参数：**

| 参数名   | 参数类型 | 必填 | 默认值 | 参数描述         |
| -------- | -------- | ---- | ------ | ---------------- |
| userName | string   | 是   | -      | HTTP认证用户名。 |
| pwd      | string   | 是   | -      | HTTP认证密码。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 认证成功返回true，失败返回false。跨平台Android和iOS底层不会有返回值，所以都返回true。 |

### isHttpAuthInfoSaved<sup>9+</sup>

isHttpAuthInfoSaved(): boolean

通知Web组件用户使用服务器缓存的帐号密码认证。

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 存在密码认证成功返回true，其他返回false。iOS底层不会有返回值，所以暂时在获取不到服务器缓存帐号密码的时候返回false，如果能获取到就进行认证并返回true。 |

## JsGeolocation

Web组件返回授权或拒绝权限功能的对象。示例代码参考[onGeolocationShow事件](#ongeolocationshow)。

### invoke

invoke(origin: string, allow: boolean, retain: boolean): void

设置网页地理位置权限状态。

**参数：**

| 参数名 | 参数类型 | 必填 | 默认值 | 参数描述                                 |
| ------ | -------- | ---- | ------ | ---------------------------------------- |
| origin | string   | 是   | -      | 指定源的字符串索引。                     |
| allow  | boolean  | 是   | -      | 设置的地理位置权限状态。                 |
| retain | boolean  | 是   | -      | 是否允许将地理位置权限状态保存到系统中。 |

## PermissionRequest<sup>9+</sup>

Web组件返回授权或拒绝权限功能的对象。示例代码参考[onPermissionRequest事件](#onpermissionrequest9)。

### deny<sup>9+</sup>

deny(): void

拒绝网页所请求的权限。

### getOrigin<sup>9+</sup>

getOrigin(): string

获取网页来源。

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 当前请求权限网页的来源。 |

### getAccessibleResource<sup>9+</sup>

getAccessibleResource(): Array\<string\>

获取网页所请求的权限资源列表，跨平台资源列表支持的类型有RESOURCE_VIDEO_CAPTURE和RESOURCE_AUDIO_CAPTURE。

**返回值：**

| 类型            | 说明                       |
| --------------- | -------------------------- |
| Array\<string\> | 网页所请求的权限资源列表。 |

### grant<sup>9+</sup>

grant(resources: Array\<string\>): void

对网页访问的给定权限进行授权，跨平台iOS不支持授予某一种类型的权限，只支持授予当前申请的权限，或拒绝当前申请的权限。

**参数：**

| 参数名    | 参数类型        | 必填 | 默认值 | 参数描述                                                |
| --------- | --------------- | ---- | ------ | ------------------------------------------------------- |
| resources | Array\<string\> | 是   | -      | 授予网页请求的权限的资源列表，跨平台iOS此参数没有作用。 |

## FileSelectorResult<sup>9+</sup>

通知Web组件的文件选择结果。示例代码参考[onShowFileSelector事件](#onshowfileselector9)。

### handleFileList<sup>9+</sup>

handleFileList(fileList: Array\<string\>): void

通知Web组件进行文件选择操作。

**参数：**

| 参数名   | 参数类型        | 必填 | 默认值 | 参数描述                 |
| -------- | --------------- | ---- | ------ | ------------------------ |
| fileList | Array\<string\> | 是   | -      | 需要进行操作的文件列表。 |

## FileSelectorParam<sup>9+</sup>

web组件获取文件对象。示例代码参考[onShowFileSelector事件](#onshowfileselector9)。

### getTitle<sup>9+</sup>

getTitle(): string

获取文件选择器标题。

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| string | 返回文件选择器标题。 |

### getMode<sup>9+</sup>

getMode(): FileSelectorMode

获取文件选择器的模式。

**返回值：**

| 类型                                          | 说明                   |
| --------------------------------------------- | ---------------------- |
| [FileSelectorMode](#fileselectormode枚举说明) | 返回文件选择器的模式。 |

### getAcceptType<sup>9+</sup>

getAcceptType(): Array\<string\>

获取文件过滤类型。

**返回值：**

| 类型            | 说明               |
| --------------- | ------------------ |
| Array\<string\> | 返回文件过滤类型。 |

### isCapture<sup>9+</sup>

isCapture(): boolean

获取是否调用多媒体能力。

**返回值：**

| 类型    | 说明                     |
| ------- | ------------------------ |
| boolean | 返回是否调用多媒体能力。 |

## FileSelectorMode枚举说明

| 名称                 | 描述                 |
| -------------------- | -------------------- |
| FileOpenMode         | 打开上传单个文件。   |
| FileOpenMultipleMode | 打开上传多个文件。   |
| FileOpenFolderMode   | 打开上传文件夹模式。 |
| FileSaveMode         | 文件保存模式。       |



## 事件

###  onBeforeUnload<sup>16+</sup>



onBeforeUnload(callback: Callback<OnBeforeUnloadEvent, boolean>)

刷新或关闭场景下，在即将离开当前页面时触发此回调。刷新或关闭当前页面应先通过点击等方式获取焦点，才会触发此回调。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| callback | Callback<[OnBeforeUnloadEvent](#onbeforeunloadevent16), boolean> | 是   | 刷新或关闭场景下，在即将离开当前页面时触发。 返回值boolean。当回调返回true时，应用可以调用自定义弹窗能力（包括确认和取消），并且需要根据用户的确认或取消操作调用JsResult通知Web组件最终是否离开当前页面。当回调返回false时，函数中绘制的自定义弹窗无效。 |

**示例：**

```
// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Web({ src: $rawfile("index.html"), controller: this.controller })
        .onBeforeUnload((event) => {
          if (event) {
            console.log("event.url:" + event.url);
            console.log("event.message:" + event.message);
            AlertDialog.show({
              title: 'onBeforeUnload',
              message: 'text',
              primaryButton: {
                value: 'cancel',
                action: () => {
                  event.result.handleCancel();
                }
              },
              secondaryButton: {
                value: 'ok',
                action: () => {
                  event.result.handleConfirm();
                }
              },
              cancel: () => {
                event.result.handleCancel();
              }
            })
          }
          return true;
        })
    }
  }
}
```

加载的html文件。

```
<!--index.html-->
<!DOCTYPE html>
<html>
<head>
  <meta name="viewport" content="width=device-width, initial-scale=1.0" charset="utf-8">
</head>
<body onbeforeunload="return myFunction()">
  <h1>WebView onBeforeUnload Demo</h1>
  <a href="https://www.example.com">Click here</a>
  <script>
    function myFunction() {
      return "onBeforeUnload Event";
    }
  </script>
</body>
</html>
```

#### OnBeforeUnloadEvent<sup>16+</sup>



定义刷新或关闭场景下，在即将离开当前页面时触发此回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                  | 必填 | 说明                        |
| ------- | --------------------- | ---- | --------------------------- |
| url     | string                | 是   | 当前显示弹窗所在网页的URL。 |
| message | string                | 是   | 弹窗中显示的信息。          |
| result  | [JsResult](#jsresult) | 是   | 通知Web组件用户操作行为。   |

###  onRefreshAccessedHistory<sup>16+</sup>

onRefreshAccessedHistory(callback: Callback<OnRefreshAccessedHistoryEvent>)

加载网页页面完成时触发该回调，用于应用更新其访问的历史链接。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                           |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------ |
| callback | Callback<[OnRefreshAccessedHistoryEvent](#onrefreshaccessedhistoryevent16)> | 是   | 在网页刷新访问历史记录时触发。 |

**示例：**

```
// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
        .onRefreshAccessedHistory((event) => {
          if (event) {
            console.log('url:' + event.url + ' isReload:' + event.isRefreshed);
          }
        })
    }
  }
}

```
#### OnRefreshAccessedHistoryEvent<sup>16+</sup>
定义网页刷新访问历史记录时触发。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称        | 类型    | 必填 | 说明                                                         |
| ----------- | ------- | ---- | ------------------------------------------------------------ |
| url         | string  | 是   | 访问的url。                                                  |
| isRefreshed | boolean | 是   | true表示该页面是被重新加载的（调用[refresh16+](#refresh16)接口），false表示该页面是新加载的。 |

####  refresh<sup>16+</sup>

refresh(): void

调用此接口通知Web组件刷新网页。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**
```
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('refresh')
        .onClick(() => {
          try {
            this.controller.refresh();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}

```
###  onFullScreenExit<sup>16+</sup>

onFullScreenExit(callback: () => void)

通知开发者Web组件退出全屏模式。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型       | 必填 | 说明                       |
| -------- | ---------- | ---- | -------------------------- |
| callback | () => void | 是   | 退出全屏模式时的回调函数。 |

**示例：**
```
// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  handler: FullScreenExitHandler | null = null;

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
        .onFullScreenExit(() => {
          console.log("onFullScreenExit...")
          if (this.handler) {
            this.handler.exitFullScreen();
          }
        })
        .onFullScreenEnter((event) => {
          this.handler = event.handler;
        })
    }
  }
}
```
###  onFullScreenEnter<sup>16+</sup>

onFullScreenEnter(callback: OnFullScreenEnterCallback)

通知开发者Web组件进入全屏模式。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                                                      | 必填 | 说明                          |
| -------- | --------------------------------------------------------- | ---- | ----------------------------- |
| callback | [OnFullScreenEnterCallback](#onfullscreenentercallback16) | 是   | Web组件进入全屏时的回调信息。 |

**示例：**
```
// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  handler: FullScreenExitHandler | null = null;

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
        .onFullScreenEnter((event) => {
          console.log("onFullScreenEnter videoWidth: " + event.videoWidth +
            ", videoHeight: " + event.videoHeight);
          // 应用可以通过 this.handler.exitFullScreen() 主动退出全屏。
          this.handler = event.handler;
        })
    }
  }
}
```
####  OnFullScreenEnterCallback<sup>16+</sup>

type OnFullScreenEnterCallback = (event: FullScreenEnterEvent) => void

Web组件进入全屏时触发的回调。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**


| 参数名 | 类型                                            | 必填 | 说明                            |
| ------ | ----------------------------------------------- | ---- | ------------------------------- |
| event  | [FullScreenEnterEvent](#fullscreenenterevent16) | 是   | Web组件进入全屏的回调事件详情。 |


####  FullScreenEnterEvent<sup>16+</sup>

Web组件进入全屏回调事件的详情。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称        | 类型                                              | 必填 | 说明                                                         |
| ----------- | ------------------------------------------------- | ---- | ------------------------------------------------------------ |
| handler     | [FullScreenExitHandler](#fullscreenexithandler16) | 是   | 用于退出全屏模式的函数句柄。                                 |
| videoWidth  | number                                            | 否   | 视频的宽度，单位：px。如果进入全屏的是 `<video>` 元素，表示其宽度；如果进入全屏的子元素中包含 `<video>` 元素，表示第一个子视频元素的宽度；其他情况下，为0。 |
| videoHeight | number                                            | 否   | 视频的高度，单位：px。如果进入全屏的是 `<video>` 元素，表示其高度；如果进入全屏的子元素中包含 `<video>` 元素，表示第一个子视频元素的高度；其他情况下，为0。 |

#### FullScreenExitHandler<sup>16+</sup>

通知开发者Web组件退出全屏。示例代码参考[onFullScreenEnter事件](#onfullscreenenter16)。

##### constructor<sup>16+</sup>

constructor()

**系统能力：** SystemCapability.Web.Webview.Core

##### exitFullScreen<sup>16+</sup>

exitFullScreen(): void

通知开发者Web组件退出全屏。

**系统能力：** SystemCapability.Web.Webview.Core
