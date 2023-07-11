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

```js
let displayClass = null;
try {
    displayClass = display.getDefaultDisplaySync();
} catch (exception) {
    console.error('Failed to obtain the default display object. Code: ' + JSON.stringify(exception));
}
```

## Display
屏幕实例。描述display对象的属性和方法。

下列API示例中都需先使用[getAllDisplays()](#displaygetalldisplays9)、[getDefaultDisplaySync()](#displaygetdefaultdisplaysync9)中的任一方法获取到Display实例，再通过此实例调用对应方法。

### 属性

**系统能力：** SystemCapability.WindowManager.WindowManager.Core

| 名称 | 类型 | 可读 | 可写 | 说明                                                                                                            |
| -------- | -------- | -------- | -------- |---------------------------------------------------------------------------------------------------------------|
| id | number | 是 | 否 | 显示设备的id号，该参数应为整数。                                                                                             |
| width | number | 是 | 否 | 显示设备的宽度，单位为像素，该参数应为整数。                                                                                        |
| height | number | 是 | 否 | 显示设备的高度，单位为像素，该参数应为整数。                                                                                        |
| orientation<sup>10+</sup> | [Orientation](#orientation10) | 是 | 否 | 表示屏幕当前显示的方向。                                                                                                  |
