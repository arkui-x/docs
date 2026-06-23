# @ohos.file.storageStatistics (应用空间统计)
该模块提供空间查询相关的常用功能：包括对内外卡的空间查询、对应用分类数据统计的查询、对应用数据的查询等。

> **说明：**
>
> 本模块首批接口从API version 26开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { storageStatistics } from '@kit.CoreFileKit';
```

## storageStatistics.getCurrentBundleStats

getCurrentBundleStats(): Promise&lt;BundleStats&gt;

应用异步获取当前应用存储空间大小（单位为Byte），使用Promise异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**返回值：**

| 类型                                       | 说明                                                        |
| ------------------------------------------ | ----------------------------------------------------------- |
| Promise&lt;[BundleStats](#bundlestats)&gt; | Promise对象，返回指定卷上的应用存储空间大小（单位为Byte）。 |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)和[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401 | The input parameter is invalid. Possible causes: Mandatory parameters are left unspecified. |
| 13600001 | IPC error. |
| 13900042 | Unknown error. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
storageStatistics.getCurrentBundleStats().then((bundleStats: storageStatistics.BundleStats) => {
  console.info("getCurrentBundleStats successfully:" + JSON.stringify(bundleStats));
}).catch((err: BusinessError) => {
  console.error("getCurrentBundleStats failed with error:"+ JSON.stringify(err));
});
```

## storageStatistics.getCurrentBundleStats

getCurrentBundleStats(callback: AsyncCallback&lt;BundleStats&gt;): void

应用异步获取当前应用存储空间大小（单位为Byte），使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**参数：**

| 参数名    | 类型                                                       | 必填  | 说明                                 |
| -------- | --------------------------------------------------------- | ---- | ------------------------------------ |
| callback | AsyncCallback&lt;[BundleStats](#bundlestats)&gt;          | 是   | 获取指定卷上的应用存储空间大小之后的回调。        |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)和[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401 | The input parameter is invalid. Possible causes: Mandatory parameters are left unspecified. |
| 13600001 | IPC error. |
| 13900042 | Unknown error. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
storageStatistics.getCurrentBundleStats((error: BusinessError, bundleStats: storageStatistics.BundleStats) => {
  if (error) {
    console.error("getCurrentBundleStats failed with error:" + JSON.stringify(error));
  } else {
    // do something
    console.info("getCurrentBundleStats successfully:" + JSON.stringify(bundleStats));
  }
});
```

## storageStatistics.getTotalSize

getTotalSize(): Promise&lt;long&gt;

获取内置存储的总空间大小（单位为Byte），使用Promise异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**返回值：**

| 类型                  | 说明                                                |
| --------------------- | --------------------------------------------------- |
| Promise&lt;long&gt; | Promise对象，返回内置存储的总空间大小（单位为Byte）。 |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息       |
| -------- | -------------- |
| 13600001 | IPC error.     |
| 13900042 | Unknown error. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
storageStatistics.getTotalSize().then((number: number) => {
  console.info("getTotalSize successfully:" + JSON.stringify(number));
}).catch((err: BusinessError) => {
  console.error("getTotalSize failed with error:"+ JSON.stringify(err));
});
```

## storageStatistics.getTotalSize

getTotalSize(callback: AsyncCallback&lt;long&gt;): void

获取内置存储的总空间大小（单位为Byte），使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**参数：**

| 参数名   | 类型                        | 必填 | 说明                               |
| -------- | --------------------------- | ---- | ---------------------------------- |
| callback | AsyncCallback&lt;long&gt; | 是   | 获取内置存储的总空间大小之后的回调。 |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)和[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | The input parameter is invalid. Possible causes: Mandatory parameters are left unspecified. |
| 13600001 | IPC error.                                                   |
| 13900042 | Unknown error.                                               |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
storageStatistics.getTotalSize((error: BusinessError, number: number) => {
  if (error) {
    console.error("getTotalSize failed with error:" + JSON.stringify(error));
  } else {
    // do something
    console.info("getTotalSize successfully:" + number);
  }
});
```

## storageStatistics.getTotalSizeSync

getTotalSizeSync(): long

同步获取内置存储的总空间大小（单位为Byte）。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**返回值：**

| 类型   | 说明                                   |
| ------ | -------------------------------------- |
| long | 返回内置存储的总空间大小（单位为Byte）。 |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息       |
| -------- | -------------- |
| 13600001 | IPC error.     |
| 13900042 | Unknown error. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
try {
  let number = storageStatistics.getTotalSizeSync();
  console.info("getTotalSizeSync successfully:" + JSON.stringify(number));
} catch (err) {
  let error: BusinessError = err as BusinessError;
  console.error("getTotalSizeSync failed with error:" + JSON.stringify(error));
}
```

## storageStatistics.getFreeSize

getFreeSize(): Promise&lt;long&gt;

获取内置存储的可用空间大小（单位为Byte），使用Promise异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**返回值：**

| 类型                  | 说明                                                  |
| --------------------- | ----------------------------------------------------- |
| Promise&lt;long&gt; | Promise对象，返回内置存储的可用空间大小（单位为Byte）。 |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息       |
| -------- | -------------- |
| 13600001 | IPC error.     |
| 13900042 | Unknown error. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
storageStatistics.getFreeSize().then((number: number) => {
  console.info("getFreeSize successfully:" + JSON.stringify(number));
}).catch((err: BusinessError) => {
  console.error("getFreeSize failed with error:" + JSON.stringify(err));
});
```

## storageStatistics.getFreeSize

getFreeSize(callback: AsyncCallback&lt;long&gt;): void

获取内置存储的可用空间大小（单位为Byte），使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                 |
| -------- | --------------------------- | ---- | ------------------------------------ |
| callback | AsyncCallback&lt;long&gt; | 是   | 获取内置存储的可用空间大小之后的回调。 |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)和[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | The input parameter is invalid. Possible causes: Mandatory parameters are left unspecified. |
| 13600001 | IPC error.                                                   |
| 13900042 | Unknown error.                                               |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
storageStatistics.getFreeSize((error: BusinessError, number: number) => {
  if (error) {
    console.error("getFreeSize failed with error:" + JSON.stringify(error));
  } else {
    // do something
    console.info("getFreeSize successfully:" + number);
  }
});
```

## storageStatistics.getFreeSizeSync

getFreeSizeSync(): long

同步获取内置存储的可用空间大小（单位为Byte）。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

**返回值：**

| 类型   | 说明                                     |
| ------ | ---------------------------------------- |
| long | 返回内置存储的可用空间大小（单位为Byte）。 |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息       |
| -------- | -------------- |
| 13600001 | IPC error.     |
| 13900042 | Unknown error. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
try {
  let number = storageStatistics.getFreeSizeSync();
  console.info("getFreeSizeSync successfully:" + JSON.stringify(number));
} catch (err) {
  let error: BusinessError = err as BusinessError;
  console.error("getFreeSizeSync failed with error:" + JSON.stringify(error));
}
```

## BundleStats

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**支持平台**：Android、iOS 

| 名称                    | 类型                                          | 只读 | 可选 | 说明                                       | Android平台                              | iOS平台                                  |
| :---------------------- |---------------------------------------------| ---- | ---- | ------------------------------------------ | ------------------------------------------ | ------------------------------------------ |
| appSize   | long  | 否 | 否 | 应用安装文件大小（单位为Byte）。    | 支持  | 支持  |
| cacheSize | long  | 否 | 否  | 应用缓存文件大小（单位为Byte）。   | 支持 | 支持 |
| dataSize  | long  | 否 | 否  | 应用文件存储大小（除应用安装文件）（单位为Byte）。 | 支持 | 支持 |

## storageStatistics.getTotalInodes

getTotalInodes(): Promise&lt;long&gt;

获取文件系统的inode资源总量，仅支持查询系统数据分区。使用Promise异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**模型约束**：此接口仅可在Stage模型下使用。

**支持平台**：Android、iOS 

**返回值：**

| 类型                  | 说明                                                  |
| --------------------- | ----------------------------------------------------- |
| Promise&lt;long&gt; | Promise对象，返回文件系统inode资源总量。                |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息                                                      |
| -------- | ------------------------------------------------------------ |
| 13600001 | IPC error.                                                   |
| 13600016 | Failed to query the inode information of the data partition. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

storageStatistics.getTotalInodes().then((totalInodes: number) => {
  console.info("getTotalInodes successfully: " + totalInodes);
}).catch((err: BusinessError) => {
  console.error(`getTotalInodes failed. Code: ${err.code}, Message: ${err.message}`);
});
```

## storageStatistics.getFreeInodes

getFreeInodes(): Promise&lt;long&gt;

获取文件系统的inode资源剩余量，仅支持查询系统数据分区。使用Promise异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**模型约束**：此接口仅可在Stage模型下使用。

**支持平台**：Android、iOS 

**返回值：**

| 类型                  | 说明                                                  |
| --------------------- | ----------------------------------------------------- |
| Promise&lt;long&gt; | Promise对象，返回文件系统inode资源剩余量。               |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息                                                      |
| -------- | ------------------------------------------------------------ |
| 13600001 | IPC error.                                                   |
| 13600016 | Failed to query the inode information of the data partition. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

storageStatistics.getFreeInodes().then((freeInodes: number) => {
  console.info("getFreeInodes successfully: " + freeInodes);
}).catch((err: BusinessError) => {
  console.error(`getFreeInodes failed. Code: ${err.code}, Message: ${err.message}`);
});
```

## storageStatistics.getCurrentBundleInodes

getCurrentBundleInodes(): Promise&lt;long&gt;

获取当前应用的inode占用量，使用Promise异步回调。

**系统能力**：SystemCapability.FileManagement.StorageService.SpatialStatistics

**模型约束**：此接口仅可在Stage模型下使用。

**支持平台**：Android、iOS 

**返回值：**

| 类型                  | 说明                                                  |
| --------------------- | ----------------------------------------------------- |
| Promise&lt;long&gt; | Promise对象，返回当前应用的inode占用量。               |

**错误码：**

以下错误码的详细介绍请参见[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息                                                      |
| -------- | ------------------------------------------------------------ |
| 13600001 | IPC error.                                                   |
| 13600002 | File system not supported.                                   |
| 13600017 | Failed to query the inode information of the application.    |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

storageStatistics.getCurrentBundleInodes().then((curInodes: number) => {
  console.info("getCurrentBundleInodes successfully: " + curInodes);
}).catch((err: BusinessError) => {
  console.error(`getCurrentBundleInodes failed. Code: ${err.code}, Message: ${err.message}`);
});
```