# @ohos.display (屏幕属性)

屏幕属性提供管理显示设备的一些基础能力，包括获取默认显示设备的信息，获取所有显示设备的信息以及监听显示设备的插拔行为。

> **说明：**
>
> 本模块首批接口从API version 7开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import display from '@ohos.display';
```

## Orientation<sup>10+</sup>

显示设备当前显示的方向枚举。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称 | 值 | 说明 |
| -------- | -------- | -------- |
| PORTRAIT | 0 | 表示设备当前以竖屏方式显示。|
| LANDSCAPE | 1 | 表示设备当前以横屏方式显示。 |
| PORTRAIT_INVERTED | 2 | 表示设备当前以反向竖屏方式显示。|
| LANDSCAPE_INVERTED | 3 | 表示设备当前以反向横屏方式显示。|

## Rect<sup>9+</sup>

矩形区域。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称   | 类型 | 可读 | 可写 | 说明               |
| ------ | -------- | ---- | ---- | ------------------ |
| left   | number   | 是   | 是   | 矩形区域的左边界，单位为像素，该参数应为整数。 |
| top    | number   | 是   | 是   | 矩形区域的上边界，单位为像素，该参数应为整数。 |
| width  | number   | 是   | 是   | 矩形区域的宽度，单位为像素，该参数应为整数。   |
| height | number   | 是   | 是   | 矩形区域的高度，单位为像素，该参数应为整数。   |

## FoldStatus<sup>20+</sup>

当前可折叠设备的折叠状态枚举。

**系统能力：** SystemCapability.Window.SessionManager

| 名称 | 值 | 说明 |
| -------- | -------- | -------- |
| FOLD_STATUS_UNKNOWN | 0 | 表示设备当前折叠状态未知。|
| FOLD_STATUS_EXPANDED | 1 | 表示设备当前折叠状态为完全展开。|
| FOLD_STATUS_FOLDED | 2 | 表示设备当前折叠状态为折叠。该状态在现有折叠屏接口中暂时无法获取|
| FOLD_STATUS_HALF_FOLDED | 3 | 表示设备当前折叠状态为半折叠。半折叠指完全展开和折叠之间的状态。|

## display.getDefaultDisplaySync<sup>9+</sup>

getDefaultDisplaySync(): Display

获取当前默认的display对象。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**返回值：**

| 类型                           | 说明                                           |
| ------------------------------| ----------------------------------------------|
| [Display](#display) | 返回默认的display对象。 |

**错误码：**

以下错误码的详细介绍请参见[屏幕错误码](../errorcodes/errorcode-display.md)。

| 错误码ID | 错误信息 |
| ------- | ----------------------- |
| 1400001 | Invalid display or screen. |

**示例：**

```ts
import display from '@ohos.display';

let displayClass: display.Display | null = null;
try {
  displayClass = display.getDefaultDisplaySync();
} catch (exception) {
  console.error('Failed to obtain the default display object. Code: ' + JSON.stringify(exception));
}
```

## Display
屏幕实例。描述display对象的属性和方法。

下列API示例中都需先使用[getDefaultDisplaySync()](#displaygetdefaultdisplaysync9)中的任一方法获取到Display实例，再通过此实例调用对应方法。

### 属性

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称 | 类型 | 可读 | 可写 | 说明                                                                                                            |
| -------- | -------- | -------- | -------- |---------------------------------------------------------------------------------------------------------------|
| id | number | 是 | 否 | 显示设备的id号，该参数应为整数。                                                                                             |
| width | number | 是 | 否 | 显示设备的宽度，单位为像素，该参数应为整数。                                                                                        |
| height | number | 是 | 否 | 显示设备的高度，单位为像素，该参数应为整数。                                                                                        |
| orientation<sup>10+</sup> | [Orientation](#orientation10) | 是 | 否 | 表示屏幕当前显示的方向。                                                                                                  |
| xDPI<sup>20+</sup> | number | 是 | 否 | x方向中每英寸屏幕的确切物理像素值，该参数为浮点数。                                                                                                  |
| yDPI<sup>20+</sup> | number | 是 | 否 | y方向中每英寸屏幕的确切物理像素值，该参数为浮点数。                                                                                                  |

## display.on('add'|'remove'|'change')<sup>20+</sup>

on(type: 'add'|'remove'|'change', callback: Callback&lt;number&gt;): void

开启显示设备变化的监听。add和remove暂不支持。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**支持平台：** Android

**参数：**

| 参数名 | 类型 | 必填 | 说明                                                                                                                              |
| -------- | -------- | -------- |---------------------------------------------------------------------------------------------------------------------------------|
| type | string | 是 | 监听事件。<br/>- type为"add"，表示增加显示设备事件。例如：插入显示器。<br/>- type为"remove"，表示移除显示设备事件。例如：移除显示器。<br/>- type为"change"，表示改变显示设备事件。例如：显示器方向改变。 |
| callback | Callback&lt;number&gt; | 是 | 回调函数。返回监听到的显示设备的id，该参数应为整数。                                                                                                     |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | ----------------------- |
| 401     | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types.|

**示例：**

```ts
import { Callback } from '@kit.BasicServicesKit';

let callback: Callback<number> = (data: number) => {
  console.info('Listening enabled. Data: ' + JSON.stringify(data));
};

display.on("add", callback);
```

## display.off('add'|'remove'|'change')<sup>20+</sup>

off(type: 'add'|'remove'|'change', callback?: Callback&lt;number&gt;): void

关闭显示设备变化的监听。add和remove暂不支持。

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

**支持平台：** Android

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| type | string | 是 | 监听事件。<br/>- type为"add"，表示增加显示设备事件。例如：插入显示器。<br/>- type为"remove"，表示移除显示设备事件。例如：移除显示器。<br/>- type为"change"，表示改变显示设备事件。例如：显示器方向改变。 |
| callback | Callback&lt;number&gt; | 否 | 需要取消注册的回调函数。若无此参数，则取消注册当前type类型事件监听的所有回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | ----------------------- |
| 401     | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types.|

**示例：**

```ts

// 如果通过on注册多个callback，同时关闭所有callback监听
display.off("remove");

let callback: Callback<number> = (data: number) => {
  console.info('Succeeded in unregistering the callback for display remove. Data: ' + JSON.stringify(data))
};
// 关闭传入的callback监听
display.off('remove', callback);
```

## display.isFoldable<sup>20+</sup>
isFoldable(): boolean

检查设备是否可折叠。支持在展开或半折叠状态下获取判断值，完全折叠场景下无法获取判断值。

**系统能力：** SystemCapability.Window.SessionManager

**支持平台：** Android

**接口依赖：** 修改.arkui-x\android目录相关文件配置:
<br/>- gradle.properties中android.useAndroidX=true
<br/>- APP目录下build.gradle中增加androidx库依赖
<br/>-- implementation "androidx.window:window-java:1.0.0"
<br/>-- implementation "androidx.core:core:1.8.0"

**返回值：**

| 类型 | 说明 |
| ----------------------------------------------- | ------------------------------------------------------- |
| boolean | boolean对象，返回当前设备是否可折叠的结果。false表示不可折叠，true表示可折叠。|

**错误码：**

以下错误码的详细介绍请参见[屏幕错误码](../errorcodes/errorcode-display.md)。

| 错误码ID | 错误信息 |
| ------- | ----------------------- |
| 1400003 | This display manager service works abnormally. |

**示例：**

```ts
import { display } from '@kit.ArkUI';

let ret: boolean = false;
ret = display.isFoldable();
```

## display.getFoldStatus<sup>20+</sup>
getFoldStatus(): FoldStatus

获取可折叠设备的当前折叠状态。支持在展开或半折叠状态下获取状态值，完全折叠场景下无法获取状态值。

**系统能力：** SystemCapability.Window.SessionManager

**支持平台：** Android

**接口依赖：** 修改.arkui-x\android目录相关文件配置:
<br/>- gradle.properties中android.useAndroidX=true
<br/>- APP目录下build.gradle中增加androidx库依赖
<br/>-- implementation "androidx.window:window-java:1.0.0"
<br/>-- implementation "androidx.core:core:1.8.0"

**返回值：**

| 类型 | 说明 |
| ----------------------------------------------- | ------------------------------------------------------- |
| [FoldStatus](#foldstatus20) | FoldStatus对象，返回当前可折叠设备的折叠状态。 |

**错误码：**

以下错误码的详细介绍请参见[屏幕错误码](../errorcodes/errorcode-display.md)。

| 错误码ID | 错误信息 |
| ------- | ----------------------- |
| 1400003 | This display manager service works abnormally. |

**示例：**

```ts
import { display } from '@kit.ArkUI';

let data: display.FoldStatus = display.getFoldStatus();
console.info('Succeeded in obtaining fold status. Data: ' + JSON.stringify(data));
```

## display.on('foldStatusChange')<sup>20+</sup>

on(type: 'foldStatusChange', callback: Callback&lt;FoldStatus&gt;): void

开启折叠设备折叠状态变化的监听。能够获取展开或半折叠的状态值，无法获取完全折叠的状态值。

**系统能力：** SystemCapability.Window.SessionManager

**支持平台：** Android

**接口依赖：** 修改.arkui-x\android目录相关文件配置:
<br/>- gradle.properties中android.useAndroidX=true
<br/>- APP目录下build.gradle中增加androidx库依赖
<br/>-- implementation "androidx.window:window-java:1.0.0"
<br/>-- implementation "androidx.core:core:1.8.0"

**参数：**

| 参数名   | 类型                                       | 必填 | 说明                                                    |
| -------- |------------------------------------------| ---- | ------------------------------------------------------- |
| type     | string                                   | 是   | 监听事件，固定为'foldStatusChange'，表示折叠设备折叠状态发生变化。 |
| callback | Callback&lt;[FoldStatus](#foldstatus20)&gt; | 是   | 回调函数。表示折叠设备折叠状态。部分手机型号返回两次状态 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[屏幕错误码](../errorcodes/errorcode-display.md)。

| 错误码ID | 错误信息 |
| ------- | ----------------------- |
| 401     | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types.|
| 1400003 | This display manager service works abnormally. |

**示例：**

```ts
import { Callback } from '@kit.BasicServicesKit';

let callback: Callback<display.FoldStatus> = (data: display.FoldStatus) => {
  console.info('Listening enabled. Data: ' + JSON.stringify(data));
};
display.on('foldStatusChange', callback);
```

## display.off('foldStatusChange')<sup>20+</sup>

off(type: 'foldStatusChange', callback?: Callback&lt;FoldStatus&gt;): void

关闭折叠设备折叠状态变化的监听。

**系统能力：** SystemCapability.Window.SessionManager

**支持平台：** Android

**参数：**

| 参数名   | 类型                                       | 必填 | 说明                                                    |
| -------- |------------------------------------------| ---- | ------------------------------------------------------- |
| type     | string                                   | 是   | 监听事件，固定为'foldStatusChange'，表示折叠设备折叠状态发生变化。 |
| callback | Callback&lt;[FoldStatus](#foldstatus20)&gt; | 否   | 需要取消注册的回调函数。若无此参数，则取消注册折叠状态变化监听的所有回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[屏幕错误码](../errorcodes/errorcode-display.md)。

| 错误码ID | 错误信息 |
| ------- | ----------------------- |
| 401     | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types.|
| 1400003 | This display manager service works abnormally. |

**示例：**

```ts

// 如果通过on注册多个callback，同时关闭所有callback监听
display.off('foldStatusChange');

let callback: Callback<display.FoldStatus> = (data: display.FoldStatus) => {
  console.info('unregistering FoldStatus changes callback. Data: ' + JSON.stringify(data));
};
// 关闭传入的callback监听
display.off('foldStatusChange', callback);
```