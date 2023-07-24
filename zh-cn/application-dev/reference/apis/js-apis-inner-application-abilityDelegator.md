# AbilityDelegator

AbilityDelegator提供添加用于监视指定ability的生命周期状态更改的AbilityMonitor对象的能力，包括对AbilityMonitor实例的添加、删除、等待ability到达OnCreate生命周期、设置等待时间等、获取指定ability的生命周期状态、获取当前应用顶部ability、启动指定ability等。

> **说明：**
> 
> 本模块首批接口从API version 8开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。 

## 导入模块

```ts
import AbilityDelegatorRegistry from '@ohos.app.ability.abilityDelegatorRegistry';
```

## 使用说明

通过AbilityDelegatorRegistry中[getAbilityDelegator](js-apis-app-ability-abilityDelegatorRegistry.md#abilitydelegatorregistrygetabilitydelegator)方法获取。
```ts
import AbilityDelegatorRegistry from '@ohos.app.ability.abilityDelegatorRegistry';
```

## AbilityDelegator

### addAbilityMonitor<sup>9+</sup>

addAbilityMonitor(monitor: AbilityMonitor, callback: AsyncCallback\<void>): void;

添加AbilityMonitor实例（callback形式）

**系统能力：**SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| monitor  | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor) | 是       | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor)实例 |
| callback | AsyncCallback\<void>                                         | 是       | 表示指定的回调方法                                           |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | AddAbilityMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback, data: ${JSON.stringify(data)}');
}

let monitor = {
    abilityName: 'abilityname',
    onAbilityCreate: onAbilityCreateCallback
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.addAbilityMonitor(monitor, (error : any) => {
    console.error('addAbilityMonitor fail, error: ${JSON.stringify(error)}');
});
```

### addAbilityMonitor<sup>9+</sup>

addAbilityMonitor(monitor: AbilityMonitor): Promise\<void>;

添加AbilityMonitor实例（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型                                                         | 必填 | 说明                                                         |
| ------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor) | 是   | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor)实例 |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<void> | 以Promise形式返回。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | AddAbilityMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    abilityName: 'abilityname',
    onAbilityCreate: onAbilityCreateCallback
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.addAbilityMonitor(monitor).then(() => {
    console.info('addAbilityMonitor promise');
});
```

### removeAbilityMonitor<sup>9+</sup>

removeAbilityMonitor(monitor: AbilityMonitor, callback: AsyncCallback\<void>): void;

删除已经添加的AbilityMonitor实例（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor  | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor) | 是   | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor)实例 |
| callback | AsyncCallback\<void>                                         | 是   | 表示指定的回调方法                                           |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | RemoveAbilityMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    abilityName: 'abilityname',
    onAbilityCreate: onAbilityCreateCallback
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.removeAbilityMonitor(monitor, (error : any) => {
    console.error('removeAbilityMonitor fail, error: ${JSON.stringify(error)}');
});
```

### removeAbilityMonitor<sup>9+</sup>

removeAbilityMonitor(monitor: AbilityMonitor): Promise\<void>;

删除已经添加的AbilityMonitor实例（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名    | 类型                                                         | 必填 | 说明                                                         |
| ------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor) | 是   | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor)实例 |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<void> | 以Promise形式返回。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | RemoveAbilityMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

- 示例

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    abilityName: 'abilityname',
    onAbilityCreate: onAbilityCreateCallback
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.removeAbilityMonitor(monitor).then(() => {
    console.info('removeAbilityMonitor promise');
});
```

### waitAbilityMonitor<sup>9+</sup>

waitAbilityMonitor(monitor: AbilityMonitor, callback: AsyncCallback\<UIAbility>): void;

等待与AbilityMonitor实例匹配的ability到达OnCreate生命周期，并返回ability实例（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor  | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor) | 是   | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor)实例 |
| callback | AsyncCallback\<[UIAbility](js-apis-app-ability-uiAbility.md)> | 是   | 表示指定的回调方法                                           |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | WaitAbilityMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    abilityName: 'abilityname',
    onAbilityCreate: onAbilityCreateCallback
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.waitAbilityMonitor(monitor, (error : any, data : any) => {
    if (error) {
        console.error('waitAbilityMonitor fail, error: ${JSON.stringify(error)}');
    } else {
        console.log('waitAbilityMonitor success, data: ${JSON.stringify(data)}');
    }
});
```

### waitAbilityMonitor<sup>9+</sup>

waitAbilityMonitor(monitor: AbilityMonitor, timeout: number, callback: AsyncCallback\<UIAbility>): void;

设置等待时间，等待与AbilityMonitor实例匹配的ability到达OnCreate生命周期，并返回ability实例（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor  | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor) | 是   | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor)实例 |
| timeout  | number                                                       | 否   | 最大等待时间，单位毫秒（ms）                                 |
| callback | AsyncCallback\<[UIAbility](js-apis-app-ability-uiAbility.md)> | 是   | 表示指定的回调方法                                           |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | WaitAbilityMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let timeout = 100;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    abilityName: 'abilityname',
    onAbilityCreate: onAbilityCreateCallback
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.waitAbilityMonitor(monitor, timeout, (error : any, data : any) => {
    if (error && error.code !== 0) {
        console.error('waitAbilityMonitor fail, error: ${JSON.stringify(error)}');
    } else {
        console.log('waitAbilityMonitor success, data: ${JSON.stringify(data)}');
    }
});
```



### waitAbilityMonitor<sup>9+</sup>

waitAbilityMonitor(monitor: AbilityMonitor, timeout?: number): Promise\<UIAbility>;

设置等待时间，等待与AbilityMonitor实例匹配的ability到达OnCreate生命周期，并返回ability实例（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型                                                         | 必填 | 说明                                                         |
| ------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor) | 是   | [AbilityMonitor](js-apis-inner-application-abilityMonitor.md#AbilityMonitor)实例 |
| timeout | number                                                       | 否   | 最大等待时间，单位毫秒（ms）                                 |

**返回值：**

| 类型                                                        | 说明                       |
| ----------------------------------------------------------- | -------------------------- |
| Promise\<[UIAbility](js-apis-app-ability-uiAbility.md)> | 以Promise形式返回Ability。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | WaitAbilityMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    abilityName: 'abilityname',
    onAbilityCreate: onAbilityCreateCallback
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.waitAbilityMonitor(monitor).then((data : any) => {
    console.info('waitAbilityMonitor promise');
});
```

### getAppContext<sup>9+</sup>

getAppContext(): Context;

获取应用Context

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**返回值：**

| 类型                                  | 说明                                        |
| ------------------------------------- | ------------------------------------------- |
| [Context](js-apis-inner-application-context.md) | 应用[Context](js-apis-inner-application-context.md)。 |

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
let context = abilityDelegator.getAppContext();
```

### getAbilityState<sup>9+</sup>

getAbilityState(ability: UIAbility): number;

获取指定ability的生命周期状态

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型                                              | 必填 | 说明            |
| ------- | ------------------------------------------------- | ---- | --------------- |
| ability | [UIAbility](js-apis-app-ability-uiAbility.md) | 是   | 指定Ability对象 |

**返回值：**

| 类型   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| number | 指定ability的生命周期状态。状态枚举值使用[AbilityLifecycleState](js-apis-app-ability-abilityDelegatorRegistry.md#AbilityLifecycleState)。 |

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let ability;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.getCurrentTopAbility((err : any, data : any) => {
    console.info('getCurrentTopAbility callback');
    ability = data;
    let state = abilityDelegator.getAbilityState(ability);
    console.info('getAbilityState ${state}');
});
```

### getCurrentTopAbility<sup>9+</sup>

getCurrentTopAbility(callback: AsyncCallback\<UIAbility>): void;

获取当前应用顶部ability（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明               |
| -------- | ------------------------------------------------------------ | ---- | ------------------ |
| callback | AsyncCallback\<[UIAbility](js-apis-app-ability-uiAbility.md)> | 是   | 表示指定的回调方法 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | GetCurrentTopAbility failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let ability;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.getCurrentTopAbility((err : any, data : any) => {
    console.info('getCurrentTopAbility callback');
    ability = data;
});
```

### getCurrentTopAbility<sup>9+</sup>

getCurrentTopAbility(): Promise\<UIAbility>;

获取当前应用顶部ability（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**返回值：**

| 类型                                                        | 说明                                   |
| ----------------------------------------------------------- | -------------------------------------- |
| Promise\<[UIAbility](js-apis-app-ability-uiAbility.md)> | 以Promise形式返回当前应用顶部ability。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | GetCurrentTopAbility failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let ability;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.getCurrentTopAbility().then((data : any) => {
    console.info('getCurrentTopAbility promise');
    ability = data;
});
```

### startAbility<sup>9+</sup>

startAbility(want: Want, callback: AsyncCallback\<void>): void;

启动指定ability（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                   | 必填 | 说明               |
| -------- | -------------------------------------- | ---- | ------------------ |
| want     | [Want](js-apis-app-ability-want.md) | 是   | 启动Ability参数    |
| callback | AsyncCallback\<void>                   | 是   | 表示指定的回调方法 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000001 | The specified ability does not exist. |
| 16000002 | Incorrect ability type. |
| 16000004 | Can not start invisible component. |
| 16000005 | The specified process does not have the permission. |
| 16000006 | Cross-user operations are not allowed. |
| 16000008 | The crowdtesting application expires. |
| 16000009 | An ability cannot be started or stopped in Wukong mode. |
| 16000010 | The call with the continuation flag is forbidden. |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |
| 16000053 | The ability is not on the top of the UI. |
| 16000055 | Installation-free timed out. |
| 16200001 | The caller has been released. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let want = {
    bundleName: 'bundleName',
    abilityName: 'abilityName'
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.startAbility(want, (err : any, data : any) => {
    console.info('startAbility callback');
});
```

### startAbility<sup>9+</sup>

startAbility(want: Want): Promise\<void>;

启动指定ability（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名 | 类型                                   | 必填 | 说明            |
| ------ | -------------------------------------- | ---- | --------------- |
| want   | [Want](js-apis-app-ability-want.md) | 是   | 启动Ability参数 |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<void> | 以Promise形式返回。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000001 | The specified ability does not exist. |
| 16000002 | Incorrect ability type. |
| 16000004 | Can not start invisible component. |
| 16000005 | The specified process does not have the permission. |
| 16000006 | Cross-user operations are not allowed. |
| 16000008 | The crowdtesting application expires. |
| 16000009 | An ability cannot be started or stopped in Wukong mode. |
| 16000010 | The call with the continuation flag is forbidden. |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |
| 16000053 | The ability is not on the top of the UI. |
| 16000055 | Installation-free timed out. |
| 16200001 | The caller has been released. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let want = {
    bundleName: 'bundleName',
    abilityName: 'abilityName'
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.startAbility(want).then((data: any) => {
    console.info('startAbility promise');
});
```

### doAbilityForeground<sup>9+</sup>

doAbilityForeground(ability: UIAbility, callback: AsyncCallback\<void>): void;

调度指定ability生命周期状态到Foreground状态（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                    |
| -------- | ----------------------- | ---- | ------------------------------------------------------- |
| ability  | UIAbility               | 是   | 指定Ability对象                                         |
| callback | AsyncCallback\<void>    | 是   | 表示指定的回调方法<br/>\- true：成功<br/>\- false：失败 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | DoAbilityForeground failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let ability;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.getCurrentTopAbility((err : any, data : any) => {
    console.info('getCurrentTopAbility callback');
    ability = data;
    abilityDelegator.doAbilityForeground(ability, (err : any) => {
        console.info("doAbilityForeground callback");
    });
});
```

### doAbilityForeground<sup>9+</sup>

doAbilityForeground(ability: UIAbility): Promise\<void>;

调度指定ability生命周期状态到Foreground状态（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型    | 必填 | 说明            |
| ------- | ------- | ---- | --------------- |
| ability | UIAbility | 是   | 指定Ability对象 |

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回执行结果。<br/>\- true：成功<br/>\- false：失败 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | DoAbilityForeground failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let ability;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.getCurrentTopAbility((err : any, data : any) => {
    console.info('getCurrentTopAbility callback');
    ability = data;
    abilityDelegator.doAbilityForeground(ability).then(() => {
        console.info("doAbilityForeground promise");
    });
});
```

### doAbilityBackground<sup>9+</sup>

doAbilityBackground(ability: UIAbility, callback: AsyncCallback\<void>): void;

调度指定ability生命周期状态到Background状态（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                    |
| -------- | ----------------------- | ---- | ------------------------------------------------------- |
| ability  | UIAbility                 | 是   | 指定Ability对象                                         |
| callback | AsyncCallback\<void> | 是   | 表示指定的回调方法<br/>\- true：成功<br/>\- false：失败 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | DoAbilityBackground failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let ability;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.getCurrentTopAbility((err : any, data : any) => {
    console.info('getCurrentTopAbility callback');
    ability = data;
    abilityDelegator.doAbilityBackground(ability, (err : any) => {
        console.info("doAbilityBackground callback");
    });
});
```

### doAbilityBackground<sup>9+</sup>

doAbilityBackground(ability: UIAbility): Promise\<void>;

调度指定ability生命周期状态到Background状态（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型    | 必填 | 说明            |
| ------- | ------- | ---- | --------------- |
| ability | UIAbility | 是   | 指定Ability对象 |

**返回值：**

| 类型              | 说明                                                         |
| ----------------- | ------------------------------------------------------------ |
| Promise\<boolean> | 以Promise形式返回执行结果。<br/>\- true：成功<br/>\- false：失败 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | DoAbilityBackground failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let ability;

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.getCurrentTopAbility((err : any, data : any) => {
    console.info('getCurrentTopAbility callback');
    ability = data;
    abilityDelegator.doAbilityBackground(ability).then(() => {
        console.info("doAbilityBackground promise");
    });
});
```

### printSync<sup>9+</sup>

printSync(msg: string): void;

打印日志信息到单元测试终端控制台

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明       |
| ------ | ------ | ---- | ---------- |
| msg    | string | 是   | 日志字符串 |

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let msg = 'msg';

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.printSync(msg);
```

### print

print(msg: string, callback: AsyncCallback\<void>): void;

打印日志信息到单元测试终端控制台（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                 | 必填 | 说明               |
| -------- | -------------------- | ---- | ------------------ |
| msg      | string               | 是   | 日志字符串         |
| callback | AsyncCallback\<void> | 是   | 表示指定的回调方法 |

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let msg = 'msg';

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.print(msg, (err : any) => {
    console.info('print callback');
});
```

### print

print(msg: string): Promise\<void>;

打印日志信息到单元测试终端控制台（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明       |
| ------ | ------ | ---- | ---------- |
| msg    | string | 是   | 日志字符串 |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<void> | 以Promise形式返回。 |

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let msg = 'msg';

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.print(msg).then(() => {
    console.info('print promise');
});
```

### finishTest<sup>9+</sup>

finishTest(msg: string, code: number, callback: AsyncCallback\<void>): void;

结束测试并打印日志信息到单元测试终端控制台（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                 | 必填 | 说明               |
| -------- | -------------------- | ---- | ------------------ |
| msg      | string               | 是   | 日志字符串         |
| code     | number               | 是   | 日志码             |
| callback | AsyncCallback\<void> | 是   | 表示指定的回调方法 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | FinishTest failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let msg = 'msg';

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.finishTest(msg, 0, (err : any) => {
    console.info('finishTest callback');
});
```

### finishTest<sup>9+</sup>

finishTest(msg: string, code: number): Promise\<void>;

结束测试并打印日志信息到单元测试终端控制台（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明       |
| ------ | ------ | ---- | ---------- |
| msg    | string | 是   | 日志字符串 |
| code   | number | 是   | 日志码     |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<void> | 以Promise形式返回。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | FinishTest failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let msg = 'msg';

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.finishTest(msg, 0).then(() => {
    console.info('finishTest promise');
});
```

### addAbilityStageMonitor<sup>9+</sup>

addAbilityStageMonitor(monitor: AbilityStageMonitor, callback: AsyncCallback\<void>): void;

添加一个AbilityStageMonitor对象，用于监视指定abilityStage的生命周期状态更改。（callback形式）

**系统能力：**SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| monitor  | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) | 是       | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) 实例 |
| callback | AsyncCallback\<void>                                         | 是       | 表示指定的回调方法                                           |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | AddAbilityStageMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

let monitor = {
    moduleName: 'moduleName',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.addAbilityStageMonitor(monitor, (err : any) => {
    console.info('addAbilityStageMonitor callback');
});
```

### addAbilityStageMonitor<sup>9+</sup>

addAbilityStageMonitor(monitor: AbilityStageMonitor): Promise\<void>;

添加一个AbilityStageMonitor对象，用于监视指定abilityStage的生命周期状态更改。（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型                                                         | 必填 | 说明                                                         |
| ------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) | 是   | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) 实例 |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<void> | 以Promise形式返回。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | AddAbilityStageMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

let monitor = {
    moduleName: 'moduleName',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.addAbilityStageMonitor(monitor).then(() => {
    console.info('addAbilityStageMonitor promise');
});
```

### removeAbilityStageMonitor<sup>9+</sup>

removeAbilityStageMonitor(monitor: AbilityStageMonitor, callback: AsyncCallback\<void>): void;

从应用程序内存中删除指定的AbilityStageMonitor对象。（callback形式）

**系统能力：**SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| monitor  | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) | 是       | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) 实例 |
| callback | AsyncCallback\<void>                                         | 是       | 表示指定的回调方法                                           |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | RemoveAbilityStageMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

let monitor = {
    moduleName: 'moduleName',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.removeAbilityStageMonitor(monitor, (err : any) => {
    console.info('removeAbilityStageMonitor callback');
});
```

### removeAbilityStageMonitor<sup>9+</sup>

removeAbilityStageMonitor(monitor: AbilityStageMonitor): Promise\<void>;

从应用程序内存中删除指定的AbilityStageMonitor对象。（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型                                                         | 必填 | 说明                                                         |
| ------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) | 是   | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) 实例 |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<void> | 以Promise形式返回。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | RemoveAbilityStageMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

let monitor = {
    moduleName: 'moduleName',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.removeAbilityStageMonitor(monitor).then(() => {
    console.info('removeAbilityStageMonitor promise');
});
```

### waitAbilityStageMonitor<sup>9+</sup>

waitAbilityStageMonitor(monitor: AbilityStageMonitor, callback: AsyncCallback\<AbilityStage>): void;

等待并返回与给定AbilityStageMonitor中设置的条件匹配的AbilityStage对象。（callback形式）

**系统能力：**SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| monitor  | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) | 是       | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) 实例 |
| callback | AsyncCallback\<AbilityStage>                                         | 是       | 成功返回AbilityStage对象，失败返回空。             |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | WaitAbilityStageMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    moduleName: 'moduleName',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.waitAbilityStageMonitor(monitor, (err : any, data : any) => {
    console.info('waitAbilityStageMonitor callback');
});
```

### waitAbilityStageMonitor<sup>9+</sup>

waitAbilityStageMonitor(monitor: AbilityStageMonitor, timeout?: number): Promise\<AbilityStage>;

等待并返回与给定AbilityStageMonitor中设置的条件匹配的AbilityStage对象。（promise形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型                                                         | 必填 | 说明                                                         |
| ------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) | 是   | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) 实例 |
| timeout | number | 否   | 超时最大等待时间，以毫秒为单位。 |

**返回值：**

| 类型           | 说明                |
| -------------- | ------------------- |
| Promise\<AbilityStage> | 成功返回AbilityStage对象，失败返回空。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | WaitAbilityStageMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    moduleName: 'moduleName',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.waitAbilityStageMonitor(monitor).then((data : any) => {
    console.info('waitAbilityStageMonitor promise');
});
```

### waitAbilityStageMonitor<sup>9+</sup>

waitAbilityStageMonitor(monitor: AbilityStageMonitor, timeout: number, callback: AsyncCallback\<AbilityStage>): void;

等待并返回与给定AbilityStageMonitor中设置的条件匹配的AbilityStage对象。（callback形式）

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名  | 类型                                                         | 必填 | 说明                                                         |
| ------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| monitor | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) | 是   | [AbilityStageMonitor](js-apis-inner-application-abilityStageMonitor.md) 实例 |
| timeout | number | 否   | 超时最大等待时间，以毫秒为单位。 |
| callback | AsyncCallback\<AbilityStage>                                         | 是       | 成功返回AbilityStage对象，失败返回空。                     |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000100 | WaitAbilityStageMonitor failed. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
let abilityDelegator: AbilityDelegatorRegistry.AbilityDelegator;
let timeout = 100;

function onAbilityCreateCallback(data) {
    console.info('onAbilityCreateCallback');
}

let monitor = {
    moduleName: 'moduleName',
    srcEntrance: './ets/Application/MyAbilityStage.ts',
};

abilityDelegator = AbilityDelegatorRegistry.getAbilityDelegator();
abilityDelegator.waitAbilityStageMonitor(monitor, timeout, (err : any, data : any) => {
    console.info('waitAbilityStageMonitor callback');
});
```