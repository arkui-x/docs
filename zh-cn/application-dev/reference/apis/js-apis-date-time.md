# @ohos.systemDateTime (系统时间、时区)

本模块主要由系统时间和系统时区功能组成。开发者可以获取系统时间及系统时区。

> **说明：**
>
> 本模块首批接口从API version 16开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { systemDateTime } from '@kit.BasicServicesKit';
```

## TimeType

定义获取时间的枚举类型。

**支持平台**： Android 、iOS

| 名称    | 值   | 说明                                             | Android平台 | iOS平台 |
| ------- | ---- | ------------------------------------------------ | ----------- | ------- |
| STARTUP | 0    | 自系统启动以来经过的时间，包括深度睡眠时间。   | 支持        | 支持    |
| ACTIVE  | 1    | 自系统启动以来经过的时间，不包括深度睡眠时间。 | 支持        | 支持    |

## systemDateTime.getTime

getTime(isNanoseconds?: boolean): number

 使用同步方式获取自Unix纪元以来经过的时间。

**支持平台**： Android 、iOS

**参数：**

| 参数名        | 类型    | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------- | ------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| isNanoseconds | boolean | 否   | 返回结果是否为纳秒数。<br>- true：表示返回结果为纳秒数（ns）。 <br>- false：表示返回结果为毫秒数（ms）。<br>默认值为false。 | 支持        | 支持    |

**返回值**：

| 类型   | 说明                       |
| ------ | -------------------------- |
| number | 自Unix纪元以来经过的时间。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

try {
  let time = systemDateTime.getTime(true)
} catch(e) {
  let error = e as BusinessError;
  console.info(`Failed to get time. message: ${error.message}, code: ${error.code}`);
}
```

## systemDateTime.getUptime

getUptime(timeType: TimeType, isNanoseconds?: boolean): number

使用同步方式获取自系统启动以来经过的时间。

**支持平台**： Android 、iOS

**参数：**

| 参数名        | 类型                  | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------- | --------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| timeType      | [TimeType](#timetype) | 是   | 获取时间的类型，仅能为`STARTUP`或者`ACTIVE`。                | 支持        | 支持    |
| isNanoseconds | boolean               | 否   | 返回结果是否为纳秒数。<br/>- true：表示返回结果为纳秒数（ns）。 <br/>- false：表示返回结果为毫秒数（ms）。<br>默认值为false。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                       |
| ------ | -------------------------- |
| number | 自系统启动以来经过的时间。 |

**错误码：**

| 错误码ID | 错误信息                                                                                                           |
| -------- |----------------------------------------------------------------------------------------------------------------|
| 401       | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified; 2.Incorrect parameter types; 3.Parameter verification failed. This error code was added due to missing issues. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

try {
  let time = systemDateTime.getUptime(systemDateTime.TimeType.ACTIVE, false);
} catch(e) {
  let error = e as BusinessError;
  console.info(`Failed to get uptime. message: ${error.message}, code: ${error.code}`);
}
```

## systemDateTime.getTimezone

getTimezone(callback: AsyncCallback&lt;string&gt;): void

获取系统时区，使用callback异步回调。

**支持平台**： Android 、iOS

**参数：**

| 参数名   | 类型              | 必填 | 说明                 | Android平台        | iOS平台            |
| -------- | --------- | ---- | ------------------------ | ------------------------ | ------------------------ |
| callback | AsyncCallback&lt;string&gt; | 是   | 回调函数，返回系统时区。 | 支持 | 支持 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

try {
  systemDateTime.getTimezone((error: BusinessError, data: string) => {
    if (error) {
      console.info(`Failed to get timezone. message: ${error.message}, code: ${error.code}`);
      return;
    }
    console.info(`Succeeded in get timezone : ${data}`);;
  });
} catch(e) {
  let error = e as BusinessError;
  console.info(`Failed to get timezone. message: ${error.message}, code: ${error.code}`);
}
```

## systemDateTime.getTimezone

getTimezone(): Promise&lt;string&gt;

获取系统时区，使用Promise异步回调。

**支持平台**： Android 、iOS

**返回值：**

| 类型                  | 说明                        |
| --------------------- | --------------------------- |
| Promise&lt;string&gt; | Promise对象，返回系统时区。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

try {
  systemDateTime.getTimezone().then((data: string) => {
    console.info(`Succeeded in getting timezone: ${data}`);
  }).catch((error: BusinessError) => {
    console.info(`Failed to get timezone. message: ${error.message}, code: ${error.code}`);
  });
} catch(e) {
  let error = e as BusinessError;
  console.info(`Failed to get timezone. message: ${error.message}, code: ${error.code}`);
}
```

## systemDateTime.getTimezoneSync

getTimezoneSync(): string

获取系统时区，使用同步方式。

**支持平台**： Android 、iOS

**返回值：**

| 类型   | 说明           |
| ------ | -------------- |
| string | 返回系统时区。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

try {
  let timezone = systemDateTime.getTimezoneSync();
} catch(e) {
  let error = e as BusinessError;
  console.info(`Failed to get timezone. message: ${error.message}, code: ${error.code}`);
}
```
