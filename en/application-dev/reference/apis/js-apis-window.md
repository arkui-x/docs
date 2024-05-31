# @ohos.window (Window)

The **Window** module provides basic window management capabilities, such as creating and destroying the current window, setting properties for the current window, and managing and scheduling windows.

This module provides the following common window-related functions:

- [Window](#window): the current window instance, which is the basic unit managed by the window manager.
- [WindowStage](#windowstage9): window manager that manages windows.

> **NOTE**
>
> The initial APIs of this module are supported since API version 6. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```js
import window from '@ohos.window';
```

## Configuration<sup>9+</sup>

Defines the parameters for creating a subwindow or system window.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name| Type| Mandatory| Description                                                                         |
| ---------- | -------------------------- | -- |-----------------------------------------------------------------------------|
| name       | string                     | Yes| Name of the window.                                                                      |
| ctx        | BaseContext                | No| Current application context. If no value is passed, no context is used.<br>You do not need to set this parameter to create a system window in the stage model.|
| displayId  | number                     | No| ID of the current physical screen. If no value is passed, the default value **-1** is used. The value must be an integer.                                            |
| parentId   | number                     | No| ID of the parent window. If no value is passed, the default value **-1** is used. The value must be an integer.                                                          |

## Orientation<sup>9+</sup>

Enumerates the window orientations.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name                                 | Value  | Description                         |
| ------------------------------------- | ---- | ----------------------------- |
| UNSPECIFIED                           | 0    | Unspecified. The orientation is determined by the system.|
| PORTRAIT                              | 1    | Portrait.            |
| LANDSCAPE                             | 2    | Landscape.  |
| PORTRAIT_INVERTED                     | 3    | Reverse portrait.  |
| LANDSCAPE_INVERTED                    | 4    | Reverse landscape.|


## Rect<sup>7+</sup>

Describes the rectangular area of the window.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name  | Type| Readable| Writable| Description              |
| ------ | -------- | ---- | ---- | ------------------ |
| left   | number   | Yes  | Yes  | Left boundary of the rectangle, in pixels. The value must be an integer.|
| top    | number   | Yes  | Yes  | Top boundary of the rectangle, in pixels. The value must be an integer.|
| width  | number   | Yes  | Yes  | Width of the rectangle, in pixels. The value must be an integer.|
| height | number   | Yes  | Yes  | Height of the rectangle, in pixels. The value must be an integer.|

## Size<sup>7+</sup>

Describes the window size.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name  | Type| Readable| Writable| Description      |
| ------ | -------- | ---- | ---- | ---------- |
| width  | number   | Yes  | Yes  | Window width, in pixels. The value must be an integer.|
| height | number   | Yes  | Yes  | Window height, in pixels. The value must be an integer.|

## WindowProperties

Describes the window properties.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name                                 | Type                 | Readable| Writable| Description                                                                                                    |
| ------------------------------------- | ------------------------- | ---- | ---- |--------------------------------------------------------------------------------------------------------|
| windowRect<sup>7+</sup>               | [Rect](#rect7)             | Yes  | Yes  | Window size.                                                                                                 |
| brightness                            | number                    | Yes  | Yes  | Screen brightness. The value is a floating point number in the range [0.0, 1.0], and the value **1.0** means the brightest. If no value is passed, the brightness follows the system. In this case, the obtained brightness value is **-1**.                     |
| isKeepScreenOn                        | boolean                   | Yes  | Yes  | Whether the screen is always on. The default value is **false**. The value **true** means that the screen is always on, and **false** means the opposite.                                                                  |
## window.createWindow<sup>9+</sup>

createWindow(config: Configuration, callback: AsyncCallback&lt;Window&gt;): void

Creates a subwindow or system window. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------------------------------------- | -- | --------------------------------- |
| config   | [Configuration](#configuration9)       | Yes| Parameters used for creating the window.  |
| callback | AsyncCallback&lt;[Window](#window)&gt; | Yes| Callback used to return the window created.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------- |
| 1300001 | Repeated operation. |
| 1300006 | This window context is abnormal. |
| 1300008 | The operation is on invalid display. |
| 1300009 | The parent window is invalid. |

**Example**

```js
let windowClass = null;
let config = {name: "alertWindow", ctx: this.context};
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

Creates a subwindow or system window. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| ------ | -------------------------------- | -- | ------------------ |
| config | [Configuration](#configuration9) | Yes| Parameters used for creating the window.|

**Return value**

| Type| Description|
| -------------------------------- | ------------------------------------ |
| Promise&lt;[Window](#window)&gt; | Promise used to return the window created.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------- |
| 1300001 | Repeated operation. |
| 1300006 | This window context is abnormal. |
| 1300008 | The operation is on invalid display. |
| 1300009 | The parent window is invalid. |

**Example**

```js
let windowClass = null;
let config = {name: "alertWindow", ctx: this.context};
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

Finds a window based on the name.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type  | Mandatory| Description    |
| ------ | ------ | ---- | -------- |
| name   | string | Yes  | Window ID.|

**Return value**

| Type| Description|
| ----------------- | ------------------- |
| [Window](#window) | Window found.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------- |
| 1300002 | This window state is abnormal. |

**Example**

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

Obtains the top window of the current application. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------------------------------------- | -- | ---------------------------------------- |
| ctx      | BaseContext                            | Yes| Current application context.|
| callback | AsyncCallback&lt;[Window](#window)&gt; | Yes| Callback used to return the top window obtained.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------- |
| 1300002 | This window state is abnormal.   |
| 1300006 | This window context is abnormal. |

**Example**

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

Obtains the top window of the current application. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| ------ | ----------- | ---- | ------------------------------------------------------------ |
| ctx    | BaseContext | Yes  | Current application context.|

**Return value**

| Type| Description|
| -------------------------------- | ------------------------------------------- |
| Promise&lt;[Window](#window)&gt; | Promise used to return the top window obtained.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------- |
| 1300002 | This window state is abnormal.   |
| 1300006 | This window context is abnormal. |

**Example**

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

Represents the current window instance, which is the basic unit managed by the window manager.

In the following API examples, you must use [getLastWindow()](#windowgetlastwindow9), [createWindow()](#windowcreatewindow9), or [findWindow()](#windowfindwindow9) to obtain a **Window** instance and then call a method in this instance.

### showWindow<sup>9+</sup>

showWindow(callback: AsyncCallback&lt;void&gt;): void

Shows this window. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | ------------------------- | -- | --------- |
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

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

Shows this window. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type| Description|
| ------------------- | ----------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

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

Destroys this window. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | ------------------------- | -- | --------- |
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Destroys this window. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type| Description|
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Moves this window. This API uses an asynchronous callback to return the result.

This operation is not supported in a window in full-screen mode.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | ------------------------- | -- | --------------------------------------------- |
| x        | number                    | Yes| Distance that the window moves along the x-axis, in px. A positive value indicates that the window moves to the right. The value must be an integer.|
| y        | number                    | Yes| Distance that the window moves along the y-axis, in px. A positive value indicates that the window moves downwards. The value must be an integer.|
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.                                    |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Moves this window. This API uses a promise to return the result.

This operation is not supported in a window in full-screen mode.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -- | ----- | -- | --------------------------------------------- |
| x | number | Yes| Distance that the window moves along the x-axis, in px. A positive value indicates that the window moves to the right. The value must be an integer.|
| y | number | Yes| Distance that the window moves along the y-axis, in px. A positive value indicates that the window moves downwards. The value must be an integer.|

**Return value**

| Type| Description|
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Changes the size of this window. This API uses an asynchronous callback to return the result.

The main window and subwindow have the following size limits: [320, 2560] in width and [240, 2560] in height, both in units of vp.

The system window has the following size limits: [0, 2560] in width and [0, 2560] in height, both in units of vp.

The new width and height you set must meet the limits.

This operation is not supported in a window in full-screen mode.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | ------------------------- | -- | ------------------------ |
| width    | number                    | Yes| New width of the window, in px. The value must be an integer.|
| height   | number                    | Yes| New height of the window, in px. The value must be an integer.|
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.               |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Changes the size of this window. This API uses a promise to return the result.

The main window and subwindow have the following size limits: [320, 2560] in width and [240, 2560] in height, both in units of vp.

The system window has the following size limits: [0, 2560] in width and [0, 2560] in height, both in units of vp.

The new width and height you set must meet the limits.

This operation is not supported in a window in full-screen mode.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| ------ | ------ | -- | ------------------------ |
| width  | number | Yes| New width of the window, in px. The value must be an integer.|
| height | number | Yes| New height of the window, in px. The value must be an integer.|

**Return value**

| Type| Description|
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Obtains the properties of this window.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type| Description|
| ------------------------------------- | ------------- |
| [WindowProperties](#windowproperties) | Window properties obtained.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

```js
try {
    let properties = windowClass.getWindowProperties();
} catch (exception) {
    console.error('Failed to obtain the window properties. Cause: ' + JSON.stringify(exception));
}
```

### setWindowSystemBarEnable<sup>9+</sup>

setWindowSystemBarEnable(names: Array<'status' | 'navigation'>, callback: AsyncCallback&lt;void&gt;): void

Sets whether to display the status bar and navigation bar when the window is in full-screen mode. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | ---------------------------- | -- | --------- |
| names    | Array<'status'\|'navigation'> | Yes| Whether to display the status bar and navigation bar when the window is in full-screen mode.<br>For example, to display the status bar and navigation bar, set this parameter to **['status', 'navigation']**. By default, they are not displayed.|
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

```js
// In this example, the status bar and navigation bar are not displayed.
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

Sets whether to display the status bar and navigation bar when the window is in full-screen mode. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type | Mandatory| Description|
| ----- | ---------------------------- | -- | --------------------------------- |
| names | Array<'status'\|'navigation'> | Yes| Whether to display the status bar and navigation bar when the window is in full-screen mode.<br>For example, to display the status bar and navigation bar, set this parameter to **['status', 'navigation']**. By default, they are not displayed.|

**Return value**

| Type| Description|
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

```js
// In this example, the status bar and navigation bar are not displayed.
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

Sets the preferred orientation for this window. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name             | Type                                       | Mandatory| Description                  |
| ------------------- | ------------------------------------------- | ---- | ---------------------- |
| Orientation         | [Orientation](#orientation9)                | Yes  | Orientation to set.        |
| callback            | AsyncCallback&lt;void&gt;                   | Yes  | Callback used to return the result.            |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

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

Sets the preferred orientation for this window. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name             | Type                                       | Mandatory| Description                  |
| ------------------- | ------------------------------------------- | ---- | ---------------------- |
| Orientation         | [Orientation](#orientation9)                | Yes  | Orientation to set.      |

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

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

Obtain a **UIContext** instance.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type      | Description                  |
| ---------- | ---------------------- |
| UIContext  | **UIContext** instance obtained.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        // Load content for the main window.
        windowStage.loadContent("pages/page2", (err) => {
            if (err.code) {
                console.error('Failed to load the content. Cause:' + JSON.stringify(err));
                return;
            }
            console.info('Succeeded in loading the content.');
        });
        // Obtain the main window.
        let windowClass = null;
        windowStage.getMainWindow((err, data) => {
            if (err.code) {
                console.error('Failed to obtain the main window. Cause: ' + JSON.stringify(err));
                return;
            }
            windowClass = data;
            console.info('Succeeded in obtaining the main window. Data: ' + JSON.stringify(data));
            // Obtain a UIContext instance.
            globalThis.uiContext = windowClass.getUIContext();
        })
    }
};
```

### setUIContent<sup>9+</sup>

setUIContent(path: string, callback: AsyncCallback&lt;void&gt;): void

Loads content from a page to this window. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | ------------------------- | -- | -------------------- |
| path     | string                    | Yes| Path of the page from which the content will be loaded.|
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.         |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Loads content from a page to this window. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| ---- | ------ | -- | ------------------ |
| path | string | Yes| Path of the page from which the content will be loaded.|

**Return value**

| Type| Description|
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Loads content from a page associated with a local storage to this window. This API uses an asynchronous callback to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                                           | Mandatory| Description                                                        |
| -------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                                          | Yes  | Path of the page from which the content will be loaded.                                        |
| storage  | LocalStorage                                    | Yes  | A storage unit, which provides storage for variable state properties and non-variable state properties of an application.|
| callback | AsyncCallback&lt;void&gt;                       | Yes  | Callback used to return the result.                                                  |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Loads content from a page associated with a local storage to this window. This API uses a promise to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name | Type                                           | Mandatory| Description                                                        |
| ------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path    | string                                          | Yes  | Path of the page from which the content will be loaded.                                        |
| storage | LocalStorage                                    | Yes  | A storage unit, which provides storage for variable state properties and non-variable state properties of an application.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Checks whether this window is displayed.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type| Description|
| ------- | ------------------------------------------------------------------ |
| boolean | Whether the window is displayed. The value **true** means that the window is displayed, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

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

Sets the background color for this window. In the stage model, this API must be used after the call of [loadContent](#loadcontent9) or [setUIContent()](#setuicontent9) takes effect.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| ----- | ------ | -- | ----------------------------------------------------------------------- |
| color | string | Yes| Background color to set. The value is a hexadecimal RGB or ARGB color code and is case insensitive, for example, **#00FF00** or **#FF00FF00**.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |

**Example**

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

Sets the screen brightness for this window. This API uses an asynchronous callback to return the result.

When the screen brightness setting for the window takes effect, Control Panel cannot adjust the system screen brightness. It can do so only after the window screen brightness is restored to the default value.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description                                       |
| ---------- | ------------------------- | -- |-------------------------------------------|
| brightness | number                    | Yes| Brightness to set. The value is a floating point number in the range [0.0, 1.0], and the value **1.0** means the brightest.|
| callback   | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.                                    |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Sets the screen brightness for this window. This API uses a promise to return the result.

When the screen brightness setting for the window takes effect, Control Panel cannot adjust the system screen brightness. It can do so only after the window screen brightness is restored to the default value.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description                                    |
| ---------- | ------ | -- |----------------------------------------|
| brightness | number | Yes| Brightness to set. The value is a floating point number in the range [0.0, 1.0], and the value **1.0** indicates the brightest.|

**Return value**

| Type| Description|
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Sets whether to keep the screen always on. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------------- | ------------------------- | -- | ---------------------------------------------------- |
| isKeepScreenOn | boolean                   | Yes| Whether to keep the screen always on. The value **true** means to keep the screen always on, and **false** means the opposite. |
| callback       | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.                                           |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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

Sets whether to keep the screen always on. This API uses a promise to return the result.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------------- | ------- | -- | --------------------------------------------------- |
| isKeepScreenOn | boolean | Yes| Whether to keep the screen always on. The value **true** means to keep the screen always on, and **false** means the opposite.|

**Return value**

| Type| Description|
| ------------------- | ------------------------ |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | -------------------------------------------- |
| 1300002 | This window state is abnormal.               |
| 1300003 | This window manager service works abnormally. |

**Example**

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


## WindowStageEventType<sup>9+</sup>

Describes the lifecycle of a window stage.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name      | Value| Description      |
| ---------- | ------ | ---------- |
| SHOWN      | 1      | The window stage is running in the foreground.|
| ACTIVE     | 2      | The window stage gains focus.|
| INACTIVE   | 3      | The window stage loses focus.|
| HIDDEN     | 4      | The window stage is running in the background.|

## WindowStage<sup>9+</sup>

Implements a window manager, which manages each basic window unit, that is, [Window](#window) instance.

Before calling any of the following APIs, you must use onWindowStageCreate() to create a **WindowStage** instance.

### getMainWindow<sup>9+</sup>

getMainWindow(callback: AsyncCallback&lt;Window&gt;): void

Obtains the main window of this window stage. This API uses an asynchronous callback to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                                  | Mandatory| Description                                         |
| -------- | -------------------------------------- | ---- | --------------------------------------------- |
| callback | AsyncCallback&lt;[Window](#window)&gt; | Yes  | Callback used to return the main window.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Obtains the main window of this window stage. This API uses a promise to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type                            | Description                                            |
| -------------------------------- | ------------------------------------------------ |
| Promise&lt;[Window](#window)&gt; | Promise used to return the main window.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Obtains the main window of this window stage.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type| Description|
| ----------------- | --------------------------------- |
| [Window](#window) | return the main window.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Creates a subwindow for this window stage. This API uses an asynchronous callback to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                                  | Mandatory| Description                                         |
| -------- | -------------------------------------- | ---- | --------------------------------------------- |
| name     | string                                 | Yes  | Name of the subwindow.                               |
| callback | AsyncCallback&lt;[Window](#window)&gt; | Yes  | Callback used to return the subwindow.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Creates a subwindow for this window stage. This API uses a promise to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name| Type  | Mandatory| Description          |
| ------ | ------ | ---- | -------------- |
| name   | string | Yes  | Name of the subwindow.|

**Return value**

| Type                            | Description                                            |
| -------------------------------- | ------------------------------------------------ |
| Promise&lt;[Window](#window)&gt; | Promise used to return the subwindow.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Obtains all the subwindows of this window stage. This API uses an asynchronous callback to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                                               | Mandatory| Description                                             |
| -------- | --------------------------------------------------- | ---- | ------------------------------------------------- |
| callback | AsyncCallback&lt;Array&lt;[Window](#window)&gt;&gt; | Yes  | Callback used to return all the subwindows.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300005 | This window stage is abnormal. |

**Example**

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

Obtains all the subwindows of this window stage. This API uses a promise to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type                                         | Description                                                |
| --------------------------------------------- | ---------------------------------------------------- |
| Promise&lt;Array&lt;[Window](#window)&gt;&gt; | Promise used to return all the subwindows.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300005 | This window stage is abnormal. |

**Example**

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

Loads content from a page associated with a local storage to the main window in this window stage. This API uses an asynchronous callback to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                                           | Mandatory| Description                                                        |
| -------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                                          | Yes  | Path of the page from which the content will be loaded.                                        |
| storage  | LocalStorage                                    | Yes  | A storage unit, which provides storage for variable state properties and non-variable state properties of an application.|
| callback | AsyncCallback&lt;void&gt;                       | Yes  | Callback used to return the result.                                                  |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Loads content from a page associated with a local storage to the main window in this window stage. This API uses a promise to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name | Type                                           | Mandatory| Description                                                        |
| ------- | ----------------------------------------------- | ---- | ------------------------------------------------------------ |
| path    | string                                          | Yes  | Path of the page from which the content will be loaded.                                        |
| storage | LocalStorage                                    | No   | A storage unit, which provides storage for variable state properties and non-variable state properties of an application.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Loads content from a page to this window stage. This API uses an asynchronous callback to return the result.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                |
| -------- | ------------------------- | ---- | -------------------- |
| path     | string                    | Yes  | Path of the page from which the content will be loaded.|
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback used to return the result.          |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Subscribes to the window stage lifecycle change event.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                                        |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | Yes  | Event type. The value is fixed at **'windowStageEvent'**, indicating the window stage lifecycle change event.|
| callback | Callback&lt;[WindowStageEventType](#windowstageeventtype9)&gt; | Yes  | Callback used to return the window stage lifecycle state.               |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

Unsubscribes from the window stage lifecycle change event.

**Model restriction**: This API can be used only in the stage model.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                                        |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | Yes  | Event type. The value is fixed at **'windowStageEvent'**, indicating the window stage lifecycle change event.|
| callback | Callback&lt;[WindowStageEventType](#windowstageeventtype9)&gt; | No  | Callback used to return the window stage lifecycle state. If a value is passed in, the corresponding subscription is canceled. If no value is passed in, all subscriptions to the specified event are canceled.               |

**Error codes**

For details about the error codes, see [Window Error Codes](../errorcodes/errorcode-window.md).

| ID| Error Message|
| ------- | ------------------------------ |
| 1300002 | This window state is abnormal. |
| 1300005 | This window stage is abnormal. |

**Example**

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

