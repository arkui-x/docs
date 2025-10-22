# ArkTSUtils.locks

为了解决多并发实例间的数据竞争问题，ArkTS语言基础库引入了异步锁能力。为了开发者的开发效率，AsyncLock对象支持跨并发实例引用传递。

由于ArkTS语言支持异步操作，阻塞锁容易产生死锁问题，因此我们在ArkTS中仅支持异步锁（非阻塞式锁）。

使用异步锁的方法需要标记为async，调用方需要await修饰调用，才能保证时序正确。因此会导致外层调用函数全部标记成async。

> **说明：**
>
> 本模块首批接口从API version 22开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 此模块仅支持在ArkTS文件（文件后缀为.ets）中导入使用。

## 导入模块

```ts
import { ArkTSUtils } from '@kit.ArkTS'
```

## ConditionVariable

实现异步等待功能的类，支持异步等待通知操作。该类使用[@Sendable装饰器](../../arkts-utils/arkts-sendable.md)装饰。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

### constructor

constructor()

默认构造函数。创建一个异步等待通知操作的对象。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**示例：**

```ts
let conditionVariable = new ArkTSUtils.locks.ConditionVariable();
```

### request

static request(name: string): ConditionVariable

使用指定的名称查找或创建（如果未找到）异步等待通知操作的对象。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 名称 | 类型   | 必填 | 说明                             |
| ---- | ------ | ---- | -------------------------------- |
| name | string | 是   | 按指定名称查找或创建等待通知操作的对象名称，字符串无特别限制。 |

**返回值：**

| 类型                    | 说明                             |
| ----------------------- | -------------------------------- |
| [ConditionVariable](#conditionvariable) | 返回查找到或创建后的异步等待通知操作的实例。 |

**示例：**

```ts
let conditionVariable = ArkTSUtils.locks.ConditionVariable.request("conditionName");
```

### wait

wait(): Promise\<void>

异步调用进入等待中，将在被唤醒后继续执行。使用Promise异步回调。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型        | 说明                        |
| ----------- | --------------------------- |
| Promise\<void> | 无返回结果的Promise对象。 |

**示例：**

```ts
const conditionVariable: ArkTSUtils.locks.ConditionVariable = new ArkTSUtils.locks.ConditionVariable();
conditionVariable.wait().then(() => {
  console.info(`Thread being awakened, then continue...`); //被唤醒后输出日志
});
```

### waitFor

waitFor(timeout : number) : Promise\<void>

异步调用进入等待中, 将在被唤醒或者等待时间结束后继续执行。使用Promise异步回调。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 名称 | 类型   | 必填 | 说明       |
| -------- | -------- | ---- | ---------- |
| timeout | number | 是   | 等待时间，单位为ms，正整数。 |

**返回值：**

| 类型        | 说明                        |
| ----------- | --------------------------- |
| Promise\<void> | 无返回结果的Promise对象。 |

**示例：**

```ts
const conditionVariable: ArkTSUtils.locks.ConditionVariable = new ArkTSUtils.locks.ConditionVariable();
conditionVariable.waitFor(3000).then(() => {
  console.info(`Thread being awakened, then continue...`); //被唤醒后输出日志
});
```

### notifyAll

notifyAll() : void

通知所有等待的线程。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**示例：**

```ts
const conditionVariable: ArkTSUtils.locks.ConditionVariable = new ArkTSUtils.locks.ConditionVariable();
conditionVariable.waitFor(3000).then(() => {
  console.info(`Thread being awakened, then continue...`); //被唤醒后输出日志
});
// 通知所有等待的线程。
conditionVariable.notifyAll();
```

### notifyOne

notifyOne() : void

通知第一个等待的线程。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**示例：**

```ts
const conditionVariable: ArkTSUtils.locks.ConditionVariable = new ArkTSUtils.locks.ConditionVariable();
conditionVariable.waitFor(3000).then(() => {
  console.info(`Thread a being awakened, then continue...`); //被唤醒后输出日志
});
// 通知第一个等待的线程。
conditionVariable.notifyOne();
```