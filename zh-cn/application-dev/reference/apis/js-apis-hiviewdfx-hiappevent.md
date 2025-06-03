# @ohos.hiviewdfx.hiAppEvent (应用事件打点)

本模块提供了跨平台的应用事件打点能力，包括应用事件订阅、应用事件清理、打点功能配置等功能。

> **说明：**
>
> 本模块首批接口从API version 19开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```ts
import { hiAppEvent } from '@kit.PerformanceAnalysisKit';
```

## hiAppEvent.write

write(info: AppEventInfo, callback: AsyncCallback&lt;void&gt;): void

应用事件打点方法，写入事件时触发addWatcher订阅的应用事件观察者方法，可接收AppEventInfo类型的事件对象，使用callback方式作为异步回调。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                           | 必填 | 说明           |
| -------- | ------------------------------ | ---- | -------------- |
| info     | [AppEventInfo](#appeventinfo) | 是   | 应用事件对象。 |
| callback | AsyncCallback&lt;void&gt;      | 是   | 打点回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息                                      |
| -------- | --------------------------------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 11100001 | Function disabled.                            |
| 11101001 | Invalid event domain.                         |
| 11101002 | Invalid event name.                           |
| 11101003 | Invalid number of event parameters.           |
| 11101004 | Invalid string length of the event parameter. |
| 11101005 | Invalid event parameter name.                 |
| 11101006 | Invalid array length of the event parameter.  |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { hilog } from '@kit.PerformanceAnalysisKit';

let eventParams: Record<string, number | string> = {
  "int_data": 100,
  "str_data": "strValue",
};
hiAppEvent.write({
  domain: "test_domain",
  name: "test_event",
  eventType: hiAppEvent.EventType.FAULT,
  params: eventParams,
}, (err: BusinessError) => {
  if (err) {
    hilog.error(0x0000, 'hiAppEvent', `code: ${err.code}, message: ${err.message}`);
    return;
  }
  hilog.info(0x0000, 'hiAppEvent', `success to write event`);
});
```

## hiAppEvent.write

write(info: AppEventInfo): Promise&lt;void&gt;

应用事件打点方法，写入事件时触发addWatcher订阅的应用事件观察者方法，可接收AppEventInfo类型的事件对象，使用Promise方式作为异步回调。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                           | 必填 | 说明           |
| ------ | ------------------------------ | ---- | -------------- |
| info   | [AppEventInfo](#appeventinfo) | 是   | 应用事件对象。 |

**返回值：**

| 类型                | 说明          |
| ------------------- | ------------- |
| Promise&lt;void&gt; | Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息                                      |
| -------- | --------------------------------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 11100001 | Function disabled.                            |
| 11101001 | Invalid event domain.                         |
| 11101002 | Invalid event name.                           |
| 11101003 | Invalid number of event parameters.           |
| 11101004 | Invalid string length of the event parameter. |
| 11101005 | Invalid event parameter name.                 |
| 11101006 | Invalid array length of the event parameter.  |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { hilog } from '@kit.PerformanceAnalysisKit';

let eventParams: Record<string, number | string> = {
  "int_data": 100,
  "str_data": "strValue",
};
hiAppEvent.write({
  domain: "test_domain",
  name: "test_event",
  eventType: hiAppEvent.EventType.FAULT,
  params: eventParams,
}).then(() => {
  hilog.info(0x0000, 'hiAppEvent', `success to write event`);
}).catch((err: BusinessError) => {
  hilog.error(0x0000, 'hiAppEvent', `code: ${err.code}, message: ${err.message}`);
});
```

## AppEventInfo

提供了应用事件信息的参数选项。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称      | 类型                    | 必填 | 说明                                                         |
| --------- | ----------------------- | ---- | ------------------------------------------------------------ |
| domain    | string                  | 是   | 事件领域。事件领域名称支持数字、字母、下划线字符，需要以字母开头且不能以下划线结尾，长度非空且不超过32个字符。 |
| name      | string                  | 是   | 事件名称。首字符必须为字母字符或$字符，中间字符必须为数字字符、字母字符或下划线字符，结尾字符必须为数字字符或字母字符，长度非空且不超过48个字符。 |
| eventType | [EventType](#eventtype) | 是   | 事件类型。                                                   |
| params    | object                  | 是   | 事件参数对象，每个事件参数包括参数名和参数值。**系统事件中params包含的字段已由各系统事件定义，具体字段含义在各类系统事件指南的介绍中，例如[崩溃事件介绍](hiappevent-watcher-crash-events.md)。** 针对应用事件，打点写入的参数由开发者定义，其规格如下：<br>- 参数名为string类型，首字符必须为字母字符或$字符，中间字符必须为数字字符、字母字符或下划线字符，结尾字符必须为数字字符或字母字符，长度非空且不超过32个字符。<br>- 参数值支持string、number、boolean、数组类型，string类型参数长度需在8*1024个字符以内，超出会做丢弃处理；number类型参数取值需在Number.MIN_SAFE_INTEGER~Number.MAX_SAFE_INTEGER范围内，超出可能会产生不确定值；数组类型参数中的元素类型只能全为string、number、boolean中的一种，且元素个数需在100以内，超出会做丢弃处理。<br>- 参数个数需在32个以内，超出的参数会做丢弃处理。 |

## hiAppEvent.setEventParam

setEventParam(params: Record&lt;string, ParamType&gt;, domain: string, name?: string): Promise&lt;void&gt;

事件自定义参数设置方法，使用Promise方式作为异步回调。在同一生命周期中，可以通过事件领域和事件名称关联系统事件和应用事件，系统事件仅支持崩溃事件。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                           | 必填 | 说明           |
| ------ | ------------------------------ | ---- | -------------- |
| params | Record&lt;string, [ParamType](#paramtype)&gt; | 是 | 事件自定义参数对象。参数名和参数值规格定义如下：<br>- 参数名为string类型，首字符必须为字母字符或$字符，中间字符必须为数字字符、字母字符或下划线字符，结尾字符必须为数字字符或字母字符，长度非空且不超过32个字符。<br>- 参数值为[ParamType](#paramtype)类型，参数值长度需在1024个字符以内。<br>- 参数个数需在64个以内。 |
| domain | string                        | 是 | 事件领域。事件领域可支持关联应用事件和系统事件（hiAppEvent.domain.OS）。 |
| name   | string                        | 否 | 事件名称。默认为空字符串，空字符串表示关联事件领域下的所有事件名称。事件名称可支持关联应用事件和系统事件，其中系统事件仅支持关联崩溃事件（hiAppEvent.event.APP_CRASH）。 |

**返回值：**

| 类型                | 说明          |
| ------------------- | ------------- |
| Promise&lt;void&gt; | Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息                                      |
| -------- | --------------------------------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 11101007 | The number of parameter keys exceeds the limit. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { hilog } from '@kit.PerformanceAnalysisKit';

let params: Record<string, hiAppEvent.ParamType> = {
  "int_data": 100,
  "str_data": "strValue",
};
// 给应用事件追加自定义参数
hiAppEvent.setEventParam(params, "test_domain", "test_event").then(() => {
  hilog.info(0x0000, 'hiAppEvent', `success to set event param`);
}).catch((err: BusinessError) => {
  hilog.error(0x0000, 'hiAppEvent', `code: ${err.code}, message: ${err.message}`);
});
```

## ParamType

type ParamType = number | string | boolean | Array&lt;string&gt;

事件自定义参数值的类型。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 类型                       | 说明                |
|--------------------------|-------------------|
| number                   | 表示值类型为数字。         |
| string                   | 表示值类型为字符串。        |
| boolean                  | 表示值类型为布尔值。        |
| Array&lt;string&gt;      | 表示值类型为字符串类型的数组。   |

## hiAppEvent.configure

configure(config: ConfigOption): void

应用事件打点配置方法，可用于配置打点开关、目录存储配额大小等功能。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                          | 必填 | 说明                     |
| ------ | ----------------------------- | ---- | ------------------------ |
| config | [ConfigOption](#configoption) | 是   | 应用事件打点配置项对象。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
// 配置打点开关为关闭状态
let config1: hiAppEvent.ConfigOption = {
  disable: true,
};
hiAppEvent.configure(config1);
```

## ConfigOption

提供了对应用事件打点功能的配置选项。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称    | 类型    | 必填 | 说明                                                         |
| ------- | ------- | ---- | ------------------------------------------------------------ |
| disable | boolean | 否   | 打点功能开关，默认值为false。true：关闭打点功能，false：不关闭打点功能。 |

## hiAppEvent.setUserId

setUserId(name: string, value: string): void

设置用户ID。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                      | 必填 | 说明           |
| --------- | ------------------------- | ---- | -------------  |
| name      | string                    | 是   | 用户ID的key。只能包含大小写字母、数字、下划线和 $，不能以数字开头，长度非空且不超过256个字符。   |
| value     | string                    | 是   | 用户ID的值。长度不超过256，当值为null或空字符串时，则清除用户ID。 |

**错误码：**

| 错误码ID | 错误信息          |
| ------- | ----------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
import { hilog } from '@kit.PerformanceAnalysisKit';

try {
  hiAppEvent.setUserId('key', 'value');
} catch (error) {
  hilog.error(0x0000, 'hiAppEvent', `failed to setUserId event, code=${error.code}`);
}
```

## hiAppEvent.getUserId

getUserId(name: string): string

获取之前通过setUserId接口设置的value值。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                    | 必填 | 说明         |
| --------- | ----------------------- | ---- | ----------  |
| name      | string                  | 是   | 用户ID的key。只能包含大小写字母、数字、下划线和 $，不能以数字开头，长度不超过256。|

**返回值：**

| 类型    | 说明                            |
| ------ | ------------------------------- |
| string | 用户ID的值。没有查到返回空字符串。 |

**错误码：**

| 错误码ID | 错误信息          |
| ------- | ----------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
import { hilog } from '@kit.PerformanceAnalysisKit';

hiAppEvent.setUserId('key', 'value');
try {
  let value: string = hiAppEvent.getUserId('key');
  hilog.info(0x0000, 'hiAppEvent', `getUserId event was successful, userId=${value}`);
} catch (error) {
  hilog.error(0x0000, 'hiAppEvent', `failed to getUserId event, code=${error.code}`);
}
```

## hiAppEvent.setUserProperty

setUserProperty(name: string, value: string): void

设置用户属性。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                      | 必填 | 说明           |
| --------- | ------------------------- | ---- | -------------- |
| name      | string                    | 是   | 用户属性的key。只能包含大小写字母、数字、下划线和 $，不能以数字开头，长度非空且不超过256个字符。  |
| value     | string                    | 是   | 用户属性的值。长度不超过1024，当值为null或空字符串时，则清除用户属性。  |

**错误码：**

| 错误码ID | 错误信息          |
| ------- | ----------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
import { hilog } from '@kit.PerformanceAnalysisKit';

try {
  hiAppEvent.setUserProperty('key', 'value');
} catch (error) {
  hilog.error(0x0000, 'hiAppEvent', `failed to setUserProperty event, code=${error.code}`);
}
```

## hiAppEvent.getUserProperty

getUserProperty(name: string): string

获取之前通过setUserProperty接口设置的value值。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                    | 必填 | 说明          |
| --------- | ----------------------- | ---- | ----------    |
| name      | string                  | 是   | 用户属性的key。只能包含大小写字母、数字、下划线和 $，不能以数字开头，长度不超过256。|

**返回值：**

| 类型    | 说明                             |
| ------ | -------------------------------- |
| string | 用户属性的值。 没有查到返回空字符串。 |

**错误码：**

| 错误码ID | 错误信息          |
| ------- | ----------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
import { hilog } from '@kit.PerformanceAnalysisKit';

hiAppEvent.setUserProperty('key', 'value');
try {
  let value: string = hiAppEvent.getUserProperty('key');
  hilog.info(0x0000, 'hiAppEvent', `getUserProperty event was successful, userProperty=${value}`);
} catch (error) {
  hilog.error(0x0000, 'hiAppEvent', `failed to getUserProperty event, code=${error.code}`);
}
```

## hiAppEvent.addWatcher

addWatcher(watcher: Watcher): AppEventPackageHolder

添加应用事件观察者方法，可用于订阅应用事件。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                 | 必填 | 说明             |
| ------- | -------------------- | ---- | ---------------- |
| watcher | [Watcher](#watcher) | 是   | 应用事件观察者。 |

**返回值：**

| 类型                                             | 说明                                 |
| ------------------------------------------------ | ------------------------------------ |
| [AppEventPackageHolder](#appeventpackageholder) | 订阅数据持有者，订阅失败时返回null。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息                        |
| -------- | ------------------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 11102001 | Invalid watcher name.           |
| 11102002 | Invalid filtering event domain. |
| 11102003 | Invalid row value.              |
| 11102004 | Invalid size value.             |
| 11102005 | Invalid timeout value.          |

**示例：**

```ts
import { hilog } from '@kit.PerformanceAnalysisKit';

// 1. 如果观察者传入了回调的相关参数，则可以选择在自动触发的回调函数中对订阅事件进行处理
hiAppEvent.addWatcher({
  name: "watcher1",
  appEventFilters: [
    {
      domain: "test_domain",
      eventTypes: [hiAppEvent.EventType.FAULT, hiAppEvent.EventType.BEHAVIOR]
    }
  ],
  triggerCondition: {
    row: 10,
    size: 1000,
    timeOut: 1
  },
  onTrigger: (curRow: number, curSize: number, holder: hiAppEvent.AppEventPackageHolder) => {
    if (holder == null) {
      hilog.error(0x0000, 'hiAppEvent', "holder is null");
      return;
    }
    hilog.info(0x0000, 'hiAppEvent', `curRow=${curRow}, curSize=${curSize}`);
    let eventPkg: hiAppEvent.AppEventPackage | null = null;
    while ((eventPkg = holder.takeNext()) != null) {
      hilog.info(0x0000, 'hiAppEvent', `eventPkg.packageId=${eventPkg.packageId}`);
      hilog.info(0x0000, 'hiAppEvent', `eventPkg.row=${eventPkg.row}`);
      hilog.info(0x0000, 'hiAppEvent', `eventPkg.size=${eventPkg.size}`);
      for (const eventInfo of eventPkg.data) {
        hilog.info(0x0000, 'hiAppEvent', `eventPkg.data=${eventInfo}`);
      }
    }
  }
});

// 2. 如果观察者未传入回调的相关参数，则可以选择使用返回的holder对象手动去处理订阅事件
let holder = hiAppEvent.addWatcher({
  name: "watcher2",
});
if (holder != null) {
  let eventPkg: hiAppEvent.AppEventPackage | null = null;
  while ((eventPkg = holder.takeNext()) != null) {
    hilog.info(0x0000, 'hiAppEvent', `eventPkg.packageId=${eventPkg.packageId}`);
    hilog.info(0x0000, 'hiAppEvent', `eventPkg.row=${eventPkg.row}`);
    hilog.info(0x0000, 'hiAppEvent', `eventPkg.size=${eventPkg.size}`);
    for (const eventInfo of eventPkg.data) {
      hilog.info(0x0000, 'hiAppEvent', `eventPkg.data=${eventInfo}`);
    }
  }
}

// 3. 观察者可以在实时回调函数onReceive中处理订阅事件
hiAppEvent.addWatcher({
  name: "watcher3",
  appEventFilters: [
    {
      domain: "test_domain",
      eventTypes: [hiAppEvent.EventType.FAULT, hiAppEvent.EventType.BEHAVIOR]
    }
  ],
  onReceive: (domain: string, appEventGroups: Array<hiAppEvent.AppEventGroup>) => {
    hilog.info(0x0000, 'hiAppEvent', `domain=${domain}`);
    for (const eventGroup of appEventGroups) {
      hilog.info(0x0000, 'hiAppEvent', `eventName=${eventGroup.name}`);
      for (const eventInfo of eventGroup.appEventInfos) {
        hilog.info(0x0000, 'hiAppEvent', `event=${JSON.stringify(eventInfo)}`, );
      }
    }
  }
});
```

## hiAppEvent.removeWatcher

removeWatcher(watcher: Watcher): void

移除应用事件观察者方法，可用于取消订阅应用事件。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                 | 必填 | 说明             |
| ------- | -------------------- | ---- | ---------------- |
| watcher | [Watcher](#watcher) | 是   | 应用事件观察者。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息              |
| -------- | --------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 11102001 | Invalid watcher name. |

**示例：**

```ts
// 1. 定义一个应用事件观察者
let watcher: hiAppEvent.Watcher = {
  name: "watcher1",
}

// 2. 添加一个应用事件观察者来订阅事件
hiAppEvent.addWatcher(watcher);

// 3. 移除该应用事件观察者以取消订阅事件
hiAppEvent.removeWatcher(watcher);
```

## Watcher

提供了应用事件观察者的参数选项。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称             | 类型                                                         | 必填 | 说明                                                         |
| ---------------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| name             | string                                                       | 是   | 观察者名称，用于唯一标识观察者。                             |
| triggerCondition | [TriggerCondition](#triggercondition)                        | 否   | 订阅回调触发条件，需要与回调函数onTrigger一同传入才会生效。           |
| appEventFilters  | [AppEventFilter](#appeventfilter)[]                          | 否   | 订阅过滤条件，在需要对订阅事件进行过滤时传入。               |
| onTrigger        | (curRow: number, curSize: number, holder: [AppEventPackageHolder](#appeventpackageholder)) => void | 否   | 订阅回调函数，需要与回调触发条件triggerCondition一同传入才会生效，函数入参说明如下：<br>curRow：在本次回调触发时的订阅事件总数量； <br>curSize：在本次回调触发时的订阅事件总大小，单位为byte；  <br/>holder：订阅数据持有者对象，可以通过其对订阅事件进行处理。 |
| onReceive        | (domain: string, appEventGroups: Array<[AppEventGroup](#appeventgroup)>) => void | 否 | 订阅实时回调函数，与回调函数onTrigger同时存在时，只触发此回调，函数入参说明如下：<br>domain：回调事件的领域名称； <br>appEventGroups：回调事件集合。 |

## TriggerCondition

提供了回调触发条件的参数选项，只要满足任一条件就会触发订阅回调。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称    | 类型   | 必填 | 说明                                                         |
| ------- | ------ | ---- | ------------------------------------------------------------ |
| row     | number | 否   | 满足触发回调的事件总数量，正整数。默认值0，不触发回调。当使用try...catch...设定triggerCondition为负数时，row会被设定为默认值0，程序不会崩溃，catch可以捕获到错误码11102003。 |
| size    | number | 否   | 满足触发回调的事件总大小，正整数，单位为byte。默认值0，不触发回调。当使用try...catch...设定triggerCondition为负数时，size会被设定为默认值0，程序不会崩溃，catch可以捕获到错误码11102003。 |
| timeOut | number | 否   | 满足触发回调的超时时长，正整数，单位为30s。默认值0，不触发回调。当使用try...catch...设定triggerCondition为负数时，timeOut会被设定为默认值0，程序不会崩溃，catch可以捕获到错误码11102003。 |

## AppEventFilter

提供了过滤应用事件的参数选项。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称       | 类型                      | 必填 | 说明                     |
| ---------- | ------------------------- | ---- | ------------------------ |
| domain     | string                    | 是   | 需要订阅的事件领域。     |
| eventTypes | [EventType](#eventtype)[] | 否   | 需要订阅的事件类型集合。 |
| names      | string[]                  | 否   | 需要订阅的事件名称集合。 |

## AppEventPackageHolder

订阅数据持有者类，用于对订阅事件进行处理。

### constructor

constructor(watcherName: string)

类构造函数，创建订阅数据持有者实例，通过观察者名称关联到应用内已添加的观察者对象。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型              | 必填 | 说明                     |
| ------ | ----------------- | ---- | ------------------------ |
| watcherName | string | 是   | 观察者名称。 |

**示例：**

```ts
let holder1: hiAppEvent.AppEventPackageHolder = new hiAppEvent.AppEventPackageHolder("watcher1");
```

### setSize

setSize(size: number): void

设置每次取出的应用事件包的数据大小阈值。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| size   | number | 是   | 数据大小阈值，单位为byte，取值范围是大于等于0的数，超出范围会抛异常。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息            |
| -------- | ------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 11104001 | Invalid size value. |

**示例：**

```ts
let holder2: hiAppEvent.AppEventPackageHolder = new hiAppEvent.AppEventPackageHolder("watcher2");
holder2.setSize(1000);
```

### setRow

setRow(size: number): void

设置每次取出的应用事件包的数据条数，优先级高于setSize，和setSize同时调用时仅setRow生效。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| size   | number | 是   | 应用事件条数，单位为条，取值范围是大于0的数，超出范围会抛异常。 |

**错误码：**

以下错误码的详细介绍请参见[应用事件打点错误码](../errorcodes/errorcode-hiappevent.md)。

| 错误码ID | 错误信息            |
| -------- | ------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 11104001 | Invalid size value. |

**示例：**

```ts
let holder3: hiAppEvent.AppEventPackageHolder = new hiAppEvent.AppEventPackageHolder("watcher3");
holder3.setRow(1000);
```

### takeNext

takeNext(): AppEventPackage

根据设置的数据大小阈值或条数来取出订阅事件数据，当订阅事件数据全部被取出时返回null作为标识。
1、应用仅调用setSize不调用setRow时，根据数据大小限制取订阅事件。
2、应用调用setRow，无论是否调用setSize，都根据setRow设置的条数取订阅事件。
3、setSize和setRow都没被调用时，默认取1条订阅事件。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明                                                   |
| ----------------------------------- | ------------------------------------------------------ |
| [AppEventPackage](#appeventpackage) | 取出的事件包对象，订阅事件数据被全部取出后会返回null。 |

**示例：**

```ts
let holder4: hiAppEvent.AppEventPackageHolder = new hiAppEvent.AppEventPackageHolder("watcher4");
let eventPkg = holder4.takeNext();
```

## AppEventPackage

提供了订阅返回的应用事件包的参数定义。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称      | 类型     | 必填 | 说明                           |
| --------- | -------- | ---- | ------------------------------ |
| packageId | number   | 是   | 事件包ID，从0开始自动递增。   |
| row       | number   | 是   | 事件包的事件数量。            |
| size      | number   | 是   | 事件包的事件大小，单位为byte。 |
| data      | string[] | 是   | 事件包的事件信息。             |
| appEventInfos | Array<[AppEventInfo](#appeventinfo)> | 是   | 事件对象集合。 |

## AppEventGroup

提供了订阅返回的事件组的参数定义。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称          | 类型                            | 必填  | 说明          |
| ------------- | ------------------------------- | ---- | ------------- |
| name          | string                          | 是   | 事件名称。     |
| appEventInfos | Array<[AppEventInfo](#appeventinfo)> | 是   | 事件对象集合。 |

## hiAppEvent.clearData

clearData(): void

应用事件打点数据清理方法，将应用存储在本地的打点数据进行清除。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

**示例：**

```ts
hiAppEvent.clearData();
```


## EventType

事件类型枚举。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称      | 值   | 说明           |
| --------- | ---- | -------------- |
| FAULT     | 1    | 故障类型事件。 |
| STATISTIC | 2    | 统计类型事件。 |
| SECURITY  | 3    | 安全类型事件。 |
| BEHAVIOR  | 4    | 行为类型事件。 |


## hiappevent.domain

提供了所有预定义事件的领域名称常量。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称 | 类型   | 只读   | 说明       |
| ---  | ------ | ------ | ---------- |
| OS   | string | 是 | 系统领域。 |


## hiappevent.event

提供了所有预定义事件的事件名称常量。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称                      | 类型   | 只读   | 说明                 |
| ------------------------- | ------ | ------ | -------------------- |
| APP_CRASH | string | 是   | 应用崩溃事件。 |


## hiappevent.param

提供了所有预定义参数的参数名称常量。

**系统能力：** SystemCapability.HiviewDFX.HiAppEvent

**支持平台：** Android、iOS

| 名称                            | 类型   | 只读   | 说明               |
| ------------------------------- | ------ | ------ | ------------------ |
| USER_ID                         | string | 是 | 用户自定义ID。     |
| DISTRIBUTED_SERVICE_NAME        | string | 是 | 分布式服务名称。   |
| DISTRIBUTED_SERVICE_INSTANCE_ID | string | 是 | 分布式服务实例ID。 |