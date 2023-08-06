# @ohos.buffer (Buffer)

A **Buffer** object represents a byte sequence of a fixed length. It is used to store binary data.

You can use the APIs provided by the Buffer module to process images and a large amount of binary data, and receive or upload files.

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```ts
import buffer from '@ohos.buffer';
```

## BufferEncoding

Enumerates the supported encoding formats of strings.

**System capability**: SystemCapability.Utils.Lang

| Encoding Format   | Description                |
| ------- | -------------------- |
| 'ascii' | ASCII format, which is case insensitive.|
| 'utf8' | UTF-8 format, which is case insensitive.|
| 'utf-8' | UTF-8 format, which is case insensitive.|
| 'utf16le' | UTF-16 little-endian format, which is case insensitive.|
| 'ucs2' | UTF-16 little-endian format, which is case insensitive.|
| 'ucs-2' | UTF-16 little-endian format, which is case insensitive.|
| 'base64' | Base64 format, which is case insensitive.|
| 'base64url' | Base64 format, which is case insensitive.|
| 'latin1' | ASCII format, which is case insensitive.|
| 'binary' | Binary format, which is case insensitive.|
| 'hex' | Hexadecimal format, which is case insensitive.|

## buffer.alloc

alloc(size: number, fill?: string | Buffer | number, encoding?: BufferEncoding): Buffer

Creates and initializes a **Buffer** instance of the specified length.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| size | number | Yes| Size of the **Buffer** instance to create, in bytes.|
| fill | string&nbsp;\|&nbsp;Buffer&nbsp;\|&nbsp;number | No| Value to be filled in the buffer. The default value is **0**.|
| encoding | [BufferEncoding](#bufferencoding) | No| Encoding format (valid only when **fill** is a string). The default value is **utf-8**.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance created.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.alloc(5);
let buf2 = buffer.alloc(5, 'a');
let buf3 = buffer.alloc(11, 'aGVsbG8gd29ybGQ=', 'base64');
```

## buffer.allocUninitializedFromPool

allocUninitializedFromPool(size: number): Buffer

Creates a **Buffer** instance of the specified size from the buffer pool, without initializing it.
You need to use [fill()](#fill) to initialize the **Buffer** instance created.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| size | number | Yes| Size of the **Buffer** instance to create, in bytes.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | Uninitialized **Buffer** instance.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(10);
buf.fill(0);
```

## buffer.allocUninitialized

allocUninitialized(size: number): Buffer

Creates a **Buffer** instance of the specified size, without initializing it. This API does not allocate memory from the buffer pool.
You need to use [fill()](#fill) to initialize the **Buffer** instance created.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| size | number | Yes|Size of the **Buffer** instance to create, in bytes.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | Uninitialized **Buffer** instance.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitialized(10);
buf.fill(0);
```

## buffer.byteLength

byteLength(string: string | Buffer | TypedArray | DataView | ArrayBuffer | SharedArrayBuffer, encoding?: BufferEncoding): number

Obtains the number of bytes of a string based on the encoding format.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| string | string&nbsp;\|&nbsp;Buffer&nbsp;\|&nbsp;TypedArray&nbsp;\|&nbsp;DataView&nbsp;\|&nbsp;ArrayBuffer&nbsp;\|&nbsp;SharedArrayBuffer | Yes| Target string.|
| encoding | [BufferEncoding](#bufferencoding) | No| Encoding format of the string. The default value is **utf-8**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes of the string.|

**Example**

```ts
import buffer from '@ohos.buffer';

let str = '\u00bd + \u00bc = \u00be';
console.log(`${str}: ${str.length} characters, ${buffer.byteLength(str, 'utf-8')} bytes`);
// Print: ½ + ¼ = ¾: 9 characters, 12 bytes
```

## buffer.compare

compare(buf1: Buffer | Uint8Array, buf2: Buffer | Uint8Array): -1 | 0 | 1

Compares two **Buffer** instances. This API is used for sorting **Buffer** instances.


**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| buf1 | Buffer&nbsp;\|&nbsp;Uint8Array | Yes| **Buffer** instance to compare.|
| buf2 | Buffer&nbsp;\|&nbsp;Uint8Array | Yes| **Buffer** instance to compare.|

**Return value**

| Type| Description|
| -------- | -------- |
| -1&nbsp;\|&nbsp;0&nbsp;\|&nbsp;1 | Returns **0** if **buf1** is the same as **buf2**.<br>Returns **1** if **buf1** comes after **buf2** when sorted.<br>Returns **-1** if **buf1** comes before **buf2** when sorted.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from('1234');
let buf2 = buffer.from('0123');
let res = buf1.compare(buf2);

console.log(Number(res).toString()); // Print 1.
```

## buffer.concat

concat(list: Buffer[] | Uint8Array[], totalLength?: number): Buffer

Concatenates an array of **Buffer** instances of the specified length into a new instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| list | Buffer[]&nbsp;\|&nbsp;Uint8Array[] | Yes| Array of instances to concatenate.|
| totalLength | number | No| Total length of bytes to be copied. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance created.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "totalLength" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from("1234");
let buf2 = buffer.from("abcd");
let buf = buffer.concat([buf1, buf2]);
console.log(buf.toString('hex')); // 3132333461626364
```

## buffer.from

from(array: number[]): Buffer;

Creates a **Buffer** instance with the specified array.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| array | number[] | Yes| Array to create a **Buffer** instance.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance created.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x62, 0x75, 0x66, 0x66, 0x65, 0x72]);
console.log(buf.toString('hex')); // 627566666572
```

## buffer.from

from(arrayBuffer: ArrayBuffer | SharedArrayBuffer, byteOffset?: number, length?: number): Buffer

Creates a **Buffer** instance of the specified length that shares memory with **arrayBuffer**.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| arrayBuffer | ArrayBuffer&nbsp;\|&nbsp;SharedArrayBuffer | Yes| Array of **Buffer** instances, whose memory is to be shared.|
| byteOffset | number | No| Byte offset. The default value is **0**.|
| length | number | No| Length of the **Buffer** instance to create, in bytes. The default value is the length of **arrayBuffer** minus **byteOffset**.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance with shared memory.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[byteOffset/length]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let ab = new ArrayBuffer(10);
let buf = buffer.from(ab, 0, 2);
```

## buffer.from

from(buffer: Buffer | Uint8Array): Buffer

Creates a **Buffer** instance with the copy of another instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| buffer | Buffer&nbsp;\|&nbsp;Uint8Array | Yes| **Buffer** instance to copy.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance created.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from('buffer');
let buf2 = buffer.from(buf1);
```

## buffer.from

from(object: Object, offsetOrEncoding: number | string, length: number): Buffer

Creates a **Buffer** instance based on the specified object.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| object | Object | Yes| Object that supports **Symbol.toPrimitive** or **valueOf()**.|
| offsetOrEncoding | number&nbsp;\|&nbsp;string | Yes| Byte offset or encoding format.|
| length | number | Yes| Length of the **Buffer** instance to create.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance created.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from(new String('this is a test'), 'utf8', 14);
```

## buffer.from

from(string: String, encoding?: BufferEncoding): Buffer

Creates a **Buffer** instance based on a string in the given encoding format.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| string | String | Yes| String.|
| encoding | [BufferEncoding](#bufferencoding) | No| Encoding format of the string. The default value is **utf-8**.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance created.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from('this is a test');
let buf2 = buffer.from('7468697320697320612074c3a97374', 'hex');

console.log (buf1.toString()); // Print: this is a test
console.log (buf2.toString()); // print: this is a test
```


## buffer.isBuffer

isBuffer(obj: Object): boolean

Checks whether the specified object is a **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| obj | Object | Yes| Object to check.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the object is a **Buffer** instance; returns **false** otherwise.|

**Example**

```ts
import buffer from '@ohos.buffer';

let result = buffer.isBuffer(buffer.alloc(10)); // true
let result1 = buffer.isBuffer(buffer.from('foo')); // true
let result2 = buffer.isBuffer('a string'); // false
let result3 = buffer.isBuffer([]); // false
let result4 = buffer.isBuffer(new Uint8Array(1024)); // false
```

## buffer.isEncoding

isEncoding(encoding: string): boolean

Checks whether the encoding format is supported.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| encoding | string | Yes| Encoding format.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the encoding format is supported; returns **false** otherwise.|

**Example**

```ts
import buffer from '@ohos.buffer';

console.log(buffer.isEncoding('utf-8').toString());	// Print: true
console.log(buffer.isEncoding('hex').toString());	// Print: true
console.log(buffer.isEncoding('utf/8').toString());	// Print: false
console.log(buffer.isEncoding('').toString());	// Print: false
```

## buffer.transcode

transcode(source: Buffer | Uint8Array, fromEnc: string, toEnc: string): Buffer

Transcodes the given **Buffer** or **Uint8Array** object from one encoding format to another.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| source | Buffer&nbsp;\|&nbsp;Uint8Array | Yes| Instance to encode.|
| fromEnc | string | Yes| Current encoding format.|
| toEnc | string | Yes| Target encoding format.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | New **Buffer** instance in the target encoding format.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.alloc(50);
let newBuf = buffer.transcode(buffer.from('€'), 'utf-8', 'ascii');
console.log(newBuf.toString('ascii'));
```

## Buffer

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| length | number | Yes| No| Length of the **Buffer** instance, in bytes.|
| buffer | ArrayBuffer | Yes| No| **ArrayBuffer** object.|
| byteOffset | number | Yes| No| Offset of the **Buffer** instance in the memory pool.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200013 | Cannot set property ${propertyName} of Buffer which has only a getter. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from("1236");
console.log(JSON.stringify(buf.length));
let arrayBuffer = buf.buffer;
console.log(JSON.stringify(new Uint8Array(arrayBuffer)));
console.log(JSON.stringify(buf.byteOffset));
```

### compare

compare(target: Buffer | Uint8Array, targetStart?: number, targetEnd?: number, sourceStart?: number, sourceEnd?: number): -1 | 0 | 1

Compares this **Buffer** instance with another instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| target | Buffer&nbsp;\|&nbsp;Uint8Array | Yes| Target **Buffer** instance to compare.|
| targetStart | number | No| Offset to the start of the data to compare in the target **Buffer** instance. The default value is **0**.|
| targetEnd | number | No| Offset to the end of the data to compare in the target **Buffer** instance (not inclusive). The default value is the length of the target **Buffer** instance.|
| sourceStart | number | No| Offset to the start of the data to compare in this **Buffer** instance. The default value is **0**.|
| sourceEnd | number | No| Offset to the end of the data to compare in this **Buffer** instance (not inclusive). The default value is the length of this **Buffer** instance.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Returns **0** if the two **Buffer** instances are the same.<br>Returns **1** if this instance comes after the target instance when sorted. <br>Returns **-1** if this instance comes before the target instance when sorted.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[targetStart/targetEnd/sourceStart/sourceEnd]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from([1, 2, 3, 4, 5, 6, 7, 8, 9]);
let buf2 = buffer.from([5, 6, 7, 8, 9, 1, 2, 3, 4]);

console.log(buf1.compare(buf2, 5, 9, 0, 4).toString());	// Print: 0
console.log(buf1.compare(buf2, 0, 6, 4).toString());	// Print: -1
console.log(buf1.compare(buf2, 5, 6, 5).toString());	// Print: 1
```

### copy

copy(target: Buffer| Uint8Array, targetStart?: number, sourceStart?: number, sourceEnd?: number): number

Copies data at the specified position in this **Buffer** instance to the specified position in another **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| target | Buffer&nbsp;\|&nbsp;Uint8Array | Yes| Instance to which data is copied.|
| targetStart | number | No| Offset to the start position in the target instance where data is copied. The default value is **0**.|
| sourceStart | number | No| Offset to the start position in this **Buffer** instance where data is copied. The default value is **0**.|
| sourceEnd | number | No| Offset to the end position in this **Buffer** instance (not inclusive). The default value is the length of this **Buffer** instance.|

**Return value**

| Type| Description|
| -------- | -------- |
| number |  Total length of the data copied, in bytes.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[targetStart/sourceStart/sourceEnd]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.allocUninitializedFromPool(26);
let buf2 = buffer.allocUninitializedFromPool(26).fill('!');

for (let i = 0; i < 26; i++) {
  buf1[i] = i + 97;
}

buf1.copy(buf2, 8, 16, 20);
console.log(buf2.toString('ascii', 0, 25));
// Print: !!!!!!! qrst!!!!!!!!!!!!!
```

### entries

entries(): IterableIterator&lt;[number,&nbsp;number]&gt;

Creates and returns an iterator that contains key-value pairs of this **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number |  Key of the iterator.|
| number |  Value of the iterator.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from('buffer');
for (let pair of buf.entries()) {
  console.log(pair.toString());
}
```

### equals

equals(otherBuffer: Uint8Array | Buffer): boolean

Checks whether this **Buffer** instance is the same as another **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| otherBuffer | Uint8Array&nbsp;\|&nbsp;Buffer | Yes| **Buffer** instance to compare.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the two instances are the same; returns **false** otherwise.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from('ABC');
let buf2 = buffer.from('414243', 'hex');
let buf3 = buffer.from('ABCD');

console.log(buf1.equals(buf2).toString());	// Print: true
console.log(buf1.equals(buf3).toString());	// Print: false
```

### fill

fill(value: string | Buffer | Uint8Array | number, offset?: number, end?: number, encoding?: BufferEncoding): Buffer

Fills this **Buffer** instance at the specified position. By default, data is filled cyclically.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | string&nbsp;\|&nbsp;Buffer&nbsp;\|&nbsp;Uint8Array&nbsp;\|&nbsp;number | Yes| Value to fill.|
| offset | number | No| Offset to the start position in this **Buffer** instance where data is filled. The default value is **0**.|
| end | number | No| Offset to the end position in this **Buffer** instance (not inclusive). The default value is the length of this **Buffer** instance.|
| encoding | [BufferEncoding](#bufferencoding) | No| Encoding format (valid only when **value** is a string). The default value is **utf-8**.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance filled with the specified value.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[offset/end]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let b = buffer.allocUninitializedFromPool(50).fill('h');
console.log(b.toString());
```


### includes

includes(value: string | number | Buffer | Uint8Array, byteOffset?: number, encoding?: BufferEncoding): boolean

Checks whether this **Buffer** instance contains the specified value.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | string&nbsp;\|&nbsp;number&nbsp;\|&nbsp;Buffer&nbsp;\|&nbsp;Uint8Array | Yes| Value to match.|
| byteOffset | number | No| Number of bytes to skip before starting to check data. If the offset is a negative number, data is checked from the end of the **Buffer** instance. The default value is **0**.|
| encoding | [BufferEncoding](#bufferencoding) | No| Encoding format (valid only when **value** is a string). The default value is **utf-8**.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the instance contains the specified value; returns **false** otherwise.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from('this is a buffer');
console.log(buf.includes('this').toString());	// Print: true
console.log(buf.includes('be').toString());	// Print: false
```

### indexOf

indexOf(value: string | number | Buffer | Uint8Array, byteOffset?: number, encoding?: BufferEncoding): number

Obtains the index of the first occurrence of the specified value in this **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | string&nbsp;\|&nbsp;number&nbsp;\|&nbsp;Buffer&nbsp;\|&nbsp;Uint8Array | Yes| Value to match.|
| byteOffset | number | No| Number of bytes to skip before starting to check data. If the offset is a negative number, data is checked from the end of the **Buffer** instance. The default value is **0**.|
| encoding | [BufferEncoding](#bufferencoding) | No| Encoding format (valid only when **value** is a string). The default value is **utf-8**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Index obtained. <br>If **-1** is returned, the **Buffer** instance does not contain the specified value.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from('this is a buffer');
console.log(buf.indexOf('this').toString());	// Print: 0
console.log(buf.indexOf('is').toString());		// Print: 2
```

### keys

keys(): IterableIterator&lt;number&gt;

Creates and returns an iterator that contains the keys of this **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
|  IterableIterator&lt;number&gt; | Iterator created.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from('buffer');
for (const key of buf.keys()) {
  console.log(key.toString());
}
```

### lastIndexOf

lastIndexOf(value: string | number | Buffer | Uint8Array, byteOffset?: number, encoding?: BufferEncoding): number

Obtains the index of the last occurrence of the specified value in this **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | string&nbsp;\|&nbsp;number&nbsp;\|&nbsp;Buffer&nbsp;\|&nbsp;Uint8Array | Yes| Value to match.|
| byteOffset | number | No| Number of bytes to skip before starting to check data. If the offset is a negative number, data is checked from the end of the **Buffer** instance. The default value is **0**.|
| encoding | [BufferEncoding](#bufferencoding) | No| Encoding format (valid only when **value** is a string). The default value is **utf-8**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Index obtained.<br>If **-1** is returned, the **Buffer** instance does not contain the specified value.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from('this buffer is a buffer');
console.log(buf.lastIndexOf('this').toString());	// Print: 0
console.log(buf.lastIndexOf('buffer').toString());	// Print: 17
```


### readBigInt64BE

readBigInt64BE(offset?: number): bigint

Reads a 64-bit, big-endian, signed big integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| bigint | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69, 0x70, 
        0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77, 0x78]);
console.log(buf.readBigInt64BE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(8);
let result = buf1.writeBigInt64BE(BigInt(0x0102030405060708), 0);
```

### readBigInt64LE

readBigInt64LE(offset?: number): bigint

Reads a 64-bit, little-endian, signed big integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| bigint | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69, 0x70, 
        0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77, 0x78]);
console.log(buf.readBigInt64LE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(8);
let result = buf1.writeBigInt64BE(BigInt(0x0102030405060708), 0);
```

### readBigUInt64BE

readBigUInt64BE(offset?: number): bigint

Reads a 64-bit, big-endian, unsigned big integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| bigint | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69, 0x70, 
        0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77, 0x78]);
console.log(buf.readBigUInt64BE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(8);
let result = buf1.writeBigUInt64BE(BigInt(0xdecafafecacefade), 0);
```

### readBigUInt64LE

readBigUInt64LE(offset?: number): bigint

Reads a 64-bit, little-endian, unsigned big integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| bigint | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69, 0x70, 
        0x71, 0x72, 0x73, 0x74, 0x75, 0x76, 0x77, 0x78]);
console.log(buf.readBigUInt64LE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(8);
let result = buf1.writeBigUInt64BE(BigInt(0xdecafafecacefade), 0);
```

### readDoubleBE

readDoubleBE(offset?: number): number

Reads a 64-bit, big-endian, double-precision floating-point number from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([1, 2, 3, 4, 5, 6, 7, 8]);
console.log(buf.readDoubleBE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(8);
let result = buf1.writeDoubleBE(123.456, 0);
```

### readDoubleLE

readDoubleLE(offset?: number): number

Reads a 64-bit, little-endian, double-precision floating-point number from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([1, 2, 3, 4, 5, 6, 7, 8]);
console.log(buf.readDoubleLE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(8);
let result = buf1.writeDoubleLE(123.456, 0);
```

### readFloatBE

readFloatBE(offset?: number): number

Reads a 32-bit, big-endian, single-precision floating-point number from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([1, 2, 3, 4, 5, 6, 7, 8]);
console.log(buf.readFloatBE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeFloatBE(0xcabcbcbc, 0);
```

### readFloatLE

readFloatLE(offset?: number): number

Reads a 32-bit, little-endian, single-precision floating-point number from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([1, 2, 3, 4, 5, 6, 7, 8]);
console.log(buf.readFloatLE(0).toString());

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeFloatLE(0xcabcbcbc, 0);
```

### readInt8

readInt8(offset?: number): number

Reads a 8-bit signed integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([-1, 5]);
console.log(buf.readInt8(0).toString());	// Print: -1
console.log(buf.readInt8(1).toString());	// Print: 5

let buf1 = buffer.allocUninitializedFromPool(2);
let result = buf1.writeInt8(0x12);
```

### readInt16BE

readInt16BE(offset?: number): number

Reads a 16-bit, big-endian, signed integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0, 5]);
console.log(buf.readInt16BE(0).toString());	// Print: 5

let buf1 = buffer.alloc(2);
let result = buf1.writeInt16BE(0x1234, 0);
```

### readInt16LE

readInt16LE(offset?: number): number

Reads a 16-bit, little-endian, signed integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0, 5]);
console.log(buf.readInt16LE(0).toString());	// Print: 1280

let buf1 = buffer.alloc(2);
let result = buf1.writeInt16BE(0x1234, 0);
```

### readInt32BE

readInt32BE(offset?: number): number

Reads a 32-bit, big-endian, signed integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0, 0, 0, 5]);
console.log(buf.readInt32BE(0).toString());	// Print: 5

let buf1 = buffer.alloc(4);
let result = buf1.writeInt32BE(0x12345678, 0);
```

### readInt32LE

readInt32LE(offset?: number): number

Reads a 32-bit, little-endian, signed integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0, 0, 0, 5]);
console.log(buf.readInt32LE(0).toString());	// Print: 83886080

let buf1 = buffer.alloc(4);
let result = buf1.writeInt32BE(0x12345678, 0);
```

### readIntBE

readIntBE(offset: number, byteLength: number): number

Reads the specified number of bytes from this **Buffer** instance at the specified offset, and interprets the result as a big-endian, two's complement signed value that supports up to 48 bits of precision.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | Yes| Number of bytes to skip before starting to read data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to read.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from("ab");
let num = buf.readIntBE(0, 1);
console.log(num.toString()); // 97

let buf1 = buffer.allocUninitializedFromPool(6);
let result = buf1.writeIntBE(0x123456789011, 0, 6);
```


### readIntLE

readIntLE(offset: number, byteLength: number): number

Reads the specified number of bytes from this **Buffer** instance at the specified offset and interprets the result as a little-endian, two's complement signed value that supports up to 48 bits of precision.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | Yes| Number of bytes to skip before starting to read data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to read.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x12, 0x34, 0x56, 0x78, 0x90, 0xab]);
console.log(buf.readIntLE(0, 6).toString(16));

let buf1 = buffer.allocUninitializedFromPool(6);
let result = buf1.writeIntLE(0x123456789011, 0, 6);
```

### readUInt8

readUInt8(offset?: number): number

Reads a 8-bit unsigned integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([1, -2]);
console.log(buf.readUInt8(0).toString());
console.log(buf.readUInt8(1).toString());

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeUInt8(0x42);
```

### readUInt16BE

readUInt16BE(offset?: number): number

Reads a 16-bit, big-endian, unsigned integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x12, 0x34, 0x56]);
console.log(buf.readUInt16BE(0).toString(16));
console.log(buf.readUInt16BE(1).toString(16));

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeUInt16BE(0x1234, 0);
```

### readUInt16LE

readUInt16LE(offset?: number): number

Reads a 16-bit, little-endian, unsigned integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x12, 0x34, 0x56]);
console.log(buf.readUInt16LE(0).toString(16));
console.log(buf.readUInt16LE(1).toString(16));

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeUInt16LE(0x1234, 0);
```

### readUInt32BE

readUInt32BE(offset?: number): number

Reads a 32-bit, big-endian, unsigned integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x12, 0x34, 0x56, 0x78]);
console.log(buf.readUInt32BE(0).toString(16));

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeUInt32BE(0x12345678, 0);
```

### readUInt32LE

readUInt32LE(offset?: number): number

Reads a 32-bit, little-endian, unsigned integer from this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | No| Number of bytes to skip before starting to read data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "offset" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x12, 0x34, 0x56, 0x78]);
console.log(buf.readUInt32LE(0).toString(16));

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeUInt32LE(0x12345678, 0);
```

### readUIntBE

readUIntBE(offset: number, byteLength: number): number

Reads the specified number of bytes from this **Buffer** instance at the specified offset, and interprets the result as an unsigned, big-endian integer that supports up to 48 bits of precision.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | Yes| Number of bytes to skip before starting to read data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to read.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x12, 0x34, 0x56, 0x78, 0x90, 0xab]);
console.log(buf.readUIntBE(0, 6).toString(16));

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeUIntBE(0x13141516, 0, 4);
```

### readUIntLE

readUIntLE(offset: number, byteLength: number): number

Reads the specified number of bytes from this **Buffer** instance at the specified offset, and interprets the result as an unsigned, little-endian integer that supports up to 48 bits of precision.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| offset | number | Yes| Number of bytes to skip before starting to read data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to read.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Data read.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.from([0x12, 0x34, 0x56, 0x78, 0x90, 0xab]);
console.log(buf.readUIntLE(0, 6).toString(16));

let buf1 = buffer.allocUninitializedFromPool(4);
let result = buf1.writeUIntLE(0x13141516, 0, 4);
```

### subarray

subarray(start?: number, end?: number): Buffer

Truncates this **Buffer** instance from the specified position to create a new **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| start | number | No| Offset to the start position in this **Buffer** instance where data is truncated. The default value is **0**.|
| end | number | No|  Offset to the end position in this **Buffer** instance (not inclusive). The default value is the length of this **Buffer** instance.|

**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance created.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.allocUninitializedFromPool(26);

for (let i = 0; i < 26; i++) {
  buf1[i] = i + 97;
}
const buf2 = buf1.subarray(0, 3);
console.log(buf2.toString('ascii', 0, buf2.length));
// Print: abc
```

### swap16

swap16(): Buffer

Interprets this **Buffer** instance as an array of unsigned 16-bit integers and swaps the byte order in place.

**System capability**: SystemCapability.Utils.Lang


**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance swapped.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200009 | Buffer size must be a multiple of 16-bits |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from([0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8]);
console.log(buf1.toString('hex'));	// Print: 0102030405060708

buf1.swap16();
console.log(buf1.toString('hex'));	// Print: 0201040306050807
```

### swap32

swap32(): Buffer

Interprets this **Buffer** instance as an array of unsigned 32-bit integers and swaps the byte order in place.

**System capability**: SystemCapability.Utils.Lang


**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance swapped.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200009 | Buffer size must be a multiple of 32-bits |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from([0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8]);
console.log(buf1.toString('hex'));	// Print: 0102030405060708

buf1.swap32();
console.log(buf1.toString('hex'));	// Print: 0403020108070605
```

### swap64

swap64(): Buffer

Interprets this **Buffer** instance as an array of unsigned 64-bit integers and swaps the byte order in place.

**System capability**: SystemCapability.Utils.Lang


**Return value**

| Type| Description|
| -------- | -------- |
| Buffer | **Buffer** instance swapped.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200009 | Buffer size must be a multiple of 64-bits |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from([0x1, 0x2, 0x3, 0x4, 0x5, 0x6, 0x7, 0x8]);
console.log(buf1.toString('hex'));	// Print: 0102030405060708
buf1.swap64();
console.log(buf1.toString('hex'));	// Print: 0807060504030201
```

### toJSON

toJSON(): Object

Converts this **Buffer** instance into a JSON object.

**System capability**: SystemCapability.Utils.Lang


**Return value**

| Type| Description|
| -------- | -------- |
| Object | JSON object.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from([0x1, 0x2, 0x3, 0x4, 0x5]);
let obj = buf1.toJSON();
console.log(JSON.stringify(obj))
// Print: {"type":"Buffer","data":[1,2,3,4,5]}
```

### toString

toString(encoding?: string, start?: number, end?: number): string

Converts the data at the specified position in this **Buffer** instance into a string in the specified encoding format.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| encoding | string | No| Encoding format (valid only when **value** is a string). The default value is **utf-8**.|
| start  | number | No|  Offset to the start position of the data to convert. The default value is **0**.|
| end  | number | No|  Offset to the end position of the data to convert. The default value is the length of this **Buffer** instance.|

**Return value**

| Type| Description|
| -------- | -------- |
| string | String obtained.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.allocUninitializedFromPool(26);
for (let i = 0; i < 26; i++) {
  buf1[i] = i + 97;
}
console.log(buf1.toString('utf-8'));
// Print: abcdefghijklmnopqrstuvwxyz
```

### values

values(): IterableIterator&lt;number&gt;

Creates and returns an iterator that contains the values of this **Buffer** instance.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| IterableIterator&lt;number&gt; | Iterator.|

**Example**

```ts
import buffer from '@ohos.buffer';

let buf1 = buffer.from('buffer');
for (let value of buf1.values()) {
  console.log(value.toString());
}
```

### write

write(str: string, offset?: number, length?: number, encoding?: string): number

Writes a string of the specified length to this **Buffer** instance at the specified position in the given encoding format.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| str | string | Yes| String to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|
| length | number | No| Maximum number of bytes to write. The default value is the length of the **Buffer** instance minus the offset.|
| encoding | string | No| Encoding format of the string. The default value is **utf-8**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[offset/length]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.alloc(256);
let len = buf.write('\u00bd + \u00bc = \u00be', 0);
console.log(`${len} bytes: ${buf.toString('utf-8', 0, len)}`);
// Print: 12 bytes: ½ + ¼ = ¾

let buffer1 = buffer.alloc(10);
let length = buffer1.write('abcd', 8);
```

### writeBigInt64BE

writeBigInt64BE(value: bigint, offset?: number): number

Writes a 64-bit, big-endian, signed big integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | bigint | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeBigInt64BE(BigInt(0x0102030405060708), 0);
```

### writeBigInt64LE

writeBigInt64LE(value: bigint, offset?: number): number

Writes a 64-bit, little-endian, signed big integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | bigint | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeBigInt64LE(BigInt(0x0102030405060708), 0);
```

### writeBigUInt64BE

writeBigUInt64BE(value: bigint, offset?: number): number

Writes a 64-bit, big-endian, unsigned big integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | bigint | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeBigUInt64BE(BigInt(0xdecafafecacefade), 0);
```

### writeBigUInt64LE

writeBigUInt64LE(value: bigint, offset?: number): number

Writes a 64-bit, little-endian, unsigned big integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | bigint | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeBigUInt64LE(BigInt(0xdecafafecacefade), 0);
```

### writeDoubleBE

writeDoubleBE(value: number, offset?: number): number

Writes a 64-bit, big-endian, double-precision floating-point number to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeDoubleBE(123.456, 0);
```

### writeDoubleLE

writeDoubleLE(value: number, offset?: number): number

Writes a 64-bit, little-endian, double-precision floating-point number to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeDoubleLE(123.456, 0);
```

### writeFloatBE

writeFloatBE(value: number, offset?: number): number

Writes a 32-bit, big-endian, single-precision floating-point number to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeFloatBE(0xcafebabe, 0);
```


### writeFloatLE

writeFloatLE(value: number, offset?: number): number

Writes a 32-bit, little-endian, single-precision floating-point number to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(8);
let result = buf.writeFloatLE(0xcafebabe, 0);
```

### writeInt8

writeInt8(value: number, offset?: number): number

Writes a 8-bit signed integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(2);
let result = buf.writeInt8(2, 0);
let result1 = buf.writeInt8(-2, 1);
```


### writeInt16BE

writeInt16BE(value: number, offset?: number): number

Writes a 16-bit, big-endian, signed integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(2);
let result = buf.writeInt16BE(0x0102, 0);
```


### writeInt16LE

writeInt16LE(value: number, offset?: number): number

Writes a 16-bit, little-endian, signed integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(2);
let result = buf.writeInt16LE(0x0304, 0);
```

### writeInt32BE

writeInt32BE(value: number, offset?: number): number

Writes a 32-bit, big-endian, signed integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(4);
let result = buf.writeInt32BE(0x01020304, 0);
```


### writeInt32LE

writeInt32LE(value: number, offset?: number): number

Writes a 32-bit, little-endian, signed integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(4);
let result = buf.writeInt32LE(0x05060708, 0);
```

### writeIntBE

writeIntBE(value: number, offset: number, byteLength: number): number

Writes a big-endian signed value of the specified length to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | Yes| Number of bytes to skip before starting to write data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to write.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(6);
let result = buf.writeIntBE(0x1234567890ab, 0, 6);
```


### writeIntLE

writeIntLE(value: number, offset: number, byteLength: number): number

Writes a little-endian signed value of the specified length to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | Yes| Number of bytes to skip before starting to write data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to write.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(6);
let result = buf.writeIntLE(0x1234567890ab, 0, 6);
```

### writeUInt8

writeUInt8(value: number, offset?: number): number

Writes a 8-bit unsigned integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(4);
let result = buf.writeUInt8(0x3, 0);
let result1 = buf.writeUInt8(0x4, 1);
let result2 = buf.writeUInt8(0x23, 2);
let result3 = buf.writeUInt8(0x42, 3);
```

### writeUInt16BE

writeUInt16BE(value: number, offset?: number): number

Writes a 16-bit, big-endian, unsigned integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(4);
let result = buf.writeUInt16BE(0xdead, 0);
let result1 = buf.writeUInt16BE(0xbeef, 2);
```

### writeUInt16LE

writeUInt16LE(value: number, offset?: number): number

Writes a 16-bit, little-endian, unsigned integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(4);
let result = buf.writeUInt16LE(0xdead, 0);
let result1 = buf.writeUInt16LE(0xbeef, 2);
```

### writeUInt32BE

writeUInt32BE(value: number, offset?: number): number

Writes a 32-bit, big-endian, unsigned integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(4);
let result = buf.writeUInt32BE(0xfeedface, 0);
```

### writeUInt32LE

writeUInt32LE(value: number, offset?: number): number

Writes a 32-bit, little-endian, unsigned integer to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | No| Number of bytes to skip before starting to write data. The default value is **0**.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(4);
let result = buf.writeUInt32LE(0xfeedface, 0);
```

### writeUIntBE

writeUIntBE(value: number, offset: number, byteLength: number): number

Writes an unsigned big-endian value of the specified length to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | Yes| Number of bytes to skip before starting to write data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to write.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(6);
let result = buf.writeUIntBE(0x1234567890ab, 0, 6);
```

### writeUIntLE

writeUIntLE(value: number, offset: number, byteLength: number): number

Writes an unsigned little-endian value of the specified length to this **Buffer** instance at the specified offset.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| value | number | Yes| Data to write.|
| offset | number | Yes| Number of bytes to skip before starting to write data. The default value is **0**.|
| byteLength | number | Yes| Number of bytes to write.|


**Return value**

| Type| Description|
| -------- | -------- |
| number | Number of bytes written.|

**Error codes**

For details about the error codes, see [Utils Error Codes](../errorcodes/errorcode-utils.md).

| ID| Error Message|
| -------- | -------- |
| 10200001 | The value of "[value/offset/byteLength]" is out of range. |

**Example**

```ts
import buffer from '@ohos.buffer';

let buf = buffer.allocUninitializedFromPool(6);
let result = buf.writeUIntLE(0x1234567890ab, 0, 6);
```

## Blob

### Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| size | number | Yes| No| Total size of the **Blob** instance, in bytes.|
| type | string | Yes| No| Type of the data in the **Blob** instance.|

### constructor

constructor(sources: string[] | ArrayBuffer[] | TypedArray[] | DataView[] | Blob[] , options?: Object)

A constructor used to create a **Blob** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| sources | string[]&nbsp;\|&nbsp;ArrayBuffer[]&nbsp;\|&nbsp;TypedArray[]&nbsp;\|&nbsp;DataView[]&nbsp;\|&nbsp;Blob[] | Yes| Data sources of the **Blob** instance.|
| options | Object | No| options:<br>- **endings**: specifies how the terminator **'\n'** is output. The value can be **'native'** or **'transparent'**. **'native'** means that the terminator follows the system. **'transparent'** means that the terminator stored in the **Blob** instance remains unchanged. The default value is **'transparent'**.<br>- **type**: type of the data in the **Blob** instance. This type represents the MIME type of the data. However, it is not used for type format validation. The default value is **''**.|


**Example**
```ts
import buffer from '@ohos.buffer';

let blob = new buffer.Blob(['a', 'b', 'c']);
let blob1 = new buffer.Blob(['a', 'b', 'c'], {endings:'native', type: 'MIME'});
```

### arrayBuffer

arrayBuffer(): Promise&lt;ArrayBuffer&gt;

Puts the **Blob** data into an **ArrayBuffer** instance. This API uses a promise to return the result.

**System capability**: SystemCapability.Utils.Lang

**Return value**
| Type| Description|
| -------- | -------- |
| Promise&lt;ArrayBuffer&gt; | Promise used to return the **ArrayBuffer** containing the **Blob** data.|

**Example**
```ts
let blob = new buffer.Blob(['a', 'b', 'c']);
let pro = blob.arrayBuffer();
pro.then(val => {
  let uintarr = new Uint8Array(val);
  console.log(uintarr.toString());
});
```
### slice

slice(start?: number, end?: number, type?: string): Blob

Creates a **Blob** instance by copying specified data from this **Blob** instance.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| start | number | No| Offset to the start position of the data to copy. The default value is **0**.|
| end | number | No| Offset to the end position of the data to copy. The default value is the data length in the original **Blob** instance.|
| type | string | No| Type of the data in the new **Blob** instance. The default value is **''**.|

**Return value**
| Type| Description|
| -------- | -------- |
| Blob | New **Blob** instance created.|

**Example**
```ts
let blob = new buffer.Blob(['a', 'b', 'c']);
let blob2 = blob.slice(0, 2);
let blob3 = blob.slice(0, 2, "MIME");
```

### text

text(): Promise&lt;string&gt;

Returns text in UTF-8 format. This API uses a promise to return the result.

**System capability**: SystemCapability.Utils.Lang

**Return value**
| Type| Description|
| -------- | -------- |
| Promise&lt;string&gt; | Promise used to return the text encoded in UTF-8.|

**Example**
```ts
let blob = new buffer.Blob(['a', 'b', 'c']);
let pro = blob.text();
pro.then(val => {
    console.log(val)
});
```
