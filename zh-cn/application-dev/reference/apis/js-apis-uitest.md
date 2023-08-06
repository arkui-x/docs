# @ohos.UiTest

UiTest提供模拟UI操作的能力，供开发者在测试场景使用，主要支持如点击、双击、长按、滑动等UI操作能力。

该模块提供以下功能：

- [On<sup>9+</sup>](#on9)：提供控件特征描述能力，用于控件筛选匹配查找。
- [Component<sup>9+</sup>](#component9)：代表UI界面上的指定控件，提供控件属性获取，控件点击，滑动查找，文本注入等能力。
- [Driver<sup>9+</sup>](#driver9)：入口类，提供控件匹配/查找，按键注入，坐标点击/滑动，截图等能力。

>本模块首批接口从API version 8开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块 

```js
import {Component, Driver, ON, MatchPattern, PointerMatrix} from '@ohos.UiTest';
```

## MatchPattern

控件属性支持的匹配模式。

**系统能力**：SystemCapability.Test.UiTest

| 名称        | 值   | 说明           |
| ----------- | ---- | -------------- |
| EQUALS      | 0    | 等于给定值。   |
| CONTAINS    | 1    | 包含给定值。   |
| STARTS_WITH | 2    | 以给定值开始。 |
| ENDS_WITH   | 3    | 以给定值结束。 |

## Point<sup>9+</sup>

坐标点信息。

**系统能力**：SystemCapability.Test.UiTest

| 名称 | 类型   | 可读 | 可写 | 说明             |
| ---- | ------ | ---- | ---- | ---------------- |
| x    | number | 是   | 否   | 坐标点的横坐标。 |
| y    | number | 是   | 否   | 坐标点的纵坐标。 |



## On<sup>9+</sup>

UiTest框架在API 9中，通过On类提供了丰富的控件特征描述API，用于进行控件筛选来匹配/查找出目标控件。<br>
On提供的API能力具有以下几个特点:<br>1、支持单属性匹配和多属性组合匹配，例如同时指定目标控件text和id。<br>2、控件属性支持多种匹配模式。<br>On类提供的所有API均为同步接口，建议使用者通过静态构造器ON来链式创建On对象。

```js
ON.text('123').type('button');
```

### text<sup>9+</sup>

text(txt: string, pattern?: MatchPattern): On

指定目标控件文本属性，支持多种匹配模式，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名  | 类型                          | 必填 | 说明                                                |
| ------- | ----------------------------- | ---- | --------------------------------------------------- |
| txt     | string                        | 是   | 指定控件文本，用于匹配目标控件文本。                |
| pattern | [MatchPattern](#matchpattern) | 否   | 指定的文本匹配模式，默认为[EQUALS](#matchpattern)。 |

**返回值：**

| 类型       | 说明                               |
| ---------- | ---------------------------------- |
| [On](#on9) | 返回指定目标控件文本属性的On对象。 |

**示例：**

```js
let on = ON.text('123'); // 使用静态构造器ON创建On对象，指定目标控件的text属性。
```


### id<sup>9+</sup>

id(id: string): On

指定目标控件id属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明             |
| ------ | ------ | ---- | ---------------- |
| id     | string | 是   | 指定控件的id值。 |

**返回值：**

| 类型       | 说明                             |
| ---------- | -------------------------------- |
| [On](#on9) | 返回指定目标控件id属性的On对象。 |

**示例：**

```js
let on = ON.id('123'); // 使用静态构造器ON创建On对象，指定目标控件的id属性。
```


### type<sup>9+</sup>

type(tp: string): On

指定目标控件的控件类型属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| tp     | string | 是   | 指定控件类型。 |

**返回值：**

| 类型       | 说明                                     |
| ---------- | ---------------------------------------- |
| [On](#on9) | 返回指定目标控件的控件类型属性的On对象。 |

**示例：**

```js
let on = ON.type('button'); // 使用静态构造器ON创建On对象，指定目标控件的控件类型属性。
```


### clickable<sup>9+</sup>

clickable(b?: boolean): On

指定目标控件的可点击状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                         |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | 否   | 指定控件可点击状态，true：可点击，false：不可点击。默认为true。 |

**返回值：**

| 类型       | 说明                                       |
| ---------- | ------------------------------------------ |
| [On](#on9) | 返回指定目标控件的可点击状态属性的On对象。 |

**示例：**

```js
let on = ON.clickable(true); // 使用静态构造器ON创建On对象，指定目标控件的可点击状态属性。
```

### longClickable<sup>9+</sup>

longClickable(b?: boolean): On

指定目标控件的可长按点击状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                         |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | 否   | 指定控件可长按点击状态，true：可长按点击，false：不可长按点击。默认为true。 |

**返回值：**

| 类型       | 说明                                           |
| ---------- | ---------------------------------------------- |
| [On](#on9) | 返回指定目标控件的可长按点击状态属性的On对象。 |

**示例：**

```js
let on = ON.longClickable(true); // 使用静态构造器ON创建On对象，指定目标控件的可长按点击状态属性。
```


### scrollable<sup>9+</sup>

scrollable(b?: boolean): On

指定目标控件的可滑动状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                        |
| ------ | ------- | ---- | ----------------------------------------------------------- |
| b      | boolean | 否   | 控件可滑动状态，true：可滑动，false：不可滑动。默认为true。 |

**返回值：**

| 类型       | 说明                                       |
| ---------- | ------------------------------------------ |
| [On](#on9) | 返回指定目标控件的可滑动状态属性的On对象。 |

**示例：**

```js
let on = ON.scrollable(true); // 使用静态构造器ON创建On对象，指定目标控件的可滑动状态属性。
```

### enabled<sup>9+</sup>

enabled(b?: boolean): On

指定目标控件的使能状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                      |
| ------ | ------- | ---- | --------------------------------------------------------- |
| b      | boolean | 否   | 指定控件使能状态，true：使能，false：未使能。默认为true。 |

**返回值：**

| 类型       | 说明                                     |
| ---------- | ---------------------------------------- |
| [On](#on9) | 返回指定目标控件的使能状态属性的On对象。 |

**示例：**

```js
let on = ON.enabled(true); // 使用静态构造器ON创建On对象，指定目标控件的使能状态属性。
```

### focused<sup>9+</sup>

focused(b?: boolean): On

指定目标控件的获焦状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                  |
| ------ | ------- | ---- | ----------------------------------------------------- |
| b      | boolean | 否   | 控件获焦状态，true：获焦，false：未获焦。默认为true。 |

**返回值：**

| 类型       | 说明                                     |
| ---------- | ---------------------------------------- |
| [On](#on9) | 返回指定目标控件的获焦状态属性的On对象。 |

**示例：**

```js
let on = ON.focused(true); // 使用静态构造器ON创建On对象，指定目标控件的获焦状态属性。
```

### selected<sup>9+</sup>

selected(b?: boolean): On

指定目标控件的被选中状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                         |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | 否   | 指定控件被选中状态，true：被选中，false：未被选中。默认为true。 |

**返回值：**

| 类型       | 说明                                       |
| ---------- | ------------------------------------------ |
| [On](#on9) | 返回指定目标控件的被选中状态属性的On对象。 |

**示例：**

```js
let on = ON.selected(true); // 使用静态构造器ON创建On对象，指定目标控件的被选中状态属性。
```

### checked<sup>9+</sup>

checked(b?: boolean): On

指定目标控件的被勾选状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                         |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | 否   | 指定控件被勾选状态，true：被勾选，false：未被勾选。默认为false。 |

**返回值：**

| 类型       | 说明                                       |
| ---------- | ------------------------------------------ |
| [On](#on9) | 返回指定目标控件的被勾选状态属性的On对象。 |

**示例：**

```js
let on = ON.checked(true); // 使用静态构造器ON创建On对象，指定目标控件的被勾选状态属性
```

### checkable<sup>9+</sup>

checkable(b?: boolean): On

指定目标控件能否被勾选状态属性，返回On对象自身。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                         |
| ------ | ------- | ---- | ------------------------------------------------------------ |
| b      | boolean | 否   | 指定控件能否被勾选状态，true：能被勾选，false：不能被勾选。默认为false。 |

**返回值：**

| 类型       | 说明                                         |
| ---------- | -------------------------------------------- |
| [On](#on9) | 返回指定目标控件能否被勾选状态属性的On对象。 |

**示例：**

```js
let on = ON.checkable(true); // 使用静态构造器ON创建On对象，指定目标控件的能否被勾选状态属性。
```

## Component<sup>9+</sup>

UiTest框架在API9中，Component类代表了UI界面上的一个控件，提供控件属性获取，控件点击，滑动查找，文本注入等API。
该类提供的所有方法都使用Promise方式作为异步方法，需使用await调用。

### click<sup>9+</sup>

click(): Promise\<void>

控件对象进行点击操作。

**系统能力**：SystemCapability.Test.UiTest

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    await button.click();
}
```

### doubleClick<sup>9+</sup>

doubleClick(): Promise\<void>

控件对象进行双击操作。

**系统能力**：SystemCapability.Test.UiTest

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    await button.doubleClick();
}
```

### longClick<sup>9+</sup>

longClick(): Promise\<void>

控件对象进行长按操作。

**系统能力**：SystemCapability.Test.UiTest

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    await button.longClick();
}
```

### getId<sup>9+</sup>

getId(): Promise\<string>

获取控件对象的id值。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型             | 说明                            |
| ---------------- | ------------------------------- |
| Promise\<string> | 以Promise形式返回的控件的id值。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let num = await button.getId();
}
```

### getText<sup>9+</sup>

getText(): Promise\<string>

获取控件对象的文本信息。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型             | 说明                              |
| ---------------- | --------------------------------- |
| Promise\<string> | 以Promise形式返回控件的文本信息。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let text = await button.getText();
}
```

### getType<sup>9+</sup>

getType(): Promise\<string>

获取控件对象的控件类型。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型             | 说明                          |
| ---------------- | ----------------------------- |
| Promise\<string> | 以Promise形式返回控件的类型。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let type = await button.getType();
}
```

### getBoundsCenter<sup>9+</sup>

getBoundsCenter(): Promise\<Point>

获取控件对象所占区域的中心点信息。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型                       | 说明                                            |
| -------------------------- | ----------------------------------------------- |
| Promise\<[Point](#point9)> | 以Promise形式返回控件对象所占区域的中心点信息。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.type('button'));
    let point = await button.getBoundsCenter();
}
```

### isClickable<sup>9+</sup>

isClickable(): Promise\<boolean>

获取控件对象可点击属性。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回控件对象是否可点击，true：可点击，false：不可点击。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

获取控件对象可长按点击属性。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回控件对象是否可安装点击，true：可长按点击，false：不可长按点击。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

获取控件对象被勾选状态。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回控件对象被勾选状态，true：被勾选，false：未被勾选。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

获取控件对象能否被勾选属性。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回控件对象能否可被勾选属性，true：可被勾选，false：不可被勾选。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

获取控件对象可滑动属性。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回控件对象是否可滑动，true：可滑动，false：不可滑动。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

获取控件使能状态。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                       |
| ----------------- | ---------------------------------------------------------- |
| Promise\<boolean> | 以Promise形式返回控件使能状态，true：使能，false：未使能。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

判断控件对象获焦状态。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回控件对象获焦状态，true：获焦，false：未获焦。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

获取控件对象被选中状态。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型              | 说明                                                |
| ----------------- | --------------------------------------------------- |
| Promise\<boolean> | 控件对象被选中状态，true：被选中，false：未被选中。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

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

向控件中输入文本(适用于文本框控件)。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| text   | string | 是   | 输入的文本信息，当前输入支持小写字母和数字。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let text = await driver.findComponent(ON.text('hello world'));
    await text.inputText('123');
}
```

### clearText<sup>9+</sup>

clearText(): Promise\<void>

清除控件的文本信息(适用于文本框控件)。

**系统能力**：SystemCapability.Test.UiTest

**错误码：**

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let text = await driver.findComponent(ON.text('hello world'));
    await text.clearText();
}
```

### scrollSearch<sup>9+</sup>

scrollSearch(on: On): Promise\<Component>

在控件上滑动查找目标控件(适用支持滑动的控件)。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型       | 必填 | 说明                 |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | 是   | 目标控件的属性要求。 |

**返回值：**

| 类型                               | 说明                                  |
| ---------------------------------- | ------------------------------------- |
| Promise\<[Component](#component9)> | 以Promise形式返回找到的目标控件对象。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let scrollBar = await driver.findComponent(ON.type('Scroll'));
    let button = await scrollBar.scrollSearch(ON.text('next page'));
}
```

### scrollToTop<sup>9+</sup>

scrollToTop(speed?: number): Promise\<void>

在控件上滑动到顶部(适用支持滑动的控件)。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| speed  | number | 否   | 滑动速率，范围：200-15000，不在范围内设为默认值为600，单位：像素点/秒。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let scrollBar = await driver.findComponent(ON.type('Scroll'));
    await scrollBar.scrollToTop();
}
```

### scrollToBottom<sup>9+</sup>

scrollToBottom(speed?: number): Promise\<void>

在控件上滑动到底部(适用支持滑动的控件)。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| speed  | number | 否   | 滑动速率，范围：200-15000，不在范围内设为默认值为600，单位：像素点/秒。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000004 | if the component is invisible or destroyed.           |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let scrollBar = await driver.findComponent(ON.type('Scroll'));
    await scrollBar.scrollToBottom();
}
```

## Driver<sup>9+</sup>

Driver类为uitest测试框架的总入口，提供控件匹配/查找，按键注入，坐标点击/滑动等能力。
该类提供的方法除Driver.create()以外的所有方法都使用Promise方式作为异步方法，需使用await方式调用。

### create<sup>9+</sup>

static create(): Driver

静态方法，构造一个Driver对象，并返回该对象。

**系统能力**：SystemCapability.Test.UiTest

**返回值：**

| 类型 | 说明           |
| -------- | ---------------------- |
| Driver   | 返回构造的Driver对象。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息           |
| -------- | ------------------ |
| 17000001 | if the test framework failed to initialize. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
}
```

### delayMs<sup>9+</sup>

delayMs(duration: number): Promise\<void>

Driver对象在给定的时间内延时。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名   | 类型   | 必填 | 说明                   |
| -------- | ------ | ---- | ---------------------- |
| duration | number | 是   | 给定的时间，单位：ms。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.delayMs(1000);
}
```

### findComponent<sup>9+</sup>

findComponent(on: On): Promise\<Component>

在Driver对象中，根据给出的目标控件属性要求查找目标控件。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型       | 必填 | 说明                 |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | 是   | 目标控件的属性要求。 |

**返回值：**

| 类型                               | 说明                              |
| ---------------------------------- | --------------------------------- |
| Promise\<[Component](#component9)> | 以Promise形式返回找到的控件对象。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let button = await driver.findComponent(ON.text('next page'));
}
```

### findComponents<sup>9+</sup>

findComponents(on: On): Promise\<Array\<Component>>

在Driver对象中，根据给出的目标控件属性要求查找出所有匹配控件，以列表保存。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型       | 必填 | 说明                 |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | 是   | 目标控件的属性要求。 |

**返回值：**

| 类型                                       | 说明                                    |
| ------------------------------------------ | --------------------------------------- |
| Promise\<Array\<[Component](#component9)>> | 以Promise形式返回找到的控件对象的列表。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    let buttonList = await driver.findComponents(ON.text('next page'));
}
```

### assertComponentExist<sup>9+</sup>

assertComponentExist(on: On): Promise\<void>

断言API，用于断言当前界面是否存在满足给出的目标属性的控件。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型       | 必填 | 说明                 |
| ------ | ---------- | ---- | -------------------- |
| on     | [On](#on9) | 是   | 目标控件的属性要求。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |
| 17000003 | if the assertion failed.    |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.assertComponentExist(ON.text('next page'));
}
```

### pressBack<sup>9+</sup>

pressBack(): Promise\<void>

Driver对象进行点击BACK键的操作。

**系统能力**：SystemCapability.Test.UiTest

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.pressBack();
}
```


### click<sup>9+</sup>

click(x: number, y: number): Promise\<void>

Driver对象采取如下操作：在目标坐标点单击。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明                                   |
| ------ | ------ | ---- | -------------------------------------- |
| x      | number | 是   | 以number的形式传入目标点的横坐标信息。 |
| y      | number | 是   | 以number的形式传入目标点的纵坐标信息。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.click(100,100);
}
```

### doubleClick<sup>9+</sup>

doubleClick(x: number, y: number): Promise\<void>

Driver对象采取如下操作：在目标坐标点双击。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明                                   |
| ------ | ------ | ---- | -------------------------------------- |
| x      | number | 是   | 以number的形式传入目标点的横坐标信息。 |
| y      | number | 是   | 以number的形式传入目标点的纵坐标信息。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.doubleClick(100,100);
}
```

### longClick<sup>9+</sup>

longClick(x: number, y: number): Promise\<void>

Driver对象采取如下操作：在目标坐标点长按。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明                                   |
| ------ | ------ | ---- | -------------------------------------- |
| x      | number | 是   | 以number的形式传入目标点的横坐标信息。 |
| y      | number | 是   | 以number的形式传入目标点的纵坐标信息。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.longClick(100,100);
}
```

### swipe<sup>9+</sup>

swipe(startx: number, starty: number, endx: number, endy: number, speed?: number): Promise\<void>

Driver对象采取如下操作：从起始坐标点滑向目的坐标点。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| startx | number | 是   | 以number的形式传入起始点的横坐标信息。                       |
| starty | number | 是   | 以number的形式传入起始点的纵坐标信息。                       |
| endx   | number | 是   | 以number的形式传入目的点的横坐标信息。                       |
| endy   | number | 是   | 以number的形式传入目的点的纵坐标信息。                       |
| speed  | number | 否   | 滑动速率，范围：200-15000，不在范围内设为默认值为600，单位：像素点/秒。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.swipe(100,100,200,200,600);
}
```

### fling<sup>9+</sup>

fling(from: Point, to: Point, stepLen: number, speed: number): Promise\<void>

模拟手指滑动后脱离屏幕的快速滑动操作。

**系统能力**：SystemCapability.Test.UiTest

**参数：**

| 参数名  | 类型             | 必填 | 说明                                                         |
| ------- | ---------------- | ---- | ------------------------------------------------------------ |
| from    | [Point](#point9) | 是   | 手指接触屏幕的起始点坐标。                                   |
| to      | [Point](#point9) | 是   | 手指离开屏幕时的坐标点。                                     |
| stepLen | number           | 是   | 间隔距离，单位：像素点。                                     |
| speed   | number           | 是   | 滑动速率，范围：200-40000，不在范围内设为默认值为600，单位：像素点/秒。 |

**错误码：**

以下错误码的详细介绍请参见[uitest测试框架错误码](../errorcodes/errorcode-uitest.md)。

| 错误码ID | 错误信息                               |
| -------- | ---------------------------------------- |
| 17000002 | if the async function was not called with await. |

**示例：**

```js
async function demo() {
    let driver = Driver.create();
    await driver.fling({x: 500, y: 480},{x: 450, y: 480},5,600);
}
```
