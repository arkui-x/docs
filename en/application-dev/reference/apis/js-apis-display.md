# @ohos.display (Display)

The **Display** module provides APIs for managing displays, such as obtaining information about the default display, obtaining information about all displays, and listening for the addition and removal of displays.

> **NOTE**
>
> The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```js
import display from '@ohos.display';
```

## Orientation<sup>10+</sup>

Enumerates the orientations of the display.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name| Value| Description|
| -------- | -------- | -------- |
| PORTRAIT | 0 | The display is in portrait mode.|
| LANDSCAPE | 1 | The display is in landscape mode.|
| PORTRAIT_INVERTED | 2 | The display is in reverse portrait mode.|
| LANDSCAPE_INVERTED | 3 | The display is in reverse landscape mode.|

## Rect<sup>9+</sup>

Describes a rectangle on the display.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name  | Type| Readable| Writable| Description              |
| ------ | -------- | ---- | ---- | ------------------ |
| left   | number   | Yes  | Yes  | Left boundary of the rectangle, in pixels. The value must be an integer.|
| top    | number   | Yes  | Yes  | Top boundary of the rectangle, in pixels. The value must be an integer.|
| width  | number   | Yes  | Yes  | Width of the rectangle, in pixels. The value must be an integer.  |
| height | number   | Yes  | Yes  | Height of the rectangle, in pixels. The value must be an integer.  |

## display.getDefaultDisplaySync<sup>9+</sup>

getDefaultDisplaySync(): Display

Obtains the default display object. This API returns the result synchronously.

**System capability**: SystemCapability.WindowManager.WindowManager.Core

**Return value**

| Type                          | Description                                          |
| ------------------------------| ----------------------------------------------|
| [Display](#display) | Default display object.|

**Error codes**

For details about the error codes, see [Display Error Codes](../errorcodes/errorcode-display.md).

| ID| Error Message|
| ------- | ----------------------- |
| 1400001 | Invalid display or screen. |

**Example**

```js
let displayClass = null;
try {
    displayClass = display.getDefaultDisplaySync();
} catch (exception) {
    console.error('Failed to obtain the default display object. Code: ' + JSON.stringify(exception));
}
```

## Display
Implements a **Display** instance, with properties and APIs defined.

Before calling any API in **Display**, you must use [getDefaultDisplaySync()](#displaygetdefaultdisplaysync9) to obtain a **Display** instance.

### Attributes

**System capability**: SystemCapability.WindowManager.WindowManager.Core

| Name| Type| Readable| Writable| Description                                                                                                           |
| -------- | -------- | -------- | -------- |---------------------------------------------------------------------------------------------------------------|
| id | number | Yes| No| ID of the display. The value must be an integer.                                                                                            |                                                                                               |
| width | number | Yes| No| Width of the display, in pixels. The value must be an integer.                                                                                       |
| height | number | Yes| No| Height of the display, in pixels. The value must be an integer.                                                                                       |
| orientation<sup>10+</sup> | [Orientation](#orientation10) | Yes| No| Orientation of the display.                                                                                                 |

