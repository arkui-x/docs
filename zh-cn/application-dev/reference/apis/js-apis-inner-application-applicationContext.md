# ApplicationContext

ApplicationContext模块提供开发者应用级别的的上下文的能力，包括提供注册及取消注册应用内组件生命周期的监听接口。

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。  
> 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import common from '@ohos.app.ability.common';
```

## 使用说明

在使用ApplicationContext的功能前，需要通过context的实例获取。

```ts
let applicationContext: common.ApplicationContext = this.context.getApplicationContext();
```

## ApplicationContext.on(type: 'abilityLifecycle', callback: AbilityLifecycleCallback)

on(type: 'abilityLifecycle', callback: AbilityLifecycleCallback): **number**;

注册监听应用内生命周期

**参数：**

| 参数名                   | 类型     | 必填 | 说明                           |
| ------------------------ | -------- | ---- | ------------------------------ |
| type | 'abilityLifecycle' | 是   | 监听事件的类型。 |
| callback | [AbilityLifecycleCallback](js-apis-app-ability-abilityLifecycleCallback.md) | 是   | 回调方法，返回注册监听事件的ID。 |

**返回值：**

| 类型   | 说明                           |
| ------ | ------------------------------ |
| number | 返回的此次注册监听生命周期的ID（每次注册该ID会自增+1，当超过监听上限数量2^63-1时，返回-1）。|

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let lifecycleId;

export default class EntryAbility extends UIAbility {
    onCreate() {
        console.log('MyAbility onCreate');
        let AbilityLifecycleCallback = {
            onAbilityCreate(ability) {
                console.log('AbilityLifecycleCallback onAbilityCreate ability: ${ability}');
            },
            onWindowStageCreate(ability, windowStage) {
                console.log('AbilityLifecycleCallback onWindowStageCreate ability: ${ability}');
                console.log('AbilityLifecycleCallback onWindowStageCreate windowStage: ${windowStage}');
            },
            onWindowStageActive(ability, windowStage) {
                console.log('AbilityLifecycleCallback onWindowStageActive ability: ${ability}');
                console.log('AbilityLifecycleCallback onWindowStageActive windowStage: ${windowStage}');
            },
            onWindowStageInactive(ability, windowStage) {
                console.log('AbilityLifecycleCallback onWindowStageInactive ability: ${ability}');
                console.log('AbilityLifecycleCallback onWindowStageInactive windowStage: ${windowStage}');
            },
            onWindowStageDestroy(ability, windowStage) {
                console.log('AbilityLifecycleCallback onWindowStageDestroy ability: ${ability}');
                console.log('AbilityLifecycleCallback onWindowStageDestroy windowStage: ${windowStage}');
            },
            onAbilityDestroy(ability) {
                console.log('AbilityLifecycleCallback onAbilityDestroy ability: ${ability}');
            },
            onAbilityForeground(ability) {
                console.log('AbilityLifecycleCallback onAbilityForeground ability: ${ability}');
            },
            onAbilityBackground(ability) {
                console.log('AbilityLifecycleCallback onAbilityBackground ability: ${ability}');
            },
            onAbilityContinue(ability) {
                console.log('AbilityLifecycleCallback onAbilityContinue ability: ${ability}');
            }
        }
        // 1.通过context属性获取applicationContext
        let applicationContext = this.context.getApplicationContext();
        // 2.通过applicationContext注册监听应用内生命周期
        lifecycleId = applicationContext.on('abilityLifecycle', AbilityLifecycleCallback);
        console.log('registerAbilityLifecycleCallback lifecycleId: ${lifecycleId)}');
    }
}
```

## ApplicationContext.off(type: 'abilityLifecycle', callbackId: number, callback: AsyncCallback\<void>)

off(type: 'abilityLifecycle', callbackId: **number**,  callback: AsyncCallback<**void**>): **void**;

取消监听应用内生命周期

**参数：**

| 参数名        | 类型     | 必填 | 说明                       |
| ------------- | -------- | ---- | -------------------------- |
| type | 'abilityLifecycle' | 是   | 取消监听事件的类型。 |
| callbackId    | number   | 是   | 注册监听应用内生命周期的ID。 |
| callback | AsyncCallback\<void> | 是   | 回调方法。                   |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let lifecycleId;

export default class EntryAbility extends UIAbility {
    onDestroy() {
        let applicationContext = this.context.getApplicationContext();
        console.log('stage applicationContext: ${applicationContext}');
        applicationContext.off('abilityLifecycle', lifecycleId, (error, data) => {
            if (error) {
                console.error('unregisterAbilityLifecycleCallback fail, err: ${JSON.stringify(error)}');    
            } else {
                console.log('unregisterAbilityLifecycleCallback success, data: ${JSON.stringify(data)}');
            }
        });
    }
}
```

## ApplicationContext.off(type: 'abilityLifecycle', callbackId: number)

off(type: 'abilityLifecycle', callbackId: **number**): **void**;

取消监听应用内生命周期

**参数：**

| 参数名        | 类型     | 必填 | 说明                       |
| ------------- | -------- | ---- | -------------------------- |
| type | 'abilityLifecycle' | 是   | 取消监听事件的类型。 |
| callbackId    | number   | 是   | 注册监听应用内生命周期的ID。 |

**示例：**

```ts
import Ability from '@ohos.app.ability.UIAbility';

let lifecycleId;

export default class MyAbility extends Ability {
    onDestroy() {
        let applicationContext = this.context.getApplicationContext();
        console.log('stage applicationContext: ${applicationContext}');
        applicationContext.off('abilityLifecycle', lifecycleId);
    }
}
```

## ApplicationContext.getRunningProcessInformation<sup>9+</sup>

getRunningProcessInformation(): Promise\<Array\<ProcessInformation>>;

获取有关运行进程的信息。

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise\<Array\<[ProcessInformation](js-apis-inner-application-processInformation.md)>> | 以Promise方式返回接口运行结果及有关运行进程的信息，可进行错误处理或其他自定义处理。 |

**示例：**

```ts
let applicationContext = this.context.getApplicationContext();
applicationContext.getRunningProcessInformation().then((data) => {
    console.log('The process running information is: ${JSON.stringify(data)}');
}).catch((error) => {
    console.error('error: ${JSON.stringify(error)}');
});
```

## ApplicationContext.getRunningProcessInformation<sup>9+</sup>

getRunningProcessInformation(callback: AsyncCallback\<Array\<ProcessInformation>>): void;

获取有关运行进程的信息。

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
|AsyncCallback\<Array\<[ProcessInformation](js-apis-inner-application-processInformation.md)>> | 以回调方式返回接口运行结果及有关运行进程的信息，可进行错误处理或其他自定义处理。 |

**示例：**

```ts
let applicationContext = this.context.getApplicationContext();
applicationContext.getRunningProcessInformation((err, data) => {
    if (err) {
        console.error('getRunningProcessInformation faile, err: ${JSON.stringify(err)}');
    } else {
        console.log('The process running information is: ${JSON.stringify(data)}');
    }
})
```
