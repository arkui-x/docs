# @ohos.request.cacheDownload (Download and Cache)

The **request** module provides applications with the basic capabilities of file upload and download and background transfer proxy.

- The child component **cacheDownload** provides the basic capability of caching application resources in advance.

- **cacheDownload** uses HTTP and HTTPS to download data and caches data resources to the application memory or specified files in the application sandbox directory.

- The cached data can be used by specific ArkUI components (such as **Image**) to improve resource loading efficiency. Check whether the ArkUI components support this function by referring to the ArkUI component topics.

- ArkUI-X platform support: **cacheDownload** supports the Android and iOS platforms on ArkUI-X. The cached resources can be used directly by the ArkUI **Image** component (for network images). You are advised to pre-cache resources via cacheDownload.download and then load them with **Image** for the best loading experience.

> **NOTE**
>
> The initial APIs of this module are supported since API version 18. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Required Permissions

#### Android Platform

> **NOTE**
>
> When using the cache download APIs on the Android platform, you must configure the network permission. **If the network permission is not configured, runtime exceptions will occur.**
>
> **How to configure**: Add android.permission.INTERNET to the uses-permission field in .arkui-x/android/app/src/main/AndroidManifest.xml (both network access and DNS resolution depend on this permission).

Example:

```
    <uses-permission android:name="android.permission.INTERNET"/>
```

## Modules to Import

```js
import { cacheDownload } from '@kit.BasicServicesKit';
```

## ArkUI-X Platform Differences

The ArkUI-X implementation differs from HarmonyOS native in the following ways. Take note of these differences when using the APIs:

- **cancel**: On the Android platform, after [cancel](#cachedownloadcancel) is called, the task callbacks are removed, but network transmission that has already started is not interrupted (the transfer may continue in the background until it completes; the data already received will be discarded and not written to the cache). On the iOS platform, the network transmission is actually canceled.
- **clearMemoryCache/clearFileCache**: The cache content of running tasks is not cleared; after a task completes, its result still enters the cache. File cache cleanup only covers the cache files indexed within the current session. Cache files left over after the application is restarted are out of the cleanup scope (such files are brought back under cache management when accessed again).
- **Retry scope**: Only transmission errors (DNS, TCP, SSL, and other enumerated errors) trigger a retry; HTTP status code errors (100-599, such as 404 and 503) are not retried.
- **headers**: Entries in the request headers whose value type is not string are silently ignored, without reporting an error.
- **Resources exceeding the size limit**: Resources larger than 20 MB do not enter the memory cache, but in the current version they are still written to the file cache (see [cacheDownload.download](#cachedownloaddownload) for details).
- **Performance data**: The collection scope of PerformanceInfo varies by platform. See the [PerformanceInfo](#performanceinfo20) description for details.

## SslType<sup>21+</sup>

Enumerates secure communication protocols.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name| Value|Description|
| -------- | -------- |-------- |
| TLS | 'TLS' | Uses the TLS secure communication protocol.|
| TLCP | 'TLCP' | Uses the TLCP secure communication protocol. |

> **NOTE**
>
> The iOS platform does not support the TLCP protocol (when sslType is set to TLCP, the task fails and an SSL error is returned through onDownloadError).

## ErrorCode<sup>23+</sup>

Enumerates the specific types of returned error code.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name| Value|Description|
| -------- | -------- |-------- |
| OTHERS | 0xFF | Other types of errors that are not classified.|
| DNS | 0x00 | DNS-related errors.|
| TCP | 0x10 | TCP-related errors.|
| SSL | 0x20 | SSL-related errors.|
| HTTP | 0x30 | HTTP-related errors.|

## CacheStrategy<sup>23+</sup>

Enumerates cache update strategies.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name| Value|Description|
| -------- | -------- |-------- |
| FORCE | 0 | Forcibly updates the cache, regardless of whether the cache already exists.|
| LAZY | 1 | Updates the cache only when the cache does not exist.|

## TimeoutOptions

Task timeout configuration options, including the timeout for establishing a network connection and the timeout for completing an HTTP request.

**Since**: 26.0.0

**System capability**: SystemCapability.Request.FileTransferAgent

| Name  | Type    | Read-Only| Optional| Description                           |
|------|--------|----|----|-------------------------------|
| networkCheckTimeout | number | No | Yes| Timeout for checking the network connection, in seconds. The default value is 20, the minimum value is 0, and the maximum value is 20. An exception is thrown when the value is out of range.<br>ArkUI-X platform difference: On Android, this value is mapped to the TCP connection timeout (connectTimeout; a value of 0 indicates unlimited waiting rather than skipping the check). The iOS platform does not currently use this parameter. |
| httpTotalTimeout | number | No | Yes| Timeout for completing an HTTP request, in seconds. The default value is 60, the minimum value is 1, and the maximum value is 4294967 |

## RetryOptions

Task retry configuration options, which set the maximum number of retries for a task.

**Since**: 26.0.0

**System capability**: SystemCapability.Request.FileTransferAgent

| Name  | Type    | Read-Only| Optional| Description                           |
|------|--------|----|----|-------------------------------|
| maxRetryCount | number | No | Yes| Maximum number of retries when a task fails. The default value is 1, the minimum value is 0, and the maximum value is 10. An exception is thrown when the value is out of range. |

## CacheDownloadOptions

Provides configuration options for download and cache, including HTTP options, transmission options, and task options.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name  | Type    | Read-Only| Optional| Description                           |
|------|--------|----|----|-------------------------------|
| headers | Record\<string, string\> | No | Yes| Request header used by a cache download task during HTTP transfer. The default value is empty.|
| sslType<sup>21+</sup> | [SslType](#ssltype21) | No | Yes| Uses the secure communication protocol TLS or TLCP. TLS is used by default. Currently, neither TLS nor TLCP supports two-way authentication. |
| caPath<sup>21+</sup> | string | No | Yes| CA certificate path. Currently, only certificates in .pem format are supported, and the CA certificate preset by the system is used by default. On ArkUI-X, this parameter takes effect only on the Android platform; on the iOS platform, it does not currently take effect (the system default trust chain is used). |
| cacheStrategy<sup>23+</sup> | [CacheStrategy](#cachestrategy23) | No | Yes| Uses the cache update strategy FORCE or LAZY. The **FORCE** strategy is used by default. |
| retry | [RetryOptions](#retryoptions) | No | Yes| Retry configuration of the task, used to customize the retry behavior of a single task. Set this parameter when you need to specify the retry count for the task; if it is not set, the configuration falls back to the global retry configuration (setGlobalRetryOptions), and then to the default values (the default value of maxRetryCount is 1).<br>**Since**: 26.0.0 |
| timeout | [TimeoutOptions](#timeoutoptions) | No | Yes| Timeout configuration of the task, used to customize the timeout behavior of a single task. Set this parameter when you need to specify the network connection timeout or HTTP request timeout for the task; if it is not set, the configuration falls back to the global timeout configuration (setGlobalTimeoutOptions), and then to the default values (the default value of networkCheckTimeout is 20 and that of httpTotalTimeout is 60).<br>**Since**: 26.0.0 |

## ResourceInfo<sup>20+</sup>

Describes the pre-downloaded resource information.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name  | Type    | Read-Only| Optional| Description                           |
|------|--------|----|----|-------------------------------|
| size | number | Yes | No | Size of a pre-downloaded resource after decompression, in bytes. If the value is a positive integer, the resource is successfully downloaded; if the value is **-1**, the resource fails to be downloaded.|

## NetworkInfo<sup>20+</sup>

Describes the pre-downloaded network information.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name        | Type      | Read-Only| Optional| Description               |
|------------|----------|----|----|-------------------|
| dnsServers | string[] | Yes | No | List of DNS servers used for downloading resources. The current ArkUI-X version does not collect DNS server information and always returns an empty array. |
| ip<sup>23+</sup> | string | Yes | Yes | IP address of the URL used for downloading resources. When the DNS resolution fails, the IP address is undefined.|

## PerformanceInfo<sup>20+</sup>

Describes the pre-downloaded performance information.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name              | Type    | Read-Only| Optional| Description                           |
|------------------|--------|----|----|-------------------------------|
| dnsTime          | number | Yes | No | Time taken from DNS startup to resolution completion, in milliseconds.  |
| connectTime      | number | Yes | No | Time taken from TCP startup to connection completion, in milliseconds.  |
| tlsTime          | number | Yes | No | Time taken from TLS startup to connection completion, in milliseconds.  |
| firstSendTime    | number | Yes | No | Time taken from startup to sending the first byte, in milliseconds.|
| firstReceiveTime | number | Yes | No | Time taken from startup to receiving the first byte, in milliseconds.  |
| totalTime        | number | Yes | No | Time taken from startup to request completion, in milliseconds.     |
| redirectTime     | number | Yes | No | Time taken from startup to redirection completion, in milliseconds.|

> **NOTE**
>
> The current ArkUI-X version collects data on a per-platform basis: the Android platform collects firstReceiveTime and totalTime, and the iOS platform collects only totalTime. The items not collected (dnsTime, connectTime, tlsTime, firstSendTime, redirectTime, and firstReceiveTime on iOS) return 0.

## DownloadInfo<sup>20+</sup>

Describes the pre-downloaded download information.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name         | Type                                   | Read-Only| Optional| Description       |
|-------------|---------------------------------------|----|----|-----------|
| resource    | [ResourceInfo](#resourceinfo20)       | Yes | No | Pre-downloaded resource information.|
| network     | [NetworkInfo](#networkinfo20)         | Yes | No | Pre-downloaded network information.|
| performance | [PerformanceInfo](#performanceinfo20) | Yes | No | Pre-downloaded performance information.|

## DownloadError<sup>23+</sup>

Describes the information returned by the pre-download error callback.

**System capability**: SystemCapability.Request.FileTransferAgent

| Name         | Type                                   | Read-Only| Optional| Description       |
|-------------|---------------------------------------|----|----|-----------|
| errorCode    | [ErrorCode](#errorcode23)       | Yes | No | Specific error type returned by the pre-download error callback.|
| message     | string         | Yes | No | A [universal error code](../errorcodes/errorcode-universal.md) or an [HTTP error code](../errorcodes/errorcode-net-http.md) is returned. |

## cacheDownload.download

download(url: string, options: CacheDownloadOptions): void

Starts a cache download task. If the transfer is successful, the data is downloaded to the memory cache and file cache.

- The size of the target resource after automatic decompression during HTTP transmission cannot exceed 20971520 bytes (20 MB). Otherwise, it is not saved to the memory cache. In the current ArkUI-X version, resources exceeding the limit are still written to the file cache (this is a known deviation on the implementation side, which may be aligned with the memory cache behavior in later versions).

- When caching the downloaded data, the cache refresh behavior is determined by cacheStrategy: when the FORCE policy (default) is used, if cached content already exists under the URL, the new cached content overwrites the old cached content; when the LAZY policy is used, the cache is updated only when no cache exists.

- When the target resource is stored in the memory cache or file cache, if it exceeds the capacity limit of the corresponding cache type, the LRU (least recently used) mode is used by default to replace (evict) the existing cached content.

- This API returns the result synchronously, without blocking the calling thread.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name    | Type                                                        | Mandatory| Description                            |
|---------|------------------------------------------------------------|----|--------------------------------|
| url     | string                                                     | Yes | URL of the target resource. HTTP and HTTPS are supported. The URL length cannot exceed 8192 bytes.|
| options | [CacheDownloadOptions](#cachedownloadoptions) | Yes | Cache download options for the target resource.                  |

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcodes/errorcode-universal.md).

| ID| Error Message                                                                                                                                     |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------|
| 401      | parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  // Provide configuration options for the cache download task.
  let options: cacheDownload.CacheDownloadOptions = {
    headers: { 'Accept': 'application/json' },
    sslType: cacheDownload.SslType.TLS,
    caPath: '/path/to/ca.pem',
    cacheStrategy: cacheDownload.CacheStrategy.FORCE,
    retry: { maxRetryCount: 1 },
    timeout: {
      networkCheckTimeout: 20,
      httpTotalTimeout: 60,
    }
  };

  try {
    // Start the cache download. If the resource is downloaded successfully, it will be cached to the application memory or a specified file in the application sandbox directory.
    cacheDownload.download("https://www.example.com", options);
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.cancel

cancel(url: string): void

Removes an ongoing cache download task based on the URL. The saved memory cache and file cache are not affected.

- If there is no task with the specified URL, this API has no other effect.

- When this API is used for synchronous execution, the calling thread is not blocked.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name | Type    | Mandatory| Description                            |
|------|--------|----|--------------------------------|
| url  | string | Yes | URL of the target resource. HTTP and HTTPS are supported. The URL length cannot exceed 8192 bytes.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcodes/errorcode-universal.md).

| ID| Error Message                                                                                                                                     |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------|
| 401      | parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  // Provide configuration options for the cache download task.
  let options: cacheDownload.CacheDownloadOptions = {};

  try {
    // Start the cache download. If the resource is downloaded successfully, it will be cached to the application memory or a specified file in the application sandbox directory.
    cacheDownload.download("https://www.example.com", options);
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }

  // Handle other service logic.

  try {
    // Remove the cache download task when the task cache is no longer needed. The cached content is not affected.
    cacheDownload.cancel("https://www.example.com");
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to cancel the task. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.setMemoryCacheSize

setMemoryCacheSize(bytes: number): void

Sets the upper limit of the memory cache that the **cacheDownload** component can save.

- When this API is used to adjust the cache size, the LRU (least recently used) mode is used by default to clear redundant cached memory cache content. When this API is used, if **bytes** is set to **0**, all cached memory cache content will be cleared.

- This API returns the result synchronously, without blocking the calling thread.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name  | Type    | Mandatory| Description                                     |
|-------|--------|----|-----------------------------------------|
| bytes | number | Yes | Upper limit of the cache. On ArkUI-X, the default value is 20 MB, the minimum value is 0, and the maximum value cannot exceed 1073741824 bytes (1 GB). If **bytes** is set to **0**, all cached memory cache content will be cleared.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcodes/errorcode-universal.md).

| ID| Error Message                                                                                                                                     |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------|
| 401      | parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    // Set the upper limit of the memory cache size.
    cacheDownload.setMemoryCacheSize(10 * 1024 * 1024);
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to set memory cache size. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.setFileCacheSize

setFileCacheSize(bytes: number): void

Sets the upper limit of the file cache that the **cacheDownload** component can save.

- When this API is used to adjust the cache size, the LRU (least recently used) mode is used by default to clear redundant cached file cache content.

- When this API is used, if **bytes** is set to **0**, all cache files will be deleted. ArkUI-X platform difference: The deletion applies only to the cache files indexed within the current session. Cache files left over after the application is restarted are out of the cleanup scope of this API (such files are brought back under cache management when accessed again).

- This API returns the result synchronously, without blocking the calling thread.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name  | Type    | Mandatory| Description                                                        |
|-------|--------|----|------------------------------------------------------------|
| bytes | number | Yes | Upper limit of the cache. The default value is 104857600 bytes (100 MB), the minimum value is 0, and the maximum value cannot exceed 4294967296 bytes (4 GB).|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcodes/errorcode-universal.md).

| ID| Error Message                                                                                                                                     |
|----------|-------------------------------------------------------------------------------------------------------------------------------------------|
| 401      | parameter error. Possible causes: 1. Missing mandatory parameters. 2. Incorrect parameter type. 3. Parameter verification failed. |

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    // Set the upper limit of the file cache size.
    cacheDownload.setFileCacheSize(100 * 1024 * 1024);
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to set file cache size. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

> **NOTE**
>
> * The network cache files downloaded by the pre-download module are stored in the cache directory of the application sandbox.
> * An application can use this API to clear cache files. NOTE: On ArkUI-X, the cleanup covers only the cache files indexed within the current session (cache files left over after the application is restarted are not affected; for a thorough cleanup, the application can manage the sandbox cache directory on its own).
> * You are advised not to modify the cache directory or files directly, to avoid function exceptions.

## cacheDownload.setDownloadInfoListSize<sup>20+</sup>

setDownloadInfoListSize(size: number): void

Sets the size of the download information list.

- The download information list is used to store pre-download information.

- Download information corresponds to URLs one by one. Each pre-download generates a piece of download information, and only the latest download information is saved for the same URL.

- When this API is used to adjust the list size, if **size** is increased, the original information in the list remains unchanged; if **size** is decreased, the LRU (least recently used) mode is used by default to clear redundant cached information.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name | Type    | Mandatory| Description                                           |
|------|--------|----|-----------------------------------------------|
| size | number | Yes | Size of the download information list. The value ranges from 0 to 8192. The default value is **0**, indicating that no download information is stored. An exception is thrown when the value is out of range.|

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    // Set the size of the download information list.
    cacheDownload.setDownloadInfoListSize(2048);
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to set download information list size. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.getDownloadInfo<sup>20+</sup>

getDownloadInfo(url: string): DownloadInfo | undefined

Obtains the pre-download download information based on the URL. The information is stored in the download information list in memory and is cleared when the application exits. You must call [setDownloadInfoListSize](#cachedownloadsetdownloadinfolistsize20) to set the list size (greater than 0) before pre-download information can be stored in the list. Otherwise, the list size defaults to 0, no download information is stored, and getDownloadInfo will return undefined.

- If the specified URL is found in the download information list, the latest [DownloadInfo](#downloadinfo20) corresponding to the URL is returned.

- If the specified URL cannot be found in the download information list, **undefined** is returned.

- When caching download information, if cached information already exists under the URL, the new cached content will overwrite the old cache.

- When the target information is stored in the memory, the LRU (least recently used) mode is used to replace the existing cached data.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type    | Mandatory| Description                  |
|-----|--------|----|----------------------|
| url | string | Yes | URL to be queried. HTTP and HTTPS are supported. The maximum length is 8192 bytes, and an exception is thrown when the length limit is exceeded.|

**Return value**

| Type                                          | Description                              |
|----------------------------------------------|----------------------------------|
| [DownloadInfo](#downloadinfo20) \| undefined | Returns the download information of the corresponding URL. If the URL is not recorded, **undefined** is returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcodes/errorcode-universal.md).

On ArkUI-X, this API does not perform permission verification and does not throw error code 201; it throws 401 (a universal error code) when the parameters are invalid.

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    // Set the size of the download information list.
    cacheDownload.setDownloadInfoListSize(2048);
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to set download information list size. err code: ${err.code}, err message: ${err.message}`);
  }

  // Provide configuration options for the cache download task.
  let options: cacheDownload.CacheDownloadOptions = {};

  try {
    // Start the cache download. If the resource is downloaded successfully, it will be cached to the application memory or a specified file in the application sandbox directory.
    cacheDownload.download("https://www.example.com", options);
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }

  // Handle other service logic.

  try {
    // Obtain the cache download information after the cache download is complete.
    let downloadInfo = cacheDownload.getDownloadInfo("https://www.example.com");
    if (downloadInfo == undefined) {
      console.error(`CacheDownload get download info undefined.`);
    } else {
      console.info(`CacheDownload get download info : ${JSON.stringify(downloadInfo)}`);
    }
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to get download info. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.clearMemoryCache<sup>23+</sup>

clearMemoryCache(): void

Clears the memory cache of the cached download content.

**System capability**: SystemCapability.Request.FileTransferAgent

**Example**

```ts
import { cacheDownload } from '@kit.BasicServicesKit';

cacheDownload.clearMemoryCache();
```

## cacheDownload.clearFileCache<sup>23+</sup>

clearFileCache(): void

Clears the file cache that stores the downloaded content. ArkUI-X platform difference: Only the cache files indexed within the current session are cleared; cache files left over after the application is restarted are not affected (they are brought back under cache management when accessed again). For a thorough cleanup, the application can manage the sandbox cache directory on its own.

**System capability**: SystemCapability.Request.FileTransferAgent

**Example**

```ts
import { cacheDownload } from '@kit.BasicServicesKit';

cacheDownload.clearFileCache();
```

## cacheDownload.onDownloadSuccess<sup>23+</sup>

onDownloadSuccess(url: string, callback: Callback&lt;void&gt;): void

Subscribes to pre-download completion events. This API uses an asynchronous callback to return the result. Use it together with offDownloadSuccess(): when completion events are no longer needed, call offDownloadSuccess() to unsubscribe, to avoid unnecessary callback overhead.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type    | Mandatory| Description                  |
|-----|--------|----|----------------------|
| url | string | Yes | URL for which the callback is to be registered. HTTP and HTTPS are supported. The URL string can be a maximum of 8192 bytes, and an exception is thrown when the length limit is exceeded. |
| callback | Callback&lt;void&gt; | Yes| Callback invoked when the download is successful. It has no callback parameter.|

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    const successCallback = () => {
      console.info("Succeeded in getting callback from cacheDownload");
    };
    // Subscribe to pre-download completion events. The callback is invoked when the download is complete.
    cacheDownload.onDownloadSuccess("https://www.example.com", successCallback);
    // Start the cache download. If the resource is downloaded successfully, it will be cached to the application memory or a specified file in the application sandbox directory.
    cacheDownload.download("https://www.example.com", {});
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.onDownloadError<sup>23+</sup>

onDownloadError(url: string, callback: Callback&lt;DownloadError&gt;): void

Subscribes to pre-download error events. This API uses an asynchronous callback to return the result. Use it together with offDownloadError(): when error events are no longer needed, call offDownloadError() to unsubscribe, to avoid unnecessary callback overhead.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type    | Mandatory| Description                  |
|-----|--------|----|----------------------|
| url | string | Yes | URL for which the callback is to be registered. HTTP and HTTPS are supported. The URL string can be a maximum of 8192 bytes, and an exception is thrown when the length limit is exceeded. |
| callback | Callback&lt;[DownloadError](#downloaderror23)&gt; | Yes| Callback used to return the error information about the pre-download.|

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    const errorCallback = (error: cacheDownload.DownloadError) => {
      console.error(`Error callback from cacheDownload. error code: ${error.errorCode}, error message: ${error.message}`);
    };
    // Subscribe to pre-download error events. When a download error occurs, the callback is invoked to return error information.
    cacheDownload.onDownloadError("https://www.example.com", errorCallback);
    // Start the cache download. If the resource is downloaded successfully, it will be cached to the application memory or a specified file in the application sandbox directory.
    cacheDownload.download("https://www.example.com", {});
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.offDownloadSuccess<sup>23+</sup>

offDownloadSuccess(url: string, callback?: Callback&lt;void&gt;): void

Unsubscribes from pre-download completion events.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type    | Mandatory| Description                  |
|-----|--------|----|----------------------|
| url | string | Yes | URL for which the callback is to be registered. HTTP and HTTPS are supported. The URL string can be a maximum of 8192 bytes, and an exception is thrown when the length limit is exceeded. |
| callback | Callback&lt;void&gt; | No| Callback to be unregistered. If this parameter is left empty, all completion callbacks of the URL are unsubscribed.|

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    const successCallback = () => {
      console.info("Succeeded in getting callback from cacheDownload");
    };
    // Subscribe to pre-download completion events. The callback is invoked when the download is complete.
    cacheDownload.onDownloadSuccess("https://www.example.com", successCallback);
    // Unsubscribe from pre-download completion events.
    cacheDownload.offDownloadSuccess("https://www.example.com", successCallback);
    // Start the cache download. If the resource is downloaded successfully, it will be cached to the application memory or a specified file in the application sandbox directory.
    cacheDownload.download("https://www.example.com", {});
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.offDownloadError<sup>23+</sup>

offDownloadError(url: string, callback?: Callback&lt;DownloadError&gt;): void

Unsubscribes from pre-download error events.

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type    | Mandatory| Description                  |
|-----|--------|----|----------------------|
| url | string | Yes | URL for which the callback is to be registered. HTTP and HTTPS are supported. The URL string can be a maximum of 8192 bytes, and an exception is thrown when the length limit is exceeded. |
| callback | Callback&lt;[DownloadError](#downloaderror23)&gt; | No| Callback to be unregistered. If this parameter is left empty, all error callbacks of the URL are unsubscribed.|

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    const errorCallback = (error: cacheDownload.DownloadError) => {
      console.error(`Error callback from cacheDownload. error code: ${error.errorCode}, error message: ${error.message}`);
    };
    // Subscribe to pre-download error events. When a download error occurs, the callback is invoked to return error information.
    cacheDownload.onDownloadError("https://www.example.com", errorCallback);
    // Unsubscribe from pre-download error events.
    cacheDownload.offDownloadError("https://www.example.com", errorCallback);
    // Start the cache download. If the resource is downloaded successfully, it will be cached to the application memory or a specified file in the application sandbox directory.
    cacheDownload.download("https://www.example.com", {});
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.setGlobalRetryOptions

setGlobalRetryOptions(options?: RetryOptions): void

Sets the global task retry configuration. This configuration takes effect when a task has no specific retry configuration set. Retry configuration priority: task settings > global settings > default settings.

**Since**: 26.0.0

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type    | Mandatory| Description                  |
|-----|--------|----|----------------------|
| options | [RetryOptions](#retryoptions) | No| Task retry configuration, used to customize the retry behavior of global tasks. Pass in this parameter when you need to adjust policies such as the global retry count; if it is not passed in, the current global retry configuration remains unchanged.|

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    // Set the maximum number of global task retries.
    cacheDownload.setGlobalRetryOptions({
      maxRetryCount: 1
    });
    cacheDownload.download("https://www.example.com", {});
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }
  ```

## cacheDownload.setGlobalTimeoutOptions

setGlobalTimeoutOptions(options?: TimeoutOptions): void

Sets the global task timeout configuration. This configuration takes effect when a task has no specific timeout configuration set. Timeout configuration priority: task settings > global settings > default settings.

**Since**: 26.0.0

**System capability**: SystemCapability.Request.FileTransferAgent

**Parameters**

| Name| Type    | Mandatory| Description                  |
|-----|--------|----|----------------------|
| options | [TimeoutOptions](#timeoutoptions) | No| Task timeout configuration, used to customize the timeout behavior of global tasks. Pass in this parameter when you need to adjust the global network connection timeout or HTTP request timeout; if it is not passed in, the current global timeout configuration remains unchanged.|

**Example**

  ```ts
  import { cacheDownload, BusinessError } from '@kit.BasicServicesKit';

  try {
    // Set the global task timeout configuration.
    cacheDownload.setGlobalTimeoutOptions({
      networkCheckTimeout: 20,
      httpTotalTimeout: 60,
    });
    cacheDownload.download("https://www.example.com", {});
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`Failed to download the resource. err code: ${err.code}, err message: ${err.message}`);
  }
  ```
