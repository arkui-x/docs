# @ohos.hiTraceMeter (性能打点)

本模块提供了跟踪进程轨迹，度量程序执行性能的打点能力。本模块打点的数据可供Android端的perfetto、atrace工具分析使用。

> **说明：**
>
> 本模块首批接口从API version 20开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 请注意，后续性能打点接口[startTrace](#hitracemeterstarttrace)、[finishTrace](#hitracemeterfinishtrace)、[traceByValue](#hitracemetertracebyvalue)将逐步废弃。
>
> 性能打点接口[startTrace](#hitracemeterstarttrace)、[finishTrace](#hitracemeterfinishtrace)、[traceByValue](#hitracemetertracebyvalue)无法指定跟踪输出级别。
>
> 用户态trace格式使用竖线 `|` 作为分隔符，所以通过性能打点接口传递的字符串类型参数应避免包含该字符，防止trace解析异常。
>
> 用户态trace总长度限制1023字符，超过的部分将会被截断。

## 导入模块

```js
import { hiTraceMeter } from '@kit.PerformanceAnalysisKit';
```

## hiTraceMeter.startTrace

startTrace(name: string, taskId: number): void

标记一个异步跟踪耗时任务的开始。

如果有多个相同name的任务需要跟踪或者对同一个任务要跟踪多次，并且任务同时被执行，则开发者每次调用startTrace传入的taskId需不同。

如果具有相同name的任务是串行执行的，则taskId可以相同。具体示例可参考[finishTrace](#hitracemeterfinishtrace)中的示例。

从API version 20开始，建议使用[startAsyncTrace](#hitracemeterstartasynctrace)接口（需与[finishAsyncTrace](#hitracemeterfinishasynctrace)接口配套使用）。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android（Android API 29及以上支持）

**参数：**

| 参数名 | 类型   | 必填 | 说明               | Android平台 | iOS平台 |
| ------ | ------ | ---- | ------------------ | ----------- | ------- |
| name   | string | 是   | 要跟踪的任务名称。 | 支持        | 不支持  |
| taskId | number | 是   | 任务id。           | 支持        | 不支持  |

**示例：**

```js
hiTraceMeter.startTrace("myTestFunc", 1);
```

## hiTraceMeter.finishTrace

finishTrace(name: string, taskId: number): void

标记一个异步跟踪耗时任务的结束。

finishTrace的name和taskId必须与流程开始的[startTrace](#hitracemeterstarttrace)对应参数值一致。

从API version 20开始，建议使用[finishAsyncTrace](#hitracemeterfinishasynctrace)接口（需与[startAsyncTrace](#hitracemeterstartasynctrace)接口配套使用）。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android（Android API 29及以上支持）

**参数：**

| 参数名 | 类型   | 必填 | 说明               | Android平台 | iOS平台 |
| ------ | ------ | ---- | ------------------ | ----------- | ------- |
| name   | string | 是   | 要跟踪的任务名称。 | 支持        | 不支持  |
| taskId | number | 是   | 任务id。           | 支持        | 不支持  |

**示例：**

```js
hiTraceMeter.finishTrace("myTestFunc", 1);
```

```js
// 跟踪并行执行的同名任务
hiTraceMeter.startTrace("myTestFunc", 1);
// 业务流程...... 
hiTraceMeter.startTrace("myTestFunc", 2);  // 第二个跟踪的任务开始，同时第一个跟踪的同名任务还没结束，出现了并行执行，对应接口的taskId需要不同
// 业务流程...... 
hiTraceMeter.finishTrace("myTestFunc", 1);
// 业务流程...... 
hiTraceMeter.finishTrace("myTestFunc", 2);
```

```js
// 跟踪串行执行的同名任务
hiTraceMeter.startTrace("myTestFunc", 1);
// 业务流程...... 
hiTraceMeter.finishTrace("myTestFunc", 1);  // 第一个跟踪的任务结束
// 业务流程...... 
hiTraceMeter.startTrace("myTestFunc", 1);   // 第二个跟踪的同名任务开始，同名的待跟踪任务串行执行。
// 业务流程...... 
hiTraceMeter.finishTrace("myTestFunc", 1);
```

## hiTraceMeter.traceByValue

traceByValue(name: string, count: number): void

用来标记一个跟踪的整数变量，该变量的数值会不断变化。

从API version 20开始，建议使用[traceByValue](#hitracemetertracebyvalue-1)接口。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android（Android API 29及以上支持）

**参数：**

| 参数名 | 类型   | 必填 | 说明                   | Android平台 | iOS平台 |
| ------ | ------ | ---- | ---------------------- | ----------- | ------- |
| name   | string | 是   | 要跟踪的整数变量名称。 | 支持        | 不支持  |
| count  | number | 是   | 整数变量的值。         | 支持        | 不支持  |

**示例：**

```js
let traceCount = 3;
hiTraceMeter.traceByValue("myTestCount", traceCount);
traceCount = 4;
hiTraceMeter.traceByValue("myTestCount", traceCount);
// 业务流程......
```

## HiTraceOutputLevel

枚举，跟踪输出级别。

低于系统跟踪输出级别阈值的打点将不会生效。log版本阈值为INFO；nolog版本阈值为COMMERCIAL。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** **Android与iOS平台均不支持指定跟踪输出级别，调用接口时可传入该属性，但将不会生效。**

| 名称       | 值   | 说明                                    | Android平台 | iOS平台 |
| ---------- | ---- | --------------------------------------- | ----------- | ------- |
| DEBUG      | 0    | 仅用于调试的输出级别，优先级最低。      | 不支持      | 不支持  |
| INFO       | 1    | 用于log版本的输出级别。                 | 不支持      | 不支持  |
| CRITICAL   | 2    | 用于log版本的输出级别，优先级高于INFO。 | 不支持      | 不支持  |
| COMMERCIAL | 3    | 用于nolog版本的输出级别，优先级最高。   | 不支持      | 不支持  |
| MAX        | 3    | 输出级别范围限制，MAX = COMMERCIAL。    | 不支持      | 不支持  |

## hiTraceMeter.startAsyncTrace

startAsyncTrace(level: HiTraceOutputLevel, name: string, taskId: number, customCategory: string, customArgs?: string): void

标记一个异步跟踪耗时任务的开始，分级控制跟踪输出。

如果有多个相同name的任务需要跟踪或者对同一个任务要跟踪多次，并且任务同时被执行，则开发者每次调用startAsyncTrace传入的taskId需不同。

如果具有相同name的任务是串行执行的，则taskId可以相同。具体示例可参考[finishAsyncTrace](#hitracemeterfinishasynctrace)中的示例。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android（Android API 29及以上支持）

**参数：**

| 参数名         | 类型                                      | 必填 | 说明                                                         | Android平台 | iOS平台 |
| -------------- | ----------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| level          | [HiTraceOutputLevel](#hitraceoutputlevel) | 是   | 跟踪输出级别。（尽管该参数在 Android 和 iOS 平台不支持，但由于该参数为必填项，您仍需传入） | 不支持      | 不支持  |
| name           | string                                    | 是   | 要跟踪的任务名称。                                           | 支持        | 不支持  |
| taskId         | number                                    | 是   | 任务id。                                                     | 支持        | 不支持  |
| customCategory | string                                    | 是   | 自定义聚类名称，用于聚合同一类异步跟踪打点。（尽管该参数在 Android 和 iOS 平台不支持，但由于该参数为必填项，您仍需传入） | 不支持      | 不支持  |
| customArgs     | string                                    | 否   | 自定义键值对，格式key=value，多个键值对用逗号分隔。<br>不传入该参数等同于传入空字符串。 | 不支持      | 不支持  |

**示例：**

```js
// 不需要customCategory参数时，可传入空字符串
// 不需要customArgs参数时，可不传入该参数或传入空字符串
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
hiTraceMeter.startAsyncTrace(COMMERCIAL, "myTestFunc", 1, "", "");
hiTraceMeter.startAsyncTrace(COMMERCIAL, "myTestFunc", 2, "");
// 多个键值对用逗号分隔
hiTraceMeter.startAsyncTrace(COMMERCIAL, "myTestFunc", 3, "categoryTest", "key1=value");
hiTraceMeter.startAsyncTrace(COMMERCIAL, "myTestFunc", 4, "categoryTest", "key1=value1,key2=value2");
```

## hiTraceMeter.finishAsyncTrace

finishAsyncTrace(level: HiTraceOutputLevel, name: string, taskId: number): void

标记一个异步跟踪耗时任务的结束，分级控制跟踪输出。

finishAsyncTrace的level、name和taskId必须与流程开始的[startAsyncTrace](#hitracemeterstartasynctrace)对应参数值一致。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android（Android API 29及以上支持）

**参数：**

| 参数名 | 类型                                      | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------ | ----------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| level  | [HiTraceOutputLevel](#hitraceoutputlevel) | 是   | 跟踪输出级别。（尽管该参数在 Android 和 iOS 平台不支持，但由于该参数为必填项，您仍需传入） | 不支持      | 不支持  |
| name   | string                                    | 是   | 要跟踪的任务名称。                                           | 支持        | 不支持  |
| taskId | number                                    | 是   | 任务id。                                                     | 支持        | 不支持  |

**示例：**

```js
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
hiTraceMeter.finishAsyncTrace(COMMERCIAL, "myTestFunc", 1);
```

```js
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
// 跟踪并行执行的同名任务
// 第一个跟踪的任务开始
hiTraceMeter.startAsyncTrace(COMMERCIAL, "myTestFunc", 1, "categoryTest", "key=value");
// 业务流程......
// 第二个跟踪的任务开始，同时第一个跟踪的同名任务还没结束，出现了并行执行，对应接口的taskId需要不同
hiTraceMeter.startAsyncTrace(COMMERCIAL, "myTestFunc", 2, "categoryTest", "key=value");
// 业务流程......
// 第一个跟踪的任务结束
hiTraceMeter.finishAsyncTrace(COMMERCIAL, "myTestFunc", 1);
// 业务流程......
// 第二个跟踪的任务结束
hiTraceMeter.finishAsyncTrace(COMMERCIAL, "myTestFunc", 2);
```

```js
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
// 跟踪串行执行的同名任务
// 第一个跟踪的任务开始
hiTraceMeter.startTrace(COMMERCIAL, "myTestFunc", 1, "categoryTest", "key=value");
// 业务流程......
// 第一个跟踪的任务结束
hiTraceMeter.finishAsyncTrace(COMMERCIAL, "myTestFunc", 1);
// 业务流程......
// 第二个跟踪的同名任务开始，同名的待跟踪任务串行执行
hiTraceMeter.startTrace(COMMERCIAL, "myTestFunc", 1, "categoryTest", "key=value");
// 业务流程......
// 第二个跟踪的同名任务结束
hiTraceMeter.finishAsyncTrace(COMMERCIAL, "myTestFunc", 1);
```

## hiTraceMeter.startSyncTrace

startSyncTrace(level: HiTraceOutputLevel, name: string, customArgs?: string): void

标记一个同步跟踪耗时任务的开始，分级控制跟踪输出。具体示例可参考[finishSyncTrace](#hitracemeterfinishsynctrace)中的示例。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android

**参数：**

| 参数名     | 类型                                      | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ---------- | ----------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| level      | [HiTraceOutputLevel](#hitraceoutputlevel) | 是   | 跟踪输出级别。（尽管该参数在 Android 和 iOS 平台不支持，但由于该参数为必填项，您仍需传入） | 不支持      | 不支持  |
| name       | string                                    | 是   | 要跟踪的任务名称。                                           | 支持        | 不支持  |
| customArgs | string                                    | 否   | 键值对，格式key=value，多个键值对用逗号分隔。<br>不传入该参数等同于传入空字符串。 | 不支持      | 不支持  |

**示例：**

```js
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
// 不需要customArgs参数时，可不传入该参数或传入空字符串
hiTraceMeter.startSyncTrace(COMMERCIAL, "myTestFunc");
hiTraceMeter.startSyncTrace(COMMERCIAL, "myTestFunc", "");
// 多个键值对用逗号分隔
hiTraceMeter.startSyncTrace(COMMERCIAL, "myTestFunc", "key=value");
hiTraceMeter.startSyncTrace(COMMERCIAL, "myTestFunc", "key1=value1,key2=value2");
```

## hiTraceMeter.finishSyncTrace

finishSyncTrace(level: HiTraceOutputLevel): void

标记一个同步跟踪耗时任务的结束，分级控制跟踪输出。

finishSyncTrace的level必须与流程开始的[startSyncTrace](#hitracemeterstartsynctrace)对应参数值一致。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android

**参数：**

| 参数名 | 类型                                      | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------ | ----------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| level  | [HiTraceOutputLevel](#hitraceoutputlevel) | 是   | 跟踪输出级别。（尽管该参数在 Android 和 iOS 平台不支持，但由于该参数为必填项，您仍需传入） | 不支持      | 不支持  |

**示例：**

```js
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
hiTraceMeter.finishSyncTrace(COMMERCIAL);
```

```js
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
// 可嵌套使用，相邻的startSyncTrace与finishSyncTrace匹配
// 第一个跟踪的任务开始
hiTraceMeter.startSyncTrace(COMMERCIAL, "myTestFunc1", "key=value");
// 业务流程......
// 第二个跟踪的任务开始
hiTraceMeter.startSyncTrace(COMMERCIAL, "myTestFunc2", "key=value");
// 业务流程......
// 第二个跟踪的任务结束
hiTraceMeter.finishSyncTrace(COMMERCIAL);
// 业务流程......
// 第一个跟踪的任务结束
hiTraceMeter.finishSyncTrace(COMMERCIAL);
```

## hiTraceMeter.traceByValue

traceByValue(level: HiTraceOutputLevel, name: string, count: number): void

整数跟踪事件，分级控制跟踪输出。用来标记一个预跟踪的整数变量名及整数值。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android（Android API 29及以上支持）

**参数：**

| 参数名 | 类型                                      | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------ | ----------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| level  | [HiTraceOutputLevel](#hitraceoutputlevel) | 是   | 跟踪输出级别。（尽管该参数在 Android 和 iOS 平台不支持，但由于该参数为必填项，您仍需传入） | 不支持      | 不支持  |
| name   | string                                    | 是   | 要跟踪的整数变量名称。                                       | 支持        | 不支持  |
| count  | number                                    | 是   | 整数变量的值。                                               | 支持        | 不支持  |

**示例：**

```js
const COMMERCIAL = hiTraceMeter.HiTraceOutputLevel.COMMERCIAL;
let traceCount = 3;
hiTraceMeter.traceByValue(COMMERCIAL, "myTestCount", traceCount);
traceCount = 4;
hiTraceMeter.traceByValue(COMMERCIAL, "myTestCount", traceCount);
// 业务流程......
```

## hiTraceMeter.isTraceEnabled

isTraceEnabled(): boolean

判断当前是否开启应用trace捕获。

开发者可使用Android端的perfetto、atrace命令行工具开启或关闭应用trace捕获。

**系统能力**：SystemCapability.HiviewDFX.HiTrace

**支持平台：** Android

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 返回true表示当前开启应用trace捕获，HiTraceMeter性能跟踪打点可生效；<br>返回false表示当前未开启应用trace捕获，HiTraceMeter性能跟踪打点无效。 |

**示例：**

```js
if (hiTraceMeter.isTraceEnabled()) {
    // 业务流程......
} else {
    // 业务流程......
}
```
