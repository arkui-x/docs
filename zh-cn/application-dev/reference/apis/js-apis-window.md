# @ohos.window (窗口)

窗口提供管理窗口的一些基础能力，包括对当前窗口的创建、销毁、各属性设置，以及对各窗口间的管理调度。

该模块提供以下窗口相关的常用功能：

- [Window](#window)：当前窗口实例，窗口管理器管理的基本单元。
- [WindowStage](#windowstage9)：窗口管理器。管理各个基本窗口单元。

> **说明：**
>
> 本模块首批接口从API version 6开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import window from '@ohos.window';
```

## WindowType<sup>7+</sup>

窗口类型枚举。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称              | 值 | 说明               |
| ----------------- | ------ | ------------------ |
| TYPE_APP          | 0      | 表示应用子窗口。<br>**模型约束：** 此接口仅可在FA模型下使用。 |

## Configuration<sup>9+</sup>

创建子窗口或系统窗口时的参数。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称 | 类型 | 必填 | 说明                                                                          |
| ---------- | -------------------------- | -- |-----------------------------------------------------------------------------|
| name       | string                     | 是 | 窗口名字。                                                                       |
| windowType | [WindowType](#windowtype7) | 是 | 窗口类型。                                                                       |
| ctx        | [BaseContext](js-apis-inner-application-baseContext.md) | 否 | 当前应用上下文信息。不设置，则默认为空。<br>FA模型下不需要使用该参数，即可创建子窗口。<br>Stage模型下需要使用该参数，用于创建系统窗口。 |
| displayId  | number                     | 否 | 当前物理屏幕id。不设置，则默认为-1，该参数应为整数。                                             |
| parentId   | number                     | 否 | 父窗口id。不设置，则默认为-1，该参数应为整数。                                                           |

## Orientation<sup>9+</sup>

窗口显示方向类型枚举。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称                                  | 值   | 说明                          |
| ------------------------------------- | ---- | ----------------------------- |
| UNSPECIFIED                           | 0    | 表示未定义方向模式，由系统判定。 |
| PORTRAIT                              | 1    | 表示竖屏显示模式。             |
| LANDSCAPE                             | 2    | 表示横屏显示模式。   |
| PORTRAIT_INVERTED                     | 3    | 表示反向竖屏显示模式。   |
| LANDSCAPE_INVERTED                    | 4    | 表示反向横屏显示模式。 |

## Rect<sup>7+</sup>

窗口矩形区域。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称   | 类型 | 可读 | 可写 | 说明               |
| ------ | -------- | ---- | ---- | ------------------ |
| left   | number   | 是   | 是   | 矩形区域的左边界，单位为px，该参数为整数。 |
| top    | number   | 是   | 是   | 矩形区域的上边界，单位为px，该参数应为整数。 |
| width  | number   | 是   | 是   | 矩形区域的宽度，单位为px，该参数应为整数。 |
| height | number   | 是   | 是   | 矩形区域的高度，单位为px，该参数应为整数。 |

## Size<sup>7+</sup>

窗口大小。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称   | 类型 | 可读 | 可写 | 说明       |
| ------ | -------- | ---- | ---- | ---------- |
| width  | number   | 是   | 是   | 窗口宽度，单位为px，该参数应为整数。 |
| height | number   | 是   | 是   | 窗口高度，单位为px，该参数应为整数。 |

## WindowProperties

窗口属性。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称                                  | 类型                  | 可读 | 可写 | 说明                                                                                                     |
| ------------------------------------- | ------------------------- | ---- | ---- |--------------------------------------------------------------------------------------------------------|
| windowRect<sup>7+</sup>               | [Rect](#rect7)             | 是   | 是   | 窗口尺寸。                                                                                                  |
| type<sup>7+</sup>                     | [WindowType](#windowtype7) | 是   | 是   | 窗口类型。                                                                                                  |
| brightness                            | number                    | 是   | 是   | 屏幕亮度。该参数为浮点数，可设置的亮度范围为[0.0, 1.0]，其取1.0时表示最大亮度值。如果窗口没有设置亮度值，表示亮度跟随系统，此时获取到的亮度值为-1。                      |
| isKeepScreenOn                        | boolean                   | 是   | 是   | 屏幕是否常亮，默认为false。true表示常亮；false表示不常亮。                                                                   |
## window.createWindow<sup>9+</sup>

createWindow(config: Configuration, callback: AsyncCallback&lt;Window&gt;): void

创建子窗口或者系统窗口，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------------------------------------- | -- | --------------------------------- |
| config   | [Configuration](#configuration9)       | 是 | 创建窗口时的参数。   |
| callback | AsyncCallback&lt;[Window](#window)&gt; | 是 | 回调函数。返回当前创建的窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 1300001 | Repeated operation. |
| 1300006 | This window context is abnormal. |
| 1300008 | The operation is on invalid display. |
| 1300009 | The parent window is invalid. |

**示例：**

```js
let windowClass = null;
let config = {name: "alertWindow", windowType: window.WindowType.TYPE_SYSTEM_ALERT, ctx: this.context};
try {
    window.createWindow(config, (err, data) => {
        if (err.code) {
            console.error('Failed to create the window. Cause: ' + JSON.stringify(err));
            return;
        }
        windowClass = data;
        console.info('Succeeded in creating the window. Data: ' + JSON.stringify(data));
        windowClass.resetSize(500, 1000);
    });
} catch (exception) {
    console.error('Failed to create the window. Cause: ' + JSON.stringify(exception));
}
```

## window.createWindow<sup>9+</sup>

createWindow(config: Configuration): Promise&lt;Window&gt;

创建子窗口或者系统窗口，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| ------ | -------------------------------- | -- | ------------------ |
| config | [Configuration](#configuration9) | 是 | 创建窗口时的参数。 |

**返回值：**

| 类型 | 说明 |
| -------------------------------- | ------------------------------------ |
| Promise&lt;[Window](#window)&gt; | Promise对象。返回当前创建的窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 1300001 | Repeated operation. |
| 1300006 | This window context is abnormal. |
| 1300008 | The operation is on invalid display. |
| 1300009 | The parent window is invalid. |

**示例：**

```js
let windowClass = null;
let config = {name: "alertWindow", windowType: window.WindowType.TYPE_SYSTEM_ALERT, ctx: this.context};
try {
    let promise = window.createWindow(config);
    promise.then((data)=> {
        windowClass = data;
        console.info('Succeeded in creating the window. Data:' + JSON.stringify(data));
    }).catch((err)=>{
        console.error('Failed to create the Window. Cause:' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to create the window. Cause: ' + JSON.stringify(exception));
}
```

## window.findWindow<sup>9+</sup>

findWindow(name: string): Window

查找name所对应的窗口。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明     |
| ------ | ------ | ---- | -------- |
| name   | string | 是   | 窗口id。 |

**返回值：**

| 类型 | 说明 |
| ----------------- | ------------------- |
| [Window](#window) | 当前查找的窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 1300002 | This window state is abnormal. |

**示例：**

```js
let windowClass = null;
try {
    windowClass = window.findWindow('alertWindow');
} catch (exception) {
    console.error('Failed to find the Window. Cause: ' + JSON.stringify(exception));
}
```

## window.getLastWindow<sup>9+</sup>

getLastWindow(ctx: BaseContext, callback: AsyncCallback&lt;Window&gt;): void

获取当前应用内最后显示的窗口，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------------------------------------- | -- | ---------------------------------------- |
| ctx      | [BaseContext](js-apis-inner-application-baseContext.md) | 是 | 当前应用上下文信息。 |
| callback | AsyncCallback&lt;[Window](#window)&gt; | 是 | 回调函数。返回当前应用内最后显示的窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 1300002 | This window state is abnormal.   |
| 1300006 | This window context is abnormal. |

**示例：**

```js
let windowClass = null;
try {
    window.getLastWindow(this.context, (err, data) => {
        if (err.code) {
            console.error('Failed to obtain the top window. Cause: ' + JSON.stringify(err));
            return;
        }
        windowClass = data;
        console.info('Succeeded in obtaining the top window. Data: ' + JSON.stringify(data));
    });
} catch (exception) {
    console.error('Failed to obtain the top window. Cause: ' + JSON.stringify(exception));
}
```

## window.getLastWindow<sup>9+</sup>

getLastWindow(ctx: BaseContext): Promise&lt;Window&gt;

获取当前应用内最后显示的窗口，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| ------ | ----------- | ---- | ------------------------------------------------------------ |
| ctx    | [BaseContext](js-apis-inner-application-baseContext.md) | 是   | 当前应用上下文信息。 |

**返回值：**

| 类型 | 说明 |
| -------------------------------- | ------------------------------------------- |
| Promise&lt;[Window](#window)&gt; | Promise对象。返回当前应用内最后显示的窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 1300002 | This window state is abnormal.   |
| 1300006 | This window context is abnormal. |

**示例：**

```js
let windowClass = null;
try {
    let promise = window.getLastWindow(this.context);
    promise.then((data)=> {
        windowClass = data;
        console.info('Succeeded in obtaining the top window. Data: ' + JSON.stringify(data));
    }).catch((err)=>{
        console.error('Failed to obtain the top window. Cause: ' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to obtain the top window. Cause: ' + JSON.stringify(exception));
}
```

## Window

当前窗口实例，窗口管理器管理的基本单元。

下列API示例中都需先使用[getLastWindow()](#windowgetlastwindow9)、[createWindow()](#windowcreatewindow9)、[findWindow()](#windowfindwindow9)中的任一方法获取到Window实例，再通过此实例调用对应方法。

### showWindow<sup>9+</sup>

showWindow(callback: AsyncCallback&lt;void&gt;): void

显示当前窗口，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | ------------------------- | -- | --------- |
| callback | AsyncCallback&lt;void&gt; | 是 | 回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```js
windowClass.showWindow((err) => {
    if (err.code) {
        console.error('Failed to show the window. Cause: ' + JSON.stringify(err));
        return;
    }
    console.info('Succeeded in showing the window.');
});
```

### showWindow<sup>9+</sup>

showWindow(): Promise&lt;void&gt;

显示当前窗口，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型 | 说明 |
| ------------------- | ----------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```js
let promise = windowClass.showWindow();
promise.then(()=> {
    console.info('Succeeded in showing the window.');
}).catch((err)=>{
    console.error('Failed to show the window. Cause: ' + JSON.stringify(err));
});
```

### destroyWindow<sup>9+</sup>

destroyWindow(callback: AsyncCallback&lt;void&gt;): void

销毁当前窗口，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | ------------------------- | -- | --------- |
| callback | AsyncCallback&lt;void&gt; | 是 | 回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
windowClass.destroyWindow((err) => {
    if (err.code) {
        console.error('Failed to destroy the window. Cause:' + JSON.stringify(err));
        return;
    }
    console.info('Succeeded in destroying the window.');
});
```

### destroyWindow<sup>9+</sup>

destroyWindow(): Promise&lt;void&gt;

销毁当前窗口，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型 | 说明 |
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
let promise = windowClass.destroyWindow();
promise.then(()=> {
    console.info('Succeeded in destroying the window.');
}).catch((err)=>{
    console.error('Failed to destroy the window. Cause: ' + JSON.stringify(err));
});
```

### moveWindowTo<sup>9+</sup>

moveWindowTo(x: number, y: number, callback: AsyncCallback&lt;void&gt;): void

移动窗口位置，使用callback异步回调。

全屏模式窗口不支持该操作。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | ------------------------- | -- | --------------------------------------------- |
| x        | number                    | 是 | 窗口在x轴方向移动的值，值为正表示右移，单位为px，该参数仅支持整数输入。 |
| y        | number                    | 是 | 窗口在y轴方向移动的值，值为正表示下移，单位为px，该参数仅支持整数输入。 |
| callback | AsyncCallback&lt;void&gt; | 是 | 回调函数。                                     |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
try {
    windowClass.moveWindowTo(300, 300, (err)=>{
        if (err.code) {
            console.error('Failed to move the window. Cause:' + JSON.stringify(err));
            return;
        }
        console.info('Succeeded in moving the window.');
    });
} catch (exception) {
    console.error('Failed to move the window. Cause:' + JSON.stringify(exception));
}
```

### moveWindowTo<sup>9+</sup>

moveWindowTo(x: number, y: number): Promise&lt;void&gt;

移动窗口位置，使用Promise异步回调。

全屏模式窗口不支持该操作。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -- | ----- | -- | --------------------------------------------- |
| x | number | 是 | 窗口在x轴方向移动的值，值为正表示右移，单位为px，该参数仅支持整数输入。 |
| y | number | 是 | 窗口在y轴方向移动的值，值为正表示下移，单位为px，该参数仅支持整数输入。 |

**返回值：**

| 类型 | 说明 |
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
try {
    let promise = windowClass.moveWindowTo(300, 300);
    promise.then(()=> {
        console.info('Succeeded in moving the window.');
    }).catch((err)=>{
        console.error('Failed to move the window. Cause: ' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to move the window. Cause:' + JSON.stringify(exception));
}
```

### resize<sup>9+</sup>

resize(width: number, height: number, callback: AsyncCallback&lt;void&gt;): void

改变当前窗口大小，使用callback异步回调。

应用主窗口与子窗口存在大小限制，宽度范围：[320, 2560]，高度范围：[240, 2560]，单位为vp。

系统窗口存在大小限制，宽度范围：[0, 2560]，高度范围：[0, 2560]，单位为vp。

设置的宽度与高度受到此约束限制。

全屏模式窗口不支持该操作。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | ------------------------- | -- | ------------------------ |
| width    | number                    | 是 | 目标窗口的宽度，单位为px，该参数仅支持整数输入。 |
| height   | number                    | 是 | 目标窗口的高度，单位为px，该参数仅支持整数输入。 |
| callback | AsyncCallback&lt;void&gt; | 是 | 回调函数。                |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
try {
    windowClass.resize(500, 1000, (err) => {
        if (err.code) {
            console.error('Failed to change the window size. Cause:' + JSON.stringify(err));
            return;
        }
        console.info('Succeeded in changing the window size.');
    });
} catch (exception) {
    console.error('Failed to change the window size. Cause:' + JSON.stringify(exception));
}
```

### resize<sup>9+</sup>

resize(width: number, height: number): Promise&lt;void&gt;

改变当前窗口大小，使用Promise异步回调。

应用主窗口与子窗口存在大小限制，宽度范围：[320, 2560]，高度范围：[240, 2560]，单位为vp。

系统窗口存在大小限制，宽度范围：[0, 2560]，高度范围：[0, 2560]，单位为vp。

设置的宽度与高度受到此约束限制。

全屏模式窗口不支持该操作。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| ------ | ------ | -- | ------------------------ |
| width  | number | 是 | 目标窗口的宽度，单位为px，该参数仅支持整数输入。 |
| height | number | 是 | 目标窗口的高度，单位为px，该参数仅支持整数输入。 |

**返回值：**

| 类型 | 说明 |
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
try {
    let promise = windowClass.resize(500, 1000);
    promise.then(()=> {
        console.info('Succeeded in changing the window size.');
    }).catch((err)=>{
        console.error('Failed to change the window size. Cause: ' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to change the window size. Cause: ' + JSON.stringify(exception));
}
```

### getWindowProperties<sup>9+</sup>

getWindowProperties(): WindowProperties

获取当前窗口的属性，返回WindowProperties。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型 | 说明 |
| ------------------------------------- | ------------- |
| [WindowProperties](#windowproperties) | 当前窗口属性。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```js
try {
    let properties = windowClass.getWindowProperties();
} catch (exception) {
    console.error('Failed to obtain the window properties. Cause: ' + JSON.stringify(exception));
}
```

### setWindowSystemBarEnable<sup>9+</sup>

setWindowSystemBarEnable(names: Array<'status' | 'navigation'>, callback: AsyncCallback&lt;void&gt;): void

设置窗口全屏模式时导航栏、状态栏的可见模式，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | ---------------------------- | -- | --------- |
| names    | Array<'status'\|'navigation'> | 是 | 设置窗口全屏模式时状态栏和导航栏是否显示。<br>例如，需全部显示，该参数设置为['status',&nbsp;'navigation']；不设置，则默认不显示。 |
| callback | AsyncCallback&lt;void&gt; | 是 | 回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
// 此处以不显示导航栏、状态栏为例
let names = [];
try {
    windowClass.setWindowSystemBarEnable(names, (err) => {
        if (err.code) {
            console.error('Failed to set the system bar to be invisible. Cause:' + JSON.stringify(err));
            return;
        }
        console.info('Succeeded in setting the system bar to be invisible.');
    });
} catch (exception) {
    console.error('Failed to set the system bar to be invisible. Cause:' + JSON.stringify(exception));
}
```

### setWindowSystemBarEnable<sup>9+</sup>

setWindowSystemBarEnable(names: Array<'status' | 'navigation'>): Promise&lt;void&gt;

设置窗口全屏模式时导航栏、状态栏的可见模式，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型  | 必填 | 说明 |
| ----- | ---------------------------- | -- | --------------------------------- |
| names | Array<'status'\|'navigation'> | 是 | 设置窗口全屏模式时状态栏和导航栏是否显示。<br>例如，需全部显示，该参数设置为['status',&nbsp;'navigation']；不设置，则默认不显示。 |

**返回值：**

| 类型 | 说明 |
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
// 此处以不显示导航栏、状态栏为例
let names = [];
try {
    let promise = windowClass.setWindowSystemBarEnable(names);
    promise.then(()=> {
        console.info('Succeeded in setting the system bar to be invisible.');
    }).catch((err)=>{
        console.error('Failed to set the system bar to be invisible. Cause:' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to set the system bar to be invisible. Cause:' + JSON.stringify(exception));
}
```

### setPreferredOrientation<sup>9+</sup>

setPreferredOrientation(orientation: Orientation, callback: AsyncCallback&lt;void&gt;): void

设置窗口的显示方向属性，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名              | 类型                                        | 必填 | 说明                   |
| ------------------- | ------------------------------------------- | ---- | ---------------------- |
| Orientation         | [Orientation](#orientation9)                | 是   | 窗口显示方向的属性。         |
| callback            | AsyncCallback&lt;void&gt;                   | 是   | 回调函数。             |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```js
let orientation = window.Orientation.AUTO_ROTATION;
try {
    windowClass.setPreferredOrientation(orientation, (err) => {
        if (err.code) {
            console.error('Failed to set window orientation. Cause: ' + JSON.stringify(err));
            return;
        }
        console.info('Succeeded in setting window orientation.');
    });
} catch (exception) {
    console.error('Failed to set window orientation. Cause: ' + JSON.stringify(exception));
}
```

### setPreferredOrientation<sup>9+</sup>

setPreferredOrientation(orientation: Orientation): Promise&lt;void&gt;

设置窗口的显示方向属性，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名              | 类型                                        | 必填 | 说明                   |
| ------------------- | ------------------------------------------- | ---- | ---------------------- |
| Orientation         | [Orientation](#orientation9)                | 是   | 窗口显示方向的属性。       |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```js
let orientation = window.Orientation.AUTO_ROTATION;
try {
    let promise = windowClass.setPreferredOrientation(orientation);
    promise.then(()=> {
        console.info('Succeeded in setting the window orientation.');
    }).catch((err)=>{
        console.error('Failed to set the window orientation. Cause: ' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to set window orientation. Cause: ' + JSON.stringify(exception));
}
```

### getUIContext<sup>10+</sup>

getUIContext(): UIContext

获取UIContext实例。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型       | 说明                   |
| ---------- | ---------------------- |
| [UIContext](./js-apis-arkui-UIContext.md#uicontext) | 返回UIContext实例对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        // 为主窗口加载对应的目标页面。
        windowStage.loadContent("pages/page2", (err) => {
            if (err.code) {
                console.error('Failed to load the content. Cause:' + JSON.stringify(err));
                return;
            }
            console.info('Succeeded in loading the content.');
        });
        // 获取应用主窗口。
        let windowClass = null;
        windowStage.getMainWindow((err, data) => {
            if (err.code) {
                console.error('Failed to obtain the main window. Cause: ' + JSON.stringify(err));
                return;
            }
            windowClass = data;
            console.info('Succeeded in obtaining the main window. Data: ' + JSON.stringify(data));
            // 获取UIContext实例。
            globalThis.uiContext = windowClass.getUIContext();
        })
    }
};
```

### setUIContent<sup>9+</sup>

setUIContent(path: string, callback: AsyncCallback&lt;void&gt;): void

为当前窗口加载具体页面内容，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | ------------------------- | -- | -------------------- |
| path     | string                    | 是 | 设置加载页面的路径。 |
| callback | AsyncCallback&lt;void&gt; | 是 | 回调函数。          |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
try {
    windowClass.setUIContent('pages/page2/page2', (err) => {
    if (err.code) {
            console.error('Failed to load the content. Cause:' + JSON.stringify(err));
            return;
    }
    console.info('Succeeded in loading the content.');
    });
} catch (exception) {
    console.error('Failed to load the content. Cause:' + JSON.stringify(exception));
}
```

### setUIContent<sup>9+</sup>

setUIContent(path: string): Promise&lt;void&gt;

为当前窗口加载具体页面内容，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| ---- | ------ | -- | ------------------ |
| path | string | 是 | 设置加载页面的路径。 |

**返回值：**

| 类型 | 说明 |
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
try {
    let promise = windowClass.setUIContent('pages/page2/page2');
    promise.then(()=> {
        console.info('Succeeded in loading the content.');
    }).catch((err)=>{
        console.error('Failed to load the content. Cause: ' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to load the content. Cause: ' + JSON.stringify(exception));
}
```

### loadContent<sup>9+</sup>

loadContent(path: string, storage: LocalStorage, callback: AsyncCallback&lt;void&gt;): void

为当前窗口加载与LocalStorage相关联的具体页面内容，使用callback异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                                            | 必填 | 说明                                                         |
| -------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                                          | 是   | 设置加载页面的路径。                                         |
| storage  | [LocalStorage](../../quick-start/arkts-localstorage.md) | 是   | 存储单元，为应用程序范围内的可变状态属性和非可变状态属性提供存储。 |
| callback | AsyncCallback&lt;void&gt;                       | 是   | 回调函数。                                                   |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```ts
let storage = new LocalStorage();
storage.setOrCreate('storageSimpleProp',121);
console.log('onWindowStageCreate');
try {
    windowClass.loadContent('pages/page2', storage, (err) => {
        if (err.code) {
            console.error('Failed to load the content. Cause:' + JSON.stringify(err));
            return;
        }
        console.info('Succeeded in loading the content.');
    });
} catch (exception) {
    console.error('Failed to load the content. Cause:' + JSON.stringify(exception));
}
```

### loadContent<sup>9+</sup>

loadContent(path: string, storage: LocalStorage): Promise&lt;void&gt;

为当前窗口加载与LocalStorage相关联的具体页面内容，使用Promise异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名  | 类型                                            | 必填 | 说明                                                         |
| ------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path    | string                                          | 是   | 设置加载页面的路径。                                         |
| storage | [LocalStorage](../../quick-start/arkts-localstorage.md) | 是   | 存储单元，为应用程序范围内的可变状态属性和非可变状态属性提供存储。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```ts
let storage = new LocalStorage();
storage.setOrCreate('storageSimpleProp',121);
console.log('onWindowStageCreate');
try {
    let promise = windowClass.loadContent('pages/page2', storage);
    promise.then(() => {
        console.info('Succeeded in loading the content.');
    }).catch((err) => {
        console.error('Failed to load the content. Cause:' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to load the content. Cause:' + JSON.stringify(exception));
}
```

### isWindowShowing<sup>9+</sup>

isWindowShowing(): boolean

判断当前窗口是否已显示。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型 | 说明 |
| ------- | ------------------------------------------------------------------ |
| boolean | 当前窗口是否已显示。true表示当前窗口已显示，false则表示当前窗口未显示。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```js
try {
    let data = windowClass.isWindowShowing();
    console.info('Succeeded in checking whether the window is showing. Data: ' + JSON.stringify(data));
} catch (exception) {
    console.error('Failed to check whether the window is showing. Cause: ' + JSON.stringify(exception));
}
```

### setWindowBackgroundColor<sup>9+</sup>

setWindowBackgroundColor(color: string): void

设置窗口的背景色。Stage模型下，该接口需要在[loadContent()](#loadcontent9)或[setUIContent()](#setuicontent9)调用生效后使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| ----- | ------ | -- | ----------------------------------------------------------------------- |
| color | string | 是 | 需要设置的背景色，为十六进制RGB或ARGB颜色，不区分大小写，例如`#00FF00`或`#FF00FF00`。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**示例：**

```js
let color = '#00ff33';
try {
    windowClass.setWindowBackgroundColor(color);
} catch (exception) {
    console.error('Failed to set the background color. Cause: ' + JSON.stringify(exception));
}
```

### setWindowBrightness<sup>9+</sup>

setWindowBrightness(brightness: number, callback: AsyncCallback&lt;void&gt;): void

设置屏幕亮度值，使用callback异步回调。

当前屏幕亮度规格：窗口设置屏幕亮度生效时，控制中心不可以调整系统屏幕亮度，窗口恢复默认系统亮度之后，控制中心可以调整系统屏幕亮度。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明                                        |
| ---------- | ------------------------- | -- |-------------------------------------------|
| brightness | number                    | 是 | 屏幕亮度值。该参数为浮点数，取值范围为[0.0, 1.0]。其取1.0时表示最亮。 |
| callback   | AsyncCallback&lt;void&gt; | 是 | 回调函数。                                     |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
let brightness = 1;
try {
    windowClass.setWindowBrightness(brightness, (err) => {
        if (err.code) {
            console.error('Failed to set the brightness. Cause: ' + JSON.stringify(err));
            return;
        }
        console.info('Succeeded in setting the brightness.');
    });
} catch (exception) {
    console.error('Failed to set the brightness. Cause: ' + JSON.stringify(exception));
}
```

### setWindowBrightness<sup>9+</sup>

setWindowBrightness(brightness: number): Promise&lt;void&gt;

设置屏幕亮度值，使用Promise异步回调。

当前屏幕亮度规格：窗口设置屏幕亮度生效时，控制中心不可以调整系统屏幕亮度，窗口恢复默认系统亮度之后，控制中心可以调整系统屏幕亮度。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明                                     |
| ---------- | ------ | -- |----------------------------------------|
| brightness | number | 是 | 屏幕亮度值。该参数为浮点数，取值范围为[0.0, 1.0]。1.0表示最亮。 |

**返回值：**

| 类型 | 说明 |
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
let brightness = 1;
try {
    let promise = windowClass.setWindowBrightness(brightness);
    promise.then(()=> {
        console.info('Succeeded in setting the brightness.');
    }).catch((err)=>{
        console.error('Failed to set the brightness. Cause: ' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to set the brightness. Cause: ' + JSON.stringify(exception));
}
```

### setWindowKeepScreenOn<sup>9+</sup>

setWindowKeepScreenOn(isKeepScreenOn: boolean, callback: AsyncCallback&lt;void&gt;): void

设置屏幕是否为常亮状态，使用callback异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------------- | ------------------------- | -- | ---------------------------------------------------- |
| isKeepScreenOn | boolean                   | 是 | 设置屏幕是否为常亮状态。true表示常亮；false表示不常亮。  |
| callback       | AsyncCallback&lt;void&gt; | 是 | 回调函数。                                            |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
let isKeepScreenOn = true;
try {
    windowClass.setWindowKeepScreenOn(isKeepScreenOn, (err) => {
        if (err.code) {
            console.error('Failed to set the screen to be always on. Cause: ' + JSON.stringify(err));
            return;
        }
        console.info('Succeeded in setting the screen to be always on.');
    });
} catch (exception) {
    console.error('Failed to set the screen to be always on. Cause: ' + JSON.stringify(exception));
}
```

### setWindowKeepScreenOn<sup>9+</sup>

setWindowKeepScreenOn(isKeepScreenOn: boolean): Promise&lt;void&gt;

设置屏幕是否为常亮状态，使用Promise异步回调。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------------- | ------- | -- | --------------------------------------------------- |
| isKeepScreenOn | boolean | 是 | 设置屏幕是否为常亮状态。true表示常亮；false表示不常亮。 |

**返回值：**

| 类型 | 说明 |
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**示例：**

```js
let isKeepScreenOn = true;
try {
    let promise = windowClass.setWindowKeepScreenOn(isKeepScreenOn);
    promise.then(() => {
        console.info('Succeeded in setting the screen to be always on.');
    }).catch((err)=>{
        console.info('Failed to set the screen to be always on. Cause:  ' + JSON.stringify(err));
    });
} catch (exception) {
    console.error('Failed to set the screen to be always on. Cause: ' + JSON.stringify(exception));
}
```

## WindowStage<sup>9+</sup>

窗口管理器。管理各个基本窗口单元，即[Window](#window)实例。

下列API示例中都需在[onWindowStageCreate()](js-apis-app-ability-uiAbility.md#uiabilityonwindowstagecreate)函数中使用WindowStage的实例调用对应方法。

### getMainWindow<sup>9+</sup>

getMainWindow(callback: AsyncCallback&lt;Window&gt;): void

获取该WindowStage实例下的主窗口，使用callback异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                                   | 必填 | 说明                                          |
| -------- | -------------------------------------- | ---- | --------------------------------------------- |
| callback | AsyncCallback&lt;[Window](#window)&gt; | 是   | 回调函数。返回当前WindowStage下的主窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        let windowClass = null;
        windowStage.getMainWindow((err, data) => {
            if (err.code) {
                console.error('Failed to obtain the main window. Cause: ' + JSON.stringify(err));
                return;
            }
            windowClass = data;
            console.info('Succeeded in obtaining the main window. Data: ' + JSON.stringify(data));
        });
    }
};
```

### getMainWindow<sup>9+</sup>

getMainWindow(): Promise&lt;Window&gt;

获取该WindowStage实例下的主窗口，使用Promise异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型                             | 说明                                             |
| -------------------------------- | ------------------------------------------------ |
| Promise&lt;[Window](#window)&gt; | Promise对象。返回当前WindowStage下的主窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        let windowClass = null;
        let promise = windowStage.getMainWindow();
        promise.then((data) => {
        windowClass = data;
            console.info('Succeeded in obtaining the main window. Data: ' + JSON.stringify(data));
        }).catch((err) => {
            console.error('Failed to obtain the main window. Cause: ' + JSON.stringify(err));
        });
    }
};
```

### getMainWindowSync<sup>9+</sup>

getMainWindowSync(): Window

获取该WindowStage实例下的主窗口。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型 | 说明 |
| ----------------- | --------------------------------- |
| [Window](#window) | 返回当前WindowStage下的主窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        try {
            let windowClass = windowStage.getMainWindowSync();
        } catch (exception) {
            console.error('Failed to obtain the main window. Cause: ' + JSON.stringify(exception));
        };
    }
};
```

### createSubWindow<sup>9+</sup>

createSubWindow(name: string, callback: AsyncCallback&lt;Window&gt;): void

创建该WindowStage实例下的子窗口，使用callback异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                                   | 必填 | 说明                                          |
| -------- | -------------------------------------- | ---- | --------------------------------------------- |
| name     | string                                 | 是   | 子窗口的名字。                                |
| callback | AsyncCallback&lt;[Window](#window)&gt; | 是   | 回调函数。返回当前WindowStage下的子窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        let windowClass = null;
        try {
            windowStage.createSubWindow('mySubWindow', (err, data) => {
                if (err.code) {
                    console.error('Failed to create the subwindow. Cause: ' + JSON.stringify(err));
                    return;
                }
                windowClass = data;
                console.info('Succeeded in creating the subwindow. Data: ' + JSON.stringify(data));
                windowClass.resetSize(500, 1000);
            });
        } catch (exception) {
            console.error('Failed to create the subwindow. Cause: ' + JSON.stringify(exception));
        };
    }
};
```
### createSubWindow<sup>9+</sup>

createSubWindow(name: string): Promise&lt;Window&gt;

创建该WindowStage实例下的子窗口，使用Promise异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| name   | string | 是   | 子窗口的名字。 |

**返回值：**

| 类型                             | 说明                                             |
| -------------------------------- | ------------------------------------------------ |
| Promise&lt;[Window](#window)&gt; | Promise对象。返回当前WindowStage下的子窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        let windowClass = null;
        try {
            let promise = windowStage.createSubWindow('mySubWindow');
            promise.then((data) => {
                windowClass = data;
                console.info('Succeeded in creating the subwindow. Data: ' + JSON.stringify(data));
            }).catch((err) => {
                console.error('Failed to create the subwindow. Cause: ' + JSON.stringify(err));
            });
        } catch (exception) {
            console.error('Failed to create the subwindow. Cause: ' + JSON.stringify(exception));
        };
    }
};
```

### getSubWindow<sup>9+</sup>

getSubWindow(callback: AsyncCallback&lt;Array&lt;Window&gt;&gt;): void

获取该WindowStage实例下的所有子窗口，使用callback异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                                                | 必填 | 说明                                              |
| -------- | --------------------------------------------------- | ---- | ------------------------------------------------- |
| callback | AsyncCallback&lt;Array&lt;[Window](#window)&gt;&gt; | 是   | 回调函数。返回当前WindowStage下的所有子窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        let windowClass = null;
        windowStage.getSubWindow((err, data) => {
            if (err.code) {
                console.error('Failed to obtain the subwindow. Cause: ' + JSON.stringify(err));
                return;
            }
            windowClass = data;
            console.info('Succeeded in obtaining the subwindow. Data: ' + JSON.stringify(data));
        });
    }
};
```
### getSubWindow<sup>9+</sup>

getSubWindow(): Promise&lt;Array&lt;Window&gt;&gt;

获取该WindowStage实例下的所有子窗口，使用Promise异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型                                          | 说明                                                 |
| --------------------------------------------- | ---------------------------------------------------- |
| Promise&lt;Array&lt;[Window](#window)&gt;&gt; | Promise对象。返回当前WindowStage下的所有子窗口对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        let windowClass = null;
        let promise = windowStage.getSubWindow();
        promise.then((data) => {
            windowClass = data;
            console.info('Succeeded in obtaining the subwindow. Data: ' + JSON.stringify(data));
        }).catch((err) => {
            console.error('Failed to obtain the subwindow. Cause: ' + JSON.stringify(err));
        })
    }
};
```

### loadContent<sup>9+</sup>

loadContent(path: string, storage: LocalStorage, callback: AsyncCallback&lt;void&gt;): void

为当前WindowStage的主窗口加载与LocalStorage相关联的具体页面内容，使用callback异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                                            | 必填 | 说明                                                         |
| -------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                                          | 是   | 设置加载页面的路径。                                         |
| storage  | [LocalStorage](../../quick-start/arkts-localstorage.md) | 是   | 存储单元，为应用程序范围内的可变状态属性和非可变状态属性提供存储。 |
| callback | AsyncCallback&lt;void&gt;                       | 是   | 回调函数。                                                   |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    storage : LocalStorage
    onWindowStageCreate(windowStage) {
        this.storage = new LocalStorage();
        this.storage.setOrCreate('storageSimpleProp',121);
        console.log('onWindowStageCreate');
        try {
            windowStage.loadContent('pages/page2',this.storage,(err) => {
                if (err.code) {
                    console.error('Failed to load the content. Cause:' + JSON.stringify(err));
                    return;
                }
                console.info('Succeeded in loading the content.');
            });
        } catch (exception) {
            console.error('Failed to load the content. Cause:' + JSON.stringify(exception));
        };
    }
};
```

### loadContent<sup>9+</sup>

loadContent(path: string, storage?: LocalStorage): Promise&lt;void&gt;

为当前WindowStage的主窗口加载与LocalStorage相关联的具体页面内容，使用Promise异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名  | 类型                                            | 必填 | 说明                                                         |
| ------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path    | string                                          | 是   | 设置加载页面的路径。                                         |
| storage | [LocalStorage](../../quick-start/arkts-localstorage.md) | 否   | 存储单元，为应用程序范围内的可变状态属性和非可变状态属性提供存储。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    storage : LocalStorage
    onWindowStageCreate(windowStage) {
        this.storage = new LocalStorage();
        this.storage.setOrCreate('storageSimpleProp',121);
        console.log('onWindowStageCreate');
        try {
            let promise = windowStage.loadContent('pages/page2',this.storage);
            promise.then(() => {
                console.info('Succeeded in loading the content.');
            }).catch((err) => {
                console.error('Failed to load the content. Cause:' + JSON.stringify(err));
            });
        } catch (exception) {
            console.error('Failed to load the content. Cause:' + JSON.stringify(exception));
        };
    }
};
```

### loadContent<sup>9+</sup>

loadContent(path: string, callback: AsyncCallback&lt;void&gt;): void

为当前WindowStage的主窗口加载具体页面内容，使用callback异步回调。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                 |
| -------- | ------------------------- | ---- | -------------------- |
| path     | string                    | 是   | 设置加载页面的路径。 |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数。           |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        try {
            windowStage.loadContent('pages/page2', (err) => {
                if (err.code) {
                    console.error('Failed to load the content. Cause:' + JSON.stringify(err));
                    return;
                }
                console.info('Succeeded in loading the content.');
            });
        } catch (exception) {
            console.error('Failed to load the content. Cause:' + JSON.stringify(exception));
        };
    }
};
```

### on('windowStageEvent')<sup>9+</sup>

on(eventType: 'windowStageEvent', callback: Callback&lt;WindowStageEventType&gt;): void

开启WindowStage生命周期变化的监听。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'windowStageEvent'，即WindowStage生命周期变化事件。 |
| callback | Callback&lt;[WindowStageEventType](#windowstageeventtype9)&gt; | 是   | 回调函数。返回当前的WindowStage生命周期状态。                |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        try {
            windowStage.on('windowStageEvent', (data) => {
                console.info('Succeeded in enabling the listener for window stage event changes. Data: ' +
                    JSON.stringify(data));
            });
        } catch (exception) {
            console.error('Failed to enable the listener for window stage event changes. Cause:' +
                JSON.stringify(exception));
        };
    }
};
```

### off('windowStageEvent')<sup>9+</sup>

off(eventType: 'windowStageEvent', callback?: Callback&lt;WindowStageEventType&gt;): void

关闭WindowStage生命周期变化的监听。

**模型约束：** 此接口仅可在Stage模型下使用。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'windowStageEvent'，即WindowStage生命周期变化事件。 |
| callback | Callback&lt;[WindowStageEventType](#windowstageeventtype9)&gt; | 否   | 回调函数。返回当前的WindowStage生命周期状态。                |

**错误码：**

以下错误码的详细介绍请参见[窗口错误码](../errorcodes/errorcode-window.md)。

| 错误码ID | 错误信息 |
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    // ...

    onWindowStageCreate(windowStage) {
        console.log('onWindowStageCreate');
        try {
            windowStage.off('windowStageEvent');
        } catch (exception) {
            console.error('Failed to disable the listener for window stage event changes. Cause:' +
                JSON.stringify(exception));
        };
    }
};
```

