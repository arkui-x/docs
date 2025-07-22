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
| index<sup>20+</sup> | number | No| Path index of the task. The default value is **0**.|
| begins<sup>20+</sup> | number | No| Start point of the file read when the upload task begins. The default value is **0**. The value is a closed interval, indicating that the file is read from the beginning.|
| ends<sup>20+</sup> | number | No| End point of the file read when the upload task is complete. The default value is **-1**. The value is a closed interval, indicating that the file is read till the end.|

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

## Action<sup>10+</sup>  

Defines action options.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name     | Value | Description |
| -------- | ----- | ----------- |
| DOWNLOAD | 0     | Download.   |
| UPLOAD   | 1     | Upload.     |


## Mode<sup>10+</sup>  

Defines mode options.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name       | Value | Description      |
| ---------- | ----- | ---------------- |
| FOREGROUND | 0     | Foreground task. |

## Network<sup>10+</sup>  

Defines network options.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name     | Value | Description            |
| -------- | ----- | ---------------------- |
| ANY      | 0     | Network of any type.   |
| WIFI     | 1     | Wi-Fi network.         |
| CELLULAR | 2     | Cellular data network. |


## FormItem<sup>10+</sup> 

Describes the form item of a task.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name  | Type                                                         | Mandatory | Description           |
| ----- | ------------------------------------------------------------ | --------- | --------------------- |
| name  | string                                                       | Yes       | Form parameter name.  |
| value | string \| [FileSpec](#filespec10) \| Array&lt;[FileSpec](#filespec10)&gt; | Yes       | Form parameter value. |


## Config<sup>10+</sup> 

Provides the configuration information of an upload or download task.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name        | Type                                           | Mandatory | Description                                                  |
| ----------- | ---------------------------------------------- | --------- | ------------------------------------------------------------ |
| action      | [Action](#action10)                            | Yes       | Task action.<br>- **UPLOAD**<br>- **DOWNLOAD**               |
| url         | string                                         | Yes       | Resource URL. The value contains a maximum of 2048 characters. |
| title       | string                                         | No        | Task title. The value contains a maximum of 256 characters. The default value is a null string. |
| description | string                                         | No        | Task description. The value contains a maximum of 1024 characters. The default value is a null string. |
| mode        | [Mode](#mode10)                                | No        | Task mode. The default mode is foreground .<br>- For a foreground task, a callback is used for notification. |
| overwrite   | boolean                                        | No        | Whether to overwrite an existing file during the download. The default value is **false**.<br>- **true**: Overwrite the existing file.<br>- **false**: Do not overwrite the existing file. In this case, the download fails. |
| method      | string                                         | No        | Standard HTTP method for the task. The value can be **GET**, **POST**, or **PUT**, which is case-insensitive.<br>- If the task is an upload, use **PUT** or **POST**. The default value is **PUT**.<br>- If the task is a download, use **GET** or **POST**. The default value is **GET**. |
| headers     | object                                         | No        | HTTP headers to be included in the task.<br>- If the task is an upload, the default **Content-Type** is **multipart/form-data**.<br>- If the task is a download, the default **Content-Type** is **application/json**. |
| data        | string \| Array&lt;[FormItem](#formitem10)&gt; | No        | Task data.<br>- If the task is a download, the value is a string, typically in JSON format (an object will be converted to a JSON string); the default value is null.<br>- If the task is an upload, the value is Array<[FormItem](#formitem10)>; the default value is null. |
| saveas      | string                                         | No        | Path for storing downloaded files. The options are as follows:<br>- Relative path in the cache folder of the invoker, for example, **"./xxx/yyy/zzz.html"** and **"xxx/yyy/zzz.html"**.<br>- URI (applicable when the application has the permission to access the URI), for example, **"datashare://bundle/xxx/yyy/zzz.html"**. This option is not supported currently.<br>The default value is a relative path in the cache folder of the application. |
| network     | [Network](#network10)                          | No        | Network used for the task. The default value is **ANY** (Wi-Fi or cellular). |
| metered     | boolean                                        | No        | Whether the task is allowed on a metered connection. The default value is **false**.<br>- **true**: The task is allowed on a metered connection.<br>- **false**: The task is not allowed on a metered connection. |
| roaming     | boolean                                        | No        | Whether the task is allowed on a roaming network. The default value is **true**.<br>- **true**: The task is allowed on a roaming network.<br>- **false**: The task is not allowed on a roaming network. |
| redirect    | boolean                                        | No        | Whether redirection is allowed. The default value is **true**.<br>- **true**: Redirection is allowed.<br>- **false**: Redirection is not allowed. |
| index       | number                                         | No        | Path index of the task. It is usually used for resumable downloads. The default value is **0**. |
| begins      | number                                         | No        | File start point of the task. It is usually used for resumable downloads. The default value is **0**. The value is a closed interval.<br>- If the task is a download, the value is obtained by sending an HTTP range request to read the start position when the server starts to download files.<br>- If the task is an upload, the value is obtained at the beginning of the upload. |
| ends        | number                                         | No        | File end point of the task. It is usually used for resumable downloads. The default value is **-1**. The value is a closed interval.<br>- If the task is a download, the value is obtained by sending an HTTP range request to read the end position when the server starts to download files.<br>- If the task is an upload, the value is obtained at the end of the upload. |
| precise     | boolean                                        | No        | - If this parameter is set to **true**, the task fails when the file size cannot be obtained.<br>- If this parameter is set to **false**, the task continues when the file size is set to **-1**.<br>The default value is **false**. |
| token       | string                                         | No        | Token of the task. If the task has a token configured, this token is required for query of the task. The value contains 8 to 2048 bytes. This parameter is left empty by default. |
| extras      | object                                         | No        | Additional information of the task. This parameter is left empty by default. |
| proxy<sup>20+</sup> | string | No| Proxy address. The value contains a maximum of 512 characters. <br>Android Download does not support proxy. <br>It is in the format of http://\<domain or address\>:\<port\>. By default, this parameter is left blank. |

## State<sup>10+</sup>  

Defines the current task status.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name        | Value | Description                                                  |
| ----------- | ----- | ------------------------------------------------------------ |
| INITIALIZED | 0x00  | The task is initialized based on the configuration specified in [Config](#config10). |
| WAITING     | 0x10  | The task lacks resources for running or the resources for retries do not match the network status. |
| RUNNING     | 0x20  | The task is being executed.                                  |
| RETRYING    | 0x21  | The task has failed at least once and is being executed again. |
| PAUSED      | 0x30  | The task is suspended and will be resumed later.             |
| STOPPED     | 0x31  | The task is stopped.                                         |
| COMPLETED   | 0x40  | The task is complete.                                        |
| FAILED      | 0x41  | The task fails.                                              |
| REMOVED     | 0x50  | The task is removed.                                         |


## Progress<sup>10+</sup> 

Describes the data structure of the task progress.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name      | Type                | Mandatory | Description                                                  |
| --------- | ------------------- | --------- | ------------------------------------------------------------ |
| state     | [State](#state10)   | Yes       | Current task status.                                         |
| index     | number              | Yes       | Index of the file that is being processed in the task.       |
| processed | number              | Yes       | Size of processed data in the current file in the task, in bytes. |
| sizes     | Array&lt;number&gt; | Yes       | Size of files in the task, in bytes.                         |
| extras    | object              | No        | Extra information of the task, for example, the header of the response from the server. |


## Faults<sup>10+</sup>  

Defines the cause of a task failure.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name         | Value | Description                                                  |
| ------------ | ----- | ------------------------------------------------------------ |
| OTHERS       | 0xFF  | Other fault.                                                 |
| DISCONNECTED | 0x00  | Network disconnection.                                       |
| TIMEOUT      | 0x10  | Timeout.                                                     |
| PROTOCOL     | 0x20  | Protocol error, for example, an internal server error (500) or a data range that cannot be processed (416). |
| FSIO         | 0x40  | File system I/O error, for example, an error that occurs during the open, search, read, write, or close operation. |


## Filter<sup>10+</sup>

Defines the filter criteria.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name   | Type                | Mandatory | Description                                                  |
| ------ | ------------------- | --------- | ------------------------------------------------------------ |
| before | number              | No        | Unix timestamp of the end time, in milliseconds. The default value is the invoking time. |
| after  | number              | No        | Unix timestamp of the start time, in milliseconds. The default value is the invoking time minus 24 hours. |
| state  | [State](#state10)   | No        | Task state.                                                  |
| action | [Action](#action10) | No        | Task action.<br>- **UPLOAD**<br>- **DOWNLOAD**               |
| mode   | [Mode](#mode10)     | No        | Task mode.<br>- **FOREGROUND**                               |

## TaskInfo<sup>10+</sup> 

Defines the data structure of the task information for query. The fields available vary depending on the query type.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name        | Type                                           | Mandatory | Description                                                  |
| ----------- | ---------------------------------------------- | --------- | ------------------------------------------------------------ |
| saveas      | string                                         | No        | Path for storing downloaded files. The options are as follows:<br>- Relative path in the cache folder of the invoker, for example, **"./xxx/yyy/zzz.html"** and **"xxx/yyy/zzz.html"**.<br>- URI (applicable when the application has the permission to access the URI), for example, **"datashare://bundle/xxx/yyy/zzz.html"**. This option is not supported currently.<br>The default value is a relative path in the cache folder of the application. |
| url         | string                                         | No        | Task URL.<br>- It can be obtained through [request.agent.show<sup>10+</sup>](#requestagentshow10-1), [request.agent.touch<sup>10+</sup>](#requestagenttouch10-1), or [request.agent.query<sup>10+</sup>](#requestagentquery10-1). When [request.agent.query<sup>10+</sup>](#requestagentquery10-1) is used, an empty string is returned. |
| data        | string \| Array&lt;[FormItem](#formitem10)&gt; | No        | Task value.<br>- It can be obtained through [request.agent.show<sup>10+</sup>](#requestagentshow10-1), [request.agent.touch<sup>10+</sup>](#requestagenttouch10-1), or [request.agent.query<sup>10+</sup>](#requestagentquery10-1). When [request.agent.query<sup>10+</sup>](#requestagentquery10-1) is used, an empty string is returned. |
| tid         | string                                         | Yes       | Task ID.                                                     |
| title       | string                                         | Yes       | Task title.                                                  |
| description | string                                         | Yes       | Task description.                                            |
| action      | [Action](#action10)                            | Yes       | Task action.<br>- **UPLOAD**<br>- **DOWNLOAD**               |
| mode        | [Mode](#mode10)                                | Yes       | Task mode.<br>- **FOREGROUND**                               |
| mimeType    | string                                         | Yes       | MIME type in the task configuration.                         |
| progress    | [Progress](#progress10)                        | Yes       | Task progress.                                               |
| ctime       | number                                         | Yes       | Unix timestamp when the task is created, in milliseconds. The value is generated by the system of the current device.<br>Note: When [request.agent.search<sup>10+</sup>](#requestagentsearch10-1) is used for query, this value must be within the range of [after,before] for the task ID to be obtained. For details about **before** and **after**, see [Filter](#filter10). |
| mtime       | number                                         | Yes       | Unix timestamp when the task state changes, in milliseconds. The value is generated by the system of the current device. |
| faults      | [Faults](#faults10)                            | Yes       | Failure cause of the task.<br>- **OTHERS**: other fault.<br>- **DISCONNECT**: network disconnection.<br>- **TIMEOUT**: timeout.<br>- **PROTOCOL**: protocol error.<br>- **FSIO**: file system I/O error. |
| reason      | string                                         | Yes       | Reason why the task is waiting, failed, stopped, or paused.  |
| extras      | string                                         | No        | Extra information of the task                                |
| priority<sup>20+</sup> | number | Yes| Task priority. The priority of a foreground task is higher than that of a background task. For tasks in the same mode, a smaller value indicates a higher priority.|

## HttpResponse<sup>20+</sup> 
Describes the data structure of the task response header.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| version | string | Yes| HTTP version.|
| statusCode | number | Yes| HTTP response status code.|
| reason | string | Yes| HTTP response cause.|
| headers | Map&lt;string, Array&lt;string&gt;&gt; | Yes| HTTP response header.|

## BroadcastEvent<sup>20+</sup>

Defines a custom system event. You can use a common event API to obtain the event.
The upload and download SA has the ohos.permission.SEND_TASK_COMPLETE_EVENT permission. You can configure the level-2 configuration file to which the metadata of an event points to intercept other event senders.

Use the **CommonEventData** type to transmit data related to common events. The members in **CommonEventData** are different from those described in [CommonEventData](js-apis-inner-commonEvent-commonEventData.md). Specifically, **CommonEventData.code** indicates the task status, which is **0x40 COMPLETE** or **0x41 FAILED**, and **CommonEventData.data** indicates the task ID.

<!--Del-->
For details about how to obtain the event configuration and configure the level-2 configuration file, see [Subscribing to Common Events in Static Mode (for System Applications Only)](../../basic-services/common-event/common-event-static-subscription.md).<!--DelEnd-->

**System capability**: SystemCapability.Request.FileTransferAgent

| Name| Value| Description       |
| -------- | ------- |-----------|
| COMPLETE | 'ohos.request.event.COMPLETE' | Task completion event. The returned event code can be **0x40** or **0x41**, depending on whether the task is successful or fails.|


## Task<sup>10+</sup> 

Implements an upload or download task. Before using this API, you must obtain a **Task** object, from a promise through [request.agent.create<sup>10+</sup>](#requestagentcreate10-1) or from a callback through [request.agent.create<sup>10+</sup>](#requestagentcreate10).

### Attributes

Task attributes include the task ID and task configuration.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name   | Type                | Mandatory | Description                                                  |
| ------ | ------------------- | --------- | ------------------------------------------------------------ |
| tid    | string              | Yes       | Task ID, which is unique in the system and is automatically generated by the system. |
| config | [Config](#config10) | Yes       | Task configuration.                                          |


### on('progress')<sup>10+</sup>

on(event: 'progress', callback: (progress: Progress) =&gt; void): void

Subscribes to foreground task progress changes. This API uses a callback to return the result asynchronously.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| event    | string   | Yes       | Type of the event to subscribe to.<br>The value is **'progress'**, indicating the task progress. |
| callback | function | Yes       | Callback used to return the data structure of the task progress. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message    |
| -------- | ---------------- |
| 21900005 | task mode error. |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "taskOnTest",
  value: {
    filename: "taskOnTest.avi",
    mimeType: "application/octet-stream",
    path: "./taskOnTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'taskOnTest',
  description: 'Sample code for event listening',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
let createOnCallback = (progress: request.agent.Progress) => {
  console.info('upload task progress.');
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.on('progress', createOnCallback);
  console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### on('completed')<sup>10+</sup>

on(event: 'completed', callback: (progress: Progress) =&gt; void): void

Subscribes to the foreground task completion event. This API uses a callback to return the result asynchronously.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| event    | string   | Yes       | Type of the event to subscribe to.<br>The value is **'completed'**, indicating task completion. |
| callback | function | Yes       | Callback used to return the data structure of the task progress. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message    |
| -------- | ---------------- |
| 21900005 | task mode error. |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "taskOnTest",
  value: {
    filename: "taskOnTest.avi",
    mimeType: "application/octet-stream",
    path: "./taskOnTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'taskOnTest',
  description: 'Sample code for event listening',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
let createOnCallback = (progress: request.agent.Progress) => {
  console.info('upload task completed.');
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.on('completed', createOnCallback);
  console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### on('failed')<sup>10+</sup>

on(event: 'failed', callback: (progress: Progress) =&gt; void): void

Subscribes to the foreground task failure event. This API uses a callback to return the result asynchronously.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| event    | string   | Yes       | Type of the event to subscribe to.<br>The value is **'failed'**, indicating task failure. |
| callback | function | Yes       | Callback used to return the data structure of the task progress. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message    |
| -------- | ---------------- |
| 21900005 | task mode error. |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "taskOnTest",
  value: {
    filename: "taskOnTest.avi",
    mimeType: "application/octet-stream",
    path: "./taskOnTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'taskOnTest',
  description: 'Sample code for event listening',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
let createOnCallback = (progress: request.agent.Progress) => {
  console.info('upload task failed.');
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.on('failed', createOnCallback);
  console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### on('response')<sup>20+</sup>

on(event: 'response', callback: Callback&lt;HttpResponse&gt;): void

Subscribes to task response headers. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to subscribe to.<br>- **'response'**: task response.|
| callback | Callback&lt;[HttpResponse](#httpresponse12)&gt; | Yes| Callback used to return the data structure of the task response header.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOnTest",
    value: {
      filename: "taskOnTest.avi",
      path: "./taskOnTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOnTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOnCallback = (response: request.agent.HttpResponse) => {
    console.info('upload task response.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('response', createOnCallback);
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### on('pause')<sup>20+</sup>

on(event: 'pause', callback: (progress: [Progress](#progress10)) =&gt; void): void

Subscribes to task pause events. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to subscribe to.<br>- **'pause'**: task pause.|
| callback | function | Yes| Callback used to return the data structure of the task progress.|

Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| progress | [Progress](#progress10) | Yes| Task progress.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOnTest",
    value: {
      filename: "taskOnTest.avi",
      path: "./taskOnTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOnTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOnCallback = (progress: request.agent.Progress) => {
    console.info('upload task pause.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('pause', createOnCallback);
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.pause();
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### on('resume')<sup>20+</sup>

on(event: 'resume', callback: (progress: [Progress](#progress10)) =&gt; void): void

Subscribes to task resume events. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to subscribe to.<br>- **'resume'**: task resume.|
| callback | function | Yes| Callback used to return the data structure of the task progress.|

Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| progress | [Progress](#progress10) | Yes| Task progress.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOnTest",
    value: {
      filename: "taskOnTest.avi",
      path: "./taskOnTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOnTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOnCallback = (progress: request.agent.Progress) => {
    console.info('upload task resume.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('resume', createOnCallback);
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.pause();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.resume();
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### on('remove')<sup>20+</sup>

on(event: 'remove', callback: (progress: [Progress](#progress10)) =&gt; void): void

Subscribes to task removal events. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to subscribe to.<br>- **'remove'**: task removal.|
| callback | function | Yes| Callback used to return the data structure of the task progress.|

Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| progress | [Progress](#progress10) | Yes| Task progress.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOnTest",
    value: {
      filename: "taskOnTest.avi",
      path: "./taskOnTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOnTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOnCallback = (progress: request.agent.Progress) => {
    console.info('upload task remove.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('remove', createOnCallback);
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    request.agent.remove(task.tid);
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).



### off('progress')<sup>10+</sup>

off(event: 'progress', callback?: (progress: Progress) =&gt; void): void

Unsubscribes from foreground task progress changes.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| event    | string   | Yes       | Type of the event to subscribe to.<br>The value is **'progress'**, indicating the task progress. |
| callback | function | No        | Callback to unregister. If this parameter is not specified, all callbacks of the current type will be unregistered. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message    |
| -------- | ---------------- |
| 21900005 | task mode error. |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "taskOffTest",
  value: {
    filename: "taskOffTest.avi",
    mimeType: "application/octet-stream",
    path: "./taskOffTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'taskOffTest',
  description: 'Sample code for event listening',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
let createOffCallback1 = (progress: request.agent.Progress) => {
  console.info('upload task progress.');
};
let createOffCallback2 = (progress: request.agent.Progress) => {
  console.info('upload task progress.');
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.on('progress', createOffCallback1);
  task.on('progress', createOffCallback2);
  // Unsubscribe from createOffCallback1.
  task.off('progress', createOffCallback1);
  // Unsubscribe from all callbacks of foreground task progress changes.
  task.off('progress');
  console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### off('completed')<sup>10+</sup>

off(event: 'completed', callback?: (progress: Progress) =&gt; void): void

Unsubscribes from the foreground task completion event.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| event    | string   | Yes       | Type of the event to subscribe to.<br>The value is **'completed'**, indicating task completion. |
| callback | function | No        | Callback to unregister. If this parameter is not specified, all callbacks of the current type will be unregistered. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message    |
| -------- | ---------------- |
| 21900005 | task mode error. |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "taskOffTest",
  value: {
    filename: "taskOffTest.avi",
    mimeType: "application/octet-stream",
    path: "./taskOffTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'taskOffTest',
  description: 'Sample code for event listening',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
let createOffCallback1 = (progress: request.agent.Progress) => {
  console.info('upload task completed.');
};
let createOffCallback2 = (progress: request.agent.Progress) => {
  console.info('upload task completed.');
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.on('completed', createOffCallback1);
  task.on('completed', createOffCallback2);
  // Unsubscribe from createOffCallback1.
  task.off('completed', createOffCallback1);
  // Unsubscribe from all callbacks of the foreground task completion event.
  task.off('completed');
  console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### off('failed')<sup>10+</sup>

off(event: 'failed', callback?: (progress: Progress) =&gt; void): void

Unsubscribes from the foreground task failure event.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| event    | string   | Yes       | Type of the event to subscribe to.<br>The value is **'failed'**, indicating task failure. |
| callback | function | No        | Callback to unregister. If this parameter is not specified, all callbacks of the current type will be unregistered. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message    |
| -------- | ---------------- |
| 21900005 | task mode error. |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "taskOffTest",
  value: {
    filename: "taskOffTest.avi",
    mimeType: "application/octet-stream",
    path: "./taskOffTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'taskOffTest',
  description: 'Sample code for event listening',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
let createOffCallback1 = (progress: request.agent.Progress) => {
  console.info('upload task failed.');
};
let createOffCallback2 = (progress: request.agent.Progress) => {
  console.info('upload task failed.');
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.on('failed', createOffCallback1);
  task.on('failed', createOffCallback2);
  // Unsubscribe from createOffCallback1.
  task.off('failed', createOffCallback1);
  // Unsubscribe from all callbacks of the foreground task failure event.
  task.off('failed');
  console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### off('response')<sup>20+</sup>

off(event: 'response', callback?: Callback&lt;HttpResponse&gt;): void

Unsubscribes from task response headers.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to unsubscribe from.<br>- **response**: task response.|
| callback | Callback&lt;[HttpResponse](#httpresponse12)&gt; | No| Callback to unregister. If this parameter is not specified, all callbacks of the current type will be unregistered.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOffTest",
    value: {
      filename: "taskOffTest.avi",
      path: "./taskOffTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOffTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOffCallback1 = (progress: request.agent.HttpResponse) => {
    console.info('upload task response.');
  };
  let createOffCallback2 = (progress: request.agent.HttpResponse) => {
    console.info('upload task response.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('response', createOffCallback1);
    task.on('response', createOffCallback2);
    // Unsubscribe from createOffCallback1.
    task.off('response', createOffCallback1);
    // Unsubscribe from all callbacks of the task removal event.
    task.off('response');
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### off('pause')<sup>20+</sup>

off(event: 'pause', callback?: (progress: [Progress](#progress10)) =&gt; void): void

Unsubscribes from the foreground task pause event.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to unsubscribe from.<br>- **'pause'**: task pause.|
| callback | function | No| Callback used to return the data structure of the task progress.|

Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| progress | [Progress](#progress10) | Yes| Task progress.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOffTest",
    value: {
      filename: "taskOffTest.avi",
      path: "./taskOffTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOffTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOffCallback1 = (progress: request.agent.Progress) => {
    console.info('upload task pause.');
  };
  let createOffCallback2 = (progress: request.agent.Progress) => {
    console.info('upload task pause.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('pause', createOffCallback1);
    task.on('pause', createOffCallback2);
    // Unsubscribe from createOffCallback1.
    task.off('pause', createOffCallback1);
    // Unsubscribe from all callbacks of the foreground task pause event.
    task.off('pause');
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### off('resume')<sup>20+</sup>

off(event: 'resume', callback?: (progress: [Progress](#progress10)) =&gt; void): void

Unsubscribes from the foreground task resume event.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to unsubscribe from.<br>- **'resume'**: task resume.|
| callback | function | No| Callback used to return the data structure of the task progress.|

Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| progress | [Progress](#progress10) | Yes| Task progress.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOffTest",
    value: {
      filename: "taskOffTest.avi",
      path: "./taskOffTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOffTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOffCallback1 = (progress: request.agent.Progress) => {
    console.info('upload task resume.');
  };
  let createOffCallback2 = (progress: request.agent.Progress) => {
    console.info('upload task resume.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('resume', createOffCallback1);
    task.on('resume', createOffCallback2);
    // Unsubscribe from createOffCallback1.
    task.off('resume', createOffCallback1);
    // Unsubscribe from all callbacks of the foreground task resume event.
    task.off('resume');
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### off('remove')<sup>20+</sup>

off(event: 'remove', callback?: (progress: [Progress](#progress10)) =&gt; void): void

Unsubscribes from the task removal event.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| event | string | Yes| Type of the event to unsubscribe from.<br>- **'remove'**: task removal.|
| callback | function | No| Callback used to return the data structure of the task progress.|

Parameters of the callback function

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| progress | [Progress](#progress10) | Yes| Task progress.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 401 | Parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let attachments: Array<request.agent.FormItem> = [{
    name: "taskOffTest",
    value: {
      filename: "taskOffTest.avi",
      path: "./taskOffTest.avi",
    }
  }];
  let config: request.agent.Config = {
    action: request.agent.Action.UPLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskOffTest',
    description: 'Sample code for event listening',
    mode: request.agent.Mode.FOREGROUND,
    overwrite: false,
    method: "PUT",
    data: attachments,
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  let createOffCallback1 = (progress: request.agent.Progress) => {
    console.info('upload task remove.');
  };
  let createOffCallback2 = (progress: request.agent.Progress) => {
    console.info('upload task remove.');
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.on('remove', createOffCallback1);
    task.on('remove', createOffCallback2);
    // Unsubscribe from createOffCallback1.
    task.off('remove', createOffCallback1);
    // Unsubscribe from all callbacks of the task removal event.
    task.off('remove');
    console.info(`Succeeded in creating a upload task. result: ${task.tid}`);
    task.start();
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a upload task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### start<sup>10+</sup>

start(callback: AsyncCallback&lt;void&gt;): void

Starts this task. This API cannot be used to start an initialized task. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| callback | function | Yes       | Callback used to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900007 | task state error.           |

**Example**

  ```ts
let config: request.agent.Config = {
  action: request.agent.Action.DOWNLOAD,
  url: 'http://127.0.0.1',
  title: 'taskStartTest',
  description: 'Sample code for start the download task',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "GET",
  data: "",
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.start((err: BusinessError) => {
    if (err) {
      console.error(`Failed to start the download task, Code: ${err.code}, message: ${err.message}`);
      return;
    }
    console.info(`Succeeded in starting a download task.`);
  });
  console.info(`Succeeded in creating a download task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### start<sup>10+</sup>

start(): Promise&lt;void&gt;

Starts this task. This API cannot be used to start an initialized task. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Request.FileTransferAgent

**Return value**

| Type                | Description                    |
| ------------------- | ------------------------------ |
| Promise&lt;void&gt; | Promise that returns no value. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900007 | task state error.           |

**Example**

  ```ts
let config: request.agent.Config = {
  action: request.agent.Action.DOWNLOAD,
  url: 'http://127.0.0.1',
  title: 'taskStartTest',
  description: 'Sample code for start the download task',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "GET",
  data: "",
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.start().then(() => {
    console.info(`Succeeded in starting a download task.`);
  }).catch((err: BusinessError) => {
    console.error(`Failed to start the download task, Code: ${err.code}, message: ${err.message}`);
  });
  console.info(`Succeeded in creating a download task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

### pause<sup>20+</sup>

pause(callback: AsyncCallback&lt;void&gt;): void

Pauses a task that is waiting, running, or retrying. A paused task can be resumed by [resume](#resume10). This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.|

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](errorcode-request.md).

| ID| Error Message|
| -------- | -------- |
| 13400003 | Task service ability error. |
| 21900007 | Operation with wrong task state. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let config: request.agent.Config = {
    action: request.agent.Action.DOWNLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskPauseTest',
    description: 'Sample code for pause the download task',
    mode: request.agent.Mode.BACKGROUND,
    overwrite: false,
    method: "GET",
    data: "",
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.start();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.pause((err: BusinessError) => {
      if (err) {
        console.error(`Failed to pause the download task, Code: ${err.code}, message: ${err.message}`);
        return;
      }
      console.info(`Succeeded in pausing a download task. `);
    });
    console.info(`Succeeded in creating a download task. result: ${task.tid}`);
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

### pause<sup>20+</sup>

pause(): Promise&lt;void&gt;

Pauses a task that is waiting, running, or retrying. A paused task can be resumed by [resume](#resume10). This API uses a promise to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](errorcode-request.md).

| ID| Error Message|
| -------- | -------- |
| 13400003 | Task service ability error. |
| 21900007 | Operation with wrong task state. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let config: request.agent.Config = {
    action: request.agent.Action.DOWNLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskPauseTest',
    description: 'Sample code for pause the download task',
    mode: request.agent.Mode.BACKGROUND,
    overwrite: false,
    method: "GET",
    data: "",
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.start();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.pause().then(() => {
      console.info(`Succeeded in pausing a download task. `);
    }).catch((err: BusinessError) => {
      console.error(`Failed to pause the download task, Code: ${err.code}, message: ${err.message}`);
    });
    console.info(`Succeeded in creating a download task. result: ${task.tid}`);
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
  });
  ```
### resume<sup>20+</sup>

resume(callback: AsyncCallback&lt;void&gt;): void

Resumes a paused task. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;void&gt; | Yes| Callback used to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object.|

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](errorcode-request.md) and [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 201 | Permission denied. |
| 13400003 | Task service ability error. |
| 21900007 | Operation with wrong task state. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let config: request.agent.Config = {
    action: request.agent.Action.DOWNLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskResumeTest',
    description: 'Sample code for resume the download task',
    mode: request.agent.Mode.BACKGROUND,
    overwrite: false,
    method: "GET",
    data: "",
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.start();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.pause();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.resume((err: BusinessError) => {
      if (err) {
        console.error(`Failed to resume the download task, Code: ${err.code}, message: ${err.message}`);
        return;
      }
      console.info(`Succeeded in resuming a download task. `);
    });
    console.info(`Succeeded in creating a download task. result: ${task.tid}`);
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

### resume<sup>20+</sup>

resume(): Promise&lt;void&gt;

Resumes a paused task. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Request.FileTransferAgent

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](errorcode-request.md) and [Universal Error Codes](../errorcode-universal.md).

| ID| Error Message|
| -------- | -------- |
| 201 | Permission denied. |
| 13400003 | Task service ability error. |
| 21900007 | Operation with wrong task state. |

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { common } from '@kit.AbilityKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let config: request.agent.Config = {
    action: request.agent.Action.DOWNLOAD,
    url: 'http://127.0.0.1', // Replace the URL with the HTTP address of the real server.
    title: 'taskResumeTest',
    description: 'Sample code for resume the download task',
    mode: request.agent.Mode.BACKGROUND,
    overwrite: false,
    method: "GET",
    data: "",
    saveas: "./",
    network: request.agent.Network.CELLULAR,
    metered: false,
    roaming: true,
    retry: true,
    redirect: true,
    index: 0,
    begins: 0,
    ends: -1,
    gauge: false,
    precise: false,
    token: "it is a secret"
  };
  request.agent.create(context, config).then((task: request.agent.Task) => {
    task.start();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.pause();
    for(let t = Date.now(); Date.now() - t <= 1000;); // To prevent asynchronous out-of-order, wait for 1 second before performing the next operation.
    task.resume().then(() => {
      console.info(`Succeeded in resuming a download task. `);
    }).catch((err: BusinessError) => {
      console.error(`Failed to resume the download task, Code: ${err.code}, message: ${err.message}`);
    });
    console.info(`Succeeded in creating a download task. result: ${task.tid}`);
  }).catch((err: BusinessError) => {
    console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
  });
  ```

### stop<sup>10+</sup>

stop(callback: AsyncCallback&lt;void&gt;): void

Stops this task. This API can be used to stop a running, waiting, or retrying task. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type     | Mandatory | Description                                                  |
| -------- | -------- | --------- | ------------------------------------------------------------ |
| callback | function | Yes       | Callback used to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900007 | task state error.           |

**Example**

  ```ts
let config: request.agent.Config = {
  action: request.agent.Action.DOWNLOAD,
  url: 'http://127.0.0.1',
  title: 'taskStopTest',
  description: 'Sample code for stop the download task',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "GET",
  data: "",
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.stop((err: BusinessError) => {
    if (err) {
      console.error(`Failed to stop the download task, Code: ${err.code}, message: ${err.message}`);
      return;
    }
    console.info(`Succeeded in stopping a download task. `);
  });
  console.info(`Succeeded in creating a download task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
});
  ```


### stop<sup>10+</sup>

stop(): Promise&lt;void&gt;

Stops this task. This API can be used to stop a running, waiting, or retrying task. This API uses a promise to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Return value**

| Type                | Description                    |
| ------------------- | ------------------------------ |
| Promise&lt;void&gt; | Promise that returns no value. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900007 | task state error.           |

**Example**

  ```ts
let config: request.agent.Config = {
  action: request.agent.Action.DOWNLOAD,
  url: 'http://127.0.0.1',
  title: 'taskStopTest',
  description: 'Sample code for stop the download task',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "GET",
  data: "",
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  task.stop().then(() => {
    console.info(`Succeeded in stopping a download task. `);
  }).catch((err: BusinessError) => {
    console.error(`Failed to stop the download task, Code: ${err.code}, message: ${err.message}`);
  });
  console.info(`Succeeded in creating a download task. result: ${task.tid}`);
}).catch((err: BusinessError) => {
  console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
});
  ```

## request.agent.create<sup>10+</sup>

create(context: BaseContext, config: Config, callback: AsyncCallback&lt;Task&gt;): void

Creates an upload or download task and adds it to the queue. An application can create a maximum of 10 unfinished tasks. This API uses an asynchronous callback to return the result.


**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type                                                    | Mandatory | Description                                                  |
| -------- | ------------------------------------------------------- | --------- | ------------------------------------------------------------ |
| config   | [Config](#config10)                                     | Yes       | Task configuration.                                          |
| context  | [BaseContext](js-apis-inner-application-baseContext.md) | Yes       | Application-based context.                                   |
| callback | AsyncCallback&lt;[Task](#task10)&gt;                    | Yes       | Callback used to return the configuration about the created task. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message                      |
| -------- | ---------------------------------- |
| 13400001 | file operation error.              |
| 13400003 | task service ability error.        |
| 21900004 | application task queue full error. |
| 21900005 | task mode error.                   |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "createTest",
  value: {
    filename: "createTest.avi",
    mimeType: "application/octet-stream",
    path: "./createTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'createTest',
  description: 'Sample code for create task',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
request.agent.create(getContext(), config, (err: BusinessError, task: request.agent.Task) => {
  if (err) {
    console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
    return;
  }
  console.info(`Succeeded in creating a download task. result: ${task.config}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

## request.agent.create<sup>10+</sup>

create(context: BaseContext, config: Config): Promise&lt;Task&gt;

Creates an upload or download task and adds it to the queue. An application can create a maximum of 10 unfinished tasks. This API uses a promise to return the result.


**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name    | Type                                                    | Mandatory | Description                |
| ------- | ------------------------------------------------------- | --------- | -------------------------- |
| context | [BaseContext](js-apis-inner-application-baseContext.md) | Yes       | Application-based context. |
| config  | [Config](#config10)                                     | Yes       | Task configuration.        |

**Return value**

| Type                           | Description                                                  |
| ------------------------------ | ------------------------------------------------------------ |
| Promise&lt;[Task](#task10)&gt; | Promise used to return the configuration about the created task. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message                      |
| -------- | ---------------------------------- |
| 13400001 | file operation error.              |
| 13400003 | task service ability error.        |
| 21900004 | application task queue full error. |
| 21900005 | task mode error.                   |

**Example**

  ```ts
let attachments: Array<request.agent.FormItem> = [{
  name: "createTest",
  value: {
    filename: "createTest.avi",
    mimeType: "application/octet-stream",
    path: "./createTest.avi",
  }
}];
let config: request.agent.Config = {
  action: request.agent.Action.UPLOAD,
  url: 'http://127.0.0.1',
  title: 'createTest',
  description: 'Sample code for create task',
  mode: request.agent.Mode.FOREGROUND,
  overwrite: false,
  method: "PUT",
  data: attachments,
  saveas: "./",
  network: request.agent.Network.CELLULAR,
  metered: false,
  roaming: true,
  retry: true,
  redirect: true,
  index: 0,
  begins: 0,
  ends: -1,
  gauge: false,
  precise: false,
  token: "it is a secret"
};
request.agent.create(getContext(), config).then((task: request.agent.Task) => {
  console.info(`Succeeded in creating a download task. result: ${task.config}`);
}).catch((err) => {
  console.error(`Failed to create a download task, Code: ${err.code}, message: ${err.message}`);
});
  ```

> **NOTE**
>
> For details about how to obtain the context in the example, see [Obtaining the Context of UIAbility](../../application-models/uiability-usage.md#obtaining-the-context-of-uiability).

## request.agent.remove<sup>10+</sup>

remove(id: string, callback: AsyncCallback&lt;void&gt;): void

Removes a specified task of the invoker. If the task is being executed, the task is forced to stop. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type                      | Mandatory | Description                                                  |
| -------- | ------------------------- | --------- | ------------------------------------------------------------ |
| id       | string                    | Yes       | Task ID.                                                     |
| callback | AsyncCallback&lt;void&gt; | Yes       | Callback used to return the result. If the operation is successful, **err** is **undefined**. Otherwise, **err** is an error object. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900006 | task not found error.       |

**Example**

  ```ts
request.agent.remove("123456", (err: BusinessError) => {
  if (err) {
    console.error(`Failed to removing a download task, Code: ${err.code}, message: ${err.message}`);
    return;
  }
  console.info(`Succeeded in creating a download task.`);
});
  ```


## request.agent.remove<sup>10+</sup>

remove(id: string): Promise&lt;void&gt;

Removes a specified task of the invoker. If the task is being executed, the task is forced to stop. This API uses a promise to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name | Type   | Mandatory | Description |
| ---- | ------ | --------- | ----------- |
| id   | string | Yes       | Task ID.    |

**Return value**

| Type                | Description                    |
| ------------------- | ------------------------------ |
| Promise&lt;void&gt; | Promise that returns no value. |

**Error codes**

For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900006 | task not found error.       |

**Example**

  ```ts
request.agent.remove("123456").then(() => {
  console.info(`Succeeded in removing a download task. `);
}).catch((err: BusinessError) => {
  console.error(`Failed to remove a download task, Code: ${err.code}, message: ${err.message}`);
});
  ```


## request.agent.show<sup>10+</sup>

show(id: string, callback: AsyncCallback&lt;TaskInfo&gt;): void

Shows the task details based on the task ID. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type                                         | Mandatory | Description                           |
| -------- | -------------------------------------------- | --------- | ------------------------------------- |
| id       | string                                       | Yes       | Task ID.                              |
| callback | AsyncCallback&lt;[TaskInfo](#taskinfo10)&gt; | Yes       | Callback used to return task details. |

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900006 | task not found error.       |

**Example**

  ```ts
request.agent.show("123456", (err: BusinessError, taskInfo: request.agent.TaskInfo) => {
  if (err) {
    console.error(`Failed to show a upload task, Code: ${err.code}, message: ${err.message}`);
    return;
  }
  console.info(`Succeeded in showing a upload task.`);
});
  ```


## request.agent.show<sup>10+</sup>

show(id: string): Promise&lt;TaskInfo&gt;

Queries a task details based on the task ID. This API uses a promise to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name | Type   | Mandatory | Description |
| ---- | ------ | --------- | ----------- |
| id   | string | Yes       | Task ID.    |

**Return value**

| Type                                   | Description                                  |
| -------------------------------------- | -------------------------------------------- |
| Promise&lt;[TaskInfo](#taskinfo10)&gt; | Promise Promise used to return task details. |

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900006 | task not found error.       |

**Example**

  ```ts
request.agent.show("123456").then((taskInfo: request.agent.TaskInfo) => {
  console.info(`Succeeded in showing the upload task.`);
}).catch((err: BusinessError) => {
  console.error(`Failed to show the upload task. Code: ${err.code}, message: ${err.message}`);
});
  ```


## request.agent.touch<sup>10+</sup>

touch(id: string, token: string, callback: AsyncCallback&lt;TaskInfo&gt;): void

Queries the task details based on the task ID and token. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type                                         | Mandatory | Description                           |
| -------- | -------------------------------------------- | --------- | ------------------------------------- |
| id       | string                                       | Yes       | Task ID.                              |
| token    | string                                       | Yes       | Token for task query.                 |
| callback | AsyncCallback&lt;[TaskInfo](#taskinfo10)&gt; | Yes       | Callback used to return task details. |

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900006 | task not found error.       |

**Example**

  ```ts
request.agent.touch("123456", "token", (err: BusinessError, taskInfo: request.agent.TaskInfo) => {
  if (err) {
    console.error(`Failed to touch an upload task. Code: ${err.code}, message: ${err.message}`);
    return;
  }
  console.info(`Succeeded in touching an upload task.`);
});
  ```


## request.agent.touch<sup>10+</sup>

touch(id: string, token: string): Promise&lt;TaskInfo&gt;

Queries the task details based on the task ID and token. This API uses a promise to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name  | Type   | Mandatory | Description           |
| ----- | ------ | --------- | --------------------- |
| id    | string | Yes       | Task ID.              |
| token | string | Yes       | Token for task query. |

**Return value**

| Type                                   | Description                                  |
| -------------------------------------- | -------------------------------------------- |
| Promise&lt;[TaskInfo](#taskinfo10)&gt; | Promise Promise used to return task details. |

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |
| 21900006 | task not found error.       |

**Example**

  ```ts
request.agent.touch("123456", "token").then((taskInfo: request.agent.TaskInfo) => {
  console.info(`Succeeded in touching a upload task. `);
}).catch((err: BusinessError) => {
  console.error(`Failed to touch an upload task. Code: ${err.code}, message: ${err.message}`);
});
  ```

## request.agent.search<sup>10+</sup>

search(callback: AsyncCallback&lt;Array&lt;string&gt;&gt;): void

Searches for task IDs based on [Filter](#filter10). This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name     | Type                                     | Mandatory | Description                              |
| -------- | ---------------------------------------- | --------- | ---------------------------------------- |
| callback | AsyncCallback&lt;Array&lt;string&gt;&gt; | Yes       | Callback used to return task ID matches. |

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |

**Example**

  ```ts
request.agent.search((err: BusinessError, data: Array<string>) => {
  if (err) {
    console.error(`Upload task search failed. Code: ${err.code}, message: ${err.message}`);
    return;
  }
  console.info(`Upload task search succeeded. `);
});
  ```


## request.agent.search<sup>10+</sup>

search(filter?: Filter): Promise&lt;Array&lt;string&gt;&gt;

Searches for task IDs based on [Filter](#filter10). This API uses a promise to return the result.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name   | Type                | Mandatory | Description      |
| ------ | ------------------- | --------- | ---------------- |
| filter | [Filter](#filter10) | No        | Filter criteria. |

**Return value**

| Type                               | Description                                     |
| ---------------------------------- | ----------------------------------------------- |
| Promise&lt;Array&lt;string&gt;&gt; | Promise Promise used to return task ID matches. |

**Error codes**
For details about the error codes, see [Upload and Download Error Codes](../errorcodes/errorcode-request.md).

| ID       | Error Message               |
| -------- | --------------------------- |
| 13400003 | task service ability error. |

**Example**

  ```ts
let filter: request.agent.Filter = {
  bundle: "com.example.myapplication",
  action: request.agent.Action.UPLOAD,
  mode: request.agent.Mode.FOREGROUND
}
request.agent.search(filter).then((data: Array<string>) => {
  console.info(`Upload task search succeeded. `);
}).catch((err: BusinessError) => {
  console.error(`Upload task search failed. Code: ${err.code}, message: ${err.message}`);
});
  ```

<!--no_check-->