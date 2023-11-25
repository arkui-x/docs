

# @ohos.web.webview (Webview)

The **@ohos.web.webview** module provides APIs for web control. It can be used with the [<Web\>](../arkui-ts/ts-basic-components-web.md) component, which can be used to display web pages.

> **NOTE**
>
> - The initial APIs of this module are supported since API version 9. Updates will be marked with a superscript to indicate their earliest API version.
>
> - You can preview how the APIs of this module work on a real device. The preview is not yet available in the DevEco Studio Previewer.

## Modules to Import

```ts
import web_webview from '@ohos.web.webview';
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
import web_webview from '@ohos.web.webview';
import business_error from '@ohos.base';

@Entry
@Component
struct WebComponent {
  controller: web_webview.WebviewController = new web_webview.WebviewController();
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

Moves to the previous page based on the history stack. This API is generally used together with **accessBackward**.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Moves to the next page based on the history stack. This API is generally used together with **accessForward**.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web component. |

**Example**

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

Invoked to instruct the **\<Web>** component to refresh the web page.

**System capability**: SystemCapability.Web.Webview.Core

**Error codes**

| ID| Error Message                                                    |
| -------- | ------------------------------------------------------------ |
| 17100001 | Init error. The WebviewController must be associated with a Web compoent. |

**Example**

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

Deletes all HTTP authentication credentials saved in the cache. This API returns the result synchronously.

**System capability**: SystemCapability.Web.Webview.Core

**Example**

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

Obtains the cookie value of a specified URL. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Web.Webview.Core

**Parameters**

| Name  | Type                 | Mandatory| Description                                        |
| -------- | --------------------- | ---- | -------------------------------------------- |
| url      | string                | Yes  | URL of the cookie to obtain. A complete URL is recommended.|
| callback | AsyncCallback<string> | Yes  | Callback used to obtain the result.              |

**Error codes**

| ID| Error Message                |
| -------- | ------------------------ |
| 401      | Invalid input parameter. |
| 17100002 | Invalid url.             |

**Example**

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

Describes the request/response header returned by the **\<Web>** component.

**System capability**: SystemCapability.Web.Webview.Core

| Name       | Type  | Readable| Writable|Description                |
| ----------- | ------ | -----|------|------------------- |
| headerKey   | string | Yes| Yes| Key of the request/response header.  |
| headerValue | string | Yes| Yes| Value of the request/response header.|
