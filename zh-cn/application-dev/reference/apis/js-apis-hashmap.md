# @ohos.util.HashMap (非线性容器HashMap)

HashMap底层使用数组+链表+红黑树的方式实现，查询、插入和删除的效率都很高。HashMap存储内容基于key-value的键值对映射，不能有重复的key，且一个key只能对应一个value。

HashMap和[TreeMap](js-apis-treemap.md)相比，HashMap依据键的hashCode存取数据，访问速度较快。而TreeMap是有序存取，效率较低。

[HashSet](js-apis-hashset.md)基于HashMap实现。HashMap的输入参数由key、value两个值组成。在HashSet中，只对value对象进行处理。

**推荐使用场景：** 需要快速存取、删除以及插入键值对数据时，推荐使用HashMap。

文档中存在泛型的使用，涉及以下泛型标记符：<br>
- K：Key，键<br>
- V：Value，值

> **说明：**
>
> 本模块首批接口从API version 8开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```ts
import HashMap from '@ohos.util.HashMap'; 
```

## HashMap

### 属性

**系统能力：** SystemCapability.Utils.Lang

| 名称 | 类型 | 可读 | 可写 | 说明 |
| -------- | -------- | -------- | -------- | -------- |
| length | number | 是 | 否 | HashMap的元素个数。 |


### constructor

constructor()

HashMap的构造函数。

**系统能力：** SystemCapability.Utils.Lang

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200012 | The HashMap's constructor cannot be directly invoked. |

**示例：**

```ts
let hashMap = new HashMap();
```


### isEmpty

isEmpty(): boolean

判断该HashMap是否为空。

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| boolean | 为空返回true，不为空返回false。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The isEmpty method cannot be bound. |

**示例：**

```ts
const hashMap = new HashMap();
let result = hashMap.isEmpty();
```


### hasKey

hasKey(key: K): boolean

判断此HashMap中是否含有该指定key。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| key | K | 是 | 指定Key。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| boolean | 包含指定Key返回true，否则返回false。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The hasKey method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
let result = hashMap.hasKey("squirrel");
```


### hasValue

hasValue(value: V): boolean

判断此HashMap中是否含有该指定value。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| value | V | 是 | 指定value。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| boolean | 包含指定value返回true，否则返回false。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The hasValue method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
let result = hashMap.hasValue(123);
```


### get

get(key: K): V

获取指定key所对应的value。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| key | K | 是 | 查找的指定key。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| V | 返回key映射的value值。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The get method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);
let result = hashMap.get("sparrow");
```


### setAll

setAll(map: HashMap<K, V>): void

将一个HashMap中的所有元素组添加到另一个hashMap中。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| map | HashMap<K, V> | 是 | 被添加元素的hashMap。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The setAll method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);
let newHashMap = new HashMap();
newHashMap.set("newMap", 99);
hashMap.setAll(newHashMap);
```


### set

set(key: K, value: V): Object

向HashMap中添加一组数据。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| key | K | 是 | 添加成员数据的键名。 |
| value | V | 是 | 添加成员数据的值。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Object | 返回添加后的hashMap。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The set method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
let result = hashMap.set("squirrel", 123);
```


### remove

remove(key: K): V

删除指定key所对应元素。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| key | K | 是 | 指定key。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| V | 返回删除元素的值。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The remove method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);
let result = hashMap.remove("sparrow");
```


### clear

clear(): void

清除HashMap中的所有元素,并把length置为0。

**系统能力：** SystemCapability.Utils.Lang

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The clear method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);
hashMap.clear();
```


### keys

keys(): IterableIterator&lt;K&gt;

返回包含此映射中包含的键的新迭代器对象。

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| IterableIterator&lt;K&gt; | 返回一个迭代器。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The keys method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);
let iter = hashMap.keys();
let temp = iter.next().value;
while(temp != undefined) {
  console.log("value:" + temp);
  temp = iter.next().value;
}
```


### values

values(): IterableIterator&lt;V&gt;

返回包含此映射中包含的键值的新迭代器对象。

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| IterableIterator&lt;V&gt; | 返回一个迭代器。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The values method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);
let iter = hashMap.values();
let temp = iter.next().value;
while(temp != undefined) {
  console.log("value:" + temp);
  temp = iter.next().value;
}
```


### replace

replace(key: K, newValue: V): boolean

对HashMap中一组数据进行更新（替换）。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| key | K | 是 | 依据key指定替换的元素。 |
| newValue | V | 是 | 替换成员数据的值。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| boolean | 是否成功对已有数据进行替换 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The replace method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("sparrow", 123);
let result = hashMap.replace("sparrow", 357);
```


### forEach

forEach(callbackFn: (value?: V, key?: K, map?: HashMap<K, V>) => void, thisArg?: Object): void

通过回调函数来遍历HashMap实例对象上的元素以及元素对应的下标。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| callbackFn | function | 是 | 回调函数。 |
| thisArg | Object | 否 | callbackfn被调用时用作this值，默认值为当前实例对象。 |

callbackfn的参数说明：
| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| value | V | 否 | 当前遍历到的元素键值对的值，默认值为首个键值对的值。 |
| key | K | 否 | 当前遍历到的元素键值对的键，默认值为首个键值对的键。 |
| map | HashMap<K, V> | 否 | 当前调用forEach方法的实例对象，默认值为当前实例对象。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The forEach method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("sparrow", 123);
hashMap.set("gull", 357);
hashMap.forEach((value, key) => {
    console.log("value:" + value, "key:" + key);
});
```


### entries

entries(): IterableIterator&lt;[K, V]&gt;

返回包含此映射中包含的键值对的新迭代器对象。

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| IterableIterator&lt;[K, V]&gt; | 返回一个迭代器。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The entries method cannot be bound. |

**示例：**

```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);
let iter = hashMap.entries();
let temp = iter.next().value;
while(temp != undefined) {
  console.log("key:" + temp[0]);
  console.log("value:" + temp[1]);
  temp = iter.next().value;
}
```


### [Symbol.iterator]

[Symbol.iterator]\(): IterableIterator&lt;[K, V]&gt;

返回一个迭代器，迭代器的每一项都是一个 JavaScript 对象,并返回该对象。

**系统能力：** SystemCapability.Utils.Lang

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| IterableIterator&lt;[K, V]&gt; | 返回一个迭代器。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200011 | The Symbol.iterator method cannot be bound. |

**示例：**
```ts
let hashMap = new HashMap();
hashMap.set("squirrel", 123);
hashMap.set("sparrow", 356);

// 使用方法一：
for (let item of hashMap) { 
  console.log("key:" + item[0]);
  console.log("value:" + item[1]);
}

// 使用方法二：
let iter = hashMap[Symbol.iterator]();
let temp = iter.next().value;
while(temp != undefined) {
  console.log("key:" + temp[0]);
  console.log("value:" + temp[1]);
  temp = iter.next().value;
}
```