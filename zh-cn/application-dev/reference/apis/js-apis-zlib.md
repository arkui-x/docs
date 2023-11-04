# @ohos.zlib (Zip模块)

本模块提供压缩解压缩文件的能力

> **说明：**
>
> 本模块首批接口从API version 7开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```javascript
import zlib from '@ohos.zlib';
```

## zlib.compressFile<sup>9+</sup>

compressFile(inFile: string, outFile: string, options: Options, callback: AsyncCallback\<void>): void;

压缩文件，压缩的结果通过callback返回。成功时返回null，失败时返回错误码ID。

**系统能力：** SystemCapability.BundleManager.Zlib

**参数：**

| 参数名                  | 类型                | 必填 | 说明                                                         |
| ----------------------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile                  | string              | 是   | 指定压缩的文件夹路径或者文件路径，路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。 |
| outFile                 | string              | 是   | 指定的压缩结果的文件路径。                                           |
| options                 | [Options](#options) | 是   | 压缩的配置参数。                                               |
| AsyncCallback<**void**> | callback            | 否   | 压缩时的回调函数。                                             |

**错误码：**

以下错误码的详细介绍请参见[ohos.zlib错误码](../errorcodes/errorcode-zlib.md)。

| 错误码ID | 错误信息                               |
| -------- | --------------------------------------|
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |

**示例**

```typescript
// 代码中使用的路径需为应用的沙箱路径，如/data/storage/el2/base/haps,也可以通过context获取
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

压缩文件，压缩的结果通过promise返回，成功时返回null，失败时返回错误码。

**系统能力：** SystemCapability.BundleManager.Zlib

**参数：**

| 参数名  | 类型                | 必填 | 说明                                                         |
| ------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile  | string              | 是   | 指定压缩的文件夹路径或者文件路径，路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。 |
| outFile | string              | 是   | 指定的压缩结果的文件路径。                                           |
| options | [Options](#options) | 是   | 压缩的配置参数。                                               |

**错误码：**

以下错误码的详细介绍请参见[ohos.zlib错误码](../errorcodes/errorcode-zlib.md)。

| 错误码ID | 错误信息                               |
| -------- | ------------------------------------- |
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |

```typescript
// 代码中使用的路径需为应用的沙箱路径，如/data/storage/el2/base/haps,也可以通过context获取
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

解压文件，解压的结果通过callback返回，成功时返回null，失败时返回错误码。

**系统能力：** SystemCapability.BundleManager.Zlib

**参数：**

| 参数名                  | 类型                | 必填 | 说明                                                         |
| ----------------------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile                  | string              | 是   | 指定的待解压缩文件的文件路径，文件后缀需要以.zip结尾。文件路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。 |
| outFile                 | string              | 是   | 指定的解压后的文件夹路径，文件夹目录路径需要在系统中存在，不存在则会解压失败。路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。如果待解压的文件或文件夹在解压后的路径下已经存在，则会直接覆盖同名文件或同名文件夹中的同名文件。 |
| options                 | [Options](#options) | 是   | 解压的配置参数。                                             |
| AsyncCallback<**void**> | callback            | 是   | 解压的回调函数。                                             |

**错误码：**

以下错误码的详细介绍请参见[ohos.zlib错误码](../errorcodes/errorcode-zlib.md)。

| 错误码ID | 错误信息                               |
| -------- | --------------------------------------|
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |
| 900003 | The input source file is not ZIP format or damaged. |

**示例**

```typescript
// 代码中使用的路径需为应用的沙箱路径，如/data/storage/el2/base/haps,也可以通过context获取
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

解压文件，解压的结果通过promise返回，成功时返回null，失败时返回错误码。

**系统能力：** SystemCapability.BundleManager.Zlib

**参数：**

| 参数名  | 类型                | 必填 | 说明                                                         |
| ------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile  | string              | 是   | 指定的待解压缩文件的文件路径，文件后缀需要以.zip结尾。文件路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。 |
| outFile | string              | 是   | 指定的解压后的文件夹路径，文件夹目录路径需要在系统中存在，不存在则会解压失败。路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。如果待解压的文件或文件夹在解压后的路径下已经存在，则会直接覆盖同名文件或同名文件夹中的同名文件。 |
| options | [Options](#options) | 否   | 解压时的配置参数。                                           |

**错误码：**

以下错误码的详细介绍请参见[ohos.zlib错误码](../errorcodes/errorcode-zlib.md)。

| 错误码ID | 错误信息                               |
| ------ | ------------------------------------- |
| 900001 | The input source file is invalid.      |
| 900002 | The input destination file is invalid. |
| 900003 | The input source file is not ZIP format or damaged. |

```typescript
// 代码中使用的路径需为应用的沙箱路径，如/data/storage/el2/base/haps,也可以通过context获取
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

解压文件，解压的结果通过callback返回，成功时返回null，失败时返回错误码。

**系统能力：** SystemCapability.BundleManager.Zlib

**参数：**

| 参数名                  | 类型                | 必填 | 说明                                                         |
| ----------------------- | ------------------- | ---- | ------------------------------------------------------------ |
| inFile                  | string              | 是   | 指定的待解压缩文件的文件路径，文件后缀需要以.zip结尾。文件路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。 |
| outFile                 | string              | 是   | 指定的解压后的文件夹路径，文件夹目录路径需要在系统中存在，不存在则会解压失败。路径必须为沙箱路径，沙箱路径可以通过[context](js-apis-inner-application-context.md)获取。如果待解压的文件或文件夹在解压后的路径下已经存在，则会直接覆盖同名文件或同名文件夹中的同名文件。 |
| AsyncCallback<**void**> | callback            | 是   | 解压的回调函数。                                             |

**错误码：**

以下错误码的详细介绍请参见[ohos.zlib错误码](../errorcodes/errorcode-zlib.md)。

| 错误码ID | 错误信息                               |
| -------- | --------------------------------------|
| 900001   | The input source file is invalid.      |
| 900002   | The input destination file is invalid. |
| 900003 | The input source file is not ZIP format or damaged. |

**示例**

```typescript
// 代码中使用的路径需为应用的沙箱路径，如/data/storage/el2/base/haps,也可以通过context获取
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

**系统能力：** SystemCapability.BundleManager.Zlib

| 名称     | 类型             | 可读 | 可写 | 说明                                                       |
| -------- | ---------------- | ---- | ---- | ---------------------------------------------------------- |
| level    | CompressLevel     | 是   | 否   | 参考[zip.CompressLevel枚举定义](#zipcompresslevel)。       |
| memLevel | MemLevel         | 是   | 否   | 参考[zip.MemLevel枚举定义](#zipmemlevel)。                 |
| strategy | CompressStrategy | 是   | 否   | 参考[zip.CompressStrategy枚举定义](#zipcompressstrategy)。 |

## zip.CompressLevel

**系统能力：** SystemCapability.BundleManager.Zlib

| 名称                               | 值   | 说明              |
| ---------------------------------- | ---- | ----------------- |
| COMPRESS_LEVEL_NO_COMPRESSION      | 0    | 压缩率为0压缩等级。 |
| COMPRESS_LEVEL_BEST_SPEED          | 1    | 最佳速度压缩等级。  |
| COMPRESS_LEVEL_BEST_COMPRESSION    | 9    | 最佳压缩等级。      |
| COMPRESS_LEVEL_DEFAULT_COMPRESSION | -1   | 默认压缩等级。      |

## zip.MemLevel

**系统能力：** SystemCapability.BundleManager.Zlib

| 名称              | 值   | 说明                             |
| ----------------- | ---- | -------------------------------- |
| MEM_LEVEL_MIN     | 1    | zip 接口在压缩过程中最小使用内存。 |
| MEM_LEVEL_MAX     | 9    | zip 接口在压缩过程中最大使用内存。 |
| MEM_LEVEL_DEFAULT | 8    | zip 接口在压缩过程中默认使用内存。 |

## zip.CompressStrategy

**系统能力：** SystemCapability.BundleManager.Zlib

| 名称                               | 值   | 说明                     |
| ---------------------------------- | ---- | ------------------------ |
| COMPRESS_STRATEGY_DEFAULT_STRATEGY | 0    | 常规数据策略。             |
| COMPRESS_STRATEGY_FILTERED         | 1    | 过滤器产生的数据压缩策略。 |
| COMPRESS_STRATEGY_HUFFMAN_ONLY     | 2    | 霍夫曼编码格式压缩策略。   |
| COMPRESS_STRATEGY_RLE              | 3    | 游标编码压缩策略。         |
| COMPRESS_STRATEGY_FIXED            | 4    | 固定的压缩策略。           |
