# @ohos.app.ability.AbilityConstant (AbilityConstant)

AbilityConstant提供Ability相关的枚举，包括设置初次启动原因、上次退出原因等。

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。  
> 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import AbilityConstant from '@ohos.app.ability.AbilityConstant';
```

## 属性

## AbilityConstant.LaunchParam

启动参数。

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityRuntime.Core

| 名称 | 类型 | 可读 | 可写 | 说明 |
| -------- | -------- | -------- | -------- | -------- |
| launchReason | [LaunchReason](#abilityconstantlaunchreason)| 是 | 是 | 枚举类型，表示启动原因。 |
| lastExitReason | [LastExitReason](#abilityconstantlastexitreason) | 是 | 是 | 枚举类型，表示最后退出原因。 |

## AbilityConstant.LaunchReason

Ability初次启动原因，该类型为枚举，可配合[Ability](js-apis-app-ability-uiAbility.md)的[onCreate(want, launchParam)](js-apis-app-ability-uiAbility.md#uiabilityoncreate)方法根据launchParam.launchReason的不同类型执行相应操作。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

| 名称                          | 值   | 说明                                                         |
| ----------------------------- | ---- | ------------------------------------------------------------ |
| UNKNOWN          | 0    | 未知原因。 |

**示例：**

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

Ability上次退出原因，该类型为枚举，可配合[Ability](js-apis-app-ability-uiAbility.md)的[onCreate(want, launchParam)](js-apis-app-ability-uiAbility.md#uiabilityoncreate)方法根据launchParam.lastExitReason的不同类型执行相应操作。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

| 名称                          | 值   | 说明                                                         |
| ----------------------------- | ---- | ------------------------------------------------------------ |
| UNKNOWN          | 0    | 未知原因。 |

**示例：**

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