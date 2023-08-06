# @ohos.data.preferences (用户首选项)

用户首选项为应用提供Key-Value键值型的数据处理能力，支持应用持久化轻量级数据，并对其修改和查询。

数据存储形式为键值对，键的类型为字符串型，值的存储数据类型包括数字型、字符型、布尔型以及这3种类型的数组类型。


> **说明：**
>
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```js
import data_preferences from '@ohos.data.preferences';
```

## 常量

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

| 名称             | 参数类型 | 可读 | 可写 | 说明                                    |
| ---------------- | -------- | ---- | ---- | --------------------------------------- |
| MAX_KEY_LENGTH   | number   | 是   | 否   | Key的最大长度限制为80个字节。     |
| MAX_VALUE_LENGTH | number   | 是   | 否   | Value的最大长度限制为8192个字节。 |


## data_preferences.getPreferences

getPreferences(context: Context, name: string, callback: AsyncCallback&lt;Preferences&gt;): void

获取Preferences实例，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                                             | 必填 | 说明                                                         |
| -------- | ------------------------------------------------ | ---- | ------------------------------------------------------------ |
| context  | Context            | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-context.md)。                                                 |
| name     | string                                           | 是   | Preferences实例的名称。                                      |
| callback | AsyncCallback&lt;[Preferences](#preferences)&gt; | 是   | 回调函数。当获取Preferences实例成功，err为undefined，返回Preferences实例；否则err为错误对象。 |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.getPreferences(this.context, 'mystore', function (err, val) {
                if (err) {
                    console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                preferences = val;
                console.info("Succeeded in getting preferences.");
            })
        } catch (err) {
            console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.getPreferences

getPreferences(context: Context, name: string): Promise&lt;Preferences&gt;

获取Preferences实例，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名  | 类型                                  | 必填 | 说明                    |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-context.md)。            |
| name    | string                                | 是   | Preferences实例的名称。 |

**返回值：**

| 类型                                       | 说明                               |
| ------------------------------------------ | ---------------------------------- |
| Promise&lt;[Preferences](#preferences)&gt; | Promise对象，返回Preferences实例。 |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.getPreferences(this.context, 'mystore');
            promise.then((object) => {
                preferences = object;
                console.info("Succeeded in getting preferences.");
            }).catch((err) => {
                console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.getPreferences<sup>10+</sup>

getPreferences(context: Context, options: Options, callback: AsyncCallback&lt;Preferences&gt;): void

获取Preferences实例，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                                                                                                                                                                           |
| -------- | --------------------------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context  | Context                                       | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-uiAbilityContext.md)。 |
| options  | [Options](#options10)                              | 是   | 与Preferences实例相关的配置选项。                                                                                                                                              |
| callback | AsyncCallback&lt;[Preferences](#preferences)&gt; | 是   | 回调函数。当获取Preferences实例成功，err为undefined，返回Preferences实例；否则err为错误对象。                                                                                    |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.getPreferences(this.context, {name: 'mystore'}, function (err, val) {
                if (err) {
                    console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                preferences = val;
                console.info("Succeeded in getting preferences.");
            })
        } catch (err) {
            console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.getPreferences<sup>10+</sup>

getPreferences(context: Context, options: Options): Promise&lt;Preferences&gt;

获取Preferences实例，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名  | 类型             | 必填 | 说明                                                                                                                                                                           |
| ------- | ---------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context | Context          | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-uiAbilityContext.md)。 |
| options | [Options](#options10) | 是   | 与Preferences实例相关的配置选项。                                                                                                                                              |

**返回值：**

| 类型                                    | 说明                               |
| --------------------------------------- | ---------------------------------- |
| Promise&lt;[Preferences](#preferences)&gt; | Promise对象，返回Preferences实例。 |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let preferences = null;

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.getPreferences(this.context, {name: 'mystore'});
            promise.then((object) => {
                preferences = object;
                console.info("Succeeded in getting preferences.");
            }).catch((err) => {
                console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences

deletePreferences(context: Context, name: string, callback: AsyncCallback&lt;void&gt;): void

从缓存中移出指定的Preferences实例，若Preferences实例有对应的持久化文件，则同时删除其持久化文件。使用callback异步回调。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                                  | 必填 | 说明                                                 |
| -------- | ------------------------------------- | ---- | ---------------------------------------------------- |
| context  | Context | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-context.md)。                                         |
| name     | string                                | 是   | Preferences实例的名称。                              |
| callback | AsyncCallback&lt;void&gt;             | 是   | 回调函数。当移除成功，err为undefined，否则为错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                       |
| -------- | ------------------------------|
| 15500010 | Failed to delete preferences file. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.deletePreferences(this.context, 'mystore', function (err) {
                if (err) {
                    console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                console.info("Succeeded in deleting preferences." );
            })
        } catch (err) {
            console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences

deletePreferences(context: Context, name: string): Promise&lt;void&gt;

从缓存中移出指定的Preferences实例，若Preferences实例有对应的持久化文件，则同时删除其持久化文件。使用Promise异步回调。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名  | 类型                                  | 必填 | 说明                    |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-context.md)。            |
| name    | string                                | 是   | Preferences实例的名称。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                       |
| -------- | ------------------------------|
| 15500010 | Failed to delete preferences file. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try{
            let promise = data_preferences.deletePreferences(this.context, 'mystore');
            promise.then(() => {
                console.info("Succeeded in deleting preferences.");
            }).catch((err) => {
                console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences<sup>10+</sup>

deletePreferences(context: Context, options: Options, callback: AsyncCallback&lt;void&gt;): void

从缓存中移出指定的Preferences实例，若Preferences实例有对应的持久化文件，则同时删除其持久化文件。使用callback异步回调。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                                                                                                                                           |
| -------- | ------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context  | Context                   | 是   | 应用上下文。<br>FA模型的应用Context定义见[Context](js-apis-inner-app-context.md)。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-uiAbilityContext.md)。 |
| options  | [Options](#options10)          | 是   | 与Preferences实例相关的配置选项。                                                                                                                                              |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数。当移除成功，err为undefined，否则为错误对象。                                                                                                                           |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                           |
| -------- | ---------------------------------- |
| 15500010 | Failed to delete preferences file. |
| 15501001 | Only supported in stage mode. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            data_preferences.deletePreferences(this.context, {name: 'mystore'}, function (err) {
                if (err) {
                    console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
                    return;
                }
                console.info("Succeeded in deleting preferences." );
            })
        } catch (err) {
            console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.deletePreferences<sup>10+</sup>

deletePreferences(context: Context, options: Options): Promise&lt;void&gt;

从缓存中移出指定的Preferences实例，若Preferences实例有对应的持久化文件，则同时删除其持久化文件。使用Promise异步回调。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名  | 类型             | 必填 | 说明                                                                                                                                                                           |
| ------- | ---------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context | Context          | 是   | 应用上下文。<br>FA模型的应用Context定义见[Context](js-apis-inner-app-context.md)。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-uiAbilityContext.md)。 |
| options | [Options](#options10) | 是   | 与Preferences实例相关的配置选项。                                                                                                                                              |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                           |
| -------- | ---------------------------------- |
| 15500010 | Failed to delete preferences file. |
| 15501001 | Only supported in stage mode. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try{
            let promise = data_preferences.deletePreferences(this.context, {name: 'mystore'});
            promise.then(() => {
                console.info("Succeeded in deleting preferences.");
            }).catch((err) => {
                console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
            })
        } catch(err) {
            console.info("Failed to delete preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.removePreferencesFromCache

removePreferencesFromCache(context: Context, name: string, callback: AsyncCallback&lt;void&gt;): void

从缓存中移出指定的Preferences实例，使用callback异步回调。

应用首次调用[getPreferences](#data_preferencesgetpreferences)接口获取某个Preferences实例后，该实例会被会被缓存起来，后续再次[getPreferences](#data_preferencesgetpreferences)时不会再次从持久化文件中读取，直接从缓存中获取Preferences实例。调用此接口移出缓存中的实例之后，再次getPreferences将会重新读取持久化文件，生成新的Preferences实例。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                                  | 必填 | 说明                                                 |
| -------- | ------------------------------------- | ---- | ---------------------------------------------------- |
| context  | Context | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-context.md)。                                         |
| name     | string                                | 是   | Preferences实例的名称。                              |
| callback | AsyncCallback&lt;void&gt;             | 是   | 回调函数。当移除成功，err为undefined，否则为错误对象。 |

**示例：**

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
                console.info("Succeeded in removing preferences.");
            })
        } catch (err) {
            console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}

```

## data_preferences.removePreferencesFromCache

removePreferencesFromCache(context: Context, name: string): Promise&lt;void&gt;

从缓存中移出指定的Preferences实例，使用Promise异步回调。

应用首次调用[getPreferences](#data_preferencesgetpreferences)接口获取某个Preferences实例后，该实例会被会被缓存起来，后续再次[getPreferences](#data_preferencesgetpreferences)时不会再次从持久化文件中读取，直接从缓存中获取Preferences实例。调用此接口移出缓存中的实例之后，再次getPreferences将会重新读取持久化文件，生成新的Preferences实例。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名  | 类型                                  | 必填 | 说明                    |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-context.md)。            |
| name    | string                                | 是   | Preferences实例的名称。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.removePreferencesFromCache(this.context, 'mystore');
            promise.then(() => {
                console.info("Succeeded in removing preferences.");
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

从缓存中移出指定的Preferences实例，此为同步接口。

应用首次调用[getPreferences](#data_preferencesgetpreferences)接口获取某个Preferences实例后，该实例会被会被缓存起来，后续再次[getPreferences](#data_preferencesgetpreferences)时不会再次从持久化文件中读取，直接从缓存中获取Preferences实例。调用此接口移出缓存中的实例之后，再次getPreferences将会重新读取持久化文件，生成新的Preferences实例。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名  | 类型                                  | 必填 | 说明                    |
| ------- | ------------------------------------- | ---- | ----------------------- |
| context | Context | 是   | 应用上下文。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-context.md)。            |
| name    | string                                | 是   | Preferences实例的名称。 |

**示例：**

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

从缓存中移出指定的Preferences实例，使用callback异步回调。

应用首次调用[getPreferences](#data_preferencesgetpreferences)接口获取某个Preferences实例后，该实例会被会被缓存起来，后续再次[getPreferences](#data_preferencesgetpreferences)时不会再次从持久化文件中读取，直接从缓存中获取Preferences实例。调用此接口移出缓存中的实例之后，再次getPreferences将会重新读取持久化文件，生成新的Preferences实例。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                                                                                                                                           |
| -------- | ------------------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context  | Context                   | 是   | 应用上下文。<br>FA模型的应用Context定义见[Context](js-apis-inner-app-context.md)。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-uiAbilityContext.md)。 |
| options  | [Options](#options10)          | 是   | 与Preferences实例相关的配置选项。                                                                                                                                              |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数。当移除成功，err为undefined，否则为错误对象。                                                                                                                           |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**示例：**

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
                console.info("Succeeded in removing preferences.");
            })
        } catch (err) {
            console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
        }
    }
}
```

## data_preferences.removePreferencesFromCache<sup>10+</sup>

removePreferencesFromCache(context: Context, options: Options): Promise&lt;void&gt;

从缓存中移出指定的Preferences实例，使用Promise异步回调。

应用首次调用[getPreferences](#data_preferencesgetpreferences)接口获取某个Preferences实例后，该实例会被会被缓存起来，后续再次[getPreferences](#data_preferencesgetpreferences)时不会再次从持久化文件中读取，直接从缓存中获取Preferences实例。调用此接口移出缓存中的实例之后，再次getPreferences将会重新读取持久化文件，生成新的Preferences实例。

调用该接口后，不建议再使用旧的Preferences实例进行数据操作，否则会出现数据一致性问题，应将Preferences实例置为null，系统将会统一回收。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名  | 类型             | 必填 | 说明                                                                                                                                                                           |
| ------- | ---------------- | ---- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| context | Context          | 是   | 应用上下文。<br>FA模型的应用Context定义见[Context](js-apis-inner-app-context.md)。<br>Stage模型的应用Context定义见[Context](js-apis-inner-application-uiAbilityContext.md)。 |
| options | [Options](#options10) | 是   | 与Preferences实例相关的配置选项。                                                                                                                                              |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 15501001 | Only supported in stage mode. |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        try {
            let promise = data_preferences.removePreferencesFromCache(this.context, {name: 'mystore'});
            promise.then(() => {
                console.info("Succeeded in removing preferences.");
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

Preferences实例配置选项。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

| 名称        | 类型   | 必填 | 说明                                                         |
| ----------- | ------ | ---- | ------------------------------------------------------------ |
| name        | string | 是   | Preferences实例的名称。                                      |

## Preferences

首选项实例，提供获取和修改存储数据的接口。

下列接口都需先使用[data_preferences.getPreferences](#data_preferencesgetpreferences)获取到Preferences实例，再通过此实例调用对应接口。


### get

get(key: string, defValue: ValueType, callback: AsyncCallback&lt;ValueType&gt;): void

从缓存的Preferences实例中获取键对应的值，如果值为null或者非默认值类型，返回默认数据defValue，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                                                         |
| -------- | -------------------------------------------- | ---- | ------------------------------------------------------------ |
| key      | string                                       | 是   | 要获取的存储Key名称，不能为空。                              |
| defValue | [ValueType](#valuetype)                      | 是   | 默认返回值。支持number、string、boolean、Array\<number>、Array\<string>、Array\<boolean>类型。 |
| callback | AsyncCallback&lt;[ValueType](#valuetype)&gt; | 是   | 回调函数。当获取成功时，err为undefined，data为键对应的值；否则err为错误对象。 |

**示例：**

```js
try {
    preferences.get('startup', 'default', function (err, val) {
        if (err) {
            console.info("Failed to get value of 'startup'. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Succeeded in getting value of 'startup'. val： " + val);
    })
} catch (err) {
    console.info("Failed to get value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```

### get

get(key: string, defValue: ValueType): Promise&lt;ValueType&gt;

从缓存的Preferences实例中获取键对应的值，如果值为null或者非默认值类型，返回默认数据defValue，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

 **参数：**

| 参数名   | 类型                    | 必填 | 说明                                                         |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| key      | string                  | 是   | 要获取的存储Key名称，不能为空。                              |
| defValue | [ValueType](#valuetype) | 是   | 默认返回值。支持number、string、boolean、Array\<number>、Array\<string>、Array\<boolean>类型。 |

**返回值：**

| 类型                                | 说明                          |
| ----------------------------------- | ----------------------------- |
| Promise<[ValueType](#valuetype)&gt; | Promise对象，返回键对应的值。 |

**示例：**

```js
try {
    let promise = preferences.get('startup', 'default');
    promise.then((data) => {
        console.info("Succeeded in getting value of 'startup'. Data: " + data);
    }).catch((err) => {
        console.info("Failed to get value of 'startup'. code =" + err.code + ", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to get value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```

### getSync<sup>10+</sup>

getSync(key: string, defValue: ValueType): ValueType

从缓存的Preferences实例中获取键对应的值，如果值为null或者非默认值类型，返回默认数据defValue，此为同步接口。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                         |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| key      | string                  | 是   | 要获取的存储Key名称，不能为空。                              |
| defValue | [ValueType](#valuetype) | 是   | 默认返回值。支持number、string、boolean、Array\<number>、Array\<string>、Array\<boolean>类型。 |

**返回值：**

| 类型                                | 说明                          |
| ----------------------------------- | ----------------------------- |
| [ValueType](#valuetype) | 返回键对应的值。 |

**示例：**

```js
try {
    let value = preferences.getSync('startup', 'default');
    console.info("Succeeded in getting value of 'startup'. Data: " + value);
} catch(err) {
    console.info("Failed to get value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```

### getAll

getAll(callback: AsyncCallback&lt;Object&gt;): void;

从缓存的Preferences实例中获取所有键值数据。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                                         |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback&lt;Object&gt; | 是   | 回调函数。当获取成功，err为undefined，value为所有键值数据；否则err为错误对象。 |

**示例：**

```js
try {
    preferences.getAll(function (err, value) {
        if (err) {
            console.info("Failed to get all key-values. code =" + err.code + ", message =" + err.message);
            return;
        }
    let allKeys = Object.keys(value);
    console.info("getAll keys = " + allKeys);
    console.info("getAll object = " + JSON.stringify(value));
    })
} catch (err) {
    console.info("Failed to get all key-values. code =" + err.code + ", message =" + err.message);
}
```

### getAll

getAll(): Promise&lt;Object&gt;

从缓存的Preferences实例中获取所有键值数据。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**返回值：**

| 类型                  | 说明                                        |
| --------------------- | ------------------------------------------- |
| Promise&lt;Object&gt; | Promise对象，返回含有所有键值数据。 |

**示例：**

```js
try {
    let promise = preferences.getAll();
    promise.then((value) => {
        let allKeys = Object.keys(value);
        console.info('getAll keys = ' + allKeys);
        console.info("getAll object = " + JSON.stringify(value));
    }).catch((err) => {
        console.info("Failed to get all key-values. code =" + err.code + ", message =" + err.message);
    })
} catch (err) {
    console.info("Failed to get all key-values. code =" + err.code + ", message =" + err.message);
}
```

### getAllSync<sup>10+</sup>

getAllSync(): Object

从缓存的Preferences实例中获取所有键值数据。，此为同步接口。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**返回值：**

| 类型                  | 说明                                        |
| --------------------- | ------------------------------------------- |
| Object | 返回含有所有键值数据。 |

**示例：**

```js
try {
    let value = preferences.getAllSync();
    let allKeys = Object.keys(value);
    console.info('getAll keys = ' + allKeys);
    console.info("getAll object = " + JSON.stringify(value));
} catch (err) {
    console.info("Failed to get all key-values. code =" + err.code + ", message =" + err.message);
}
```

### put

put(key: string, value: ValueType, callback: AsyncCallback&lt;void&gt;): void

将数据写入缓存的Preferences实例中，可通过[flush](#flush)将Preferences实例持久化，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                         |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| key      | string                    | 是   | 要修改的存储的Key，不能为空。                                |
| value    | [ValueType](#valuetype)   | 是   | 存储的新值。支持number、string、boolean、Array\<number>、Array\<string>、Array\<boolean>类型。 |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数。当数据写入成功，err为undefined；否则为错误对象。     |

**示例：**

```js
try {
    preferences.put('startup', 'auto', function (err) {
        if (err) {
            console.info("Failed to put value of 'startup'. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Succeeded in putting value of 'startup'.");
    })
} catch (err) {
    console.info("Failed to put value of 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### put

put(key: string, value: ValueType): Promise&lt;void&gt;

将数据写入缓存的Preferences实例中，可通过[flush](#flush)将Preferences实例持久化，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名 | 类型                    | 必填 | 说明                                                         |
| ------ | ----------------------- | ---- | ------------------------------------------------------------ |
| key    | string                  | 是   | 要修改的存储的Key，不能为空。                                |
| value  | [ValueType](#valuetype) | 是   | 存储的新值。支持number、string、boolean、Array\<number>、Array\<string>、Array\<boolean>类型。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**示例：**

```js
try {
    let promise = preferences.put('startup', 'auto');
    promise.then(() => {
        console.info("Succeeded in putting value of 'startup'.");
    }).catch((err) => {
        console.info("Failed to put value of 'startup'. code =" + err.code +", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to put value of 'startup'. code =" + err.code +", message =" + err.message);
}
```


### putSync<sup>10+</sup>

putSync(key: string, value: ValueType): void

将数据写入缓存的Preferences实例中，可通过[flush](#flush)将Preferences实例持久化，此为同步接口。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名 | 类型                    | 必填 | 说明                                                         |
| ------ | ----------------------- | ---- | ------------------------------------------------------------ |
| key    | string                  | 是   | 要修改的存储的Key，不能为空。                                |
| value  | [ValueType](#valuetype) | 是   | 存储的新值。支持number、string、boolean、Array\<number>、Array\<string>、Array\<boolean>类型。 |

**示例：**

```js
try {
    preferences.putSync('startup', 'auto');
} catch(err) {
    console.info("Failed to put value of 'startup'. code =" + err.code +", message =" + err.message);
}
```


### has

has(key: string, callback: AsyncCallback&lt;boolean&gt;): void

检查缓存的Preferences实例中是否包含名为给定Key的存储键值对，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                                         |
| -------- | ---------------------------- | ---- | ------------------------------------------------------------ |
| key      | string                       | 是   | 要检查的存储key名称，不能为空。                              |
| callback | AsyncCallback&lt;boolean&gt; | 是   | 回调函数。返回Preferences实例是否包含给定key的存储键值对，true表示存在，false表示不存在。 |

**示例：**

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
            console.info("The key 'startup' dose not contain.");
        }
  })
} catch (err) {
    console.info("Failed to check the key 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### has

has(key: string): Promise&lt;boolean&gt;

检查缓存的Preferences实例中是否包含名为给定Key的存储键值对，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                            |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | 是   | 要检查的存储key名称，不能为空。 |

**返回值：**

| 类型                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Promise&lt;boolean&gt; | Promise对象。返回Preferences实例是否包含给定key的存储键值对，true表示存在，false表示不存在。 |

**示例：**

```js
try {
    let promise = preferences.has('startup');
    promise.then((val) => {
        if (val) {
            console.info("The key 'startup' is contained.");
        } else {
            console.info("The key 'startup' dose not contain.");
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

检查缓存的Preferences实例中是否包含名为给定Key的存储键值对，此为同步接口。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                            |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | 是   | 要检查的存储key名称，不能为空。 |

**返回值：**

| 类型                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| boolean | 返回Preferences实例是否包含给定key的存储键值对，true表示存在，false表示不存在。 |

**示例：**

```js
try {
    let isExist = preferences.hasSync('startup');
    if (isExist) {
        console.info("The key 'startup' is contained.");
    } else {
        console.info("The key 'startup' dose not contain.");
    }
} catch(err) {
    console.info("Failed to check the key 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### delete

delete(key: string, callback: AsyncCallback&lt;void&gt;): void

从缓存的Preferences实例中删除名为给定Key的存储键值对，可通过[flush](#flush)将Preferences实例持久化，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                 |
| -------- | ------------------------- | ---- | ---------------------------------------------------- |
| key      | string                    | 是   | 要删除的存储Key名称，不能为空。                      |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数。当删除成功，err为undefined；否则为错误对象。 |

**示例：**

```js
try {
    preferences.delete('startup', function (err) {
        if (err) {
            console.info("Failed to delete the key 'startup'. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Succeeded in deleting the key 'startup'.");
    })
} catch (err) {
    console.info("Failed to delete the key 'startup'. code =" + err.code + ", message =" + err.message);
}
```


### delete

delete(key: string): Promise&lt;void&gt;

从缓存的Preferences实例中删除名为给定Key的存储键值对，可通过[flush](#flush)将Preferences实例持久化，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                            |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | 是   | 要删除的存储key名称，不能为空。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**示例：**

```js
try {
    let promise = preferences.delete('startup');
	promise.then(() => {
        console.info("Succeeded in deleting the key 'startup'.");
    }).catch((err) => {
        console.info("Failed to delete the key 'startup'. code =" + err.code +", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to delete the key 'startup'. code =" + err.code +", message =" + err.message);
}
```


### deleteSync<sup>10+</sup>

deleteSync(key: string): void

从缓存的Preferences实例中删除名为给定Key的存储键值对，可通过[flush](#flush)将Preferences实例持久化，此为同步接口。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明                            |
| ------ | ------ | ---- | ------------------------------- |
| key    | string | 是   | 要删除的存储key名称，不能为空。 |

**示例：**

```js
try {
    preferences.deleteSync('startup');
} catch(err) {
    console.info("Failed to delete the key 'startup'. code =" + err.code +", message =" + err.message);
}
```


### flush

flush(callback: AsyncCallback&lt;void&gt;): void

将缓存的Preferences实例中的数据异步存储到用户首选项的持久化文件中，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                 |
| -------- | ------------------------- | ---- | ---------------------------------------------------- |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数。当保存成功，err为undefined；否则为错误对象。 |

**示例：**

```js
try {
    preferences.flush(function (err) {
        if (err) {
            console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Succeeded in flushing.");
    })
} catch (err) {
    console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
}
```


### flush

flush(): Promise&lt;void&gt;

将缓存的Preferences实例中的数据异步存储到用户首选项的持久化文件中，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**示例：**

```js
try {
    let promise = preferences.flush();
    promise.then(() => {
        console.info("Succeeded in flushing.");
    }).catch((err) => {
        console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
    })
} catch (err) {
    console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
}
```


### clear

clear(callback: AsyncCallback&lt;void&gt;): void

清除缓存的Preferences实例中的所有数据，可通过[flush](#flush)将Preferences实例持久化，使用callback异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                 |
| -------- | ------------------------- | ---- | ---------------------------------------------------- |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数。当清除成功，err为undefined；否则为错误对象。 |

**示例：**

```js
try {
	preferences.clear(function (err) {
        if (err) {
            console.info("Failed to clear. code =" + err.code + ", message =" + err.message);
            return;
        }
        console.info("Succeeded in clearing.");
    })
} catch (err) {
    console.info("Failed to clear. code =" + err.code + ", message =" + err.message);
}
```


### clear

clear(): Promise&lt;void&gt;

清除缓存的Preferences实例中的所有数据，可通过[flush](#flush)将Preferences实例持久化，使用Promise异步回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**示例：**

```js
try {
    let promise = preferences.clear();
	promise.then(() => {
    	console.info("Succeeded in clearing.");
    }).catch((err) => {
        console.info("Failed to clear. code =" + err.code + ", message =" + err.message);
    })
} catch(err) {
    console.info("Failed to clear. code =" + err.code + ", message =" + err.message);
}
```


### clearSync<sup>10+</sup>

clearSync(): void

清除缓存的Preferences实例中的所有数据，可通过[flush](#flush)将Preferences实例持久化，此为同步接口。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**示例：**

```js
try {
    preferences.clearSync();
} catch(err) {
    console.info("Failed to clear. code =" + err.code + ", message =" + err.message);
}
```


### on('change')

on(type: 'change', callback: Callback&lt;{ key : string }&gt;): void

订阅数据变更，订阅的Key的值发生变更后，在执行[flush](#flush)方法后，触发callback回调。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                             | 必填 | 说明                                     |
| -------- | -------------------------------- | ---- | ---------------------------------------- |
| type     | string                           | 是   | 事件类型，固定值'change'，表示数据变更。 |
| callback | Callback&lt;{ key : string }&gt; | 是   | 回调对象实例。                           |

**示例：**

```js
try {
	data_preferences.getPreferences(this.context, 'mystore', function (err, preferences) {
		if (err) {
			console.info("Failed to get preferences.");
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
			console.info("Succeeded in putting the value of 'startup'.");

			preferences.flush(function (err) {
				if (err) {
					console.info("Failed to flush. Cause: " + err);
					return;
				}
				console.info("Succeeded in flushing.");
			})
		})
	})
} catch (err) {
	console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
}
```

### on('multiProcessChange')<sup>10+</sup>

on(type: 'multiProcessChange', callback: Callback&lt;{ key : string }&gt;): void

订阅进程间数据变更，多个进程持有同一个首选项文件时，订阅的Key的值在任意一个进程发生变更后，执行[flush](#flush)方法后，触发callback回调。

此方法可以配合[removePreferencesFromCache](#data_preferencesremovepreferencesfromcache)使用，当监听到有进程更新了文件时，在回调方法中更新当前的Preferences实例，如下示例2。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                             | 必填 | 说明                                                           |
| -------- | -------------------------------- | ---- | -------------------------------------------------------------- |
| type     | string                           | 是   | 事件类型，固定值'multiProcessChange'，表示多进程间的数据变更。 |
| callback | Callback&lt;{ key : string }&gt; | 是   | 回调对象实例。                                                 |

**错误码：**

以下错误码的详细介绍请参见[用户首选项错误码](../errorcodes/errorcode-preferences.md)。

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 15500019 | Failed to obtain subscription service. |

**示例1：**

```js
try {
	data_preferences.getPreferences(this.context, {name: 'mystore'}, function (err, preferences) {
		if (err) {
			console.info("Failed to get preferences.");
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
			console.info("Succeeded in putting the value of 'startup'.");

			preferences.flush(function (err) {
				if (err) {
					console.info("Failed to flush. Cause: " + err);
					return;
				}
				console.info("Succeeded in flushing.");
			})
		})
	})
} catch (err) {
	console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
}
```

**示例2：**

```js
let preferences = null;
try {
    data_preferences.getPreferences(this.context, {name: 'mystore'}, function (err, val) {
        if (err) {
            console.info("Failed to get preferences.");
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
                    console.info("Succeeded in removing preferences.");
                })
            } catch (err) {
                console.info("Failed to remove preferences. code =" + err.code + ", message =" + err.message);
            }

            try {
                data_preferences.getPreferences(context, { name: 'mystore' }, function (err, val) {
                    if (err) {
                        console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
                        return;
                    }
                    preferences = val;
                    console.info("Succeeded in getting preferences.");
                })
            } catch (err) {
                console.info("Failed to get preferences. code =" + err.code + ", message =" + err.message);
            }
        }
        preferences.on('multiProcessChange', observer);
        preferences.put('startup', 'manual', function (err) {
            if (err) {
                console.info("Failed to put the value of 'startup'. Cause: " + err);
                return;
            }
            console.info("Succeeded in putting the value of 'startup'.");

            preferences.flush(function (err) {
                if (err) {
                    console.info("Failed to flush. Cause: " + err);
                    return;
                }
                console.info("Succeeded in flushing.");
            })
        })
    })
} catch (err) {
    console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
}
```

### off('change')

off(type: 'change', callback?: Callback&lt;{ key : string }&gt;): void

取消订阅数据变更。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                             | 必填 | 说明                                       |
| -------- | -------------------------------- | ---- | ------------------------------------------ |
| type     | string                           | 是   | 事件类型，固定值'change'，表示数据变更。   |
| callback | Callback&lt;{ key : string }&gt; | 否   | 需要取消的回调对象实例，不填写则全部取消。 |

**示例：**

```js
try {
    data_preferences.getPreferences(this.context, 'mystore', function (err, preferences) {
        if (err) {
            console.info("Failed to get preferences.");
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
            console.info("Succeeded in putting the value of 'startup'.");

            preferences.flush(function (err) {
                if (err) {
                    console.info("Failed to flush. Cause: " + err);
                    return;
                }
                console.info("Succeeded in flushing.");
            })
            preferences.off('change', observer);
        })
    })
} catch (err) {
    console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
}
```

### off('multiProcessChange')<sup>10+</sup>

off(type: 'multiProcessChange', callback?: Callback&lt;{ key : string }&gt;): void

取消订阅进程间数据变更。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

**参数：**

| 参数名   | 类型                             | 必填 | 说明                                                           |
| -------- | -------------------------------- | ---- | -------------------------------------------------------------- |
| type     | string                           | 是   | 事件类型，固定值'multiProcessChange'，表示多进程间的数据变更。 |
| callback | Callback&lt;{ key : string }&gt; | 否   | 需要取消的回调对象实例，不填写则全部取消。                     |

**示例：**

```js
try {
    data_preferences.getPreferences(this.context, {name: 'mystore'}, function (err, preferences) {
        if (err) {
            console.info("Failed to get preferences.");
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
            console.info("Succeeded in putting the value of 'startup'.");

            preferences.flush(function (err) {
                if (err) {
                    console.info("Failed to flush. Cause: " + err);
                    return;
                }
                console.info("Succeeded in flushing.");
            })
            preferences.off('multiProcessChange', observer);
        })
    })
} catch (err) {
    console.info("Failed to flush. code =" + err.code + ", message =" + err.message);
}
```
## ValueType

用于表示允许的数据字段类型。

**系统能力：** SystemCapability.DistributedDataManager.Preferences.Core

| 类型            | 说明                           |
| --------------- | ------------------------------ |
| number          | 表示值类型为数字。             |
| string          | 表示值类型为字符串。           |
| boolean         | 表示值类型为布尔值。           |
| Array\<number>  | 表示值类型为数字类型的数组。   |
| Array\<boolean> | 表示值类型为布尔类型的数组。   |
| Array\<string>  | 表示值类型为字符串类型的数组。 |
