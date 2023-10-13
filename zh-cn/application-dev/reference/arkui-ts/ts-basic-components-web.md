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
        Web({ src: 'www.example.com', controller: this.controller })
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
        Web({ src: 'www.example.com', controller: this.controller })
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
        Web({ src: 'www.example.com', controller: this.controller })
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
        Web({ src: 'www.example.com', controller: this.controller })
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
        Web({ src: 'www.example.com', controller: this.controller })
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
        Web({ src: 'www.example.com', controller: this.controller })
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


