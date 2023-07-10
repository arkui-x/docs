# AbilityStageMonitor

提供用于匹配满足指定条件的受监视的AbilityStage对象的方法。最近匹配的AbilityStage对象将保存在AbilityStageMonitor对象中。 

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。  

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityRuntime.Core

| 名称                                                         | 类型     | 可读 | 可写 | 说明                                                         |
| ------------------------------------------------------------ | -------- | ---- | ---- | ------------------------------------------------------------ |
| moduleName                                                 | string   | 是   | 是   | 要监视的abilityStage的模块名。 |
| srcEntrance | string | 是   | 是   | 要监视的abilityStage的源路径。 |

**示例：**
```ts
import AbilityDelegatorRegistry from '@ohos.app.ability.abilityDelegatorRegistry';

let monitor = {
    moduleName: 'feature_as1',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

let abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.waitAbilityStageMonitor(monitor, (error, data) => {
    if (error) {
        console.error('waitAbilityStageMonitor fail, error: ${JSON.stringify(error)}');
    } else {
        console.log('waitAbilityStageMonitor success, data: ${JSON.stringify(data)}');
    }
});
```