# @ohos.router (Page Routing)

The **Router** module provides APIs to access pages through URLs. You can use the APIs to navigate to a specified page in an application, replace the current page with another one in an application, and return to the previous page or a specified page.

> **NOTE**
>
> - The initial APIs of this module are supported since API version 8. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>
> - Page routing APIs can be invoked only after page rendering is complete. Do not call these APIs in **onInit** and **onReady** when the page is still in the rendering phase.
>
> - The functionality of this module depends on UI context. This means that the APIs of this module cannot be used where the UI context is unclear. For details, see [UIContext](./js-apis-arkui-UIContext.md#uicontext).
>
> - Since API version 10, you can use the [getRouter](./js-apis-arkui-UIContext.md#getrouter) API in [UIContext](./js-apis-arkui-UIContext.md#uicontext) to obtain the [Router](./js-apis-arkui-UIContext.md#router) object associated with the current UI context.

## Modules to Import

```
import router from '@ohos.router'
```

## router.pushUrl<sup>9+</sup>

pushUrl(options: RouterOptions): Promise&lt;void&gt;

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description       |
| ------- | ------------------------------- | ---- | --------- |
| options | [RouterOptions](#routeroptions) | Yes   | Page routing parameters.|

**Return value**

| Type               | Description       |
| ------------------- | --------- |
| Promise&lt;void&gt; | Promise used to return the result.|

**Error codes**

For details about the error codes, see [Router Error Codes](../errorcodes/errorcode-router.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**Example**

```ts
router.pushUrl({
  url: 'pages/routerpage2',
  params: {
    data1: 'message',
    data2: {
      data3: [123, 456, 789]
    }
  }
})
  .then(() => {
    // success
  })
  .catch(err => {
    console.error(`pushUrl failed, code is ${err.code}, message is ${err.message}`);
  })
```

## router.pushUrl<sup>9+</sup>

pushUrl(options: RouterOptions, callback: AsyncCallback&lt;void&gt;): void

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description       |
| ------- | ------------------------------- | ---- | --------- |
| options | [RouterOptions](#routeroptions) | Yes   | Page routing parameters.|
| callback | AsyncCallback&lt;void&gt;      | Yes  | Callback used to return the result.  |

**Error codes**

For details about the error codes, see [Router Error Codes](../errorcodes/errorcode-router.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**Example**

```ts
router.pushUrl({
  url: 'pages/routerpage2',
  params: {
    data1: 'message',
    data2: {
      data3: [123, 456, 789]
    }
  }
}, (err) => {
  if (err) {
    console.error(`pushUrl failed, code is ${err.code}, message is ${err.message}`);
    return;
  }
  console.info('pushUrl success');
})
```
## router.pushUrl<sup>9+</sup>

pushUrl(options: RouterOptions, mode: RouterMode): Promise&lt;void&gt;

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description        |
| ------- | ------------------------------- | ---- | ---------- |
| options | [RouterOptions](#routeroptions) | Yes   | Page routing parameters. |
| mode    | [RouterMode](#routermode9)      | Yes   | Routing mode.|

**Return value**

| Type               | Description       |
| ------------------- | --------- |
| Promise&lt;void&gt; | Promise used to return the result.|

**Error codes**

For details about the error codes, see [Router Error Codes](../errorcodes/errorcode-router.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**Example**

```ts
router.pushUrl({
  url: 'pages/routerpage2',
  params: {
    data1: 'message',
    data2: {
      data3: [123, 456, 789]
    }
  }
}, router.RouterMode.Standard)
  .then(() => {
    // success
  })
  .catch(err => {
    console.error(`pushUrl failed, code is ${err.code}, message is ${err.message}`);
  })
```

## router.pushUrl<sup>9+</sup>

pushUrl(options: RouterOptions, mode: RouterMode, callback: AsyncCallback&lt;void&gt;): void

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description        |
| ------- | ------------------------------- | ---- | ---------- |
| options | [RouterOptions](#routeroptions) | Yes   | Page routing parameters. |
| mode    | [RouterMode](#routermode9)      | Yes   | Routing mode.|
| callback | AsyncCallback&lt;void&gt;      | Yes  | Callback used to return the result.  |

**Error codes**

For details about the error codes, see [Router Error Codes](../errorcodes/errorcode-router.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**Example**

```ts
router.pushUrl({
  url: 'pages/routerpage2',
  params: {
    data1: 'message',
    data2: {
      data3: [123, 456, 789]
    }
  }
}, router.RouterMode.Standard, (err) => {
  if (err) {
    console.error(`pushUrl failed, code is ${err.code}, message is ${err.message}`);
    return;
  }
  console.info('pushUrl success');
})
```

## router.back

back(options?: RouterOptions ): void

Returns to the previous page or a specified page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name | Type                           | Mandatory| Description                                                        |
| ------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| options | [RouterOptions](#routeroptions) | No  | Description of the page. The **url** parameter indicates the URL of the page to return to. If the specified page does not exist in the page stack, the application does not respond. If no URL is set, the application returns to the previous page, and the page is not rebuilt. The page in the page stack is not reclaimed. It will be reclaimed after being popped up.|

**Example**

```ts
router.back({url:'pages/detail'});    
```

## router.clear

clear(): void

Clears all historical pages in the stack and retains only the current page at the top of the stack.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Example**

```ts
router.clear();    
```

## router.getLength

getLength(): string

Obtains the number of pages in the current stack.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type    | Description                |
| ------ | ------------------ |
| string | Number of pages in the stack. The maximum value is **32**.|

**Example**

```ts
let size = router.getLength();        
console.log('pages stack size = ' + size);    
```

## router.getState

getState(): RouterState

Obtains state information about the current page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type                         | Description     |
| --------------------------- | ------- |
| [RouterState](#routerstate) | Page routing state.|

**Example**

```ts
let page = router.getState();
console.log('current index = ' + page.index);
console.log('current name = ' + page.name);
console.log('current path = ' + page.path);
```

## RouterState

Describes the page routing state.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

| Name | Type  | Mandatory| Description                                                        |
| ----- | ------ | ---- | ------------------------------------------------------------ |
| index | number | Yes  | Index of the current page in the stack. The index starts from 1 from the bottom to the top of the stack.|
| name  | string | No  | Name of the current page, that is, the file name.                          |
| path  | string | Yes  | Path of the current page.                                        |

## router.showAlertBeforeBackPage<sup>9+</sup>

showAlertBeforeBackPage(options: EnableAlertOptions): void

Enables the display of a confirm dialog box before returning to the previous page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                                      | Mandatory  | Description       |
| ------- | ---------------------------------------- | ---- | --------- |
| options | [EnableAlertOptions](#enablealertoptions) | Yes   | Description of the dialog box.|

**Error codes**

For details about the error codes, see [Router Error Codes](../errorcodes/errorcode-router.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |

**Example**

```ts
try {
  router.showAlertBeforeBackPage({
    message: 'Message Info'
  });
} catch(error) {
  console.error(`showAlertBeforeBackPage failed, code is ${error.code}, message is ${error.message}`);
}
```
## EnableAlertOptions

Describes the confirm dialog box.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

| Name     | Type    | Mandatory  | Description      |
| ------- | ------ | ---- | -------- |
| message | string | Yes   | Content displayed in the confirm dialog box.|

## router.hideAlertBeforeBackPage<sup>9+</sup>

hideAlertBeforeBackPage(): void

Disables the display of a confirm dialog box before returning to the previous page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Example**

```ts
router.hideAlertBeforeBackPage();    
```

##  router.getParams

getParams(): Object

Obtains the parameters passed from the page that initiates redirection to the current page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type  | Description                              |
| ------ | ---------------------------------- |
| object | Parameters passed from the page that initiates redirection to the current page.|

**Example**

```
router.getParams();
```

## RouterOptions

Describes the page routing options.

**System capability**: SystemCapability.ArkUI.ArkUI.Lite

| Name  | Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| url    | string | Yes  | URL of the target page, in either of the following formats:<br>- Absolute path of the page. The value is available in the pages list in the **config.json** file, for example:<br>- pages/index/index<br>- pages/detail/detail<br>- Particular path. If the URL is a slash (/), the home page is displayed.|
| params | object | No  | Data that needs to be passed to the target page during redirection. The target page can use **router.getParams()** to obtain the passed parameters, for example, **this.keyValue** (**keyValue** is the value of a key in **params**). In the web-like paradigm, these parameters can be directly used on the target page. If the field specified by **key** already exists on the target page, the passed value of the key will be displayed.|


  > **NOTE**
  > The page routing stack supports a maximum of 32 pages.

## RouterMode<sup>9+</sup>

Enumerates the routing modes.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

| Name    | Description                                                        |
| -------- | ------------------------------------------------------------ |
| Standard | Multi-instance mode. It is the default routing mode.<br>The target page is added to the top of the page stack, regardless of whether a page with the same URL exists in the stack.<br>**NOTE**<br>If the routing mode is not used, the page is redirected to in multi-instance mode.|
| Single   | Singleton mode.<br>If the URL of the target page already exists in the page stack, the page closest to the top of the stack is moved to the top and becomes a new page.<br>If the URL of the target page does not exist in the page stack, the page is redirected to in multi-instance mode.|

## Examples

### JavaScript-based Web-like Development Paradigm

```ts
// Current page
export default {
  pushPage() {
    router.push({
      url: 'pages/detail/detail',
      params: {
        data1: 'message'
      }
    });
  }
}
```
```ts
// detail page
export default {
  onInit() {
    console.info('showData1:' + router.getParams()['data1']);
  }
}
```

### TypeScript-based Declarative Development Paradigm

```ts
// Navigate to the target page through router.pushUrl with the params parameter carried.
import router from '@ohos.router'

@Entry
@Component
struct Index {
  async routePage() {
    let options = {
      url: 'pages/second',
      params: {
        text: 'This is the value on the first page.',
        data: {
          array: [12, 45, 78]
        }
      }
    }
    try {
      await router.pushUrl(options)
    } catch (err) {
      console.info(` fail callback, code: ${err.code}, msg: ${err.msg}`)
    }
  }

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Text('This is the first page.')
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
      Button() {
        Text('next page')
          .fontSize(25)
          .fontWeight(FontWeight.Bold)
      }.type(ButtonType.Capsule)
      .margin({ top: 20 })
      .backgroundColor('#ccc')
      .onClick(() => {
        this.routePage()
      })
    }
    .width('100%')
    .height('100%')
  }
}
```

```ts
// Receive the transferred parameters on the second page.
import router from '@ohos.router'

@Entry
@Component
struct Second {
  private content: string = "This is the second page."
  @State text: string = router.getParams()['text']
  @State data: object = router.getParams()['data']
  @State secondData: string = ''

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Text(`${this.content}`)
        .fontSize(50)
        .fontWeight(FontWeight.Bold)
      Text(this.text)
        .fontSize(30)
        .onClick(() => {
          this.secondData = (this.data['array'][1]).toString()
        })
        .margin({ top: 20 })
      Text(`This is the data passed from the first page: ${this.secondData}`)
        .fontSize(20)
        .margin({ top: 20 })
        .backgroundColor('red')
    }
    .width('100%')
    .height('100%')
  }
}
```
