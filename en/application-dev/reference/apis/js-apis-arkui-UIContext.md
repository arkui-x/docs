# @ohos.arkui.UIContext (UIContext)

In the stage model, a window stage or window can use the **loadContent** API to load pages, create a UI instance, and render page content to the associated window. Naturally, UI instances and windows are associated on a one-by-one basis. Some global UI APIs are executed in the context of certain UI instances. When calling these APIs, you must identify the UI context, and consequently UI instance, by tracing the call chain. If these APIs are called on a non-UI page or in some asynchronous callback, the current UI context may fail to be identified, resulting in failure in API execution.

**@ohos.window** adds the **getUIContext** API in API version 10 for obtaining the **UIContext** object of a UI instance. The API provided by the **UIContext** object can be directly applied to the corresponding UI instance.

> **NOTE**
>
> The initial APIs of this module are supported since API version 10. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>
> You can preview how this component looks on a real device. The preview is not yet available in the DevEco Studio Previewer.

## UIContext

In the following API examples, you must first use **getUIContext** in **@ohos.window** to obtain a **UIContext** instance, and then call the APIs using the obtained instance. In this document, the **UIContext** instance is represented by **uiContext**.

### getUIInspector

getUIInspector(): UIInspector

Obtains the **UIInspector** object.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type                        | Description             |
| --------------------------- | ----------------------- |
| [UIInspector](#uiinspector) | **UIInspector** object. |

**Example**

```ts
uiContext.getUIInspector();
```

### getMediaQuery

getMediaQuery(): MediaQuery

Obtains a **MediaQuery** object.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type | Description         |
| ----- | ----------------- |
| [MediaQuery](#mediaquery) | **MediaQuery** object.|

**Example**

```ts
uiContext.getMediaQuery();
```

### getRouter

getRouter(): Router

Obtains a **Router** object.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type | Description         |
| ----- | ----------------- |
| [Router](#router) | **Router** object.|

**Example**

```ts
uiContext.getRouter();
```

### getPromptAction

getPromptAction(): PromptAction

Obtains a **PromptAction** object.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type | Description         |
| ----- | ----------------- |
| [PromptAction](#promptaction) | **PromptAction** object.|

**Example**

```ts
uiContext.getPromptAction();
```

### animateTo

animateTo(value: AnimateParam, event: () => void): void

Applies a transition animation for state changes.

Since API version 9, this API is supported in ArkTS widgets.

**Parameters**

| Name           | Type       |       Mandatory    |        Description       |
| ---------------- | ------------ | -------------------- | -------------------- |
| value | [AnimateParam](../arkui-ts/ts-explicit-animation.md#animateparam) | Yes| Animation settings.|
| event | () => void | Yes| Closure function that displays the dynamic effect. The system automatically inserts the transition animation if the status changes in the closure function.|

**Example**

```ts
// xxx.ets
@Entry
@Component
struct AnimateToExample {
  @State widthSize: number = 250
  @State heightSize: number = 100
  @State rotateAngle: number = 0
  private flag: boolean = true

  build() {
    Column() {
      Button('change size')
        .width(this.widthSize)
        .height(this.heightSize)
        .margin(30)
        .onClick(() => {
          if (this.flag) {
            uiContext.animateTo({
              duration: 2000,
              curve: Curve.EaseOut,
              iterations: 3,
              playMode: PlayMode.Normal,
              onFinish: () => {
                console.info('play end')
              }
            }, () => {
              this.widthSize = 150
              this.heightSize = 60
            })
          } else {
            uiContext.animateTo({}, () => {
              this.widthSize = 250
              this.heightSize = 100
            })
          }
          this.flag = !this.flag
        })
      Button('change rotate angle')
        .margin(50)
        .rotate({ x: 0, y: 0, z: 1, angle: this.rotateAngle })
        .onClick(() => {
          uiContext.animateTo({
            duration: 1200,
            curve: Curve.Friction,
            delay: 500,
            iterations: -1, // The value -1 indicates that the animation is played for an unlimited number of times.
            playMode: PlayMode.Alternate,
            onFinish: () => {
              console.info('play end')
            }
          }, () => {
            this.rotateAngle = 90
          })
        })
    }.width('100%').margin({ top: 5 })
  }
}
```

### showAlertDialog

showAlertDialog(options: AlertDialogParamWithConfirm | AlertDialogParamWithButtons): void

Shows an alert dialog box.

**Parameters**

| Name   | Type | Description|
| ---- | --------------- | -------- |
| options | [AlertDialogParamWithConfirm](../arkui-ts/ts-methods-alert-dialog-box.md#alertdialogparamwithconfirm)&nbsp;\|&nbsp;[AlertDialogParamWithButtons](../arkui-ts/ts-methods-alert-dialog-box.md#alertdialogparamwithbuttons)  | Defines and displays the **\<AlertDialog>** component.|

**Example**

```ts
uiContext.showAlertDialog(
  {
    title: 'title',
    message: 'text',
    autoCancel: true,
    alignment: DialogAlignment.Bottom,
    offset: { dx: 0, dy: -20 },
    gridCount: 3,
    confirm: {
      value: 'button',
      action: () => {
        console.info('Button-clicking callback')
      }
    },
    cancel: () => {
      console.info('Closed callbacks')
    }
  }
)
```

### showActionSheet

showActionSheet(value: ActionSheetOptions): void

Defines and shows the action sheet.

**ActionSheetOptions parameters**

| Name       | Type                   | Mandatory | Description                         |
| ---------- | -------------------------- | ------- | ----------------------------- |
| title      | [Resource](../arkui-ts/ts-types.md#resource)&nbsp;\|&nbsp;string | Yes    |  Title of the dialog box.|
| message    | [Resource](../arkui-ts/ts-types.md#resource)&nbsp;\|&nbsp;string | Yes    | Content of the dialog box. |
| autoCancel | boolean                           | No    | Whether to close the dialog box when the overlay is clicked.<br>Default value: **true**|
| confirm    | {<br>value:&nbsp;[ResourceStr](../arkui-ts/ts-types.md#resourcestr),<br>action:&nbsp;()&nbsp;=&gt;&nbsp;void<br>} | No | Text content of the confirm button and callback upon button clicking.<br>Default value:<br>**value**: button text.<br>**action**: callback upon button clicking.|
| cancel     | ()&nbsp;=&gt;&nbsp;void           | No    | Callback invoked when the dialog box is closed after the overlay is clicked.  |
| alignment  | [DialogAlignment](../arkui-ts/ts-methods-alert-dialog-box.md#dialogalignment) | No    |  Alignment mode of the dialog box in the vertical direction.<br>Default value: **DialogAlignment.Bottom**|
| offset     | {<br>dx:&nbsp;[Length](../arkui-ts/ts-types.md#length),<br>dy:&nbsp;[Length](../arkui-ts/ts-types.md#length)<br>} | No     | Offset of the dialog box relative to the alignment position.{<br>dx:&nbsp;0,<br>dy:&nbsp;0<br>} |
| sheets     | Array&lt;SheetInfo&gt; | Yes      | Options in the dialog box. Each option supports the image, text, and callback.|

**SheetInfo parameters**

| Name| Type                                                    | Mandatory| Description         |
| ------ | ------------------------------------------------------------ | ---- | ----------------- |
| title  | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | Yes  | Text of the option.      |
| icon   | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | No  | Sheet icon. By default, no icon is displayed.    |
| action | ()=&gt;void                                          | Yes  | Callback when the sheet is selected.|

**Example**

```ts
uiContext.showActionSheet({
  title: 'ActionSheet title',
  message: 'message',
  autoCancel: true,
  confirm: {
    value: 'Confirm button',
    action: () => {
      console.log('Get Alert Dialog handled')
    }
  },
  cancel: () => {
    console.log('actionSheet canceled')
  },
  alignment: DialogAlignment.Bottom,
  offset: { dx: 0, dy: -10 },
  sheets: [
    {
      title: 'apples',
      action: () => {
        console.log('apples')
      }
    },
    {
      title: 'bananas',
      action: () => {
        console.log('bananas')
      }
    },
    {
      title: 'pears',
      action: () => {
        console.log('pears')
      }
    }
  ]
})
```

### showDatePickerDialog

showDatePickerDialog(options: DatePickerDialogOptions): void

Shows a date picker dialog box.

**DatePickerDialogOptions parameters**

| Name| Type| Mandatory| Default Value| Description|
| -------- | -------- | -------- | -------- | -------- |
| start | Date | No| Date('1970-1-1') | Start date of the picker.|
| end | Date | No| Date('2100-12-31') | End date of the picker.|
| selected | Date | No| Current system date| Selected date.|
| lunar | boolean | No| false | Whether to display the lunar calendar.|
| showTime | boolean | No| false | Whether to display the time item.|
| useMilitaryTime | boolean | No| false | Whether to display time in 24-hour format.|
| disappearTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| - | Font color, font size, and font width for the top and bottom items.|
| textStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| - | Font color, font size, and font width of all items except the top, bottom, and selected items.|
| selectedTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| - | Font color, font size, and font width of the selected item.|
| onAccept | (value: [DatePickerResult](../arkui-ts/ts-basic-components-datepicker.md#datepickerresult)) => void | No| - | Callback invoked when the OK button in the dialog box is clicked.|
| onCancel | () => void | No| - | Callback invoked when the Cancel button in the dialog box is clicked.|
| onChange | (value: [DatePickerResult](../arkui-ts/ts-basic-components-datepicker.md#datepickerresult)) => void | No| - | Callback invoked when the selected item in the picker changes.|

**Example**

```ts
let selectedDate: Date = new Date("2010-1-1")
uiContext.showDatePickerDialog({
  start: new Date("2000-1-1"),
  end: new Date("2100-12-31"),
  selected: selectedDate,
  onAccept: (value: DatePickerResult) => {
    // Use the setFullYear method to set the date when the OK button is touched. In this way, when the date picker dialog box is displayed again, the selected date is the date last confirmed.
    selectedDate.setFullYear(value.year, value.month, value.day)
    console.info("DatePickerDialog:onAccept()" + JSON.stringify(value))
  },
  onCancel: () => {
    console.info("DatePickerDialog:onCancel()")
  },
  onChange: (value: DatePickerResult) => {
    console.info("DatePickerDialog:onChange()" + JSON.stringify(value))
  }
})
```

### showTimePickerDialog

showTimePickerDialog(options: TimePickerDialogOptions): void

Shows a time picker dialog box.

**TimePickerDialogOptions parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| selected | Date | No| Selected time.<br>Default value: current system time|
| useMilitaryTime | boolean | No| Whether to display time in 24-hour format. The 12-hour format is used by default.<br>Default value: **false**|
| disappearTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| Font color, font size, and font width for the top and bottom items.|
| textStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| Font color, font size, and font width of all items except the top, bottom, and selected items.|
| selectedTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| Font color, font size, and font width of the selected item.|
| onAccept | (value: [TimePickerResult](../arkui-ts/ts-basic-components-timepicker.md#timepickerresult)) => void | No| Callback invoked when the OK button in the dialog box is clicked.|
| onCancel | () => void | No| Callback invoked when the Cancel button in the dialog box is clicked.|
| onChange | (value: [TimePickerResult](../arkui-ts/ts-basic-components-timepicker.md#timepickerresult)) => void | No| Callback invoked when the selected time changes.|

**Example**

```ts
let selectTime: Date = new Date('2020-12-25T08:30:00')
uiContext.showTimePickerDialog({
  selected: this.selectTime,
  onAccept: (value: TimePickerResult) => {
    // Set selectTime to the time when the OK button is clicked. In this way, when the dialog box is displayed again, the selected time is the time when the operation was confirmed last time.
    this.selectTime.setHours(value.hour, value.minute)
    console.info("TimePickerDialog:onAccept()" + JSON.stringify(value))
  },
  onCancel: () => {
    console.info("TimePickerDialog:onCancel()")
  },
  onChange: (value: TimePickerResult) => {
    console.info("TimePickerDialog:onChange()" + JSON.stringify(value))
  }
})
```

### showTextPickerDialog

showTextPickerDialog(options: TextPickerDialogOptions): void

Shows a text picker in the given settings.

**TextPickerDialogOptions parameters**

| Name| Type| Mandatory|  Description|
| -------- | -------- | -------- |  -------- |
| range | string[]&nbsp;\|&nbsp;[Resource](../arkui-ts/ts-types.md#resource) | Yes|  Data selection range of the picker. This parameter cannot be set to an empty array. If set to an empty array, it will not be displayed.|
| selected | number | No|  Index of the selected item.<br>Default value: **0**|
| value       | string           | No   | Text of the selected item. This parameter does not take effect when the **selected** parameter is set. If the value is not within the range, the first item in the range is used instead.|
| defaultPickerItemHeight | number \| string | No| Height of the picker item.|
| disappearTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| Font color, font size, and font width for the top and bottom items.|
| textStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| Font color, font size, and font width of all items except the top, bottom, and selected items.|
| selectedTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10) | No| Font color, font size, and font width of the selected item.|
| onAccept | (value: [TextPickerResult](../arkui-ts/ts-methods-textpicker-dialog.md#textpickerresult)) => void | No|  Callback invoked when the OK button in the dialog box is clicked.|
| onCancel | () => void | No| Callback invoked when the Cancel button in the dialog box is clicked.|
| onChange | (value: [TextPickerResult](../arkui-ts/ts-methods-textpicker-dialog.md#textpickerresult)) => void | No|  Callback invoked when the selected item changes.|

**Example**

```ts
let select: number = 2
let fruits: string[] = ['apple1', 'orange2', 'peach3', 'grape4', 'banana5']
uiContext.showTextPickerDialog({
  range: this.fruits,
  selected: this.select,
  onAccept: (value: TextPickerResult) => {
    // Set select to the index of the item selected when the OK button is touched. In this way, when the text picker dialog box is displayed again, the selected item is the one last confirmed.
    this.select = value.index
    console.info("TextPickerDialog:onAccept()" + JSON.stringify(value))
  },
  onCancel: () => {
    console.info("TextPickerDialog:onCancel()")
  },
  onChange: (value: TextPickerResult) => {
    console.info("TextPickerDialog:onChange()" + JSON.stringify(value))
  }
})
```

### create

create(options: AnimatorOptions): AnimatorResult

Creates an **Animator** object.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                                 | Mandatory  | Description     |
| ------- | ----------------------------------- | ---- | ------- |
| options | [AnimatorOptions](./js-apis-animator.md#animatoroptions) | Yes   | Animator options.|

**Return value**

| Type                               | Description           |
| --------------------------------- | ------------- |
| [AnimatorResult](./js-apis-animator.md#animatorresult) | Animator result.|

**Example**

```ts
let options = {
  duration: 1500,
  easing: "friction",
  delay: 0,
  fill: "forwards",
  direction: "normal",
  iterations: 3,
  begin: 200.0,
  end: 400.0
};
uiContext.createAnimator(options);
```

### runScopedTask

runScopedTask(callback: () => void): void

Executes the specified callback in this UI context.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                                 | Mandatory  | Description     |
| ------- | ----------------------------------- | ---- | ------- |
| callback | () => void            | Yes   | Callback used to return the result.|

**Example**

```ts
uiContext.runScopedTask(
  () => {
    console.log('Succeeded in runScopedTask');
  }
);
```

## UIInspector

In the following API examples, you must first use [getUIInspector()](#getuiinspector) in **UIContext** to obtain a **UIInspector** instance, and then call the APIs using the obtained instance.

### createComponentObserver

createComponentObserver(id: string): inspector.ComponentObserver

Creates an observer for the specified component.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name | Type   | Mandatory | Description   |
| ---- | ------ | --------- | ------------- |
| id   | string | Yes       | Component ID. |

**Return value**

| Type                                                         | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [ComponentObserver](js-apis-arkui-inspector.md#componentobserver) | Component observer, which is used to register and unregister listeners for completion of component layout or drawing. |

**Example**

```ts
import { ComponentUtils, Font, PromptAction, Router, UIInspector, MediaQuery } from '@ohos.arkui.UIContext';
let inspector:UIInspector = uiContext.getUIInspector();
let listener = inspector.createComponentObserver('COMPONENT_ID');
```

## MediaQuery

In the following API examples, you must first use [getMediaQuery()](#getmediaquery) in **UIContext** to obtain a **MediaQuery** instance, and then call the APIs using the obtained instance.

### matchMediaSync

matchMediaSync(condition: string): MediaQueryListener

Sets the media query criteria and returns the corresponding listening handle.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name      | Type    | Mandatory  | Description                                      |
| --------- | ------ | ---- | ---------------------------------------- |
| condition | string | Yes   | Media query condition. |

**Return value**

| Type                | Description                    |
| ------------------ | ---------------------- |
| MediaQueryListener | Listening handle to a media event, which is used to register or deregister the listening callback.|

**Example**

```ts
let mediaquery = uiContext.getMediaQuery();
let listener = mediaquery.matchMediaSync('(orientation: landscape)'); // Listen for landscape events.
```

## Router

In the following API examples, you must first use [getRouter()](#getrouter) in **UIContext** to obtain a **Router** instance, and then call the APIs using the obtained instance.

### pushUrl

pushUrl(options: RouterOptions): Promise&lt;void&gt;

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description       |
| ------- | ------------------------------- | ---- | --------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | Yes   | Page routing parameters.|

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
let router = uiContext.getRouter();
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

### pushUrl

pushUrl(options: RouterOptions, callback: AsyncCallback&lt;void&gt;): void

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description       |
| ------- | ------------------------------- | ---- | --------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | Yes   | Page routing parameters.|
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
let router = uiContext.getRouter();
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

### pushUrl

pushUrl(options: RouterOptions, mode: RouterMode): Promise&lt;void&gt;

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description        |
| ------- | ------------------------------- | ---- | ---------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | Yes   | Page routing parameters. |
| mode    | [RouterMode](js-apis-router.md#routermode9)      | Yes   | Routing mode.|

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
let router = uiContext.getRouter();
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

### pushUrl

pushUrl(options: RouterOptions, mode: RouterMode, callback: AsyncCallback&lt;void&gt;): void

Navigates to a specified page in the application.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                             | Mandatory  | Description        |
| ------- | ------------------------------- | ---- | ---------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | Yes   | Page routing parameters. |
| mode    | [RouterMode](js-apis-router.md#routermode9)      | Yes   | Routing mode.|
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
let router = uiContext.getRouter();
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

### back

back(options?: RouterOptions ): void

Returns to the previous page or a specified page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name | Type                           | Mandatory| Description                                                        |
| ------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| options | [RouterOptions](js-apis-router.md#routeroptions) | No  | Description of the page. The **url** parameter indicates the URL of the page to return to. If the specified page does not exist in the page stack, the application does not respond. If no URL is set, the application returns to the previous page, and the page is not rebuilt. The page in the page stack is not reclaimed. It will be reclaimed after being popped up.|

**Example**

```ts
let router = uiContext.getRouter();
router.back({url:'pages/detail'});    
```

### clear

clear(): void

Clears all historical pages in the stack and retains only the current page at the top of the stack.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Example**

```ts
let router = uiContext.getRouter();
router.clear();    
```

### getLength

getLength(): string

Obtains the number of pages in the current stack.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type    | Description                |
| ------ | ------------------ |
| string | Number of pages in the stack. The maximum value is **32**.|

**Example**

```ts
let router = uiContext.getRouter();
let size = router.getLength();        
console.log('pages stack size = ' + size);    
```

### getState

getState(): RouterState

Obtains state information about the current page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type                         | Description     |
| --------------------------- | ------- |
| [RouterState](js-apis-router.md#routerstate) | Page routing state.|

**Example**

```ts
let router = uiContext.getRouter();
let page = router.getState();
console.log('current index = ' + page.index);
console.log('current name = ' + page.name);
console.log('current path = ' + page.path);
```

### showAlertBeforeBackPage

showAlertBeforeBackPage(options: EnableAlertOptions): void

Enables the display of a confirm dialog box before returning to the previous page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                                      | Mandatory  | Description       |
| ------- | ---------------------------------------- | ---- | --------- |
| options | [EnableAlertOptions](js-apis-router.md#enablealertoptions) | Yes   | Description of the dialog box.|

**Error codes**

For details about the error codes, see [Router Error Codes](../errorcodes/errorcode-router.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |

**Example**

```ts
let router = uiContext.getRouter();
try {
  router.showAlertBeforeBackPage({            
    message: 'Message Info'        
  });
} catch(error) {
  console.error(`showAlertBeforeBackPage failed, code is ${error.code}, message is ${error.message}`);
}
```

### hideAlertBeforeBackPage

hideAlertBeforeBackPage(): void

Disables the display of a confirm dialog box before returning to the previous page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Example**

```ts
let router = uiContext.getRouter();
router.hideAlertBeforeBackPage();    
```

### getParams

getParams(): Object

Obtains the parameters passed from the page that initiates redirection to the current page.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Return value**

| Type  | Description                              |
| ------ | ---------------------------------- |
| object | Parameters passed from the page that initiates redirection to the current page.|

**Example**

```ts
let router = uiContext.getRouter();
router.getParams();
```

## PromptAction

In the following API examples, you must first use [getPromptAction()](#getpromptaction) in **UIContext** to obtain a **PromptAction** instance, and then call the APIs using the obtained instance.

### showToast

showToast(options: ShowToastOptions): void

Shows a toast.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                                   | Mandatory  | Description     |
| ------- | ------------------------------------- | ---- | ------- |
| options | [ShowToastOptions](js-apis-promptAction.md#showtoastoptions) | Yes   | Toast options.|

**Error codes**

For details about the error codes, see [promptAction Error Codes](../errorcodes/errorcode-promptAction.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |

**Example**

```ts
let promptAction = uiContext.getPromptAction();
try {
  promptAction.showToast({            
    message: 'Message Info',
    duration: 2000 
  });
} catch (error) {
  console.error(`showToast args error code is ${error.code}, message is ${error.message}`);
};
```

### showDialog

showDialog(options: ShowDialogOptions, callback: AsyncCallback&lt;ShowDialogSuccessResponse&lt;): void

Shows a dialog box. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name     | Type                                      | Mandatory  | Description          |
| -------- | ---------------------------------------- | ---- | ------------ |
| options  | [ShowDialogOptions](js-apis-promptAction.md#showdialogoptions)  | Yes   | Dialog box options.|
| callback | AsyncCallback&lt;[ShowDialogSuccessResponse](js-apis-promptAction.md#showdialogsuccessresponse)&gt; | Yes   | Callback used to return the dialog box response result.  |

**Error codes**

For details about the error codes, see [promptAction Error Codes](../errorcodes/errorcode-promptAction.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |

**Example**

```ts
let promptAction = uiContext.getPromptAction();
try {
  promptAction.showDialog({
    title: 'showDialog Title Info',
    message: 'Message Info',
    buttons: [
      {
        text: 'button1',
        color: '#000000'
      },
      {
        text: 'button2',
        color: '#000000'
      }
    ]
  }, (err, data) => {
    if (err) {
      console.info('showDialog err: ' + err);
      return;
    }
    console.info('showDialog success callback, click button: ' + data.index);
  });
} catch (error) {
  console.error(`showDialog args error code is ${error.code}, message is ${error.message}`);
};
```

### showDialog

showDialog(options: ShowDialogOptions): Promise&lt;ShowDialogSuccessResponse&gt;

Shows a dialog box. This API uses a promise to return the result synchronously.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                                     | Mandatory  | Description    |
| ------- | --------------------------------------- | ---- | ------ |
| options | [ShowDialogOptions](js-apis-promptAction.md#showdialogoptions) | Yes   | Dialog box options.|

**Return value**

| Type                                      | Description      |
| ---------------------------------------- | -------- |
| Promise&lt;[ShowDialogSuccessResponse](js-apis-promptAction.md#showdialogsuccessresponse)&gt; | Promise used to return the dialog box response result.|

**Error codes**

For details about the error codes, see [promptAction Error Codes](../errorcodes/errorcode-promptAction.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |

**Example**

```ts
let promptAction = uiContext.getPromptAction();
try {
  promptAction.showDialog({
    title: 'Title Info',
    message: 'Message Info',
    buttons: [
      {
        text: 'button1',
        color: '#000000'
      },
      {
        text: 'button2',
        color: '#000000'
      }
    ],
  })
    .then(data => {
      console.info('showDialog success, click button: ' + data.index);
    })
    .catch(err => {
      console.info('showDialog error: ' + err);
    })
} catch (error) {
  console.error(`showDialog args error code is ${error.code}, message is ${error.message}`);
};
```

### showActionMenu

showActionMenu(options: ActionMenuOptions, callback: AsyncCallback&lt;ActionMenuSuccessResponse&gt;):void

Shows an action menu. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name     | Type                                      | Mandatory  | Description       |
| -------- | ---------------------------------------- | ---- | --------- |
| options  | [ActionMenuOptions](js-apis-promptAction.md#actionmenuoptions)  | Yes   | Action menu options.  |
| callback | AsyncCallback&lt;[ActionMenuSuccessResponse](js-apis-promptAction.md#actionmenusuccessresponse)> | Yes   | Callback used to return the action menu response result.|

**Error codes**

For details about the error codes, see [promptAction Error Codes](../errorcodes/errorcode-promptAction.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |

**Example**

```ts
let promptAction = uiContext.getPromptAction();
try {
  promptAction.showActionMenu({
    title: 'Title Info',
    buttons: [
      {
        text: 'item1',
        color: '#666666'
      },
      {
        text: 'item2',
        color: '#000000'
      },
    ]
  }, (err, data) => {
    if (err) {
      console.info('showActionMenu err: ' + err);
      return;
    }
    console.info('showActionMenu success callback, click button: ' + data.index);
  })
} catch (error) {
  console.error(`showActionMenu args error code is ${error.code}, message is ${error.message}`);
};
```

### showActionMenu

showActionMenu(options: ActionMenuOptions): Promise&lt;ActionMenuSuccessResponse&gt;

Shows an action menu. This API uses a promise to return the result synchronously.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                                     | Mandatory  | Description     |
| ------- | --------------------------------------- | ---- | ------- |
| options | [ActionMenuOptions](js-apis-promptAction.md#actionmenuoptions) | Yes   | Action menu options.|

**Return value**

| Type                                      | Description     |
| ---------------------------------------- | ------- |
| Promise&lt;[ActionMenuSuccessResponse](js-apis-promptAction.md#actionmenusuccessresponse)&gt; | Promise used to return the action menu response result.|

**Error codes**

For details about the error codes, see [promptAction Error Codes](../errorcodes/errorcode-promptAction.md).

| ID  | Error Message|
| --------- | ------- |
| 100001    | if UI execution context not found. |

**Example**

```ts
let promptAction = uiContext.getPromptAction();
try {
  promptAction.showActionMenu({
    title: 'showActionMenu Title Info',
    buttons: [
      {
        text: 'item1',
        color: '#666666'
      },
      {
        text: 'item2',
        color: '#000000'
      },
    ]
  })
    .then(data => {
      console.info('showActionMenu success, click button: ' + data.index);
    })
    .catch(err => {
      console.info('showActionMenu error: ' + err);
    })
} catch (error) {
  console.error(`showActionMenu args error code is ${error.code}, message is ${error.message}`);
};
```
