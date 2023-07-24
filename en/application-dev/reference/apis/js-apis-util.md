# @ohos.util (util)

The **util** module provides common utility functions, such as [TextEncoder](#textencoder) and [TextDecoder](#textdecoder) for string encoding and decoding, [RationalNumber<sup>8+</sup>](#rationalnumber8) for rational number operations, [LRUCache<sup>9+</sup>](#lrucache9) for cache management, [ScopeHelper<sup>9+</sup>](#scopehelper9) for range determination, [Base64Helper<sup>9+</sup>](#base64helper9) for Base64 encoding and decoding, and [types<sup>8+</sup>](#types8) for built-in object type check.

> **NOTE**
>
> The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import

```js
import util from '@ohos.util';
```

## util.format<sup>9+</sup>

format(format: string,  ...args: Object[]): string

Formats the specified values and inserts them into the string by replacing the wildcard in the string.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name | Type    | Mandatory| Description          |
| ------- | -------- | ---- | -------------- |
| format  | string   | Yes  | String.|
| ...args | Object[] | No  | Values to format. The formatted values will replace the wildcard in the string. If this parameter is not set, the first parameter is returned by default.|

**Return value**

| Type  | Description                        |
| ------ | ---------------------------- |
| string | String containing the formatted values.|

**Example**

  ```js
let res = util.format("%s", "hello world!");
console.log(res);
  ```

## util.errnoToString<sup>9+</sup>

errnoToString(errno: number): string

Obtains detailed information about a system error code.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type  | Mandatory| Description                      |
| ------ | ------ | ---- | -------------------------- |
| errno  | number | Yes  | Error code generated.|

**Return value**

| Type  | Description                  |
| ------ | ---------------------- |
| string | Detailed information about the error code.|

**Example**

```js
let errnum = -1; // -1 is a system error code.
let result = util.errnoToString(errnum);
console.log("result = " + result);
```

**Some error code and message examples**

| Error Code| Message                             |
| ------ | -------------------------------- |
| -1     | operation not permitted          |
| -2     | no such file or directory        |
| -3     | no such process                  |
| -4     | interrupted system call          |
| -5     | i/o error                        |
| -11    | resource temporarily unavailable |
| -12    | not enough memory                |
| -13    | permission denied                |
| -100   | network is down                  |

## util.callbackWrapper

callbackWrapper(original: Function): (err: Object, value: Object )=&gt;void

Calls back an asynchronous function. In the callback, the first parameter indicates the cause of the rejection (the value is **null** if the promise has been resolved), and the second parameter indicates the resolved value.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| original | Function | Yes| Asynchronous function.|

**Return value**

| Type| Description|
| -------- | -------- |
| Function | Callback, in which the first parameter indicates the cause of the rejection (the value is **null** if the promise has been resolved) and the second parameter indicates the resolved value.|

**Example**

  ```js
async function fn() {
   return 'hello world';
}
let cb = util.callbackWrapper(fn);
cb(1, (err, ret) => {
   if (err) throw err;
   console.log(ret);
});
  ```

## util.promisify<sup>9+</sup>

promisify(original: (err: Object, value: Object) =&gt; void): Function

Processes an asynchronous function and returns a promise.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| original | Function | Yes| Asynchronous function.|

**Return value**

| Type| Description|
| -------- | -------- |
| Function | Function in the error-first style (that is, **(err, value) =>...** is called as the last parameter) and the promise.|

**Example**

  ```js
function fun(num, callback) {
   if (typeof num === 'number') {
      callback(null, num + 3);
   } else {
      callback("type err");
   }
}

const addCall = util.promisify(fun);
(async () => {
   try {
      let res = await addCall(2);
      console.log(res);
   } catch (err) {
      console.log(err);
   }
})();
  ```

## util.generateRandomUUID<sup>9+</sup>

generateRandomUUID(entropyCache?: boolean): string

Uses a secure random number generator to generate a random universally unique identifier (UUID) of the string type in RFC 4122 version 4.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| entropyCache | boolean | No| Whether a cached UUID can be used. The default value is **true**.|

**Return value**

| Type| Description|
| -------- | -------- |
| string | A string representing the UUID generated.|

**Example**

  ```js
  let uuid = util.generateRandomUUID(true);
  console.log("RFC 4122 Version 4 UUID:" + uuid);
  // Output:
  // RFC 4122 Version 4 UUID:88368f2a-d5db-47d8-a05f-534fab0a0045
  ```

## util.generateRandomBinaryUUID<sup>9+</sup>

generateRandomBinaryUUID(entropyCache?: boolean): Uint8Array

Uses a secure random number generator to generate a random UUID of the Uint8Array type in RFC 4122 version 4.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| entropyCache | boolean | No| Whether a cached UUID can be used. The default value is **true**.|

**Return value**

| Type| Description|
| -------- | -------- |
| Uint8Array | A Uint8Array value representing the UUID generated.|

**Example**

  ```js
  let uuid = util.generateRandomBinaryUUID(true);
  console.log(JSON.stringify(uuid));
  // Output:
  // 138,188,43,243,62,254,70,119,130,20,235,222,199,164,140,150
  ```

## util.parseUUID<sup>9+</sup>

parseUUID(uuid: string): Uint8Array

Converts the UUID of the string type generated by **generateRandomUUID** to the UUID of the **Uint8Array** type generated by **generateRandomBinaryUUID**, as described in RFC 4122 version 4.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| uuid | string | Yes| A string representing the UUID.|

**Return value**

| Type| Description|
| -------- | -------- |
| Uint8Array | A Uint8Array value representing the UUID parsed. If the parsing fails, **SyntaxError** is thrown.|

**Example**

  ```js
  let uuid = util.parseUUID("84bdf796-66cc-4655-9b89-d6218d100f9c");
  console.log(JSON.stringify(uuid));
  // Output:
  // 132,189,247,150,102,204,70,85,155,137,214,33,141,16,15,156
  ```


## TextDecoder

Provides APIs to decode byte arrays into strings. It supports multiple formats, including UTF-8, UTF-16LE, UTF-16BE, ISO-8859, and Windows-1251.

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| encoding | string | Yes| No| Encoding format.<br>- Supported formats: utf-8, ibm866, iso-8859-2, iso-8859-3, iso-8859-4, iso-8859-5, iso-8859-6, iso-8859-7, iso-8859-8, iso-8859-8-i, iso-8859-10, iso-8859-13, iso-8859-14, iso-8859-15, koi8-r, koi8-u, macintosh, windows-874, windows-1250, windows-1251, windows-1252, windows-1253, windows-1254, windows-1255, windows-1256, windows-1257, windows-1258, x-mac-cyrilli, gbk, gb18030, big5, euc-jp, iso-2022-jp, shift_jis, euc-kr, utf-16be, utf-16le|
| fatal | boolean | Yes| No| Whether to display fatal errors.|
| ignoreBOM | boolean | Yes| No| Whether to ignore the byte order marker (BOM). The default value is **false**, which indicates that the result contains the BOM.|

### constructor<sup>9+</sup>

constructor()

A constructor used to create a **TextDecoder** object.

**System capability**: SystemCapability.Utils.Lang

### create<sup>9+</sup>

create(encoding?: string,options?: { fatal?: boolean; ignoreBOM?: boolean }): TextDecoder;

Creates a **TextDecoder** object. It provides the same function as the deprecated argument constructor.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name  | Type  | Mandatory| Description                                            |
| -------- | ------ | ---- | ------------------------------------------------ |
| encoding | string | No  | Encoding format. The default format is **'utf-8'**.                     |
| options  | Object | No  | Encoding-related options, which include **fatal** and **ignoreBOM**.|

**Table 1.1** options

| Name     | Type| Mandatory| Description              |
| --------- | -------- | ---- | ------------------ |
| fatal     | boolean  | No  | Whether to display fatal errors. The default value is **false**.|
| ignoreBOM | boolean  | No  | Whether to ignore the BOM. The default value is **false**. |

**Example**

```js
let result = util.TextDecoder.create('utf-8', { ignoreBOM : true })
let retStr = result.encoding
```

### decodeWithStream<sup>9+</sup>

decodeWithStream(input: Uint8Array, options?: { stream?: boolean }): string

Decodes the input content.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| input | Uint8Array | Yes| Uint8Array to decode.|
| options | Object | No| Options related to decoding.|

**Table 2** options

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| stream | boolean | No| Whether to allow data blocks in subsequent **decodeWithStream()**. If data is processed in blocks, set this parameter to **true**. If this is the last data block to process or data is not divided into blocks, set this parameter to **false**. The default value is **false**.|

**Return value**

| Type| Description|
| -------- | -------- |
| string | Data decoded.|

**Example**

  ```js
  let textDecoder = new util.TextDecoder("utf-8",{ignoreBOM: true});
  let result = new Uint8Array(6);
  result[0] = 0xEF;
  result[1] = 0xBB;
  result[2] = 0xBF;
  result[3] = 0x61;
  result[4] = 0x62;
  result[5] = 0x63;
  console.log("input num:");
  let retStr = textDecoder.decodeWithStream( result , {stream: false});
  console.log("retStr = " + retStr);
  ```


## TextEncoder

Provides APIs to encode strings into byte arrays. It supports multiple formats, including UTF-8, UTF-16LE, and UTF-16BE. When **TextEncoder** is used for encoding, the number of bytes occupied by a character varies according to the encoding format. For example, a Chinese character usually occupies three bytes in UTF-8 encoding format but two bytes in UTF-16LE or UTF-16BE encoding format. Therefore, when using **TextEncoder**, you must explicitly specify the encoding format to obtain the required encoding result.

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| encoding | string | Yes| No| Encoding format. The default format is **'utf-8'**.|


### constructor

constructor()

A constructor used to create a **TextEncoder** object.

**System capability**: SystemCapability.Utils.Lang

**Example**

  ```js
  let textEncoder = new util.TextEncoder();
  ```

### constructor<sup>9+</sup>

constructor(encoding?: string)

A constructor used to create a **TextEncoder** object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| ----- | ---- | ---- | ---- |
| encoding | string | No| Encoding format. The default format is **'utf-8'**.|

**Example**

  ```js
  let textEncoder = new util.TextEncoder("utf-8");
  ```

### encodeInto<sup>9+</sup>

encodeInto(input?: string): Uint8Array

Encodes the input content.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| input  | string | No  | String to encode. The default value is an empty string.|

**Return value**

| Type      | Description              |
| ---------- | ------------------ |
| Uint8Array | Encoded text.|

**Example**

  ```js
let textEncoder = new util.TextEncoder();
let buffer = new ArrayBuffer(20);
let result = new Uint8Array(buffer);
result = textEncoder.encodeInto("\uD800¥¥");
  ```

### encodeIntoUint8Array<sup>9+</sup>

encodeIntoUint8Array(input: string, dest: Uint8Array): { read: number; written: number }

Stores the UTF-8 encoded text.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type      | Mandatory| Description                                                   |
| ------ | ---------- | ---- | ------------------------------------------------------- |
| input  | string     | Yes  | String to encode.                                     |
| dest   | Uint8Array | Yes  | **Uint8Array** instance used to store the UTF-8 encoded text.|

**Return value**

| Type      | Description              |
| ---------- | ------------------ |
| Uint8Array | Encoded text.|

**Example**

  ```js
let that = new util.TextEncoder()
let buffer = new ArrayBuffer(4)
let dest = new Uint8Array(buffer)
let result = new Object()
result = that.encodeIntoUint8Array('abcd', dest)
  ```


## RationalNumber<sup>8+</sup>

Provides APIs to compare rational numbers and obtain numerators and denominators. For example, the **toString()** API can be used to convert a rational number into a strings.

### constructor<sup>9+</sup>

constructor()

A constructor used to create a **RationalNumber** object.

**System capability**: SystemCapability.Utils.Lang

**Example**

```js
let rationalNumber = new util.RationalNumber();
```

### parseRationalNumber<sup>9+</sup>

parseRationalNumber(numerator: number,denominator: number): RationalNumber

Parses a rational number. Previously, this processing is an internal action of the deprecated constructor.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name     | Type  | Mandatory| Description            |
| ----------- | ------ | ---- | ---------------- |
| numerator   | number | Yes  | Numerator, which is an integer.|
| denominator | number | Yes  | Denominator, which is an integer.|

**Example**

```js
let rationalNumber = util.RationalNumber.parseRationalNumber(1,2)
```

### createRationalFromString<sup>8+</sup>

static createRationalFromString​(rationalString: string): RationalNumber​

Creates a **RationalNumber** object based on the given string.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| rationalString | string | Yes| String used to create the **RationalNumber** object.|

**Return value**

| Type| Description|
| -------- | -------- |
| object | **RationalNumber** object created.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let rational = util.RationalNumber.createRationalFromString("3/4");
```

### compare<sup>9+</sup>

compare​(another: RationalNumber): number​

Compares this **RationalNumber** object with a given object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name | Type          | Mandatory| Description              |
| ------- | -------------- | ---- | ------------------ |
| another | RationalNumber | Yes  | Object used to compare with this **RationalNumber** object.|

**Return value**

| Type  | Description                                                        |
| ------ | ------------------------------------------------------------ |
| number | Returns **0** if the two objects are equal; returns **1** if the given object is less than this object; return **-1** if the given object is greater than this object.|

**Example**

  ```js
let rationalNumber = new util.RationalNumber(1,2);
let rational = util.RationalNumber.createRationalFromString("3/4");
let result = rationalNumber.compare(rational);
  ```

### valueOf<sup>8+</sup>

valueOf(): number

Obtains the value of this **RationalNumber** object as an integer or a floating-point number.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number | An integer or a floating-point number.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = rationalNumber.valueOf();
```

### equals<sup>8+</sup>

equals​(obj: Object): boolean

Checks whether this **RationalNumber** object equals the given object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| object | Object | Yes| Object used to compare with this **RationalNumber** object.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the two objects are equal; returns **false** otherwise.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let rational = util.RationalNumber.createRationalFromString("3/4");
let result = rationalNumber.equals(rational);
```

### getCommonFactor<sup>9+</sup>

getCommonFactor(number1: number,number2: number): number

Obtains the greatest common divisor of two specified integers.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name | Type  | Mandatory| Description      |
| ------- | ------ | ---- | ---------- |
| number1 | number | Yes  | The first integer used to get the greatest common divisor.|
| number2 | number | Yes  | The second integer used to get the greatest common divisor.|

**Return value**

| Type  | Description                          |
| ------ | ------------------------------ |
| number | Greatest common divisor obtained.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = util.RationalNumber.getCommonFactor(4,6);
```

### getNumerator<sup>8+</sup>

getNumerator​(): number

Obtains the numerator of this **RationalNumber** object.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number | Numerator of this **RationalNumber** object.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = rationalNumber.getNumerator();
```

### getDenominator<sup>8+</sup>

getDenominator​(): number

Obtains the denominator of this **RationalNumber** object.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number | Denominator of this **RationalNumber** object.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = rationalNumber.getDenominator();
```

### isZero<sup>8+</sup>

isZero​():boolean

Checks whether this **RationalNumber** object is **0**.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the value of this **RationalNumber** object is **0**; returns **false** otherwise.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = rationalNumber.isZero();
```

### isNaN<sup>8+</sup>

isNaN​(): boolean

Checks whether this **RationalNumber** object is a Not a Number (NaN).

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if this **RationalNumber** object is a NaN (the denominator and numerator are both **0**); returns **false** otherwise.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = rationalNumber.isNaN();
```

### isFinite<sup>8+</sup>

isFinite​():boolean

Checks whether this **RationalNumber** object represents a finite value.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if this **RationalNumber** object represents a finite value (the denominator is not **0**); returns **false** otherwise.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = rationalNumber.isFinite();
```

### toString<sup>8+</sup>

toString​(): string

Obtains the string representation of this **RationalNumber** object.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| string | Returns **NaN** if the numerator and denominator of this object are both **0**; returns a string in Numerator/Denominator format otherwise, for example, **3/5**.|

**Example**

```js
let rationalNumber = new util.RationalNumber(1,2);
let result = rationalNumber.toString();
```


## LRUCache<sup>9+</sup>

Provides APIs to discard the least recently used data to make rooms for new elements when the cache is full. This class uses the Least Recently Used (LRU) algorithm, which believes that the recently used data may be accessed again in the near future and the least accessed data is the least valuable data and should be removed from the cache.

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name  | Type  | Readable| Writable| Description                  |
| ------ | ------ | ---- | ---- | ---------------------- |
| length | number | Yes  | No  | Total number of values in this cache.|

**Example**

```js
let pro = new util.LRUCache();
pro.put(2,10);
pro.put(1,8);
let result = pro.length;
```

### constructor<sup>9+</sup>

constructor(capacity?: number)

A constructor used to create a **LruCache** instance. The default capacity of the cache is 64.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name  | Type  | Mandatory| Description                        |
| -------- | ------ | ---- | ---------------------------- |
| capacity | number | No  | Capacity of the **LruCache** to create. The default value is **64**.|

**Example**

```js
let lrubuffer= new util.LRUCache();
```


### updateCapacity<sup>9+</sup>

updateCapacity(newCapacity: number): void

Changes the **LruCache** capacity. If the new capacity is less than or equal to **0**, an exception will be thrown.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name     | Type  | Mandatory| Description                        |
| ----------- | ------ | ---- | ---------------------------- |
| newCapacity | number | Yes  | New capacity of the **LruCache**.|

**Example**

```js
let pro = new util.LRUCache();
pro.updateCapacity(100);
```


### toString<sup>9+</sup>

toString(): string

Obtains the string representation of this **LruCache** object.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                      |
| ------ | -------------------------- |
| string | String representation of this **LruCache** object.|

**Example**

```js
let pro = new util.LRUCache();
pro.put(2,10);
pro.get(2);
pro.remove(20);
let result = pro.toString();
```


### getCapacity<sup>9+</sup>

getCapacity(): number

Obtains the capacity of this cache.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                  |
| ------ | ---------------------- |
| number | Capacity of this cache.|

**Example**

  ```js
let pro = new util.LRUCache();
let result = pro.getCapacity();
  ```


### clear<sup>9+</sup>

clear(): void

Clears key-value pairs from this cache. The **afterRemoval()** method will be called to perform subsequent operations.

**System capability**: SystemCapability.Utils.Lang

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result = pro.length;
pro.clear();
  ```


### getCreateCount<sup>9+</sup>

getCreateCount(): number

Obtains the number of return values for **createDefault()**.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                             |
| ------ | --------------------------------- |
| number | Number of return values for **createDefault()**.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(1,8);
let result = pro.getCreateCount();
  ```


### getMissCount<sup>9+</sup>

getMissCount(): number

Obtains the number of times that the queried values are mismatched.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                    |
| ------ | ------------------------ |
| number | Number of times that the queried values are mismatched.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
pro.get(2);
let result = pro.getMissCount();
  ```


### getRemovalCount<sup>9+</sup>

getRemovalCount(): number

Obtains the number of removals from this cache.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                      |
| ------ | -------------------------- |
| number | Number of removals from the cache.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
pro.updateCapacity(2);
pro.put(50,22);
let result = pro.getRemovalCount();
  ```


### getMatchCount<sup>9+</sup>

getMatchCount(): number

Obtains the number of times that the queried values are matched.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                      |
| ------ | -------------------------- |
| number | Number of times that the queried values are matched.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
pro.get(2);
let result = pro.getMatchCount();
  ```


### getPutCount<sup>9+</sup>

getPutCount(): number

Obtains the number of additions to this cache.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                        |
| ------ | ---------------------------- |
| number | Number of additions to the cache.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result = pro.getPutCount();
  ```


### isEmpty<sup>9+</sup>

isEmpty(): boolean

Checks whether this cache is empty.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type   | Description                                    |
| ------- | ---------------------------------------- |
| boolean | Returns **true** if the cache does not contain any value.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result = pro.isEmpty();
  ```


### get<sup>9+</sup>

get(key: K): V | undefined

Obtains the value of the specified key.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description        |
| ------ | ---- | ---- | ------------ |
| key    | K    | Yes  | Key based on which the value is queried.|

**Return value**

| Type                    | Description                                                        |
| ------------------------ | ------------------------------------------------------------ |
| V \| undefined | Returns the value of the key if a match is found in the cache; returns **undefined** otherwise.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result  = pro.get(2);
  ```


### put<sup>9+</sup>

put(key: K,value: V): V

Adds a key-value pair to this cache.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description                      |
| ------ | ---- | ---- | -------------------------- |
| key    | K    | Yes  | Key of the key-value pair to add.            |
| value  | V    | Yes  | Value of the key-value pair to add.|

**Return value**

| Type| Description                                                        |
| ---- | ------------------------------------------------------------ |
| V    | Returns the existing value if the key already exists; returns the value added otherwise. If the key or value is null, an exception will be thrown. |

**Example**

  ```js
let pro = new util.LRUCache();
let result = pro.put(2,10);
  ```

### values<sup>9+</sup>

values(): V[]

Obtains all values in this cache, listed from the most to the least recently accessed.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type     | Description                                                        |
| --------- | ------------------------------------------------------------ |
| V&nbsp;[] | All values in the cache, listed from the most to the least recently accessed.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
pro.put(2,"anhu");
pro.put("afaf","grfb");
let result = pro.values();
  ```


### keys<sup>9+</sup>

keys(): K[]

Obtains all keys in this cache, listed from the most to the least recently accessed.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type     | Description                                                        |
| --------- | ------------------------------------------------------------ |
| K&nbsp;[] | All keys in the cache, listed from the most to the least recently accessed.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result = pro.keys();
  ```


### remove<sup>9+</sup>

remove(key: K): V | undefined

Removes the specified key and its value from this cache.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description          |
| ------ | ---- | ---- | -------------- |
| key    | K    | Yes  | Key to remove.|

**Return value**

| Type                    | Description                                                        |
| ------------------------ | ------------------------------------------------------------ |
| V&nbsp;\|&nbsp;undefined | Returns an **Optional** object containing the removed key-value pair if the key exists in the cache; returns an empty **Optional** object otherwise. If the key is null, an exception will be thrown.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result = pro.remove(20);
  ```


### afterRemoval<sup>9+</sup>

afterRemoval(isEvict: boolean,key: K,value: V,newValue: V): void

Performs subsequent operations after a value is removed.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name  | Type   | Mandatory| Description                                                        |
| -------- | ------- | ---- | ------------------------------------------------------------ |
| isEvict  | boolean | Yes  | Whether the cache capacity is insufficient. If the value is **true**, this API is called due to insufficient capacity.   |
| key      | K       | Yes  | Key removed.                                              |
| value    | V       | Yes  | Value removed.                                              |
| newValue | V       | Yes  | New value for the key if the **put()** method is called and the key to be added already exists. In other cases, this parameter is left blank.|

**Example**

  ```js
let arr = [];
class ChildLruBuffer<K, V> extends util.LRUCache<K, V>
{
	constructor()
	{
		super();
	}
	afterRemoval(isEvict, key, value, newValue)
	{
		if (isEvict === false)
		{
			arr = [key, value, newValue];
		}
	}
}
let lru = new ChildLruBuffer();
lru.afterRemoval(false,10,30,null);
  ```


### contains<sup>9+</sup>

contains(key: K): boolean

Checks whether this cache contains the specified key.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type  | Mandatory| Description            |
| ------ | ------ | ---- | ---------------- |
| key    | K | Yes  | Key to check.|

**Return value**

| Type   | Description                                      |
| ------- | ------------------------------------------ |
| boolean | Returns **true** if the cache contains the specified key; returns **false** otherwise.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let obj = {1:"key"};
let result = pro.contains(obj);
  ```


### createDefault<sup>9+</sup>

createDefault(key: K): V

Creates a value if the value of the specified key is not available.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description          |
| ------ | ---- | ---- | -------------- |
| key    | K    | Yes  | Key of which the value is missing.|

**Return value**

| Type| Description              |
| ---- | ------------------ |
| V    | Value of the key.|

**Example**

  ```js
let pro = new util.LRUCache();
let result = pro.createDefault(50);
  ```


### entries<sup>9+</sup>

entries(): IterableIterator&lt;[K,V]&gt;

Obtains a new iterator object that contains all key-value pairs in this object.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type       | Description                |
| ----------- | -------------------- |
| [K,&nbsp;V] | Iterable array.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result = pro.entries();
  ```

### [Symbol.iterator]<sup>9+</sup>

[Symbol.iterator]\(): IterableIterator&lt;[K, V]&gt;

Obtains a two-dimensional array in key-value pairs.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type       | Description                          |
| ----------- | ------------------------------ |
| [K,&nbsp;V] | Two-dimensional array in key-value pairs.|

**Example**

  ```js
let pro = new util.LRUCache();
pro.put(2,10);
let result = pro[Symbol.iterator]();
  ```

## ScopeComparable<sup>8+</sup>

The values of the **ScopeComparable** type are used to implement the **compareTo** method. Therefore, ensure that the input parameters are comparable.

**System capability**: SystemCapability.Utils.Lang

### compareTo<sup>8+</sup>

compareTo(other: ScopeComparable): boolean;

Compares two values and returns a Boolean value.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description          |
| ------ | ---- | ---- | -------------- |
| other  | [ScopeComparable](#scopecomparable8) | Yes | The other value to be compared with the current value.|

**Return value**

| Type| Description              |
| ---- | ------------------ |
| boolean | If the current value is greater than or equal to the input value, **true** is returned. Otherwise, **false** is returned.|

**Example**

Create a class to implement the **compareTo** method. The **Temperature** class is used as an example in the following sample code.

```js
class Temperature{
    // If ArkTS is used for development, add the following code:
    // private readonly _temp: Temperature;
    constructor(value) {
       this._temp = value;
    }
    compareTo(value) {
       return this._temp >= value.getTemp();
    }
    getTemp() {
       return this._temp;
    }
    toString() {
       return this._temp.toString();
    }
}
```

## ScopeType<sup>8+</sup>

Defines the type of values in a **Scope** object.

**System capability**: SystemCapability.Utils.Lang

| Type| Description|
| -------- | -------- |
| number | The value type is a number.|
| [ScopeComparable](#scopecomparable8) | The value type is ScopeComparable.|

## ScopeHelper<sup>9+</sup>

Provides APIs to define the valid range of a field. The constructor of this class creates comparable objects with lower and upper limits.

### constructor<sup>9+</sup>

constructor(lowerObj: ScopeType, upperObj: ScopeType)

A constructor used to create a **ScopeHelper** object with the specified upper and lower limits.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name  | Type                    | Mandatory| Description                  |
| -------- | ------------------------ | ---- | ---------------------- |
| lowerObj | [ScopeType](#scopetype8) | Yes  | Lower limit of the **Scope** object.|
| upperObj | [ScopeType](#scopetype8) | Yes  | Upper limit of the **Scope** object.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let range = new util.ScopeHelper(tempLower, tempUpper);
  ```


### toString<sup>9+</sup>

toString(): string

Obtains a string representation that contains this **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type  | Description                                  |
| ------ | -------------------------------------- |
| string | String representation containing the **Scope**.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.toString();
  ```


### intersect<sup>9+</sup>

intersect(range: ScopeHelper): ScopeHelper

Obtains the intersection of this **Scope** and the given **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                        | Mandatory| Description              |
| ------ | ---------------------------- | ---- | ------------------ |
| range  | [ScopeHelper](#scopehelper9) | Yes  | **Scope** specified.|

**Return value**

| Type                          | Description                          |
| ------------------------------ | ------------------------------ |
| [ScopeHelper9+](#scopehelper9) | Intersection of this **Scope** and the given **Scope**.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let range = new util.ScopeHelper(tempLower, tempUpper);
let tempMiDF = new Temperature(35);
let tempMidS = new Temperature(39);
let rangeFir = new util.ScopeHelper(tempMiDF, tempMidS);
range.intersect(rangeFir);
  ```


### intersect<sup>9+</sup>

intersect(lowerObj:ScopeType,upperObj:ScopeType):ScopeHelper

Obtains the intersection of this **Scope** and the given lower and upper limits.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name  | Type                    | Mandatory| Description            |
| -------- | ------------------------ | ---- | ---------------- |
| lowerObj | [ScopeType](#scopetype8) | Yes  | Lower limit.|
| upperObj | [ScopeType](#scopetype8) | Yes  | Upper limit.|

**Return value**

| Type                        | Description                                    |
| ---------------------------- | ---------------------------------------- |
| [ScopeHelper](#scopehelper9) | Intersection of this **Scope** and the given lower and upper limits.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let tempMiDF = new Temperature(35);
let tempMidS = new Temperature(39);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.intersect(tempMiDF, tempMidS);
  ```


### getUpper<sup>9+</sup>

getUpper(): ScopeType

Obtains the upper limit of this **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type                    | Description                  |
| ------------------------ | ---------------------- |
| [ScopeType](#scopetype8) | Upper limit of this **Scope**.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.getUpper();
  ```


### getLower<sup>9+</sup>

getLower(): ScopeType

Obtains the lower limit of this **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type                    | Description                  |
| ------------------------ | ---------------------- |
| [ScopeType](#scopetype8) | Lower limit of this **Scope**.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.getLower();
  ```


### expand<sup>9+</sup>

expand(lowerObj: ScopeType,upperObj: ScopeType): ScopeHelper

Obtains the union set of this **Scope** and the given lower and upper limits.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name  | Type                    | Mandatory| Description            |
| -------- | ------------------------ | ---- | ---------------- |
| lowerObj | [ScopeType](#scopetype8) | Yes  | Lower limit.|
| upperObj | [ScopeType](#scopetype8) | Yes  | Upper limit.|

**Return value**

| Type                        | Description                                |
| ---------------------------- | ------------------------------------ |
| [ScopeHelper](#scopehelper9) | Union set of this **Scope** and the given lower and upper limits.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let tempMiDF = new Temperature(35);
let tempMidS = new Temperature(39);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.expand(tempMiDF, tempMidS);
  ```


### expand<sup>9+</sup>

expand(range: ScopeHelper): ScopeHelper

Obtains the union set of this **Scope** and the given **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                        | Mandatory| Description              |
| ------ | ---------------------------- | ---- | ------------------ |
| range  | [ScopeHelper](#scopehelper9) | Yes  | **Scope** specified.|

**Return value**

| Type                        | Description                              |
| ---------------------------- | ---------------------------------- |
| [ScopeHelper](#scopehelper9) | Union set of this **Scope** and the given **Scope**.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let tempMiDF = new Temperature(35);
let tempMidS = new Temperature(39);
let range = new util.ScopeHelper(tempLower, tempUpper);
let rangeFir = new util.ScopeHelper(tempMiDF, tempMidS);
let result = range.expand(rangeFir);
  ```


### expand<sup>9+</sup>

expand(value: ScopeType): ScopeHelper

Obtains the union set of this **Scope** and the given value.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                    | Mandatory| Description            |
| ------ | ------------------------ | ---- | ---------------- |
| value  | [ScopeType](#scopetype8) | Yes  | Value specified.|

**Return value**

| Type                        | Description                            |
| ---------------------------- | -------------------------------- |
| [ScopeHelper](#scopehelper9) | Union set of this **Scope** and the given value.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let tempMiDF = new Temperature(35);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.expand(tempMiDF);
  ```


### contains<sup>9+</sup>

contains(value: ScopeType): boolean

Checks whether a value is within this **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                    | Mandatory| Description            |
| ------ | ------------------------ | ---- | ---------------- |
| value  | [ScopeType](#scopetype8) | Yes  | Value specified.|

**Return value**

| Type   | Description                                               |
| ------- | --------------------------------------------------- |
| boolean | Returns **true** if the value is within this **Scope**; returns **false** otherwise.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let tempMiDF = new Temperature(35);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.contains(tempMiDF);
  ```


### contains<sup>9+</sup>

contains(range: ScopeHelper): boolean

Checks whether a range is within this **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                        | Mandatory| Description              |
| ------ | ---------------------------- | ---- | ------------------ |
| range  | [ScopeHelper](#scopehelper9) | Yes  | **Scope** specified.|

**Return value**

| Type   | Description                                                 |
| ------- | ----------------------------------------------------- |
| boolean | Returns **true** if the range is within this **Scope**; returns **false** otherwise.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let range = new util.ScopeHelper(tempLower, tempUpper);
let tempLess = new Temperature(20);
let tempMore = new Temperature(45);
let rangeSec = new util.ScopeHelper(tempLess, tempMore);
let result = range.contains(rangeSec);
  ```


### clamp<sup>9+</sup>

clamp(value: ScopeType): ScopeType

Limits a value to this **Scope**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                    | Mandatory| Description          |
| ------ | ------------------------ | ---- | -------------- |
| value  | [ScopeType](#scopetype8) | Yes  | Value specified.|

**Return value**

| Type                    | Description                                                        |
| ------------------------ | ------------------------------------------------------------ |
| [ScopeType](#scopetype8) | Returns **lowerObj** if the specified value is less than the lower limit; returns **upperObj** if the specified value is greater than the upper limit; returns the specified value if it is within this **Scope**.|

**Example**

  ```js
let tempLower = new Temperature(30);
let tempUpper = new Temperature(40);
let tempMiDF = new Temperature(35);
let range = new util.ScopeHelper(tempLower, tempUpper);
let result = range.clamp(tempMiDF);
  ```

## Base64Helper<sup>9+</sup>

The Base64 encoding table contains 62 characters, which are the uppercase letters (A-Z), lowercase letters (a-z), digits (0-9), and the special characters plus sign (+) and slash (/). During encoding, the original data is divided into groups of three bytes, and each group contains a 6-bit number. Then, the corresponding characters in the Base64 encoding table are used to represent these numbers. If the last group contains only one or two bytes, the equal sign (=) is used for padding.

### constructor<sup>9+</sup>

constructor()

A constructor used to create a **Base64Helper** instance.

**System capability**: SystemCapability.Utils.Lang

**Example**

  ```js
let base64 = new  util.Base64Helper();
  ```

### encodeSync<sup>9+</sup>

encodeSync(src: Uint8Array): Uint8Array

Encodes the input content.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type      | Mandatory| Description               |
| ------ | ---------- | ---- | ------------------- |
| src    | Uint8Array | Yes  | Uint8Array to encode.|

**Return value**

| Type      | Description                         |
| ---------- | ----------------------------- |
| Uint8Array | Uint8Array encoded.|

**Example**

  ```js
let that = new util.Base64Helper();
let array = new Uint8Array([115,49,51]);
let result = that.encodeSync(array);
  ```


### encodeToStringSync<sup>9+</sup>

encodeToStringSync(src: Uint8Array): string

Encodes the input content.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type      | Mandatory| Description               |
| ------ | ---------- | ---- | ------------------- |
| src    | Uint8Array | Yes  | Uint8Array to encode.|

**Return value**

| Type  | Description                |
| ------ | -------------------- |
| string | String encoded from the Uint8Array.|

**Example**

  ```js
let that = new util.Base64Helper();
let array = new Uint8Array([115,49,51]);
let result = that.encodeToStringSync(array);
  ```


### decodeSync<sup>9+</sup>

decodeSync(src: Uint8Array | string): Uint8Array

Decodes the input content.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                          | Mandatory| Description                         |
| ------ | ------------------------------ | ---- | ----------------------------- |
| src    | Uint8Array&nbsp;\|&nbsp;string | Yes  | Uint8Array or string to decode.|

**Return value**

| Type      | Description                         |
| ---------- | ----------------------------- |
| Uint8Array | Uint8Array decoded.|

**Example**

  ```js
let that = new util.Base64Helper();
let buff = 'czEz';
let result = that.decodeSync(buff);
  ```


### encode<sup>9+</sup>

encode(src: Uint8Array): Promise&lt;Uint8Array&gt;

Encodes the input content asynchronously.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type      | Mandatory| Description                   |
| ------ | ---------- | ---- | ----------------------- |
| src    | Uint8Array | Yes  | Uint8Array to encode asynchronously.|

**Return value**

| Type                     | Description                             |
| ------------------------- | --------------------------------- |
| Promise&lt;Uint8Array&gt; | Uint8Array obtained after asynchronous encoding.|

**Example**

  ```js
let that = new util.Base64Helper();
let array = new Uint8Array([115,49,51]);
let rarray = new Uint8Array([99,122,69,122]);
that.encode(array).then(val=>{    
    for (var i = 0; i < rarray.length; i++) {        
        console.log(val[i].toString())
    }
})
  ```


### encodeToString<sup>9+</sup>

encodeToString(src: Uint8Array): Promise&lt;string&gt;

Encodes the input content asynchronously.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type      | Mandatory| Description                   |
| ------ | ---------- | ---- | ----------------------- |
| src    | Uint8Array | Yes  | Uint8Array to encode asynchronously.|

**Return value**

| Type                 | Description                    |
| --------------------- | ------------------------ |
| Promise&lt;string&gt; | String obtained after asynchronous encoding.|

**Example**

  ```js
let that = new util.Base64Helper();
let array = new Uint8Array([115,49,51]);
that.encodeToString(array).then(val=>{    
    console.log(val)
})
  ```


### decode<sup>9+</sup>

decode(src: Uint8Array | string): Promise&lt;Uint8Array&gt;

Decodes the input content asynchronously.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type                          | Mandatory| Description                             |
| ------ | ------------------------------ | ---- | --------------------------------- |
| src    | Uint8Array&nbsp;\|&nbsp;string | Yes  | Uint8Array or string to decode asynchronously.|

**Return value**

| Type                     | Description                             |
| ------------------------- | --------------------------------- |
| Promise&lt;Uint8Array&gt; | Uint8Array obtained after asynchronous decoding.|

**Example**

  ```js
let that = new util.Base64Helper();
let array = new Uint8Array([99,122,69,122]);
let rarray = new Uint8Array([115,49,51]);
that.decode(array).then(val=>{    
    for (var i = 0; i < rarray.length; i++) {        
        console.log(val[i].toString())
    }
})
  ```

## types<sup>8+</sup>

Provides APIs to check different types of built-in objects, such as ArrayBuffer, Map, and Set, so as to avoid exceptions or crashes caused by type errors.

### constructor<sup>8+</sup>

constructor()

A constructor used to create a **Types** object.

**System capability**: SystemCapability.Utils.Lang

**Example**

  ```js
  let type = new util.types();
  ```


### isAnyArrayBuffer<sup>8+</sup>

isAnyArrayBuffer(value: Object): boolean

Checks whether the input value is of the **ArrayBuffer** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **ArrayBuffer** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isAnyArrayBuffer(new ArrayBuffer(0));
  ```


### isArrayBufferView<sup>8+</sup>

isArrayBufferView(value: Object): boolean

Checks whether the input value is of the **ArrayBufferView** type.

**ArrayBufferView** is a helper type representing any of the following: **Int8Array**, **Int16Array**, **Int32Array**, **Uint8Array**, **Uint8ClampedArray**, **Uint32Array**, **Float32Array**, **Float64Array**, and **DataView**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **ArrayBufferView** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isArrayBufferView(new Int8Array([]));
  ```


### isArgumentsObject<sup>8+</sup>

isArgumentsObject(value: Object): boolean

Checks whether the input value is of the **arguments** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **arguments** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  function foo() {
      var result = that.isArgumentsObject(arguments);
  }
  let f = foo();
  ```


### isArrayBuffer<sup>8+</sup>

isArrayBuffer(value: Object): boolean

Checks whether the input value is of the **ArrayBuffer** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **ArrayBuffer** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isArrayBuffer(new ArrayBuffer(0));
  ```


### isAsyncFunction<sup>8+</sup>

isAsyncFunction(value: Object): boolean

Checks whether the input value is an asynchronous function.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is an asynchronous function; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isAsyncFunction(async function foo() {});
  ```


### isBooleanObject<sup>8+</sup>

isBooleanObject(value: Object): boolean

Checks whether the input value is of the **Boolean** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Boolean** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isBooleanObject(new Boolean(true));
  ```


### isBoxedPrimitive<sup>8+</sup>

isBoxedPrimitive(value: Object): boolean

Checks whether the input value is of the **Boolean**, **Number**, **String**, or **Symbol** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Boolean**, **Number**, **String**, or **Symbol** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isBoxedPrimitive(new Boolean(false));
  ```


### isDataView<sup>8+</sup>

isDataView(value: Object): boolean

Checks whether the input value is of the **DataView** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **DataView** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  const ab = new ArrayBuffer(20);
  let result = that.isDataView(new DataView(ab));
  ```


### isDate<sup>8+</sup>

isDate(value: Object): boolean

Checks whether the input value is of the **Date** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Date** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isDate(new Date());
  ```


### isExternal<sup>8+</sup>

isExternal(value: Object): boolean

Checks whether the input value is of the **native external** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **native external** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isExternal(true);
  ```


### isFloat32Array<sup>8+</sup>

isFloat32Array(value: Object): boolean

Checks whether the input value is of the **Float32Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Float32Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isFloat32Array(new Float32Array());
  ```


### isFloat64Array<sup>8+</sup>

isFloat64Array(value: Object): boolean

Checks whether the input value is of the **Float64Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Float64Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isFloat64Array(new Float64Array());
  ```


### isGeneratorFunction<sup>8+</sup>

isGeneratorFunction(value: Object): boolean

Checks whether the input value is a generator function.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a generator function; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isGeneratorFunction(function* foo() {});
  ```


### isGeneratorObject<sup>8+</sup>

isGeneratorObject(value: Object): boolean

Checks whether the input value is a generator object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a generator object; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  function* foo() {}
  const generator = foo();
  let result = that.isGeneratorObject(generator);
  ```


### isInt8Array<sup>8+</sup>

isInt8Array(value: Object): boolean

Checks whether the input value is of the **Int8Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Int8Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isInt8Array(new Int8Array([]));
  ```


### isInt16Array<sup>8+</sup>

isInt16Array(value: Object): boolean

Checks whether the input value is of the **Int16Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Int16Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isInt16Array(new Int16Array([]));
  ```


### isInt32Array<sup>8+</sup>

isInt32Array(value: Object): boolean

Checks whether the input value is of the **Int32Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Int32Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isInt32Array(new Int32Array([]));
  ```


### isMap<sup>8+</sup>

isMap(value: Object): boolean

Checks whether the input value is of the **Map** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Map** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isMap(new Map());
  ```


### isMapIterator<sup>8+</sup>

isMapIterator(value: Object): boolean

Checks whether the input value is of the **MapIterator** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**


| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **MapIterator** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  const map = new Map();
  let result = that.isMapIterator(map.keys());
  ```


### isNativeError<sup>8+</sup>

isNativeError(value: Object): boolean

Checks whether the input value is of the **Error** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Error** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isNativeError(new TypeError());
  ```


### isNumberObject<sup>8+</sup>

isNumberObject(value: Object): boolean

Checks whether the input value is a number object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a number object; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isNumberObject(new Number(0));
  ```


### isPromise<sup>8+</sup>

isPromise(value: Object): boolean

Checks whether the input value is a promise.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a promise; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isPromise(Promise.resolve(1));
  ```


### isProxy<sup>8+</sup>

isProxy(value: Object): boolean

Checks whether the input value is a proxy.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a proxy; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  const target = {};
  const proxy = new Proxy(target, {});
  let result = that.isProxy(proxy);
  ```


### isRegExp<sup>8+</sup>

isRegExp(value: Object): boolean

Checks whether the input value is of the **RegExp** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **RegExp** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isRegExp(new RegExp('abc'));
  ```


### isSet<sup>8+</sup>

isSet(value: Object): boolean

Checks whether the input value is of the **Set** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Set** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isSet(new Set());
  ```


### isSetIterator<sup>8+</sup>

isSetIterator(value: Object): boolean

Checks whether the input value is of the **SetIterator** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **SetIterator** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  const set = new Set();
  let result = that.isSetIterator(set.keys());
  ```


### isStringObject<sup>8+</sup>

isStringObject(value: Object): boolean

Checks whether the input value is a string object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a string object; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isStringObject(new String('foo'));
  ```


### isSymbolObjec<sup>8+</sup>

isSymbolObject(value: Object): boolean

Checks whether the input value is a symbol object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a symbol object; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  const symbols = Symbol('foo');
  let result = that.isSymbolObject(Object(symbols));
  ```


### isTypedArray<sup>8+</sup>

isTypedArray(value: Object): boolean

Checks whether the input value is of the **TypedArray** type.

**TypedArray** is a helper type representing any of the following: **Int8Array**, **Int16Array**, **Int32Array**, **Uint8Array**, **Uint8ClampedArray**, **Uint16Array**, **Uint32Array**, **Float32Array**, **Float64Array**, and **DataView**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **TypedArray** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isTypedArray(new Float64Array([]));
  ```


### isUint8Array<sup>8+</sup>

isUint8Array(value: Object): boolean

Checks whether the input value is of the **Uint8Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Uint8Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isUint8Array(new Uint8Array([]));
  ```


### isUint8ClampedArray<sup>8+</sup>

isUint8ClampedArray(value: Object): boolean

Checks whether the input value is of the **Uint8ClampedArray** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Uint8ClampedArray** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isUint8ClampedArray(new Uint8ClampedArray([]));
  ```


### isUint16Array<sup>8+</sup>

isUint16Array(value: Object): boolean

Checks whether the input value is of the **Uint16Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Uint16Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isUint16Array(new Uint16Array([]));
  ```


### isUint32Array<sup>8+</sup>

isUint32Array(value: Object): boolean

Checks whether the input value is of the **Uint32Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **Uint32Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isUint32Array(new Uint32Array([]));
  ```


### isWeakMap<sup>8+</sup>

isWeakMap(value: Object): boolean

Checks whether the input value is of the **WeakMap** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **WeakMap** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isWeakMap(new WeakMap());
  ```


### isWeakSet<sup>8+</sup>

isWeakSet(value: Object): boolean

Checks whether the input value is of the **WeakSet** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **WeakSet** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isWeakSet(new WeakSet());
  ```


### isBigInt64Array<sup>8+</sup>

isBigInt64Array(value: Object): boolean

Checks whether the input value is of the **BigInt64Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **BigInt64Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isBigInt64Array(new BigInt64Array([]));
  ```


### isBigUint64Array<sup>8+</sup>

isBigUint64Array(value: Object): boolean

Checks whether the input value is of the **BigUint64Array** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **BigUint64Array** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isBigUint64Array(new BigUint64Array([]));
  ```


### isModuleNamespaceObject<sup>8+</sup>

isModuleNamespaceObject(value: Object): boolean

Checks whether the input value is a module namespace object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is a module namespace object; returns **false** otherwise.|

**Example**

  ```js
  import url from '@ohos.url'
  let that = new util.types();
  let result = that.isModuleNamespaceObject(url);
  ```


### isSharedArrayBuffer<sup>8+</sup>

isSharedArrayBuffer(value: Object): boolean

Checks whether the input value is of the **SharedArrayBuffer** type.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the input value is of the **SharedArrayBuffer** type; returns **false** otherwise.|

**Example**

  ```js
  let that = new util.types();
  let result = that.isSharedArrayBuffer(new SharedArrayBuffer(0));
  ```
