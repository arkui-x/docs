# @ohos.arkui.UIContext (UIContext)

在Stage模型中，WindowStage/Window可以通过loadContent接口加载页面并创建UI的实例，并将页面内容渲染到关联的窗口中，所以UI实例和窗口是一一关联的。一些全局的UI接口是和具体UI实例的执行上下文相关的，在当前接口调用时，通过追溯调用链跟踪到UI的上下文，来确定具体的UI实例。若在非UI页面中或者一些异步回调中调用这类接口，可能无法跟踪到当前UI的上下文，导致接口执行失败。

@ohos.window在API version 10 新增[getUIContext](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/js-apis-window.md#getuicontext10)接口，获取UI上下文实例UIContext对象，使用UIContext对象提供的替代方法，可以直接作用在对应的UI实例上。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 示例效果请以真机运行为准，当前IDE预览器不支持。

## UIContext

以下API需先使用ohos.window中的[getUIContext()](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/js-apis-window.md#getuicontext10)方法获取UIContext实例，再通过此实例调用对应方法。本文中UIContext对象以uiContext表示。

### getUIInspector

getUIInspector(): UIInspector

获取UIInspector对象。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                        | 说明                      |
| --------------------------- | ------------------------- |
| [UIInspector](#uiinspector) | 返回UIInspector实例对象。 |

**示例：**

```ts
uiContext.getUIInspector();
```

### getMediaQuery

getMediaQuery(): MediaQuery

获取MediaQuery对象。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型  | 说明          |
| ----- | ----------------- |
| [MediaQuery](#mediaquery) | 返回MediaQuery实例对象。 |

**示例：**

```ts
uiContext.getMediaQuery();
```

### getRouter

getRouter(): Router

获取Router对象。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型  | 说明          |
| ----- | ----------------- |
| [Router](#router) | 返回Router实例对象。 |

**示例：**

```ts
uiContext.getRouter();
```

### getPromptAction

getPromptAction(): PromptAction

获取PromptAction对象。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型  | 说明          |
| ----- | ----------------- |
| [PromptAction](#promptaction) | 返回PromptAction实例对象。 |

**示例：**

```ts
uiContext.getPromptAction();
```

### getOverlayManager<sup>12+</sup>

getOverlayManager(): OverlayManager

获取OverlayManager对象。

**系统能力：**: SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明                         |
| ----------------------------------- | ---------------------------- |
| [OverlayManager](#overlaymanager12) | 返回OverlayManager实例对象。 |

**示例：**

```ts
uiContext.getOverlayManager();
```

### setOverlayManagerOptions<sup>15+</sup>

setOverlayManagerOptions(options: OverlayManagerOptions): boolean

设置[OverlayManager](#overlaymanager12)参数。用于在使用OverlayManager能力之前先初始化overlayManager的参数，包括是否需要渲染overlay根节点等属性。该方法需要在执行getOverlayManager方法之前执行生效，且该方法只生效一次。

**系统能力：**: SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                              | 必填 | 说明                 |
| ------- | ------------------------------------------------- | ---- | -------------------- |
| options | [OverlayManagerOptions](#overlaymanageroptions15) | 是   | OverlayManager参数。 |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 是否设置成功。<br/>返回true时，设置成功。返回false时，设置失败。 |

**示例：**

```ts
uiContext.setOverlayManagerOptions({ renderRootOverlay: true, enableBackPressedEvent: true });
```

### getOverlayManagerOptions<sup>15+</sup>

getOverlayManagerOptions(): OverlayManagerOptions

用于获取当前[OverlayManager](#overlaymanager12)参数。

**系统能力：**: SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**返回值：**

| 类型                                              | 说明                            |
| ------------------------------------------------- | ------------------------------- |
| [OverlayManagerOptions](#overlaymanageroptions15) | 返回当前OverlayManagerOptions。 |

**示例：**

```ts
uiContext.getOverlayManagerOptions();
```

### animateTo

animateTo(value: AnimateParam, event: () => void): void

提供animateTo接口来指定由于闭包代码导致的状态变化插入过渡动效。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数名            | 类型        |       是否必填     |        描述        |
| ---------------- | ------------ | -------------------- | -------------------- |
| value | [AnimateParam](../arkui-ts/ts-explicit-animation.md#animateparam对象说明) | 是 | 设置动画效果相关参数。 |
| event | () => void | 是 | 指定显示动效的闭包函数，在闭包函数中导致的状态变化系统会自动插入过渡动画。 |

**示例：**

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
            iterations: -1, // 设置-1表示动画无限循环
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

显示警告弹窗组件，可设置文本内容与响应回调。

**参数：**

| 参数名    | 参数类型  | 参数描述 |
| ---- | --------------- | -------- |
| options | [AlertDialogParamWithConfirm](../arkui-ts/ts-methods-alert-dialog-box.md#alertdialogparamwithconfirm对象说明)&nbsp;\|&nbsp;[AlertDialogParamWithButtons](../arkui-ts/ts-methods-alert-dialog-box.md#alertdialogparamwithbuttons对象说明)  | 定义并显示AlertDialog组件。 |

**示例：**

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

定义列表弹窗并弹出。

**ActionSheetOptions参数：**

| 参数名        | 参数类型                    | 必填  | 参数描述                          |
| ---------- | -------------------------- | ------- | ----------------------------- |
| title      | [Resource](../arkui-ts/ts-types.md#resource)&nbsp;\|&nbsp;string | 是     |  弹窗标题。 |
| message    | [Resource](../arkui-ts/ts-types.md#resource)&nbsp;\|&nbsp;string | 是     | 弹窗内容。  |
| autoCancel | boolean                           | 否     | 点击遮障层时，是否关闭弹窗。<br>默认值：true |
| confirm    | {<br/>value:&nbsp;[ResourceStr](../arkui-ts/ts-types.md#resourcestr),<br/>action:&nbsp;()&nbsp;=&gt;&nbsp;void<br/>} | 否  | 确认按钮的文本内容和点击回调。<br>默认值：<br/>value：按钮文本内容。<br/>action:&nbsp;按钮选中时的回调。 |
| cancel     | ()&nbsp;=&gt;&nbsp;void           | 否     | 点击遮障层关闭dialog时的回调。   |
| alignment  | [DialogAlignment](../arkui-ts/ts-methods-alert-dialog-box.md#dialogalignment枚举说明) | 否     |  弹窗在竖直方向上的对齐方式。<br>默认值：DialogAlignment.Bottom |
| offset     | {<br/>dx:&nbsp;[Length](../arkui-ts/ts-types.md#length),<br/>dy:&nbsp;[Length](../arkui-ts/ts-types.md#length)<br/>} | 否      | 弹窗相对alignment所在位置的偏移量。{<br/>dx:&nbsp;0,<br/>dy:&nbsp;0<br/>} |
| sheets     | Array&lt;SheetInfo&gt; | 是       | 设置选项内容，每个选择项支持设置图片、文本和选中的回调。 |

**SheetInfo接口说明：**

| 参数名 | 参数类型                                                     | 必填 | 参数描述          |
| ------ | ------------------------------------------------------------ | ---- | ----------------- |
| title  | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 是   | 选项的文本内容。       |
| icon   | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 否   | 选项的图标，默认无图标显示。     |
| action | ()=&gt;void                                          | 是   | 选项选中的回调。 |

**示例：**

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

定义日期滑动选择器弹窗并弹出。

**DatePickerDialogOptions参数：**

| 参数名 | 参数类型 | 必填 | 默认值 | 参数描述 |
| -------- | -------- | -------- | -------- | -------- |
| start | Date | 否 | Date('1970-1-1') | 设置选择器的起始日期。 |
| end | Date | 否 | Date('2100-12-31') | 设置选择器的结束日期。 |
| selected | Date | 否 | 当前系统日期 | 设置当前选中的日期。 |
| lunar | boolean | 否 | false | 日期是否显示为农历。 |
| showTime | boolean | 否 | false | 是否展示时间项。 |
| useMilitaryTime | boolean | 否 | false | 展示时间是否为24小时制。 |
| disappearTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | - | 设置所有选项中最上和最下两个选项的文本颜色、字号、字体粗细。 |
| textStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | - | 设置所有选项中除了最上、最下及选中项以外的文本颜色、字号、字体粗细。 |
| selectedTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | - | 设置选中项的文本颜色、字号、字体粗细。 |
| onAccept | (value: [DatePickerResult](../arkui-ts/ts-basic-components-datepicker.md#datepickerresult对象说明)) => void | 否 | - | 点击弹窗中的“确定”按钮时触发该回调。 |
| onCancel | () => void | 否 | - | 点击弹窗中的“取消”按钮时触发该回调。 |
| onChange | (value: [DatePickerResult](../arkui-ts/ts-basic-components-datepicker.md#datepickerresult对象说明)) => void | 否 | - | 滑动弹窗中的滑动选择器使当前选中项改变时触发该回调。 |

**示例：**

```ts
let selectedDate: Date = new Date("2010-1-1")
uiContext.showDatePickerDialog({
  start: new Date("2000-1-1"),
  end: new Date("2100-12-31"),
  selected: selectedDate,
  onAccept: (value: DatePickerResult) => {
    // 通过Date的setFullYear方法设置按下确定按钮时的日期，这样当弹窗再次弹出时显示选中的是上一次确定的日期
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

定义时间滑动选择器弹窗并弹出。

**TimePickerDialogOptions参数：**

| 参数名 | 参数类型 | 必填 | 参数描述 |
| -------- | -------- | -------- | -------- |
| selected | Date | 否 | 设置当前选中的时间。<br/>默认值：当前系统时间 |
| useMilitaryTime | boolean | 否 | 展示时间是否为24小时制，默认为12小时制。<br/>默认值：false |
| disappearTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | 设置所有选项中最上和最下两个选项的文本颜色、字号、字体粗细。 |
| textStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | 设置所有选项中除了最上、最下及选中项以外的文本颜色、字号、字体粗细。 |
| selectedTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | 设置选中项的文本颜色、字号、字体粗细。 |
| onAccept | (value: [TimePickerResult](../arkui-ts/ts-basic-components-timepicker.md#timepickerresult对象说明)) => void | 否 | 点击弹窗中的“确定”按钮时触发该回调。 |
| onCancel | () => void | 否 | 点击弹窗中的“取消”按钮时触发该回调。 |
| onChange | (value: [TimePickerResult](../arkui-ts/ts-basic-components-timepicker.md#timepickerresult对象说明)) => void | 否 | 滑动弹窗中的选择器使当前选中时间改变时触发该回调。 |

**示例：**

```ts
let selectTime: Date = new Date('2020-12-25T08:30:00')
uiContext.showTimePickerDialog({
  selected: this.selectTime,
  onAccept: (value: TimePickerResult) => {
    // 设置selectTime为按下确定按钮时的时间，这样当弹窗再次弹出时显示选中的为上一次确定的时间
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

定义文本滑动选择器弹窗并弹出。

**TextPickerDialogOptions参数：**

| 参数名 | 参数类型 | 必填 |  参数描述 |
| -------- | -------- | -------- |  -------- |
| range | string[]&nbsp;\|&nbsp;[Resource](../arkui-ts/ts-types.md#resource) | 是 |  设置文本选择器的选择范围。不可设置为空数组，若设置为空数组，则不弹出弹窗。 |
| selected | number | 否 |  设置选中项的索引值。<br>默认值：0 |
| value       | string           | 否    | 设置选中项的文本内容。当设置了selected参数时，该参数不生效。如果设置的value值不在range范围内，则默认取range第一个元素。|
| defaultPickerItemHeight | number \| string | 否 | 设置选择器中选项的高度。 |
| disappearTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | 设置所有选项中最上和最下两个选项的文本颜色、字号、字体粗细。 |
| textStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | 设置所有选项中除了最上、最下及选中项以外的文本颜色、字号、字体粗细。 |
| selectedTextStyle | [PickerTextStyle](../arkui-ts/ts-basic-components-datepicker.md#pickertextstyle10类型说明) | 否 | 设置选中项的文本颜色、字号、字体粗细。 |
| onAccept | (value: [TextPickerResult](../arkui-ts/ts-methods-textpicker-dialog.md#textpickerresult对象说明)) => void | 否 |  点击弹窗中的“确定”按钮时触发该回调。 |
| onCancel | () => void | 否 | 点击弹窗中的“取消”按钮时触发该回调。 |
| onChange | (value: [TextPickerResult](../arkui-ts/ts-methods-textpicker-dialog.md#textpickerresult对象说明)) => void | 否 |  滑动弹窗中的选择器使当前选中项改变时触发该回调。 |

**示例：**

```ts
let select: number = 2
let fruits: string[] = ['apple1', 'orange2', 'peach3', 'grape4', 'banana5']
uiContext.showTextPickerDialog({
  range: this.fruits,
  selected: this.select,
  onAccept: (value: TextPickerResult) => {
    // 设置select为按下确定按钮时候的选中项index，这样当弹窗再次弹出时显示选中的是上一次确定的选项
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

定义Animator类。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                  | 必填   | 说明      |
| ------- | ----------------------------------- | ---- | ------- |
| options | [AnimatorOptions](./js-apis-animator.md#animatoroptions) | 是    | 定义动画选项。 |

**返回值：**

| 类型                                | 说明            |
| --------------------------------- | ------------- |
| [AnimatorResult](./js-apis-animator.md#animatorresult) | Animator结果接口。 |

**示例：**

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

在当前UI上下文执行传入的回调函数。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                  | 必填   | 说明      |
| ------- | ----------------------------------- | ---- | ------- |
| callback | () => void            | 是    | 回调函数 |

**示例：**

```ts
uiContext.runScopedTask(
  () => {
    console.log('Succeeded in runScopedTask');
  }
);
```

## UIInspector

以下API需先使用UIContext中的[getUIInspector()](#getuiinspector)方法获取到UIInspector对象，再通过该对象调用对应方法。

### createComponentObserver

createComponentObserver(id: string): inspector.ComponentObserver

注册组件布局和绘制完成回调通知。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名 | 类型   | 必填 | 说明         |
| ------ | ------ | ---- | ------------ |
| id     | string | 是   | 指定组件id。 |

**返回值：** 

| 类型                                                         | 说明                                               |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [ComponentObserver](js-apis-arkui-inspector.md#componentobserver) | 组件回调事件监听句柄，用于注册和取消注册监听回调。 |

**示例：**

```ts
import { ComponentUtils, Font, PromptAction, Router, UIInspector, MediaQuery } from '@ohos.arkui.UIContext';
let inspector:UIInspector = uiContext.getUIInspector();
let listener = inspector.createComponentObserver('COMPONENT_ID');
```

## MediaQuery

以下API需先使用UIContext中的[getMediaQuery()](#getmediaquery)方法获取到MediaQuery对象，再通过该对象调用对应方法。

### matchMediaSync

matchMediaSync(condition: string): MediaQueryListener

设置媒体查询的查询条件，并返回对应的监听句柄。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名    | 类型   | 必填 | 说明                                                         |
| --------- | ------ | ---- | ------------------------------------------------------------ |
| condition | string | 是   | 媒体事件的匹配条件，具体可参考[媒体查询语法规则](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/ui/arkts-layout-development-media-query.md#%E8%AF%AD%E6%B3%95%E8%A7%84%E5%88%99)。 |

**返回值：**

| 类型                 | 说明                     |
| ------------------ | ---------------------- |
| MediaQueryListener | 媒体事件监听句柄，用于注册和去注册监听回调。 |

**示例：**

```ts
let mediaquery = uiContext.getMediaQuery();
let listener = mediaquery.matchMediaSync('(orientation: landscape)'); //监听横屏事件
```

## Router

以下API需先使用UIContext中的[getRouter()](#getrouter)方法获取到Router对象，再通过该对象调用对应方法。

### pushUrl

pushUrl(options: RouterOptions): Promise&lt;void&gt;

跳转到应用内的指定页面。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                              | 必填   | 说明        |
| ------- | ------------------------------- | ---- | --------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | 是    | 跳转页面描述信息。 |

**返回值：**

| 类型                | 说明        |
| ------------------- | --------- |
| Promise&lt;void&gt; | 异常返回结果。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-router.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**示例：**

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

跳转到应用内的指定页面。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                              | 必填   | 说明        |
| ------- | ------------------------------- | ---- | --------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | 是    | 跳转页面描述信息。 |
| callback | AsyncCallback&lt;void&gt;      | 是   | 异常响应回调。   |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-router.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**示例：**

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

跳转到应用内的指定页面。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                              | 必填   | 说明         |
| ------- | ------------------------------- | ---- | ---------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | 是    | 跳转页面描述信息。  |
| mode    | [RouterMode](js-apis-router.md#routermode9)      | 是    | 跳转页面使用的模式。 |

**返回值：**

| 类型                | 说明        |
| ------------------- | --------- |
| Promise&lt;void&gt; | 异常返回结果。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-router.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**示例：**

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

跳转到应用内的指定页面。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                              | 必填   | 说明         |
| ------- | ------------------------------- | ---- | ---------- |
| options | [RouterOptions](js-apis-router.md#routeroptions) | 是    | 跳转页面描述信息。  |
| mode    | [RouterMode](js-apis-router.md#routermode9)      | 是    | 跳转页面使用的模式。 |
| callback | AsyncCallback&lt;void&gt;      | 是   | 异常响应回调。   |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-router.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |
| 100002    | if the uri is not exist. |
| 100003    | if the pages are pushed too much. |

**示例：**

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

### replaceUrl<sup>19+</sup>

replaceUrl(options: router.RouterOptions): Promise&lt;void&gt;

用应用内的某个页面替换当前页面，并销毁被替换的页面，通过Promise获取跳转异常的返回的结果。

**支持平台**： Android 、iOS

**参数：**

| 参数名  | 类型                                                    | 必填 | 说明               | Android平台 | iOS平台 |
| ------- | ------------------------------------------------------- | ---- | ------------------ | ----------- | ------- |
| options | [router.RouterOptions](js-apis-router.md#routeroptions) | 是   | 替换页面描述信息。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明           |
| ------------------- | -------------- |
| Promise&lt;void&gt; | 异常返回结果。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](../errorcodes/errorcode-router.md)错误码。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 100001   | The UI execution context is not found. This error code is thrown only in the standard system. |
| 200002   | Uri error. The URI of the page to be used for replacement is incorrect or does not exist. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit'

@Entry
@Component
struct Index {
  async routePage() {
    this.getUIContext().getRouter().replaceUrl({
        url: 'pages/detail',
        params: {
          data1: 'message'
        }
      })
      .then(() => {
        console.info('succeeded')
      })
      .catch((error: BusinessError) => {
        console.error(`pushUrl failed, code is ${error.code}, message is ${error.message}`);
      })
  }

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
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

### replaceUrl<sup>19+<sup>

replaceUrl(options: router.RouterOptions, callback: AsyncCallback&lt;void&gt;): void

用应用内的某个页面替换当前页面，并销毁被替换的页面。

**支持平台**： Android 、iOS

**参数：**

| 参数名   | 类型                                                    | 必填 | 说明               | Android平台 | iOS平台 |
| -------- | ------------------------------------------------------- | ---- | ------------------ | ----------- | ------- |
| options  | [router.RouterOptions](js-apis-router.md#routeroptions) | 是   | 替换页面描述信息。 | 支持        | 支持    |
| callback | AsyncCallback&lt;void&gt;                               | 是   | 异常响应回调。     | 支持        | 支持    |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](../errorcodes/errorcode-router.md)错误码。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 100001   | The UI execution context is not found. This error code is thrown only in the standard system. |
| 200002   | Uri error. The URI of the page to be used for replacement is incorrect or does not exist. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit'

@Entry
@Component
struct Index {
  async routePage() {
    this.getUIContext().getRouter().replaceUrl({
      url: 'pages/detail',
      params: {
        data1: 'message'
      }
    }, (err: Error) => {
      if (err) {
        let message = (err as BusinessError).message;
        let code = (err as BusinessError).code;
        console.error(`replaceUrl failed, code is ${code}, message is ${message}`);
        return;
      }
      console.info('replaceUrl success');
    })
  }

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
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

### replaceUrl<sup>19+<sup>

replaceUrl(options: router.RouterOptions, mode: router.RouterMode): Promise&lt;void&gt;

用应用内的某个页面替换当前页面，并销毁被替换的页面，通过Promise获取跳转异常的返回结果。

**支持平台**： Android 、iOS

**参数：**

| 参数名  | 类型                                                    | 必填 | 说明                 | Android平台 | iOS平台 |
| ------- | ------------------------------------------------------- | ---- | -------------------- | ----------- | ------- |
| options | [router.RouterOptions](js-apis-router.md#routeroptions) | 是   | 替换页面描述信息。   | 支持        | 支持    |
| mode    | [router.RouterMode](js-apis-router.md#routermode9)     | 是   | 跳转页面使用的模式。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明           |
| ------------------- | -------------- |
| Promise&lt;void&gt; | 异常返回结果。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](../errorcodes/errorcode-router.md)错误码。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 100001   | Failed to get the delegate. This error code is thrown only in the standard system. |
| 200002   | Uri error. The URI of the page to be used for replacement is incorrect or does not exist. |

**示例：**

```ts
import { router } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit'

class RouterTmp {
  Standard: router.RouterMode = router.RouterMode.Standard
}

let rtm: RouterTmp = new RouterTmp()

@Entry
@Component
struct Index {
  async routePage() {
    this.getUIContext().getRouter().replaceUrl({
        url: 'pages/detail',
        params: {
          data1: 'message'
        }
      }, rtm.Standard)
      .then(() => {
        console.info('succeeded')
      })
      .catch((error: BusinessError) => {
        console.error(`pushUrl failed, code is ${error.code}, message is ${error.message}`);
      })
  }

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
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

### replaceUrl<sup>19+<sup>

replaceUrl(options: router.RouterOptions, mode: router.RouterMode, callback: AsyncCallback&lt;void&gt;): void

用应用内的某个页面替换当前页面，并销毁被替换的页面。

**支持平台**： Android 、iOS

**参数：**

| 参数名   | 类型                                                    | 必填 | 说明                 | Android平台 | iOS平台 |
| -------- | ------------------------------------------------------- | ---- | -------------------- | ----------- | ------- |
| options  | [router.RouterOptions](js-apis-router.md#routeroptions) | 是   | 替换页面描述信息。   | 支持        | 支持    |
| mode     | [router.RouterMode](js-apis-router.md#routermode9)     | 是   | 跳转页面使用的模式。 | 支持        | 支持    |
| callback | AsyncCallback&lt;void&gt;                               | 是   | 异常响应回调。       | 支持        | 支持    |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](../errorcodes/errorcode-router.md)错误码。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 100001   | The UI execution context is not found. This error code is thrown only in the standard system. |
| 200002   | Uri error. The URI of the page to be used for replacement is incorrect or does not exist. |

**示例：**

```ts
import { router } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

class RouterTmp {
  Standard: router.RouterMode = router.RouterMode.Standard
}

let rtm: RouterTmp = new RouterTmp()

@Entry
@Component
struct Index {
  async routePage() {
    this.getUIContext().getRouter().replaceUrl({
      url: 'pages/detail',
      params: {
        data1: 'message'
      }
    }, rtm.Standard, (err: Error) => {
      if (err) {
        let message = (err as BusinessError).message;
        let code = (err as BusinessError).code;
        console.error(`replaceUrl failed, code is ${code}, message is ${message}`);
        return;
      }
      console.info('replaceUrl success');
    });
  }

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
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

### back

back(options?: RouterOptions ): void

返回上一页面或指定的页面。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名  | 类型                            | 必填 | 说明                                                         |
| ------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| options | [RouterOptions](js-apis-router.md#routeroptions) | 否   | 返回页面描述信息，其中参数url指路由跳转时会返回到指定url的界面，如果页面栈上没有url页面，则不响应该情况。如果url未设置，则返回上一页，页面不会重新构建，页面栈里面的page不会回收，出栈后会被回收。 |

**示例：**

```ts
let router = uiContext.getRouter();
router.back({url:'pages/detail'});    
```

### back<sup>19+<sup>

back(index: number, params?: Object): void;

返回指定的页面。

**支持平台**： Android 、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                   | Android平台 | iOS平台 |
| ------ | ------ | ---- | ---------------------- | ----------- | ------- |
| index  | number | 是   | 跳转目标页面的索引值。 | 支持        | 支持    |
| params | Object | 否   | 页面返回时携带的参数。 | 支持        | 支持    |

**示例：**

```ts
import { Router } from '@kit.ArkUI';

let router: Router = uiContext.getRouter();
router.back(1);
```

```ts
import { Router } from '@kit.ArkUI';

let router: Router = uiContext.getRouter();
router.back(1, {info:'来自Home页'}); //携带参数返回
```

### clear

clear(): void

清空页面栈中的所有历史页面，仅保留当前页面作为栈顶页面。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**示例：**

```ts
let router = uiContext.getRouter();
router.clear();    
```

### getLength

getLength(): string

获取当前在页面栈内的页面数量。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型     | 说明                 |
| ------ | ------------------ |
| string | 页面数量，页面栈支持最大数值是32。 |

**示例：**

```ts
let router = uiContext.getRouter();
let size = router.getLength();        
console.log('pages stack size = ' + size);    
```

### getState

getState(): RouterState

获取当前页面的状态信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                          | 说明      |
| --------------------------- | ------- |
| [RouterState](js-apis-router.md#routerstate) | 页面状态信息。 |

**示例：**

```ts
let router = uiContext.getRouter();
let page = router.getState();
console.log('current index = ' + page.index);
console.log('current name = ' + page.name);
console.log('current path = ' + page.path);
```

### showAlertBeforeBackPage

showAlertBeforeBackPage(options: EnableAlertOptions): void

开启页面返回询问对话框。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                       | 必填   | 说明        |
| ------- | ---------------------------------------- | ---- | --------- |
| options | [EnableAlertOptions](js-apis-router.md#enablealertoptions) | 是    | 文本弹窗信息描述。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.router(页面路由)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-router.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |

**示例：**

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

禁用页面返回询问对话框。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**示例：**

```ts
let router = uiContext.getRouter();
router.hideAlertBeforeBackPage();    
```

### getParams

getParams(): Object

获取发起跳转的页面往当前页传入的参数。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型   | 说明                               |
| ------ | ---------------------------------- |
| object | 发起跳转的页面往当前页传入的参数。 |

**示例：**

```ts
let router = uiContext.getRouter();
router.getParams();
```

## PromptAction

以下API需先使用UIContext中的[getPromptAction()](#getpromptaction)方法获取到PromptAction对象，再通过该对象调用对应方法。

### showToast

showToast(options: ShowToastOptions): void

创建并显示文本提示框。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                    | 必填   | 说明      |
| ------- | ------------------------------------- | ---- | ------- |
| options | [ShowToastOptions](js-apis-promptAction.md#showtoastoptions) | 是    | 文本弹窗选项。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.promptAction(弹窗)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-router.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |

**示例：**

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

创建并显示对话框，对话框响应结果异步返回。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名      | 类型                                       | 必填   | 说明           |
| -------- | ---------------------------------------- | ---- | ------------ |
| options  | [ShowDialogOptions](js-apis-promptAction.md#showdialogoptions)  | 是    | 页面显示对话框信息描述。 |
| callback | AsyncCallback&lt;[ShowDialogSuccessResponse](js-apis-promptAction.md#showdialogsuccessresponse)&gt; | 是    | 对话框响应结果回调。   |

**错误码：**

以下错误码的详细介绍请参见[ohos.promptAction(弹窗)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-promptAction.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |

**示例：**

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

创建并显示对话框，对话框响应后同步返回结果。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                      | 必填   | 说明     |
| ------- | --------------------------------------- | ---- | ------ |
| options | [ShowDialogOptions](js-apis-promptAction.md#showdialogoptions) | 是    | 对话框选项。 |

**返回值：**

| 类型                                       | 说明       |
| ---------------------------------------- | -------- |
| Promise&lt;[ShowDialogSuccessResponse](js-apis-promptAction.md#showdialogsuccessresponse)&gt; | 对话框响应结果。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.promptAction(弹窗)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-promptAction.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |

**示例：**

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

创建并显示操作菜单，菜单响应结果异步返回。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full。

**参数：**

| 参数名      | 类型                                       | 必填   | 说明        |
| -------- | ---------------------------------------- | ---- | --------- |
| options  | [ActionMenuOptions](js-apis-promptAction.md#actionmenuoptions)  | 是    | 操作菜单选项。   |
| callback | AsyncCallback&lt;[ActionMenuSuccessResponse](js-apis-promptAction.md#actionmenusuccessresponse)> | 是    | 菜单响应结果回调。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.promptAction(弹窗)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-promptAction.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |

**示例：**

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

创建并显示操作菜单，菜单响应后同步返回结果。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                                      | 必填   | 说明      |
| ------- | --------------------------------------- | ---- | ------- |
| options | [ActionMenuOptions](js-apis-promptAction.md#actionmenuoptions) | 是    | 操作菜单选项。 |

**返回值：**

| 类型                                       | 说明      |
| ---------------------------------------- | ------- |
| Promise&lt;[ActionMenuSuccessResponse](js-apis-promptAction.md#actionmenusuccessresponse)&gt; | 菜单响应结果。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.promptAction(弹窗)](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/errorcode-promptAction.md)错误码。

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 100001    | if UI execution context not found. |

**示例：**

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

## OverlayManager<sup>12+</sup>

以下API需先使用UIContext中的[getOverlayManager()](#getoverlaymanager12)方法获取到OverlayManager对象，再通过该对象调用对应方法。

> **说明：**
>
> OverlayManager上节点的层级在Page页面层级之上，在Dialog、Popup、Menu、BindSheet、BindContentCover和Toast等之下。
>
> OverlayManager上节点安全区域内外的绘制方式与Page一致，键盘避让方式与Page一致。
>
> 与OverlayManager相关的属性推荐采用AppStorage来进行应用全局存储，以免切换页面后属性值发生变化从而导致业务错误。

### addComponentContent<sup>12+</sup>

addComponentContent(content: ComponentContent, index?: number): void

在OverlayManager上新增指定节点。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型             | 必填 | 说明                                                         |
| ------- | ---------------- | ---- | ------------------------------------------------------------ |
| content | ComponentContent | 是   | 在OverlayManager上新增指定节点上添加此content。 <br>**说明：** <br/> 新增的节点默认处于页面居中，按层级堆叠。 |

| index | number | 否    | 新增节点在OverlayManager上的层级位置。<br>**说明：** <br/> 当index ≥ 0时，若index的值越大，则ComponentContent的层级越高；当多个ComponentContent的index相同时，若ComponentContent添加的时间越晚，则层级越高。<br/> 当index < 0、index = null或index = undefined时，ComponentContent默认添加至最高层。<br/>当同一个ComponentContent被添加多次时，只保留最后一次添加的ComponentContent。<br/>

**示例：**

```ts
import { ComponentContent, OverlayManager, router } from '@kit.ArkUI';

class Params {
  text: string = ""
  offset: Position
  constructor(text: string, offset: Position) {
    this.text = text
    this.offset = offset
  }
}
@Builder
function builderText(params: Params) {
  Column() {
    Text(params.text)
      .fontSize(30)
      .fontWeight(FontWeight.Bold)
  }.offset(params.offset)
}

@Entry
@Component
struct OverlayExample {
  @State message: string = 'ComponentContent';
  private uiContext: UIContext = this.getUIContext()
  private overlayNode: OverlayManager = this.uiContext.getOverlayManager()
  @StorageLink('contentArray') contentArray: ComponentContent<Params>[] = []
  @StorageLink('componentContentIndex') componentContentIndex: number = 0
  @StorageLink('arrayIndex') arrayIndex: number = 0
  @StorageLink("componentOffset") componentOffset: Position = {x: 0, y: 80}

  build() {
    Column() {
      Button("++componentContentIndex: " + this.componentContentIndex).onClick(()=>{
        ++this.componentContentIndex
      })
      Button("--componentContentIndex: " + this.componentContentIndex).onClick(()=>{
        --this.componentContentIndex
      })
      Button("增加ComponentContent" + this.contentArray.length).onClick(()=>{
        let componentContent = new ComponentContent(
          this.uiContext, wrapBuilder<[Params]>(builderText),
          new Params(this.message + (this.contentArray.length), this.componentOffset)
        )
        this.contentArray.push(componentContent)
        this.overlayNode.addComponentContent(componentContent, this.componentContentIndex)
      })
      Button("++arrayIndex: " + this.arrayIndex).onClick(()=>{
        ++this.arrayIndex
      })
      Button("--arrayIndex: " + this.arrayIndex).onClick(()=>{
        --this.arrayIndex
      })
      Button("删除ComponentContent" + this.arrayIndex).onClick(()=>{
        if (this.arrayIndex >= 0 && this.arrayIndex < this.contentArray.length) {
          let componentContent = this.contentArray.splice(this.arrayIndex, 1)
          this.overlayNode.removeComponentContent(componentContent.pop())
        } else {
          console.info("arrayIndex有误")
        }
      })
      Button("显示ComponentContent" + this.arrayIndex).onClick(()=>{
        if (this.arrayIndex >= 0 && this.arrayIndex < this.contentArray.length) {
          let componentContent = this.contentArray[this.arrayIndex]
          this.overlayNode.showComponentContent(componentContent)
        } else {
          console.info("arrayIndex有误")
        }
      })
      Button("隐藏ComponentContent" + this.arrayIndex).onClick(()=>{
        if (this.arrayIndex >= 0 && this.arrayIndex < this.contentArray.length) {
          let componentContent = this.contentArray[this.arrayIndex]
          this.overlayNode.hideComponentContent(componentContent)
        } else {
          console.info("arrayIndex有误")
        }
      })
      Button("显示所有ComponentContent").onClick(()=>{
          this.overlayNode.showAllComponentContents()
      })
      Button("隐藏所有ComponentContent").onClick(()=>{
        this.overlayNode.hideAllComponentContents()
      })

      Button("跳转页面").onClick(()=>{
        router.pushUrl({
          url: 'pages/Second'
        })
      })
    }
    .width('100%')
    .height('100%')
  }
}
```

### addComponentContentWithOrder<sup>18+</sup>

addComponentContentWithOrder(content: ComponentContent, levelOrder?: LevelOrder): void

指定显示顺序创建浮层节点。

支持在浮层节点创建时指定显示的顺序。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型             | 必填 | 说明                                                         |
| ---------- | ---------------- | ---- | ------------------------------------------------------------ |
| content    | ComponentContent | 是   | 在OverlayManager上新增指定节点上添加此content。 <br>**说明：** <br/> 新增的节点默认处于页面居中位置，按层级堆叠。 |
| levelOrder | LevelOrder       | 否   | 新增浮层节点的显示顺序。<br />**说明：**<br />- 默认值：LevelOrder.clamp(0)。 |

**示例：**

该示例通过调用addComponentContentWithOrder接口，展示了指定显示顺序创建浮层节点的功能。

```ts
import { ComponentContent, PromptAction, LevelOrder, UIContext, OverlayManager } from '@kit.ArkUI';

class Params {
  text: string = ""
  offset: Position
  constructor(text: string, offset: Position) {
    this.text = text
    this.offset = offset
  }
}
@Builder
function builderText(params: Params) {
  Column() {
    Text(params.text)
      .fontSize(30)
      .fontWeight(FontWeight.Bold)
  }.offset(params.offset)
}

@Entry
@Component
struct Index {
  @State message: string = '弹窗'
  private ctx: UIContext = this.getUIContext()
  private promptAction: PromptAction = this.ctx.getPromptAction()
  private overlayNode: OverlayManager = this.ctx.getOverlayManager()
  @StorageLink('contentArray') contentArray: ComponentContent<Params>[] = []
  @StorageLink('componentContentIndex') componentContentIndex: number = 0
  @StorageLink('arrayIndex') arrayIndex: number = 0
  @StorageLink("componentOffset") componentOffset: Position = {x: 0, y: 80}

  build() {
    Row() {
      Column({ space: 10 }) {
        Button('OverlayManager下面弹窗')
          .fontSize(20)
          .onClick(() => {
            let componentContent = new ComponentContent(
              this.ctx, wrapBuilder<[Params]>(builderText),
              new Params(this.message + (this.contentArray.length), this.componentOffset)
            )
            this.contentArray.push(componentContent)
            this.overlayNode.addComponentContentWithOrder(componentContent, LevelOrder.clamp(100.1))
            let topOrder: LevelOrder = this.promptAction.getTopOrder();
            if (topOrder !== undefined) {
              console.error('topOrder: ' + topOrder.getOrder());
            }
            let bottomOrder: LevelOrder = this.promptAction.getBottomOrder();
            if (bottomOrder !== undefined) {
              console.error('bottomOrder: ' + bottomOrder.getOrder());
            }
          })
        Button('OverlayManager上面弹窗')
          .fontSize(20)
          .onClick(() => {
            let componentContent = new ComponentContent(
              this.ctx, wrapBuilder<[Params]>(builderText),
              new Params(this.message + (this.contentArray.length), this.componentOffset)
            )
            this.contentArray.push(componentContent)
            this.overlayNode.addComponentContentWithOrder(componentContent, LevelOrder.clamp(100.2))
            let topOrder: LevelOrder = this.promptAction.getTopOrder();
            if (topOrder !== undefined) {
              console.error('topOrder: ' + topOrder.getOrder());
            }
            let bottomOrder: LevelOrder = this.promptAction.getBottomOrder();
            if (bottomOrder !== undefined) {
              console.error('bottomOrder: ' + bottomOrder.getOrder());
            }
          })
      }.width('100%')
    }.height('100%')
  }
}
```

### removeComponentContent<sup>12+</sup>

removeComponentContent(content: ComponentContent): void

在overlay上删除指定节点。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型             | 必填 | 说明                              |
| ------- | ---------------- | ---- | --------------------------------- |
| content | ComponentContent | 是   | 在OverlayManager上删除此content。 |

**示例：**

请参考[addComponentContent示例](#addcomponentcontent12)

### showComponentContent<sup>12+</sup>

showComponentContent(content: ComponentContent): void

在OverlayManager上显示指定节点。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型             | 必填 | 说明                              |
| ------- | ---------------- | ---- | --------------------------------- |
| content | ComponentContent | 是   | 在OverlayManager上显示此content。 |

**示例：**

请参考[addComponentContent示例](#addcomponentcontent12)

### hideComponentContent<sup>12+</sup>

hideComponentContent(content: ComponentContent): void

在OverlayManager上隐藏指定节点。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型             | 必填 | 说明                              |
| ------- | ---------------- | ---- | --------------------------------- |
| content | ComponentContent | 是   | 在OverlayManager上隐藏此content。 |

**示例：**

请参考[addComponentContent示例](#addcomponentcontent12)

### showAllComponentContents<sup>12+</sup>

showAllComponentContents(): void

显示OverlayManager上所有的ComponentContent。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**示例：**

请参考[addComponentContent示例](#addcomponentcontent12)

### hideAllComponentContents<sup>12+</sup>

hideAllComponentContents(): void

隐藏OverlayManager上的所有ComponentContent。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**示例：**

请参考[addComponentContent示例](#addcomponentcontent12)

## OverlayManagerOptions<sup>15+</sup>

初始化[OverlayManager](#overlaymanager12)时所用参数。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                                 | 类型    | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------------------------------ | ------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| renderRootOverlay                    | boolean | 否   | 是否渲染overlay根节点，true表示渲染overlay根节点，false表示不渲染overlay根节点，默认值为true。 | 支持        | 支持    |
| enableBackPressedEvent<sup>20+</sup> | boolean | 否   | 是否支持通过侧滑手势关闭OverlayManager下的ComponentContent，默认值为false。 | 支持        | 不支持  |
