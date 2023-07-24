# @ohos.util.ArrayList (Linear Container ArrayList)

**ArrayList** is a linear data structure that is implemented based on arrays. **ArrayList** can dynamically adjust the capacity based on project requirements. It increases the capacity by 50% each time.

When compared with **[LinkedList](js-apis-linkedlist.md)**, **ArrayList** is more efficient in random access but less efficient in the addition or removal operation, because its addition or removal operation affects the position of other elements in the container.

**Recommended use case**: Use **ArrayList** when elements in a container need to be frequently read.

This topic uses the following to identify the use of generics:
- T: Type

> **NOTE**
>
> The initial APIs of this module are supported since API version 8. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import

```ts
import ArrayList from '@ohos.util.ArrayList';
```

## ArrayList

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| length | number | Yes| No| Number of elements in an array list (called container later).|


### constructor

constructor()

A constructor used to create an **ArrayList** instance.

**System capability**: SystemCapability.Utils.Lang

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200012 | The ArrayList's constructor cannot be directly invoked. |

**Example**

```ts
let arrayList = new ArrayList();
```


### add

add(element: T): boolean

Adds an element at the end of this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| element | T | Yes| Target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the element is added successfully; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The add method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
let result = arrayList.add("a");
let result1 = arrayList.add(1);
let b = [1, 2, 3];
let result2 = arrayList.add(b);
let c = {name: "Dylon", age: "13"};
let result3 = arrayList.add(c);
let result4 = arrayList.add(false);
```

### insert

insert(element: T, index: number): void

Inserts an element at the specified position in this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| element | T | Yes| Target element.|
| index | number | Yes| Index of the position where the element is to be inserted.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The insert method cannot be bound. |
| 10200001 | The value of index is out of range. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.insert("A", 0);
arrayList.insert(0, 1);
arrayList.insert(true, 2);
```

### has

has(element: T): boolean

Checks whether this container has the specified element.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| element | T | Yes| Target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the specified element is contained; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The has method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add("squirrel");
let result = arrayList.has("squirrel");
```

### getIndexOf

getIndexOf(element: T): number

Obtains the index of the first occurrence of the specified element in this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| element | T | Yes| Target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Returns the position index if obtained; returns **-1** if the specified element is not found.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The getIndexOf method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(2);
arrayList.add(1);
arrayList.add(2);
arrayList.add(4);
let result = arrayList.getIndexOf(2);
```

### getLastIndexOf

getLastIndexOf(element: T): number

Obtains the index of the last occurrence of the specified element in this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| element | T | Yes| Target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Returns the position index if obtained; returns **-1** if the specified element is not found.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The getLastIndexOf method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(2);
arrayList.add(1);
arrayList.add(2);
arrayList.add(4);
let result = arrayList.getLastIndexOf(2);
```

### removeByIndex

removeByIndex(index: number): T

Removes an element with the specified position from this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| index | number | Yes| Position index of the target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| T | Element removed.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The removeByIndex method cannot be bound. |
| 10200001 | The value of index is out of range. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(2);
arrayList.add(4);
let result = arrayList.removeByIndex(2);
```

### remove

remove(element: T): boolean

Removes the first occurrence of the specified element from this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| element | T | Yes| Target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the element is removed successfully; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The remove method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
let result = arrayList.remove(2);
```

### removeByRange

removeByRange(fromIndex: number, toIndex: number): void

Removes from this container all of the elements within a range, including the element at the start position but not that at the end position.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| fromIndex | number | Yes| Index of the start position.|
| toIndex | number | Yes| Index of the end position.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The removeByRange method cannot be bound. |
| 10200001 | The value of fromIndex or toIndex is out of range. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
arrayList.removeByRange(2, 4);
```

### replaceAllElements

replaceAllElements(callbackFn: (value: T, index?: number, arrlist?: ArrayList&lt;T&gt;) => T,
thisArg?: Object): void

Replaces all elements in this container with new elements, and returns the new ones.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callbackFn | function | Yes| Callback invoked for the replacement.|
| thisArg | Object | No| Value of **this** to use when **callbackFn** is invoked. The default value is this instance.|

callbackFn

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | T | Yes| Value of the element that is currently traversed.|
| index | number | No| Position index of the element that is currently traversed. The default value is **0**.|
| arrlist | ArrayList&lt;T&gt; | No| Instance that calls the **replaceAllElements** API. The default value is this instance.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The replaceAllElements method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
arrayList.replaceAllElements((value) => {
    // Add the user operation logic based on the actual scenario.
    return value;
});
```

### forEach

forEach(callbackFn: (value: T, index?: number, arrlist?: ArrayList&lt;T&gt;) => void,
thisArg?: Object): void

Uses a callback to traverse the elements in this container and obtain their position indexes.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callbackFn | function | Yes| Callback invoked for the replacement.|
| thisArg | Object | No| Value of **this** to use when **callbackFn** is invoked. The default value is this instance.|

callbackFn

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | T | Yes| Value of the element that is currently traversed.|
| index | number | No| Position index of the element that is currently traversed. The default value is 0.|
| arrlist | ArrayList&lt;T&gt; | No| Instance that calls the **forEach** API. The default value is this instance.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The forEach method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
arrayList.forEach((value, index) => {
    console.log("value:" + value, "index:" + index);
});
```

### sort

sort(comparator?: (firstValue: T, secondValue: T) => number): void

Sorts elements in this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| comparator | function | No| Callback invoked for sorting. The default value is the callback function for sorting elements in ascending order.|

comparator

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| firstValue | T | Yes| Previous element.|
| secondValue | T | Yes| Next element.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The sort method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
arrayList.sort((a: number, b: number) => a - b);
arrayList.sort((a: number, b: number) => b - a);
arrayList.sort();
```

### subArrayList

subArrayList(fromIndex: number, toIndex: number): ArrayList&lt;T&gt;

Obtains elements within a range in this container, including the element at the start position but not that at the end position, and returns these elements as a new **ArrayList** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| fromIndex | number | Yes| Index of the start position.|
| toIndex | number | Yes| Index of the end position.|

**Return value**

| Type| Description|
| -------- | -------- |
| ArrayList&lt;T&gt; | New **ArrayList** instance obtained.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The subArrayList method cannot be bound. |
| 10200001 | The value of fromIndex or toIndex is out of range. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
let result = arrayList.subArrayList(2, 4);
```

### clear

clear(): void

Clears this container and sets its length to **0**.

**System capability**: SystemCapability.Utils.Lang

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The clear method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
arrayList.clear();
```

### clone

clone(): ArrayList&lt;T&gt; 

Clones this container and returns a copy. The modification to the copy does not affect the original instance.

**System capability**: SystemCapability.Utils.Lang


**Return value**

| Type| Description|
| -------- | -------- |
| ArrayList&lt;T&gt; | New **ArrayList** instance obtained.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The clone method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
let result = arrayList.clone();
```

### getCapacity

getCapacity(): number

Obtains the capacity of this container.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number | Capacity obtained.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The getCapacity method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
let result = arrayList.getCapacity();
```

### convertToArray

convertToArray(): Array&lt;T&gt;

Converts this container into an array.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| Array&lt;T&gt; | Array obtained.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The convertToArray method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
let result = arrayList.convertToArray();
```

### isEmpty

isEmpty(): boolean

Checks whether this container is empty (contains no element).

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the container is empty; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The isEmpty method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
let result = arrayList.isEmpty();
```

### increaseCapacityTo

increaseCapacityTo(newCapacity: number): void

Increases the capacity of this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| newCapacity | number | Yes| New capacity.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The increaseCapacityTo method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
arrayList.increaseCapacityTo(2);
arrayList.increaseCapacityTo(8);
```

### trimToCurrentLength

trimToCurrentLength(): void

Trims the capacity of this container to its current length.

**System capability**: SystemCapability.Utils.Lang

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The trimToCurrentLength method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);
arrayList.trimToCurrentLength();
```

### [Symbol.iterator]

[Symbol.iterator]\(): IterableIterator&lt;T&gt;

Obtains an iterator, each item of which is a JavaScript object.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| IterableIterator&lt;T&gt; | Iterator obtained.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The Symbol.iterator method cannot be bound. |

**Example**

```ts
let arrayList = new ArrayList();
arrayList.add(2);
arrayList.add(4);
arrayList.add(5);
arrayList.add(4);

// Method 1:
for (let item of arrayList) { 
    console.log(`value:${item}`); 
} 

// Method 2:
let iter = arrayList[Symbol.iterator]();
let temp = iter.next().value;
while(temp != undefined) {
    console.log(`value:${temp}`);
    temp = iter.next().value;
}
```
