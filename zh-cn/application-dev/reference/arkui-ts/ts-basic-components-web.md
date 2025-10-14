# Web

提供具有网页显示能力的Web组件，[@ohos.web.webview](../apis/js-apis-webview.md)提供web控制能力。

> **说明：**
>
> - 该组件从API Version 8开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
> - 示例效果请以真机运行为准，当前IDE预览器不支持。

## 需要权限
#### ArkUI:
访问在线网页时需添加网络权限：ohos.permission.INTERNET。

访问地理位置时需添加权限：ohos.permission.LOCATION、ohos.permission.APPROXIMATELY_LOCATION、ohos.permission.LOCATION_IN_BACKGROUND。

#### Android:
访问在线网页时需添加网络权限：android.permission.INTERNET

``` <uses-permission android:name="android.permission.INTERNET" />```

访问地理位置时需添加权限：android.permission.ACCESS_FINE_LOCATION、android.permission.ACCESS_COARSE_LOCATION

``` 
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
```

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
| incognitoMode<sup>16+</sup> | boolean                                                      | 否   | 表示当前创建的webview是否是隐私模式。true表示创建隐私模式的webview, false表示创建正常模式的webview。 默认值：false |

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
  
  隐私模式Webview加载在线网页。

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller, incognitoMode: true })
    }
  }
}
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

onPageBegin(callback: Callback\<OnPageBeginEvent\>)

网页开始加载时触发该回调，且只在主frame触发，iframe或者frameset的内容加载时不会触发此回调。

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型   | 必填   | 说明      |
| ---- | ------ | ---- | --------- |
| callback  | Callback\<[OnPageBeginEvent](#onpagebeginevent)\> | 是    | 网页加载开始时触发。 |

**注意：**Android平台和iOS平台的触发时机与OpenHarmony不完全相同，以各平台行为为准。

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

onPageEnd(callback: Callback\<OnPageEndEvent\>)

网页加载完成时触发该回调，且只在主frame触发。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名  | 类型   | 必填   | 说明      |
| ---- | ------ | ---- | --------- |
| callback  | Callback\<[OnPageEndEvent](#onpageendevent)\> | 是    | 网页加载结束时触发。 |

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

onErrorReceive(callback: Callback\<OnErrorReceiveEvent\>)

网页加载遇到错误时触发该回调。主资源与子资源出错都会回调该接口，可以通过request.isMainFrame来判断是否是主资源报错。出于性能考虑，建议此回调中尽量执行简单逻辑。在无网络的情况下，触发此回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                     | 必填   | 说明            |
| ------- | ---------------------------------------- | ---- | --------------- |
| callback | Callback\<[OnErrorReceiveEvent](#onerrorreceiveevent)\> | 是    | 网页收到 Web 资源加载错误时触发。      |

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

onHttpErrorReceive(callback: Callback\<OnHttpErrorReceiveEvent\>)

网页加载资源遇到的HTTP错误（响应码>=400）时触发该回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名      | 类型                                     | 必填   | 说明       |
| -------- | ---------------------------------------- | ---- | ---------- |
| callback  | Callback\<[OnHttpErrorReceiveEvent](#onhttperrorreceiveevent)\> | 是    | 网页收到加载资源加载HTTP错误时触发。 |

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
            }
          })
      }
    }
  }
  ```

### onProgressChange

onProgressChange(callback: Callback\<OnProgressChangeEvent\>)

网页加载进度变化时触发该回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名         | 类型   | 必填   | 说明                  |
| ----------- | ------ | ---- | --------------------- |
| callback | Callback\<[OnProgressChangeEvent](#onprogresschangeevent)\> | 是    | 页面加载进度时触发的功能。 |

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

onScroll(callback: Callback\<OnScrollEvent\>)

通知网页滚动条滚动位置。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型   | 必填   | 说明                  |
| ------ | ------ | ---- | --------------------- |
| callback | Callback\<[OnScrollEvent](#onscrollevent)\> | 是 | 当滚动条滑动到指定位置时触发。 |

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

onTitleReceive(callback: Callback\<OnTitleReceiveEvent\>)

网页document标题更改时触发该回调。

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型   | 必填   | 说明          |
| ----- | ------ | ---- | ------------- |
| callback | Callback\<[OnTitleReceiveEvent](#ontitlereceiveevent)\> | 是    | 定义主应用程序文档标题更改时触发。 |

**注意：**Android平台和iOS平台的返回值与OpenHarmony不完全相同，以各平台行为为准。

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

onConsole(callback: Callback\<OnConsoleEvent, boolean\>)

通知宿主应用JavaScript console消息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                              | 必填   | 说明      |
| ------- | --------------------------------- | ---- | --------- |
| callback | Callback\<[OnConsoleEvent](#onconsoleevent), boolean\> | 是    | 网页收到JavaScript控制台消息时触发。<br>返回值boolean。当返回true时，该条消息将不会再打印至控制台，反之仍会打印至控制台。 |

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

  加载的html文件。
  ```html
  <!-- index.html -->
  <!DOCTYPE html>
  <html>
  <body>
  <script>
      function myFunction() {
          console.log("onconsole printf");
      }
  </script>
  </body>
  </html>
  ```

### onScaleChange<sup>9+</sup>

onScaleChange(callback: Callback\<OnScaleChangeEvent\>)

当前页面显示比例的变化时触发该回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型   | 必填   | 说明                  |
| ------ | ------ | ---- | --------------------- |
| callback | Callback\<[OnScaleChangeEvent](#onscalechangeevent)\> | 是 | 当前页面显示比例的变化时触发。 |

**注意：**

Android平台和iOS平台的页面显示比例与OpenHarmony不完全相同，以各平台行为为准。

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

onLoadIntercept(callback: Callback\<OnLoadInterceptEvent, boolean\>)

当Web组件加载url之前触发该回调，用于判断是否阻止此次访问。默认允许加载。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型   | 必填   | 说明                  |
| ------ | ------ | ---- | --------------------- |
| callback | Callback\<[OnLoadInterceptEvent](#onloadinterceptevent), boolean\> | 是 | 截获资源加载时触发的回调。<br>返回值boolean。返回true表示阻止此次加载，否则允许此次加载。 |

**注意：**在Android平台，此接口在重定向时触发。

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

onAlert(callback: Callback\<OnAlertEvent, boolean\>)

网页触发alert()告警弹窗时触发回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                   | 必填   | 说明            |
| ------- | --------------------- | ---- | --------------- |
| callback     | Callback\<[OnAlertEvent](#onalertevent), boolean\>                | 是    | 网页触发alert()告警弹窗时触发<br>返回值boolean。当回调返回true时，应用可以调用自定义弹窗能力（包括确认和取消），并且需要根据用户的确认或取消操作调用JsResult通知Web组件最终是否离开当前页面。当回调返回false时，web组件暂不支持触发默认弹窗。 |

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

  加载的html文件。
  ```html
  <!--index.html-->
  <!DOCTYPE html>
  <html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" charset="utf-8">
  </head>
  <body>
    <h1>WebView onAlert Demo</h1>
    <button onclick="myFunction()">Click here</button>
    <script>
      function myFunction() {
        alert("Hello World");
      }
    </script>
  </body>
  </html>
  ```

### onConfirm

onConfirm(callback: Callback\<OnConfirmEvent, boolean\>)

网页调用confirm()告警时触发此回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                  | 必填   | 说明            |
| ------- | --------------------- | ---- | --------------- |
| callback     | Callback\<[OnConfirmEvent](#onconfirmevent), boolean\>                | 是    | 网页调用confirm()告警时触发<br>返回值boolean。当回调返回true时，应用可以调用自定义弹窗能力（包括确认和取消），并且需要根据用户的确认或取消操作调用JsResult通知Web组件最终是否离开当前页面。当回调返回false时，web组件暂不支持触发默认弹窗，跨平台目前这个返回值没有作用。 |

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

  加载的html文件。
  ```html
  <!--index.html-->
  <!DOCTYPE html>
  <html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" charset="utf-8">
  </head>

  <body>
    <h1>WebView onConfirm Demo</h1>
    <button onclick="myFunction()">Click here</button>
    <p id="demo"></p>
    <script>
      function myFunction() {
        let x;
        let r = confirm("click button!");
        if (r == true) {
          x = "ok";
        } else {
          x = "cancel";
        }
        document.getElementById("demo").innerHTML = x;
      }
    </script>
  </body>
  </html>
  ```

### onPrompt<sup>9+</sup>

onPrompt(callback: Callback\<OnPromptEvent, boolean\>)

网页调用prompt()告警时触发此回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                  | 必填   | 说明            |
| ------- | --------------------- | ---- | --------------- |
| callback     | Callback\<[OnPromptEvent](#onpromptevent), boolean\>                | 是    | 网页调用prompt()告警时触发。<br>返回值boolean。当回调返回true时，应用可以调用自定义弹窗能力（包括确认和取消），并且需要根据用户的确认或取消操作调用JsResult通知Web组件最终是否离开当前页面。当回调返回false时，web组件暂不支持触发默认弹窗，跨平台目前这个返回值没有作用。 |

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

  加载的html文件。
  ```html
  <!--index.html-->
  <!DOCTYPE html>
  <html>
  <head>
    <meta name="viewport" content="width=device-width, initial-scale=1.0" charset="utf-8">
  </head>

  <body>
    <h1>WebView onPrompt Demo</h1>
    <button onclick="myFunction()">Click here</button>
    <p id="demo"></p>
    <script>
      function myFunction() {
        let message = prompt("Message info", "Hello World");
        if (message != null && message != "") {
          document.getElementById("demo").innerHTML = message;
        }
      }
    </script>
  </body>
  </html>
  ```

### onHttpAuthRequest<sup>9+</sup>

onHttpAuthRequest(callback: Callback\<OnHttpAuthRequestEvent, boolean\>)

通知收到http auth认证请求。

Android加载页面不会直接触发该回调，iOS加载页面会直接触发该回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型   | 必填   | 说明                  |
| ------ | ------ | ---- | --------------------- |
| callback | Callback\<[OnHttpAuthRequestEvent](#onhttpauthrequestevent), boolean\> | 是 | 当浏览器需要用户的凭据时触发。<br>返回值boolean。返回false表示此次认证失败，否则成功。   |

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

onGeolocationShow(callback: Callback\<OnGeolocationShowEvent\>)

通知用户收到地理位置信息获取请求。

**系统能力：** SystemCapability.Web.Webview.Core

目前iOS不支持。

**支持平台：** Android

**参数：**

| 参数名    | 类型   | 必填   | 说明                  |
| ------ | ------ | ---- | --------------------- |
| callback      | Callback\<[OnGeolocationShowEvent](#ongeolocationshowevent)\>  | 是 | 请求显示地理位置权限时触发。     |

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

onPermissionRequest(callback: Callback\<OnPermissionRequestEvent\>)

通知收到获取权限请求，需配置"ohos.permission.CAMERA"、"ohos.permission.MICROPHONE"权限。

iOS监听到webview权限申请的前提是要在plist设置app获取权限选项，并且在首次打开应用，系统弹出获取权限窗口时选择授予。

Android监听到webview权限申请的前提是要在Manifest中静态配置。

getOrigin返回值以各平台行为为准。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型   | 必填   | 说明                  |
| ------ | ------ | ---- | --------------------- |
| callback | Callback\<[OnPermissionRequestEvent](#onpermissionrequestevent)\> | 是 | 通知收到获取权限请求触发。 |

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

onPageVisible(callback: Callback\<OnPageVisibleEvent\>)

设置旧页面不再呈现，新页面即将可见时触发的回调函数。
获取的url以各平台行为为准。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型   | 必填   | 说明                  |
| ------ | ------ | ---- | --------------------- |
| callback  | Callback\<[OnPageVisibleEvent](#onpagevisibleevent)\> | 是 | 旧页面不再呈现，新页面即将可见时触发的回调函数。 |

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

onDownloadStart(callback: Callback\<OnDownloadStartEvent\>)

通知主应用开始下载一个文件。

**系统能力：** SystemCapability.Web.Webview.Core

返回信息以各平台行为为准。

**支持平台：** Android、iOS

**参数：**

| 参数名                | 类型   | 必填   | 说明                                |
| ------------------ | ------ | ---- | ----------------------------------- |
| callback           | Callback\<[OnDownloadStartEvent](#ondownloadstartevent)\> | 是    | 开始下载时触发。  |

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

onShowFileSelector(callback: Callback\<OnShowFileSelectorEvent, boolean\>)

调用此函数以处理具有“文件”输入类型的HTML表单，以响应用户按下的“选择文件”按钮。

目前iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core


**支持平台：** Android

**参数：**

| 参数名          | 类型                                     | 必填   | 说明              |
| ------------ | ---------------------------------------- | ---- | ----------------- |
| callback       | Callback\<[OnShowFileSelectorEvent](#onshowfileselectorevent), boolean\> | 是    | 用于通知Web组件文件选择的结果。<br>返回值boolean。当返回值为true时，用户可以调用系统提供的弹窗能力。当回调返回false时，web组件暂不支持触发默认弹窗。 |

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

###  onlineImageAccess<sup>16+</sup>

onlineImageAccess(onlineImageAccess: boolean)

设置是否允许从网络加载图片资源（通过HTTP和HTTPS访问的资源），默认允许访问。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名            | 类型    | 必填 | 说明                                           |
| ----------------- | ------- | ---- | ---------------------------------------------- |
| onlineImageAccess | boolean | 是   | 设置是否允许从网络加载图片资源。默认值：true。 |

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
        .onlineImageAccess(true)
    }
  }
}
```
###  domStorageAccess<sup>16+</sup>

domStorageAccess(domStorageAccess: boolean)

设置是否开启文档对象模型存储接口（DOM Storage API）权限，默认未开启。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名           | 类型    | 必填 | 说明                                                         |
| ---------------- | ------- | ---- | ------------------------------------------------------------ |
| domStorageAccess | boolean | 是   | 设置是否开启文档对象模型存储接口（DOM Storage API）权限。默认值：false。 |

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
        .domStorageAccess(true)
    }
  }
}
```
### imageAccess<sup>16+</sup>

imageAccess(imageAccess: boolean)

设置是否允许自动加载图片资源，默认允许。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名      | 类型    | 必填 | 说明                                         |
| ----------- | ------- | ---- | -------------------------------------------- |
| imageAccess | boolean | 是   | 设置是否允许自动加载图片资源。默认值：true。 |

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
        .imageAccess(true)
    }
  }
}
```

###  mixedMode<sup>16+</sup>

mixedMode(mixedMode: MixedMode)

设置是否允许加载超文本传输协议（HTTP）和超文本传输安全协议（HTTPS）混合内容，默认不允许加载HTTP和HTTPS混合内容。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名    | 类型                              | 必填 | 说明                                       |
| --------- | --------------------------------- | ---- | ------------------------------------------ |
| mixedMode | [MixedMode](#mixedmode枚举说明16) | 是   | 要设置的混合内容。默认值：MixedMode.None。 |

**示例：**
```
// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State mode: MixedMode = MixedMode.All;
  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
        .mixedMode(this.mode)
    }
  }
}
```
####  MixedMode枚举说明<sup>16+</sup>

**系统能力：** SystemCapability.Web.Webview.Core

| 名称       | 值   | 说明                                                        |
| ---------- | ---- | ----------------------------------------------------------- |
| All        | 0    | 允许加载HTTP和HTTPS混合内容。所有不安全的内容都可以被加载。 |
| Compatible | 1    | 混合内容兼容性模式，部分不安全的内容可能被加载。            |
| None       | 2    | 不允许加载HTTP和HTTPS混合内容。                             |

###  geolocationAccess<sup>16+</sup>

geolocationAccess(geolocationAccess: boolean)

设置是否开启获取地理位置权限，默认开启。具体使用方式参考[管理位置权限](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/web/web-geolocation-permission.md)。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名            | 类型    | 必填 | 说明                                         |
| ----------------- | ------- | ---- | -------------------------------------------- |
| geolocationAccess | boolean | 是   | 设置是否开启获取地理位置权限。默认值：true。 |

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
        .geolocationAccess(true)
    }
  }
}
```
### cacheMode<sup>16+</sup>

cacheMode(cacheMode: CacheMode)

设置缓存模式。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名    | 类型                           | 必填 | 说明                                          |
| --------- |------------------------------| ---- | --------------------------------------------- |
| cacheMode | [CacheMode](#cachemode枚举说明16) | 是   | 要设置的缓存模式。默认值：CacheMode.Default。 |
```
**示例：**// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State mode: CacheMode = CacheMode.None;

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
        .cacheMode(this.mode)
    }
  }
}

```
####  CacheMode枚举说明<sup>16+</sup>

**系统能力：** SystemCapability.Web.Webview.Core

| 名称      | 值   | 说明                                                         |
| --------- | ---- | ------------------------------------------------------------ |
| Default9+ | 0    | 使用未过期的cache加载资源，如果cache中无该资源则从网络中获取。 |
| None      | 1    | 加载资源使用cache，如果cache中无该资源则从网络中获取。       |
| Online    | 2    | 加载资源不使用cache，全部从网络中获取。                      |
| Only      | 3    | 只从cache中加载资源。                                        |

###  blockNetwork<sup>16+</sup>

blockNetwork(block: boolean)

设置Web组件是否阻止从网络加载资源。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型    | 必填 | 说明                                               |
| ------ | ------- | ---- | -------------------------------------------------- |
| block  | boolean | 是   | 设置Web组件是否阻止从网络加载资源。默认值：false。 |

**示例：**
```
// xxx.ets
import { webview } from '@kit.ArkWeb';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State block: boolean = true;

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
        .blockNetwork(this.block)
    }
  }
}
```

### javaScriptProxy<sup>20+</sup>

javaScriptProxy(javaScriptProxy: JavaScriptProxy)

注入JavaScript对象到window对象中，并在window对象中调用该对象的方法。

> **说明：**
>
> javaScriptProxy接口需要和deleteJavaScriptRegister接口配合使用，防止内存泄漏。 javaScriptProxy对象的所有参数不支持更新。 注册对象时，同步与异步方法列表请至少选择一项不为空，可同时注册两类方法。 此接口只支持注册一个对象，若需要注册多个对象请使用[registerJavaScriptProxy20+](../apis/js-apis-webview.md#registerjavascriptproxy20)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名          | 类型            | 必填 | 说明                                         |
| --------------- | --------------- | ---- | -------------------------------------------- |
| javaScriptProxy | JavaScriptProxy | 是   | 参与注册的对象。只能声明方法，不能声明属性。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';

class TestObj {
  constructor() {
  }

  test(data1: string, data2: string, data3: string): string {
    console.log("data1:" + data1);
    console.log("data2:" + data2);
    console.log("data3:" + data3);
    return "AceString";
  }

  asyncTest(data: string): void {
    console.log("async data:" + data);
  }

  toString(): void {
    console.log('toString' + "interface instead.");
  }
}

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  testObj = new TestObj();
  build() {
    Column() {
      Button('deleteJavaScriptRegister')
        .onClick(() => {
          try {
            this.controller.deleteJavaScriptRegister("objName");
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
        .javaScriptAccess(true)
        .javaScriptProxy({
          object: this.testObj,
          name: "objName",
          methodList: ["test", "toString"],
          asyncMethodList: ["asyncTest"],
          controller: this.controller,
      })
    }
  }
}
```

### fileAccess<sup>22+</sup>

fileAccess(fileAccess: boolean)

设置是否开启应用中文件系统的访问。[$rawfile(filepath/filename)](../../quick-start/resource-categories-and-access.md)中的文件不受该属性影响而被限制访问。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**参数：**

| 参数名     | 类型    | 必填 | 说明                                                         |
| ---------- | ------- | ---- | ------------------------------------------------------------ |
| fileAccess | boolean | 是   | 设置是否开启应用中文件系统的访问。<br>true表示开启应用中文件系统的访问。false表示不开启应用中文件系统的访问。 |

**示例：**

  ```ts
  // xxx.ets
  import { webview } from '@kit.ArkWeb';

  @Entry
  @Component
  struct WebComponent {
    controller: webview.WebviewController = new webview.WebviewController();

    build() {
      Column() {
        Web({ src: 'www.example.com', controller: this.controller })
          .fileAccess(true)
      }
    }
  }
  ```

### textZoomRatio<sup>22+</sup>

textZoomRatio(textZoomRatio: number)

设置页面的文本缩放百分比。当属性没有显式调用时，默认缩放百分比为100%。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名        | 类型   | 必填 | 说明                                                         |
| ------------- | ------ | ---- | ------------------------------------------------------------ |
| textZoomRatio | number | 是   | 要设置的页面的文本缩放百分比。<br>取值为整数，范围为(0, 2147483647]。 |

**示例：**

  ```ts
  // xxx.ets
  import { webview } from '@kit.ArkWeb';

  @Entry
  @Component
  struct WebComponent {
    controller: webview.WebviewController = new webview.WebviewController();
    @State ratio: number = 150;

    build() {
      Column() {
        Web({ src: 'www.example.com', controller: this.controller })
          .textZoomRatio(this.ratio)
      }
    }
  }
  ```

### nestedScroll<sup>22+</sup>

nestedScroll(value: NestedScrollOptions | NestedScrollOptionsExt)

调用以设置嵌套滚动选项。

> **说明：**
>
> - 可以设置上下左右四个方向，或者设置向前、向后两个方向的嵌套滚动模式，实现与父组件的滚动联动。
> - 支持嵌套滚动的容器：Grid、List、Scroll、Swiper、Tabs、WaterFlow、Refresh、bindSheet。
> - 支持嵌套滚动的输入事件：使用手势。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                                          | 必填 | 说明                                                         |
| ------ | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| value  | NestedScrollOptions \| NestedScrollOptionsExt | 是   | 可滚动组件滚动时的嵌套滚动选项。<br> value为NestedScrollOptions（向前、向后两个方向）类型时，scrollForward、scrollBackward默认滚动选项为NestedScrollMode.SELF_FIRST。 <br> value为NestedScrollOptionsExt（上下左右四个方向）类型时，scrollUp、scrollDown、scrollLeft、scrollRight默认滚动选项为NestedScrollMode.SELF_FIRST。 |

**示例：**

  ```ts
  // xxx.ets
  import { webview } from '@kit.ArkWeb';
  @Entry
  @Component
  struct WebComponent {
    controller: webview.WebviewController = new webview.WebviewController();

    build() {
      Column() {
        Web({ src: 'www.example.com', controller: this.controller })
          .nestedScroll({
            scrollForward: NestedScrollMode.SELF_FIRST,
            scrollBackward: NestedScrollMode.SELF_FIRST,
          })
      }
    }
  }
  ```

  ```ts
  // xxx.ets
  import { webview } from '@kit.ArkWeb';
  @Entry
  @Component
  struct WebComponent {
    controller: webview.WebviewController = new webview.WebviewController()
    build() {
      Scroll(){
        Column() {
          Text("嵌套Web")
            .height("25%")
            .width("100%")
            .fontSize(30)
            .backgroundColor(Color.Yellow)
          Web({ src: $rawfile('index.html'),
                controller: this.controller })
            .nestedScroll({
              scrollUp: NestedScrollMode.SELF_FIRST,
              scrollDown: NestedScrollMode.PARENT_FIRST,
              scrollLeft: NestedScrollMode.SELF_FIRST,
              scrollRight: NestedScrollMode.SELF_FIRST,
            })
        }
      }
    }
  }
  ```

  加载的html文件。

  ```html
  <!-- index.html -->
  <!DOCTYPE html>
  <html>
  <head>
      <meta name="viewport" id="viewport" content="width=device-width, initial-scale=1.0">
      <style>
          .blue {
            background-color: lightblue;
          }
          .green {
            background-color: lightgreen;
          }
          .blue, .green {
          font-size:16px;
          height:200px;
          text-align: center;       /* 水平居中 */
          line-height: 200px;       /* 垂直居中（值等于容器高度） */
          }
      </style>
  </head>
  <body>
  <div class="blue" >webArea</div>
  <div class="green">webArea</div>
  <div class="blue">webArea</div>
  <div class="green">webArea</div>
  <div class="blue">webArea</div>
  <div class="green">webArea</div>
  <div class="blue">webArea</div>
  </body>
  </html>
  ```

## JavaScriptProxy<sup>20+</sup>

定义要注入的JavaScript对象。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

| 名称                          | 类型                                                         | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ----------------------------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| object<sup>20+</sup>          | object                                                       | 是   | 参与注册的对象。只能声明方法，不能声明属性。                 | 支持        | 支持    |
| name<sup>20+</sup>            | string                                                       | 是   | 注册对象的名称，与window中调用的对象名一致。                 | 支持        | 支持    |
| methodList<sup>20+</sup>      | Array\<string>                                               | 是   | 参与注册的应用侧JavaScript对象的同步方法。                   | 支持        | 支持    |
| controller<sup>20+</sup>      | [WebviewController](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-webview.md?init=initTree#webviewcontroller) | 是   | 控制器                                                            | 支持        | 支持    |
| asyncMethodList<sup>20+</sup> | Array\<string>                                               | 否   | 参与注册的应用侧JavaScript对象的异步方法。异步方法无法获取返回值。 | 支持        | 支持    |

## WebResourceError

Web组件资源管理错误信息对象。示例代码参考[onErrorReceive事件](#onerrorreceive)。

### constructor

constructor()

WebResourceError的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### getErrorCode

getErrorCode(): number

获取加载资源的错误码。

**错误码：**

Android和iOS的错误码与OpenHarmony不完全相同，以各平台错误码为准。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明          |
| ------ | ----------- |
| number | 返回加载资源的错误码。 |

### getErrorInfo

getErrorInfo(): string

获取加载资源的错误信息。  
Android和iOS的错误信息与OpenHarmony不完全相同，以各平台错误信息为准。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明           |
| ------ | ------------ |
| string | 返回加载资源的错误信息。 |

## WebResourceRequest

Web组件获取资源请求对象。示例代码参考[onErrorReceive事件](#onerrorreceive)。

### constructor

constructor()

WebResourceRequest的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### getRequestHeader

getRequestHeader(): Array\<Header\>

获取资源请求头信息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型                         | 说明         |
| -------------------------- | ---------- |
| Array\<[Header](#header)\> | 返回资源请求头信息。 |

### getRequestUrl

getRequestUrl(): string

获取资源请求的URL信息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| string | 返回资源请求的URL信息。 |

### isMainFrame

isMainFrame(): boolean

判断资源请求是否为主frame。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型      | 说明               |
| ------- | ---------------- |
| boolean | 返回资源请求是否为主frame。 |

### isRedirect

isRedirect(): boolean

判断资源请求是否被服务端重定向。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型      | 说明               |
| ------- | ---------------- |
| boolean | 返回资源请求是否被服务端重定向。 |

### isRequestGesture

isRequestGesture(): boolean

获取资源请求是否与手势（如点击）相关联。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型      | 说明                   |
| ------- | -------------------- |
| boolean | 返回资源请求是否与手势（如点击）相关联。 |

### getRequestMethod<sup>16+</sup>

getRequestMethod(): string

获取请求方法。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型     | 说明      |
| ------ | ------- |
| string | 返回请求方法。 |

## Header

Web组件返回的请求/响应头对象。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称          | 类型     | 必填   | 说明            |
| ----------- | ------ | ---- | ------------- |
| headerKey   | string | 是    | 请求/响应头的key。   |
| headerValue | string | 是    | 请求/响应头的value。 |

## WebResourceResponse

Web组件资源响应对象。示例代码参考[onHttpErrorReceive事件](#onhttperrorreceive)。

### constructor

constructor()

WebResourceResponse的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### getReasonMessage

getReasonMessage(): string

获取资源响应的状态码描述。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| string | 返回资源响应的状态码描述。 |

### getResponseCode

getResponseCode(): number

获取资源响应的状态码。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明          |
| ------ | ----------- |
| number | 返回资源响应的状态码。 |

### getResponseData

getResponseData(): string

获取资源响应数据。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型     | 说明        |
| ------ | --------- |
| string | 返回资源响应数据。 |

### getResponseEncoding

getResponseEncoding(): string

获取资源响应的编码。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| string | 返回资源响应的编码。 |

### getResponseHeader

getResponseHeader() : Array\<Header\>

获取资源响应头。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型                         | 说明       |
| -------------------------- | -------- |
| Array\<[Header](#header)\> | 返回资源响应头。 |

### getResponseMimeType

getResponseMimeType(): string

获取资源响应的媒体（MIME）类型。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明                 |
| ------ | ------------------ |
| string | 返回资源响应的媒体（MIME）类型。 |

### getResponseDataEx<sup>16+</sup>

getResponseDataEx(): string | number | ArrayBuffer | Resource | undefined

获取资源响应数据，支持多种数据类型。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

|类型|说明|
|---|---|
|string|返回HTML格式的字符串。|
|number|返回文件句柄。Android不支持。|
|ArrayBuffer|返回二进制数据。Android不支持。|
|[Resource](../arkui-ts/ts-types.md)|返回`$rawfile`资源。Android不支持。|
|undefined|如果没有可用数据，返回`undefined`。|

### getResponseIsReady<sup>16+</sup>

getResponseIsReady(): boolean

获取响应数据是否已准备就绪。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

|类型|说明|
|---|---|
|boolean|`true`表示响应数据已准备好，`false`表示未准备好。|

### setResponseData<sup>22+</sup>

setResponseData(data: string \| number \| Resource \| ArrayBuffer): void

设置资源响应数据。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                                       | 必填 | 说明                                                         | Android平台                | iOS平台                    |
| ------ | ------------------------------------------ | ---- | ------------------------------------------------------------ | -------------------------- | -------------------------- |
| data   | string \| number \|Resource \| ArrayBuffer | 是   | 要设置的资源响应数据。string表示HTML格式的字符串。number表示文件句柄，此句柄由系统的Web组件负责关闭。Resource表示应用rawfile目录下文件资源。ArrayBuffer表示资源的原始二进制数据。 | number和Resource类型不支持 | number和Resource类型不支持 |

### setResponseEncoding<sup>22+</sup>

setResponseEncoding(encoding: string): void

设置资源响应的编码。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型   | 必填 | 说明                     |
| -------- | ------ | ---- | ------------------------ |
| encoding | string | 是   | 要设置的资源响应的编码。 |

### setResponseMimeType<sup>22+</sup>

setResponseMimeType(mimeType: string): void

设置资源响应的媒体（MIME）类型。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型   | 必填 | 说明                                 |
| -------- | ------ | ---- | ------------------------------------ |
| mimeType | string | 是   | 要设置的资源响应的媒体（MIME）类型。 |

### setReasonMessage<sup>22+</sup>

setReasonMessage(reason: string): void

设置资源响应的状态码描述。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                           |
| ------ | ------ | ---- | ------------------------------ |
| reason | string | 是   | 要设置的资源响应的状态码描述。 |

### setResponseHeader<sup>22+</sup>

setResponseHeader(header: Array\<Header\>): void

设置资源响应头。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                       | 必填 | 说明                 |
| ------ | -------------------------- | ---- | -------------------- |
| header | Array\<[Header](#header)\> | 是   | 要设置的资源响应头。 |

### setResponseCode<sup>22+</sup>

setResponseCode(code: number): void

设置资源响应的状态码。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| code   | number | 是   | 要设置的资源响应的状态码。如果该资源以错误结束，请参考[@ohos.web.netErrorList](../apis/arkts-apis-netErrorList.md)设置相应错误码，避免设置错误码为 ERR_IO_PENDING，设置为该错误码可能会导致XMLHttpRequest同步请求阻塞。Android平台该值的取值范围为[100, 299], [400, 599]。 |

### setResponseIsReady<sup>22+</sup>

setResponseIsReady(IsReady: boolean): void

设置资源响应数据是否已经就绪。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型    | 必填 | 说明                                                         |
| ------- | ------- | ---- | ------------------------------------------------------------ |
| IsReady | boolean | 是   | 资源响应数据是否已经就绪。<br>true表示资源响应数据已经就绪，false表示资源响应数据未就绪。<br>如果数据是异步提供，需要显式设置为false。设置为非法值如null，undefined或者不设置都会被认为数据已经准备好。 |

## ConsoleMessage

Web组件获取控制台信息对象。示例代码参考[onConsole事件](#onconsole)。

### constructor

constructor()

ConsoleMessage的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### getLineNumber

getLineNumber(): number

获取ConsoleMessage的行数。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型     | 说明                   |
| ------ | -------------------- |
| number | 返回ConsoleMessage的行数。 |

### getMessage

getMessage(): string

获取ConsoleMessage的日志信息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                           |
| ------ | ------------------------------ |
| string | 返回ConsoleMessage的日志信息。 |

### getMessageLevel

getMessageLevel(): MessageLevel

获取ConsoleMessage的信息级别。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                                  | 说明                           |
| ------------------------------------- | ------------------------------ |
| [MessageLevel](#messagelevel枚举说明) | 返回ConsoleMessage的信息级别。 |

### getSourceId

getSourceId(): string

获取网页源文件路径和名字。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型     | 说明            |
| ------ | ------------- |
| string | 返回网页源文件路径和名字。 |

## MessageLevel枚举说明

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 值 | 说明    | Android平台 | iOS平台 |
| ----- | -- | ---- | ---- | ---- |
| Debug | 1 | 调试级别。 | 支持 | 支持 |
| Error | 4 | 错误级别。 | 支持 | 支持 |
| Info  | 2 | 消息级别。 | 支持 | 支持 |
| Log   | 5 | 日志级别。 | 支持 | 支持 |
| Warn  | 3 | 警告级别。 | 支持 | 支持 |

## JsResult

Web组件返回的弹窗确认或弹窗取消功能对象。示例代码参考[onAlert事件](#onalert)。

### constructor

constructor()

JsResult的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### handleCancel

handleCancel(): void

通知Web组件用户取消弹窗操作。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

### handleConfirm

handleConfirm(): void

通知Web组件用户确认弹窗操作。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

### handlePromptConfirm<sup>9+</sup>

handlePromptConfirm(result: string): void

通知Web组件用户确认弹窗操作及对话框内容。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS。

**参数：**

| 参数名    | 类型   | 必填   | 说明        |
| ------ | ------ | ---- | ----------- |
| result | string | 是    | 用户输入的对话框内容。 |

**注意：**Android平台和iOS平台，在OnAlertEvent和OnConfirmEvent事件下不支持此接口。



## HttpAuthHandler<sup>9+</sup>

Web组件返回的http auth认证请求确认或取消和使用缓存密码认证功能对象。示例代码参考[onHttpAuthRequest事件](#onhttpauthrequest9)。

### constructor

constructor()

HttpAuthHandler的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### cancel<sup>9+</sup>

cancel(): void

通知Web组件用户取消HTTP认证操作。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

### confirm<sup>9+</sup>

confirm(userName: string, password: string): boolean

使用用户名和密码进行HTTP认证操作。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名      | 类型   | 必填  | 说明       |
| -------- | ------ | ---- | ---------- |
| userName | string | 是   | HTTP认证用户名。 |
| password      | string | 是   | HTTP认证密码。  |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 认证成功返回true，失败返回false。**Android平台和iOS平台底层不会有返回值，所以都返回true**。 |

### isHttpAuthInfoSaved<sup>9+</sup>

isHttpAuthInfoSaved(): boolean

通知Web组件用户使用服务器缓存的账号密码认证。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 存在密码认证成功返回true，其他返回false。**iOS平台底层不会有返回值，所以暂时在获取不到服务器缓存帐号密码的时候返回false，如果能获取到就进行认证并返回true**。 |

## JsGeolocation

Web组件返回授权或拒绝权限功能的对象。示例代码参考[onGeolocationShow事件](#ongeolocationshow)。

### constructor

constructor()

JsGeolocation的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### invoke

invoke(origin: string, allow: boolean, retain: boolean): void

设置网页地理位置权限状态。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**参数：**

| 参数名    | 类型    | 必填  | 说明                                     |
| ------ | ------- | ---- | ---------------------------------------- |
| origin | string  | 是   | 指定源的字符串索引。                               |
| allow  | boolean | 是   | 设置的地理位置权限状态。                             |
| retain | boolean | 是   | 是否允许将地理位置权限状态保存到系统中。 |

## PermissionRequest<sup>9+</sup>

Web组件返回授权或拒绝权限功能的对象。示例代码参考[onPermissionRequest事件](#onpermissionrequest9)。

### constructor

constructor()

PermissionRequest的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### deny<sup>9+</sup>

deny(): void

拒绝网页所请求的权限。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

### getOrigin<sup>9+</sup>

getOrigin(): string

获取网页来源。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型     | 说明           |
| ------ | ------------ |
| string | 当前请求权限网页的来源。 |

### getAccessibleResource<sup>9+</sup>

getAccessibleResource(): Array\<string\>

获取网页所请求的权限资源列表，跨平台资源列表支持的类型有RESOURCE_VIDEO_CAPTURE和RESOURCE_AUDIO_CAPTURE。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型              | 说明            |
| --------------- | ------------- |
| Array\<string\> | 网页所请求的权限资源列表。 |

### grant<sup>9+</sup>

grant(resources: Array\<string\>): void

对网页访问的给定权限进行授权，跨平台iOS不支持授予某一种类型的权限，只支持授予当前申请的权限，或拒绝当前申请的权限。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名    | 参数类型        | 必填 | 默认值 | 参数描述                                                  |
| --------- | --------------- | ---- | ------ | --------------------------------------------------------- |
| resources | Array\<string\> | 是   | -      | 授予网页请求的权限的资源列表，**iOS平台此参数没有作用**。 |

## FileSelectorResult<sup>9+</sup>

通知Web组件的文件选择结果。示例代码参考[onShowFileSelector事件](#onshowfileselector9)。

### constructor

constructor()

FileSelectorResult的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### handleFileList<sup>9+</sup>

handleFileList(fileList: Array\<string\>): void

通知Web组件进行文件选择操作。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**参数：**

| 参数名      | 类型            | 必填  | 说明         |
| -------- | --------------- | ---- | ------------ |
| fileList | Array\<string\> | 是   | 需要进行操作的文件列表。 |

## FileSelectorParam<sup>9+</sup>

Web组件获取文件对象。示例代码参考[onShowFileSelector事件](#onshowfileselector9)。

### constructor

constructor()

FileSelectorParam的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

### getTitle<sup>9+</sup>

getTitle(): string

获取文件选择器标题。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型     | 说明         |
| ------ | ---------- |
| string | 返回文件选择器标题。 |

### getMode<sup>9+</sup>

getMode(): FileSelectorMode

获取文件选择器的模式。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型                                       | 说明          |
| ---------------------------------------- | ----------- |
| [FileSelectorMode](#fileselectormode枚举说明) | 返回文件选择器的模式。 |

### getAcceptType<sup>9+</sup>

getAcceptType(): Array\<string\>

获取文件过滤类型。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型              | 说明        |
| --------------- | --------- |
| Array\<string\> | 返回文件过滤类型。 |

### isCapture<sup>9+</sup>

isCapture(): boolean

获取是否调用多媒体能力。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android

**返回值：**

| 类型      | 说明           |
| ------- | ------------ |
| boolean | 返回是否调用多媒体能力。 |

## SslErrorHandler<sup>22+</sup>

Web组件返回的SSL错误通知事件用户处理功能对象。

### constructor<sup>22+</sup>

constructor()

SslErrorHandler的构造函数。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

### handleCancel<sup>22+</sup>

handleCancel(): void

通知Web组件取消此请求。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

### handleConfirm<sup>22+</sup>

handleConfirm(): void

通知Web组件继续使用SSL证书。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

## SslError<sup>22+</sup>

onSslErrorEventReceive接口返回的SSL错误的具体原因。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称         | 值   | 说明                   | Android平台 | iOS平台 |
| ------------ | ---- | ---------------------- | ----------- | ------- |
| HostMismatch | 1    | 主机名不匹配。         | 支持        | 支持    |
| DateInvalid  | 2    | 证书日期无效。         | 支持        | 支持    |
| Untrusted    | 3    | 证书颁发机构不受信任。 | 支持        | 支持    |

## SslErrorEvent<sup>22+</sup>

用户加载资源时发生SSL错误时触发的回调详情。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称          | 类型                                  | 只读 | 可选 | 说明                                                  | Android平台 | iOS平台 |
| ------------- | ------------------------------------- | ---- | ---- | ----------------------------------------------------- | ----------- | ------- |
| handler       | [SslErrorHandler](#sslerrorhandler22) | 否   | 否   | 通知Web组件用户操作行为。                             | 支持        | 支持    |
| error         | [SslError](#sslerror22)               | 否   | 否   | 错误码。                                              | 支持        | 支持    |
| url           | string                                | 否   | 否   | url地址。                                             | 支持        | 支持    |
| originalUrl   | string                                | 否   | 否   | 请求的原始url地址。                                   | 支持        | 支持    |
| referrer      | string                                | 否   | 否   | referrer url地址。                                    | 支持        | 支持    |
| isMainFrame   | boolean                               | 否   | 否   | 是否是主资源。<br>true表示主资源，false表示非主资源。 | 支持        | 支持    |
| certChainData | Array<Uint8Array\>                    | 否   | 是   | 证书链数据。                                          | 支持        | 支持    |

## FileSelectorMode枚举说明

| 名称                 | 描述                 | Android平台 | iOS平台 |
| -------------------- | -------------------- | ----------- | ------- |
| FileOpenMode         | 打开上传单个文件。   | 支持        | 不支持    |
| FileOpenMultipleMode | 打开上传多个文件。   | 支持        | 不支持    |
| FileOpenFolderMode   | 打开上传文件夹模式。 | 支持        | 不支持    |
| FileSaveMode         | 文件保存模式。       | 支持        | 不支持    |

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

onRefreshAccessedHistory(callback: Callback\<OnRefreshAccessedHistoryEvent>)

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

## OnPageEndEvent

定义网页加载结束时触发的函数。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称 | 类型   | 必填 | 说明            | Android平台 | iOS平台 |
| ---- | ------ | ---- | --------------- | ----------- | ------- |
| url  | string | 是   | 页面的URL地址。 | 支持        | 支持    |

## OnPageBeginEvent

定义网页加载开始时触发的函数。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称 | 类型   | 必填 | 说明            | Android平台 | iOS平台 |
| ---- | ------ | ---- | --------------- | ----------- | ------- |
| url  | string | 是   | 页面的URL地址。 | 支持        | 支持    |

## OnProgressChangeEvent

定义网页加载进度变化时触发该回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称        | 类型   | 必填 | 说明                                   | Android平台 | iOS平台 |
| ----------- | ------ | ---- | -------------------------------------- | ----------- | ------- |
| newProgress | number | 是   | 新的加载进度，取值范围为0到100的整数。 | 支持        | 支持    |

## OnTitleReceiveEvent

定义网页document标题更改时触发该回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称  | 类型   | 必填 | 说明               | Android平台 | iOS平台 |
| ----- | ------ | ---- | ------------------ | ----------- | ------- |
| title | string | 是   | document标题内容。 | 支持        | 支持    |

## OnGeolocationShowEvent

定义通知用户收到地理位置信息获取请求。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称        | 类型                            | 必填 | 说明                      | Android平台 | iOS平台 |
| ----------- | ------------------------------- | ---- | ------------------------- | ----------- | ------- |
| origin      | string                          | 是   | 指定源的字符串索引。      | 支持        | 不支持  |
| geolocation | [JsGeolocation](#jsgeolocation) | 是   | 通知Web组件用户操作行为。 | 支持        | 不支持  |

## OnAlertEvent

定义网页触发alert()告警弹窗时触发回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                  | 必填 | 说明                        | Android平台 | iOS平台 |
| ------- | --------------------- | ---- | --------------------------- | ----------- | ------- |
| url     | string                | 是   | 当前显示弹窗所在网页的URL。 | 支持        | 支持    |
| message | string                | 是   | 弹窗中显示的信息。          | 支持        | 支持    |
| result  | [JsResult](#jsresult) | 是   | 通知Web组件用户操作行为。   | 支持        | 支持    |

## OnConfirmEvent

定义网页调用confirm()告警时触发此回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                  | 必填 | 说明                        | Android平台 | iOS平台 |
| ------- | --------------------- | ---- | --------------------------- | ----------- | ------- |
| url     | string                | 是   | 当前显示弹窗所在网页的URL。 | 支持        | 支持    |
| message | string                | 是   | 弹窗中显示的信息。          | 支持        | 支持    |
| result  | [JsResult](#jsresult) | 是   | 通知Web组件用户操作行为。   | 支持        | 支持    |

## OnPromptEvent

定义网页调用prompt()告警时触发此回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                  | 必填 | 说明                        | Android平台 | iOS平台 |
| ------- | --------------------- | ---- | --------------------------- | ----------- | ------- |
| url     | string                | 是   | 当前显示弹窗所在网页的URL。 | 支持        | 支持    |
| message | string                | 是   | 弹窗中显示的信息。          | 支持        | 支持    |
| value   | string                | 是   | 提示对话框的信息。          | 支持        | 支持    |
| result  | [JsResult](#jsresult) | 是   | 通知Web组件用户操作行为。   | 支持        | 支持    |

## OnConsoleEvent

定义通知宿主应用JavaScript console消息。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                              | 必填 | 说明               | Android平台 | iOS平台 |
| ------- | --------------------------------- | ---- | ------------------ | ----------- | ------- |
| message | [ConsoleMessage](#consolemessage) | 是   | 触发的控制台信息。 | 支持        | 支持    |

## OnErrorReceiveEvent

定义网页加载遇到错误时触发该回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                                      | 必填 | 说明                          | Android平台 | iOS平台 |
| ------- | ----------------------------------------- | ---- | ----------------------------- | ----------- | ------- |
| request | [WebResourceRequest](#webresourcerequest) | 是   | 网页请求的封装信息。          | 支持        | 支持    |
| error   | [WebResourceError](#webresourceerror)     | 是   | 网页加载资源错误的封装信息 。 | 支持        | 支持    |

## OnHttpErrorReceiveEvent

定义网页收到加载资源加载HTTP错误时触发。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称     | 类型                                        | 必填 | 说明                 | Android平台 | iOS平台 |
| -------- | ------------------------------------------- | ---- | -------------------- | ----------- | ------- |
| request  | [WebResourceRequest](#webresourcerequest)   | 是   | 网页请求的封装信息。 | 支持        | 支持    |
| response | [WebResourceResponse](#webresourceresponse) | 是   | 资源响应的封装信息。 | 支持        | 支持    |

## OnDownloadStartEvent

定义通知主应用开始下载一个文件。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称               | 类型   | 必填 | 说明                                               | Android平台 | iOS平台 |
| ------------------ | ------ | ---- | -------------------------------------------------- | ----------- | ------- |
| url                | string | 是   | 文件下载的URL。                                    | 支持        | 支持    |
| userAgent          | string | 是   | 用于下载的用户代理。                               | 支持        | 支持    |
| contentDisposition | string | 是   | 服务器返回的 Content-Disposition响应头，可能为空。 | 支持        | 不支持  |
| mimetype           | string | 是   | 服务器返回内容媒体类型（MIME）信息。               | 支持        | 支持    |
| contentLength      | number | 是   | 服务器返回文件的长度。                             | 支持        | 支持    |

## OnShowFileSelectorEvent

定义文件选择器结果。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称         | 类型                                       | 必填 | 说明                            | Android平台 | iOS平台 |
| ------------ | ------------------------------------------ | ---- | ------------------------------- | ----------- | ------- |
| result       | [FileSelectorResult](#fileselectorresult9) | 是   | 用于通知Web组件文件选择的结果。 | 支持        | 支持    |
| fileSelector | [FileSelectorParam](#fileselectorparam9)   | 是   | 文件选择器的相关信息。          | 支持        | 支持    |

## OnScaleChangeEvent

定义当前页面显示比例的变化时触发。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称     | 类型   | 必填 | 说明                     | Android平台 | iOS平台 |
| -------- | ------ | ---- | ------------------------ | ----------- | ------- |
| oldScale | number | 是   | 变化前的显示比例百分比。 | 支持        | 支持    |
| newScale | number | 是   | 变化后的显示比例百分比。 | 支持        | 支持    |

## OnHttpAuthRequestEvent

定义通知收到http auth认证请求。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                                 | 必填 | 说明                         | Android平台 | iOS平台 |
| ------- | ------------------------------------ | ---- | ---------------------------- | ----------- | ------- |
| handler | [HttpAuthHandler](#httpauthhandler9) | 是   | 通知Web组件用户操作行为。    | 支持        | 支持    |
| host    | string                               | 是   | HTTP身份验证凭据应用的主机。 | 支持        | 支持    |
| realm   | string                               | 是   | HTTP身份验证凭据应用的域。   | 支持        | 支持    |

## OnPermissionRequestEvent

定义通知收到获取权限请求。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型                                     | 必填 | 说明                      | Android平台 | iOS平台 |
| ------- | ---------------------------------------- | ---- | ------------------------- | ----------- | ------- |
| request | [PermissionRequest](#permissionrequest9) | 是   | 通知Web组件用户操作行为。 | 支持        | 支持    |

## OnScrollEvent

定义滚动条滑动到指定位置时触发。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 类型   | 必填 | 说明                                         | Android平台 | iOS平台 |
| ------- | ------ | ---- | -------------------------------------------- | ----------- | ------- |
| xOffset | number | 是   | 以网页最左端为基准，水平滚动条滚动所在位置。 | 支持        | 支持    |
| yOffset | number | 是   | 以网页最上端为基准，竖直滚动条滚动所在位置。 | 支持        | 支持    |

## OnPageVisibleEvent

定义旧页面不再呈现，新页面即将可见时触发的回调函数。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称 | 类型   | 必填 | 说明                                              | Android平台 | iOS平台 |
| ---- | ------ | ---- | ------------------------------------------------- | ----------- | ------- |
| url  | string | 是   | 旧页面不再呈现，新页面即将可见时新页面的url地址。 | 支持        | 支持    |

## OnLoadInterceptEvent

定义截获资源加载时触发的回调。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称 | 类型                                      | 必填 | 说明                | Android平台 | iOS平台 |
| ---- | ----------------------------------------- | ---- | ------------------- | ----------- | ------- |
| data | [WebResourceRequest](#webresourcerequest) | 是   | url请求的相关信息。 | 支持        | 支持    |

## OnInterceptRequest<sup>22+</sup>

onInterceptRequest(callback: Callback<OnInterceptRequestEvent, WebResourceResponse>)

当Web组件加载URL之前触发该回调，用于拦截URL并返回响应数据。`onInterceptRequest`可拦截所有跳转请求并返回响应数据，但无法访问POST请求体（Body）内容，且不支持分片缓冲（buffer）类型数据获取。此类场景需改用[WebSchemeHandler](../apis/arkts-apis-webview-WebSchemeHandler.md)实现，依据具体业务需求进行判断。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| callback | Callback\<[OnInterceptRequestEvent](#oninterceptrequestevent22), [WebResourceResponse](#webresourceresponse)\> | 是   | 当Web组件加载url之前触发。<br>返回值[WebResourceResponse](#webresourceresponse)。返回响应数据则按照响应数据加载，无响应数据则返回null表示按照原来的方式加载。 |

**示例：**

  ```ts
  // xxx.ets
  import { webview } from '@kit.ArkWeb';

  @Entry
  @Component
  struct WebComponent {
    controller: webview.WebviewController = new webview.WebviewController();
    responseWeb: WebResourceResponse = new WebResourceResponse();
    heads: Header[] = new Array();
    webData: string = "<!DOCTYPE html>\n" +
      "<html>\n" +
      "<head>\n" +
      "<title>intercept test</title>\n" +
      "</head>\n" +
      "<body>\n" +
      "<h1>intercept test</h1>\n" +
      "</body>\n" +
      "</html>";

    build() {
      Column() {
        Web({ src: 'www.example.com', controller: this.controller })
          .onInterceptRequest((event) => {
            if (event) {
              console.info('url:' + event.request.getRequestUrl());
            }
            let head1: Header = {
              headerKey: "Connection",
              headerValue: "keep-alive"
            }
            let head2: Header = {
              headerKey: "Cache-Control",
              headerValue: "no-cache"
            }
            // 将新元素追加到数组的末尾，并返回数组的新长度。
            let length = this.heads.push(head1);
            length = this.heads.push(head2);
            console.info('The response header result length is :' + length);
            const promise: Promise<String> = new Promise((resolve: Function, reject: Function) => {
              this.responseWeb.setResponseHeader(this.heads);
              this.responseWeb.setResponseData(this.webData);
              this.responseWeb.setResponseEncoding('utf-8');
              this.responseWeb.setResponseMimeType('text/html');
              this.responseWeb.setResponseCode(200);
              this.responseWeb.setReasonMessage('OK');
              resolve("success");
            })
            promise.then(() => {
              console.info("prepare response ready");
              this.responseWeb.setResponseIsReady(true);
            })
            this.responseWeb.setResponseIsReady(false);
            return this.responseWeb;
          })
      }
    }
  }
  ```

## OnInterceptRequestEvent<sup>22+</sup>

定义当Web组件加载url之前触发。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

| 名称    | 类型                                      | 只读 | 可选 | 说明                |
| ------- | ----------------------------------------- | ---- | ---- | ------------------- |
| request | [WebResourceRequest](#WebResourceRequest) | 否   | 否   | url请求的相关信息。 |

## OnSslErrorEventReceiveEvent<sup>22+</sup>

定义网页收到SSL错误时触发。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

| 名称          | 类型                                  | 只读 | 可选 | 说明                      |
| ------------- | ------------------------------------- | ---- | ---- | ------------------------- |
| handler       | [SslErrorHandler](#SslErrorHandler22) | 否   | 否   | 通知Web组件用户操作行为。 |
| error         | [SslError](#sslerror22)               | 否   | 否   | 错误码。                  |
| certChainData | Array<Uint8Array\>                    | 否   | 是   | 证书链数据。              |

## onSslErrorEventReceive<sup>22+</sup>

onSslErrorEventReceive(callback: Callback\<OnSslErrorEventReceiveEvent\>)

通知用户加载资源时发生SSL错误，只支持主资源。
如果需要支持子资源，请使用[OnSslErrorEvent](#onsslerrorevent22)接口。

> **说明：**
>
> - 主资源：浏览器加载网页的入口文件，通常是HTML文档。  
> - 子资源：主资源中引用的依赖文件，由主资源解析过程中遇到特定标签时触发加载。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                      |
| -------- | ------------------------------------------------------------ | ---- | ------------------------- |
| callback | Callback\<[OnSslErrorEventReceiveEvent](#onsslerroreventreceiveevent22)\> | 是   | 当网页收到SSL错误时触发。 |

**示例：**

  ```ts
  // xxx.ets
  import { webview } from '@kit.ArkWeb';
  import { cert } from '@kit.DeviceCertificateKit';
  
  function LogCertInfo(certChainData : Array<Uint8Array> | undefined) {
    if (!(certChainData instanceof Array)) {
      console.error('failed, cert chain data type is not array');
      return;
    }

    for (let i = 0; i < certChainData.length; i++) {
      let encodeBlobData: cert.EncodingBlob = {
        data: certChainData[i],
        encodingFormat: cert.EncodingFormat.FORMAT_DER
      }
      cert.createX509Cert(encodeBlobData, (error, x509Cert) => {
        if (error) {
          console.error('Index : ' + i + ',createX509Cert failed, errCode: ' + error.code + ', errMsg: ' + error.message);
        } else {
          console.info('createX509Cert success');
          console.info(ParseX509CertInfo(x509Cert));
        }
      });
    }
    return;
  }
  
  function Uint8ArrayToString(dataArray: Uint8Array) {
    let dataString = '';
    for (let i = 0; i < dataArray.length; i++) {
      dataString += String.fromCharCode(dataArray[i]);
    }
    return dataString;
  }

  function ParseX509CertInfo(x509Cert: cert.X509Cert) {
    let res: string = 'getCertificate success, '
      + 'issuer name = '
      + Uint8ArrayToString(x509Cert.getIssuerName().data) + ', subject name = '
      + Uint8ArrayToString(x509Cert.getSubjectName().data) + ', valid start = '
      + x509Cert.getNotBeforeTime()
      + ', valid end = ' + x509Cert.getNotAfterTime();
    return res;
  }

  @Entry
  @Component
  struct WebComponent {
    controller: webview.WebviewController = new webview.WebviewController();
    uiContext: UIContext = this.getUIContext();

    build() {
      Column() {
        Web({ src: 'www.example.com', controller: this.controller })
          .onSslErrorEventReceive((event) => {
            LogCertInfo(event.certChainData);
            this.uiContext.showAlertDialog({
              title: 'onSslErrorEventReceive',
              message: 'text',
              primaryButton: {
                value: 'confirm',
                action: () => {
                  event.handler.handleConfirm();
                }
              },
              secondaryButton: {
                value: 'cancel',
                action: () => {
                  event.handler.handleCancel();
                }
              },
              cancel: () => {
                event.handler.handleCancel();
              }
            })
          })
      }
    }
  }
  ```

## onSslErrorEvent<sup>22+</sup>

onSslErrorEvent(callback: OnSslErrorEventCallback)

通知用户加载资源（主资源+子资源）时发生SSL错误，如果只想处理主资源的SSL错误，请用[isMainFrame](#ismainframe)字段进行区分。

> **说明：**
>
> - 主资源：浏览器加载网页的入口文件，通常是HTML文档。  
> - 子资源：主资源中引用的依赖文件，由主资源解析过程中遇到特定标签时触发加载。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                  | 必填 | 说明                            |
| -------- | ----------------------------------------------------- | ---- | ------------------------------- |
| callback | [OnSslErrorEventCallback](#onsslerroreventcallback22) | 是   | 通知用户加载资源时发生SSL错误。 |

**示例：**

  ```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { cert } from '@kit.DeviceCertificateKit';

function LogCertInfo(certChainData : Array<Uint8Array> | undefined) {
  if (!(certChainData instanceof Array)) {
    console.error('failed, cert chain data type is not array');
    return;
  }

  for (let i = 0; i < certChainData.length; i++) {
    let encodeBlobData: cert.EncodingBlob = {
      data: certChainData[i],
      encodingFormat: cert.EncodingFormat.FORMAT_DER
    }
    cert.createX509Cert(encodeBlobData, (error, x509Cert) => {
      if (error) {
        console.error('Index : ' + i + ',createX509Cert failed, errCode: ' + error.code + ', errMsg: ' + error.message);
      } else {
        console.info('createX509Cert success');
        console.info(ParseX509CertInfo(x509Cert));
      }
    });
  }
  return;
}

function Uint8ArrayToString(dataArray: Uint8Array) {
  let dataString = '';
  for (let i = 0; i < dataArray.length; i++) {
    dataString += String.fromCharCode(dataArray[i]);
  }
  return dataString;
}

function ParseX509CertInfo(x509Cert: cert.X509Cert) {
  let res: string = 'getCertificate success, '
    + 'issuer name = '
    + Uint8ArrayToString(x509Cert.getIssuerName().data) + ', subject name = '
    + Uint8ArrayToString(x509Cert.getSubjectName().data) + ', valid start = '
    + x509Cert.getNotBeforeTime()
    + ', valid end = ' + x509Cert.getNotAfterTime();
  return res;
}

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  uiContext: UIContext = this.getUIContext();

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
        .onSslErrorEvent((event: SslErrorEvent) => {
          console.info("onSslErrorEvent url: " + event.url);
          console.info("onSslErrorEvent error: " + event.error);
          console.info("onSslErrorEvent originalUrl: " + event.originalUrl);
          console.info("onSslErrorEvent referrer: " + event.referrer);
          console.info("onSslErrorEvent isMainFrame: " + event.isMainFrame);
          LogCertInfo(event.certChainData);
          this.uiContext.showAlertDialog({
            title: 'onSslErrorEvent',
            message: 'text',
            primaryButton: {
              value: 'confirm',
              action: () => {
                event.handler.handleConfirm();
              }
            },
            secondaryButton: {
              value: 'cancel',
              action: () => {
                event.handler.handleCancel();
              }
            },
            cancel: () => {
              event.handler.handleCancel();
            }
          })
        })
    }
  }
}
  ```

## OnSslErrorEventCallback<sup>22+</sup>

type OnSslErrorEventCallback = (sslErrorEvent: SslErrorEvent) => void

用户加载资源时发生SSL错误时触发的回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名        | 类型                              | 必填 | 说明                                        |
| ------------- | --------------------------------- | ---- | ------------------------------------------- |
| sslErrorEvent | [SslErrorEvent](#sslerrorevent22) | 是   | 用户加载资源时发生SSL错误时触发的回调详情。 |

## OnOverrideUrlLoadingCallback<sup>22+</sup>

type OnOverrideUrlLoadingCallback = (webResourceRequest: WebResourceRequest) => boolean

onOverrideUrlLoading的回调。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名             | 类型                                      | 必填 | 说明                |
| ------------------ | ----------------------------------------- | ---- | ------------------- |
| webResourceRequest | [WebResourceRequest](#WebResourceRequest) | 是   | url请求的相关信息。 |

**返回值：**

| 类型    | 说明                                         |
| ------- | -------------------------------------------- |
| boolean | 返回true表示阻止此次加载，否则允许此次加载。 |

