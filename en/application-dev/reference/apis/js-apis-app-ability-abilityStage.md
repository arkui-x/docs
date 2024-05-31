# @ohos.app.ability.AbilityStage (AbilityStage)

**AbilityStage** is a runtime class for HAP files.

**AbilityStage** notifies you of when you can perform HAP initialization such as resource pre-loading and thread creation during the HAP loading.

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.
> The APIs of this module can be used only in the stage model.

## Modules to Import

```ts
import AbilityStage from '@ohos.app.ability.AbilityStage';
```

## AbilityStage.onCreate

onCreate(): void

Called when the application is created.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Example**

```ts
import AbilityStage from '@ohos.app.ability.AbilityStage';

class MyAbilityStage extends AbilityStage {
    onCreate() {
        console.log('MyAbilityStage.onCreate is called');
    }
}
```


## AbilityStage.onConfigurationUpdate

onConfigurationUpdate(newConfig: Configuration): void;

Called when the global configuration is updated.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Parameters**

  | Name| Type| Mandatory| Description|
  | -------- | -------- | -------- | -------- |
  | newConfig | [Configuration](js-apis-app-ability-configuration.md) | Yes| Callback invoked when the global configuration is updated. The global configuration indicates the configuration of the environment where the application is running and includes the language and color mode.|

**Example**

```ts
import AbilityStage from '@ohos.app.ability.AbilityStage';
import { Configuration } from '@ohos.app.ability.Configuration';

class MyAbilityStage extends AbilityStage {
    onConfigurationUpdate(config: Configuration) {
        console.log('onConfigurationUpdate, language: ${config.language}');
    }
}
```

## AbilityStage.context

context: AbilityStageContext;

Defines the context of **AbilityStage**.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

| Name     | Type                       | Description                                                        |
| ----------- | --------------------------- | ------------------------------------------------------------ |
| context  | [AbilityStageContext](js-apis-inner-application-abilityStageContext.md) | The context is obtained in the callback invoked when initialization is performed during ability startup.|
