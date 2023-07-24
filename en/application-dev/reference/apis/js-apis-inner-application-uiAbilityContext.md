# UIAbilityContext

**UIAbilityContext**, inherited from [Context](js-apis-inner-application-context.md), provides the context environment for [UIAbility](js-apis-app-ability-uiAbility.md). **UIAbilityContext** provides UIAbility-related configuration and APIs for operating UIAbilities. For example, you can use the APIs to start a UIAbility, terminate a UIAbility to which the UIAbilityContext belongs, and start, terminate, connect to.

> **NOTE**
>
>  - The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>  - The APIs of this module can be used only in the stage model.

## Modules to Import

```ts
import common from '@ohos.app.ability.common';
```

## Attributes

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| abilityInfo | [AbilityInfo](js-apis-bundleManager-abilityInfo.md) | Yes| No| UIAbility information.|
| currentHapModuleInfo | [HapModuleInfo](js-apis-bundleManager-hapModuleInfo.md) | Yes| No| HAP information.|
| config | [Configuration](js-apis-app-ability-configuration.md) | Yes| No| UIAbility configuration, such as the language and color mode.|

> **NOTE**
>
> In the sample code provided in this topic, **this.context** is used to obtain **UIAbilityContext**, where **this** indicates a UIAbility instance inherited from **UIAbility**. To use **UIAbilityContext** APIs on pages, see [Obtaining the Context of UIAbility](https://gitee.com/openharmony/docs/blob/master/en/application-dev/application-models/uiability-usage.md#%E8%8E%B7%E5%8F%96uiability%E7%9A%84%E4%B8%8A%E4%B8%8B%E6%96%87%E4%BF%A1%E6%81%AF).

## UIAbilityContext.startAbility

startAbility(want: Want, callback: AsyncCallback&lt;void&gt;): void;

Starts an ability. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-app-ability-want.md) | Yes| Want information about the target ability.|
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |

For details about the error codes, see [Ability Error Codes](../errorcodes/errorcode-ability.md).

**Example**

  ```ts
let want = {
  bundleName: 'com.example.myapplication',
  abilityName: 'EntryAbility'
};

try {
  this.context.startAbility(want, (err) => {
    if (err.code) {
      // Process service logic errors.
      console.error(`startAbility failed, code is ${err.code}, message is ${err.message}`);
      return;
    }
    // Carry out normal service processing.
    console.info('startAbility succeed');
  });
} catch (err) {
  // Process input parameter errors.
  console.error(`startAbility failed failed, code is ${err.code}, message is ${err.message}`);
}
  ```

## UIAbilityContext.startAbility

startAbility(want: Want, options?: StartOptions): Promise&lt;void&gt;;

Starts an ability. This API uses a promise to return the result.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-app-ability-want.md) | Yes| Want information about the target ability.|
| options | [StartOptions]| No| Parameters used for starting the ability. Cross-platform is not currently supported.|

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;void&gt; | Promise used to return the result.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |

For details about the error codes, see [Ability Error Codes](../errorcodes/errorcode-ability.md).

**Example**

  ```ts
let want = {
  bundleName: 'com.example.myapplication',
  abilityName: 'EntryAbility'
};
let options = {
  windowMode: 0,
};

try {
  this.context.startAbility(want, options)
    .then(() => {
      // Carry out normal service processing.
      console.info('startAbility succeed');
    })
    .catch((err) => {
      // Process service logic errors.
      console.error(`startAbility failed, code is ${err.code}, message is ${err.message}`);
    });
} catch (err) {
  // Process input parameter errors.
  console.error(`startAbility failed, code is ${err.code}, message is ${err.message}`);
}
  ```

## UIAbilityContext.terminateSelf

terminateSelf(callback: AsyncCallback&lt;void&gt;): void;

Terminates this ability. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result.|

**Example**

  ```ts
  try {
    this.context.terminateSelf((err) => {
      if (err.code) {
        // Process service logic errors.
        console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
        return;
      }
      // Carry out normal service processing.
      console.info('terminateSelf succeed');
    });
  } catch (err) {
    // Capture the synchronization parameter error.
    console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
  }
  ```


## UIAbilityContext.terminateSelf

terminateSelf(): Promise&lt;void&gt;;

Terminates this ability. This API uses a promise to return the result.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;void&gt; | Promise used to return the result.|

**Example**

  ```ts
  try {
    this.context.terminateSelf()
      .then(() => {
        // Carry out normal service processing.
        console.info('terminateSelf succeed');
      })
      .catch((err) => {
        // Process service logic errors.
        console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
      });
  } catch (error) {
    // Capture the synchronization parameter error.
    console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
  }
  ```