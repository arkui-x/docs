# AbilityMonitor

AbilityMonitor模块提供匹配满足指定条件的受监视能力对象的方法的能力，最近匹配的ability对象将保存在AbilityMonitor对象中。

> **说明：**
> 
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。  

## 导入模块

```ts
import AbilityDelegatorRegistry from '@ohos.app.ability.abilityDelegatorRegistry';
```

## 使用说明

可以作为abilityDelegator中的[addAbilityMonitor](js-apis-inner-application-abilityDelegator.md#addabilitymonitor9)的入参来监听指定Ability的生命周期变化。

## AbilityMonitor

Ability监听器

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityRuntime.Core

| 名称                                                         | 类型     | 可读 | 可写 | 说明                                                         |
| ------------------------------------------------------------ | -------- | ---- | ---- | ------------------------------------------------------------ |
| abilityName                                                  | string   | 是   | 是   | 当前AbilityMonitor绑定的ability名称 |
| moduleName?                                                  | string   | 是   | 是   | 当前AbilityMonitor绑定的模块名称 |
| onAbilityCreate?:(data: [UIAbility](js-apis-app-ability-uiAbility.md)) | function | 是   | 是   | ability被启动初始化时的回调函数<br/>不设置该属性则不能收到该生命周期回调 |
| onAbilityForeground?:(data: [UIAbility](js-apis-app-ability-uiAbility.md)) | function | 是   | 是   | ability状态变成前台时的回调函数<br/>不设置该属性则不能收到该生命周期回调 |
| onAbilityBackground?:(data: [UIAbility](js-apis-app-ability-uiAbility.md)) | function | 是   | 是   | ability状态变成后台时的回调函数<br/>不设置该属性则不能收到该生命周期回调 |
| onAbilityDestroy?:(data: [UIAbility](js-apis-app-ability-uiAbility.md)) | function | 是   | 是   | ability被销毁前的回调函数<br/>不设置该属性则不能收到该生命周期回调<br/> |
| onWindowStageCreate?:(data: [UIAbility](js-apis-app-ability-uiAbility.md)) | function | 是   | 是   | window stage被创建时的回调函数<br/>不设置该属性则不能收到该生命周期回调<br/> |
| onWindowStageDestroy?:(data: [UIAbility](js-apis-app-ability-uiAbility.md)) | function | 是   | 是   | window stage被销毁前的回调函数<br/>不设置该属性则不能收到该生命周期回调<br/> |

**示例：**

```ts
import AbilityDelegatorRegistry from '@ohos.app.ability.abilityDelegatorRegistry';

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback, data: ${JSON.stringify(data)}');
}

let monitor = {
    abilityName: 'abilityname',
    moduleName: "moduleName",
    onAbilityCreate: onAbilityCreateCallback
};

let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.addAbilityMonitor(monitor, (error : any) => {
    if (error) {
        console.error('addAbilityMonitor fail, error: ${JSON.stringify(error)}');
    }
});
```

