# @ohos.app.ability.ConfigurationConstant (ConfigurationConstant)

ConfigurationConstant模块提供配置信息枚举值定义的能力。

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import ConfigurationConstant from '@ohos.app.ability.ConfigurationConstant';
```

## ConfigurationConstant.ColorMode

使用时通过ConfigurationConstant.ColorMode获取。

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityBase

| 名称 | 值 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- |
| COLOR_MODE_NOT_SET | -1 | 未设置颜色模式。 | 支持 | 支持 |
| COLOR_MODE_DARK | 0 | 深色模式。 | 支持 | 支持 |
| COLOR_MODE_LIGHT | 1 | 浅色模式。 | 支持 | 支持 |


## ConfigurationConstant.Direction

使用时通过ConfigurationConstant.Direction获取。

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityBase

| 名称 | 值 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- |
| DIRECTION_NOT_SET | -1 | 未设置方向。 | 支持 | 支持 |
| DIRECTION_VERTICAL | 0 | 垂直方向。 | 支持 | 支持 |
| DIRECTION_HORIZONTAL | 1 | 水平方向。 | 支持 | 支持 |

## ConfigurationConstant.ScreenDensity<sup>16+</sup>

表示屏幕像素的枚举。
> **说明：**
>
> iOS设备屏幕像素为实际iPhone机型的像素密度(PPI)。
>
> Android设备支持返回ConfigurationConstant.ScreenDensity之外的值。


**系统能力**：SystemCapability.Ability.AbilityBase

| 名称 | 值 | 说明 |
| -------- | -------- | -------- |
| SCREEN_DENSITY_NOT_SET | 0 | 未设置屏幕像素密度。 |
| SCREEN_DENSITY_SDPI | 120 | 屏幕像素密度为'SDPI'。 |
| SCREEN_DENSITY_MDPI | 160 | 屏幕像素密度为'MDPI'。 |
| SCREEN_DENSITY_LDPI | 240 | 屏幕像素密度为'LDPI'。 |
| SCREEN_DENSITY_XLDPI | 320 | 屏幕像素密度为'XLDPI'。 |
| SCREEN_DENSITY_XXLDPI | 480 | 屏幕像素密度为'XXLDPI'。 |
| SCREEN_DENSITY_XXXLDPI | 640 | 屏幕像素密度为'XXXLDPI'。 |