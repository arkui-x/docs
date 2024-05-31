# @ohos.app.ability.Configuration (Configuration)

The **Configuration** module defines environment change information. **Configuration** is an interface definition and is used only for field declaration.

> **NOTE**
> 
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```ts
import Configuration from '@ohos.app.ability.Configuration';
```

**System capability**: SystemCapability.Ability.AbilityBase

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| colorMode | [ColorMode](js-apis-app-ability-configurationConstant.md#configurationconstantcolormode) | Yes| Yes| Color mode. The default value is **COLOR_MODE_LIGHT**. The options are as follows:<br>- **COLOR_MODE_NOT_SET**: The color mode is not set.<br>- **COLOR_MODE_LIGHT**: light mode.<br>- **COLOR_MODE_DARK**: dark mode.|
| direction | [Direction](js-apis-app-ability-configurationConstant.md#configurationconstantdirection) | Yes| No| Screen orientation. The options are as follows:<br>- **DIRECTION_NOT_SET**: The screen orientation is not set.<br>- **DIRECTION_HORIZONTAL**: horizontal direction.<br>- **DIRECTION_VERTICAL**: vertical direction.|

For details about the fields, see the **ohos.app.ability.Configuration.d.ts** file.
