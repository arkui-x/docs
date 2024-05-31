# @ohos.UiTest

The **UiTest** module provides APIs that you can use to simulate UI actions during testing, such as clicks, double-clicks, long-clicks, and swipes.

This module provides the following functions:

- [On<sup>9+</sup>](#on9): provides UI component feature description APIs for component filtering and matching.
- [Component<sup>9+</sup>](#component9): represents a component on the UI and provides APIs for obtaining component attributes, clicking a component, scrolling to search for a component, and text injection.
- [Driver<sup>9+</sup>](#driver9): works as the entry class and provides APIs for features such as component matching/search, key injection, coordinate clicking/sliding, and screenshot.

>**NOTE**
>
>The initial APIs of this module are supported since API version 8. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import

```js
import {Component, Driver, ON, MatchPattern} from '@ohos.UiTest';
```

## MatchPattern

Enumerates the match patterns supported for component attributes.

**System capability**: SystemCapability.Test.UiTest

| Name       | Value  | Description          |
| ----------- | ---- | -------------- |
| EQUALS      | 0    | Equals the given value.  |
| CONTAINS    | 1    | Contains the given value.  |
| STARTS_WITH | 2    | Starts with the given value.|
| ENDS_WITH   | 3    | Ends with the given value.|

## Point<sup>9+</sup>

Provides the coordinates of a point.

**System capability**: SystemCapability.Test.UiTest

| Name| Type  | Readable| Writable| Description            |
| ---- | ------ | ---- | ---- | ---------------- |
| x    | number | Yes  | No  | X-coordinate of a point.|
| y    | number | Yes  | No  | Y-coordinate of a point.|

## On<sup>9+</sup>

Since API version 9, the UiTest framework provides a wide range of UI component feature description APIs in the **On** class to filter and match components.

The API capabilities provided by the **On** class exhibit the following features:

- Allow one or more attributes as the match conditions. For example, you can specify both the **text** and **id** attributes to find the target component.
- Provide multiple match patterns for component attributes.

All APIs provided in the **On** class are synchronous. You are advised to use the static constructor **ON** to create an **On** object in chain mode.

```js
ON.text('123').type('button');
```

### text<sup>9+</sup>

text(txt: string, pattern?: MatchPattern): On

Specifies the text attribute of the target component. Multiple match patterns are supported.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name | Type                         | Mandatory| Description                                               |
| ------- | ----------------------------- | ---- | --------------------------------------------------- |
| txt     | string                        | Yes  | Component text, used to match the target component.               |
| pattern | [MatchPattern](#matchpattern) | No  | Match pattern. The default value is [EQUALS](#matchpattern).|

**Return value**

| Type      | Description                              |
| ---------- | ---------------------------------- |
| [On](#on9) | **On** object that matches the text attribute of the target component.|

**Example**

```js
let on = ON.text('123'); // Use the static constructor ON to create an On object and specify the text attribute of the target component.
```


### id<sup>9+</sup>

id(id: string): On

Specifies the ID attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description            |
| ------ | ------ | ---- | ---------------- |
| id     | string | Yes  | Component ID.|

**Return value**

| Type      | Description                            |
| ---------- | -------------------------------- |
| [On](#on9) | **On** object that matches the ID attribute of the target component.|

**Example**

```js
let on = ON.id('123'); // Use the static constructor ON to create an On object and specify the ID attribute of the target component.
```


### type<sup>9+</sup>

type(tp: string): On

Specifies the type attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description          |
| ------ | ------ | ---- | -------------- |
| tp     | string | Yes  | Component type.|

**Return value**

| Type      | Description                                    |
| ---------- | ---------------------------------------- |
| [On](#on9) | **On** object that matches the type attribute of the target component.|

**Example**

```js
let on = ON.type('button'); // Use the static constructor ON to create an On object and specify the type attribute of the target component.
```


### clickable<sup>9+</sup>

clickable(b?: boolean): On

Specifies the clickable status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                        |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | No  | Clickable status of the target component.<br>**true**: clickable.<br>**false**: not clickable.<br> Default value: **true**|

**Return value**

| Type      | Description                                      |
| ---------- | ------------------------------------------ |
| [On](#on9) | **On** object that matches the clickable status attribute of the target component.|

**Example**

```js
let on = ON.clickable(true); // Use the static constructor ON to create an On object and specify the clickable status attribute of the target component.
```

### longClickable<sup>9+</sup>

longClickable(b?: boolean): On

Specifies the long-clickable status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                        |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | No  | Long-clickable status of the target component.<br>**true**: long-clickable.<br>**false**: not long-clickable.<br> Default value: **true**|

**Return value**

| Type      | Description                                          |
| ---------- | ---------------------------------------------- |
| [On](#on9) | **On** object that matches the long-clickable status attribute of the target component.|

**Example**

```js
let on = ON.longClickable(true); // Use the static constructor ON to create an On object and specify the long-clickable status attribute of the target component.
```


### scrollable<sup>9+</sup>

scrollable(b?: boolean): On

Specifies the scrollable status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                       |
| ------ | ------- | ---- | ----------------------------------------------------------- |
| b      | boolean | No  | Scrollable status of the target component.<br>**true**: scrollable.<br>**false**: not scrollable.<br> Default value: **true**|

**Return value**

| Type      | Description                                      |
| ---------- | ------------------------------------------ |
| [On](#on9) | **On** object that matches the scrollable status attribute of the target component.|

**Example**

```js
let on = ON.scrollable(true); // Use the static constructor ON to create an On object and specify the scrollable status attribute of the target component.
```

### enabled<sup>9+</sup>

enabled(b?: boolean): On

Specifies the enabled status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                     |
| ------ | ------- | ---- | --------------------------------------------------------- |
| b      | boolean | No  | Enabled status of the target component.<br>**true**: enabled.<br>**false**: not enabled.<br> Default value: **true**|

**Return value**

| Type      | Description                                    |
| ---------- | ---------------------------------------- |
| [On](#on9) | **On** object that matches the enabled status attribute of the target component.|

**Example**

```js
let on = ON.enabled(true); // Use the static constructor ON to create an On object and specify the enabled status attribute of the target component.
```

### focused<sup>9+</sup>

focused(b?: boolean): On

Specifies the focused status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                 |
| ------ | ------- | ---- | ----------------------------------------------------- |
| b      | boolean | No  | Focused status of the target component.<br>**true**: focused.<br>**false**: not focused.<br> Default value: **true**|

**Return value**

| Type      | Description                                    |
| ---------- | ---------------------------------------- |
| [On](#on9) | **On** object that matches the focused status attribute of the target component.|

**Example**

```js
let on = ON.focused(true); // Use the static constructor ON to create an On object and specify the focused status attribute of the target component.
```

### selected<sup>9+</sup>

selected(b?: boolean): On

Specifies the selected status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                        |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | No  | Selected status of the target component.<br>**true**: selected.<br>**false**: not selected.<br> Default value: **true**|

**Return value**

| Type      | Description                                      |
| ---------- | ------------------------------------------ |
| [On](#on9) | **On** object that matches the selected status attribute of the target component.|

**Example**

```js
let on = ON.selected(true); // Use the static constructor ON to create an On object and specify the selected status attribute of the target component.
```

### checked<sup>9+</sup>

checked(b?: boolean): On

Specifies the checked status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                        |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | No  | Checked status of the target component.<br>**true**: checked.<br>**false**: not checked.<br> Default value: **false**|

**Return value**

| Type      | Description                                      |
| ---------- | ------------------------------------------ |
| [On](#on9) | **On** object that matches the checked status attribute of the target component.|

**Example**

```js
let on = ON.checked(true); // Use the static constructor ON to create an On object and specify the checked status attribute of the target component.
```

### checkable<sup>9+</sup>

checkable(b?: boolean): On

Specifies the checkable status attribute of the target component.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type   | Mandatory| Description                                                        |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | No  | Checkable status of the target component.<br>**true**: checkable.<br>**false**: not checkable.<br> Default value: **false**|

**Return value**

| Type      | Description                                        |
| ---------- | -------------------------------------------- |
| [On](#on9) | **On** object that matches the checkable status attribute of the target component.|

**Example**

```js
let on = ON.checkable(true); // Use the static constructor ON to create an On object and specify the checkable status attribute of the target component.
```

## Component<sup>9+</sup>

Represents a component on the UI and provides APIs for obtaining component attributes, clicking a component, scrolling to search for a component, and text injection.

All APIs provided in this class use a promise to return the result and must be invoked using **await**.

### click<sup>9+</sup>

click(): Promise\<void>

Clicks this component.

**System capability**: SystemCapability.Test.UiTest

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                                |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    await button.click();
}
```

### doubleClick<sup>9+</sup>

doubleClick(): Promise\<void>

Double-clicks this component.

**System capability**: SystemCapability.Test.UiTest

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                                |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    await button.doubleClick();
}
```

### longClick<sup>9+</sup>

longClick(): Promise\<void>

Long-clicks this component.

**System capability**: SystemCapability.Test.UiTest

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                                |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    await button.longClick();
}
```

### getId<sup>9+</sup>

getId(): Promise\<string>

Obtains the ID of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type            | Description                           |
| ---------------- | ------------------------------- |
| Promise\<string> | Promise used to return the ID of the component.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                                |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let num = await button.getId();
}
```

### getText<sup>9+</sup>

getText(): Promise\<string>

Obtains the text information of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type            | Description                             |
| ---------------- | --------------------------------- |
| Promise\<string> | Promise used to return the text information of the component.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let text = await button.getText();
}
```

### getType<sup>9+</sup>

getType(): Promise\<string>

Obtains the type of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type            | Description                         |
| ---------------- | ----------------------------- |
| Promise\<string> | Promise used to return the type of the component.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let type = await button.getType();
}
```

### getBoundsCenter<sup>9+</sup>

getBoundsCenter(): Promise\<Point>

Obtains the information about the center of the bounding box around this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type                      | Description                                           |
| -------------------------- | ----------------------------------------------- |
| Promise\<[Point](#point9)> | Promise used to return the information about the center of the bounding box around the component.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let point = await button.getBoundsCenter();
}
```

### isClickable<sup>9+</sup>

isClickable(): Promise\<boolean>

Obtains the clickable status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                                        |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | Promise used to return the result. The value **true** means that the component is clickable, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    if(await button.isClickable()) {
        console.info('This button can be Clicked');
    } else {
        console.info('This button can not be Clicked');
    }
}
```

### isLongClickable<sup>9+</sup>

isLongClickable(): Promise\<boolean> 

Obtains the long-clickable status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                                        |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | Promise used to return the long-clickable status of the component. The value **true** means that the component is long-clickable, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    if(await button.isLongClickable()) {
        console.info('This button can longClick');
    } else {
        console.info('This button can not longClick');
    }
}
```

### isChecked<sup>9+</sup>

isChecked(): Promise\<boolean>

Obtains the checked status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                                        |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | Promise used to return the checked status of the component. The value **true** means that the component is checked, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let checkBox = await driver.findComponent(ON.type('Checkbox'));
    if(await checkBox.isChecked()) {
        console.info('This checkBox is checked');
    } else {
        console.info('This checkBox is not checked');
    }
}
```

### isCheckable<sup>9+</sup>

isCheckable(): Promise\<boolean>

Obtains the checkable status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                                        |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | Promise used to return the checkable status of the component. The value **true** means that the component is checkable, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                                |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let checkBox = await driver.findComponent(ON.type('Checkbox'));
    if(await checkBox.isCheckable()) {
        console.info('This checkBox is checkable');
    } else {
        console.info('This checkBox is not checkable');
    }
}
```

### isScrollable<sup>9+</sup>

isScrollable(): Promise\<boolean>

Obtains the scrollable status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                                        |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | Promise used to return the result. The value **true** means that the component is scrollable, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let scrollBar = await driver.findComponent(ON.scrollable(true));
    if(await scrollBar.isScrollable()) {
        console.info('This scrollBar can be operated');
    } else {
        console.info('This scrollBar can not be operated');
    }
}
```


### isEnabled<sup>9+</sup>

isEnabled(): Promise\<boolean>

Obtains the enabled status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                                      |
| ----------------- | ---------------------------------------------------------- |
| Promise\<boolean> | Promise used to return the result. The value **true** means that the component is enabled, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    if(await button.isEnabled()) {
        console.info('This button can be operated');
    } else {
        console.info('This button can not be operated');
    }
}

```

### isFocused<sup>9+</sup>

isFocused(): Promise\<boolean>

Obtains the focused status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                                        |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | Promise used to return the focused status of the component. The value **true** means that the component is focused, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    if(await button.isFocused()) {
        console.info('This button is focused');
    } else {
        console.info('This button is not focused');
	}
}
```

### isSelected<sup>9+</sup>

isSelected(): Promise\<boolean>

Obtains the selected status of this component.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type             | Description                                               |
| ----------------- | --------------------------------------------------- |
| Promise\<boolean> | Promise used to return the result. The value **true** means that the component is selected, and **false** means the opposite.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    if(await button.isSelected()) {
        console.info('This button is selected');
	} else {
        console.info('This button is not selected');
    }
}
```

### inputText<sup>9+</sup>

inputText(text: string): Promise\<void>

Enters text into this component (available for text boxes).

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description                                    |
| ------ | ------ | ---- | ---------------------------------------- |
| text   | string | Yes  | Text to enter, which can contain lower English and numbers |

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let text = await driver.findComponent(ON.text('hello world'));
    await text.inputText('123');
}
```

### clearText<sup>9+</sup>

clearText(): Promise\<void>

Clears text in this component. This API is applicable to text boxes.

**System capability**: SystemCapability.Test.UiTest

**Error codes**

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let text = await driver.findComponent(ON.text('hello world'));
    await text.clearText();
}
```

### scrollSearch<sup>9+</sup>

scrollSearch(on: On): Promise\<Component>

Scrolls on this component to search for the target component. This API is applicable to components that support scrolling.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type      | Mandatory| Description                |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | Yes  | Attributes of the target component.|

**Return value**

| Type                              | Description                                 |
| ---------------------------------- | ------------------------------------- |
| Promise\<[Component](#component9)> | Promise used to return the target component.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('Scroll'));
    let button = await scrollBar.scrollSearch(ON.text('next page'));
}
```

### scrollToTop<sup>9+</sup>

scrollToTop(speed?: number): Promise\<void>

Scrolls to the top of this component. This API is applicable to components that support scrolling.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| speed  | number | No  | Scroll speed, in pixel/s. The value ranges from 200 to 15000. If the set value is not in the range, the default value 600 is used.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let scrollBar = await driver.findComponent(ON.type('Scroll'));
    await scrollBar.scrollToTop();
}
```

### scrollToBottom<sup>9+</sup>

scrollToBottom(speed?: number): Promise\<void>

Scrolls to the bottom of this component. This API is applicable to components that support scrolling.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| speed  | number | No  | Scroll speed, in pixel/s. The value ranges from 200 to 15000. If the set value is not in the range, the default value 600 is used.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let scrollBar = await driver.findComponent(ON.type('Scroll'));
    await scrollBar.scrollToBottom();
}
```

## Driver<sup>9+</sup>

The **Driver** class is the main entry to the UiTest framework. It provides APIs for features such as component matching/search, key injection and coordinate clicking/sliding.

All APIs provided by this class, except for **Driver.create()**, use a promise to return the result and must be invoked using **await**.

### create<sup>9+</sup>

static create(): Driver

Creates a **Driver** object and returns the object created. This API is a static API.

**System capability**: SystemCapability.Test.UiTest

**Return value**

| Type| Description          |
| -------- | ---------------------- |
| Driver   | **Driver** object created.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message          |
| -------- | ------------------ |
| 17000001 | if the test framework failed to initialize. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
}
```

### delayMs<sup>9+</sup>

delayMs(duration: number): Promise\<void>

Delays this **Driver** object within the specified duration.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name  | Type  | Mandatory| Description                  |
| -------- | ------ | ---- | ---------------------- |
| duration | number | Yes  | Duration of time, in ms.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.delayMs(1000);
}
```

### findComponent<sup>9+</sup>

findComponent(on: On): Promise\<Component>

Searches this **Driver** object for the target component that matches the given attributes.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type      | Mandatory| Description                |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | Yes  | Attributes of the target component.|

**Return value**

| Type                              | Description                             |
| ---------------------------------- | --------------------------------- |
| Promise\<[Component](#component9)> | Promise used to return the found component.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.text('next page'));
}
```

### findComponents<sup>9+</sup>

findComponents(on: On): Promise\<Array\<Component>>

Searches this **Driver** object for all components that match the given attributes.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type      | Mandatory| Description                |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | Yes  | Attributes of the target component.|

**Return value**

| Type                                      | Description                                   |
| ------------------------------------------ | --------------------------------------- |
| Promise\<Array\<[Component](#component9)>> | Promise used to return a list of found components.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    let buttonList = await driver.findComponents(ON.text('next page'));
}
```

### assertComponentExist<sup>9+</sup>

assertComponentExist(on: On): Promise\<void>

Asserts that a component that matches the given attributes exists on the current page.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type      | Mandatory| Description                |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | Yes  | Attributes of the target component.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000003 | if the assertion failed.    |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.assertComponentExist(ON.text('next page'));
}
```

### pressBack<sup>9+</sup>

pressBack(): Promise\<void>

Presses the Back button on this **Driver** object.

**System capability**: SystemCapability.Test.UiTest

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.pressBack();
}
```


### click<sup>9+</sup>

click(x: number, y: number): Promise\<void>

Clicks a specific point of this **Driver** object based on the given coordinates.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description                                  |
| ------ | ------ | ---- | -------------------------------------- |
| x      | number | Yes  | X-coordinate of the target point.|
| y      | number | Yes  | Y-coordinate of the target point.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.click(100,100);
}
```

### doubleClick<sup>9+</sup>

doubleClick(x: number, y: number): Promise\<void>

Double-clicks a specific point of this **Driver** object based on the given coordinates.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description                                  |
| ------ | ------ | ---- | -------------------------------------- |
| x      | number | Yes  | X-coordinate of the target point.|
| y      | number | Yes  | Y-coordinate of the target point.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.doubleClick(100,100);
}
```

### longClick<sup>9+</sup>

longClick(x: number, y: number): Promise\<void>

Long-clicks a specific point of this **Driver** object based on the given coordinates.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description                                  |
| ------ | ------ | ---- | -------------------------------------- |
| x      | number | Yes  | X-coordinate of the target point.|
| y      | number | Yes  | Y-coordinate of the target point.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.longClick(100,100);
}
```

### swipe<sup>9+</sup>

swipe(startx: number, starty: number, endx: number, endy: number, speed?: number): Promise\<void>

Swipes on this **Driver** object from the given start point to the given end point.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| startx | number | Yes  | X-coordinate of the start point.                      |
| starty | number | Yes  | Y-coordinate of the start point.                      |
| endx   | number | Yes  | X-coordinate of the end point.                      |
| endy   | number | Yes  | Y-coordinate of the end point.                      |
| speed  | number | No  | Scroll speed, in pixel/s. The value ranges from 200 to 15000. If the set value is not in the range, the default value 600 is used.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.swipe(100,100,200,200,600);
}
```

### fling<sup>9+</sup>

fling(from: Point, to: Point, stepLen: number, speed: number): Promise\<void>

Simulates a fling operation on the screen.

**System capability**: SystemCapability.Test.UiTest

**Parameters**

| Name | Type            | Mandatory| Description                                                        |
| ------- | ---------------- | ---- | ------------------------------------------------------------ |
| from    | [Point](#point9) | Yes  | Coordinates of the point where the finger touches the screen.                                  |
| to      | [Point](#point9) | Yes  | Coordinates of the point where the finger leaves the screen.                                    |
| stepLen | number           | Yes  | Fling step length, in pixels.                                    |
| speed   | number           | Yes  | Fling speed, in pixel/s. The value ranges from 200 to 40000. If the set value is not in the range, the default value 600 is used.|

**Error codes**

For details about the error codes, see [UiTest Error Codes](../errorcodes/errorcode-uitest.md).

| ID| Error Message                              |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**Example**

```js
async function demo() {
    let driver = Driver.create();
    await driver.fling({x: 500, y: 480},{x: 450, y: 480},5,600);
}
```
