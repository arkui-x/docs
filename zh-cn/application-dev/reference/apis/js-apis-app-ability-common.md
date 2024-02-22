# @ohos.app.ability.common (应用上下文Context)

Common模块将二级模块API组织在一起方便开发者进行导出。

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
> 本模块接口仅可在Stage模型下使用

## 导入模块

```ts
import common from '@ohos.app.ability.common';
```

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityBase

| 名称        | 类型                 | 说明                                                         |
| ----------- | -------------------- | ------------------------------------------------------------ |
| UIAbilityContext    | [UIAbilityContext](js-apis-inner-application-uiAbilityContext.md)               | UIAbilityContext二级模块。                                |
| AbilityStageContext   | [AbilityStageContext](js-apis-inner-application-abilityStageContext.md)               | AbilityStageContext二级模块。 |
| ApplicationContext   | [ApplicationContext](js-apis-inner-application-applicationContext.md)               | ApplicationContext二级模块。 |
| BaseContext   | [BaseContext](js-apis-inner-application-baseContext.md)               | BaseContext二级模块。 |
| Context   | [Context](js-apis-inner-application-context.md)               | Context二级模块。 |
| EventHub<sup>12+</sup>   | [EventHub](js-apis-inner-application-eventHub.md)               | EventHub二级模块。 |

**示例：**
```ts
import common from '@ohos.app.ability.common';

let uiAbilityContext: common.UIAbilityContext;
let abilityStageContext: common.AbilityStageContext;
let applicationContext: common.ApplicationContext;
let baseContext: common.BaseContext;
let context: common.Context;
```
