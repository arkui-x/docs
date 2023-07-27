# Context

The **Context** module provides context for abilities or applications. It allows access to application-specific resources.

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

| Name         | Type    | Readable  | Writable  | Description     |
| ----------- | ------ | ---- | ---- | ------- |
| resourceManager     | resmgr.[ResourceManager](js-apis-resource-manager.md) | Yes   | No   | Object for resource management.  |
| applicationInfo | [ApplicationInfo](js-apis-bundleManager-applicationInfo.md) | Yes   | No   | Application information.|
| cacheDir | string | Yes   | No   | Cache directory.|
| tempDir | string | Yes   | No   | Temporary directory.|
| filesDir | string | Yes   | No   | File directory.|
| databaseDir | string | Yes   | No   | Database directory.|
| preferencesDir | string | Yes   | No   | Preferences directory.|
| bundleCodeDir | string | Yes   | No   | Bundle code directory. Do not access resource files by concatenating paths. Use the [resourceManager API](js-apis-resource-manager.md) instead.|

## Context.createModuleContext

createModuleContext(moduleName: string): Context;

Creates the context based on the module name.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Parameters**

| Name      | Type                    | Mandatory  | Description           |
| -------- | ---------------------- | ---- | ------------- |
| moduleName | string | Yes   | Module name.|

**Return value**

| Type| Description|
| -------- | -------- |
| Context | Context created.|

**Example**

```ts
let moduleContext: common.Context;
try {
    moduleContext = this.context.createModuleContext('entry');
} catch (error) {
    console.error('createModuleContext failed, error.code: ${error.code}, error.message: ${error.message}');
}
```

## Context.getApplicationContext

getApplicationContext(): ApplicationContext;

Obtains the context of this application.

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

**Return value**

| Type| Description|
| -------- | -------- |
| [ApplicationContext](js-apis-inner-application-applicationContext.md) | Application context obtained.|

**Example**

```ts
let applicationContext: common.Context;
try {
    applicationContext = this.context.getApplicationContext();
} catch (error) {
    console.error('getApplicationContext failed, error.code: ${error.code}, error.message: ${error.message}');
}
```
