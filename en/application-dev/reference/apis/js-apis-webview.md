# @ohos.web.webview (Webview)

The **@ohos.web.webview** module provides APIs for web control. It can be used with the [<Web\>](../arkui-ts/ts-basic-components-web.md) component, which can be used to display web pages.

> **NOTE**
>
> - The initial APIs of this module are supported since API version 9. Updates will be marked with a superscript to indicate their earliest API version.
>
> - You can preview how the APIs of this module work on a real device. The preview is not yet available in the DevEco Studio Previewer.

## Modules to Import

```ts
import { webview } from '@kit.ArkWeb';
```


## WebviewController

Implements a **WebviewController** to control the behavior of the **\<Web>** component. A **WebviewController** can control only one **\<Web>** component, and the APIs (except static APIs) in the **WebviewController** can be invoked only after it has been bound to the target **\<Web>** component.


### loadUrl

loadUrl(url: string | Resource, headers?: Array\<WebHeader>): void

Loads a specified URL.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name | Type            | Mandatory| Description                 |
| ------- | ---------------- | ---- | :-------------------- |
| url     | string \| Resource | Yes  | URL to load.     |
| headers | Array\<[WebHeader](#webheader)> | No  | Additional HTTP request header of the URL.|

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100003 | Invalid resource path or file type.                          |

**Example**

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
            // The URL to be loaded is of the string type.
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
            // The headers parameter is passed.
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

This example loads local resource files using $rawfile.


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
            // Load a local resource file through $rawfile.
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

Loads specified data.

When loading a local image, you need to obtain the Android or iOS sandbox path and combine it with the relative path of the image.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name    | Type  | Mandatory| Description                                                        |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| data       | string | Yes  | Character string obtained after being Base64 or URL encoded.                   |
| mimeType   | string | Yes  | Media type (MIME).                                          |
| encoding   | string | Yes  | Encoding type, which can be Base64 or URL.                      |
| baseUrl    | string | No  | URL (HTTP/HTTPS/data compliant), which is assigned by the **\<Web>** component to **window.origin**.|
| historyUrl | string | No  | URL used for historical records. If this parameter is not empty, historical records are managed based on this URL. This parameter is invalid when **baseUrl** is left empty. This parameter is not supported on iOS.|

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100002 | Invalid url.                                                 |
| 401      | Invalid input parameter.                                     |

**Example**

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

Obtains the URL of this page.

The return value varies by platform.

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type  | Description               |
| ------ | ------------------- |
| string | URL of the current page.|

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Executes a JavaScript script. This API uses an asynchronous callback to return the script execution result. **runJavaScript** can be invoked only after **loadUrl** is executed. For example, it can be invoked in **onPageEnd**.

This API is supported by Android 4.4, iOS 8.0, and later versions.

If the script does not return a value, **'(null)'** is returned from iOS.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type                  | Mandatory| Description                                                        |
| -------- | ---------------------- | ---- | ------------------------------------------------------------ |
| script   | string                 | Yes  | JavaScript script.                                            |
| callback | AsyncCallback\<string> | Yes  | Callback used to return the result. Returns **null** if the JavaScript script fails to be executed or no value is returned.|

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |
| 401      | Invalid input parameter.                                     |

**Example**

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
      Button ('Start Script')
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

 HTML file to be loaded:

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

Executes a JavaScript script. This API uses a promise to return the script execution result. **runJavaScript** can be invoked only after **loadUrl** is executed. For example, it can be invoked in **onPageEnd**.
This API is supported by Android 4.4, iOS 8.0, and later versions.
If the script does not return a value, **'(null)'** is returned from iOS.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name| Type  | Mandatory| Description            |
| ------ | ------ | ---- | ---------------- |
| script | string | Yes  | JavaScript script.|

**Return value**

| Type            | Description                                               |
| ---------------- | --------------------------------------------------- |
| Promise\<string> | Promise used to return the result if the operation is successful and null otherwise.|

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |
| 401      | Invalid input parameter.                                     |

**Example**

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
      Button ('Start Script')
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

Local resource file:

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

Checks whether going to the previous page can be performed on the current page.

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type   | Description                             |
| ------- | --------------------------------- |
| boolean | Returns **true** if going to the previous page can be performed on the current page; returns **false** otherwise.|

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Checks whether going to the next page can be performed on the current page.

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type   | Description                             |
| ------- | --------------------------------- |
| boolean | Returns **true** if going to the next page can be performed on the current page; returns **false** otherwise.|

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Moves to the previous page based on the history stack. This API is generally used together with **accessBackward**.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Moves to the next page based on the history stack. This API is generally used together with **accessForward**.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Invoked to instruct the **\<Web>** component to refresh the web page.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |

**Example**

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

### getWebId<sup>16+</sup>

getWebId(): number

Obtains the index value of this **Web** component, which can be used for **Web** component management.

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type   | Description                                   |
| ------ | --------------------------------------------- |
| number | Index value of the current **Web** component. |

**Error codes**

| ID       | Error Message                                                |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Scrolls the page down by half the viewport or jumps to the bottom of the page.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name   | Type    | Mandatory | Description                                                  |
| ------ | ------- | --------- | ------------------------------------------------------------ |
| bottom | boolean | Yes       | Whether to jump to the bottom of the page. The value **true** means to jump to the bottom of the page; and **false** means to scroll the page down by half the viewport. |

**Error codes**

| ID       | Error Message                                                |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.                                     |

**Example**

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

Scrolls the page up by half the viewport or jumps to the top of the page.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name | Type    | Mandatory | Description                                                  |
|------| ------- | --------- | ------------------------------------------------------------ |
| top  | boolean | Yes       | Whether to jump to the bottom of the page. The value **true** means to jump to the bottom of the page; and **false** means to scroll the page down by half the viewport. |

**Error codes**

| ID       | Error Message                                                |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.                                     |

**Example**

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

Loads the specified URL with **postData** using the POST method. If **url** is not a network URL, it will be loaded with [loadUrl](#loadurl) instead, and the **postData** parameter will be ignored.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name     | Type        | Mandatory | Description                                                  |
| -------- | ----------- | --------- | ------------------------------------------------------------ |
| url      | string      | Yes       | URL to load.                                                 |
| postData | ArrayBuffer | Yes       | Data to transfer using the POST method. The request must be encoded in "application/x-www-form-urlencoded" format. |

**Error codes**

| ID       | Error Message                                                |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100002 | Invalid url.                                                 |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.                                     |

**Example**

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
            // Convert data to the ArrayBuffer type.
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

### setWebDebuggingAccess<sup>16+</sup>

static setWebDebuggingAccess(webDebuggingAccess: boolean): void

Sets whether to enable web debugging. By default, web debugging is disabled. For details, see

Android： [Debugging Frontend Pages by Using DevTools](./web-debugging-with-devtools-android.md);iOS：[Debugging Frontend Pages by Using DevTools](./web-debugging-with-devtools-ios.md).

NOTE: Enabling web debugging allows users to check and modify the internal status of the web page, which poses security risks. Therefore, you are advised not to enable this function in the officially released version of the app.

iOS 16.4 and above versions are supported.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name               | Type    | Mandatory | Description                           |
| ------------------ | ------- | --------- | ------------------------------------- |
| webDebuggingAccess | boolean | Yes       | Sets whether to enable web debugging. |

**Error codes**

| ID   | Error Message            |
| ---- | ------------------------ |
| 401  | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. |

**Example**

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

### zoomIn<sup>16+</sup>

zoomIn(): void

Zooms in on this web page by 20%.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID       | Error Message                                                |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100004 | Function not enabled.                                     |

**Example**

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

Zooms out of this web page by 20%.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID       | Error Message                                                |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |
| 17100004 | Function not enabled.                                     |

**Example**

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

### getOriginalUrl<sup>16+</sup>

getOriginalUrl(): string

Obtains the original URL of this page.

Risk warning: If you want to obtain the URL for JavaScriptProxy communication API authentication, use getLastJavascriptProxyCallingFrameUrl<sup>12+</sup>.

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type   | Description                                   |
| ------ | --------------------------------------------- |
| string | Original URL of the current page. |

**Error codes**

| ID       | Error Message                                                |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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


## webDatabase

Implements a **WebDataBase** object.

> **NOTE**
>
> You must load the **\<Web>** component before calling the APIs in **WebDataBase**.


### existHttpAuthCredentials

static existHttpAuthCredentials(): boolean

Checks whether any saved HTTP authentication credentials exist. This API returns the result synchronously.  

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type   | Description                                                        |
| ------- | ------------------------------------------------------------ |
| boolean | Whether any saved HTTP authentication credentials exist. Returns **true** if any saved HTTP authentication credentials exist; returns **false** otherwise.|

**Example**

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

Deletes all HTTP authentication credentials saved in the cache. This API returns the result synchronously.

**System capability**: SystemCapability.Web.Webview.Core

**Example**

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

Retrieves HTTP authentication credentials for a given host and realm. This API returns the result synchronously.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name| Type  | Mandatory| Description                        |
| ------ | ------ | ---- | ---------------------------- |
| host   | string | Yes  | Host to which HTTP authentication credentials apply.|
| realm  | string | Yes  | Realm to which HTTP authentication credentials apply.  |

**Return value**

| Type          | Description                                        |
| -------------- | -------------------------------------------- |
| Array\<string> | Returns the array of the matching user names and passwords if the operation is successful; returns an empty array otherwise.|

**Error codes**

| ID| Error Message                |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |

**Example**

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

Saves HTTP authentication credentials for a given host and realm. This API returns the result synchronously.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type  | Mandatory| Description                        |
| -------- | ------ | ---- | ---------------------------- |
| host     | string | Yes  | Host to which HTTP authentication credentials apply.|
| realm    | string | Yes  | Realm to which HTTP authentication credentials apply.  |
| username | string | Yes  | User name.                    |
| password | string | Yes  | Password.                      |

**Error codes**

| ID| Error Message                |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |

**Example**

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

Implements a **WebCookieManager** instance to manage behavior of cookies in **\<Web>** components. All **\<Web>** components in an application share a **WebCookieManager** instance.

> **NOTE**
>
> You must load the **\<Web>** component before calling APIs in **WebCookieManager**.

###  configCookie11+

static configCookie(url: string, value: string, callback: AsyncCallback\<void>): void

Sets the value of a single cookie for a specified URL. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type               | Mandatory| Description                                        |
| -------- | ------------------- | ---- | -------------------------------------------- |
| url      | string              | Yes  | URL of the cookie to set. A complete URL is recommended.|
| value    | string              | Yes  | Cookie value to set.                        |
| callback | AsyncCallback\<void> | Yes  | Callback used to return the result.    |

**Error codes**

| ID| Error Message                |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**Example**

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

Sets the value of a single cookie for a specified URL. This API uses a promise to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name| Type  | Mandatory| Description                                        |
| ------ | ------ | ---- | -------------------------------------------- |
| url    | string | Yes  | URL of the cookie to set. A complete URL is recommended.|
| value  | string | Yes  | Cookie value to set.                        |

**Return value**

| Type           | Description                                                  |
| --------------- | ------------------------------------------------------ |
| Promise\<void> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**Example**

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

Obtains the cookie value of a specified URL. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type                 | Mandatory| Description                                        |
| -------- | --------------------- | ---- | -------------------------------------------- |
| url      | string                | Yes  | URL of the cookie to obtain. A complete URL is recommended.|
| callback | AsyncCallback\<string> | Yes  | Callback used to obtain the result.              |

**Error codes**

| ID| Error Message                |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**Example**

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

Obtains the cookie value of a specified URL. This API uses a promise to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name| Type  | Mandatory| Description                                        |
| ------ | ------ | ---- | -------------------------------------------- |
| url    | string | Yes  | URL of the cookie to obtain. A complete URL is recommended.|

**Return value**

| Type           | Description                                        |
| --------------- | -------------------------------------------- |
| Promise\<string> | Promise used to obtain the result.|

**Error codes**

| ID| Error Message                |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**Example**

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

Clears all cookies. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type               | Mandatory| Description                                          |
| -------- | ------------------- | ---- | ---------------------------------------------- |
| callback | AsyncCallback\<void> | Yes  | Callback used to obtain the result.|

**Example**

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

Clears all cookies. This API uses a promise to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type         | Description                                         |
| ------------- | --------------------------------------------- |
| Promise\<void> | Promise used to obtain the result.|

**Example**

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

Checks whether cookies exist.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name                    | Type    | Mandatory | Description                                                  |
| ----------------------- | ------- | --------- | ------------------------------------------------------------ |
| incognito<sup>11+</sup> | boolean | No        | Whether to check for cookies in incognito mode. The value **true** means to check for cookies in incognito mode, and **false** means the opposite.(Android、iOS do not support incognito.) |

**Return value**

| Type    | Description                                                  |
| ------- | ------------------------------------------------------------ |
| boolean | Whether cookies exist. The value **true** means that cookies exist, and **false** means the opposite. |

**Example**

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

###  clearSessionCookie<sup>16+</sup>

static clearSessionCookie(callback: AsyncCallback\<void>): void

Clears all session cookies. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name     | Type                 | Mandatory | Description                         |
| -------- |----------------------| --------- | ----------------------------------- |
| callback | AsyncCallback\<void> | Yes       | Callback used to return the result. |

**Error codes**

| ID   | Error Message            |
| ---- | ------------------------ |
| 401  | Invalid input parameter. |

**Example**

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

Implements a **WebStorage** object to manage the Web SQL database and HTML5 Web Storage APIs. All **Web** components in an application share a **WebStorage** object.

> **NOTE**
>
> You must load the **Web** component before calling the APIs in **WebStorage**.

### deleteOrigin

static deleteOrigin(origin: string): void

Deletes all data in the specified origin.

iOS: This API is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name| Type  | Mandatory| Description                    |
| ------ | ------ | ---- | ------------------------ |
| origin | string | Yes  | Index of the origin, which is obtained through [getOrigins](#getorigins).|

**Error codes**

| ID| Error Message                                              |
| -------- | ------------------------------------------------------ |
| 17100011 | Invalid origin.                             |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**Example**

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

HTML file to be loaded:
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
                document.querySelector('#status').innerHTML = '<p>Data table created, with one data records inserted.</p>';
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
  <div id="status" name="status">status information </div>
  </body>
  </html>
 ```

### getOrigins

static getOrigins(callback: AsyncCallback\<Array\<WebStorageOrigin>>): void

Obtains information about all origins that are currently using the Web SQL Database. This API uses an asynchronous callback to return the result.

iOS: This API is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type                                  | Mandatory| Description                                                  |
| -------- | -------------------------------------- | ---- | ------------------------------------------------------ |
| callback | AsyncCallback\<Array\<[WebStorageOrigin](#webstorageorigin)>> | Yes  | Callback used to return the information about the origins. For details, see [WebStorageOrigin](#webstorageorigin).|

**Error codes**

| ID| Error Message                                              |
| -------- | ------------------------------------------------------ |
| 17100012 | Invalid web storage origin.                             |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**Example**

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

For details about the HTML file loaded, see the HTML file loaded using the [deleteOrigin](#deleteorigin) API.

### getOrigins

static getOrigins(): Promise\<Array\<WebStorageOrigin>>

Obtains information about all origins that are currently using the Web SQL Database. This API uses a promise to return the result.

iOS: This API is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Return value**

| Type                            | Description                                                        |
| -------------------------------- | ------------------------------------------------------------ |
| Promise\<Array\<[WebStorageOrigin](#webstorageorigin)>> | Promise used to return the information about the origins. For details, see [WebStorageOrigin](#webstorageorigin).|

**Error codes**

| ID| Error Message                                              |
| -------- | ------------------------------------------------------ |
| 17100012 | Invalid web storage origin.                             |

**Example**

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

For details about the HTML file loaded, see the HTML file loaded using the [deleteOrigin](#deleteorigin) API.

### getOriginQuota

static getOriginQuota(origin: string, callback: AsyncCallback\<number>): void

Obtains the storage quota of an origin in the Web SQL Database, in bytes. This API uses an asynchronous callback to return the result.

iOS: This API is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type                 | Mandatory| Description              |
| -------- | --------------------- | ---- | ------------------ |
| origin   | string                | Yes  | Index of the origin.|
| callback | AsyncCallback\<number> | Yes  | Storage quota of the origin.  |

**Error codes**

| ID| Error Message                                              |
| -------- | ------------------------------------------------------ |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**Example**

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

For details about the HTML file loaded, see the HTML file loaded using the [deleteOrigin](#deleteorigin) API.

### getOriginQuota

static getOriginQuota(origin: string): Promise\<number>

Obtains the storage quota of an origin in the Web SQL Database, in bytes. This API uses a promise to return the result.

iOS: This API is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| origin | string | Yes  | Index of the origin.|

**Return value**

| Type           | Description                                   |
| --------------- | --------------------------------------- |
| Promise\<number> | Promise used to return the storage quota of the origin.|

**Error codes**

| ID| Error Message                                              |
| -------- | ------------------------------------------------------ |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**Example**

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

For details about the HTML file loaded, see the HTML file loaded using the [deleteOrigin](#deleteorigin) API.

### getOriginUsage

static getOriginUsage(origin: string, callback: AsyncCallback\<number>): void

Obtains the storage usage of an origin in the Web SQL Database, in bytes. This API uses an asynchronous callback to return the result.

iOS: This API is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type                 | Mandatory| Description              |
| -------- | --------------------- | ---- | ------------------ |
| origin   | string                | Yes  | Index of the origin.|
| callback | AsyncCallback\<number> | Yes  | Storage usage of the origin.  |

**Error codes**

| ID| Error Message                                              |
| -------- | ------------------------------------------------------ |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**Example**

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

For details about the HTML file loaded, see the HTML file loaded using the [deleteOrigin](#deleteorigin) API.

### getOriginUsage

static getOriginUsage(origin: string): Promise\<number>

Obtains the storage usage of an origin in the Web SQL Database, in bytes. This API uses a promise to return the result.

iOS: This API is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| origin | string | Yes  | Index of the origin.|

**Return value**

| Type           | Description                                 |
| --------------- | ------------------------------------- |
| Promise\<number> | Promise used to return the storage usage of the origin.|

**Error codes**

| ID| Error Message                                             |
| -------- | ----------------------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed. |

**Example**

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

For details about the HTML file loaded, see the HTML file loaded using the [deleteOrigin](#deleteorigin) API.

### deleteAllData

static deleteAllData(incognito?: boolean): void

Deletes all data in the Web SQL Database.

iOS: This API is not supported.
incognito: This Parameter is not supported.

**System capability**: SystemCapability.Web.Webview.Core

**Example**

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

For details about the HTML file loaded, see the HTML file loaded using the [deleteOrigin](#deleteorigin) API.

## WebHeader

Describes the request/response header returned by the **\<Web>** component.

**System capability**: SystemCapability.Web.Webview.Core

| Name       | Type  | Readable| Writable|Description                |
| ----------- | ------ | -----|------|------------------- |
| headerKey   | string | Yes| Yes| Key of the request/response header.  |
| headerValue | string | Yes| Yes| Value of the request/response header.|

## WebStorageOrigin

Provides usage information of the Web SQL Database.

**System capability**: SystemCapability.Web.Webview.Core

| Name  | Type  | Readable| Writable| Description|
| ------ | ------ | ---- | ---- | ---- |
| origin | string | Yes | Yes| Index of the origin.|
| usage  | number | Yes | Yes| Storage usage of the origin.    |
| quota  | number | Yes | Yes| Storage quota of the origin.  |