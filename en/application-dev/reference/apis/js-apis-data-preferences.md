# @ohos.data.preferences (User Preferences)

The **Preferences** module provides APIs for processing data in the form of key-value (KV) pairs, and supports persistence of the KV pairs when required, as well as modification and query of the data.

The key is of the string type, and the value can be a number, a string, a Boolean value, or an array of numbers, strings, or Boolean values.


> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import

```js
import data_preferences from '@ohos.data.preferences';
```

## Constants

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

| Name            | Type| Readable| Writable| Description                                   |
| ---------------- | -------- | ---- | ---- | --------------------------------------- |
| MAX_KEY_LENGTH   | number   | Yes  | No  | Maximum length of a key, which is 80 bytes.    |
| MAX_VALUE_LENGTH | number   | Yes  | No  | Maximum length of a value, which is 8192 bytes.|


## data_preferences.getPreferences

getPreferences(context: Context, name: string, callback: AsyncCallback&lt;Preferences&gt;): void

Obtains a **Preferences** instance. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                                            | Mandatory| Description                                                        |
| -------- | ------------------------------------------------ | ---- | ------------------------------------------------------------ |
| context  | Context            | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-context.md).                                                |
| name     | string                                           | Yes  | Name of the **Preferences** instance.                                     |
| callback | AsyncCallback&lt;[Preferences](#preferences)&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined** and the **Preferences** instance obtained is returned. Otherwise, **err** is an error object.|

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.getPreferences(this.context, 'mystore', function (err, val) {
                if (err) {
                    console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                preferences = val;
                console.info("Obtained the preferences successfully.");
            })
        } catch (err) {
            console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.getPreferences

getPreferences(context: Context, name: string): Promise&lt;Preferences&gt;

Obtains a **Preferences** instance. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name | Type                                 | Mandatory| Description                   |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-context.md).           |
| name    | string                                | Yes  | Name of the **Preferences** instance.|

**Return value**

| Type                                      | Description                              |
| ------------------------------------------ | ---------------------------------- |
| Promise&lt;[Preferences](#preferences)&gt; | Promise used to return the **Preferences** instance obtained.|

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.getPreferences(this.context, 'mystore');
            promise.then((object) => {
                preferences = object;
                console.info("Obtained the preferences successfully.");
            }).catch((err) => {
                console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.getPreferences<sup>10+</sup>

getPreferences(context: Context, options: Options, callback: AsyncCallback&lt;Preferences&gt;): void

Obtains a **Preferences** instance. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                                         | Mandatory| Description                                                                                                                                                                          |
| -------- | --------------------------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context  | Context                                       | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| options  | [Options](#options10)                              | Yes  | Configuration options of the **Preferences** instance.                                                                                                                                             |
| callback | AsyncCallback&lt;[Preferences](#preferences)&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined** and the **Preferences** instance obtained is returned. Otherwise, **err** is an error object.                                                                                   |

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                      |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.getPreferences(this.context, {name: 'mystore'}, function (err, val) {
                if (err) {
                    console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                preferences = val;
                console.info("Obtained the preferences successfully.");
            })
        } catch (err) {
            console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.getPreferences<sup>10+</sup>

getPreferences(context: Context, options: Options): Promise&lt;Preferences&gt;

Obtains a **Preferences** instance. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name | Type            | Mandatory| Description                                                                                                                                                                          |
| ------- | ---------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context | Context          | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| options | [Options](#options10) | Yes  | Configuration options of the **Preferences** instance.                                                                                                                                             |

**Return value**

| Type                                   | Description                              |
| --------------------------------------- | ---------------------------------- |
| Promise&lt;[Preferences](#preferences)&gt; | Promise used to return the **Preferences** instance obtained.|

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                      |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.getPreferences(this.context, {name: 'mystore'});
            promise.then((object) => {
                preferences = object;
                console.info("Obtained the preferences successfully.");
            }).catch((err) => {
                console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences

deletePreferences(context: Context, name: string, callback: AsyncCallback&lt;void&gt;): void

Deletes a **Preferences** instance from the cache. If the **Preferences** instance has a persistent file, the persistent file will also be deleted. This API uses an asynchronous callback to return the result.

After the **Preferences** instance is deleted, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the deleted **Preferences** instance to null. The system will reclaim the deleted **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                                 | Mandatory| Description                                                |
| -------- | ------------------------------------- | ---- | ---------------------------------------------------- |
| context  | Context | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-context.md).                                        |
| name     | string                                | Yes  | Name of the **Preferences** instance.                             |
| callback | AsyncCallback&lt;void&gt;             | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.|

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                      |
| -------- | ------------------------------|
| 15500010 | Failed to delete preferences file. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.deletePreferences(this.context, 'mystore', function (err) {
                if (err) {
                    console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                console.info("Deleted the preferences successfully." );
            })
        } catch (err) {
            console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences

deletePreferences(context: Context, name: string): Promise&lt;void&gt;

Deletes a **Preferences** instance from the cache. If the **Preferences** instance has a persistent file, the persistent file will also be deleted. This API uses a promise to return the result.

After the **Preferences** instance is deleted, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the deleted **Preferences** instance to null. The system will reclaim the deleted **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name | Type                                 | Mandatory| Description                   |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-context.md).           |
| name    | string                                | Yes  | Name of the **Preferences** instance.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                      |
| -------- | ------------------------------|
| 15500010 | Failed to delete preferences file. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try{
            let promise = data_preferences.deletePreferences(this.context, 'mystore');
            promise.then(() => {
                console.info("Deleted the preferences successfully.");
            }).catch((err) => {
                console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences<sup>10+</sup>

deletePreferences(context: Context, options: Options, callback: AsyncCallback&lt;void&gt;): void

Deletes a **Preferences** instance from the cache. If the **Preferences** instance has a persistent file, the persistent file will also be deleted. This API uses an asynchronous callback to return the result.

After the **Preferences** instance is deleted, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the deleted **Preferences** instance to null. The system will reclaim the deleted **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                                                                                                                                                                          |
| -------- | ------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context  | Context                   | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| options  | [Options](#options10)          | Yes  | Configuration options of the **Preferences** instance.                                                                                                                                             |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.                                                                                                                          |

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                          |
| -------- | ---------------------------------- |
| 15500010 | Failed to delete preferences file. |
| 15501001 | Only supported in stage mode. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.deletePreferences(this.context, {name: 'mystore'}, function (err) {
                if (err) {
                    console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                console.info("Deleted the preferences successfully." );
            })
        } catch (err) {
            console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences<sup>10+</sup>

deletePreferences(context: Context, options: Options): Promise&lt;void&gt;

Deletes a **Preferences** instance from the cache. If the **Preferences** instance has a persistent file, the persistent file will also be deleted. This API uses a promise to return the result.

After the **Preferences** instance is deleted, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the deleted **Preferences** instance to null. The system will reclaim the deleted **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name | Type            | Mandatory| Description                                                                                                                                                                          |
| ------- | ---------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context | Context          | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| options | [Options](#options10) | Yes  | Configuration options of the **Preferences** instance.                                                                                                                                             |

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                          |
| -------- | ---------------------------------- |
| 15500010 | Failed to delete preferences file. |
| 15501001 | Only supported in stage mode. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try{
            let promise = data_preferences.deletePreferences(this.context, {name: 'mystore'});
            promise.then(() => {
                console.info("Deleted the preferences successfully.");
            }).catch((err) => {
                console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to delete the preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.removePreferencesFromCache

removePreferencesFromCache(context: Context, name: string, callback: AsyncCallback&lt;void&gt;): void

Removes a **Preferences** instance from the cache. This API uses an asynchronous callback to return the result.

After an application calls [getPreferences](#data_preferencesgetpreferences) for the first time to obtain a **Preferences** instance, the obtained **Preferences** instance is cached. When the application calls [getPreferences](#data_preferencesgetpreferences) again, the **Preferences** instance will be read from the cache instead of from the persistent file. After this API is called to remove the instance from the cache, calling **getPreferences** again will read data from the persistent file and create a new **Preferences** instance.

After the **Preferences** instance is removed, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the removed **Preferences** instance to null. The system will reclaim the removed **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                                 | Mandatory| Description                                                |
| -------- | ------------------------------------- | ---- | ---------------------------------------------------- |
| context  | Context | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-context.md).                                        |
| name     | string                                | Yes  | Name of the **Preferences** instance.                             |
| callback | AsyncCallback&lt;void&gt;             | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.|

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.removePreferencesFromCache(this.context, 'mystore', function (err) {
                if (err) {
                    console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                console.info("Removed the preferences successfully.");
            })
        } catch (err) {
            console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}

```

## data_preferences.removePreferencesFromCache

removePreferencesFromCache(context: Context, name: string): Promise&lt;void&gt;

Removes a **Preferences** instance from the cache. This API uses a promise to return the result.

After an application calls [getPreferences](#data_preferencesgetpreferences) for the first time to obtain a **Preferences** instance, the obtained **Preferences** instance is cached. When the application calls [getPreferences](#data_preferencesgetpreferences) again, the **Preferences** instance will be read from the cache instead of from the persistent file. After this API is called to remove the instance from the cache, calling **getPreferences** again will read data from the persistent file and create a new **Preferences** instance.

After the **Preferences** instance is removed, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the removed **Preferences** instance to null. The system will reclaim the removed **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name | Type                                 | Mandatory| Description                   |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-context.md).           |
| name    | string                                | Yes  | Name of the **Preferences** instance.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.removePreferencesFromCache(this.context, 'mystore');
            promise.then(() => {
                console.info("Removed the preferences successfully.");
            }).catch((err) => {
                console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.removePreferencesFromCacheSync<sup>10+</sup>

removePreferencesFromCacheSync(context: Context, name: string): void

Synchronously removes a **Preferences** instance from the cache.

After an application calls [getPreferences](#data_preferencesgetpreferences) for the first time to obtain a **Preferences** instance, the obtained **Preferences** instance is cached. When the application calls [getPreferences](#data_preferencesgetpreferences) again, the **Preferences** instance will be read from the cache instead of from the persistent file. After this API is called to remove the instance from the cache, calling **getPreferences** again will read data from the persistent file and create a new **Preferences** instance.

After the **Preferences** instance is removed, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the removed **Preferences** instance to null. The system will reclaim the removed **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name | Type                                 | Mandatory| Description                   |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).           |
| name    | string                                | Yes  | Name of the **Preferences** instance.|

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.removePreferencesFromCacheSync(this.context, 'mystore');
        } catch(err) {
            console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.removePreferencesFromCache<sup>10+</sup>

removePreferencesFromCache(context: Context, options: Options, callback: AsyncCallback&lt;void&gt;): void

Removes a **Preferences** instance from the cache. This API uses an asynchronous callback to return the result.

After an application calls [getPreferences](#data_preferencesgetpreferences) for the first time to obtain a **Preferences** instance, the obtained **Preferences** instance is cached. When the application calls [getPreferences](#data_preferencesgetpreferences) again, the **Preferences** instance will be read from the cache instead of from the persistent file. After this API is called to remove the instance from the cache, calling **getPreferences** again will read data from the persistent file and create a new **Preferences** instance.

After the **Preferences** instance is removed, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the removed **Preferences** instance to null. The system will reclaim the removed **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                                                                                                                                                                          |
| -------- | ------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context  | Context                   | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| options  | [Options](#options10)          | Yes  | Configuration options of the **Preferences** instance.                                                                                                                                             |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.                                                                                                                          |

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                      |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.removePreferencesFromCache(this.context, {name: 'mystore'}, function (err) {
                if (err) {
                    console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                console.info("Removed the preferences successfully.");
            })
        } catch (err) {
            console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.removePreferencesFromCache<sup>10+</sup>

removePreferencesFromCache(context: Context, options: Options): Promise&lt;void&gt;

Removes a **Preferences** instance from the cache. This API uses a promise to return the result.

After an application calls [getPreferences](#data_preferencesgetpreferences) for the first time to obtain a **Preferences** instance, the obtained **Preferences** instance is cached. When the application calls [getPreferences](#data_preferencesgetpreferences) again, the **Preferences** instance will be read from the cache instead of from the persistent file. After this API is called to remove the instance from the cache, calling **getPreferences** again will read data from the persistent file and create a new **Preferences** instance.

After the **Preferences** instance is removed, do not use it to perform data operations. Otherwise, data inconsistency may be caused. For this purpose, set the removed **Preferences** instance to null. The system will reclaim the removed **Preferences** instances in a unified manner.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name | Type            | Mandatory| Description                                                                                                                                                                          |
| ------- | ---------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context | Context          | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| options | [Options](#options10) | Yes  | Configuration options of the **Preferences** instance.                                                                                                                                             |

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                      |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**Example**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.removePreferencesFromCache(this.context, {name: 'mystore'});
            promise.then(() => {
                console.info("Removed the preferences successfully.");
            }).catch((err) => {
                console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## Options<sup>10+</sup>

Represents the configuration options of a **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

| Name       | Type  | Mandatory| Description                                                        |
| ----------- | ------ | ---- | ------------------------------------------------------------ |
| name        | string | Yes  | Name of the **Preferences** instance.                                     |

## Preferences

Provides APIs for obtaining and modifying the stored data.

Before calling any method of **Preferences**, you must obtain a **Preferences** instance by using [data_preferences.getPreferences](#data_preferencesgetpreferences).


### get

get(key: string, defValue: ValueType, callback: AsyncCallback&lt;ValueType&gt;): void

Obtains the value corresponding to the specified key from the cached **Preferences** instance. This API uses an asynchronous callback to return the result. If the value is null or is not of the default value type, **defValue** is returned.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                                        | Mandatory| Description                                                        |
| -------- | -------------------------------------------- | ---- | ------------------------------------------------------------ |
| key      | string                                       | Yes  | Key of the data to obtain. It cannot be empty.                             |
| defValue | [ValueType](#valuetype)                      | Yes  | Default value to be returned. The value can be a number, a string, a Boolean value, or an array of numbers, strings, or Boolean values.|
| callback | AsyncCallback&lt;[ValueType](#valuetype)&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined** and **data** is the value obtained. Otherwise, **err** is an error object.|

**Example**

```js
try {
    preferences.get('startup', 'default', function (err, val) {
        if (err) {
            console.info("Failed to obtain value of 'startup'. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Obtained the value of 'startup' successfully. val: " + val);
    })
} catch (err) {
    console.info("Failed to obtain value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```

### get

get(key: string, defValue: ValueType): Promise&lt;ValueType&gt;

Obtains the value corresponding to the specified key from the cached **Preferences** instance. This API uses a promise to return the result. If the value is null or is not of the default value type, **defValue** is returned.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

 **Parameters**

| Name  | Type                   | Mandatory| Description                                                        |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| key      | string                  | Yes  | Key of the data to obtain. It cannot be empty.                             |
| defValue | [ValueType](#valuetype) | Yes  | Default value to be returned. The value can be a number, a string, a Boolean value, or an array of numbers, strings, or Boolean values.|

**Return value**

| Type                               | Description                         |
| ----------------------------------- | ----------------------------- |
| Promise<[ValueType](#valuetype)&gt; | Promise used to return the value obtained.|

**Example**

```js
try {
    let promise = preferences.get('startup', 'default');
    promise.then((data) => {
        console.info("Got the value of 'startup'. Data: " + data);
    }).catch((err) => {
        console.info("Failed to obtain value of 'startup'. code =" + err.code + ", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to obtain value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```

### getSync<sup>10+</sup>

getSync(key: string, defValue: ValueType): ValueType

Synchronously obtains the value corresponding to the specified key from the cached **Preferences** instance. If the value is null or is not of the default value type, **defValue** is returned.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                   | Mandatory| Description                                                        |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| key      | string                  | Yes  | Key of the data to obtain. It cannot be empty.                             |
| defValue | [ValueType](#valuetype) | Yes  | Default value to be returned. The value can be a number, a string, a Boolean value, or an array of numbers, strings, or Boolean values.|

**Return value**

| Type                               | Description                         |
| ----------------------------------- | ----------------------------- |
| [ValueType](#valuetype) | Returns the value obtained.|

**Example**

```js
try {
    let value = preferences.getSync('startup', 'default');
    console.info("Obtained the value of 'startup'. Data: " + value);
} catch(err) {
    console.info("Failed to obtain value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```

### getAll

getAll(callback: AsyncCallback&lt;Object&gt;): void;

Obtains all KV pairs from the cached **Preferences** instance. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                       | Mandatory| Description                                                        |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback&lt;Object&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined** and **value** provides all KV pairs obtained. Otherwise, **err** is an error object.|

**Example**

```js
try {
    preferences.getAll(function (err, value) {
        if (err) {
            console.info("Failed to obtain all KV pairs. code =" + err.code + ", message =" + err.message);
            return;
        }
    let allKeys = Object.keys(value);
    console.info("getAll keys = " + allKeys);
    console.info("getAll object = " + JSON.stringify(value));
    })
} catch (err) {
    console.info("Failed to obtain all KV pairs. code =" + err.code + ", message =" + err.message);
}
```

### getAll

getAll(): Promise&lt;Object&gt;

Obtains all KV pairs from the cached **Preferences** instance. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Return value**

| Type                 | Description                                       |
| --------------------- | ------------------------------------------- |
| Promise&lt;Object&gt; | Promise used to return the KV pairs obtained.|

**Example**

```js
try {
    let promise = preferences.getAll();
    promise.then((value) => {
        let allKeys = Object.keys(value);
        console.info('getAll keys = ' + allKeys);
        console.info("getAll object = " + JSON.stringify(value));
    }).catch((err) => {
        console.info("Failed to obtain all KV pairs. code =" + err.code + ", message =" + err.message);
    })
} catch (err) {
    console.info("Failed to obtain all KV pairs. code =" + err.code + ", message =" + err.message);
}
```

### getAllSync<sup>10+</sup>

getAllSync(): Object

Obtains all KV pairs from the cached **Preferences** instance synchronously.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Return value**

| Type                 | Description                                       |
| --------------------- | ------------------------------------------- |
| Object | Returns all KV pairs obtained.|

**Example**

```js
try {
    let value = preferences.getAllSync();
    let allKeys = Object.keys(value);
    console.info('getAll keys = ' + allKeys);
    console.info("getAll object = " + JSON.stringify(value));
} catch (err) {
    console.info("Failed to obtain all KV pairs. code =" + err.code + ", message =" + err.message);
}
```

### put

put(key: string, value: ValueType, callback: AsyncCallback&lt;void&gt;): void

Writes data to the cached **Preferences** instance. This API uses an asynchronous callback to return the result. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                                                        |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| key      | string                    | Yes  | Key of the data. It cannot be empty.                               |
| value    | [ValueType](#valuetype)   | Yes  | Value to write. The value can be a number, a string, a Boolean value, or an array of numbers, strings, or Boolean values.|
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result. If data is written successfully, **err** is **undefined**. Otherwise, **err** is an error object.    |

**Example**

```js
try {
    preferences.put('startup', 'auto', function (err) {
        if (err) {
            console.info("Failed to put the value of 'startup'. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Successfully put the value of 'startup'.");
    })
} catch (err) {
    console.info("Failed to put the value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### put

put(key: string, value: ValueType): Promise&lt;void&gt;

Writes data to the cached **Preferences** instance. This API uses a promise to return the result. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name| Type                   | Mandatory| Description                                                        |
| ------ | ----------------------- | ---- | ------------------------------------------------------------ |
| key    | string                  | Yes  | Key of the data. It cannot be empty.                               |
| value  | [ValueType](#valuetype) | Yes  | Value to write. The value can be a number, a string, a Boolean value, or an array of numbers, strings, or Boolean values.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Example**

```js
try {
    let promise = preferences.put('startup', 'auto');
    promise.then(() => {
        console.info("Successfully put the value of 'startup'.");
    }).catch((err) => {
        console.info("Failed to put the value of 'startup'. code =" + err.code +", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to put the value of 'startup'. code =" + err.code +", message =" + err.message);
}
```


### putSync<sup>10+</sup>

putSync(key: string, value: ValueType): void

Synchronously writes data to the cached **Preferences** instance. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name| Type                   | Mandatory| Description                                                        |
| ------ | ----------------------- | ---- | ------------------------------------------------------------ |
| key    | string                  | Yes  | Key of the data. It cannot be empty.                               |
| value  | [ValueType](#valuetype) | Yes  | Value to write. The value can be a number, a string, a Boolean value, or an array of numbers, strings, or Boolean values.|

**Example**

```js
try {
    preferences.putSync('startup', 'auto');
} catch(err) {
    console.info("Failed to put the value of 'startup'. code =" + err.code +", message =" + err.message);
}
```


### has

has(key: string, callback: AsyncCallback&lt;boolean&gt;): void

Checks whether the cached **Preferences** instance contains the KV pair of the given key. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                        | Mandatory| Description                                                        |
| -------- | ---------------------------- | ---- | ------------------------------------------------------------ |
| key      | string                       | Yes  | Key of the data to check. It cannot be empty.                             |
| callback | AsyncCallback&lt;boolean&gt; | Yes  | Callback invoked to return the result. If the **Preferences** instance contains the KV pair, **true** will be returned. Otherwise, **false** will be returned.|

**Example**

```js
try {
    preferences.has('startup', function (err, val) {
        if (err) {
            console.info("Failed to check the key 'startup'. code =" + err.code + ", message =" + err.message);
            return;
        }
        if (val) {
            console.info("The key 'startup' is contained.");
        } else {
            console.info("The key 'startup' is not contained.");
        }
  })
} catch (err) {
    console.info("Failed to check the key 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### has

has(key: string): Promise&lt;boolean&gt;

Checks whether the cached **Preferences** instance contains the KV pair of the given key. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name| Type  | Mandatory| Description                           |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | Yes  | Key of the data to check. It cannot be empty.|

**Return value**

| Type                  | Description                                                        |
| ---------------------- | ------------------------------------------------------------ |
| Promise&lt;boolean&gt; | Promise used to return the result. If the **Preferences** instance contains the KV pair, **true** will be returned. Otherwise, **false** will be returned.|

**Example**

```js
try {
    let promise = preferences.has('startup');
    promise.then((val) => {
        if (val) {
            console.info("The key 'startup' is contained.");
        } else {
            console.info("The key 'startup' is not contained.");
        }
    }).catch((err) => {
        console.info("Failed to check the key 'startup'. code =" + err.code + ", message =" + err.message);
  })
} catch(err) {
    console.info("Failed to check the key 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### hasSync<sup>10+</sup>

hasSync(key: string): boolean

Synchronously checks whether the cached **Preferences** instance contains the KV pair of the given key.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name| Type  | Mandatory| Description                           |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | Yes  | Key of the data to check. It cannot be empty.|

**Return value**

| Type                  | Description                                                        |
| ---------------------- | ------------------------------------------------------------ |
| boolean | If the **Preferences** instance contains the KV pair, **true** will be returned. Otherwise, **false** will be returned.|

**Example**

```js
try {
    let isExist = preferences.hasSync('startup');
    if (isExist) {
        console.info("The key 'startup' is contained.");
    } else {
        console.info("The key 'startup' is not contained.");
    }
} catch(err) {
    console.info("Failed to check the key 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### delete

delete(key: string, callback: AsyncCallback&lt;void&gt;): void

Deletes a KV pair from the cached **Preferences** instance based on the specified key. This API uses an asynchronous callback to return the result. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                                                |
| -------- | ------------------------- | ---- | ---------------------------------------------------- |
| key      | string                    | Yes  | Key of the KV pair to delete. It cannot be empty.                     |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.|

**Example**

```js
try {
    preferences.delete('startup', function (err) {
        if (err) {
            console.info("Failed to delete the key 'startup'. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Deleted the key 'startup'.");
    })
} catch (err) {
    console.info("Failed to delete the key 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### delete

delete(key: string): Promise&lt;void&gt;

Deletes a KV pair from the cached **Preferences** instance based on the specified key. This API uses a promise to return the result. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name| Type  | Mandatory| Description                           |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | Yes  | Key of the KV pair to delete. It cannot be empty.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Example**

```js
try {
    let promise = preferences.delete('startup');
	promise.then(() => {
        console.info("Deleted the key 'startup'.");
    }).catch((err) => {
        console.info("Failed to delete the key 'startup'. code =" + err.code +", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to delete the key 'startup'. code =" + err.code +", message =" + err.message);
}
```


### deleteSync<sup>10+</sup>

deleteSync(key: string): void

Synchronously deletes a KV pair from the cached **Preferences** instance based on the specified key. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name| Type  | Mandatory| Description                           |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | Yes  | Key of the KV pair to delete. It cannot be empty.|

**Example**

```js
try {
    preferences.deleteSync('startup');
} catch(err) {
    console.info("Failed to delete the key 'startup'. code =" + err.code +", message =" + err.message);
}
```


### flush

flush(callback: AsyncCallback&lt;void&gt;): void

Flushes the data in the cached **Preferences** instance to the persistent file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                                                |
| -------- | ------------------------- | ---- | ---------------------------------------------------- |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.|

**Example**

```js
try {
    preferences.flush(function (err) {
        if (err) {
            console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Successfully flushed data.");
    })
} catch (err) {
    console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
}
```


### flush

flush(): Promise&lt;void&gt;

Flushes the data in the cached **Preferences** instance to the persistent file. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Example**

```js
try {
    let promise = preferences.flush();
    promise.then(() => {
        console.info("Successfully flushed data.");
    }).catch((err) => {
        console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
    })
} catch (err) {
    console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
}
```


### clear

clear(callback: AsyncCallback&lt;void&gt;): void

Clears all data in the cached **Preferences** instance. This API uses an asynchronous callback to return the result. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                                                |
| -------- | ------------------------- | ---- | ---------------------------------------------------- |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.|

**Example**

```js
try {
	preferences.clear(function (err) {
        if (err) {
            console.info("Failed to clear data. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Successfully cleared data.");
    })
} catch (err) {
    console.info("Failed to clear data. code =" + err.code + ", message =" + err.message);
}
```


### clear

clear(): Promise&lt;void&gt;

Clears all data in the cached **Preferences** instance. This API uses a promise to return the result. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Example**

```js
try {
    let promise = preferences.clear();
	promise.then(() => {
    	console.info("Successfully cleared data.");
    }).catch((err) => {
        console.info("Failed to clear data. code =" + err.code + ", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to clear data. code =" + err.code + ", message =" + err.message);
}
```


### clearSync<sup>10+</sup>

clearSync(): void

Synchronously clears all data in the cached **Preferences** instance. You can use [flush](#flush) to persist the **Preferences** instance.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Example**

```js
try {
    preferences.clearSync();
} catch(err) {
    console.info("Failed to clear data. code =" + err.code + ", message =" + err.message);
}
```


### on('change')

on(type: 'change', callback: Callback&lt;{ key : string }&gt;): void

Subscribes to data changes. A callback will be triggered to return the new value if the value of the subscribed key is changed and [flushed](#flush).

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                            | Mandatory| Description                                    |
| -------- | -------------------------------- | ---- | ---------------------------------------- |
| type     | string                           | Yes  | Event type to subscribe to. The value **change** indicates data change events.|
| callback | Callback&lt;{ key : string }&gt; | Yes  | Callback invoked to return data changes.                          |

**Example**

```js
try {
	data_preferences.getPreferences(this.context, 'mystore', function (err, preferences) {
		if (err) {
			console.info("Failed to obtain the preferences.");
			return;
		}
		let observer = function (key) {
			console.info("The key " + key + " changed.");
		}
		preferences.on('change', observer);
		preferences.put('startup', 'manual', function (err) {
			if (err) {
				console.info("Failed to put the value of 'startup'. Cause: " + err);
				return;
			}
			console.info("Successfully put the value of 'startup'.");

			preferences.flush(function (err) {
				if (err) {
					console.info("Failed to flush data. Cause: " + err);
					return;
				}
				console.info("Successfully flushed data.");
			})
		})
	})
} catch (err) {
	console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
}
```

### on('multiProcessChange')<sup>10+</sup>

on(type: 'multiProcessChange', callback: Callback&lt;{ key : string }&gt;): void

Subscribes to inter-process data changes. For the multiple processes holding the same preference file, if the value of the subscribed key changes in any process, the callback in this API will be invoked after [flush()](#flush) is executed.

This API can be used with [removePreferencesFromCache](#data_preferencesremovepreferencesfromcache) to update the **Preferences** instance in the callback when detecting that a process updates a file. For details, see example 2.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                            | Mandatory| Description                                                          |
| -------- | -------------------------------- | ---- | -------------------------------------------------------------- |
| type     | string                           | Yes  | Event type. The value is **multiProcessChange**, which indicates data changes between multiple processes.|
| callback | Callback&lt;{ key : string }&gt; | Yes  | Callback invoked to return data changes.                                                |

**Error codes**

For details about the error codes, see [User Preference Error Codes](../errorcodes/errorcode-preferences.md).

| ID| Error Message                              |
| -------- | -------------------------------------- |
| 15500019 | Failed to obtain subscription service. |

**Example 1**

```js
try {
	data_preferences.getPreferences(this.context, {name: 'mystore'}, function (err, preferences) {
		if (err) {
			console.info("Failed to obtain the preferences.");
			return;
		}
		let observer = function (key) {
			console.info("The key " + key + " changed.");
		}
		preferences.on('multiProcessChange', observer);
		preferences.put('startup', 'manual', function (err) {
			if (err) {
				console.info("Failed to put the value of 'startup'. Cause: " + err);
				return;
			}
			console.info("Successfully put the value of 'startup'.");

			preferences.flush(function (err) {
				if (err) {
					console.info("Failed to flush data. Cause: " + err);
					return;
				}
				console.info("Successfully flushed data.");
			})
		})
	})
} catch (err) {
	console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
}
```

**Example 2**

```js
let preferences = null;
try {
    data_preferences.getPreferences(this.context, { name: 'mystore' }, function (err, val) {
        if (err) {
            console.info("Failed to obtain the preferences.");
            return;
        }
        preferences = val;
        let observer = function (key) {
            console.info("The key " + key + " changed.");
            try {
                data_preferences.removePreferencesFromCache(context, { name: 'mystore' }, function (err) {
                    if (err) {
                        console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
                        return;
                    }
                    preferences = null;
                    console.info("Removed the preferences successfully.");
                })
            } catch (err) {
                console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
            }

            try {
                data_preferences.getPreferences(context, { name: 'mystore' }, function (err, val) {
                    if (err) {
                        console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
                        return;
                    }
                    preferences = val;
                    console.info("Obtained the preferences successfully.");
                })
            } catch (err) {
                console.info("Failed to obtain the preferences. code =" + err.code + ", message =" + err.message);
            }
        }
        preferences.on('multiProcessChange', observer);
        preferences.put('startup', 'manual', function (err) {
            if (err) {
                console.info("Failed to put the value of 'startup'. Cause: " + err);
                return;
            }
            console.info("Successfully put the value of 'startup'.");

            preferences.flush(function (err) {
                if (err) {
                    console.info("Failed to flush data. Cause: " + err);
                    return;
                }
                console.info("Successfully flushed data.");
            })
        })
    })
} catch (err) {
    console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
}
```

### off('change')

off(type: 'change', callback?: Callback&lt;{ key : string }&gt;): void

Unsubscribes from data changes.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                            | Mandatory| Description                                      |
| -------- | -------------------------------- | ---- | ------------------------------------------ |
| type     | string                           | Yes  | Event type to unsubscribe from. The value **change** indicates data change events.  |
| callback | Callback&lt;{ key : string }&gt; | No  | Callback to unregister. If this parameter is left blank, the callbacks for all data changes will be unregistered.|

**Example**

```js
try {
    data_preferences.getPreferences(this.context, 'mystore', function (err, preferences) {
        if (err) {
            console.info("Failed to obtain the preferences.");
            return;
        }
        let observer = function (key) {
            console.info("The key " + key + " changed.");
        }
        preferences.on('change', observer);
        preferences.put('startup', 'auto', function (err) {
            if (err) {
                console.info("Failed to put the value of 'startup'. Cause: " + err);
                return;
            }
            console.info("Successfully put the value of 'startup'.");

            preferences.flush(function (err) {
                if (err) {
                    console.info("Failed to flush data. Cause: " + err);
                    return;
                }
                console.info("Successfully flushed data.");
            })
            preferences.off('change', observer);
        })
    })
} catch (err) {
    console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
}
```

### off('multiProcessChange')<sup>10+</sup>

off(type: 'multiProcessChange', callback?: Callback&lt;{ key : string }&gt;): void

Unsubscribes from inter-process data changes.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

**Parameters**

| Name  | Type                            | Mandatory| Description                                                          |
| -------- | -------------------------------- | ---- | -------------------------------------------------------------- |
| type     | string                           | Yes  | Event type. The value is **multiProcessChange**, which indicates data changes between multiple processes.|
| callback | Callback&lt;{ key : string }&gt; | No  | Callback to unregister. If this parameter is left blank, all callbacks for **multiProcessChange** will be unregistered.                    |

**Example**

```js
try {
    data_preferences.getPreferences(this.context, {name: 'mystore'}, function (err, preferences) {
        if (err) {
            console.info("Failed to obtain the preferences.");
            return;
        }
        let observer = function (key) {
            console.info("The key " + key + " changed.");
        }
        preferences.on('multiProcessChange', observer);
        preferences.put('startup', 'auto', function (err) {
            if (err) {
                console.info("Failed to put the value of 'startup'. Cause: " + err);
                return;
            }
            console.info("Successfully put the value of 'startup'.");

            preferences.flush(function (err) {
                if (err) {
                    console.info("Failed to flush data. Cause: " + err);
                    return;
                }
                console.info("Successfully flushed data.");
            })
            preferences.off('multiProcessChange', observer);
        })
    })
} catch (err) {
    console.info("Failed to flush data. code =" + err.code + ", message =" + err.message);
}
```
## ValueType

Enumerates the value types.

**System capability**: SystemCapability.DistributedDataManager.Preferences.Core

| Type           | Description                          |
| --------------- | ------------------------------ |
| number          | The value is a number.            |
| string          | The value is a string.          |
| boolean         | The value is of Boolean type.          |
| Array\<number>  | The value is an array of numbers.  |
| Array\<boolean> | The value is a Boolean array.  |
| Array\<string>  | The value is an array of strings.|