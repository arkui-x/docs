# @ohos.zlib (Zip)

The **@ohos.zlib** module provides APIs for file compression and decompression.

> **NOTE**
>
> The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```javascript
import zlib from '@ohos.zlib';
```

## zlib.compressFile<sup>9+</sup>

compressFile(inFile: string, outFile: string, options: Options, callback: AsyncCallback\<void>): void;

Compresses a file. This API uses an asynchronous callback to return the result.  

**System capability**: SystemCapability.BundleManager.Zlib

**Parameters**

| Name                 | Type               | Mandatory| Description                                                        |
| ----------------------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile                  | string              | Yes  | Path of the folder or file to compress. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md).|
| outFile                 | string              | Yes  | Path of the compressed file.                                          |
| options                 | [Options](#options) | Yes  | Compression parameters.                                              |
| AsyncCallback<**void**> | callback            | No  | Callback used to return the result. If the operation is successful, **null** is returned; otherwise, a specific error code is returned.                                            |

**Error codes**

For details about the error codes, see [zlib Error Codes](../errorcodes/errorcode-zlib.md).

| ID| Error Message                              |
| -------- | --------------------------------------|
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |

**Example**

```typescript
// The path used in the code must be an application sandbox path, for example, /data/storage/el2/base/haps. You can obtain the path through the context.
import zlib from '@ohos.zlib';
import { BusinessError } from '@ohos.base';

let inFile = '/xxx/filename.xxx';
let outFile = '/xxx/xxx.zip';
let options: zlib.Options = {
  level: zlib.CompressLevel.COMPRESS_LEVEL_DEFAULT_COMPRESSION,
  memLevel: zlib.MemLevel.MEM_LEVEL_DEFAULT,
  strategy: zlib.CompressStrategy.COMPRESS_STRATEGY_DEFAULT_STRATEGY
};

try {
    zlib.compressFile(inFile, outFile, options, (errData: BusinessError) => {
        if (errData !== null) {
            console.log(`errData is errCode:${errData.code}  message:${errData.message}`);
        }
    })
} catch(errData) {
    let code = (errData as BusinessError).code;
    let message = (errData as BusinessError).message;
    console.log(`errData is errCode:${code}  message:${message}`);
}
```

## zlib.compressFile<sup>9+</sup>

compressFile(inFile: string, outFile: string, options: Options): Promise\<void>;

Compresses a file. This API uses a promise to return the result.

**System capability**: SystemCapability.BundleManager.Zlib

**Parameters**

| Name | Type               | Mandatory| Description                                                        |
| ------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile  | string              | Yes  | Path of the folder or file to compress. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md).|
| outFile | string              | Yes  | Path of the compressed file.                                          |
| options | [Options](#options) | Yes  | Compression parameters.                                              |

**Error codes**

For details about the error codes, see [zlib Error Codes](../errorcodes/errorcode-zlib.md).

| ID| Error Message                              |
| -------- | ------------------------------------- |
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |

```typescript
// The path used in the code must be an application sandbox path, for example, /data/storage/el2/base/haps. You can obtain the path through the context.
import zlib from '@ohos.zlib';
import { BusinessError } from '@ohos.base';

let inFile = '/xxx/filename.xxx';
let outFile = '/xxx/xxx.zip';
let options: zlib.Options = {
  level: zlib.CompressLevel.COMPRESS_LEVEL_DEFAULT_COMPRESSION,
  memLevel: zlib.MemLevel.MEM_LEVEL_DEFAULT,
  strategy: zlib.CompressStrategy.COMPRESS_STRATEGY_DEFAULT_STRATEGY
};

try {
    zlib.compressFile(inFile, outFile, options).then((data: void) => {
        console.info('compressFile success');
    }).catch((errData: BusinessError) => {
        console.log(`errData is errCode:${errData.code}  message:${errData.message}`);
    })
} catch(errData) {
    let code = (errData as BusinessError).code;
    let message = (errData as BusinessError).message;
    console.log(`errData is errCode:${code}  message:${message}`);
}
```

## zlib.decompressFile<sup>9+</sup>

decompressFile(inFile: string, outFile: string, options: Options, callback: AsyncCallback\<void>): void;

Decompresses a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.BundleManager.Zlib

**Parameters**

| Name                 | Type               | Mandatory| Description                                                        |
| ----------------------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile                  | string              | Yes  | Path of the file to decompress. The file name extension must be .zip. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md).|
| outFile                 | string              | Yes  | Path of the decompressed file. The path must exist in the system. Otherwise, the decompression fails. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md). If a file or folder with the same name already exists in the path, they will be overwritten.|
| options                 | [Options](#options) | Yes  | Decompression parameters.                                            |
| AsyncCallback<**void**> | callback            | Yes  | Callback used to return the result. If the operation is successful, **null** is returned; otherwise, a specific error code is returned.                                            |

**Error codes**

For details about the error codes, see [zlib Error Codes](../errorcodes/errorcode-zlib.md).

| ID| Error Message                              |
| -------- | --------------------------------------|
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |
| 900003 | The input source file is not ZIP format or damaged. |

**Example**

```typescript
// The path used in the code must be an application sandbox path, for example, /data/storage/el2/base/haps. You can obtain the path through the context.
import zlib from '@ohos.zlib';
import { BusinessError } from '@ohos.base';

let inFile = '/xx/xxx.zip';
let outFileDir = '/xxx';
let options: zlib.Options = {
  level: zlib.CompressLevel.COMPRESS_LEVEL_DEFAULT_COMPRESSION
};

try {
    zlib.decompressFile(inFile, outFileDir, options, (errData: BusinessError) => {
        if (errData !== null) {
            console.log(`errData is errCode:${errData.code}  message:${errData.message}`);
        }
    })
} catch(errData) {
    let code = (errData as BusinessError).code;
    let message = (errData as BusinessError).message;
    console.log(`errData is errCode:${code}  message:${message}`);
}
```

## zlib.decompressFile<sup>9+</sup>

decompressFile(inFile: string, outFile: string, options?: Options): Promise\<void>;

Decompresses a file. This API uses a promise to return the result.

**System capability**: SystemCapability.BundleManager.Zlib

**Parameters**

| Name | Type               | Mandatory| Description                                                        |
| ------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile  | string              | Yes  | Path of the file to decompress. The file name extension must be .zip. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md).|
| outFile | string              | Yes  | Path of the decompressed file. The path must exist in the system. Otherwise, the decompression fails. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md). If a file or folder with the same name already exists in the path, they will be overwritten.|
| options | [Options](#options) | No  | Decompression parameters.                                          |

**Error codes**

For details about the error codes, see [zlib Error Codes](../errorcodes/errorcode-zlib.md).

| ID| Error Message                              |
| ------ | ------------------------------------- |
| 900001 | The input source file is invalid.      |
| 900002 | The input destination file is invalid. |
| 900003 | The input source file is not ZIP format or damaged. |

```typescript
// The path used in the code must be an application sandbox path, for example, /data/storage/el2/base/haps. You can obtain the path through the context.
import zlib from '@ohos.zlib';
import { BusinessError } from '@ohos.base';

let inFile = '/xx/xxx.zip';
let outFileDir = '/xxx';
let options: zlib.Options = {
  level: zlib.CompressLevel.COMPRESS_LEVEL_DEFAULT_COMPRESSION
};

try {
    zlib.decompressFile(inFile, outFileDir, options).then((data: void) => {
        console.info('decompressFile success');
    }).catch((errData: BusinessError) => {
        console.log(`errData is errCode:${errData.code}  message:${errData.message}`);
    })
} catch(errData) {
    let code = (errData as BusinessError).code;
    let message = (errData as BusinessError).message;
    console.log(`errData is errCode:${code}  message:${message}`);
}
```

## zlib.decompressFile<sup>10+</sup>

decompressFile(inFile: string, outFile: string, callback: AsyncCallback\<void\>): void;

Decompresses a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.BundleManager.Zlib

**Parameters**

| Name                 | Type               | Mandatory| Description                                                        |
| ----------------------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile                  | string              | Yes  | Path of the file to decompress. The file name extension must be .zip. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md).|
| outFile                 | string              | Yes  | Path of the decompressed file. The path must exist in the system. Otherwise, the decompression fails. The path must be an application sandbox path, which can be obtained through [context](js-apis-inner-application-context.md). If a file or folder with the same name already exists in the path, they will be overwritten.|
| AsyncCallback<**void**> | callback            | Yes  | Callback used to return the result. If the operation is successful, **null** is returned; otherwise, a specific error code is returned.                                            |

**Error codes**

For details about the error codes, see [zlib Error Codes](../errorcodes/errorcode-zlib.md).

| ID| Error Message                              |
| -------- | --------------------------------------|
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |
| 900003 | The input source file is not ZIP format or damaged. |

**Example**

```typescript
// The path used in the code must be an application sandbox path, for example, /data/storage/el2/base/haps. You can obtain the path through the context.
import zlib from '@ohos.zlib';
import { BusinessError } from '@ohos.base';
let inFile = '/xx/xxx.zip';
let outFileDir = '/xxx';

try {
    zlib.decompressFile(inFile, outFileDir, (errData: BusinessError) => {
        if (errData !== null) {
            console.log(`decompressFile failed. code is ${errData.code}, message is ${errData.message}`);
        }
    })
} catch(errData) {
    let code = (errData as BusinessError).code;
    let message = (errData as BusinessError).message;
    console.log(`decompressFile failed. code is ${code}, message is ${message}`);
}
```

## Options

**System capability**: SystemCapability.BundleManager.Zlib

| Name    | Type            | Readable| Writable| Description                                                      |
| -------- | ---------------- | ---- | ---- | ---------------------------------------------------------- |
| level    | CompressLevel     | Yes  | No  | See [zip.CompressLevel](#zipcompresslevel).      |
| memLevel | MemLevel         | Yes  | No  | See [zip.MemLevel](#zipmemlevel).                |
| strategy | CompressStrategy | Yes  | No  | See [zip.CompressStrategy](#zipcompressstrategy).|

## zip.CompressLevel

**System capability**: SystemCapability.BundleManager.Zlib

| Name                              | Value  | Description             |
| ---------------------------------- | ---- | ----------------- |
| COMPRESS_LEVEL_NO_COMPRESSION      | 0    | Compress level 0 that indicates uncompressed.|
| COMPRESS_LEVEL_BEST_SPEED          | 1    | Compression level 1 that gives the best speed. |
| COMPRESS_LEVEL_BEST_COMPRESSION    | 9    | Compression level 9 that gives the best compression.     |
| COMPRESS_LEVEL_DEFAULT_COMPRESSION | -1   | Default compression level.     |

## zip.MemLevel

**System capability**: SystemCapability.BundleManager.Zlib

| Name             | Value  | Description                            |
| ----------------- | ---- | -------------------------------- |
| MEM_LEVEL_MIN     | 1    | Minimum memory used by the **zip** API during compression.|
| MEM_LEVEL_MAX     | 9    | Maximum memory used by the **zip** API during compression.|
| MEM_LEVEL_DEFAULT | 8    | Default memory used by the **zip** API during compression.|

## zip.CompressStrategy

**System capability**: SystemCapability.BundleManager.Zlib

| Name                              | Value  | Description                    |
| ---------------------------------- | ---- | ------------------------ |
| COMPRESS_STRATEGY_DEFAULT_STRATEGY | 0    | Default compression strategy.            |
| COMPRESS_STRATEGY_FILTERED         | 1    | Filtered compression strategy.|
| COMPRESS_STRATEGY_HUFFMAN_ONLY     | 2    | Huffman coding compression strategy.  |
| COMPRESS_STRATEGY_RLE              | 3    | RLE compression strategy.        |
| COMPRESS_STRATEGY_FIXED            | 4    | Fixed compression strategy.          |
