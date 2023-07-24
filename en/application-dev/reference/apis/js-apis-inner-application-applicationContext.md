# ApplicationContext

The **ApplicationContext** module provides application-level context. You can use the APIs of this module to register and deregister the ability lifecycle listener in an application.

> **NOTE**
> 
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version. 
> The APIs of this module can be used only in the stage model.

## Modules to Import

```ts
import common from '@ohos.app.ability.common';
```

## Usage

Before calling any APIs in **ApplicationContext**, obtain an **ApplicationContext** instance through the **context** instance.

```ts
let applicationContext: common.ApplicationContext = this.context.getApplicationContext();
```

## ApplicationContext.on(type: 'abilityLifecycle', callback: AbilityLifecycleCallback)

on(type: 'abilityLifecycle', callback: AbilityLifecycleCallback): **number**;

Registers a listener to monitor the ability lifecycle of the application.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Parameters**

| Name                  | Type    | Mandatory| Description                          |
| ------------------------ | -------- | ---- | ------------------------------ |
| type | 'abilityLifecycle' | Yes  | Event type.|
| callback | [AbilityLifecycleCallback](js-apis-app-ability-abilityLifecycleCallback.md) | Yes  | Callback used to return the ID of the registered listener.|

**Return value**

| Type  | Description                          |
| ------ | ------------------------------ |
| number | ID of the registered listener. The ID is incremented by 1 each time the listener is registered. When the ID exceeds 2^63-1, **-1** is returned.|

**Example**

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
            }
        }
        // 1. Obtain applicationContext through the context attribute.
        let applicationContext = this.context.getApplicationContext();
        // 2. Use applicationContext to register a listener for the ability lifecycle in the application.
        lifecycleId = applicationContext.on('abilityLifecycle', AbilityLifecycleCallback);
        console.log('registerAbilityLifecycleCallback lifecycleId: ${lifecycleId)}');
    }
}
```

## ApplicationContext.off(type: 'abilityLifecycle', callbackId: number, callback: AsyncCallback\<void>)

off(type: 'abilityLifecycle', callbackId: **number**,  callback: AsyncCallback<**void**>): **void**;

Deregisters the listener that monitors the ability lifecycle of the application.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Parameters**

| Name       | Type    | Mandatory| Description                      |
| ------------- | -------- | ---- | -------------------------- |
| type | 'abilityLifecycle' | Yes  | Event type.|
| callbackId    | number   | Yes  | ID of the listener to deregister.|
| callback | AsyncCallback\<void> | Yes  | Callback used to return the result.                  |

**Example**

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

## ApplicationContext.getRunningProcessInformation<sup>9+</sup>

getRunningProcessInformation(): Promise\<Array\<ProcessInformation>>;

Obtains information about the running processes. This API uses a promise to return the result.

**Required permissions**: ohos.permission.GET_RUNNING_INFO

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**System API**: This is a system API and cannot be called by third-party applications.

**Return value**

| Type| Description|
| -------- | -------- |
| Promise\<Array\<[ProcessInformation](js-apis-inner-application-processInformation.md)>> | Promise used to return the API call result and the process running information. You can perform error handling or custom processing in this callback.|

**Error codes**

| ID| Error Message|
| ------- | -------- |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |

For details about the error codes, see [Ability Error Codes](../errorcodes/errorcode-ability.md).

**Example**

```ts
applicationContext.getRunningProcessInformation().then((data) => {
    console.log('The process running information is: ${JSON.stringify(data)}');
}).catch((error) => {
    console.error('error: ${JSON.stringify(error)}');
});
```

## ApplicationContext.getRunningProcessInformation<sup>9+</sup>

getRunningProcessInformation(callback: AsyncCallback\<Array\<ProcessInformation>>): void;

Obtains information about the running processes. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.GET_RUNNING_INFO

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**System API**: This is a system API and cannot be called by third-party applications.

**Return value**

| Type| Description|
| -------- | -------- |
|AsyncCallback\<Array\<[ProcessInformation](js-apis-inner-application-processInformation.md)>> | Callback used to return the API call result and the process running information. You can perform error handling or custom processing in this callback.|

**Error codes**

| ID| Error Message|
| ------- | -------- |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |

For details about the error codes, see [Ability Error Codes](../errorcodes/errorcode-ability.md).

**Example**

```ts
applicationContext.getRunningProcessInformation((err, data) => {
    if (err) {
        console.error('getRunningProcessInformation faile, err: ${JSON.stringify(err)}');
    } else {
        console.log('The process running information is: ${JSON.stringify(data)}');
    }
})
```
