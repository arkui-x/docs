# @ohos.request (上传下载)

request部件主要给应用提供上传下载文件、后台传输代理的基础能力。

> **说明：**
>
> 本模块首批接口从API version 6开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块


```js
import request from '@ohos.request';
```

## 常量

**需要权限**：ohos.permission.INTERNET

**系统能力**: 以下各项对应的系统能力均为SystemCapability.MiscServices.Download。

### 网络类型
下载支持自定义网络类型，可以在[DownloadConfig](#downloadconfig)中通过networkType配置成以下网络类型。

| 名称 | 参数类型 | 数值 | 说明 |
| -------- | -------- | -------- | -------- |
| NETWORK_MOBILE | number | 0x00000001 | 使用蜂窝网络时允许下载的位标志。 |
| NETWORK_WIFI | number | 0x00010000 | 使用WLAN时允许下载的位标志。 |

### 下载任务的错误码
下载[on('fail')<sup>7+</sup>](#onfail7)事件callback的错误参数、[getTaskInfo<sup>9+</sup>](#gettaskinfo9)返回值的failedReason字段取值。

| 名称 | 参数类型 | 数值 | 说明 |
| -------- | -------- | -------- | -------- |
| ERROR_CANNOT_RESUME<sup>7+</sup> | number |   0   | 网络原因导致恢复下载失败。 |
| ERROR_DEVICE_NOT_FOUND<sup>7+</sup> | number |   1   | 找不到SD卡等存储设备。 |
| ERROR_FILE_ALREADY_EXISTS<sup>7+</sup> | number |   2   | 要下载的文件已存在，下载会话不能覆盖现有文件。 |
| ERROR_FILE_ERROR<sup>7+</sup> | number |   3   | 文件操作失败。 |
| ERROR_HTTP_DATA_ERROR<sup>7+</sup> | number |   4   | HTTP传输失败。 |
| ERROR_INSUFFICIENT_SPACE<sup>7+</sup> | number |   5   | 存储空间不足。 |
| ERROR_TOO_MANY_REDIRECTS<sup>7+</sup> | number |   6   | 网络重定向过多导致的错误。 |
| ERROR_UNHANDLED_HTTP_CODE<sup>7+</sup> | number |   7   | 无法识别的HTTP代码。 |
| ERROR_UNKNOWN<sup>7+</sup> | number |   8   | 未知错误。 |
| ERROR_OFFLINE<sup>9+</sup> | number |   9   | 网络未连接。 |
| ERROR_UNSUPPORTED_NETWORK_TYPE<sup>9+</sup> | number |   10   | 网络类型不匹配。 |


### 下载任务暂停原因
下载相关[getTaskInfo<sup>9+</sup>](#gettaskinfo9)返回值的pausedReason字段取值。

| 名称 | 参数类型 | 数值 | 说明 |
| -------- | -------- | -------- | -------- |
| PAUSED_QUEUED_FOR_WIFI<sup>7+</sup> | number |   0   | 下载被暂停并等待WLAN连接，因为文件大小超过了使用蜂窝网络的会话允许的最大值。 |
| PAUSED_WAITING_FOR_NETWORK<sup>7+</sup> | number |   1   | 由于网络问题（例如网络断开）而暂停下载。 |
| PAUSED_WAITING_TO_RETRY<sup>7+</sup> | number |   2   | 发生网络错误，将重试下载会话。 |
| PAUSED_BY_USER<sup>9+</sup> | number |   3   | 用户暂停会话。 |
| PAUSED_UNKNOWN<sup>7+</sup> | number |   4   | 未知原因导致暂停下载。 |

### 下载任务状态码
下载相关[getTaskInfo<sup>9+</sup>](#gettaskinfo9)返回值的status字段取值。

| 名称 | 参数类型 | 数值 | 说明 |
| -------- | -------- | -------- | -------- |
| SESSION_SUCCESSFUL<sup>7+</sup> | number |   0   | 下载会话已完成。 |
| SESSION_RUNNING<sup>7+</sup> | number |   1   | 下载会话正在进行中。 |
| SESSION_PENDING<sup>7+</sup> | number |   2   | 正在调度下载会话。 |
| SESSION_PAUSED<sup>7+</sup> | number |   3   | 下载会话已暂停。 |
| SESSION_FAILED<sup>7+</sup> | number |   4   | 下载会话已失败，将不会重试。 |


## request.uploadFile<sup>9+</sup>

uploadFile(context: BaseContext, config: UploadConfig): Promise&lt;UploadTask&gt;

上传，异步方法，使用promise形式返回结果。通过[on('complete'|'fail')<sup>9+</sup>](#oncomplete--fail9)可获取任务上传时的错误信息。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | context | [BaseContext](js-apis-inner-application-baseContext.md) | 是 | 基于应用程序的上下文。 |
  | config | [UploadConfig](#uploadconfig) | 是 | 上传的配置信息。 |


**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;[UploadTask](#uploadtask)&gt; | 返回上传任务。 |

**错误码：**

以下错误码的详细介绍请参见[上传下载错误码](../errorcodes/errorcode-request.md)。

  | 错误码ID | 错误信息 |
  | -------- | -------- |
  | 13400002 | bad file path. |

**示例：**

  ```js
  let uploadTask;
  let context;
  let uploadConfig = {
    url: 'http://patch',
    header: { key1: "value1", key2: "value2" },
    method: "POST",
    files: [{ filename: "test", name: "test", uri: "internal://cache/test.jpg", type: "jpg" }],
    data: [{ name: "name123", value: "123" }],
  };
  try {
    request.uploadFile(context, uploadConfig).then((data) => {
      uploadTask = data;
    }).catch((err) => {
      console.error(`Failed to request the upload. Code: ${err.code}, message: ${err.message}`);
    });
  } catch (err) {
    console.error(`Failed to request the upload. Code: ${err.code}, message: ${err.message}`);
  }
  ```

> **说明：**
>
> 示例中context的获取方式请参见[获取UIAbility的上下文信息](../../application-models/uiability-usage.md#获取uiability的上下文信息)。


## request.uploadFile<sup>9+</sup>

uploadFile(context: BaseContext, config: UploadConfig, callback: AsyncCallback&lt;UploadTask&gt;): void

上传，异步方法，使用callback形式返回结果。通过[on('complete'|'fail')<sup>9+</sup>](#oncomplete--fail9)可获取任务上传时的错误信息。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | context | [BaseContext](js-apis-inner-application-baseContext.md) | 是 | 基于应用程序的上下文。 |
  | config | [UploadConfig](#uploadconfig) | 是 | 上传的配置信息。 |
  | callback | AsyncCallback&lt;[UploadTask](#uploadtask)&gt; | 是 | 回调函数，异步返回UploadTask对象。 |

**错误码：**

以下错误码的详细介绍请参见[上传下载错误码](../errorcodes/errorcode-request.md)。

  | 错误码ID | 错误信息 |
  | -------- | -------- |
  | 13400002 | bad file path. |

**示例：**

  ```js
  let uploadTask;
  let context;
  let uploadConfig = {
    url: 'http://patch',
    header: { key1: "value1", key2: "value2" },
    method: "POST",
    files: [{ filename: "test", name: "test", uri: "internal://cache/test.jpg", type: "jpg" }],
    data: [{ name: "name123", value: "123" }],
  };
  try {
    request.uploadFile(context, uploadConfig, (err, data) => {
      if (err) {
        console.error(`Failed to request the upload. Code: ${err.code}, message: ${err.message}`);
        return;
      }
      uploadTask = data;
    });
  } catch (err) {
    console.error(`Failed to request the upload. Code: ${err.code}, message: ${err.message}`);
  }
  ```

> **说明：**
>
> 示例中context的获取方式请参见[获取UIAbility的上下文信息](../../application-models/uiability-usage.md#获取uiability的上下文信息)。

## UploadTask

上传任务，使用下列方法前，需要先获取UploadTask对象，promise形式通过[request.uploadFile<sup>9+</sup>](#requestuploadfile9)获取，callback形式通过[request.uploadFile<sup>9+</sup>](#requestuploadfile9-1)获取。



### on('progress')

on(type: 'progress', callback:(uploadedSize: number, totalSize: number) =&gt; void): void

订阅上传任务进度监听，同步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 订阅的事件类型，取值为'progress'（上传的进度信息）。 |
  | callback | function | 是 | 上传进度的回调函数。 |

  回调函数的参数

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| uploadedSize | number | 是 | 当前已上传文件大小，单位为B。 |
| totalSize | number | 是 | 上传文件的总大小，单位为B。 |

**示例：**

  ```js
  let upProgressCallback = (uploadedSize, totalSize) => {
    console.info("upload totalSize:" + totalSize + "  uploadedSize:" + uploadedSize);
  };
  uploadTask.on('progress', upProgressCallback);
  ```


### on('headerReceive')<sup>7+</sup>

on(type: 'headerReceive', callback:  (header: object) =&gt; void): void

订阅上传任务HTTP标头监听，同步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 订阅的事件类型，取值为'headerReceive'（接收响应头）。 |
  | callback | function | 是 | HTTP&nbsp;Response&nbsp;Header事件的回调函数。 |

  回调函数的参数：

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| header | object | 是 | HTTP&nbsp;Response&nbsp;Header。 |

**示例：**

  ```js
  let headerCallback = (headers) => {
    console.info("upOnHeader headers:" + JSON.stringify(headers));
  };
  uploadTask.on('headerReceive', headerCallback);
  ```


### on('complete' | 'fail')<sup>9+</sup>

 on(type:'complete' | 'fail', callback: Callback&lt;Array&lt;TaskState&gt;&gt;): void;

订阅上传任务完成或失败监听，同步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 订阅的事件类型，取值为'complete'，表示上传任务完成；取值为'fail'，表示上传任务失败。|
  | callback | Callback&lt;Array&lt;TaskState&gt;&gt; | 是 | 上传任务完成或失败的回调函数。 |

  回调函数的参数

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| taskstates | Array&lt;[TaskState](#taskstate9)&gt; | 是 | 上传任务返回结果 |

**示例：**

  ```js
  let upCompleteCallback = (taskStates) => {
    for (let i = 0; i < taskStates.length; i++ ) {
      console.info("upOnComplete taskState:" + JSON.stringify(taskStates[i]));
    }
  };
  uploadTask.on('complete', upCompleteCallback);

  let upFailCallback = (taskStates) => {
    for (let i = 0; i < taskStates.length; i++ ) {
      console.info("upOnFail taskState:" + JSON.stringify(taskStates[i]));
    }
  };
  uploadTask.on('fail', upFailCallback);
  ```


### off('progress')

off(type:  'progress',  callback?: (uploadedSize: number, totalSize: number) =&gt;  void): void

取消订阅上传任务进度监听，同步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 取消订阅的事件类型，取值为'progress'（上传的进度信息）。 |
  | callback | function | 否 | 需要删除的上传任务进度的回调函数。<br/>uploadedSize：当前已上传文件的大小，单位为B。<br/>totalSize：上传文件的总大小，单位为B。 |

**示例：**

  ```js
  let upProgressCallback = (uploadedSize, totalSize) => {
    console.info('Upload delete progress notification.' + 'totalSize:' + totalSize + 'uploadedSize:' + uploadedSize);
  };
  uploadTask.off('progress', upProgressCallback);
  ```


### off('headerReceive')<sup>7+</sup>

off(type: 'headerReceive', callback?: (header: object) =&gt; void): void

取消订阅上传任务HTTP标头监听，同步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 取消订阅的事件类型，取值为'headerReceive'（接收响应头）。 |
  | callback | function | 否 | HTTP&nbsp;Response&nbsp;需要删除的Header事件的回调函数。<br/>header：HTTP&nbsp;Response&nbsp;Header。 |

**示例：**

  ```js
  let headerCallback = (header) => {
    console.info(`Upload delete headerReceive notification. header: ${JSON.stringify(header)}`);
  };
  uploadTask.off('headerReceive', headerCallback);
  ```

### off('complete' | 'fail')<sup>9+</sup>

 off(type:'complete' | 'fail', callback?: Callback&lt;Array&lt;TaskState&gt;&gt;): void;

取消订阅上传任务完成或失败监听，同步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 订阅的事件类型，取值为'complete'，表示上传任务完成；取值为'fail'，表示上传任务失败。|
  | callback | Callback&lt;Array&lt;TaskState&gt;&gt; | 否 | 需要删除的上传任务完成或失败的回调函数。<br/>taskstates：上传任务返回结果 |

**示例：**

  ```js
  let upCompleteCallback = (taskStates) => {
    console.info('Upload delete complete notification.');
    for (let i = 0; i < taskStates.length; i++ ) {
      console.info('taskState:' + JSON.stringify(taskStates[i]));
    }
  };
  uploadTask.off('complete', upCompleteCallback);

  let upFailCallback = (taskStates) => {
    console.info('Upload delete fail notification.');
    for (let i = 0; i < taskStates.length; i++ ) {
      console.info('taskState:' + JSON.stringify(taskStates[i]));
    }
  };
  uploadTask.off('fail', upFailCallback);
  ```

### delete<sup>9+</sup>
delete(): Promise&lt;boolean&gt;

移除上传的任务，异步方法，使用promise形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;boolean&gt; | 移除任务是否成功。true：成功，false：不成功。 |

**示例：**

  ```js
  uploadTask.delete().then((result) => {
    if (result) {
      console.info('Succeeded in deleting the upload task.');
    } else {
      console.error(`Failed to delete the upload task. Code: ${err.code}, message: ${err.message}`);
    }
  }).catch((err) => {
    console.error(`Failed to delete the upload task. Code: ${err.code}, message: ${err.message}`);
  });
  ```


### delete<sup>9+</sup>

delete(callback: AsyncCallback&lt;boolean&gt;): void

移除上传的任务，异步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Upload

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | callback | AsyncCallback&lt;boolean&gt; | 是 | 移除任务的回调函数。 |

**示例：**

  ```js
  uploadTask.delete((err, result) => {
    if (err) {
      console.error(`Failed to delete the upload task. Code: ${err.code}, message: ${err.message}`);
      return;
    }
    if (result) {
      console.info('Succeeded in deleting the upload task.');
    } else {
      console.error(`Failed to delete the upload task. Code: ${err.code}, message: ${err.message}`);
    }
  });
  ```

## UploadConfig
上传任务的配置信息。

**需要权限**：ohos.permission.INTERNET

**系统能力**: 以下各项对应的系统能力均为SystemCapability.MiscServices.Upload。

| 名称 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| url | string | 是 | 资源地址。 |
| header | Object | 是 | 添加要包含在上传请求中的HTTP或HTTPS标志头。 |
| method | string | 是 | 请求方法：POST、PUT。缺省为POST。 |
| files | Array&lt;[File](#file)&gt; | 是 | 要上传的文件列表。请使用&nbsp;multipart/form-data提交。 |
| data | Array&lt;[RequestData](#requestdata)&gt; | 是 | 请求的表单数据。 |

## TaskState<sup>9+</sup>
上传任务信息，[on('complete' | 'fail')<sup>9+</sup>](#oncomplete--fail9)和[off('complete' | 'fail')<sup>9+</sup>](#offcomplete--fail9)接口的回调参数。

**需要权限**：ohos.permission.INTERNET

**系统能力**: 以下各项对应的系统能力均为SystemCapability.MiscServices.Upload。

| 名称 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| path | string | 是 | 文件路径 |
| responseCode | number | 是 | 上传任务返回值 |
| message | string | 是 | 上传任务结果描述信息 |

## File
[UploadConfig](#uploadconfig)中的文件列表。

**需要权限**：ohos.permission.INTERNET

**系统能力**: 以下各项对应的系统能力均为SystemCapability.MiscServices.Download。

| 名称 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| filename | string | 是 | multipart提交时，请求头中的文件名。 |
| name | string | 是 | multipart提交时，表单项目的名称，缺省为file。 |
| uri | string | 是 | 文件的本地存储路径。<br/>仅支持"internal"协议类型，"internal://cache/"为必填字段，示例：<br/>internal://cache/path/to/file.txt |
| type | string | 是 | 文件的内容类型，默认根据文件名或路径的后缀获取。 |


## RequestData
[UploadConfig](#uploadconfig)中的表单数据。

**需要权限**：ohos.permission.INTERNET

**系统能力**: 以下各项对应的系统能力均为SystemCapability.MiscServices.Download。

| 名称 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| name | string | 是 | 表示表单元素的名称。 |
| value | string | 是 | 表示表单元素的值。 |

## request.downloadFile<sup>9+</sup>

downloadFile(context: BaseContext, config: DownloadConfig): Promise&lt;DownloadTask&gt;

下载，异步方法，使用promise形式返回结果。通过[on('complete'|'pause'|'remove')<sup>7+</sup>](#oncompletepauseremove7)可获取任务下载时的状态信息，包括任务完成、暂停或移除。通过[on('fail')<sup>7+</sup>](#onfail7)可获取任务下载时的错误信息。


**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | context | [BaseContext](js-apis-inner-application-baseContext.md) | 是 | 基于应用程序的上下文。 |
  | config | [DownloadConfig](#downloadconfig) | 是 | 下载的配置信息。 |

**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;[DownloadTask](#downloadtask)&gt; | 返回下载任务。 |

**错误码：**

以下错误码的详细介绍请参见[上传下载错误码](../errorcodes/errorcode-request.md)。

  | 错误码ID | 错误信息 |
  | -------- | -------- |
  | 13400001 | file operation error. |
  | 13400002 | bad file path. |
  | 13400003 | task service ability error. |

**示例：**

  ```js
  let downloadTask;
  let context;
  try {
    request.downloadFile(context, { url: 'https://xxxx/xxxx.hap' }).then((data) => {
      downloadTask = data;
    }).catch((err) => {
      console.error(`Failed to request the download. Code: ${err.code}, message: ${err.message}`);
    })
  } catch (err) {
    console.error(`Failed to request the download. Code: ${err.code}, message: ${err.message}`);
  }
  ```

> **说明：**
>
> 示例中context的获取方式请参见[获取UIAbility的上下文信息](../../application-models/uiability-usage.md#获取uiability的上下文信息)。


## request.downloadFile<sup>9+</sup>

downloadFile(context: BaseContext, config: DownloadConfig, callback: AsyncCallback&lt;DownloadTask&gt;): void;

下载，异步方法，使用callback形式返回结果。通过[on('complete'|'pause'|'remove')<sup>7+</sup>](#oncompletepauseremove7)可获取任务下载时的状态信息，包括任务完成、暂停或移除。通过[on('fail')<sup>7+</sup>](#onfail7)可获取任务下载时的错误信息。


**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | context | [BaseContext](js-apis-inner-application-baseContext.md) | 是 | 基于应用程序的上下文。 |
  | config | [DownloadConfig](#downloadconfig) | 是 | 下载的配置信息。 |
  | callback | AsyncCallback&lt;[DownloadTask](#downloadtask)&gt; | 是 | 下载接口的回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[上传下载错误码](../errorcodes/errorcode-request.md)。

  | 错误码ID | 错误信息 |
  | -------- | -------- |
  | 13400001 | file operation error. |
  | 13400002 | bad file path. |
  | 13400003 | task service ability error. |

**示例：**

  ```js
  let downloadTask;
  let context;
  try {
    request.downloadFile(context, { url: 'https://xxxx/xxxxx.hap', 
    filePath: 'xxx/xxxxx.hap'}, (err, data) => {
      if (err) {
        console.error(`Failed to request the download. Code: ${err.code}, message: ${err.message}`);
        return;
      }
      downloadTask = data;
    });
  } catch (err) {
    console.error(`Failed to request the download. Code: ${err.code}, message: ${err.message}`);
  }
  ```

> **说明：**
>
> 示例中context的获取方式请参见[获取UIAbility的上下文信息](../../application-models/uiability-usage.md#获取uiability的上下文信息)。

## DownloadTask

下载任务，使用下列方法前，需要先获取DownloadTask对象，promise形式通过[request.downloadFile<sup>9+</sup>](#requestdownloadfile9)获取，callback形式通过[request.downloadFile<sup>9+</sup>](#requestdownloadfile9-1)获取。


### on('progress')

on(type: 'progress', callback:(receivedSize: number, totalSize: number) =&gt; void): void

订阅下载任务进度监听，同步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 订阅的事件类型，取值为'progress'（下载的进度信息）。 |
  | callback | function | 是 | 下载任务进度的回调函数。 |

  回调函数的参数：

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| receivedSize | number | 是 | 当前下载的进度，单位为B。 |
| totalSize | number | 是 | 下载文件的总大小，单位为B。 |

**示例：**

  ```js
  let progresCallback = (receivedSize, totalSize) => {
    console.info("download receivedSize:" + receivedSize + " totalSize:" + totalSize);
  };
  downloadTask.on('progress', progresCallback);
  ```


### off('progress')

off(type: 'progress', callback?: (receivedSize: number, totalSize: number) =&gt; void): void

取消订阅下载任务进度监听，同步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 取消订阅的事件类型，取值为'progress'（下载的进度信息）。 |
  | callback | function | 否 | 需要删除的下载任务进度的回调。 <br/>receivedSize：当前下载任务的进度；<br/>totalSize：下载文件的总大小。 |

**示例：**

  ```js
  let progresCallback = (receivedSize, totalSize) => {
    console.info('Download delete progress notification.' + 'receivedSize:' + receivedSize + 'totalSize:' + totalSize);
  };
  downloadTask.off('progress', progresCallback);
  ```


### on('complete'|'pause'|'remove')<sup>7+</sup>

on(type: 'complete'|'pause'|'remove', callback:() =&gt; void): void

订阅下载任务相关的监听，异步方法，使用callback形式返回。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 订阅的事件类型。<br>- 取值为'complete'，表示下载任务完成；<br/>- 取值为'pause'，表示下载任务暂停；<br/>- 取值为'remove'，表示下载任务移除。 |
  | callback | function | 是 | 下载任务相关的回调函数。|

**示例：**

  ```js
  let completeCallback = () => {
    console.info('Download task completed.');
  };
  downloadTask.on('complete', completeCallback);

  let pauseCallback = () => {
    console.info('Download task pause.');
  };
  downloadTask.on('pause', pauseCallback);

  let removeCallback = () => {
    console.info('Download task remove.');
  };
  downloadTask.on('remove', removeCallback);
  ```


### off('complete'|'pause'|'remove')<sup>7+</sup>

off(type: 'complete'|'pause'|'remove', callback?:() =&gt; void): void

取消订阅下载任务相关的监听，同步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 取消订阅的事件类型。<br/>- 取值为'complete'，表示下载任务完成；<br/>- 取值为'pause'，表示下载任务暂停；<br/>- 取值为'remove'，表示下载任务移除。 |
  | callback | function | 否 | 需要删除的下载任务相关的回调。 |

**示例：**

  ```js
  let completeCallback = () => {
    console.info('Download delete complete notification.');
  };
  downloadTask.off('complete', completeCallback);

  let pauseCallback = () => {
    console.info('Download delete pause notification.');
  };
  downloadTask.off('pause', pauseCallback);

  let removeCallback = () => {
    console.info('Download delete remove notification.');
  };
  downloadTask.off('remove', removeCallback);
  ```


### on('fail')<sup>7+</sup>

on(type: 'fail', callback: (err: number) =&gt; void): void

订阅下载任务失败监听，同步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 订阅的事件类型，取值为'fail'（下载失败）。 |
  | callback | function | 是 | 下载失败的回调函数。 |

  回调函数的参数：

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| err | number | 是 | 下载失败的错误码，错误原因见[下载任务的错误码](#下载任务的错误码)。 |

**示例：**

  ```js
  let failCallback = (err) => {
    console.error(`Failed to download the task. Code: ${err.code}, message: ${err.message}`);
  };
  downloadTask.on('fail', failCallback);
  ```


### off('fail')<sup>7+</sup>

off(type: 'fail', callback?: (err: number) =&gt; void): void

取消订阅下载任务失败监听，同步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | type | string | 是 | 取消订阅的事件类型，取值为'fail'（下载失败）。 |
  | callback | function | 否 | 需要删除的下载失败的回调函数。<br/>err：下载失败的错误码。 |

**示例：**

  ```js
  let failCallback = (err) => {
    console.error(`Failed to download the task. Code: ${err.code}, message: ${err.message}`);
  };
  downloadTask.off('fail', failCallback);
  ```

### delete<sup>9+</sup>

delete(): Promise&lt;boolean&gt;

移除下载的任务，异步方法，使用promise形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;boolean&gt; | 移除任务是否成功。 |

**示例：**

  ```js
  downloadTask.delete().then((result) => {
  if (result) {
    console.info('Succeeded in removing the download task.');
  } else {
    console.error(`Failed to remove the download task. Code: ${err.code}, message: ${err.message}`);
  }
  }).catch ((err) => {
    console.error(`Failed to remove the download task. Code: ${err.code}, message: ${err.message}`);
  });
  ```


### delete<sup>9+</sup>

delete(callback: AsyncCallback&lt;boolean&gt;): void

移除下载的任务，异步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | callback | AsyncCallback&lt;boolean&gt; | 是 | 移除任务是否成功。 |

**示例：**

  ```js
  downloadTask.delete((err, result)=>{
    if(err) {
      console.error(`Failed to remove the download task. Code: ${err.code}, message: ${err.message}`);
      return;
    } 
    if (result) {
      console.info('Succeeded in removing the download task.');
    } else {
      console.error(`Failed to remove the download task. Code: ${err.code}, message: ${err.message}`);
    } 
  });
  ```


### getTaskInfo<sup>9+</sup>

getTaskInfo(): Promise&lt;DownloadInfo&gt;

查询下载任务，异步方法，使用promise形式返回DownloadInfo里的信息。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;[DownloadInfo](#downloadinfo7)&gt; | 查询下载任务信息。 |

**示例：**

  ```js
  downloadTask.getTaskInfo().then((downloadInfo) => {    
    console.info('Succeeded in querying the download task')
  }) .catch((err) => {
    console.error(`Failed to query the download task. Code: ${err.code}, message: ${err.message}`)
  });
  ```


### getTaskInfo<sup>9+</sup>

getTaskInfo(callback: AsyncCallback&lt;DownloadInfo&gt;): void

查询下载的任务，异步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | callback | AsyncCallback&lt;[DownloadInfo](#downloadinfo7)&gt; | 是 | 查询下载任务的回调函数。 |

**示例：**

  ```js
  downloadTask.getTaskInfo((err, downloadInfo)=>{
    if(err) {
      console.error(`Failed to query the download mimeType. Code: ${err.code}, message: ${err.message}`);
    } else {
      console.info('Succeeded in querying the download mimeType');
    }
  });
  ```


### getTaskMimeType<sup>9+</sup>

getTaskMimeType(): Promise&lt;string&gt;

查询下载的任务的 MimeType，异步方法，使用promise形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;string&gt; | 查询下载任务的MimeType。 |

**示例：**

  ```js
  downloadTask.getTaskMimeType().then((data) => {    
    console.info('Succeeded in querying the download MimeType');
  }).catch((err) => {
    console.error(`Failed to query the download MimeType. Code: ${err.code}, message: ${err.message}`)
  });
  ```


### getTaskMimeType<sup>9+</sup>

getTaskMimeType(callback: AsyncCallback&lt;string&gt;): void;

查询下载的任务的 MimeType，异步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | callback | AsyncCallback&lt;string&gt; | 是 | 查询下载任务的MimeType的回调函数。 |

**示例：**

  ```js
  downloadTask.getTaskMimeType((err, data)=>{
    if(err) {
      console.error(`Failed to query the download mimeType. Code: ${err.code}, message: ${err.message}`);
    } else {
      console.info('Succeeded in querying the download mimeType');
    }
  });
  ```


### suspend<sup>9+</sup>

suspend(): Promise&lt;boolean&gt;

暂停下载任务，异步方法，使用promise形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;boolean&gt; | 暂停下载任务是否成功。 |

**示例：**

  ```js
  downloadTask.suspend().then((result) => {    
    if (result) {
      console.info('Succeeded in pausing the download task.');
    } else {
      console.error(`Failed to pause the download task. Code: ${err.code}, message: ${err.message}`);
    }
  }).catch((err) => {
    console.error(`Failed to pause the download task. Code: ${err.code}, message: ${err.message}`);
  });
  ```


### suspend<sup>9+</sup>

suspend(callback: AsyncCallback&lt;boolean&gt;): void

暂停下载任务，异步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | callback | AsyncCallback&lt;boolean&gt; | 是 | 暂停下载任务的回调函数。 |

**示例：**

  ```js
  downloadTask.suspend((err, result)=>{
    if(err) {
      console.error(`Failed to pause the download task. Code: ${err.code}, message: ${err.message}`);
      return;
    }
    if (result) {
      console.info('Succeeded in pausing the download task.');
    } else {
      console.error(`Failed to pause the download task. Code: ${err.code}, message: ${err.message}`);
    }
  });
  ```


### restore<sup>9+</sup>

restore(): Promise&lt;boolean&gt;

重新启动暂停的下载任务，异步方法，使用promise形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**返回值：**

  | 类型 | 说明 |
  | -------- | -------- |
  | Promise&lt;boolean&gt; | 重新启动暂停的下载任务是否成功。 |

**示例：**

  ```js
  downloadTask.restore().then((result) => {
    if (result) {
      console.info('Succeeded in resuming the download task.')
    } else {
      console.error(`Failed to resume the download task. Code: ${err.code}, message: ${err.message}`);
    }
    console.info('Succeeded in resuming the download task.')
  }).catch((err) => {
    console.error(`Failed to resume the download task. Code: ${err.code}, message: ${err.message}`);
  });
  ```


### restore<sup>9+</sup>

restore(callback: AsyncCallback&lt;boolean&gt;): void

重新启动暂停的下载任务，异步方法，使用callback形式返回结果。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

**参数：**

  | 参数名 | 类型 | 必填 | 说明 |
  | -------- | -------- | -------- | -------- |
  | callback | AsyncCallback&lt;boolean&gt; | 是 | 重新启动暂停的下载任务的回调函数。 |

**示例：**

  ```js
  downloadTask.restore((err, result)=>{
    if (err) {
      console.error(`Failed to resume the download task. Code: ${err.code}, message: ${err.message}`);
      return;
    } 
    if (result) {
      console.info('Succeeded in resuming the download task.');
    } else {
      console.error(`Failed to resume the download task. Code: ${err.code}, message: ${err.message}`);
    }
  });
  ```

## DownloadConfig
下载任务的配置信息。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

| 名称 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| url | string | 是 | 资源地址。 |
| header | Object | 否 | 添加要包含在下载请求中的HTTPS标志头。<br/>开发者可以通过header的X-TLS-Version参数指定需要使用的TLS版本(如果不指定，则默认使用CURL_SSLVERSION_TLSv1_2版本，指定则使用指定版本。)<br/>CURL_SSLVERSION_TLSv1_0<br/>CURL_SSLVERSION_TLSv1_1<br/>CURL_SSLVERSION_TLSv1_2<br/>CURL_SSLVERSION_TLSv1_3<br/>通过header的X-Cipher-List参数指定需要使用的密码套件(如果不指定，则默认使用安全密码套件，指定则使用指定密码套件。)<br/>-1.2允许使用的密码套件白名单：<br/>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256,TLS_DHE_RSA_WITH_AES_256_GCM_SHA384,<br/>TLS_DHE_DSS_WITH_AES_128_GCM_SHA256,TLS_DSS_RSA_WITH_AES_256_GCM_SHA384,<br/>TLS_PSK_WITH_AES_256_GCM_SHA384,TLS_DHE_PSK_WITH_AES_128_GCM_SHA256,<br/>TLS_DHE_PSK_WITH_AES_256_GCM_SHA384,TLS_DHE_PSK_WITH_CHACHA20_POLY1305_SHA256,<br/>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,<br/>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,<br/>TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256,TLS_ECDHE_PSK_WITH_CHACHA20_POLY1305_SHA256,<br/>TLS_ECDHE_PSK_WITH_AES_128_GCM_SHA256,TLS_ECDHE_PSK_WITH_AES_256_GCM_SHA384,<br/>TLS_ECDHE_PSK_WITH_AES_128_GCM_SHA256,TLS_DHE_RSA_WITH_AES_128_CCM,<br/>TLS_DHE_RSA_WITH_AES_256_CCM,TLS_DHE_RSA_WITH_CHACHA20_POLY1305_SHA256,<br/>TLS_PSK_WITH_AES_256_CCM,TLS_DHE_PSK_WITH_AES_128_CCM,<br/>TLS_DHE_PSK_WITH_AES_256_CCM,TLS_ECDHE_ECDSA_WITH_AES_128_CCM,<br/>TLS_ECDHE_ECDSA_WITH_AES_256_CCM,TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256<br/>-1.3允许使用的密码套件白名单：<br/>TLS_AES_128_GCM_SHA256,TLS_AES_256_GCM_SHA384,TLS_CHACHA20_POLY1305_SHA256,TLS_AES_128_CCM_SHA256<br/>-1.3新增国密算法套：<br/>TLS_SM4_GCM_SM3,TLS_SM4_CCM_SM3 |
| enableMetered | boolean | 否 | 设置是否允许在按流量计费的连接下下载(默认使用false)。Wi-Fi为非计费网络，数据流量为计费网络。<br/>-&nbsp;true：是<br/>-&nbsp;false：否 |
| enableRoaming | boolean | 否 | 设置是否允许在漫游网络中下载(默认使用false)。 <br/>-&nbsp;true：是<br/>-&nbsp;false：否|
| description | string | 否 | 设置下载会话的描述。 |
| filePath<sup>7+</sup> | string | 否 | 设置下载路径。<br/>-&nbsp;FA模型下使用[context](js-apis-inner-application-context.md) 获取应用存储路径，比如：\`${featureAbility.getContext().getFilesDir()}/test.txt\`，并将文件存储在此路径下。<br/>-&nbsp;Stage模型下使用[AbilityContext](js-apis-inner-application-context.md) 类获取文件路径，比如：\`${globalThis.abilityContext.tempDir}/test.txt\`，并将文件存储在此路径下。|
| networkType | number | 否 | 设置允许下载的网络类型(默认使用NETWORK_MOBILE&NETWORK_WIFI)。<br/>-&nbsp;NETWORK_MOBILE：0x00000001<br/>-&nbsp;NETWORK_WIFI：0x00010000|
| title | string | 否 | 设置下载任务名称。 |
| background<sup>9+</sup> | boolean | 否 | 后台任务通知开关，开启后可在通知中显示下载状态(默认使用false)。 |


## DownloadInfo<sup>7+</sup>
下载任务信息，[getTaskInfo<sup>9+</sup>](#gettaskinfo9)接口的回调参数。

**需要权限**：ohos.permission.INTERNET

**系统能力**: SystemCapability.MiscServices.Download

| 名称 | 类型 |必填 |  说明 |
| -------- | -------- | -------- | -------- |
| downloadId | number |是 | 下载任务ID。 |
| failedReason | number|是 | 下载失败原因，可以是任何[下载任务的错误码](#下载任务的错误码)常量。 |
| fileName | string |是| 下载的文件名。 |
| filePath | string |是| 存储文件的URI。 |
| pausedReason | number |是| 会话暂停的原因，可以是任何[下载任务暂停原因](#下载任务暂停原因)常量。 |
| status | number |是| 下载状态码，可以是任何[下载任务状态码](#下载任务状态码)常量。 |
| targetURI | string |是| 下载文件的URI。 |
| downloadTitle | string |是| 下载任务名称。 |
| downloadTotalBytes | number |是| 下载的文件的总大小（int&nbsp;bytes）。 |
| description | string |是| 待下载任务的描述信息。 |
| downloadedBytes | number |是| 实时下载大小（int&nbsp;&nbsp;bytes）。 |


<!--no_check-->