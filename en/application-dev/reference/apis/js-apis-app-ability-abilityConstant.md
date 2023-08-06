# @ohos.app.ability.AbilityConstant (AbilityConstant)

The **AbilityConstant** module defines the ability-related enums, including the initial launch reasons, reasons for the last exit, ability continuation results, and window modes.

> **NOTE**
> 
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version. 
> The APIs of this module can be used only in the stage model.

## Modules to Import

```ts
import AbilityConstant from '@ohos.app.ability.AbilityConstant';
```

## Attributes

## AbilityConstant.LaunchParam

Defines the parameters for starting an ability.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| launchReason | [LaunchReason](#abilityconstantlaunchreason)| Yes| Yes| Ability launch reason, which is an enumerated type.|
| lastExitReason | [LastExitReason](#abilityconstantlastexitreason) | Yes| Yes| Reason for the last exit, which is an enumerated type.|

## AbilityConstant.LaunchReason

Enumerates the initial ability launch reasons. You can use it together with [onCreate(want, launchParam)](js-apis-app-ability-uiAbility.md#uiabilityoncreate) of [Ability](js-apis-app-ability-uiAbility.md) to complete different operations.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

| Name                         | Value  | Description                                                        |
| ----------------------------- | ---- | ------------------------------------------------------------ |
| UNKNOWN          | 0    | Unknown reason.|

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class MyAbility extends UIAbility {
    onCreate(want, launchParam) {
        if (launchParam.launchReason === AbilityConstant.LaunchReason.UNKNOWN) {
            console.log('The ability has been started.');
        }
    }
}
```

## AbilityConstant.LastExitReason

Enumerates the reasons for the last exit. You can use it together with [onCreate(want, launchParam)](js-apis-app-ability-uiAbility.md#uiabilityoncreate) of [Ability](js-apis-app-ability-uiAbility.md) to complete different operations.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

| Name                         | Value  | Description                                                        |
| ----------------------------- | ---- | ------------------------------------------------------------ |
| UNKNOWN          | 0    | Unknown reason.|

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class MyAbility extends UIAbility {
    onCreate(want, launchParam) {
        if (launchParam.lastExitReason === AbilityConstant.LastExitReason.UNKNOWN) {
            console.log('The ability has exit last.');
        }
    }
}
```