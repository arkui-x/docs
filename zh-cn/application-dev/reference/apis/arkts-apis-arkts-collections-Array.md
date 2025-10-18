# Class (Array)

> **说明：**
>
> 本模块首批接口从API version 22开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 此模块仅支持在ArkTS文件（文件后缀为.ets）中导入使用。

一种线性数据结构，底层基于数组实现，可以在ArkTS上并发实例间传递。

当需要在ArkTS上并发实例间传递Array时，可以通过传递Array引用提升传递性能。

文档中存在泛型的使用，涉及以下泛型标记符：

- T：Type，支持[Sendable支持的数据类型](../../arkui-ts/arkts-sendable.md#sendable支持的数据类型)。

**装饰器类型：**\@Sendable

## 导入模块

```ts
import { collections } from '@kit.ArkTS';
```

## 属性

**系统能力：** SystemCapability.Utils.Lang

| 名称   | 类型   | 只读 | 可选 | 说明              |
| ------ | ------ | ---- | ---- | ----------------- |
| length | number | 是   | 否   | Array的元素个数。 |

## from

static from\<T>(arrayLike: ArrayLike\<T> | Iterable\<T>, mapFn: ArrayFromMapFn\<T, T>): Array\<T>

从一个实现了ArrayLike接口的对象创建一个新的ArkTS Array，并且使用自定义函数处理每个数组元素。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名    | 类型          | 必填 | 说明                            |
| --------- | ------------- | ---- | ------------------------------- |
| arrayLike | ArrayLike\<T> \| Iterable\<T> | 是   | 用于构造ArkTS Array的对象。 |
| mapFn | [ArrayFromMapFn](arkts-apis-arkts-collections-Types.md#arrayfrommapfn)\<T,T> | 是   | 调用数组每个元素的函数。 |

**返回值：**

| 类型      | 说明                    |
| --------- | ----------------------- |
| Array\<T> | 新创建的ArkTS Array实例。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 401 | Parameter error: Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
let array : Array<number> = [1, 2, 3]; // 原生Array<T>，T是Sendable数据类型。
let newArray = collections.Array.from<number>(array, (value, index) => value + index); // 返回新的 Array<T>
console.info(newArray.toString()); // 预期输出： 1, 3, 5
```

## from

static from\<U, T>(arrayLike: ArrayLike\<U> | Iterable\<U>, mapFn: ArrayFromMapFn\<U, T>): Array\<T>

从一个实现了ArrayLike接口的对象创建一个新的ArkTS Array，并且使用自定义函数处理每个数组元素，ArrayLike接口对象的元素类型可以和数组元素的类型不一样。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名    | 类型          | 必填 | 说明                            |
| --------- | ------------- | ---- | ------------------------------- |
| arrayLike | ArrayLike\<U> \| Iterable\<U> | 是   | 用于构造ArkTS Array的对象。 |
| mapFn | [ArrayFromMapFn](arkts-apis-arkts-collections-Types.md#arrayfrommapfn)\<U, T> | 是   | 调用数组每个元素的函数。 |

**返回值：**

| 类型      | 说明                    |
| --------- | ----------------------- |
| Array\<T> | 新创建的ArkTS Array实例。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 401 | Parameter error: Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
let array : Array<number> = [1, 2, 3]; // 原生Array<T>
let newArray = collections.Array.from<number, string>(array, (value, index) => value + "." + index); // 返回新的 Array<T>
console.info(newArray.toString()); // 预期输出： 1.0, 2.1, 3.2
```

## isArray

static isArray(value: Object | undefined | null): boolean

检查传入的参数是否是一个ArkTS Array。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名    | 类型          | 必填 | 说明                            |
| --------- | ------------- | ---- | ------------------------------- |
| value | Object \| undefined \| null | 是   | 需要被检查的值。 |

**返回值：**

| 类型      | 说明                    |
| --------- | ----------------------- |
| boolean | 假如给定对象是ArkTS Array数组，返回true，否则返回false。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 401 | Parameter error: Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
let arr: collections.Array<string> = new collections.Array('a', 'b', 'c', 'd');
let result: boolean = collections.Array.isArray(arr);
console.info(result + ''); // 预期输出： true
```

## of

static of\<T>(...items: T\[]): Array\<T>

通过可变数量的参数创建一个新的ArkTS Array。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名    | 类型          | 必填 | 说明                            |
| --------- | ------------- | ---- | ------------------------------- |
| items | T[] | 否   | 用于创建数组的元素集合，参数个数可以是0个、1个或者多个。 |

**返回值：**

| 类型      | 说明                    |
| --------- | ----------------------- |
| Array\<T> | 新的ArkTS Array实例。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 401 | Parameter error: Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
let arr: collections.Array<string> = collections.Array.of('a', 'b', 'c', 'd');
console.info(arr.toString()); // 预期输出： a, b, c, d
```

## copyWithin
copyWithin(target: number, start: number, end?: number): Array\<T>

从ArkTS Array指定范围内的元素依次拷贝到目标位置。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名  | 类型   | 必填 | 说明                                                         |
| ------- | ------ | ---- | ------------------------------------------------------------ |
| target | number | 是 | 目标起始位置的下标，如果`target < 0`，则会从`target + array.length`位置开始。 |
| start | number | 是 | 源起始位置下标，如果`start < 0`，则会从`start + array.length`位置开始。 |
| end | number | 否 | 源终止位置下标，如果`end < 0`，则会从`end + array.length`位置终止。默认为ArkTS Array的长度。|

**返回值：**

| 类型         | 说明      |
| ------------ | --------- |
| Array\<T> | 修改后的Array。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------ |
| 401 | Parameter error: Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |
| 10200011 | The copyWithin method cannot be bound.           |
| 10200201 | Concurrent modification error.               |

**示例：**

```ts
let array: collections.Array<number> = collections.Array.from([1, 2, 3, 4, 5, 6, 7, 8]);
let copied: collections.Array<number> = array.copyWithin(3, 1, 3);
console.info(copied.toString()); // 预期输出： 1, 2, 3, 2, 3, 6, 7, 8
```

## lastIndexOf

lastIndexOf(searchElement: T, fromIndex?: number): number

返回ArkTS Array实例中最后一次出现searchElement的索引，如果对象不包含，则为-1。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名           | 类型     | 必填  | 说明                                                                                |
| ------------- | ------ | --- | --------------------------------------------------------------------------------- |
| searchElement | T | 是   | 待索引的值。                                                                            |
| fromIndex     | number | 否   | 搜索的起始下标。默认值为0。如果下标大于等于ArkTS Array的长度，则返回-1。如果提供的下标值是负数，则从数组末尾开始倒数计数：使用 fromIndex + array.length 的值。 |

**返回值：**

| 类型     | 说明                      |
| ------ | ----------------------- |
| number | 数组中元素的最后一个索引；没有找到，则返回-1。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                    |
| -------- | --------------------------------------- |
| 10200011 | The lastIndexOf method cannot be bound. |
| 10200201 | Concurrent modification error.      |

**示例：**

```ts
let array: collections.Array<number> = collections.Array.from([3, 5, 9]);
console.info(array.lastIndexOf(3) + ''); // 预期输出： 0
console.info(array.lastIndexOf(7) + ''); // 预期输出： -1
console.info(array.lastIndexOf(9, 2) + ''); // 预期输出： 2
console.info(array.lastIndexOf(9, -2) + ''); // 预期输出： -1
```

## some
some(predicate: ArrayPredicateFn\<T, Array\<T>>): boolean

测试ArkTS Array是否存在满足指定条件的元素。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名  | 类型   | 必填 | 说明                                                  |
| ------- | ------ | ---- | ---------------------------------------------------- |
| predicate | [ArrayPredicateFn](arkts-apis-arkts-collections-Types.md#arraypredicatefn)\<T, Array\<T>> | 是 | 用于测试的断言函数。|

**返回值：**

| 类型         | 说明      |
| ------------ | --------- |
| boolean | 如果存在元素满足指定条件返回true，否则返回false。|

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                            |
| -------- | ---------------------------------- |
| 10200011 | The some method cannot be bound.   |
| 10200201 | Concurrent modification error. |

**示例：**

```ts
let newArray: collections.Array<number> = collections.Array.from([-10, 20, -30, 40, -50]);
console.info(newArray.some((element: number) => element < 0) + ''); // 预期输出： true
```

## reduceRight

reduceRight(callbackFn: ArrayReduceCallback\<T, T, Array\<T>>): T

对Array中的每个元素按照从右到左顺序执行回调函数，将其结果作为累加值，并返回最终的结果。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名        | 类型                                                                               | 必填  | 说明                                         |
| ---------- | -------------------------------------------------------------------------------- | --- | ------------------------------------------ |
| callbackFn | [ArrayReduceCallback](arkts-apis-arkts-collections-Types.md#arrayreducecallback)\<T, T, Array\<T>> | 是   | 一个接受四个参数的函数，用于对每个元素执行操作，并将结果作为累加值传递给下一个元素。 |

**返回值：**

| 类型  | 说明            |
| --- | ------------- |
| T   | 回调函数执行后的最终结果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                    |
| -------- | --------------------------------------- |
| 401 | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types. |
| 10200011 | The reduceRight method cannot be bound. |
| 10200201 | Concurrent modification error.          |

**示例：**

```ts
let array = new collections.Array<number>(1, 2, 3, 4, 5);
let reducedValue = array.reduceRight((accumulator, value) => accumulator + value); // 累加所有元素
console.info(reducedValue + ''); // 预期输出： 15
```

## reduceRight

reduceRight\<U = T>(callbackFn: ArrayReduceCallback\<U, T, Array\<T>>, initialValue: U): U

与 [reduceRight](#reduceright)方法类似，但它接受一个初始值作为第二个参数，用于在Array从右到左顺序遍历开始前初始化累加器。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名          | 类型                                                                                           | 必填  | 说明                                         |
| ------------ | -------------------------------------------------------------------------------------------- | --- | ------------------------------------------ |
| callbackFn   | [ArrayReduceCallback](arkts-apis-arkts-collections-Types.md#arrayreducecallback)\<U, T, Array\<T>> | 是   | 一个接受四个参数的函数，用于对每个元素执行操作，并将结果作为累加值传递给下一个元素。 |
| initialValue | U                                                                                            | 是   | 用于初始化累加器的值。                                |

**返回值：**

| 类型  | 说明            |
| --- | ------------- |
| U   | 回调函数执行后的最终结果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                    |
| -------- | --------------------------------------- |
| 401 | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types. |
| 10200011 | The reduceRight method cannot be bound. |
| 10200201 | Concurrent modification error.          |

**示例：**

```ts
// 此处使用一个初始值为0的累加器，并将其与Array中的每个元素相加，最终返回累加后的总和
let array = new collections.Array<number>(1, 2, 3, 4, 5);
let reducedValue = array.reduceRight<number>((accumulator: number, value: number) => accumulator + value, 0); // 累加所有元素，初始值为0
console.info(reducedValue + ''); // 预期输出： 15
```

## reverse

reverse(): Array\<T>

反转ArkTS Array数组中的元素，并返回同一数组的引用。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型    | 说明                 |
| ----- | ------------------ |
| Array\<T> | 反转后的ArkTS Array对象。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID    | 错误信息                                |
| -------- | ----------------------------------- |
| 10200011 | The reverse method cannot be bound. |
| 10200201 | Concurrent modification error.  |

**示例：**

```ts
let array = new collections.Array<number>(1, 2, 3, 4, 5);
let reversed = array.reverse();
console.info(array.toString()); // 预期输出： 5, 4, 3, 2, 1
```

## toString

toString(): string

ArkTS数组转换为字符串。

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
let array = new collections.Array<number>(1, 2, 3, 4, 5);
let stringArray = array.toString();
console.info(stringArray); // 预期输出：1,2,3,4,5
```

## every

every(predicate: ArrayPredicateFn\<T, Array\<T>>): boolean

测试ArkTS Array中的所有元素是否满足指定条件。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.Utils.Lang

**参数：**
| 参数名  | 类型   | 必填 | 说明                                                    |
| ------- | ------ | ---- | ----------------------------------------------------- |
| predicate | [ArrayPredicateFn](arkts-apis-arkts-collections-Types.md#arraypredicatefn)\<T, Array\<T>> | 是 | 用于测试的断言函数。|

**返回值：**

| 类型         | 说明      |
| ------------ | --------- |
| boolean | 如果所有元素都满足指定条件则返回true，否则返回false。|

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------- |
| 10200011 | The every method cannot be bound. |
| 10200201 | Concurrent modification error. |

**示例：**

```ts
let newArray: collections.Array<number> = collections.Array.from([-10, 20, -30, 40, -50]);
console.info(newArray.every((element: number) => element > 0) + ''); // 预期输出：false
```

## toLocaleString

toLocaleString(): string

根据当前应用的系统地区获取符合当前文化习惯的字符串表示形式，让每个元素调用自己的toLocaleString方法转换为字符串，然后使用逗号将每个元素的结果字符串按照顺序拼接成字符串。

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
let array = new collections.Array<number | string>(1000, 'Test', 53621);
let stringArray = array.toLocaleString();
console.info(stringArray); // 预期输出：1,000,Test,53,621
```