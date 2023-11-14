

# @ohos.web.webview (Webview)

@ohos.web.webview提供web控制能力，[web](../arkui-ts/ts-basic-components-web.md)组件提供具有网页显示能力。

> **说明：**
>
> - 本模块接口从API Version 9开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>
> - 示例效果请以真机运行为准，当前IDE预览器不支持。

## 导入模块

```ts
import web_webview from '@ohos.web.webview';
```


## WebviewController

通过WebviewController可以控制Web组件各种行为。一个WebviewController对象只能控制一个Web组件，且必须在Web组件和WebviewController绑定后，才能调用WebviewController上的方法（静态方法除外）。


### loadUrl

loadUrl(url: string | Resource, headers?: Array\<WebHeader>): void

加载指定的URL。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名  | 类型             | 必填 | 说明                  |
| ------- | ---------------- | ---- | :-------------------- |
| url     | string \| Resource | 是   | 需要加载的 URL。      |
| headers | Array\<[WebHeader](#webheader)> | 否   | URL的附加HTTP请求头。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100003 | Invalid resource path or file type.                          |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('loadUrl')
        .onClick(() => {
          try {
            // 需要加载的URL是string类型。
            this.controller.loadUrl('www.example.com');
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('loadUrl')
        .onClick(() => {
          try {
            // 带参数headers。
            this.controller.loadUrl('https://www.example.com', [{ headerKey: "headerKey", headerValue: "headerValue" }]);
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

加载本地网页，加载本地资源文件通过rawfile方式。


```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('loadUrl')
        .onClick(() => {
          try {
            // 通过$rawfile加载本地资源文件。
            this.controller.loadUrl($rawfile('index.html'));
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### loadData

loadData(data: string, mimeType: string, encoding: string, baseUrl?: string, historyUrl?: string): void

加载指定的数据。
当加载本地图片时，需要开发者取得Android沙箱路径或iOS沙盒路径并拼接图片相对路径。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名     | 类型   | 必填 | 说明                                                         |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| data       | string | 是   | 按照”Base64“或者”URL"编码后的一段字符串。                    |
| mimeType   | string | 是   | 媒体类型（MIME）。                                           |
| encoding   | string | 是   | 编码类型，具体为“Base64"或者”URL编码。                       |
| baseUrl    | string | 否   | 指定的一个URL路径（“http”/“https”/"data"协议），并由Web组件赋值给window.origin。 |
| historyUrl | string | 否   | 用作历史记录所使用的URL。非空时，历史记录以此URL进行管理。当baseUrl为空时，此属性无效。iOS不支持。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100002 | Invalid url.                                                 |
| 401      | Invalid input parameter.                                     |

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('loadData')
        .onClick(() => {
          try {
            this.controller.loadData(
              "<html><body bgcolor=\"white\">Source:<pre>source</pre></body></html>",
              "text/html",
              "UTF-8"
            );
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
  ```

### getUrl

getUrl(): string

获取当前页面的url地址。
Android和iOS的返回值与OpenHarmony不完全相同，以各平台行为为准。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型   | 说明                |
| ------ | ------------------- |
| string | 当前页面的url地址。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('getUrl')
        .onClick(() => {
          try {
            let url = this.controller.getUrl();
            console.log("url: " + url);
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
  ```

### runJavaScript

runJavaScript(script: string, callback : AsyncCallback\<string>): void

异步执行JavaScript脚本，并通过回调方式返回脚本执行的结果。runJavaScript需要在loadUrl完成后，比如onPageEnd中调用。
Android4.4以上版本支持，iOS8.0以上版本支持。
在脚本无返回值场合，iOS返回值为'(null)'

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| -------- | ---------------------- | ---- | ------------------------------------------------------------ |
| script   | string                 | 是   | JavaScript脚本。                                             |
| callback | AsyncCallback\<string> | 是   | 回调执行JavaScript脚本结果。JavaScript脚本若执行失败或无返回值时，返回null。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |
| 401      | Invalid input parameter.                                     |

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();
  @State webResult: string = ''

  build() {
    Column({space:5}) {
      Button('启动脚本')
        .onClick(() => {
          try {
            this.controller.runJavaScript(
              'test()',
              (error, result) => {
                if (error) {
                  console.info(`run JavaScript error: ` + JSON.stringify(error))
                  return;
                }
                if (result) {
                  this.webResult = result
                  console.info(`The test() return value is: ${result}`)
                }
              });
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
        .height(200)
        .javaScriptAccess(true)
    }
  }
}
  ```

 加载的html文件。

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
  <meta charset="utf-8">
  <body>
      Hello world!
  </body>
  <script type="text/javascript">
  function test() {
      console.log('Ark WebComponent')
      return "This value is from index.html"
  }
  </script>
</html>
```

### runJavaScript

runJavaScript(script: string): Promise\<string>

异步执行JavaScript脚本，并通过Promise方式返回脚本执行的结果。runJavaScript需要在loadUrl完成后，比如onPageEnd中调用。
Android4.4以上版本支持，iOS8.0以上版本支持。
在脚本无返回值场合，iOS返回值为'(null)'

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明             |
| ------ | ------ | ---- | ---------------- |
| script | string | 是   | JavaScript脚本。 |

**返回值：**

| 类型             | 说明                                                |
| ---------------- | --------------------------------------------------- |
| Promise\<string> | Promise实例，返回脚本执行的结果，执行失败返回null。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |
| 401      | Invalid input parameter.                                     |

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();
  @State webResult: string = ''

  build() {
    Column() {
      Text(this.webResult).fontSize(20)
      Button('启动脚本')
        .onClick(() => {
          try {
            this.controller.runJavaScript('test()')
              .then((result) => {
                this.webResult = result
                console.info(`The test() return value is: ${result}`);
              })
              .catch((error: business_error.BusinessError) => {
                console.error("error: " + error);
              })
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
        .javaScriptAccess(true)
    }
  }
}
  ```

本地资源文件

```html
<!-- index.html -->
<!DOCTYPE html>
<html>
<meta charset="utf-8">
<body>
Hello world!
</body>
<script type="text/javascript">
  function test() {
      console.log('Ark WebComponent')
      return "This value is from index.html"
  }
  </script>
</html>
```

### accessBackward

accessBackward(): boolean

当前页面是否可后退，即当前页面是否有返回历史记录。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型    | 说明                              |
| ------- | --------------------------------- |
| boolean | 可以后退返回true，否则返回false。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('accessBackward')
        .onClick(() => {
          try {
            let result = this.controller.accessBackward();
            console.log('result:' + result);
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### accessForward

accessForward(): boolean

当前页面是否可前进，即当前页面是否有前进历史记录。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型    | 说明                              |
| ------- | --------------------------------- |
| boolean | 可以前进返回true，否则返回false。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('accessForward')
        .onClick(() => {
          try {
            let result = this.controller.accessForward();
            console.log('result:' + result);
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### backward

backward(): void

按照历史栈，后退一个页面。一般结合accessBackward一起使用。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('backward')
        .onClick(() => {
          try {
            this.controller.backward();
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### forward

forward(): void

按照历史栈，前进一个页面。一般结合accessForward一起使用。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('forward')
        .onClick(() => {
          try {
            this.controller.forward();
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### refresh

refresh(): void

调用此接口通知Web组件刷新网页。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('refresh')
        .onClick(() => {
          try {
            this.controller.refresh();
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

## webDatabase

web组件数据库管理对象。

> **说明：**
>
> 目前调用WebDataBase下的方法，都需要先加载Web组件。


### existHttpAuthCredentials

static existHttpAuthCredentials(): boolean

判断是否存在任何已保存的HTTP身份验证凭据，该方法为同步方法。存在返回true，不存在返回false。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 是否存在任何已保存的HTTP身份验证凭据。存在返回true，不存在返回false |

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('existHttpAuthCredentials')
        .onClick(() => {
          try {
            let result = web_webview.WebDataBase.existHttpAuthCredentials();
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
  ```

### deleteHttpAuthCredentials

static deleteHttpAuthCredentials(): void

清除所有已保存的HTTP身份验证凭据，该方法为同步方法。

**系统能力：** SystemCapability.Web.Webview.Core

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('deleteHttpAuthCredentials')
        .onClick(() => {
          try {
            web_webview.WebDataBase.deleteHttpAuthCredentials();
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
  ```

### getHttpAuthCredentials

static getHttpAuthCredentials(host: string, realm: string): Array\<string>

检索给定主机和域的HTTP身份验证凭据，该方法为同步方法。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                         |
| ------ | ------ | ---- | ---------------------------- |
| host   | string | 是   | HTTP身份验证凭据应用的主机。 |
| realm  | string | 是   | HTTP身份验证凭据应用的域。   |

**返回值：**

| 类型           | 说明                                         |
| -------------- | -------------------------------------------- |
| Array\<string> | 包含用户名和密码的组数，检索失败返回空数组。 |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();
  host: string = "www.spincast.org";
  realm: string = "protected example";
  username_password: string[] = [];

  build() {
    Column() {
      Button('getHttpAuthCredentials')
        .onClick(() => {
          try {
            this.username_password = web_webview.WebDataBase.getHttpAuthCredentials(this.host, this.realm);
            console.log('num: ' + this.username_password.length);
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
  ```

### saveHttpAuthCredentials

static saveHttpAuthCredentials(host: string, realm: string, username: string, password: string): void

保存给定主机和域的HTTP身份验证凭据，该方法为同步方法。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型   | 必填 | 说明                         |
| -------- | ------ | ---- | ---------------------------- |
| host     | string | 是   | HTTP身份验证凭据应用的主机。 |
| realm    | string | 是   | HTTP身份验证凭据应用的域。   |
| username | string | 是   | 用户名。                     |
| password | string | 是   | 密码。                       |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |

**示例：**

  ```ts
// xxx.ets
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();
  host: string = "www.spincast.org";
  realm: string = "protected example";

  build() {
    Column() {
      Button('saveHttpAuthCredentials')
        .onClick(() => {
          try {
            web_webview.WebDataBase.saveHttpAuthCredentials(this.host, this.realm, "Stromgol", "Laroche");
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
  ```

## WebCookieManager

通过WebCookie可以控制Web组件中的cookie的各种行为，其中每个应用中的所有web组件共享一个WebCookieManager实例。

> **说明：**
>
> 目前调用WebCookieManager下的方法，都需要先加载Web组件。

###  configCookie11+

static configCookie(url: string, value: string, callback: AsyncCallback<void>): void

异步callback方式为指定url设置单个cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                | 必填 | 说明                                         |
| -------- | ------------------- | ---- | -------------------------------------------- |
| url      | string              | 是   | 要获取的cookie所属的url，建议使用完整的url。 |
| value    | string              | 是   | 要设置的cookie的值。                         |
| callback | AsyncCallback<void> | 是   | callback回调，用于获取设置cookie的结果。     |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('configCookie')
        .onClick(() => {
          try {
            web_webview.WebCookieManager.configCookie('https://www.example.com', "a=b", (error) => {
              if (error) {
                console.log("error: " + JSON.stringify(error));
              }
            })
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

###  configCookie11+

static configCookie(url: string, value: string): Promise<void>

以异步Promise方式为指定url设置单个cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| url    | string | 是   | 要获取的cookie所属的url，建议使用完整的url。 |
| value  | string | 是   | 要设置的cookie的值。                         |

**返回值：**

| 类型            | 说明                                                   |
| --------------- | ------------------------------------------------------ |
| Promise<string> | Promise实例，用于获取指定url设置单个cookie值是否成功。 |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('configCookie')
        .onClick(() => {
          try {
            web_webview.WebCookieManager.configCookie('https://www.example.com', 'a=b')
              .then(() => {
                console.log('configCookie success!');
              })
              .catch((error:business_error.BusinessError) => {
                console.log('error: ' + JSON.stringify(error));
              })
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

###  fetchCookie11+

static fetchCookie(url: string, callback: AsyncCallback<string>): void

异步callback方式获取指定url对应cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                  | 必填 | 说明                                         |
| -------- | --------------------- | ---- | -------------------------------------------- |
| url      | string                | 是   | 要获取的cookie所属的url，建议使用完整的url。 |
| callback | AsyncCallback<string> | 是   | callback回调，用于获取cookie。               |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('fetchCookie')
        .onClick(() => {
          try {
            web_webview.WebCookieManager.fetchCookie('https://www.example.com', (error, cookie) => {
              if (error) {
                console.log('error: ' + JSON.stringify(error));
                return;
              }
              if (cookie) {
                console.log('fetchCookie cookie = ' + cookie);
              }
            })
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

###  fetchCookie11+

static fetchCookie(url: string): Promise<string>

以Promise方式异步获取指定url对应cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| url    | string | 是   | 要获取的cookie所属的url，建议使用完整的url。 |

**返回值：**

| 类型            | 说明                                         |
| --------------- | -------------------------------------------- |
| Promise<string> | Promise实例，用于获取指定url对应的cookie值。 |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('fetchCookie')
        .onClick(() => {
          try {
            web_webview.WebCookieManager.fetchCookie('https://www.example.com')
              .then(cookie => {
                console.log("fetchCookie cookie = " + cookie);
              })
              .catch((error:business_error.BusinessError) => {
                console.log('error: ' + JSON.stringify(error));
              })
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

###  clearAllCookies11+

static clearAllCookies(callback: AsyncCallback<void>): void

异步callback方式清除所有cookie。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                | 必填 | 说明                                           |
| -------- | ------------------- | ---- | ---------------------------------------------- |
| callback | AsyncCallback<void> | 是   | callback回调，用于获取清除所有cookie是否成功。 |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('clearAllCookies')
        .onClick(() => {
          try {
            web_webview.WebCookieManager.clearAllCookies((error) => {
              if (error) {
                console.log("error: " + error);
              }
            })
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### clearAllCookies11+

static clearAllCookies(): Promise<void>

异步promise方式清除所有cookie。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型          | 说明                                          |
| ------------- | --------------------------------------------- |
| Promise<void> | Promise实例，用于获取清除所有cookie是否成功。 |

**示例：**

```ts
// xxx.ets
import web_webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();

  build() {
    Column() {
      Button('clearAllCookies')
        .onClick(() => {
          try {
            web_webview.WebCookieManager.clearAllCookies()
              .then(() => {
                console.log("clearAllCookies success!");
              })
              .catch((error:business_error.BusinessError) => {
                console.error("error: " + error);
              });
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

## WebHeader

Web组件返回的请求/响应头对象。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称        | 类型   | 可读 | 可写 |说明                 |
| ----------- | ------ | -----|------|------------------- |
| headerKey   | string | 是 | 是 | 请求/响应头的key。   |
| headerValue | string | 是 | 是 | 请求/响应头的value。 |


