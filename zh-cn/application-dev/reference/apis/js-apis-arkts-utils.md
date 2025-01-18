# @arkts.utils (ArkTS工具库)

本模块提供了各种ArkTS实用工具函数。

> **说明：**
>
> 本模块首批接口从API version 16开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { ArkTSUtils } from '@kit.ArkTS'
```

## ArkTSUtils.locks

为了解决多并发实例间的数据竞争问题，ArkTS语言基础库引入了异步锁能力。为了开发者的开发效率，AsyncLock对象支持跨并发实例引用传递。

由于ArkTS语言支持异步操作，阻塞锁容易产生死锁问题，因此我们在ArkTS中仅支持异步锁（非阻塞式锁）。

使用异步锁的方法需要标记为async，调用方需要await修饰调用，才能保证时序正确。因此会导致外层调用函数全部标记成async。

**支持平台：** Android、iOS

### AsyncLockCallback

type AsyncLockCallback\<T> = () => T | Promise\<T>

这是一个补充类型别名，表示[lockAsync](#lockasync)函数所有重载中的回调。

**支持平台：** Android、iOS

### AsyncLock

实现异步锁功能的类，允许在锁下执行异步操作。

**支持平台：** Android、iOS

#### 属性

| 名称 | 类型   | 可读 | 可写 | 说明       | Android平台 | iOS平台 |
| ---- | ------ | ---- | ---- | ---------- | ----------- | ------- |
| name | string | 是   | 否   | 锁的名称。 | 支持        | 支持    |

**示例：**

```ts
// 示例一：
@Sendable
class A {
  count_: number = 0;
  async getCount(): Promise<number> {
    let lock: ArkTSUtils.locks.AsyncLock = ArkTSUtils.locks.AsyncLock.request("lock_1");
    return lock.lockAsync(() => {
      return this.count_;
    })
  }
  async setCount(count: number) {
    let lock: ArkTSUtils.locks.AsyncLock = ArkTSUtils.locks.AsyncLock.request("lock_1");
    await lock.lockAsync(() => {
      this.count_ = count;
    })
  }
}

// 示例二：
@Sendable
class A {
  count_: number = 0;
  lock_: ArkTSUtils.locks.AsyncLock = new ArkTSUtils.locks.AsyncLock();
  async getCount(): Promise<number> {
    return this.lock_.lockAsync(() => {
      return this.count_;
    })
  }
  async setCount(count: number) {
    await this.lock_.lockAsync(() => {
      this.count_ = count;
    })
  }
}

@Concurrent
async function foo(a: A) {
  await a.setCount(10)
}
```

#### constructor

constructor()

默认构造函数。创建一个异步锁。

**支持平台：** Android、iOS

**返回值：**

| 类型                    | 说明               |
| ----------------------- | ------------------ |
| [AsyncLock](#asynclock) | 创建的异步锁实例。 |

**示例：**

```ts
let lock = new ArkTSUtils.locks.AsyncLock();
```

#### request

static request(name: string): AsyncLock

使用指定的名称查找或创建（如果未找到）异步锁实例。

**支持平台：** Android、iOS

**参数：**

| 名称 | 类型   | 必填 | 说明                             | Android平台 | iOS平台 |
| ---- | ------ | ---- | -------------------------------- | ----------- | ------- |
| name | string | 是   | 按指定名称查找或创建异步锁实例。 | 支持        | 支持    |

**返回值：**

| 类型                    | 说明                             |
| ----------------------- | -------------------------------- |
| [AsyncLock](#asynclock) | 返回查找到或创建后的异步锁实例。 |

**示例：**

```ts
let lockName = 'isAvailableLock';
let lock = ArkTSUtils.locks.AsyncLock.request(lockName);
```

#### query

static query(name: string): AsyncLockState

查询指定异步锁的信息。

**支持平台：** Android、iOS

**参数：**

| 名称 | 类型   | 必填 | 说明       | Android平台 | iOS平台 |
| ---- | ------ | ---- | ---------- | ----------- | ------- |
| name | string | 是   | 锁的名称。 | 支持        | 支持    |

**返回值：**

| 类型                              | 说明                               |
| --------------------------------- | ---------------------------------- |
| [AsyncLockState](#asynclockstate) | 一个包含状态描述的异步锁状态实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息      |
| -------- | ------------- |
| 10200030 | The lock does not exist. |

**示例：**

```ts
// 你已经在别的地方创建了一个锁。
// let lock = ArkTSUtils.locks.AsyncLock.request("queryTestLock");
let state = ArkTSUtils.locks.AsyncLock.query('queryTestLock');
if (!state) {
    throw new Error('测试失败：期望有效的状态，但得到的是 ' + state);
}
let pending: ArkTSUtils.locks.AsyncLockInfo[] = state.pending;
let held: ArkTSUtils.locks.AsyncLockInfo[] = state.held;
```

#### queryAll

static queryAll(): AsyncLockState[]

查询所有现有锁的信息。

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明                             |
| ----------------------------------- | -------------------------------- |
| [AsyncLockState](#asynclockstate)[] | 包含锁状态信息的异步锁状态数组。 |

**示例：**

```ts
let states: ArkTSUtils.locks.AsyncLockState[] = ArkTSUtils.locks.AsyncLock.queryAll();
if (states.length == 0) {
    throw new Error('测试失败：期望至少有1个状态，但得到的是 ' + states.length);
}
```

#### lockAsync

lockAsync\<T>(callback: AsyncLockCallback\<T>): Promise\<T>

在获取的锁下独占执行操作。该方法首先获取锁，然后调用回调，最后释放锁。回调在调用[lockAsync](#lockasync)的同一线程中以异步方式执行。

**支持平台：** Android、iOS

**参数：**

| 名称     | 类型                                    | 必填 | 说明                   | Android平台 | iOS平台 |
| -------- | --------------------------------------- | ---- | ---------------------- | ----------- | ------- |
| callback | [AsyncLockCallback](#asynclockcallback) | 是   | 获取锁后要调用的函数。 | 支持        | 支持    |

**返回值：**

| 类型        | 说明                        |
| ----------- | --------------------------- |
| Promise\<T> | 回调执行后将解决的Promise。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息      |
| -------- | ------------- |
| 10200030 | The lock does not exist. |

**示例：**

```ts
let lock = new ArkTSUtils.locks.AsyncLock();
let p1 = lock.lockAsync<void>(() => {
    // 执行某些操作
});
```

#### lockAsync

lockAsync\<T>(callback: AsyncLockCallback\<T>, mode: AsyncLockMode): Promise\<T>

在获取的锁下执行操作。该方法首先获取锁，然后调用回调，最后释放锁。回调在调用[lockAsync](#lockasync)的同一线程中以异步方式执行。

**支持平台：** Android、iOS

**参数：**

| 名称     | 类型                                    | 必填 | 说明                   | Android平台 | iOS平台 |
| -------- | --------------------------------------- | ---- | ---------------------- | ----------- | ------- |
| callback | [AsyncLockCallback](#asynclockcallback) | 是   | 获取锁后要调用的函数。 | 支持        | 支持    |
| mode     | [AsyncLockMode](#asynclockmode)         | 是   | 锁的操作模式。         | 支持        | 支持    |

**返回值：**

| 类型        | 说明                        |
| ----------- | --------------------------- |
| Promise\<T> | 回调执行后将解决的Promise。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息      |
| -------- | ------------- |
| 10200030 | The lock does not exist. |

**示例：**

```ts
let lock = new ArkTSUtils.locks.AsyncLock();
let p1 = lock.lockAsync<void>(() => {
    // 执行某些操作
}, ArkTSUtils.locks.AsyncLockMode.EXCLUSIVE);
```

#### lockAsync

lockAsync\<T, U>(callback: AsyncLockCallback\<T>, mode: AsyncLockMode, options: AsyncLockOptions\<U>): Promise\<T | U>

在获取的锁下执行操作。该方法首先获取锁，然后调用回调，最后释放锁。回调在调用[lockAsync](#lockasync)的同一线程中以异步方式执行。在[AsyncLockOptions](#asynclockoptions)中可以提供一个可选的超时值。在这种情况下，如果超时前未能获取锁，lockAsync将拒绝返回的Promise并带上一个BusinessError实例。这种情况下，错误信息将包含持有的锁和等待的锁的信息以及可能的死锁警告。

**支持平台：** Android、iOS

**参数：**

| 名称     | 类型                                      | 必填 | 说明                   | Android平台 | iOS平台 |
| -------- | ----------------------------------------- | ---- | ---------------------- | ----------- | ------- |
| callback | [AsyncLockCallback](#asynclockcallback)   | 是   | 获取锁后要调用的函数。 | 支持        | 支持    |
| mode     | [AsyncLockMode](#asynclockmode)           | 是   | 锁的操作模式。         | 支持        | 支持    |
| options  | [AsyncLockOptions\<U>](#asynclockoptions) | 是   | 锁的操作选项。         | 支持        | 支持    |

**返回值：**

| 类型             | 说明                                               |
| ---------------- | -------------------------------------------------- |
| Promise\<T \| U> | 回调执行后解决的 Promise，或者在超时情况下被拒绝。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息          |
| -------- | ----------------- |
| 10200030 | The lock does not exist.     |
| 10200031 | Timeout exceeded. |

**示例：**

```ts
let lock = new ArkTSUtils.locks.AsyncLock();
let options = new ArkTSUtils.locks.AsyncLockOptions<void>();
options.timeout = 1000;
let p: Promise<void> = lock.lockAsync<void, void>(
    () => {
        // 执行某些操作
    },
    ArkTSUtils.locks.AsyncLockMode.EXCLUSIVE,
    options
);
```

### AsyncLockMode

锁操作对应的模式枚举。

**支持平台：** Android、iOS

| 名称      | 值   | 说明                                                     | Android平台 | iOS平台 |
| --------- | ---- | -------------------------------------------------------- | ----------- | ------- |
| SHARED    | 1    | 共享锁模式。如果指定了此模式，可以在任意线程同时执行。   | 支持        | 支持    |
| EXCLUSIVE | 2    | 独占锁模式。如果指定了此模式，仅在独占获取锁时才能执行。 | 支持        | 支持    |

**示例：**

```ts
let lock = new ArkTSUtils.locks.AsyncLock();
// shared0可获取锁并开始执行
lock.lockAsync(async () => {
    console.log('shared0');
    await new Promise<void>((resolve) => setTimeout(resolve, 1000));
}, ArkTSUtils.locks.AsyncLockMode.SHARED);
// shared1可获取锁并开始执行，无需等待shared0
lock.lockAsync(async () => {
    console.log('shared1');
    await new Promise<void>((resolve) => setTimeout(resolve, 1000));
}, ArkTSUtils.locks.AsyncLockMode.SHARED);
// exclusive0需等待shared0、1执行完后才可获取锁并执行
lock.lockAsync(async () => {
    console.log('exclusive0');
    await new Promise<void>((resolve) => setTimeout(resolve, 1000));
}, ArkTSUtils.locks.AsyncLockMode.EXCLUSIVE);
// shared2需等待exclusive0执行完后才可获取锁并执行
lock.lockAsync(async () => {
    console.log('shared2');
    await new Promise<void>((resolve) => setTimeout(resolve, 1000));
}, ArkTSUtils.locks.AsyncLockMode.SHARED);
// shared3需等待exclusive0执行完后才可获取锁并执行，无需等待shared2
lock.lockAsync(async () => {
    console.log('shared3');
    await new Promise<void>((resolve) => setTimeout(resolve, 1000));
}, ArkTSUtils.locks.AsyncLockMode.SHARED);
```

### AsyncLockOptions

class AsyncLockOptions\<T>

表示锁操作选项的类。

**支持平台：** Android、iOS

#### constructor

constructor()

默认构造函数。创建一个所有属性均具有默认值的异步锁配置项实例。

**支持平台：** Android、iOS

**返回值：**

| 类型                                  | 说明                   |
| ------------------------------------- | ---------------------- |
| [AsyncLockOptions](#asynclockoptions) | 新的异步锁配置项实例。 |

**示例：**

```ts
let s: ArkTSUtils.locks.AbortSignal<string> = { aborted: false, reason: 'Aborted' };
let options = new ArkTSUtils.locks.AsyncLockOptions<string>();
options.isAvailable = false;
options.signal = s;
```

#### 属性

| 名称        | 类型                                  | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ----------- | ------------------------------------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| isAvailable | boolean                               | 是   | 是   | 当前锁是否可用。取值为true，则只有在尚未持有锁定请求时才会授予该锁定请求；为false则表示将等待当前锁被释放。默认为 false。 | 支持        | 支持    |
| signal      | [AbortSignal\<T>](#abortsignal)\|null | 是   | 是   | 用于中止异步操作的对象。如果signal.aborted为true，则锁请求将被丢弃；为null则请求正常排队运行。默认为 null。 | 支持        | 支持    |
| timeout     | number                                | 是   | 是   | 锁操作的超时时间（毫秒）。如果该值大于零，且运行超过该时间，[lockAsync](#lockasync)将返回被拒绝的Promise。默认为 0。 | 支持        | 支持    |

### AsyncLockState

用于存储特定异步锁实例上当前执行的所有锁操作的信息的类。

**支持平台：** Android、iOS

#### 属性

| 名称    | 类型                              | 可读 | 可写 | 说明             | Android平台 | iOS平台 |
| ------- | --------------------------------- | ---- | ---- | ---------------- | ----------- | ------- |
| held    | [AsyncLockInfo[]](#asynclockinfo) | 是   | 是   | 持有的锁信息。   | 支持        | 支持    |
| pending | [AsyncLockInfo[]](#asynclockinfo) | 是   | 是   | 等待中的锁信息。 | 支持        | 支持    |

### AsyncLockInfo

关于锁的信息。

**支持平台：** Android、iOS

#### 属性

| 名称      | 类型                            | 可读 | 可写 | 说明                                                      | Android平台 | iOS平台 |
| --------- | ------------------------------- | ---- | ---- | --------------------------------------------------------- | ----------- | ------- |
| name      | string                          | 是   | 是   | 锁的名称。                                                | 支持        | 支持    |
| mode      | [AsyncLockMode](#asynclockmode) | 是   | 是   | 锁的模式。                                                | 支持        | 支持    |
| contextId | number                          | 是   | 是   | [AsyncLockMode](#asynclockmode)调用者的执行上下文标识符。 | 支持        | 支持    |

### AbortSignal

用于中止异步操作的对象。该类的实例必须在其创建的同一线程中访问。从其他线程访问此类的字段会导致未定义的行为。

**支持平台：** Android、iOS

#### 属性

| 名称    | 类型    | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------- | ------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| aborted | boolean | 是   | 是   | 设置为true以中止操作。                                       | 支持        | 支持    |
| reason  | \<T>    | 是   | 是   | 中止的原因。此值将用于拒绝[lockAsync](#lockasync)返回的Promise。 | 支持        | 支持    |

## ArkTSUtils.ASON

为支持将JSON字符串解析成共享数据，即[Sendable支持的数据类型](../arkui-ts/arkts-sendable.md#sendable支持的数据类型)，ArkTS语言基础库新增了ASON工具。ASON支持开发者解析JSON字符串，并生成共享数据进行跨并发域传输，同时ASON也支持将共享数据转换成JSON字符串。

**支持平台：** Android、iOS

### ISendable

type ISendable = lang.ISendable

ISendable是所有Sendable类型（除`null`和`undefined`）的父类型。自身没有任何必须的方法和属性。

**支持平台：** Android、iOS

| 类型 | 说明   | Android平台 | iOS平台 |
| ------ | ------ | ------ | ------ |
| [lang.ISendable](js-apis-arkts-lang.md#langisendable)   | 所有Sendable类型的父类型。 | 支持 | 支持 |

### Transformer

type Transformer = (this: ISendable, key: string, value: ISendable | undefined | null) => ISendable | undefined | null

用于转换结果函数的类型。

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明            | Android平台   | iOS平台       |
| ------ | ------ | ---- | --------------- | --------------- | --------------- |
| this   | [ISendable](#isendable) | 是 | 在解析的键值对所属的对象。| 支持 | 支持 |
| key  | string | 是 | 属性名。| 支持 | 支持 |
| value  | [ISendable](#isendable) | 是 | 在解析的键值对的值。| 支持 | 支持 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [ISendable](#isendable) \| undefined \| null | 返回转换结果后的ISendable对象或undefined或null。|

### BigIntMode

定义处理BigInt的模式。

**支持平台：** Android、iOS

| 名称 | 值| 说明            | Android平台   | iOS平台 |
| ------ | ------ | --------------- | --------------- | --------------- |
| DEFAULT   | 0 |不支持BigInt。|支持|支持|
| PARSE_AS_BIGINT   | 1 |当整数小于-(2^53-1)或大于(2^53-1)时，解析为BigInt。|支持|支持|
| ALWAYS_PARSE_AS_BIGINT   | 2 |所有整数都解析为BigInt。|支持|支持|

### ParseReturnType

定义解析结果的返回类型。

**支持平台：** Android、iOS

| 名称 | 值| 说明            | Android平台   | iOS平台 |
| ------ | ------ | --------------- | --------------- | --------------- |
| OBJECT   | 0 |返回 SendableObject 对象。<br>**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。|支持|支持|

### ParseOptions

解析的选项，可定义处理BigInt的模式与解析结果的返回类型。

**支持平台：** Android、iOS

| 名称 | 类型| 必填 | 说明            | Android平台   | iOS平台 |
| ------ | ------ | ---- | --------------- | --------------- | --------------- |
| bigIntMode   | [BigIntMode](#bigintmode) | 是 |定义处理BigInt的模式。|支持|支持|
| parseReturnType   | [ParseReturnType](#parsereturntype) | 是 |定义解析结果的返回类型。|支持|支持|

### parse

parse(text: string, reviver?: Transformer, options?: ParseOptions): ISendable | null

用于解析JSON字符串生成ISendable数据或null。

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明            | Android平台   | iOS平台       |
| ------ | ------ | ---- | --------------- | --------------- | --------------- |
| text   | string | 是 | 有效的JSON字符串。| 支持 | 支持 |
| reviver   | [Transformer](#transformer) | 否 | 转换函数，传入该参数，可以用来修改解析生成的原始值。默认值是undefined。目前只支持传入undefined。| 支持 | 支持 |
| options   | [ParseOptions](#parseoptions) | 否 | 解析的配置，传入该参数，可以用来控制解析生成的结果类型。默认值是undefined。| 支持 | 支持 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [ISendable](#isendable) \| null | 返回ISendable数据或null。当入参是null时，返回null。|

**示例：**

```ts
import { lang } from '@kit.ArkTS';
import { collections } from '@kit.ArkTS';

type ISendable = lang.ISendable;
let jsonText = '{"name": "John", "age": 30, "city": "ChongQing"}';
let obj = ArkTSUtils.ASON.parse(jsonText) as ISendable;
console.info((obj as object)?.["name"]);
// 期望输出: 'John'
console.info((obj as object)?.["age"]);
// 期望输出: 30
console.info((obj as object)?.["city"]);
// 期望输出: 'ChongQing'

let options: ArkTSUtils.ASON.ParseOptions = {
  bigIntMode: ArkTSUtils.ASON.BigIntMode.PARSE_AS_BIGINT,
  parseReturnType: ArkTSUtils.ASON.ParseReturnType.OBJECT,
}
let numberText = '{"largeNumber":112233445566778899}';
let numberObj = ArkTSUtils.ASON.parse(numberText,undefined,options) as ISendable;

console.info((numberObj as object)?.["largeNumber"]);
// 期望输出: 112233445566778899

let options2: ArkTSUtils.ASON.ParseOptions = {
    bigIntMode: ArkTSUtils.ASON.BigIntMode.PARSE_AS_BIGINT,
    parseReturnType: ArkTSUtils.ASON.ParseReturnType.MAP,
  }
let mapText = '{"largeNumber":112233445566778899}';
let map  = ArkTSUtils.ASON.parse(mapText,undefined,options2);
console.info("map is " + map);
// 期望输出: map is [object SendableMap]
console.info("largeNumber is " + (map as collections.Map<string,bigint>).get("largeNumber"));
// 期望输出: largeNumber is 112233445566778899
```

### stringify

stringify(value: ISendable | null | undefined): string

该方法将ISendable数据转换为JSON字符串。

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- |
| value | [ISendable](#isendable) \| null \| undefined  | 是 | ISendable数据。| 支持 | 支持 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| string | 转换后的JSON字符串。|

**示例：**

```ts
import { collections } from '@kit.ArkTS';

let arr = new collections.Array(1, 2, 3);
let str = ArkTSUtils.ASON.stringify(arr);
console.info(str);
// 期望输出: '[1,2,3]'
```

### isSendable

isSendable(value: Object | null | undefined): boolean

该方法用于判断value是否为Sendable数据类型。

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- |
| value | Object \| null \| undefined  | 是 | 待校验的对象。| 支持 | 支持 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| boolean | value是否为Sendable数据类型，true表示value是Sendable数据类型，否则为false。|

**示例：**

```ts
import { ArkTSUtils } from '@kit.ArkTS'

@Sendable
function sendableFunc()
{
  console.info("sendableFunc")
}

if (ArkTSUtils.isSendable(sendableFunc)) {
  console.info("sendableFunc is Sendable");
} else {
  console.info("sendableFunc is not Sendable");
}
// 期望输出: 'SendableFunc is Sendable'
```
