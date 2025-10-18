# Class (Uint16Array)

> **说明：**
>
> 本模块首批接口从API version 22开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 此模块仅支持在ArkTS文件（文件后缀为.ets）中导入使用。

一种线性数据结构，底层基于[ArkTS ArrayBuffer](arkts-apis-arkts-collections-ArrayBuffer.md)实现。

**装饰器类型：**\@Sendable

## 导入模块

```ts
import { collections } from '@kit.ArkTS';
```

## 属性

**系统能力：** SystemCapability.Utils.Lang

| 名称   | 类型   | 只读 | 可选 | 说明              |
| ------ | ------ | ---- | ---- | ----------------|
| buffer | ArrayBuffer | 是   | 否  | ArkTS Uint16Array底层使用的buffer。|
| byteLength | number | 是   | 否   | ArkTS Uint16Array的所占的字节数。|
| byteOffset | number | 是   | 否   | ArkTS Uint16Array距离其ArrayBuffer起始位置的偏移。|
| length | number | 是   | 否  | ArkTS Uint16Array元素个数。|
| BYTES_PER_ELEMENT | number | 是   | 否   | ArkTS Uint16Array中每个元素所占用的字节数。|

## of

static of(...items: number[]): Uint16Array

通过可变数量的参数创建一个新的ArkTS Uint16Array对象，参数个数可以是0个、1个或者多个。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名    | 类型          | 必填 | 说明                            |
| --------- | ------------- | ---- | ------------------------------- |
| items | number[] | 否   | 用于创建数组的元素，参数个数可以是0个、1个或者多个。 |

**返回值：**

| 类型      | 说明                    |
| --------- | ----------------------- |
| Uint16Array | 新的ArkTS Uint16Array实例。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 401 | Parameter error: Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |

**示例：**

```ts
let arr: collections.Uint16Array = collections.Uint16Array.of(1, 2, 3, 4);
console.info(arr.toString()); // 预期输出：1,2,3,4
```

## toString

toString(): string

ArkTS Uint16Array转换为字符串。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型         | 说明            |
| ---------- | ------------- |
| string | 一个包含数组所有元素的字符串。 |

**错误码：**

以下错误码详细介绍请参考[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                 |
| -------- | ------------------------------------ |
| 10200011 | The toString method cannot be bound. |
| 10200201 | Concurrent modification error.       |

**示例：**

```ts
let array = new collections.Uint16Array([1, 2, 3, 4, 5]);
let stringArray = array.toString();
console.info(stringArray); // 预期输出：1,2,3,4,5
```

## toLocaleString

toLocaleString(): string

根据当前应用的系统地区获取符合当前文化习惯的数字表示形式，让每个元素调用自己的toLocaleString方法把数字转换为字符串，然后使用逗号将每个元素的结果字符串按照顺序拼接成字符串。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型         | 说明            |
| ---------- | ------------- |
| string | 一个包含数组所有元素的字符串。 |

**错误码：**

以下错误码详细介绍请参考[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                       |
| -------- | ------------------------------------------ |
| 10200011 | The toLocaleString method cannot be bound. |
| 10200201 | Concurrent modification error.             |

**示例：**

```ts
// 当前应用所在系统为法国地区
let array = new collections.Uint16Array([1000, 2000, 3000]);
let stringArray = array.toLocaleString();
console.info(stringArray); // 预期输出：1,000,2,000,3,000
```

## lastIndexOf

lastIndexOf(searchElement: number, fromIndex?: number): number

返回ArkTS Uint16Array实例中最后一次出现searchElement的索引，如果对象不包含，则为-1。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名           | 类型     | 必填  | 说明                                                                                |
| ------------- | ------ | --- | --------------------------------------------------------------------------------- |
| searchElement | number | 是   | 待索引的值。                                                                            |
| fromIndex     | number | 否   | 搜索的起始下标。默认值为0。如果下标大于等于ArkTS Uint16Array的长度，则返回-1。如果提供的下标值是负数，则被当做距离数组尾部的偏移，从后到前搜索。 |

**返回值：**

| 类型     | 说明                      |
| ------ | ----------------------- |
| number | 数组中给定元素的最后一个索引；没有找到，则返回-1。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                    |
| -------- | --------------------------------------- |
| 10200011 | The lastIndexOf method cannot be bound. |
| 10200201 | Concurrent modification error. |

**示例：**

```ts
let array: collections.Uint16Array = collections.Uint16Array.from([3, 5, 9]);
console.info(array.lastIndexOf(3) + ''); // 预期输出：0
console.info(array.lastIndexOf(7) + ''); // 预期输出：-1
console.info(array.lastIndexOf(9, 2) + ''); // 预期输出：2
console.info(array.lastIndexOf(9, -2) + ''); // 预期输出：-1
```

## reduceRight

reduceRight(callbackFn: TypedArrayReduceCallback\<number, number, Uint16Array>): number

反向遍历ArkTS Uint16Array，对ArkTS Uint16Array中的每个元素执行归约函数，并返回最终的归约结果。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**
| 参数名     | 类型   | 必填 |  说明     |
| ---------- | ---------------------- | ---- | ------------------------------------------------------------ |
| callbackFn | [TypedArrayReduceCallback](arkts-apis-arkts-collections-Types.md#typedarrayreducecallback)\<number, number, Uint16Array> | 是 | 归约函数。 |

**返回值：**

| 类型     | 说明          |
| ------ | ----------- |
| number | 由归约函数返回的结果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                    |
| -------- | --------------------------------------- |
| 401 | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types. |
| 10200011 | The reduceRight method cannot be bound. |
| 10200201 | Concurrent modification error.      |

**示例：**

```ts
let array: collections.Uint16Array = collections.Uint16Array.from([1, 2, 3, 4, 5]);
let reducedValue: number = array.reduceRight((accumulator: number, value: number) => accumulator + value);
console.info(reducedValue + ''); // 预期输出： 15
```

## reduceRight

reduceRight\<U = number>(callbackFn: TypedArrayReduceCallback\<U, number, Uint16Array>, initialValue: U): U

反向遍历ArkTS Uint16Array，对ArkTS Uint16Array中的每个元素执行归约函数，且接收一个初始值作为归约函数首次调用的参数，并返回最终的归约结果。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**
| 参数名    | 类型   | 必填 | 说明                                                 |
| --------- | ------ | ---- | --------------------------------------------------- |
| callbackFn | [TypedArrayReduceCallback](arkts-apis-arkts-collections-Types.md#typedarrayreducecallback)\<U, number, Uint16Array> | 是  | 归约函数。 |
| initialValue | U | 是  | 初始值。 |

**返回值：**

| 类型     | 说明          |
| ------ | ----------- |
| U | 由归约函数返回的结果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                    |
| -------- | --------------------------------------- |
| 401 | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types. |
| 10200011 | The reduceRight method cannot be bound. |
| 10200201 | Concurrent modification error.      |

**示例：**

```ts
let array: collections.Uint16Array = collections.Uint16Array.from([1, 2, 3, 4, 5]);
let reducedValue: number = array.reduceRight((accumulator: number, value: number) => accumulator + value, 1);
console.info(reducedValue + ''); // 预期输出： 16
```