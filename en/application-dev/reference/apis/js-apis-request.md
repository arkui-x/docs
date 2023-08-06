# @ohos.request (Upload and Download)

The **request** module provides applications with basic upload, download, and background transmission agent capabilities.

> **NOTE**
>
> The initial APIs of this module are supported since API version 6. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import


```js
import request from '@ohos.request';
```

## Constants

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

### Network Types
You can set **networkType** in [DownloadConfig](#downloadconfig) to specify the network type for the download service.

| Name| Type| Value| Description|
| -------- | -------- | -------- | -------- |
| NETWORK_MOBILE | number | 0x00000001 | Whether download is allowed on a mobile network.|
| NETWORK_WIFI | number | 0x00010000 | Whether download is allowed on a WLAN.|

### Download Error Codes
The table below lists the values of **err** in the callback of [on('fail')<sup>7+</sup>](#onfail7) and the values of **failedReason** returned by [getTaskInfo<sup>9+</sup>](#gettaskinfo9).

| Name| Type| Value| Description|
| -------- | -------- | -------- | -------- |
| ERROR_CANNOT_RESUME<sup>7+</sup> | number |   0   | Failure to resume the download due to network errors.|
| ERROR_DEVICE_NOT_FOUND<sup>7+</sup> | number |   1   | Failure to find a storage device such as a memory card.|
| ERROR_FILE_ALREADY_EXISTS<sup>7+</sup> | number |   2   | Failure to download the file because it already exists.|
| ERROR_FILE_ERROR<sup>7+</sup> | number |   3   | File operation failure.|
| ERROR_HTTP_DATA_ERROR<sup>7+</sup> | number |   4   | HTTP transmission failure.|
| ERROR_INSUFFICIENT_SPACE<sup>7+</sup> | number |   5   | Insufficient storage space.|
| ERROR_TOO_MANY_REDIRECTS<sup>7+</sup> | number |   6   | Error caused by too many network redirections.|
| ERROR_UNHANDLED_HTTP_CODE<sup>7+</sup> | number |   7   | Unidentified HTTP code.|
| ERROR_UNKNOWN<sup>7+</sup> | number |   8   | Unknown error.|
| ERROR_OFFLINE<sup>9+</sup> | number |   9   | No network connection.|
| ERROR_UNSUPPORTED_NETWORK_TYPE<sup>9+</sup> | number |   10   | Network type mismatch.|


### Causes of Download Pause
The table below lists the values of **pausedReason** returned by [getTaskInfo<sup>9+</sup>](#gettaskinfo9).

| Name| Type| Value| Description|
| -------- | -------- | -------- | -------- |
| PAUSED_QUEUED_FOR_WIFI<sup>7+</sup> | number |   0   | Download paused and queuing for a WLAN connection, because the file size exceeds the maximum value allowed by a mobile network session.|
| PAUSED_WAITING_FOR_NETWORK<sup>7+</sup> | number |   1   | Download paused due to a network connection problem, for example, network disconnection.|
| PAUSED_WAITING_TO_RETRY<sup>7+</sup> | number |   2   | Download paused and then retried.|
| PAUSED_BY_USER<sup>9+</sup> | number |   3   | The user paused the session.|
| PAUSED_UNKNOWN<sup>7+</sup> | number |   4   | Download paused due to unknown reasons.|

### Download Task Status Codes
The table below lists the values of **status** returned by [getTaskInfo<sup>9+</sup>](#gettaskinfo9).

| Name| Type| Value| Description|
| -------- | -------- | -------- | -------- |
| SESSION_SUCCESSFUL<sup>7+</sup> | number |   0   | Successful download.|
| SESSION_RUNNING<sup>7+</sup> | number |   1   | Download in progress.|
| SESSION_PENDING<sup>7+</sup> | number |   2   | Download pending.|
| SESSION_PAUSED<sup>7+</sup> | number |   3   | Download paused.|
| SESSION_FAILED<sup>7+</sup> | number |   4   | Download failure without retry.|


## request.uploadFile<sup>9+</sup>

uploadFile(context: BaseContext, config: UploadConfig): Promise&lt;UploadTask&gt;

Uploads files. This API uses a promise to return the result. You can use [on('complete'|'fail')<sup>9+</sup>](#oncomplete--fail9) to obtain the upload error information.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| context | [BaseContext](js-apis-inner-application-baseContext.md) | Yes| Application-based context.|
| config | [UploadConfig](#uploadconfig) | Yes| Upload configurations.|


**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;[UploadTask](#uploadtask)&gt; | Promise used to return the **UploadTask** object.|

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID| Error Message|
| -------- | -------- |
| 13400002 | bad file path. |

**Example**

  ```js
  let uploadTask;
  let uploadConfig = {
    url: 'http://patch',
    header: { key1: "value1", key2: "value2" },
    method: "POST",
    files: [{ filename: "test", name: "test", uri: "internal://cache/test.jpg", type: "jpg" }],
    data: [{ name: "name123", value: "123" }],
  };
  try {
    request.uploadFile(globalThis.abilityContext, uploadConfig).then((data) => {
      uploadTask = data;
    }).catch((err) => {
        console.error('Failed to request the upload. Cause: ' + JSON.stringify(err));
    });
  } catch (err) {
    console.error('err.code : ' + err.code + ', err.message : ' + err.message);
  }
  ```


## request.uploadFile<sup>9+</sup>

uploadFile(context: BaseContext, config: UploadConfig, callback: AsyncCallback&lt;UploadTask&gt;): void

Uploads files. This API uses an asynchronous callback to return the result. You can use [on('complete'|'fail')<sup>9+</sup>](#oncomplete--fail9) to obtain the upload error information.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| context | [BaseContext](js-apis-inner-application-baseContext.md) | Yes| Application-based context.|
| config | [UploadConfig](#uploadconfig) | Yes| Upload configurations.|
| callback | AsyncCallback&lt;[UploadTask](#uploadtask)&gt; | Yes| Callback used to return the **UploadTask** object.|

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID| Error Message|
| -------- | -------- |
| 13400002 | bad file path. |

**Example**

  ```js
  let uploadTask;
  let uploadConfig = {
    url: 'http://patch',
    header: { key1: "value1", key2: "value2" },
    method: "POST",
    files: [{ filename: "test", name: "test", uri: "internal://cache/test.jpg", type: "jpg" }],
    data: [{ name: "name123", value: "123" }],
  };
  try {
    request.uploadFile(globalThis.abilityContext, uploadConfig, (err, data) => {
      if (err) {
          console.error('Failed to request the upload. Cause: ' + JSON.stringify(err));
          return;
      }
      uploadTask = data;
    });
  } catch (err) {
    console.error('err.code : ' + err.code + ', err.message : ' + err.message);
  }
  ```

## UploadTask

Implements file uploads. Before using any APIs of this class, you must obtain an **UploadTask** object through [request.uploadFile<sup>9+</sup>](#requestuploadfile9) in promise mode or [request.uploadFile<sup>9+</sup>](#requestuploadfile9-1) in callback mode.



### on('progress')

on(type: 'progress', callback:(uploadedSize: number, totalSize: number) =&gt; void): void

Subscribes to upload progress events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to subscribe to. The value is **'progress'** (upload progress).|
| callback | function | Yes| Callback for the upload progress event.|

  Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| uploadedSize | number | Yes| Size of the uploaded files, in bytes.|
| totalSize | number | Yes| Total size of the files to upload, in bytes.|

**Example**

  ```js
  let upProgressCallback = (uploadedSize, totalSize) => {
      console.info("upload totalSize:" + totalSize + "  uploadedSize:" + uploadedSize);
  };
  uploadTask.on('progress', upProgressCallback);
  ```


### on('headerReceive')<sup>7+</sup>

on(type: 'headerReceive', callback:  (header: object) =&gt; void): void

Subscribes to HTTP header events for an upload task. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to subscribe to. The value is **'headerReceive'** (response header).|
| callback | function | Yes| Callback for the HTTP Response Header event.|

  Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| header | object | Yes| HTTP Response Header.|

**Example**

  ```js
  let headerCallback = (headers) => {
      console.info("upOnHeader headers:" + JSON.stringify(headers));
  };
  uploadTask.on('headerReceive', headerCallback);
  ```


### on('complete' | 'fail')<sup>9+</sup>

 on(type:'complete' | 'fail', callback: Callback&lt;Array&lt;TaskState&gt;&gt;): void;

Subscribes to upload completion or failure events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to subscribe to. The value **'complete'** means the upload completion event, and **'fail'** means the upload failure event.|
| callback | Callback&lt;Array&lt;TaskState&gt;&gt; | Yes| Callback used to return the result.|

  Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| taskstates | Array&lt;[TaskState](#taskstate9)&gt; | Yes| Upload result.|

**Example**

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

Unsubscribes from upload progress events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to unsubscribe from. The value is **'progress'** (upload progress).|
| callback | function | No| Callback used to return the result.<br>**uploadedSize**: size of the uploaded files, in bytes.<br>**totalSize**: Total size of the files to upload, in bytes. |

**Example**

  ```js
  let upProgressCallback = (uploadedSize, totalSize) => {
      console.info('Upload delete progress notification.' + 'totalSize:' + totalSize + 'uploadedSize:' + uploadedSize);
  };
  uploadTask.off('progress', upProgressCallback);
  ```


### off('headerReceive')<sup>7+</sup>

off(type: 'headerReceive', callback?: (header: object) =&gt; void): void

Unsubscribes from HTTP header events for an upload task. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to unsubscribe from. The value is **'headerReceive'** (response header).|
| callback | function | No| Callback used to return the result.<br>**header**: HTTP response header.|

**Example**

  ```js
  let headerCallback = (header) => {
      console.info(`Upload delete headerReceive notification. header: ${JSON.stringify(header)}`);
  };
  uploadTask.off('headerReceive', headerCallback);
  ```

### off('complete' | 'fail')<sup>9+</sup>

 off(type:'complete' | 'fail', callback?: Callback&lt;Array&lt;TaskState&gt;&gt;): void;

Unsubscribes from upload completion or failure events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to subscribe to. The value **'complete'** means the upload completion event, and **'fail'** means the upload failure event.|
| callback | Callback&lt;Array&lt;TaskState&gt;&gt; | No| Callback used to return the result.<br>**taskstates**: upload task result.|

**Example**

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

Deletes this upload task. This API uses a promise to return the result. 

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;boolean&gt; | Promise used to return the task removal result. It returns **true** if the operation is successful and returns **false** otherwise.|

**Example**

  ```js
  uploadTask.delete().then((result) => {
      if (result) {
          console.info('Upload task removed successfully. ');
      } else {
          console.error('Failed to remove the upload task. ');
      }
  }).catch((err) => {
      console.error('Failed to remove the upload task. Cause: ' + JSON.stringify(err));
  });
  ```


### delete<sup>9+</sup>

delete(callback: AsyncCallback&lt;boolean&gt;): void

Deletes this upload task. This API uses an asynchronous callback to return the result. 

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;boolean&gt; | Yes| Callback used to return the result.|

**Example**

  ```js
  uploadTask.delete((err, result) => {
      if (err) {
          console.error('Failed to remove the upload task. Cause: ' + JSON.stringify(err));
          return;
      }
      if (result) {
          console.info('Upload task removed successfully.');
      } else {
          console.error('Failed to remove the upload task.');
      }
  });
  ```


## UploadConfig
Describes the configuration for an upload task.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| url | string | Yes| Resource URL.|
| header | Object | Yes| HTTP or HTTPS header added to an upload request.|
| method | string | Yes| Request method, which can be **'POST'** or **'PUT'**. The default value is **'POST'**.|
| files | Array&lt;[File](#file)&gt; | Yes| List of files to upload, which is submitted through **multipart/form-data**.|
| data | Array&lt;[RequestData](#requestdata)&gt; | Yes| Form data in the request body.|

## TaskState<sup>9+</sup>
Implements a **TaskState** object, which is the callback parameter of the [on('complete' | 'fail')<sup>9+</sup>](#oncomplete--fail9) and [off('complete' | 'fail')<sup>9+</sup>](#offcomplete--fail9) APIs.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Upload

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| path | string | Yes| File path.|
| responseCode | number | Yes| Return value of an upload task.|
| message | string | Yes| Description of the upload task result.|

## File
Describes the list of files in [UploadConfig](#uploadconfig).

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| filename | string | Yes| File name in the header when **multipart** is used.|
| name | string | Yes| Name of a form item when **multipart** is used. The default value is **file**.|
| uri | string | Yes| Local path for storing files.<br>Only the **internal** protocol type is supported. In the value, **internal://cache/** is mandatory. Example:<br>internal://cache/path/to/file.txt |
| type | string | Yes| Type of the file content. By default, the type is obtained based on the extension of the file name or URI.|


## RequestData
Describes the form data in [UploadConfig](#uploadconfig).

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Name of a form element.|
| value | string | Yes| Value of a form element.|

## request.downloadFile<sup>9+</sup>

downloadFile(context: BaseContext, config: DownloadConfig): Promise&lt;DownloadTask&gt;

Downloads files. This API uses a promise to return the result. You can use [on('complete'|'pause'|'remove')<sup>7+</sup>](#oncompletepauseremove7) to obtain the download task state, which can be completed, paused, or removed. You can also use [on('fail')<sup>7+</sup>](#onfail7) to obtain the task download error information.


**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| context | [BaseContext](js-apis-inner-application-baseContext.md) | Yes| Application-based context.|
| config | [DownloadConfig](#downloadconfig) | Yes| Download configurations.|

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;[DownloadTask](#downloadtask)&gt; | Promise used to return the result.|

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID| Error Message|
| -------- | -------- |
| 13400001 | file operation error. |
| 13400002 | bad file path. |
| 13400003 | task manager service error. |

**Example**

  ```js
  let downloadTask;
  try {
    request.downloadFile(globalThis.abilityContext, { url: 'https://xxxx/xxxx.hap' }).then((data) => {
        downloadTask = data;
    }).catch((err) => {
        console.error('Failed to request the download. Cause: ' + JSON.stringify(err));
    })
  } catch (err) {
    console.error('err.code : ' + err.code + ', err.message : ' + err.message);
  }
  ```


## request.downloadFile<sup>9+</sup>

downloadFile(context: BaseContext, config: DownloadConfig, callback: AsyncCallback&lt;DownloadTask&gt;): void;

Downloads files. This API uses an asynchronous callback to return the result. You can use [on('complete'|'pause'|'remove')<sup>7+</sup>](#oncompletepauseremove7) to obtain the download task state, which can be completed, paused, or removed. You can also use [on('fail')<sup>7+</sup>](#onfail7) to obtain the task download error information.


**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| context | [BaseContext](js-apis-inner-application-baseContext.md) | Yes| Application-based context.|
| config | [DownloadConfig](#downloadconfig) | Yes| Download configurations.|
| callback | AsyncCallback&lt;[DownloadTask](#downloadtask)&gt; | Yes| Callback used to return the result.|

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID| Error Message|
| -------- | -------- |
| 13400001 | file operation error. |
| 13400002 | bad file path. |
| 13400003 | task manager service error. |

**Example**

  ```js
  let downloadTask;
  try {
    request.downloadFile(globalThis.abilityContext, { url: 'https://xxxx/xxxxx.hap', 
    filePath: 'xxx/xxxxx.hap'}, (err, data) => {
        if (err) {
            console.error('Failed to request the download. Cause: ' + JSON.stringify(err));
            return;
        }
        downloadTask = data;
    });
  } catch (err) {
    console.error('err.code : ' + err.code + ', err.message : ' + err.message);
  }
  ```

## DownloadTask

Implements file downloads. Before using any APIs of this class, you must obtain a **DownloadTask** object through [request.downloadFile<sup>9+</sup>](#requestdownloadfile9) in promise mode or [request.downloadFile<sup>9+</sup>](#requestdownloadfile9-1) in callback mode.


### on('progress')

on(type: 'progress', callback:(receivedSize: number, totalSize: number) =&gt; void): void

Subscribes to download progress events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to subscribe to. The value is **'progress'** (download progress).|
| callback | function | Yes| Callback used to return the result.|

  Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| receivedSize | number | Yes| Size of the downloaded files, in bytes.|
| totalSize | number | Yes| Total size of the files to download, in bytes.|

**Example**

  ```js
  let progresCallback = (receivedSize, totalSize) => {
      console.info("download receivedSize:" + receivedSize + " totalSize:" + totalSize);
  };
  downloadTask.on('progress', progresCallback);
  ```


### off('progress')

off(type: 'progress', callback?: (receivedSize: number, totalSize: number) =&gt; void): void

Unsubscribes from download progress events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to unsubscribe from. The value is **'progress'** (download progress).|
| callback | function | No| Callback used to return the result.<br>**receivedSize**: size of the downloaded files.<br>**totalSize**: total size of the files to download.|

**Example**

  ```js
  let progresCallback = (receivedSize, totalSize) => {
      console.info('Download delete progress notification.' + 'receivedSize:' + receivedSize + 'totalSize:' + totalSize);
  };
  downloadTask.off('progress', progresCallback);
  ```


### on('complete'|'pause'|'remove')<sup>7+</sup>

on(type: 'complete'|'pause'|'remove', callback:() =&gt; void): void

Subscribes to download events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to subscribe to.<br>- **'complete'**: download task completion event.<br>- **'pause'**: download task pause event.<br>- **'remove'**: download task removal event.|
| callback | function | Yes| Callback used to return the result.|

**Example**

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

Unsubscribes from download events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to unsubscribe from.<br>- **'complete'**: download task completion event.<br>- **'pause'**: download task pause event.<br>- **'remove'**: download task removal event.|
| callback | function | No| Callback used to return the result.|

**Example**

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

Subscribes to download failure events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to subscribe to. The value is **'fail'** (download failure).|
| callback | function | Yes| Callback for the download task failure event.|

  Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| err | number | Yes| Error code of the download failure. For details about the error codes, see [Download Error Codes](#download-error-codes).|

**Example**

  ```js
  let failCallback = (err) => {
      console.info('Download task failed. Cause:' + err);
  };
  downloadTask.on('fail', failCallback);
  ```


### off('fail')<sup>7+</sup>

off(type: 'fail', callback?: (err: number) =&gt; void): void

Unsubscribes from download failure events. This API uses a callback to return the result synchronously.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Type of the event to unsubscribe from. The value is **'fail'** (download failure).|
| callback | function | No| Callback used to return the result.<br>**err**: error code of the download failure. |

**Example**

  ```js
  let failCallback = (err) => {
      console.info(`Download delete fail notification. err: ${err.message}`);
  };
  downloadTask.off('fail', failCallback);
  ```

### delete<sup>9+</sup>

delete(): Promise&lt;boolean&gt;

Removes this download task. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;boolean&gt; | Promise used to return the task removal result.|

**Example**

  ```js
  downloadTask.delete().then((result) => {
      if (result) {
          console.info('Download task removed.');
      } else {
          console.error('Failed to remove the download task.');
      }
  }).catch ((err) => {
      console.error('Failed to remove the download task.');
  });
  ```


### delete<sup>9+</sup>

delete(callback: AsyncCallback&lt;boolean&gt;): void

Deletes this download task. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;boolean&gt; | Yes| Callback used to return the task deletion result.|

**Example**

  ```js
  downloadTask.delete((err, result)=>{
      if(err) {
          console.error('Failed to remove the download task.');
          return;
      } 
      if (result) {
          console.info('Download task removed.');
      } else {
          console.error('Failed to remove the download task.');
      } 
  });
  ```


### getTaskInfo<sup>9+</sup>

getTaskInfo(): Promise&lt;DownloadInfo&gt;

Obtains the information about this download task. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;[DownloadInfo](#downloadinfo7)&gt; | Promise used to return the download task information.|

**Example**

  ```js
  downloadTask.getTaskInfo().then((downloadInfo) => {    
      console.info('Download task queried. Data:' + JSON.stringify(downloadInfo))
  }) .catch((err) => {
      console.error('Failed to query the download task. Cause:' + err)
  });
  ```


### getTaskInfo<sup>9+</sup>

getTaskInfo(callback: AsyncCallback&lt;DownloadInfo&gt;): void

Obtains the information about this download task. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;[DownloadInfo](#downloadinfo7)&gt; | Yes| Callback used to return the download task information.|

**Example**

  ```js
  downloadTask.getTaskInfo((err, downloadInfo)=>{
      if(err) {
          console.error('Failed to query the download mimeType. Cause:' + JSON.stringify(err));
      } else {
          console.info('download query success. data:'+ JSON.stringify(downloadInfo));
      }
  });
  ```


### getTaskMimeType<sup>9+</sup>

getTaskMimeType(): Promise&lt;string&gt;

Obtains the **MimeType** of this download task. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;string&gt; | Promise used to return the **MimeType** of the download task.|

**Example**

  ```js
  downloadTask.getTaskMimeType().then((data) => {    
      console.info('Download task queried. Data:' + JSON.stringify(data));
  }).catch((err) => {
      console.error('Failed to query the download MimeType. Cause:' + JSON.stringify(err))
  });
  ```


### getTaskMimeType<sup>9+</sup>

getTaskMimeType(callback: AsyncCallback&lt;string&gt;): void;

Obtains the **MimeType** of this download task. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;string&gt; | Yes| Callback used to return the **MimeType** of the download task.|

**Example**

  ```js
  downloadTask.getTaskMimeType((err, data)=>{
      if(err) {
          console.error('Failed to query the download mimeType. Cause:' + JSON.stringify(err));
      } else {
          console.info('Download task queried. data:' + JSON.stringify(data));
      }
  });
  ```


### suspend<sup>9+</sup>

suspend(): Promise&lt;boolean&gt;

Pauses this download task. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;boolean&gt; | Promise used to return the download task pause result.|

**Example**

  ```js
  downloadTask.suspend().then((result) => {    
      if (result) {
           console.info('Download task paused. ');
      } else {
          console.error('Failed to pause the download task. Cause:' + JSON.stringify(result));
      }
  }).catch((err) => {
      console.error('Failed to pause the download task. Cause:' + JSON.stringify(err));
  });
  ```


### suspend<sup>9+</sup>

suspend(callback: AsyncCallback&lt;boolean&gt;): void

Pauses this download task. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;boolean&gt; | Yes| Callback used to return the result.|

**Example**

  ```js
  downloadTask.suspend((err, result)=>{
      if(err) {
          console.error('Failed to pause the download task. Cause:' + JSON.stringify(err));
          return;
      }
      if (result) {
           console.info('Download task paused. ');
      } else {
          console.error('Failed to pause the download task. Cause:' + JSON.stringify(result));
      }
  });
  ```


### restore<sup>9+</sup>

restore(): Promise&lt;boolean&gt;

Resumes this download task. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;boolean&gt; | Promise used to return the result.|

**Example**

  ```js
  downloadTask.restore().then((result) => {
      if (result) {
          console.info('Download task resumed.')
      } else {
          console.error('Failed to resume the download task. ');
      }
      console.info('Download task resumed.')
  }).catch((err) => {
      console.error('Failed to resume the download task. Cause:' + err);
  });
  ```


### restore<sup>9+</sup>

restore(callback: AsyncCallback&lt;boolean&gt;): void

Resumes this download task. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;boolean&gt; | Yes| Callback used to return the result.|

**Example**

  ```js
  downloadTask.restore((err, result)=>{
      if (err) {
          console.error('Failed to resume the download task. Cause:' + err);
          return;
      } 
      if (result) {
          console.info('Download task resumed.');
      } else {
          console.error('Failed to resume the download task.');
      }
  });
  ```

## DownloadConfig
Defines the download task configuration.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| url | string | Yes| Resource URL.|
| header | Object | No| HTTPS flag header to be included in the download request.<br>The **X-TLS-Version** parameter in **header** specifies the TLS version to be used. If this parameter is not set, the CURL_SSLVERSION_TLSv1_2 version is used. Available options are as follows:<br>CURL_SSLVERSION_TLSv1_0<br>CURL_SSLVERSION_TLSv1_1<br>CURL_SSLVERSION_TLSv1_2<br>CURL_SSLVERSION_TLSv1_3<br>The **X-Cipher-List** parameter in **header** specifies the cipher suite list to be used. If this parameter is not specified, the secure cipher suite list is used. Available options are as follows:<br>- The TLS 1.2 cipher suite list includes the following ciphers:<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256,TLS_DHE_RSA_WITH_AES_256_GCM_SHA384,<br>TLS_DHE_DSS_WITH_AES_128_GCM_SHA256,TLS_DSS_RSA_WITH_AES_256_GCM_SHA384,<br>TLS_PSK_WITH_AES_256_GCM_SHA384,TLS_DHE_PSK_WITH_AES_128_GCM_SHA256,<br>TLS_DHE_PSK_WITH_AES_256_GCM_SHA384,TLS_DHE_PSK_WITH_CHACHA20_POLY1305_SHA256,<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256,TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384,<br>TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305_SHA256,TLS_ECDHE_PSK_WITH_CHACHA20_POLY1305_SHA256,<br>TLS_ECDHE_PSK_WITH_AES_128_GCM_SHA256,TLS_ECDHE_PSK_WITH_AES_256_GCM_SHA384,<br>TLS_ECDHE_PSK_WITH_AES_128_GCM_SHA256,TLS_DHE_RSA_WITH_AES_128_CCM,<br>TLS_DHE_RSA_WITH_AES_256_CCM,TLS_DHE_RSA_WITH_CHACHA20_POLY1305_SHA256,<br>TLS_PSK_WITH_AES_256_CCM,TLS_DHE_PSK_WITH_AES_128_CCM,<br>TLS_DHE_PSK_WITH_AES_256_CCM,TLS_ECDHE_ECDSA_WITH_AES_128_CCM,<br>TLS_ECDHE_ECDSA_WITH_AES_256_CCM,TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305_SHA256<br>- The TLS 1.3 cipher suite list includes the following ciphers:<br>TLS_AES_128_GCM_SHA256,TLS_AES_256_GCM_SHA384,TLS_CHACHA20_POLY1305_SHA256,TLS_AES_128_CCM_SHA256<br>- The TLS 1.3 cipher suite list adds the Chinese national cryptographic algorithm:<br>TLS_SM4_GCM_SM3,TLS_SM4_CCM_SM3 |
| enableMetered | boolean | No| Whether download is allowed on a metered connection. The default value is **false**. In general cases, a mobile data connection is metered, while a Wi-Fi connection is not.<br>- **true**: allowed<br>- **false**: not allowed|
| enableRoaming | boolean | No| Whether download is allowed on a roaming network. The default value is **false**.<br>- **true**: allowed<br>- **false**: not allowed|
| description | string | No| Description of the download session.|
| filePath<sup>7+</sup> | string | No| Path where the downloaded file is stored.<br>- In the FA model, use [context](js-apis-inner-application-context.md) to obtain the cache directory of the application, for example, **\${featureAbility.getContext().getFilesDir()}/test.txt\**, and store the file in this directory.<br>- In the stage model, use [AbilityContext](js-apis-inner-application-context.md) to obtain the file path, for example, **\${globalThis.abilityContext.tempDir}/test.txt\**, and store the file in this path.|
| networkType | number | No| Network type allowed for download. The default value is **NETWORK_MOBILE and NETWORK_WIFI**.<br>- NETWORK_MOBILE: 0x00000001<br>- NETWORK_WIFI: 0x00010000|
| title | string | No| Download task name.|
| background<sup>9+</sup> | boolean | No| Whether to enable background task notification so that the download status is displayed in the notification panel. The default value is false.|


## DownloadInfo<sup>7+</sup>
Defines the download task information, which is the callback parameter of the [getTaskInfo<sup>9+</sup>](#gettaskinfo9) API.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.MiscServices.Download

| Name| Type | Mandatory |  Description |
| -------- | -------- | -------- | -------- |
| downloadId | number |Yes| ID of the download task.|
| failedReason | number|Yes| Cause of the download failure. The value can be any constant in [Download Error Codes](#download-error-codes).|
| fileName | string |Yes| Name of the downloaded file.|
| filePath | string |Yes| URI of the saved file.|
| pausedReason | number |Yes| Cause of download pause. The value can be any constant in [Causes of Download Pause](#causes-of-download-pause).|
| status | number |Yes| Download task status code. The value can be any constant in [Download Task Status Codes](#download-task-status-codes).|
| targetURI | string |Yes| URI of the downloaded file.|
| downloadTitle | string |Yes| Name of the download task.|
| downloadTotalBytes | number |Yes| Total size of the files to download, in bytes.|
| description | string |Yes| Description of the download task.|
| downloadedBytes | number |Yes| Size of the files downloaded, in bytes.|

<!--no_check-->