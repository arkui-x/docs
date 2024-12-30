# @ohos.app.ability.Want (Want)

Want is a carrier for information transfer between objects (application components). Want can be used as a parameter of **startAbility** to specify a startup target and information that needs to be carried during startup, for example, **bundleName** and **abilityName**, which respectively indicate the bundle name of the target ability and the ability name in the bundle. When UIAbility A needs to start UIAbility B and transfer some data to UIAbility B, it can use Want a carrier to transfer the data.

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```ts
import Want from '@ohos.app.ability.Want';
```

## Attributes

**System capability**: SystemCapability.Ability.AbilityBase

| Name       | Type                | Mandatory| Description                                                        |
| ----------- | -------------------- | ---- | ------------------------------------------------------------ |
| bundleName   | string               | Yes  | Bundle name of the ability.|
| moduleName | string | Yes| Name of the module to which the ability belongs.|
| abilityName  | string               | Yes  | Name of the ability. |
| parameters   | {[key: string]: any} | No  | Want parameters in the form of custom key-value (KV) pairs. |

**Example**

- Basic usage: called in a UIAbility object, as shown in the example below. For details about how to obtain the context, see [Obtaining the Context of UIAbility](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/application-models/uiability-usage.md#obtaining-the-context-of-uiability).

  ```ts
  import common from '@ohos.app.ability.common';
  import Want from '@ohos.app.ability.Want';

  let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
  let want: Want = {
    deviceId: '', // An empty deviceId indicates the local device.
    bundleName: 'com.example.myapplication',
    abilityName: 'FuncAbility',
    moduleName: 'entry', // moduleName is optional.
  };

  context.startAbility(want, (err) => {
    // Start an ability explicitly. The bundleName, abilityName, and moduleName parameters work together to uniquely identify an ability.
    console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
  });
  ```

- Currently, the following data types are supported by parameters: string, number, Boolean.

    * String
        ```ts
        import common from '@ohos.app.ability.common';
        import Want from '@ohos.app.ability.Want';

        let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
        let want: Want = {
          bundleName: 'com.example.myapplication',
          abilityName: 'FuncAbility',
          parameters: {
            keyForString: 'str',
          },
        };

        context.startAbility(want, (err) => {
          console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
        });
        ```
    * Number
        ```ts
        import common from '@ohos.app.ability.common';
        import Want from '@ohos.app.ability.Want';

        let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
        let want: Want = {
          bundleName: 'com.example.myapplication',
          abilityName: 'FuncAbility',
          parameters: {
            keyForInt: 100,
            keyForDouble: 99.99,
          },
        };
        ```
    * Boolean
        ```ts
        import Want from '@ohos.app.ability.Want';

        let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
        let want: Want = {
          bundleName: 'com.example.myapplication',
          abilityName: 'FuncAbility',
          parameters: {
            keyForBool: true,
          },
        };

        context.startAbility(want, (err) => {
          console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
        });
        ```
    - Usage of **parameters**: The following uses **ability.params.backToOtherMissionStack** as an example. When a ServiceExtensionAbility starts a UIAbility, redirection back across mission stacks is supported.
