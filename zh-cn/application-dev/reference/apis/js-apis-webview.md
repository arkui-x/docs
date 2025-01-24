# @ohos.web.webview (Webview)

@ohos.web.webview提供web控制能力，[web](../arkui-ts/ts-basic-components-web.md)组件提供具有网页显示能力。

> **说明：**
>
> - 本模块接口从API Version 9开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>
> - 示例效果请以真机运行为准，当前IDE预览器不支持。

## 导入模块

```ts
import { webview } from '@kit.ArkWeb';
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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

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
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### accessStep

accessStep(step: number): boolean

当前页面是否可前进或者后退给定的step步。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                                       |
| ------ | ------ | ---- | ------------------------------------------ |
| step   | number | 是   | 要跳转的步数，正数代表前进，负数代表后退。 |

**返回值：**

| 类型    | 说明                 |
| ------- | -------------------- |
| boolean | 页面是否前进或后退。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State steps: number = 2;

  build() {
    Column() {
      Button('accessStep')
        .onClick(() => {
          try {
            let result = this.controller.accessStep(this.steps);
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

### clearHistory

clearHistory(): void

删除所有前进后退记录。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('clearHistory')
        .onClick(() => {
          try {
            this.controller.clearHistory();
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

### scrollTo

scrollTo(x:number, y:number): void

将页面滚动到指定的绝对位置。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                    |
| ------ | ------ | ---- | ------------------------------------------------------- |
| x      | number | 是   | 绝对位置的水平坐标，当传入数值为负数时，按照传入0处理。 |
| y      | number | 是   | 绝对位置的垂直坐标，当传入数值为负数时，按照传入0处理。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('scrollTo')
        .onClick(() => {
          try {
            this.controller.scrollTo(50, 50);
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src:'https://www.example.com', controller: this.controller })
    }
  }
}
```

### scrollBy

scrollBy(deltaX:number, deltaY:number): void

将页面滚动指定的偏移量。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                               |
| ------ | ------ | ---- | ---------------------------------- |
| deltaX | number | 是   | 水平偏移量，其中水平向右为正方向。 |
| deltaY | number | 是   | 垂直偏移量，其中垂直向下为正方向。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('scrollBy')
        .onClick(() => {
          try {
            this.controller.scrollBy(50, 50);
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

### stop

stop(): void

停止页面加载。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('stop')
        .onClick(() => {
          try {
            this.controller.stop();
          } catch (error) {
            let e:business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        });
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```

### zoom

zoom(factor: number): void

调整当前网页的缩放比例。

android平台的参数范围是(0.01,100]，iOS平台的参数范围是(0,100]。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| factor | number | 是   | 基于当前网页所需调整的相对缩放比例，入参要求大于0，当入参为1时为默认加载网页的缩放比例，入参小于1为缩小，入参大于1为放大。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State factor: number = 1.2;

  build() {
    Column() {
      Button('zoom')
        .onClick(() => {
          try {
            this.controller.zoom(this.factor);
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

### zoomIn<sup>16+</sup>

zoomIn(): void

调用此接口将当前网页进行放大，比例为20%。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100004 | Function not enabled.                                    |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('zoomIn')
        .onClick(() => {
          try {
            this.controller.zoomIn();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### zoomOut<sup>16+</sup>

zoomOut(): void

调用此接口将当前网页进行缩小，比例为20%。

**系统能力：** SystemCapability.Web.Webview.Core

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100004 | Function not enabled.                                    |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('zoomOut')
        .onClick(() => {
          try {
            this.controller.zoomOut();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

###  getOriginalUrl<sup>16+</sup>

getOriginalUrl(): string

获取当前页面的原始url地址。

风险提示：如果想获取url来做JavascriptProxy通信接口认证，请使用getLastJavascriptProxyCallingFrameUrl<sup>12+</sup>

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| string | 当前页面的原始url地址。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('getOrgUrl')
        .onClick(() => {
          try {
            let url = this.controller.getOriginalUrl();
            console.log("original url: " + url);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getCustomUserAgent<sup>10+</sup>

getCustomUserAgent(): string

获取自定义用户代理。

在直接调用getCustomUserAgent的场景下，Android可以获取到手机型号信息，iOS返回空。

**系统能力：**  SystemCapability.Web.Webview.Core

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| string | 用户自定义代理信息。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State userAgent: string = ''

  build() {
    Column() {
      Button('getCustomUserAgent')
        .onClick(() => {
          try {
            this.userAgent = this.controller.getCustomUserAgent();
            console.log("userAgent: " + this.userAgent);
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

### setCustomUserAgent<sup>10+</sup>

setCustomUserAgent(userAgent: string): void

设置自定义用户代理。

**系统能力：**  SystemCapability.Web.Webview.Core

**参数：**

| 参数名    | 类型   | 必填 | 说明                 |
| --------- | ------ | ---- | -------------------- |
| userAgent | string | 是   | 用户自定义代理信息。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State userAgent: string = 'test'

  build() {
    Column() {
      Button('setCustomUserAgent')
        .onClick(() => {
          try {
            this.controller.setCustomUserAgent(this.userAgent);
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

### removeCache

removeCache(clearRom: boolean): void

清除应用中的资源缓存文件，此方法将会清除同一应用中所有webview的缓存文件。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型    | 必填 | 说明                                                         |
| -------- | ------- | ---- | ------------------------------------------------------------ |
| clearRom | boolean | 是   | 设置为true时同时清除rom和ram中的缓存，设置为false时只清除ram中的缓存。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('removeCache')
        .onClick(() => {
          try {
            this.controller.removeCache(true);
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

### backOrForward

backOrForward(step: number): void

按照历史栈，前进或者后退指定步长的页面，当历史栈中不存在对应步长的页面时，不会进行页面跳转。

前进或者后退页面时，直接使用已加载过的网页，无需重新加载网页。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                   |
| ------ | ------ | ---- | ---------------------- |
| step   | number | 是   | 需要前进或后退的步长。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State step: number = -2;

  build() {
    Column() {
      Button('backOrForward')
        .onClick(() => {
          try {
            this.controller.backOrForward(this.step);
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

### getTitle

getTitle(): string

获取当前网页的标题。

在不设置title的场景下，以各平台行为为准。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| string | 当前网页的标题。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例:**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('getTitle')
        .onClick(() => {
          try {
            let title = this.controller.getTitle();
            console.log("title: " + title);
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

### getBackForwardEntries

getBackForwardEntries(): BackForwardList

获取当前Webview的历史信息列表。

PixelMap不支持跨平台。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型                                | 说明                        |
| ----------------------------------- | --------------------------- |
| [BackForwardList](#backforwardlist) | 当前Webview的历史信息列表。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State log: string = "";

  build() {
    Column() {
      Text("log: " + this.log).fontSize(16)
      Button('getBackForwardEntries')
        .onClick(() => {
          try {
            this.log = ""
            for (let i = 0; i < this.controller.getBackForwardEntries().size; i++) {
              let tempLog: string = `
              index: ${i}
              title: ${this.controller.getBackForwardEntries().getItemAtIndex(i).title}
              historyUrl: ${this.controller.getBackForwardEntries().getItemAtIndex(i).historyUrl}
              historyRawUrl: ${this.controller.getBackForwardEntries().getItemAtIndex(i).historyRawUrl}
`;
              console.log(tempLog);
              this.log += tempLog;
            }
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

### getPageHeight

getPageHeight(): number

获取当前网页的页面高度。

不同机型获取的页面高度会有所差异。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| number | 当前网页的页面高度。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例:**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('getPageHeight')
        .onClick(() => {
          try {
            let pageHeight = this.controller.getPageHeight();
            console.log("pageHeight : " + pageHeight);
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

###  getWebId<sup>16+</sup>

getWebId(): number

获取当前Web组件的索引值，用于多个Web组件的管理。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型   | 说明                  |
| ------ | --------------------- |
| number | 当前Web组件的索引值。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('getWebId')
        .onClick(() => {
          try {
            let id = this.controller.getWebId();
            console.log("id: " + id);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### pageDown<sup>16+</sup>

pageDown(bottom: boolean): void

将Webview的内容向下滚动半个视框大小或者跳转到页面最底部，通过bottom入参控制。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                         |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| bottom | boolean | 是   | 是否跳转到页面最底部，设置为false时将页面内容向下滚动半个视框大小，设置为true时跳转到页面最底部。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('pageDown')
        .onClick(() => {
          try {
            this.controller.pageDown(false);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### pageUp<sup>16+</sup>

pageUp(top: boolean): void

将Webview的内容向上滚动半个视框大小或者跳转到页面最顶部，通过top入参控制。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                        |
|-----| ------- | ---- | ----------------------------------------------------------- |
| top | boolean | 是   | 是否跳转到页面最顶部，设置为false时将页面内容向上滚动半个视框大小，设置为true时跳转到页面最顶部。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.  |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('pageUp')
        .onClick(() => {
          try {
            this.controller.pageUp(false);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

###  postUrl<sup>16+</sup>

postUrl(url: string, postData: ArrayBuffer): void

使用"POST"方法加载带有postData的url。如果url不是网络url，则会使用[loadUrl](#loadurl)方法加载url，忽略postData参数。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型        | 必填 | 说明                                                         |
| -------- | ----------- | ---- | ------------------------------------------------------------ |
| url      | string      | 是   | 需要加载的 URL。                                             |
| postData | ArrayBuffer | 是   | 使用"POST"方法传递数据。 该请求必须采用"application/x-www-form-urlencoded"编码。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100002 | Invalid url.                                                 |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.                                   |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

class TestObj {
  constructor() {
  }

  test(str: string): ArrayBuffer {
    let buf = new ArrayBuffer(str.length);
    let buff = new Uint8Array(buf);

    for (let i = 0; i < str.length; i++) {
      buff[i] = str.charCodeAt(i);
    }
    return buf;
  }
}

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State testObjtest: TestObj = new TestObj();

  build() {
    Column() {
      Button('postUrl')
        .onClick(() => {
          try {
            // 数据转化为ArrayBuffer类型。
            let postData = this.testObjtest.test("Name=test&Password=test");
            this.controller.postUrl('www.example.com', postData);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: '', controller: this.controller })
    }
  }
}
```

###  setWebDebuggingAccess<sup>16+</sup>

static setWebDebuggingAccess(webDebuggingAccess: boolean): void

设置是否启用网页调试功能，默认不开启。详情请参考:

Android：[DevTools工具](./web-debugging-with-devtools-android.md);iOS：[DevTools工具](./web-debugging-with-devtools-ios.md)。

安全提示：启用网页调试功能可以让用户检查修改Web页面内部状态，存在安全隐患，不建议在应用正式发布版本中启用。

iOS16.4及以上版本支持

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名             | 类型    | 必填 | 说明                       |
| ------------------ | ------- | ---- | -------------------------- |
| webDebuggingAccess | boolean | 是   | 设置是否启用网页调试功能。 |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  aboutToAppear(): void {
    try {
      webview.WebviewController.setWebDebuggingAccess(true);
    } catch (error) {
      console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
    }
  }

  build() {
    Column() {
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### runJavaScriptExt<sup>16+</sup>

runJavaScriptExt(script: string | ArrayBuffer, callback : AsyncCallback\<JsMessageExt\>): void

异步执行JavaScript脚本，并通过回调方式返回脚本执行的结果。runJavaScriptExt需要在loadUrl完成后，比如onPageEnd中调用。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：**Android、iOS

**参数：**

| 参数名   | 类型                                                       | 必填 | 说明                         | Android平台                      | iOS平台                          |
| -------- | ---------------------------------------------------------- | ---- | ---------------------------- | -------------------------------- | -------------------------------- |
| script   | string \| ArrayBuffer                                      | 是   | JavaScript脚本。             | 支持string,**不支持ArrayBuffer** | 支持string,**不支持ArrayBuffer** |
| callback | AsyncCallback\<[JsMessageExt](#jsmessageext16)> | 是   | 回调执行JavaScript脚本结果。 | 支持                             | 支持                             |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State msg1: string = '';
  @State msg2: string = '';

  build() {
    Column() {
      Text(this.msg1).fontSize(20)
      Text(this.msg2).fontSize(20)
      Web({ src: $rawfile('index.html'), controller: this.controller })
        .javaScriptAccess(true)
        .onPageEnd(e => {
          try {
            this.controller.runJavaScriptExt(
              'test()',
              (error, result) => {
                if (error) {
                  console.error(`run JavaScript error, ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`)
                  return;
                }
                if (result) {
                  try {
                    let type = result.getType();
                    switch (type) {
                      case webview.JsMessageType.STRING: {
                        this.msg1 = "result type:" + typeof (result.getString());
                        this.msg2 = "result getString:" + ((result.getString()));
                        break;
                      }
                      case webview.JsMessageType.NUMBER: {
                        this.msg1 = "result type:" + typeof (result.getNumber());
                        this.msg2 = "result getNumber:" + ((result.getNumber()));
                        break;
                      }
                      case webview.JsMessageType.BOOLEAN: {
                        this.msg1 = "result type:" + typeof (result.getBoolean());
                        this.msg2 = "result getBoolean:" + ((result.getBoolean()));
                        break;
                      }
                      case webview.JsMessageType.ARRAY: {
                        this.msg1 = "result type:" + typeof (result.getArray());
                        this.msg2 = "result getArray:" + result.getArray();
                        break;
                      }
                      default: {
                        this.msg1 = "default break, type:" + type;
                        break;
                      }
                    }
                  }
                  catch (resError) {
                    console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
                  }
                }
              });
            if (e) {
              console.info('url: ', e.url);
            }
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
    }
  }
}
```

### runJavaScriptExt<sup>16+</sup>

runJavaScriptExt(script: string | ArrayBuffer): Promise<[JsMessageExt](#jsmessageext16)>

异步执行JavaScript脚本，并通过Promise方式返回脚本执行的结果。runJavaScriptExt需要在loadUrl完成后，比如onPageEnd中调用。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS平台

**参数：**

| 参数名 | 类型                  | 必填 | 说明             | Android平台                      | iOS平台                          |
| ------ | --------------------- | ---- | ---------------- | -------------------------------- | -------------------------------- |
| script | string \| ArrayBuffer | 是   | JavaScript脚本。 | 支持string,**不支持ArrayBuffer** | 支持string,**不支持ArrayBuffer** |

**返回值：**

| 类型                                     | 说明                              |
| ---------------------------------------- | --------------------------------- |
| Promise<[JsMessageExt](#jsmessageext16)> | Promise实例，返回脚本执行的结果。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State webResult: string = '';
  @State msg1: string = '';
  @State msg2: string = '';

  build() {
    Column() {
      Text(this.webResult).fontSize(20)
      Text(this.msg1).fontSize(20)
      Text(this.msg2).fontSize(20)
      Web({ src: $rawfile('index.html'), controller: this.controller })
        .javaScriptAccess(true)
        .onPageEnd(() => {
          this.controller.runJavaScriptExt('test()')
            .then((result) => {
              try {
                let type = result.getType();
                switch (type) {
                  case webview.JsMessageType.STRING: {
                    this.msg1 = "result type:" + typeof (result.getString());
                    this.msg2 = "result getString:" + ((result.getString()));
                    break;
                  }
                  case webview.JsMessageType.NUMBER: {
                    this.msg1 = "result type:" + typeof (result.getNumber());
                    this.msg2 = "result getNumber:" + ((result.getNumber()));
                    break;
                  }
                  case webview.JsMessageType.BOOLEAN: {
                    this.msg1 = "result type:" + typeof (result.getBoolean());
                    this.msg2 = "result getBoolean:" + ((result.getBoolean()));
                    break;
                  }
                  case webview.JsMessageType.ARRAY: {
                    this.msg1 = "result type:" + typeof (result.getArray());
                    this.msg2 = "result getArray:" + result.getArray();
                    break;
                  }
                  default: {
                    this.msg1 = "default break, type:" + type;
                    break;
                  }
                }
              }
              catch (resError) {
                console.error(`ErrorCode: ${(resError as BusinessError).code},  Message: ${(resError as BusinessError).message}`);
              }
            }).catch((error: BusinessError) => {
            console.error("error: " + error);
          })
        })
    }
  }
}
```

加载的html文件。

```html
<!-- index.html -->
<!DOCTYPE html>
<html lang="en-gb">
<body>
<h1>run JavaScript Ext demo</h1>
</body>
<script type="text/javascript">
function test() {
  return "hello, world";
}
</script>
</html>
```

### setDownloadDelegate<sup>16+</sup>

setDownloadDelegate(delegate: WebDownloadDelegate): void

为当前的Web组件设置一个WebDownloadDelegate，该delegate用来接收页面内触发的下载与下载的进展。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                       |
| -------- | --------------------------------------------- | ---- | -------------------------- |
| delegate | [WebDownloadDelegate](#webdownloaddelegate16) | 是   | 用来接收下载进回调的委托。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### startDownload<sup>16+</sup>

startDownload(url: string): void

使用Web组件的下载能力来下载指定的url, 比如下载网页中指定的图片。

**Android平台AndroidManifest.xml文件中需要添加权限：**

```
android.permission.WRITE_EXTERNAL_STORAGE
android.permission.READ_EXTERNAL_STORAGE
```

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明       |
| ------ | ------ | ---- | ---------- |
| url    | string | 是   | 下载地址。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100002 | Invalid url.                                                 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

## WebMessagePort

通过WebMessagePort可以进行消息的发送以及接收。

### createWebMessagePorts

createWebMessagePorts(isExtentionType?: boolean): Array\<WebMessagePort>

创建Web消息端口。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名                        | 类型    | 必填 | 说明                                                         |
| ----------------------------- | ------- | ---- | :----------------------------------------------------------- |
| isExtentionType<sup>10+</sup> | boolean | 否   | 是否使用扩展增强接口，默认false不使用。 从API version 10开始，该接口支持此参数。跨平台只支持false。 |

**返回值：**

| 类型                   | 说明              |
| ---------------------- | ----------------- |
| Array\<WebMessagePort> | web消息端口列表。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  ports: webview.WebMessagePort[] = [];

  build() {
    Column() {
      Button('createWebMessagePorts')
        .onClick(() => {
          try {
            this.ports = this.controller.createWebMessagePorts();
            console.log("createWebMessagePorts size:" + this.ports.length)
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

### postMessage

postMessage(name: string, ports: Array\<WebMessagePort>, uri: string): void

发送Web消息端口到HTML5。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型                   | 必填 | 说明               |
| ------ | ---------------------- | ---- | :----------------- |
| name   | string                 | 是   | 要发送的消息名称。 |
| ports  | Array\<WebMessagePort> | 是   | 要发送的消息端口。 |
| uri    | string                 | 是   | 接收该消息的URI。  |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Invalid input parameter.                                     |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  ports: webview.WebMessagePort[] = [];
  @State sendFromEts: string = 'Send this message from ets to HTML';
  @State receivedFromHtml: string = 'Display received message send from HTML';

  build() {
    Column() {
      // 展示接收到的来自HTML的内容
      Text(this.receivedFromHtml)
      // 输入框的内容发送到html
      TextInput({ placeholder: 'Send this message from ets to HTML' })
        .onChange((value: string) => {
          this.sendFromEts = value;
        })

      Button('postMessage')
        .onClick(() => {
          try {
            // 1、创建两个消息端口。
            this.ports = this.controller.createWebMessagePorts();
            // 2、在应用侧的消息端口(如端口1)上注册回调事件。
            this.ports[1].onMessageEvent((result: webview.WebMessage) => {
              let msg = 'Got msg from HTML:';
              if (typeof(result) == "string") {
                console.log("received string message from html5, string is:" + result);
                msg = msg + result;
              } else {
                console.log("not support");
              }
              this.receivedFromHtml = msg;
            })
            // 3、将另一个消息端口(如端口0)发送到HTML侧，由HTML侧保存并使用。
            this.controller.postMessage('__init_port__', [this.ports[0]], '*');
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })

      // 4、使用应用侧的端口给另一个已经发送到html的端口发送消息。
      Button('SendDataToHTML')
        .onClick(() => {
          try {
            if (this.ports && this.ports[1]) {
              this.ports[1].postMessageEvent(this.sendFromEts);
            } else {
              console.error(`ports is null, Please initialize first`);
            }
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
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
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>WebView Message Port Demo</title>
</head>

  <body>
    <h1>WebView Message Port Demo</h1>
    <div>
        <input type="button" value="SendToEts" onclick="PostMsgToEts(msgFromJS.value);"/><br/>
        <input id="msgFromJS" type="text" value="send this message from HTML to ets"/><br/>
    </div>
    <p class="output">display received message send from ets</p>
  </body>
  <script src="xxx.js"></script>
</html>
```

```js
//xxx.js
var h5Port;
var output = document.querySelector('.output');
window.addEventListener('message', function (event) {
    if (event.data == '__init_port__') {
        if (event.ports[0] != null) {
            h5Port = event.ports[0]; // 1. 保存从ets侧发送过来的端口
            h5Port.onmessage = function (event) {
              // 2. 接收ets侧发送过来的消息.
              var msg = 'Got message from ets:';
              var result = event.data;
              if (typeof(result) == "string") {
                console.log("received string message from html5, string is:" + result);
                msg = msg + result;
              } else {
                console.log("not support");
              }
              output.innerHTML = msg;
            }
        }
    }
})

// 3. 使用h5Port往ets侧发送消息.
function PostMsgToEts(data) {
    if (h5Port) {
      h5Port.postMessage(data);
    } else {
      console.error("h5Port is null, Please initialize first");
    }
}
```

### postMessageEvent

postMessageEvent(message: WebMessage): void

发送消息。完整示例代码参考[postMessage](#postmessage)。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名  | 类型                      | 必填 | 说明           |
| ------- | ------------------------- | ---- | :------------- |
| message | [WebMessage](#webmessage) | 是   | 要发送的消息。 |

**错误码：**

| 错误码ID | 错误信息                                               |
| -------- | ------------------------------------------------------ |
| 401      | Invalid input parameter.                               |
| 17100010 | Can not post message using this port.（只支持Android） |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  ports: webview.WebMessagePort[] = [];

  build() {
    Column() {
      Button('postMessageEvent')
        .onClick(() => {
          try {
            this.ports = this.controller.createWebMessagePorts();
            this.controller.postMessage('__init_port__', [this.ports[0]], '*');
            this.ports[1].postMessageEvent("post message from ets to html5");
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

### onMessageEvent

onMessageEvent(callback: (result: WebMessage) => void): void

注册回调函数，接收HTML5侧发送过来的消息。完整示例代码参考[postMessage](#postmessage)。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型                      | 必填 | 说明           |
| ------ | ------------------------- | ---- | :------------- |
| result | [WebMessage](#webmessage) | 是   | 接收到的消息。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100006 | Can not register message event using this port.（只支持Android） |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  ports: webview.WebMessagePort[] = [];

  build() {
    Column() {
      Button('onMessageEvent')
        .onClick(() => {
          try {
            this.ports = this.controller.createWebMessagePorts();
            this.ports[1].onMessageEvent((msg) => {
              if (typeof(msg) == "string") {
                console.log("received string message from html5, string is:" + msg);
              } else {
                console.log("not support");
              }
            })
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

### close

close(): void

关闭该消息端口。在使用close前，请先使用[createWebMessagePorts](#createwebmessageports)创建消息端口。

**系统能力：** SystemCapability.Web.Webview.Core

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  msgPort: webview.WebMessagePort[] = [];

  build() {
    Column() {
      // 先使用createWebMessagePorts创建端口。
      Button('createWebMessagePorts')
        .onClick(() => {
          try {
            this.msgPort = this.controller.createWebMessagePorts();
            console.log("createWebMessagePorts size:" + this.msgPort.length)
          } catch (error) {
            let e: business_error.BusinessError = error as business_error.BusinessError;
            console.error(`ErrorCode: ${e.code},  Message: ${e.message}`);
          }
        })
      Button('close')
        .onClick(() => {
          try {
            if (this.msgPort && this.msgPort.length == 2) {
              this.msgPort[1].close();
            } else {
              console.error("msgPort is null, Please initialize first");
            }
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

### postMessageEventExt<sup>16+</sup>

postMessageEventExt(message: WebMessageExt): void

发送[WebMessageType](#webmessagetype16)类型消息给HTML5侧，必须先调用[onMessageEventExt](#onmessageeventext16)，否则会发送失败。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                              | 必填 | 说明           |
| ------- | --------------------------------- | ---- | -------------- |
| message | [WebMessageExt](#webmessageext16) | 是   | 要发送的消息。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100010 | Failed to post messages through the port.                    |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

### onMessageEventExt<sup>16+</sup>

onMessageEventExt(callback: (result: WebMessageExt) => void): void

在应用侧的消息端口上注册回调函数，接收HTML5侧发送过来的[WebMessageType](#webmessagetype16)类型消息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                | 必填 | 说明           |
| -------- | --------------------------------------------------- | ---- | -------------- |
| callback | (result: [WebMessageExt](#webmessageext16)) => void | 是   | 接收到的消息。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100006 | Failed to register a message event for the port.             |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

class TestObj {
  test(str: string): ArrayBuffer {
    let buf = new ArrayBuffer(str.length);
    let buff = new Uint8Array(buf);

    for (let i = 0; i < str.length; i++) {
      buff[i] = str.charCodeAt(i);
    }
    return buf;
  }
}

// 应用与网页互发消息的示例：使用"init_web_messageport"的通道，通过端口0在应用侧接受网页发送的消息，通过端口1在网页侧接受应用发送的消息。
@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  ports: webview.WebMessagePort[] = [];
  nativePort: webview.WebMessagePort | null = null;
  @State msg1: string = "";
  @State msg2: string = "";
  message: webview.WebMessageExt = new webview.WebMessageExt();
  @State testObjtest: TestObj = new TestObj();

  build() {
    Column() {
      Text(this.msg1).fontSize(16)
      Text(this.msg2).fontSize(16)
      Button('SendToH5 setString').margin({
        right: 800,
      })
        .onClick(() => {
          // 使用本侧端口发送消息给HTML5
          try {
            console.log("In ArkTS side send true start");
            if (this.nativePort) {
              this.message.setType(1);
              this.message.setString("helloFromEts");
              this.nativePort.postMessageEventExt(this.message);
            }
          }
          catch (error) {
            console.error(`In ArkTS side send message catch error, ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
        Button('SendToH5 setNumber').margin({
        top: 10,
        right: 800,
      })
        .onClick(() => {
          // 使用本侧端口发送消息给HTML5
          try {
            console.log("In ArkTS side send true start");
            if (this.nativePort) {
              this.message.setType(2);
              this.message.setNumber(12345);
              this.nativePort.postMessageEventExt(this.message);
            }
          }
          catch (error) {
            console.error(`In ArkTS side send message catch error, ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('SendToH5 setBoolean').margin({
        top: -90,
      })
        .onClick(() => {
          // 使用本侧端口发送消息给HTML5
          try {
            console.log("In ArkTS side send true start");
            if (this.nativePort) {
              this.message.setType(3);
              this.message.setBoolean(true);
              this.nativePort.postMessageEventExt(this.message);
            }
          }
          catch (error) {
            console.error(`In ArkTS side send message catch error, ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('SendToH5 setArrayBuffer').margin({
        top: 10,
      })
        .onClick(() => {
          // 使用本侧端口发送消息给HTML5
          try {
            console.log("In ArkTS side send true start");
            if (this.nativePort) {
              this.message.setType(4);
              this.message.setArrayBuffer(this.testObjtest.test("Name=test&Password=test"));
              this.nativePort.postMessageEventExt(this.message);
            }
          }
          catch (error) {
            console.error(`In ArkTS side send message catch error, ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('SendToH5 setArray').margin({
        top: -90,
        left: 800,
      })
        .onClick(() => {
          // 使用本侧端口发送消息给HTML5
          try {
            console.log("In ArkTS side send true start");
            if (this.nativePort) {
              this.message.setType(5);
              this.message.setArray([1, 2, 3]);
              this.nativePort.postMessageEventExt(this.message);
            }
          }
          catch (error) {
            console.error(`In ArkTS side send message catch error, ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('SendToH5 setError').margin({
        top: 10,
        left: 800,
      })
        .onClick(() => {
          // 使用本侧端口发送消息给HTML5
          try {
            console.log("In ArkTS side send true start");
            throw new ReferenceError("ReferenceError");
          }
          catch (error) {
            if (this.nativePort) {
              this.message.setType(6);
              this.message.setError(error);
              this.nativePort.postMessageEventExt(this.message);
            }
            console.error(`In ArkTS side send message catch error, ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })

      Web({ src: $rawfile('index.html'), controller: this.controller })
        .onPageEnd(() => {
          console.log("In ArkTS side message onPageEnd init message channel");
          // 1. 创建消息端口
          this.ports = this.controller.createWebMessagePorts(true);
          // 2. 发送端口1到HTML5
          this.controller.postMessage("init_web_messageport", [this.ports[1]], "*");
          // 3. 保存端口0到本地
          this.nativePort = this.ports[0];
          // 4. 设置回调函数
          this.nativePort.onMessageEventExt((result) => {
            console.log("In ArkTS side got message");
            try {
              let type = result.getType();
              console.log("In ArkTS side getType:" + type);
              switch (type) {
                case webview.WebMessageType.STRING: {
                  this.msg1 = "result type:" + typeof (result.getString());
                  this.msg2 = "result getString:" + ((result.getString()));
                  break;
                }
                case webview.WebMessageType.NUMBER: {
                  this.msg1 = "result type:" + typeof (result.getNumber());
                  this.msg2 = "result getNumber:" + ((result.getNumber()));
                  break;
                }
                case webview.WebMessageType.BOOLEAN: {
                  this.msg1 = "result type:" + typeof (result.getBoolean());
                  this.msg2 = "result getBoolean:" + ((result.getBoolean()));
                  break;
                }
                case webview.WebMessageType.ARRAY_BUFFER: {
                  this.msg1 = "result type:" + typeof (result.getArrayBuffer());
                  this.msg2 = "result getArrayBuffer byteLength:" + ((result.getArrayBuffer().byteLength));
                  break;
                }
                case webview.WebMessageType.ARRAY: {
                  this.msg1 = "result type:" + typeof (result.getArray());
                  this.msg2 = "result getArray:" + result.getArray();
                  break;
                }
                case webview.WebMessageType.ERROR: {
                  this.msg1 = "result type:" + typeof (result.getError());
                  this.msg2 = "result getError:" + result.getError();
                  break;
                }
                default: {
                  this.msg1 = "default break, type:" + type;
                  break;
                }
              }
            }
            catch (error) {
              console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
            }
          });
        })
    }
  }
}
```

加载的html文件。

```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en-gb">
<head>
    <title>WebView MessagePort Demo</title>
</head>

<body>
<h1>Html5 Send and Receive Message</h1>
<h3 id="msg">Receive string:</h3>
<h3 id="msg2">Receive arraybuffer:</h3>
<div style="font-size: 10pt; text-align: center;">
    <input type="button" value="Send String" onclick="postStringToApp();" /><br/>
</div>
</body>
<script src="index.js"></script>
</html>
```

```ts
//index.js
var h5Port;
window.addEventListener('message', function(event) {
    if (event.data == 'init_web_messageport') {
        if(event.ports[0] != null) {
            h5Port = event.ports[0]; // 1. 保存从ets侧发送过来的端口
            h5Port.onmessage = function(event) {
                console.log("hwd In html got message");
                // 2. 接收ets侧发送过来的消息.
                var result = event.data;
                console.log("In html got message, typeof: ", typeof(result));
                console.log("In html got message, result: ", (result));
                if (typeof(result) == "string") {
                    console.log("In html got message, String: ", result);
                    document.getElementById("msg").innerHTML  =  "String:" + result;
                } else if (typeof(result) == "number") {
                  console.log("In html side got message, number: ", result);
                    document.getElementById("msg").innerHTML = "Number:" + result;
                } else if (typeof(result) == "boolean") {
                    console.log("In html side got message, boolean: ", result);
                    document.getElementById("msg").innerHTML = "Boolean:" + result;
                } else if (typeof(result) == "object") {
                    if (result instanceof Error) {
                        console.log("In html error message, err:" + (result));
                        console.log("In html error message, typeof err:" + typeof(result));
                        document.getElementById("msg2").innerHTML  =  "Error:" + result.name + ", msg:" + result.message;
                    } else if (result instanceof Array) {
                        console.log("In html got message, Array");
                        console.log("In html got message, Array length:" + result.length);
                        console.log("In html got message, Array[0]:" + (result[0]));
                        console.log("In html got message, typeof Array[0]:" + typeof(result[0]));
                        document.getElementById("msg2").innerHTML  =  "Array len:" + result.length + ", value:" + result;
                    } else {
                        console.log("In html got message, not any instance of support type");
                        document.getElementById("msg").innerHTML  = "not any instance of support type";
                    }
                } else {
                    console.log("In html got message, not support type");
                    document.getElementById("msg").innerHTML  = "not support type";
                }
            }
            h5Port.onmessageerror = (event) => {
                console.error(`hwd In html Error receiving message: ${event}`);
            };
        }
    }
})

// 使用h5Port往ets侧发送String类型的消息.
function postStringToApp() {
    if (h5Port) {
        console.log("In html send string message");
        h5Port.postMessage("hello");
        console.log("In html send string message end");
    } else {
        console.error("In html h5port is null, please init first");
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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('existHttpAuthCredentials')
        .onClick(() => {
          try {
            let result = webview.WebDataBase.existHttpAuthCredentials();
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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('deleteHttpAuthCredentials')
        .onClick(() => {
          try {
            webview.WebDataBase.deleteHttpAuthCredentials();
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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  host: string = "www.spincast.org";
  realm: string = "protected example";
  username_password: string[] = [];

  build() {
    Column() {
      Button('getHttpAuthCredentials')
        .onClick(() => {
          try {
            this.username_password = webview.WebDataBase.getHttpAuthCredentials(this.host, this.realm);
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
import { webview } from '@kit.ArkWeb';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  host: string = "www.spincast.org";
  realm: string = "protected example";

  build() {
    Column() {
      Button('saveHttpAuthCredentials')
        .onClick(() => {
          try {
            webview.WebDataBase.saveHttpAuthCredentials(this.host, this.realm, "Stromgol", "Laroche");
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

static configCookie(url: string, value: string, callback: AsyncCallback\<void>): void

异步callback方式为指定url设置单个cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                | 必填 | 说明                                         |
| -------- | ------------------- | ---- | -------------------------------------------- |
| url      | string              | 是   | 要获取的cookie所属的url，建议使用完整的url。 |
| value    | string              | 是   | 要设置的cookie的值。                         |
| callback | AsyncCallback\<void> | 是   | callback回调，用于获取设置cookie的结果。     |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('configCookie')
        .onClick(() => {
          try {
            webview.WebCookieManager.configCookie('https://www.example.com', "a=b", (error) => {
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

static configCookie(url: string, value: string): Promise\<void>

以异步Promise方式为指定url设置单个cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| url    | string | 是   | 要获取的cookie所属的url，建议使用完整的url。 |
| value  | string | 是   | 要设置的cookie的值。                         |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('configCookie')
        .onClick(() => {
          try {
            webview.WebCookieManager.configCookie('https://www.example.com', 'a=b')
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

static fetchCookie(url: string, callback: AsyncCallback\<string>): void

异步callback方式获取指定url对应cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                  | 必填 | 说明                                         |
| -------- | --------------------- | ---- | -------------------------------------------- |
| url      | string                | 是   | 要获取的cookie所属的url，建议使用完整的url。 |
| callback | AsyncCallback\<string> | 是   | callback回调，用于获取cookie。               |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('fetchCookie')
        .onClick(() => {
          try {
            webview.WebCookieManager.fetchCookie('https://www.example.com', (error, cookie) => {
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

static fetchCookie(url: string): Promise\<string>

以Promise方式异步获取指定url对应cookie的值。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| url    | string | 是   | 要获取的cookie所属的url，建议使用完整的url。 |

**返回值：**

| 类型            | 说明                                         |
| --------------- | -------------------------------------------- |
| Promise\<string> | Promise实例，用于获取指定url对应的cookie值。 |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('fetchCookie')
        .onClick(() => {
          try {
            webview.WebCookieManager.fetchCookie('https://www.example.com')
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

static clearAllCookies(callback: AsyncCallback\<void>): void

异步callback方式清除所有cookie。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                | 必填 | 说明                                           |
| -------- | ------------------- | ---- | ---------------------------------------------- |
| callback | AsyncCallback\<void> | 是   | callback回调，用于获取清除所有cookie是否成功。 |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('clearAllCookies')
        .onClick(() => {
          try {
            webview.WebCookieManager.clearAllCookies((error) => {
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

static clearAllCookies(): Promise\<void>

异步promise方式清除所有cookie。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型          | 说明                                          |
| ------------- | --------------------------------------------- |
| Promise\<void> | Promise实例，用于获取清除所有cookie是否成功。 |

**示例：**

```ts
// xxx.ets
import webview from '@ohos.web.webview'
import business_error from '@ohos.base'

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('clearAllCookies')
        .onClick(() => {
          try {
            webview.WebCookieManager.clearAllCookies()
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

###  existCookie<sup>16+</sup>

static existCookie(incognito?: boolean): boolean

获取是否存在cookie。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名                  | 类型    | 必填 | 说明                                                         |
| ----------------------- | ------- | ---- | ------------------------------------------------------------ |
| incognito<sup>11+</sup> | boolean | 否   | true表示隐私模式下查询是否存在cookies，false表示正常非隐私模式下查询是否存在cookies。(Android、iOS不支持incognito) |

**返回值：**

| 类型    | 说明                                        |
| ------- | ------------------------------------------- |
| boolean | true表示存在cookie，false表示不存在cookie。 |

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
      Button('existCookie')
        .onClick(() => {
          let result = webview.WebCookieManager.existCookie();
          console.log("result: " + result);
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### clearSessionCookie<sup>16+</sup>

static clearSessionCookie(callback: AsyncCallback\<void>): void

异步callback方式清除所有会话cookie。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                               |
| -------- |----------------------| ---- | -------------------------------------------------- |
| callback | AsyncCallback\<void> | 是   | callback回调，用于获取清除所有会话cookie是否成功。 |

**错误码：**

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('clearSessionCookie')
        .onClick(() => {
          try {
            webview.WebCookieManager.clearSessionCookie((error) => {
              if (error) {
                console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
              }
            })
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

## WebStorage

通过WebStorage可管理Web SQL数据库接口和HTML5 Web存储接口，每个应用中的所有Web组件共享一个WebStorage。

> **说明：**
>
> 目前调用WebStorage下的方法，都需要先加载Web组件。

### deleteOrigin

static deleteOrigin(origin: string): void

清除指定源所使用的存储。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                     |
| ------ | ------ | ---- | ------------------------ |
| origin | string | 是   | 指定源的字符串索引，来自于[getOrigins](#getorigins)。 |

**错误码：**

| 错误码ID | 错误信息                                               |
| -------- | ------------------------------------------------------ |
| 17100011 | Invalid origin.                             |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  origin: string = "resource://rawfile/";

  build() {
    Column() {
      Button('deleteOrigin')
        .onClick(() => {
          try {
            webview.WebStorage.deleteOrigin(this.origin);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }

        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
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
      <meta name="viewport" content="width=device-width, initial-scale=1.0" charset="utf-8">
      <title>test</title>
      <script type="text/javascript">
          var request = window.indexedDB.open("MyDatabase", 1);
          request.onsuccess = function(event) {
            var db = event.target.result;
            var transaction = db.transaction(["MyObjectStore"], "readwrite");
            var objectStore = transaction.objectStore("MyObjectStore");

            var data = { key: "myKey", value: "myValue" };
            var requestAdd = objectStore.add(data);
          
            requestAdd.onsuccess = function(event) {
                document.querySelector('#status').innerHTML = '<p>数据表已创建,且插入了一条数据。</p>';
            };
            
            requestAdd.onerror = function(event) {
                console.log("Error adding data:", event.target.errorCode);
            };
          };

          request.onerror = function(event) {
            console.log("Database error:", event.target.errorCode);
          };

          request.onupgradeneeded = function(event) {
            var db = event.target.result;
            if (!db.objectStoreNames.contains("MyObjectStore")) {
              var objectStore = db.createObjectStore("MyObjectStore", { keyPath: "key" });
            }
          };
      </script>
  </head>
  <body>
  <div id="status" name="status">状态信息</div>
  </body>
  </html>
 ```

### getOrigins

static getOrigins(callback: AsyncCallback\<Array\<WebStorageOrigin>>): void

以回调方式异步获取当前使用Web SQL数据库的所有源的信息。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                                   | 必填 | 说明                                                   |
| -------- | -------------------------------------- | ---- | ------------------------------------------------------ |
| callback | AsyncCallback\<Array\<[WebStorageOrigin](#webstorageorigin)>> | 是   | 以数组方式返回源的信息，信息内容参考[WebStorageOrigin](#webstorageorigin)。 |

**错误码：**

| 错误码ID | 错误信息                                               |
| -------- | ------------------------------------------------------ |
| 17100012 | Invalid web storage origin.                             |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('getOrigins')
        .onClick(() => {
          try {
            webview.WebStorage.getOrigins((error, origins) => {
              if (error) {
                console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
                return;
              }
              for (let i = 0; i < origins.length; i++) {
                console.log('origin: ' + origins[i].origin);
                console.log('usage: ' + origins[i].usage);
                console.log('quota: ' + origins[i].quota);
              }
            })
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }

        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```

加载的html文件，请参考[deleteOrigin](#deleteorigin)接口下的html文件。

### getOrigins

static getOrigins(): Promise\<Array\<WebStorageOrigin>>

以Promise方式异步获取当前使用Web SQL数据库的所有源的信息。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**返回值：**

| 类型                             | 说明                                                         |
| -------------------------------- | ------------------------------------------------------------ |
| Promise\<Array\<[WebStorageOrigin](#webstorageorigin)>> | Promise实例，用于获取当前所有源的信息，信息内容参考[WebStorageOrigin](#webstorageorigin)。 |

**错误码：**

| 错误码ID | 错误信息                                               |
| -------- | ------------------------------------------------------ |
| 17100012 | Invalid web storage origin.                             |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('getOrigins')
        .onClick(() => {
          try {
            webview.WebStorage.getOrigins()
              .then(origins => {
                for (let i = 0; i < origins.length; i++) {
                  console.log('origin: ' + origins[i].origin);
                  console.log('usage: ' + origins[i].usage);
                  console.log('quota: ' + origins[i].quota);
                }
              })
              .catch((e: BusinessError) => {
                console.log('error: ' + JSON.stringify(e));
              })
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }

        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```

加载的html文件，请参考[deleteOrigin](#deleteorigin)接口下的html文件。

### getOriginQuota

static getOriginQuota(origin: string, callback: AsyncCallback\<number>): void

使用callback回调异步获取指定源的Web SQL数据库的存储配额，配额以字节为单位。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                  | 必填 | 说明               |
| -------- | --------------------- | ---- | ------------------ |
| origin   | string                | 是   | 指定源的字符串索引 |
| callback | AsyncCallback\<number> | 是   | 指定源的存储配额   |

**错误码：**

| 错误码ID | 错误信息                                               |
| -------- | ------------------------------------------------------ |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  origin: string = "resource://rawfile/";

  build() {
    Column() {
      Button('getOriginQuota')
        .onClick(() => {
          try {
            webview.WebStorage.getOriginQuota(this.origin, (error, quota) => {
              if (error) {
                console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
                return;
              }
              console.log('quota: ' + quota);
            })
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }

        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```

加载的html文件，请参考[deleteOrigin](#deleteorigin)接口下的html文件。

### getOriginQuota

static getOriginQuota(origin: string): Promise\<number>

以Promise方式异步获取指定源的Web SQL数据库的存储配额，配额以字节为单位。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| origin | string | 是   | 指定源的字符串索引 |

**返回值：**

| 类型            | 说明                                    |
| --------------- | --------------------------------------- |
| Promise\<number> | Promise实例，用于获取指定源的存储配额。 |

**错误码：**

| 错误码ID | 错误信息                                               |
| -------- | ------------------------------------------------------ |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  origin: string = "resource://rawfile/";

  build() {
    Column() {
      Button('getOriginQuota')
        .onClick(() => {
          try {
            webview.WebStorage.getOriginQuota(this.origin)
              .then(quota => {
                console.log('quota: ' + quota);
              })
              .catch((e: BusinessError) => {
                console.log('error: ' + JSON.stringify(e));
              })
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }

        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```

加载的html文件，请参考[deleteOrigin](#deleteorigin)接口下的html文件。

### getOriginUsage

static getOriginUsage(origin: string, callback: AsyncCallback\<number>): void

以回调方式异步获取指定源的Web SQL数据库的存储量，存储量以字节为单位。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名   | 类型                  | 必填 | 说明               |
| -------- | --------------------- | ---- | ------------------ |
| origin   | string                | 是   | 指定源的字符串索引 |
| callback | AsyncCallback\<number> | 是   | 指定源的存储量。   |

**错误码：**

| 错误码ID | 错误信息                                               |
| -------- | ------------------------------------------------------ |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  origin: string = "resource://rawfile/";

  build() {
    Column() {
      Button('getOriginUsage')
        .onClick(() => {
          try {
            webview.WebStorage.getOriginUsage(this.origin, (error, usage) => {
              if (error) {
                console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
                return;
              }
              console.log('usage: ' + usage);
            })
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }

        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```

加载的html文件，请参考[deleteOrigin](#deleteorigin)接口下的html文件。

### getOriginUsage

static getOriginUsage(origin: string): Promise\<number>

以Promise方式异步获取指定源的Web SQL数据库的存储量，存储量以字节为单位。

iOS不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| origin | string | 是   | 指定源的字符串索引 |

**返回值：**

| 类型            | 说明                                  |
| --------------- | ------------------------------------- |
| Promise\<number> | Promise实例，用于获取指定源的存储量。 |

**错误码：**

| 错误码ID | 错误信息                                              |
| -------- | ----------------------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  origin: string = "resource://rawfile/";

  build() {
    Column() {
      Button('getOriginUsage')
        .onClick(() => {
          try {
            webview.WebStorage.getOriginUsage(this.origin)
              .then(usage => {
                console.log('usage: ' + usage);
              }).catch((e: BusinessError) => {
              console.error('error: ' + JSON.stringify(e));
            })
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```

加载的html文件，请参考[deleteOrigin](#deleteorigin)接口下的html文件。

### deleteAllData

static deleteAllData(incognito?: boolean): void

清除Web SQL数据库当前使用的所有存储。

iOS不支持。
参数incognito不支持。

**系统能力：** SystemCapability.Web.Webview.Core

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();

  build() {
    Column() {
      Button('deleteAllData')
        .onClick(() => {
          try {
            webview.WebStorage.deleteAllData();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: $rawfile('index.html'), controller: this.controller })
    }
  }
}
```

加载的html文件，请参考[deleteOrigin](#deleteorigin)接口下加载的html文件。

## WebMessage

用于描述[WebMessagePort](#webmessageport)所支持的数据类型。

**系统能力：** SystemCapability.Web.Webview.Core

| 类型   | 说明             |
| ------ | ---------------- |
| string | 字符串类型数据。 |

## WebStorageOrigin

提供Web SQL数据库的使用信息。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称   | 类型   | 可读 | 可写 | 说明 |
| ------ | ------ | ---- | ---- | ---- |
| origin | string | 是  | 是 | 指定源的字符串索引。 |
| usage  | number | 是  | 是 | 指定源的存储量。     |
| quota  | number | 是  | 是 | 指定源的存储配额。   |

## BackForwardList

当前Webview的历史信息列表。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称         | 类型   | 可读 | 可写 | 说明                                                         |
| ------------ | ------ | ---- | ---- | ------------------------------------------------------------ |
| currentIndex | number | 是   | 否   | 当前在页面历史列表中的索引。                                 |
| size         | number | 是   | 否   | 历史列表中索引的数量，最多保存50条，超过时起始记录会被覆盖。 |

### getItemAtIndex

getItemAtIndex(index: number): HistoryItem

获取历史列表中指定索引的历史记录项信息。

**系统能力：** SystemCapability.Web.Webview.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                   |
| ------ | ------ | ---- | ---------------------- |
| index  | number | 是   | 指定历史列表中的索引。 |

**返回值：**

| 类型                        | 说明         |
| --------------------------- | ------------ |
| [HistoryItem](#historyitem) | 历史记录项。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import image from "@ohos.multimedia.image";
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  @State icon: image.PixelMap | undefined = undefined;

  build() {
    Column() {
      Button('getBackForwardEntries')
        .onClick(() => {
          try {
            let list = this.controller.getBackForwardEntries();
            let historyItem = list.getItemAtIndex(list.currentIndex);
            console.log("HistoryItem: " + JSON.stringify(historyItem));
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

## HistoryItem

页面历史记录项。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称          | 类型   | 可读 | 可写 | 说明                      |
| ------------- | ------ | ---- | ---- | ------------------------- |
| historyUrl    | string | 是   | 否   | 历史记录项的url地址。     |
| historyRawUrl | string | 是   | 否   | 历史记录项的原始url地址。 |
| title         | string | 是   | 否   | 历史记录项的标题。        |

## WebHeader

Web组件返回的请求/响应头对象。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称        | 类型   | 可读 | 可写 |说明                 |
| ----------- | ------ | -----|------|------------------- |
| headerKey   | string | 是 | 是 | 请求/响应头的key。   |
| headerValue | string | 是 | 是 | 请求/响应头的value。 |

## WebDownloadDelegate<sup>16+</sup>

下载任务的状态会通过该类的回调接口通知给用户。

### onBeforeDownload<sup>16+</sup>

onBeforeDownload(callback: Callback\<WebDownloadItem\>): void

下载开始前通知给用户，用户需要在此接口中调用WebDownloadItem.start("xxx")并提供下载路径，否则下载会一直处于PENDING状态。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android，iOS

**参数：**

| 参数名   | 类型                                            | 必填 | 说明             |
| -------- | ----------------------------------------------- | ---- | ---------------- |
| callback | Callback<[WebDownloadItem](#webdownloaditem16)> | 是   | 触发下载的回调。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### onDownloadUpdated<sup>16+</sup>

onDownloadUpdated(callback: Callback\<WebDownloadItem\>): void

下载过程中的回调，通过该回调的参数可以了解下载进度等信息。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android，iOS

**参数：**

| 参数名   | 类型                                            | 必填 | 说明               |
| -------- | ----------------------------------------------- | ---- | ------------------ |
| callback | Callback<[WebDownloadItem](#webdownloaditem16)> | 是   | 下载的回调已更新。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### onDownloadFinish<sup>16+</sup>

onDownloadFinish(callback: Callback\<WebDownloadItem\>): void

下载完成的通知。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android，iOS

**参数：**

| 参数名   | 类型                                            | 必填 | 说明               |
| -------- | ----------------------------------------------- | ---- | ------------------ |
| callback | Callback<[WebDownloadItem](#webdownloaditem16)> | 是   | 下载的回调已完成。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
     Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### onDownloadFailed<sup>16+</sup>

onDownloadFailed(callback: Callback\<WebDownloadItem\>): void

下载失败的通知。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                            | 必填 | 说明           |
| -------- | ----------------------------------------------- | ---- | -------------- |
| callback | Callback<[WebDownloadItem](#webdownloaditem16)> | 是   | 下载回调失败。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })runJavaScriptExt
    }
  }
}
```

## WebMessageExt<sup>16+</sup>

[webMessagePort](#webmessageport)接口接收、发送的数据对象。

### getType<sup>16+</sup>

getType(): WebMessageType

获取数据对象的类型。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明                                                    |
| ----------------------------------- | ------------------------------------------------------- |
| [WebMessageType](#webmessagetype16) | [webMessagePort](#webmessageport)接口所支持的数据类型。 |

### getString<sup>16+</sup>

getString(): string

获取数据对象的字符串类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                   |
| ------ | ---------------------- |
| string | 返回字符串类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getNumber<sup>16+</sup>

getNumber(): number

获取数据对象的数值类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| number | 返回数值类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getBoolean<sup>16+</sup>

getBoolean(): boolean

获取数据对象的布尔类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型    | 说明                 |
| ------- | -------------------- |
| boolean | 返回布尔类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getArray<sup>16+</sup>

getArray(): Array<string | number | boolean>

获取数据对象的数组类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型                               | 说明                 |
| ---------------------------------- | -------------------- |
| Array<string \| number \| boolean> | 返回数组类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getError<sup>16+</sup>

getError(): Error

获取数据对象的错误类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型  | 说明                     |
| ----- | ------------------------ |
| Error | 返回错误对象类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### setType<sup>16+</sup>

setType(type: WebMessageType): void

设置数据对象的类型。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                                | 必填 | 说明                                                    |
| ------ | ----------------------------------- | ---- | ------------------------------------------------------- |
| type   | [WebMessageType](#webmessagetype16) | 是   | [webMessagePort](#webmessageport)接口所支持的数据类型。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100014 | The type and value of the message do not match.              |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

### setString<sup>16+</sup>

setString(message: string): void

设置数据对象的字符串类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型   | 必填 | 说明             |
| ------- | ------ | ---- | ---------------- |
| message | string | 是   | 字符串类型数据。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100014 | The type and value of the message do not match.              |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

### setNumber<sup>16+</sup>

setNumber(message: number): void

设置数据对象的数值类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**参数：**

| 参数名  | 类型   | 必填 | 说明           |
| ------- | ------ | ---- | -------------- |
| message | number | 是   | 数值类型数据。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100014 | The type and value of the message do not match.              |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

### setBoolean<sup>16+</sup>

setBoolean(message: boolean): void

设置数据对象的布尔类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**参数：**

| 参数名  | 类型    | 必填 | 说明           |
| ------- | ------- | ---- | -------------- |
| message | boolean | 是   | 布尔类型数据。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100014 | The type and value of the message do not match.              |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

### setArray<sup>16+</sup>

setArray(message: Array<string | number | boolean>): void

设置数据对象的数组类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**参数：**

| 参数名  | 类型                               | 必填 | 说明           |
| ------- | ---------------------------------- | ---- | -------------- |
| message | Array<string \| number \| boolean> | 是   | 数组类型数据。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100014 | The type and value of the message do not match.              |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

### setError<sup>16+</sup>

setError(message: Error): void

设置数据对象的错误对象类型数据。完整示例代码参考[onMessageEventExt](#onmessageeventext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**参数：**

| 参数名  | 类型  | 必填 | 说明               |
| ------- | ----- | ---- | ------------------ |
| message | Error | 是   | 错误对象类型数据。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 17100014 | The type and value of the message do not match.              |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

## JsMessageExt<sup>16+</sup>

[runJavaScriptExt](#runjavascriptext16)接口执行脚本返回的数据对象。

### getType<sup>16+</sup>

getType(): JsMessageType

获取数据对象的类型。完整示例代码参考[runJavaScriptExt](#runjavascriptext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                              | 说明                                                         |
| --------------------------------- | ------------------------------------------------------------ |
| [JsMessageType](#jsmessagetype16) | [runJavaScriptExt](#runjavascriptext16)接口脚本执行后返回的结果的类型。 |

###  getString<sup>16+</sup>

getString(): string

获取数据对象的字符串类型数据。完整示例代码参考[runJavaScriptExt](#runjavascriptext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                   |
| ------ | ---------------------- |
| string | 返回字符串类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getNumber<sup>16+</sup>

getNumber(): number

获取数据对象的数值类型数据。完整示例代码参考[runJavaScriptExt](#runjavascriptext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| number | 返回数值类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getBoolean<sup>16+</sup>

getBoolean(): boolean

获取数据对象的布尔类型数据。完整示例代码参考[runJavaScriptExt](#runjavascriptext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型    | 说明                 |
| ------- | -------------------- |
| boolean | 返回布尔类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getArray<sup>16+</sup>

getArray(): Array<string | number | boolean>

获取数据对象的数组类型数据。完整示例代码参考[runJavaScriptExt](#runjavascriptext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型                               | 说明                 |
| ---------------------------------- | -------------------- |
| Array<string \| number \| boolean> | 返回数组类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

### getError<sup>16+</sup>

getError(): Error

获取数据对象的错误类型数据。完整示例代码参考[runJavaScriptExt](#runjavascriptext16)。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**返回值：**

| 类型  | 说明                     |
| ----- | ------------------------ |
| Error | 返回错误对象类型的数据。 |

**错误码：**

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 17100014 | The type and value of the message do not match. |

## JsMessageType<sup>16+</sup>

[runJavaScriptExt](#runjavascriptext16)接口脚本执行后返回的结果的类型。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 值   | 说明         | Android平台 | iOS平台 |
| ------- | ---- | ------------ | ----------- | ------- |
| STRING  | 1    | 字符串类型。 | 支持        | 支持    |
| NUMBER  | 2    | 数值类型。   | 不支持      | 支持    |
| BOOLEAN | 3    | 布尔类型。   | 不支持      | 支持    |
| ARRAY   | 5    | 数组类型。   | 不支持      | 支持    |

## WebMessageType<sup>16+</sup>

[webMessagePort](#webmessageport)接口所支持的数据类型。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称    | 值   | 说明         | Android平台 | iOS平台 |
| ------- | ---- | ------------ | ----------- | ------- |
| STRING  | 1    | 字符串类型。 | 支持        | 支持    |
| NUMBER  | 2    | 数值类型。   | 不支持      | 支持    |
| BOOLEAN | 3    | 布尔类型。   | 不支持      | 支持    |
| ARRAY   | 5    | 数组类型。   | 不支持      | 支持    |
| ERROR   | 6    | 错误类型。   | 不支持      | 支持    |

## WebDownloadItem<sup>16+</sup>

表示下载任务，您可以使用此对象来操作相应的下载任务。

> **说明：**
>
> 在下载过程中，下载的进程会通过WebDownloadDelegate通知给使用者，使用者可以通过参数WebDownloadItem来操作下载任务。

### getGuid<sup>16+</sup>

getGuid(): string

获取下载任务的唯一ID。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明               |
| ------ | ------------------ |
| string | 下载任务的唯一ID。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getCurrentSpeed<sup>16+</sup>

getCurrentSpeed(): number

获取下载的速度，单位：字节每秒。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| number | 下载的速度（字节每秒）。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update current speed: " + webDownloadItem.getCurrentSpeed());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getPercentComplete<sup>16+</sup>

getPercentComplete(): number

获取下载的进度，100代表下载完成。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                              |
| ------ | --------------------------------- |
| number | 下载完成的进度，100代表下载完成。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getTotalBytes<sup>16+</sup>

getTotalBytes(): number

获取待下载文件的总长度。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| number | 待下载文件的总长度。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update total bytes: " + webDownloadItem.getTotalBytes());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getState<sup>16+</sup>

getState(): WebDownloadState

获取下载的状态。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                                    | 说明         |
| --------------------------------------- | ------------ |
| [WebDownloadState](#webdownloadstate16) | 下载的状态。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update download state: " + webDownloadItem.getState());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getLastErrorCode<sup>16+</sup>

getLastErrorCode(): WebDownloadErrorCode

获取下载的错误码。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                                            | 说明                         |
| ----------------------------------------------- | ---------------------------- |
| [WebDownloadErrorCode](#webdownloaderrorcode16) | 下载发生错误的时候的错误码。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
              console.log("download error code: " + webDownloadItem.getLastErrorCode());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getMethod<sup>16+</sup>

getMethod(): string

获取下载任务的请求方式。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| string | 下载的请求方式。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download， method:" + webDownloadItem.getMethod());
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getMimeType<sup>16+</sup>

getMimeType(): string

获取下载的媒体类型（例如，一个声音文件可能被标记为 audio/ogg ，一个图像文件可能是 image/png）。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| string | 下载的媒体类型（例如，一个声音文件可能被标记为 audio/ogg ，一个图像文件可能是 image/png）。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download， mime type:" + webDownloadItem.getMimeType());
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getUrl<sup>16+</sup>

getUrl(): string

获取下载的请求地址。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| string | 下载的请求地址。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download, url:" + webDownloadItem.getUrl());
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getSuggestedFileName<sup>16+</sup>

getSuggestedFileName(): string

获取下载的建议文件名。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明               |
| ------ | ------------------ |
| string | 下载的建议文件名。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download, suggest name:" + webDownloadItem.getSuggestedFileName());
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getReceivedBytes<sup>16+</sup>

getReceivedBytes(): number

获取已经接收的字节数。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明               |
| ------ | ------------------ |
| number | 已经接收的字节数。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              console.log("download update received bytes: " + webDownloadItem.getReceivedBytes());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### getFullPath<sup>16+</sup>

getFullPath(): string

获取下载文件在磁盘上的全路径。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                       |
| ------ | -------------------------- |
| string | 下载文件在磁盘上的全路径。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
              console.log("download finish full path: " + webDownloadItem.getFullPath());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### start<sup>16+</sup>

start(downloadPath: string): void

开始下载到指定目录，参数为下载文件的磁盘存储路径(包含文件名)。

> **说明：**
>
> 该接口应在WebDownloadDelegate的onBeforeDownload回调中使用。若在WebDownloadDelegate的onBeforeDownload中未调用start('xxx')，则下载任务将保持在PENDING状态。处于PENDING状态的下载会将文件下载到临时目录，临时文件会在WebDownloadItem.start指定目标路径后被重命名为目标路径，未下载完成的部分会在WebDownloadItem.start指定目标路径后直接下载到目标路径。

**Android平台AndroidManifest.xml文件中需要添加权限：**

```
android.permission.WRITE_EXTERNAL_STORAGE
android.permission.READ_EXTERNAL_STORAGE
```

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名       | 类型   | 必填 | 说明                         |
| ------------ | ------ | ---- | ---------------------------- |
| downloadPath | string | 是   | 下载文件的路径(包含文件名)。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1. Incorrect parameter types. 2. Parameter verification failed. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### cancel<sup>16+</sup>

cancel(): void

取消一个正在下载的下载任务。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### pause<sup>16+</sup>

pause(): void

暂停一个正在下载的下载任务。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**错误码：**

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 17100019 | The download task is not started yet. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('pause')
        .onClick(() => {
          try {
            this.download.pause();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

### resume<sup>16+</sup>

resume(): void

恢复一个暂停的下载任务。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** iOS

**错误码：**

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 17100016 | The download task is not paused. |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('pause')
        .onClick(() => {
          try {
            this.download.pause();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('resume')
        .onClick(() => {
          try {
            this.download.resume();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

## WebDownloadState<sup>16+</sup>

下载任务的状态。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称        | 值   | 说明                 | Android平台 | iOS平台 |
| ----------- | ---- | -------------------- | ----------- | ------- |
| IN_PROGRESS | 0    | 下载任务正在进行中。 | 支持        | 支持    |
| COMPLETED   | 1    | 下载任务已经完成。   | 支持        | 支持    |
| CANCELED    | 2    | 下载任务已经被取消。 | 支持        | 支持    |
| INTERRUPTED | 3    | 下载任务被中断。     | 支持        | 支持    |
| PENDING     | 4    | 下载任务等待开始。   | 支持        | 支持    |
| PAUSED      | 5    | 下载任务已经被暂停。 | 支持        | 支持    |
| UNKNOWN     | 6    | 下载任务未知状态。   | 支持        | 支持    |

## WebDownloadErrorCode<sup>16+</sup>

下载任务的错误码。

**系统能力：** SystemCapability.Web.Webview.Core

| 名称          | 值   | 说明             | Android平台 | iOS平台 |
| ------------- | ---- | ---------------- | ----------- | ------- |
| USER_CANCELED | 40   | 用户取消了下载。 | 支持        | 支持    |

## WebDownloadManager<sup>16+</sup>

可以通过该类提供的接口来恢复失败的下载任务。

### setDownloadDelegate<sup>16+</sup>

static setDownloadDelegate(delegate: WebDownloadDelegate): void

设置用于接收从WebDownloadManager触发的下载进度的委托。

**系统能力：** SystemCapability.Web.Webview.Core

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                       |
| -------- | --------------------------------------------- | ---- | -------------------------- |
| delegate | [WebDownloadDelegate](#webdownloaddelegate16) | 是   | 用来接收下载进回调的委托。 |

**示例：**

```ts
// xxx.ets
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct WebComponent {
  controller: webview.WebviewController = new webview.WebviewController();
  delegate: webview.WebDownloadDelegate = new webview.WebDownloadDelegate();
  download: webview.WebDownloadItem = new webview.WebDownloadItem();

  build() {
    Column() {
      Button('setDownloadDelegate')
        .onClick(() => {
          try {
            this.delegate.onBeforeDownload((webDownloadItem: webview.WebDownloadItem) => {
              console.log("will start a download.");
              // 传入一个下载路径，并开始下载。
              webDownloadItem.start("/data/storage/el2/base/cache/web/" + webDownloadItem.getSuggestedFileName());
            })
            this.delegate.onDownloadUpdated((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download update percent complete: " + webDownloadItem.getPercentComplete());
              this.download = webDownloadItem;
            })
            this.delegate.onDownloadFailed((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download failed guid: " + webDownloadItem.getGuid());
            })
            this.delegate.onDownloadFinish((webDownloadItem: webview.WebDownloadItem) => {
              console.log("download finish guid: " + webDownloadItem.getGuid());
            })
            this.controller.setDownloadDelegate(this.delegate);
            webview.WebDownloadManager.setDownloadDelegate(this.delegate);
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('startDownload')
        .onClick(() => {
          try {
            this.controller.startDownload('https://www.example.com');
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Button('cancel')
        .onClick(() => {
          try {
            this.download.cancel();
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'www.example.com', controller: this.controller })
    }
  }
}
```

