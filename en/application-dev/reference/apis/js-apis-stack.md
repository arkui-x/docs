# @ohos.util.Stack (Linear Container Stack)

**Stack** is implemented based on the array data structure. It follows the principle Last Out First In (LOFI) and supports data insertion and removal at one end.

Unlike **[Queue](js-apis-queue.md)**, which is implemented based on the queue data structure and supports insertion at one end and removal at the other end, **Stack** supports insertion and removal at the same end.

**Recommended use case**: Use **Stack** in LOFI scenarios.

This topic uses the following to identify the use of generics:
- T: Type

> **NOTE**
>
> The initial APIs of this module are supported since API version 8. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import

```ts
import Stack from '@ohos.util.Stack';  
```

## Stack

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| length | number | Yes| No| Number of elements in a stack (called container later).|


### constructor

constructor()

A constructor used to create a **Stack** instance.

**System capability**: SystemCapability.Utils.Lang

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200012 | The Stack's constructor cannot be directly invoked. |

**Example**

```ts
let stack = new Stack();
```


### push

push(item: T): T

Adds an element at the top of this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| item | T | Yes| Target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| T | Element added.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The push method cannot be bound. |

**Example**

```ts
let stack = new Stack();
let result = stack.push("a");
let result1 = stack.push(1);
let b = [1, 2, 3];
let result2 = stack.push(b);
let c = {name : "Dylon", age : "13"};
let result3 = stack.push(c);
```

### pop

pop(): T

Removes the top element from this container.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| T | Element removed.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The pop method cannot be bound. |

**Example**

```ts
let stack = new Stack();
stack.push(2);
stack.push(4);
stack.push(5);
stack.push(2);
stack.push(4);
let result = stack.pop();
```

### peek

peek(): T

Obtains the top element of this container.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| T | Element obtained.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The peek method cannot be bound. |

**Example**

```ts
let stack = new Stack();
stack.push(2);
stack.push(4);
stack.push(5);
stack.push(2);
let result = stack.peek();
```

### locate

locate(element: T): number

Obtains the index of the first occurrence of the specified element in this container.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| element | T | Yes| Target element.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Returns the position index if obtained; returns **-1** otherwise.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The locate method cannot be bound. |

**Example**

```ts
let stack = new Stack();
stack.push(2);
stack.push(4);
stack.push(5);
stack.push(2);
let result = stack.locate(2);
```

### forEach

forEach(callbackFn: (value: T, index?: number, stack?: Stack&lt;T&gt;) => void,
thisArg?: Object): void

Uses a callback to traverse the elements in this container and obtain their position indexes.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callbackFn | function | Yes| Callback invoked to traverse the elements in the container.|
| thisArg | Object | No| Value of **this** to use when **callbackFn** is invoked. The default value is this instance.|

callbackFn

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | T | Yes| Value of the element that is currently traversed.|
| index | number | No| Position index of the element that is currently traversed. The default value is **0**.|
| stack | Stack&lt;T&gt; | No| Instance that calls the **forEach** API. The default value is this instance.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200011 | The forEach method cannot be bound. |

**Example**

```ts
let stack = new Stack();
stack.push(2);
stack.push(4);
stack.push(5);
stack.push(4);
stack.forEach((value, index) => {
    console.log("value:" + value, "index:" + index);
});
```

### isEmpty

isEmpty(): boolean

Checks whether this container is empty (contains no elements).

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
let stack = new Stack();
stack.push(2);
stack.push(4);
stack.push(5);
stack.push(4);
let result = stack.isEmpty();
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
let stack = new Stack();
stack.push(2);
stack.push(4);
stack.push(5);
stack.push(4);

// Method 1:
for (let item of stack) { 
  console.log("value:" + item); 
}

// Method 2:
let iter = stack[Symbol.iterator]();
let temp = iter.next().value;
while(temp != undefined) {
  console.log("value:" + temp);
  temp = iter.next().value;
}
```
