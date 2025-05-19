# @ohos.file.fs (File Management)

The **fs** module provides APIs for file operations, including basic file management, directory management, file information statistics, and data read and write using a stream.

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```js
import fs from '@ohos.file.fs';
```

## Guidelines

Before using the APIs provided by this module to perform operations on a file or directory, obtain the application sandbox path of the file or directory as follows:

**Stage Model**

 ```js
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        let context = this.context;
        let pathDir = context.filesDir;
    }
}
 ```

## fs.stat

stat(file: string|number): Promise&lt;Stat&gt;

Obtains detailed file information. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                      |
| ------ | ------ | ---- | -------------------------- |
| file   | string\|number | Yes  | Application sandbox path or file descriptor (FD) of the file.|

**Return value**

  | Type                          | Description        |
  | ---------------------------- | ---------- |
  | Promise&lt;[Stat](#stat)&gt; | Promise used to return the file information obtained.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.stat(filePath).then((stat) => {
      console.info("get file info succeed, the size of file is " + stat.size);
  }).catch((err) => {
      console.info("get file info failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.stat

stat(file: string|number, callback: AsyncCallback&lt;Stat&gt;): void

Obtains detailed file information. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                              | Mandatory| Description                          |
| -------- | ---------------------------------- | ---- | ------------------------------ |
| file     | string\|number                            | Yes  | Application sandbox path or FD of the file.    |
| callback | AsyncCallback&lt;[Stat](#stat)&gt; | Yes  | Callback invoked to return the file information obtained.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  fs.stat(pathDir, (err, stat) => {
    if (err) {
      console.info("get file info failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("get file info succeed, the size of file is " + stat.size);
    }
  });
  ```

## fs.statSync

statSync(file: string|number): Stat

Obtains detailed file information synchronously. 

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                      |
| ------ | ------ | ---- | -------------------------- |
| file   | string\|number | Yes  | Application sandbox path or file descriptor (FD) of the file.|

**Return value**

  | Type           | Description        |
  | ------------- | ---------- |
  | [Stat](#stat) | File information obtained.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let stat = fs.statSync(pathDir);
  console.info("get file info succeed, the size of file is " + stat.size);
  ```

## fs.access

access(path: string, mode?: AccessModeType): Promise&lt;boolean&gt;

Checks whether the file or directory exists or has the operation permission. This API uses a promise to return the result.<br>If the read, write, or read and write permission verification fails, the error code 13900012 (Permission denied) will be thrown.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file or directory.                                  |
| mode<sup>20+</sup>   | [AccessModeType](#accessmodetype20) | No  | Permission on the file or directory to check. If this parameter is left blank, the system checks whether the file exists.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;boolean&gt; | Promise used to return a Boolean value. The value **true** means the file exists; the value **false** means the opposite.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

| ID                    | Error Message       |
| ---------------------------- | ---------- |
| 401 | 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.|
| 13900020 | Invalid parameter value.|

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  fs.access(filePath).then((res: boolean) => {
    if (res) {
      console.info("file exists");
    } else {
      console.info("file not exists");
    }
  }).catch((err: BusinessError) => {
    console.error("access failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.access

access(path: string, callback: AsyncCallback&lt;boolean&gt;): void

Checks whether a file or directory exists. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                     | Mandatory| Description                                                        |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                    | Yes  | Application sandbox path of the file or directory.                                  |
| callback | AsyncCallback&lt;boolean&gt; | Yes  | Callback used to return the result. The value **true** means the file exists; the value **false** means the opposite.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

| ID                    | Error Message       |
| ---------------------------- | ---------- |
| 401 | 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.|
| 13900020 | Invalid parameter value.|

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  fs.access(filePath, (err: BusinessError, res: boolean) => {
    if (err) {
      console.error("access failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      if (res) {
        console.info("file exists");
      } else {
        console.info("file not exists");
      }
    }
  });
  ```

## fs.accessSync

accessSync(path: string, mode?: AccessModeType): boolean

Checks whether a file or directory exists or has the operation permission. This API returns the result synchronously.<br>If the read, write, or read and write permission verification fails, the error code 13900012 (Permission denied) will be thrown.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file or directory.                                  |
| mode<sup>20+</sup>   | [AccessModeType](#accessmodetype20) | No  | Permission on the file or directory to check. If this parameter is left blank, the system checks whether the file or directory exists.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | boolean | The value **true** means the file exists; the value **false** means the opposite.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

| ID                    | Error Message       |
| ---------------------------- | ---------- |
| 401 | 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.|
| 13900020 | Invalid parameter value.|

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  try {
    let res = fs.accessSync(filePath);
    if (res) {
      console.info("file exists");
    } else {
      console.info("file not exists");
    }
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error("accessSync failed with error message: " + err.message + ", error code: " + err.code);
  }
  ```

## fs.close

close(file: File|number): Promise&lt;void&gt;

Closes a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description          |
  | ---- | ------ | ---- | ------------ |
  | file   | [File](#file)\|number | Yes   | File object or FD of the file to close.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.close(file).then(() => {
      console.info("File closed");
      fs.closeSync(file);
  }).catch((err) => {
      console.info("close file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.close

close(file: File|number, callback: AsyncCallback&lt;void&gt;): void

Closes a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                       | Mandatory  | Description          |
  | -------- | ------------------------- | ---- | ------------ |
  | file       | [File](#file)\|number                  | Yes   | File object or FD of the file to close.|
  | callback | AsyncCallback&lt;void&gt; | Yes   | Callback invoked immediately after the file is closed.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.close(file, (err) => {
    if (err) {
      console.info("close file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("close file success");
    }
  });
  ```

## fs.closeSync

closeSync(file: File|number): void

Synchronously closes a file.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description          |
  | ---- | ------ | ---- | ------------ |
  | file   | [File](#file)\|number | Yes   | File object or FD of the file to close.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.closeSync(file);
  ```

## fs.copyFile

copyFile(src: string|number, dest: string|number, mode?: number): Promise&lt;void&gt;

Copies a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type                        | Mandatory  | Description                                      |
  | ---- | -------------------------- | ---- | ---------------------------------------- |
  | src  | string\|number | Yes   | Path or FD of the file to copy.                     |
  | dest | string\|number | Yes   | Destination path of the file or FD of the file created.                         |
  | mode | number                     | No   | Whether to overwrite the file with the same name in the destination directory. The default value is **0**, which is the only value supported.<br>**0**: overwrite the file with the same name.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcPath = pathDir + "/srcDir/test.txt";
  let dstPath = pathDir + "/dstDir/test.txt";
  fs.copyFile(srcPath, dstPath).then(() => {
      console.info("copy file succeed");
  }).catch((err) => {
      console.info("copy file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.copyFile

copyFile(src: string|number, dest: string|number, mode?: number, callback: AsyncCallback&lt;void&gt;): void

Copies a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                        | Mandatory  | Description                                      |
  | -------- | -------------------------- | ---- | ---------------------------------------- |
  | src      | string\|number | Yes   | Path or FD of the file to copy.                     |
  | dest     | string\|number | Yes   | Destination path of the file or FD of the file created.                         |
  | mode     | number                     | No   | Whether to overwrite the file with the same name in the destination directory. The default value is **0**, which is the only value supported.<br>**0**: overwrite the file with the same name and truncate the part that is not overwritten.|
  | callback | AsyncCallback&lt;void&gt;  | Yes   | Callback invoked immediately after the file is copied.                            |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcPath = pathDir + "/srcDir/test.txt";
  let dstPath = pathDir + "/dstDir/test.txt";
  fs.copyFile(srcPath, dstPath, (err) => {
    if (err) {
      console.info("copy file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("copy file success");
    }
  });
  ```

## fs.copyFileSync

copyFileSync(src: string|number, dest: string|number, mode?: number): void

Synchronously copies a file.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type                        | Mandatory  | Description                                      |
  | ---- | -------------------------- | ---- | ---------------------------------------- |
  | src  | string\|number | Yes   | Path or FD of the file to copy.                     |
  | dest | string\|number | Yes   | Destination path of the file or FD of the file created.                         |
  | mode | number                     | No   | Whether to overwrite the file with the same name in the destination directory. The default value is **0**, which is the only value supported.<br>**0**: overwrite the file with the same name and truncate the part that is not overwritten.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcPath = pathDir + "/srcDir/test.txt";
  let dstPath = pathDir + "/dstDir/test.txt";
  fs.copyFileSync(srcPath, dstPath);
  ```

## fs.copyDir<sup>20+</sup>

copyDir(src: string, dest: string, mode?: number): Promise\<void>

Copies the source directory to the destination path. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | mode | number | No   | Copy mode. The default value is **0**.<br>- **0**: Throw an exception if a file conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory, and a file with the same name exists in the conflict directory. All the non-conflicting files in the source directory will be moved to the destination directory, and the non-conflicting files in the destination directory will be retained. The data attribute in the error returned provides information about the conflicting files in the Array\<[ConflictFiles](#conflictfiles20)> format.<br>- **1**: Forcibly overwrite the files with the same name in the destination directory.<br> When the destination directory contains a directory with the same name as the source directory, the files with the same names in the destination directory are overwritten forcibly; the files without conflicts in the destination directory are retained.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  // Copy srcPath to destPath.
  let srcPath = pathDir + "/srcDir/";
  let destPath = pathDir + "/destDir/";
  fs.copyDir(srcPath, destPath, 0).then(() => {
    console.info("copy directory succeed");
  }).catch((err: BusinessError) => {
    console.error("copy directory failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.copyDir<sup>20+</sup>

copyDir(src: string, dest: string, mode: number, callback: AsyncCallback\<void, Array\<ConflictFiles>>): void

Copies the source directory to the destination path. You can set the copy mode. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | mode | number | Yes   | Copy mode. The default value is **0**.<br>- **0**: Throw an exception if a file conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory, and a file with the same name exists in the conflict directory. All the non-conflicting files in the source directory will be moved to the destination directory, and the non-conflicting files in the destination directory will be retained. The data attribute in the error returned provides information about the conflicting files in the Array\<[ConflictFiles](#conflictfiles20)> format.<br>- **1**: Forcibly overwrite the files with the same name in the destination directory.<br> When the destination directory contains a directory with the same name as the source directory, the files with the same names in the destination directory are overwritten forcibly; the files without conflicts in the destination directory are retained.|
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | Yes   | Callback used to return the result.             |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ConflictFiles } from '@kit.CoreFileKit';
  // Copy srcPath to destPath.
  let srcPath = pathDir + "/srcDir/";
  let destPath = pathDir + "/destDir/";
  fs.copyDir(srcPath, destPath, 0, (err: BusinessError<Array<ConflictFiles>>) => {
    if (err && err.code == 13900015 && err.data?.length !== undefined) {
      for (let i = 0; i < err.data.length; i++) {
        console.error("copy directory failed with conflicting files: " + err.data[i].srcFile + " " + err.data[i].destFile);
      }
    } else if (err) {
      console.error("copy directory failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("copy directory succeed");
    }
  });
  ```

## fs.copyDir<sup>20+</sup>

copyDir(src: string, dest: string, callback: AsyncCallback\<void, Array\<ConflictFiles>>): void

Copies the source directory to the destination path. This API uses an asynchronous callback to return the result.

An exception will be thrown if the destination directory contains a directory with the same name as the source directory and there are files with the same name in the conflicting directory. All the non-conflicting files in the source directory will be moved to the destination directory, and the non-conflicting files in the destination directory will be retained. The data attribute in the error returned provides information about the conflicting files in the Array\<[ConflictFiles](#conflictfiles20)> format.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | Yes   | Callback used to return the result.             |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ConflictFiles } from '@kit.CoreFileKit';
  // Copy srcPath to destPath.
  let srcPath = pathDir + "/srcDir/";
  let destPath = pathDir + "/destDir/";
  fs.copyDir(srcPath, destPath, (err: BusinessError<Array<ConflictFiles>>) => {
    if (err && err.code == 13900015 && err.data?.length !== undefined) {
      for (let i = 0; i < err.data.length; i++) {
        console.error("copy directory failed with conflicting files: " + err.data[i].srcFile + " " + err.data[i].destFile);
      }
    } else if (err) {
      console.error("copy directory failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("copy directory succeed");
    }
  });
  ```

## fs.copyDirSync<sup>20+</sup>

copyDirSync(src: string, dest: string, mode?: number): void

Copies the source directory to the destination path. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | mode | number | No   | Copy mode. The default value is **0**.<br>- **0**: Throw an exception if a file conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory, and a file with the same name exists in the conflict directory. All the non-conflicting files in the source directory will be moved to the destination directory, and the non-conflicting files in the destination directory will be retained. The data attribute in the error returned provides information about the conflicting files in the Array\<[ConflictFiles](#conflictfiles20)> format.<br>- **1**: Forcibly overwrite the files with the same name in the destination directory.<br> When the destination directory contains a directory with the same name as the source directory, the files with the same names in the destination directory are overwritten forcibly; the files without conflicts in the destination directory are retained.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  // Copy srcPath to destPath.
  let srcPath = pathDir + "/srcDir/";
  let destPath = pathDir + "/destDir/";
  try {
    fs.copyDirSync(srcPath, destPath, 0);
    console.info("copy directory succeed");
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error("copy directory failed with error message: " + err.message + ", error code: " + err.code);
  }
  ```

## fs.setxattr<sup>20+</sup>

setxattr(path: string, key: string, value: string): Promise&lt;void&gt;

Sets an extended attribute of a file or directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file or directory.                                  |
| key   | string | Yes  | Key of the extended attribute to obtain. The value is a string of less than 256 bytes and can contain only the **user.** prefix. |
| value   | string | Yes  | Value of the extended attribute to set.                                  |

**Return value**

  | Type    | Description                                      |
  | ------ | ---------------------------------------- |
  | Promise&lt;void&gt;| Promise that returns no value.                            |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';

  let filePath = pathDir + "/test.txt";
  let attrKey = "user.comment";
  let attrValue = "Test file.";

  fs.setxattr(filePath, attrKey, attrValue).then(() => {
    console.info("Set extended attribute successfully.");
  }).catch((err: BusinessError) => {
    console.error("Failed to set extended attribute with error message: " + err.message + ", error code: " + err.code);
  });

  ```

## fs.setxattrSync<sup>20+</sup>

setxattrSync(path: string, key: string, value: string): void

Sets an extended attribute of a file or directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file or directory.                                  |
| key   | string | Yes  | Key of the extended attribute to obtain. The value is a string of less than 256 bytes and can contain only the **user.** prefix.  |
| value   | string | Yes  | Value of the extended attribute to set.                                  |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';

  let filePath = pathDir + "/test.txt";
  let attrKey = "user.comment";
  let attrValue = "Test file.";

  try {
    fs.setxattrSync(filePath, attrKey, attrValue);
    console.info("Set extended attribute successfully.");
  } catch (err) {
    console.error("Failed to set extended attribute with error message: " + err.message + ", error code: " + err.code);
  }

  ```

## fs.getxattr<sup>20+</sup>

getxattr(path: string, key: string): Promise&lt;string&gt;

Obtains an extended attribute of a file or directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file or directory.                                  |
| key   | string | Yes  | Key of the extended attribute to obtain.                                  |

**Return value**

  | Type    | Description                                      |
  | ------ | ---------------------------------------- |
  | Promise&lt;string&gt;| Promise used to return the value of the extended attribute obtained.   |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';

  let filePath = pathDir + "/test.txt";
  let attrKey = "user.comment";

  fs.getxattr(filePath, attrKey).then((attrValue: string) => {
    console.info("Get extended attribute succeed, the value is: " + attrValue);
  }).catch((err: BusinessError) => {
    console.error("Failed to get extended attribute with error message: " + err.message + ", error code: " + err.code);
  });

  ```

## fs.getxattrSync<sup>20+</sup>

getxattrSync(path: string, key: string): string

Obtains an extended attribute of a file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file or directory.                                  |
| key   | string | Yes  | Key of the extended attribute to obtain.                                  |

**Return value**

  | Type    | Description                                      |
  | ------ | ---------------------------------------- |
  | key| Value of the extended attribute obtained.     |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';

  let filePath = pathDir + "/test.txt";
  let attrKey = "user.comment";

  try {
    let attrValue = fs.getxattrSync(filePath, attrKey);
    console.info("Get extended attribute succeed, the value is: " + attrValue);
  } catch (err) {
    console.error("Failed to get extended attribute with error message: " + err.message + ", error code: " + err.code);
    }

  ```

## fs.mkdir

mkdir(path: string): Promise&lt;void&gt;

Creates a directory. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the directory.                                  |

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.mkdir(dirPath).then(() => {
      console.info("Directory created");
  }).catch((err) => {
      console.info("mkdir failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.mkdir<sup>20+</sup>

mkdir(path: string, recursion: boolean): Promise\<void>

Creates a directory. This API uses a promise to return the result. The value **true** means to create a directory recursively.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the directory.                                  |
| recursion   | boolean | Yes  | Whether to create a directory recursively.<br>The value **true** means to create a directory recursively. The value **false** means to create a single-level directory.  |

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let dirPath = pathDir + "/testDir1/testDir2/testDir3";
  fs.mkdir(dirPath, true).then(() => {
    console.info("Directory created");
  }).catch((err: BusinessError) => {
    console.error("mkdir failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.mkdir

mkdir(path: string, callback: AsyncCallback&lt;void&gt;): void

Creates a directory. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                     | Mandatory| Description                                                        |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                    | Yes  | Application sandbox path of the directory.                                  |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked when the directory is created asynchronously.                            |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.mkdir(dirPath, (err) => {
    if (err) {
      console.info("mkdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("mkdir success");
    }
  });
  ```

## fs.mkdir<sup>20+</sup>

mkdir(path: string, recursion: boolean, callback: AsyncCallback&lt;void&gt;): void

Creates a directory. This API uses an asynchronous callback to return the result. The value **true** means to create a directory recursively.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                     | Mandatory| Description                                                        |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                    | Yes  | Application sandbox path of the directory.                                  |
| recursion   | boolean | Yes  | Whether to create a directory recursively.<br>The value **true** means to create a directory recursively. The value **false** means to create a single-level directory.  |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback used to return the result.                            |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let dirPath = pathDir + "/testDir1/testDir2/testDir3";
  fs.mkdir(dirPath, true, (err: BusinessError) => {
    if (err) {
      console.error("mkdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("Directory created");
    }
  });
  ```

## fs.mkdirSync

mkdirSync(path: string): void

Synchronously creates a directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the directory.                                  |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.mkdirSync(dirPath);
  ```

## fs.mkdirSync<sup>20+</sup>

mkdirSync(path: string, recursion: boolean): void

Creates a directory. This API returns the result synchronously. The value **true** means to create a directory recursively.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the directory.                                  |
| recursion   | boolean | Yes  | Whether to create a directory recursively.<br>The value **true** means to create a directory recursively. The value **false** means to create a single-level directory.  |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let dirPath = pathDir + "/testDir1/testDir2/testDir3";
  fs.mkdirSync(dirPath, true);
  ```

## fs.open

open(path: string, mode?: number): Promise&lt;File&gt;

Opens a file. This API uses a promise to return the result. File uniform resource identifiers (URIs) are supported. 

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path or URI of the file.                                  |
| mode  | number | No  | [Mode](#openmode) for opening the file. You must specify one of the following options. By default, the file is open in read-only mode.<br>- **OpenMode.READ_ONLY(0o0)**: Open the file in read-only mode.<br>- **OpenMode.WRITE_ONLY(0o1)**: Open the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Open the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the file exists and is open in write-only or read/write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Open the file in append mode. New data will be added to the end of the file.<br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the open file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Open the file in synchronous I/O mode.|

**Return value**

  | Type                   | Description         |
  | --------------------- | ----------- |
  | Promise&lt;[File](#file)&gt; | Promise used to return the file object.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.open(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE).then((file) => {
      console.info("file fd: " + file.fd);
  }).catch((err) => {
      console.info("open file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.open

open(path: string, mode?: number, callback: AsyncCallback&lt;File&gt;): void

Opens a file. This API uses an asynchronous callback to return the result. File URIs are supported. 

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                           | Mandatory| Description                                                        |
| -------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                          | Yes  | Application sandbox path or URI of the file.                                  |
| mode  | number | No  | [Mode](#openmode) for opening the file. You must specify one of the following options. By default, the file is open in read-only mode.<br>- **OpenMode.READ_ONLY(0o0)**: Open the file in read-only mode.<br>- **OpenMode.WRITE_ONLY(0o1)**: Open the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Open the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the file exists and is open in write-only or read/write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Open the file in append mode. New data will be added to the end of the file.<br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the open file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Open the file in synchronous I/O mode.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.open(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE, (err, file) => {
    if (err) {
      console.info("mkdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("file fd: " + file.fd);
    }
  });
  ```

## fs.openSync

openSync(path: string, mode?: number): File

Synchronously opens a file. File URIs are supported. 

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path or URI of the file.                                  |
| mode  | number | No  | [Mode](#openmode) for opening the file. You must specify one of the following options. By default, the file is open in read-only mode.<br>- **OpenMode.READ_ONLY(0o0)**: Open the file in read-only mode.<br>- **OpenMode.WRITE_ONLY(0o1)**: Open the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Open the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the file exists and is open in write-only or read/write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Open the file in append mode. New data will be added to the end of the file.<br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the open file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Open the file in synchronous I/O mode.|

**Return value**

  | Type    | Description         |
  | ------ | ----------- |
  | [File](#file) | File object opened.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  console.info("file fd: " + file.fd);
  fs.closeSync(file);
  ```

## fs.read

read(fd: number, buffer: ArrayBuffer, options?: ReadOptions): Promise&lt;number&gt;

Reads file data. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name | Type       | Mandatory| Description                                                        |
| ------- | ----------- | ---- | ------------------------------------------------------------ |
| fd      | number      | Yes  | FD of the file.                                    |
| buffer  | ArrayBuffer | Yes  | Buffer used to store the file data read.                          |
| options | [ReadOptions](#readoptions20)      | No  | The options are as follows:<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.|

**Return value**

  | Type                                | Description    |
  | ---------------------------------- | ------ |
  | Promise&lt;number&gt; | Promise used to return the length of the data read, in bytes.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { buffer } from '@kit.ArkTS';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE);
  let arrayBuffer = new ArrayBuffer(4096);
  fs.read(file.fd, arrayBuffer).then((readLen: number) => {
    console.info("Read file data successfully");
    let buf = buffer.from(arrayBuffer, 0, readLen);
    console.info(`The content of file: ${buf.toString()}`);
  }).catch((err: BusinessError) => {
    console.error("read file data failed with error message: " + err.message + ", error code: " + err.code);
  }).finally(() => {
    fs.closeSync(file);
  });
  ```

## fs.read

read(fd: number, buffer: ArrayBuffer, options?: ReadOptions, callback: AsyncCallback&lt;number&gt;): void

Reads data from a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                                      | Mandatory  | Description                                      |
  | -------- | ---------------------------------------- | ---- | ---------------------------------------- |
  | fd       | number                                   | Yes   | FD of the file.                            |
  | buffer   | ArrayBuffer                              | Yes   | Buffer used to store the file data read.                       |
  | options | [ReadOptions](#readoptions11)      | No  | The options are as follows:<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.|
  | callback | AsyncCallback&lt;number&gt; | Yes   | Callback used to return the length of the data read, in bytes.                            |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { buffer } from '@kit.ArkTS';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE);
  let arrayBuffer = new ArrayBuffer(4096);
  fs.read(file.fd, arrayBuffer, (err: BusinessError, readLen: number) => {
    if (err) {
      console.error("read failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("Read file data successfully");
      let buf = buffer.from(arrayBuffer, 0, readLen);
      console.info(`The content of file: ${buf.toString()}`);
    }
    fs.closeSync(file);
  });
  ```

## fs.readSync

readSync(fd: number, buffer: ArrayBuffer, options?: ReadOptions): number

Reads data from a file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type         | Mandatory  | Description                                      |
  | ------- | ----------- | ---- | ---------------------------------------- |
  | fd      | number      | Yes   | FD of the file.                            |
  | buffer  | ArrayBuffer | Yes   | Buffer used to store the file data read.                       |
  | options | [ReadOptions](#readoptions20)      | No  | The options are as follows:<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.|

**Return value**

  | Type    | Description      |
  | ------ | -------- |
  | number | Length of the data read, in bytes.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE);
  let buf = new ArrayBuffer(4096);
  fs.readSync(file.fd, buf);
  fs.closeSync(file);
  ```

## fs.rmdir

rmdir(path: string): Promise&lt;void&gt;

Deletes a directory. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                      |
| ------ | ------ | ---- | -------------------------- |
| path   | string | Yes  | Application sandbox path of the directory.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.rmdir(dirPath).then(() => {
      console.info("Directory deleted");
  }).catch((err) => {
      console.info("rmdir failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.rmdir

rmdir(path: string, callback: AsyncCallback&lt;void&gt;): void

Deletes a directory. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                     | Mandatory| Description                      |
| -------- | ------------------------- | ---- | -------------------------- |
| path     | string                    | Yes  | Application sandbox path of the directory.|
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked when the directory is deleted asynchronously.  |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.rmdir(dirPath, (err) => {
    if (err) {
      console.info("rmdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("Directory deleted");
    }
  });
  ```

## fs.rmdirSync

rmdirSync(path: string): void

Synchronously deletes a directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                      |
| ------ | ------ | ---- | -------------------------- |
| path   | string | Yes  | Application sandbox path of the directory.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.rmdirSync(dirPath);
  ```

## fs.unlink

unlink(path: string): Promise&lt;void&gt;

Deletes a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                      |
| ------ | ------ | ---- | -------------------------- |
| path   | string | Yes  | Application sandbox path of the file.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.unlink(filePath).then(() => {
      console.info("File deleted");
  }).catch((err) => {
      console.info("remove file failed with error message: " + err.message + ", error code: " + err.codeor);
  });
  ```

## fs.unlink

unlink(path: string, callback: AsyncCallback&lt;void&gt;): void

Deletes a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                     | Mandatory| Description                      |
| -------- | ------------------------- | ---- | -------------------------- |
| path     | string                    | Yes  | Application sandbox path of the file.|
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked immediately after the file is deleted.  |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.unlink(filePath, (err) => {
    if (err) {
      console.info("remove file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("File deleted");
    }
  });
  ```

## fs.unlinkSync

unlinkSync(path: string): void

Synchronously deletes a file.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                      |
| ------ | ------ | ---- | -------------------------- |
| path   | string | Yes  | Application sandbox path of the file.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.unlinkSync(filePath);
  ```

## fs.write

write(fd: number, buffer: ArrayBuffer | string, options?: WriteOptions): Promise&lt;number&gt;

Writes data to a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type                             | Mandatory  | Description                                      |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | fd      | number                          | Yes   | FD of the file.                            |
  | buffer  | ArrayBuffer \| string | Yes   | Data to write. It can be a string or data from a buffer.                    |
  | options | [WriteOptions](#writeoptions20)                          | No   | The options are as follows:<br>- **offset** (number): start position to write the data in the file. This parameter is optional. By default, data is written from the current position.<br>- **length** (number): length of the data to write. This parameter is optional. The default value is the buffer length.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported currently.|

**Return value**

  | Type                   | Description      |
  | --------------------- | -------- |
  | Promise&lt;number&gt; | Promise used to return the length of the data written, in bytes.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  let str: string = "hello, world";
  fs.write(file.fd, str).then((writeLen: number) => {
    console.info("write data to file succeed and size is:" + writeLen);
  }).catch((err: BusinessError) => {
    console.error("write data to file failed with error message: " + err.message + ", error code: " + err.code);
  }).finally(() => {
    fs.closeSync(file);
  });
  ```

## fs.write

write(fd: number, buffer: ArrayBuffer | string, options?: WriteOptions, callback: AsyncCallback&lt;number&gt;): void

Writes data to a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                             | Mandatory  | Description                                      |
  | -------- | ------------------------------- | ---- | ---------------------------------------- |
  | fd       | number                          | Yes   | FD of the file.                            |
  | buffer   | ArrayBuffer \| string | Yes   | Data to write. It can be a string or data from a buffer.                    |
  | options | [WriteOptions](#writeoptions20)                          | No   | The options are as follows:<br>- **offset** (number): start position to write the data in the file. This parameter is optional. By default, data is written from the current position.<br>- **length** (number): length of the data to write. This parameter is optional. The default value is the buffer length.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported currently.|
  | callback | AsyncCallback&lt;number&gt;     | Yes   | Callback used to return the result.                      |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  let str: string = "hello, world";
  fs.write(file.fd, str, (err: BusinessError, writeLen: number) => {
    if (err) {
      console.error("write data to file failed with error message:" + err.message + ", error code: " + err.code);
    } else {
      console.info("write data to file succeed and size is:" + writeLen);
    }
    fs.closeSync(file);
  });
  ```

## fs.writeSync

writeSync(fd: number, buffer: ArrayBuffer | string, options?: WriteOptions): number

Writes data to a file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type                             | Mandatory  | Description                                      |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | fd      | number                          | Yes   | FD of the file.                            |
  | buffer  | ArrayBuffer \| string | Yes   | Data to write. It can be a string or data from a buffer.                    |
  | options | [WriteOptions](#writeoptions20)                          | No   | The options are as follows:<br>- **offset** (number): start position to write the data in the file. This parameter is optional. By default, data is written from the current position.<br>- **length** (number): length of the data to write. This parameter is optional. The default value is the buffer length.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported currently.|

**Return value**

  | Type    | Description      |
  | ------ | -------- |
  | number | Length of the data written, in bytes.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  let str: string = "hello, world";
  let writeLen = fs.writeSync(file.fd, str);
  console.info("write data to file succeed and size is:" + writeLen);
  fs.closeSync(file);
  ```

## fs.truncate

truncate(file: string|number, len?: number): Promise&lt;void&gt;

Truncates a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                            |
| ------ | ------ | ---- | -------------------------------- |
| file   | string\|number | Yes  | Application sandbox path or FD of the file.      |
| len    | number | No  | File length, in bytes, after truncation. The default value is **0**.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let len = 5;
  fs.truncate(filePath, len).then(() => {
      console.info("File truncated");
  }).catch((err) => {
      console.info("truncate file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.truncate

truncate(file: string|number, len?: number, callback: AsyncCallback&lt;void&gt;): void

Truncates a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                     | Mandatory| Description                            |
| -------- | ------------------------- | ---- | -------------------------------- |
| file     | string\|number                    | Yes  | Application sandbox path or FD of the file.      |
| len      | number                    | No  | File length, in bytes, after truncation. The default value is **0**.|
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback that returns no value.  |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let len = 5;
  fs.truncate(filePath, len, (err) => {
    if (err) {
      console.info("truncate failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("truncate success");
    }
  });
  ```

## fs.truncateSync

truncateSync(file: string|number, len?: number): void

Synchronously truncates a file.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                            |
| ------ | ------ | ---- | -------------------------------- |
| file   | string\|number | Yes  | Application sandbox path or FD of the file.      |
| len    | number | No  | File length, in bytes, after truncation. The default value is **0**.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let len = 5;
  fs.truncateSync(filePath, len);
  ```

## fs.readLines<sup>20+</sup>

readLines(filePath: string, options?: Options): Promise&lt;ReaderIterator&gt;

Reads the text content of a file line by line. This API uses a promise to return the result. Only the files in UTF-8 format are supported.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | string | Yes  | Application sandbox path of the file.                                  |
| options | [Options](#options20) | No  | Options for reading the text. The options are as follows:<br>- **encoding** (string): format of the data to be encoded.<br>It is valid only when the data is of the string type.<br>The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type                   | Description        |
  | --------------------- | ---------- |
  | Promise&lt;[ReaderIterator](#readeriterator20)&gt; | Promise used to return a **ReaderIterator** object.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, Options } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let options: Options = {
    encoding: 'utf-8'
  };
  fs.readLines(filePath, options).then((readerIterator: fs.ReaderIterator) => {
    for (let it = readerIterator.next(); !it.done; it = readerIterator.next()) {
      console.info("content: " + it.value);
    }
  }).catch((err: BusinessError) => {
    console.error("readLines failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.readLines<sup>20+</sup>

readLines(filePath: string, options?: Options, callback: AsyncCallback&lt;ReaderIterator&gt;): void

Reads a file text line by line. This API uses an asynchronous callback to return the result. Only the files in UTF-8 format are supported.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | string | Yes  | Application sandbox path of the file.                                  |
| options | [Options](#options20) | No  | Options for reading the text. The options are as follows:<br>- **encoding** (string): format of the data to be encoded.<br>It is valid only when the data is of the string type.<br>The default value is **'utf-8'**, which is the only value supported.|
| callback | AsyncCallback&lt;[ReaderIterator](#readeriterator20)&gt; | Yes  | Callback used to return a **ReaderIterator** object.                                  |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, Options } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let options: Options = {
    encoding: 'utf-8'
  };
  fs.readLines(filePath, options, (err: BusinessError, readerIterator: fs.ReaderIterator) => {
    if (err) {
      console.error("readLines failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      for (let it = readerIterator.next(); !it.done; it = readerIterator.next()) {
        console.info("content: " + it.value);
      }
    }
  });
  ```

## fs.readLinesSync<sup>20+</sup>

readLinesSync(filePath: string, options?: Options): ReaderIterator

Reads the text content of a file line by line. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | string | Yes  | Application sandbox path of the file.                                  |
| options | [Options](#options20) | No  | Options for reading the text. The options are as follows:<br>- **encoding** (string): format of the data to be encoded.<br>It is valid only when the data is of the string type.<br>The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type                   | Description        |
  | --------------------- | ---------- |
  | [ReaderIterator](#readeriterator20) | **ReaderIterator** object.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { fileIo as fs, Options } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let options: Options = {
    encoding: 'utf-8'
  };
  let readerIterator = fs.readLinesSync(filePath, options);
  for (let it = readerIterator.next(); !it.done; it = readerIterator.next()) {
    console.info("content: " + it.value);
  }
  ```

## ReaderIterator<sup>20+</sup>

Provides a **ReaderIterator** object. Before calling APIs of **ReaderIterator**, you need to use **readLines()** to create a **ReaderIterator** instance.

### next<sup>20+</sup>

next(): ReaderIteratorResult

Obtains the **ReaderIterator** result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type                   | Description        |
  | --------------------- | ---------- |
  | [ReaderIteratorResult](#readeriteratorresult20) | **ReaderIteratorResult** object obtained.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

> **NOTE**
>
> If the line read by ReaderIterator is not in UTF-8 format, error code 13900037 will be returned.

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, Options } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let options: Options = {
    encoding: 'utf-8'
  };
  fs.readLines(filePath, options).then((readerIterator: fs.ReaderIterator) => {
    for (let it = readerIterator.next(); !it.done; it = readerIterator.next()) {
      console.info("content: " + it.value);
    }
  }).catch((err: BusinessError) => {
    console.error("readLines failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## ReaderIteratorResult<sup>20+</sup>

Represents the information obtained by the **ReaderIterator** object.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Description               |
| ----------- | --------------- | ------------------ |
| done | boolean     |  Whether the iteration is complete. The value **true** means the iteration is complete; the value **false** means the opposite.         |
| value    | string     | File text content read line by line.|

## fs.readText

readText(filePath: string, options?: ReadTextOptions): Promise&lt;string&gt;

Reads the text content of a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | string | Yes  | Application sandbox path of the file.                                  |
| options  | [ReadTextOptions](#readtextoptions20) | No  | The options are as follows:<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the file length.<br>- **encoding** (string): format of the data to be encoded.<br>It is valid only when the data is of the string type. The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type                   | Description        |
  | --------------------- | ---------- |
  | Promise&lt;string&gt; | Promise used to return the file content read.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  fs.readText(filePath).then((str: string) => {
    console.info("readText succeed:" + str);
  }).catch((err: BusinessError) => {
    console.error("readText failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.readText

readText(filePath: string, options?: ReadTextOptions, callback: AsyncCallback&lt;string&gt;): void

Reads the text content of a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                       | Mandatory| Description                                                        |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| filePath | string                      | Yes  | Application sandbox path of the file.                                  |
| options  | [ReadTextOptions](#readtextoptions20)                      | No  | The options are as follows:<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the file length.<br>- **encoding** (string): format of the data to be encoded. The default value is **'utf-8'**, which is the only value supported.|
| callback | AsyncCallback&lt;string&gt; | Yes  | Callback used to return the content read.                        |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ReadTextOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let readTextOption: ReadTextOptions = {
      offset: 1,
      length: 0,
      encoding: 'utf-8'
  };
  let stat = fs.statSync(filePath);
  readTextOption.length = stat.size;
  fs.readText(filePath, readTextOption, (err: BusinessError, str: string) => {
    if (err) {
      console.error("readText failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("readText succeed:" + str);
    }
  });
  ```

## fs.readTextSync

readTextSync(filePath: string, options?: ReadTextOptions): string

Reads the text content of a file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | string | Yes  | Application sandbox path of the file.                                  |
| options  | [ReadTextOptions](#readtextoptions20) | No  | The options are as follows:<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the file length.<br>- **encoding** (string): format of the data to be encoded.<br>It is valid only when the data is of the string type. The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type  | Description                |
  | ------ | -------------------- |
  | string | Content of the file read.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { fileIo as fs, ReadTextOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let readTextOptions: ReadTextOptions = {
    offset: 1,
    length: 0,
    encoding: 'utf-8'
  };
  let stat = fs.statSync(filePath);
  readTextOptions.length = stat.size;
  let str = fs.readTextSync(filePath, readTextOptions);
  console.info("readText succeed:" + str);
  ```

## fs.lstat

lstat(path: string): Promise&lt;Stat&gt;

Obtains information about a symbolic link. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                  |
| ------ | ------ | ---- | -------------------------------------- |
| path   | string | Yes  | Application sandbox path of the file.|

**Return value**

  | Type                          | Description        |
  | ---------------------------- | ---------- |
  | Promise&lt;[Stat](#stat)&gt; | Promise used to return the symbolic link information obtained. For details, see **stat**.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.lstat(filePath).then((stat) => {
      console.info("get link status succeed, the size of file is" + stat.size);
  }).catch((err) => {
      console.info("get link status failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.lstat

lstat(path: string, callback: AsyncCallback&lt;Stat&gt;): void

Obtains information about a symbolic link. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                              | Mandatory| Description                                  |
| -------- | ---------------------------------- | ---- | -------------------------------------- |
| path     | string                             | Yes  | Application sandbox path of the file.|
| callback | AsyncCallback&lt;[Stat](#stat)&gt; | Yes  | Callback invoked to return the symbolic link information obtained.      |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.lstat(filePath, (err, stat) => {
      if (err) {
        console.info("lstat failed with error message: " + err.message + ", error code: " + err.code);
      } else {
        console.info("get link status succeed, the size of file is" + stat.size);
      }
  });
  ```

## fs.lstatSync

lstatSync(path: string): Stat

Obtains information about a symbolic link synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                  |
| ------ | ------ | ---- | -------------------------------------- |
| path   | string | Yes  | Application sandbox path of the file.|

**Return value**

  | Type           | Description        |
  | ------------- | ---------- |
  | [Stat](#stat) | File information obtained.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let stat = fs.lstatSync(filePath);
  ```

## fs.rename

rename(oldPath: string, newPath: string): Promise&lt;void&gt;

Renames a file or folder. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name | Type  | Mandatory| Description                        |
| ------- | ------ | ---- | ---------------------------- |
| oldPath | string | Yes  | Application sandbox path of the file to rename.|
| newPath | string | Yes  | Application sandbox path of the renamed file.  |

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcFile = pathDir + "/test.txt";
  let dstFile = pathDir + "/new.txt";
  fs.rename(srcFile, dstFile).then(() => {
      console.info("File renamed");
  }).catch((err) => {
      console.info("rename failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.rename

rename(oldPath: string, newPath: string, callback: AsyncCallback&lt;void&gt;): void

Renames a file or folder. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                     | Mandatory| Description                        |
| -------- | ------------------------- | ---- | ---------------------------- |
| oldPath | string | Yes  | Application sandbox path of the file to rename.|
| newPath | string | Yes  | Application sandbox path of the renamed file.  |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked when the file is asynchronously renamed.  |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcFile = pathDir + "/test.txt";
  let dstFile = pathDir + "/new.txt";
  fs.rename(srcFile, dstFile, (err) => {
    if (err) {
      console.info("rename failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("rename success");
    }
  });
  ```

## fs.renameSync

renameSync(oldPath: string, newPath: string): void

Renames a file or folder synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name | Type  | Mandatory| Description                        |
| ------- | ------ | ---- | ---------------------------- |
| oldPath | string | Yes  | Application sandbox path of the file to rename.|
| newPath | string | Yes  | Application sandbox path of the renamed file.  |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcFile = pathDir + "/test.txt";
  let dstFile = pathDir + "/new.txt";
  fs.renameSync(srcFile, dstFile);
  ```

## fs.fsync

fsync(fd: number): Promise&lt;void&gt;

Flushes data of a file to disk. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description          |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | Yes   | FD of the file.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fsync(file.fd).then(() => {
      console.info("Data flushed");
  }).catch((err) => {
      console.info("sync data failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.fsync

fsync(fd: number, callback: AsyncCallback&lt;void&gt;): void

Flushes data of a file to disk. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                       | Mandatory  | Description             |
  | -------- | ------------------------- | ---- | --------------- |
  | fd       | number                    | Yes   | FD of the file.   |
  | Callback | AsyncCallback&lt;void&gt; | Yes   | Callback invoked when the file data is synchronized in asynchronous mode.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fsync(file.fd, (err) => {
    if (err) {
      console.info("fsync failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("fsync success");
      fs.closeSync(file);
    }
  });
  ```

## fs.fsyncSync

fsyncSync(fd: number): void

Flushes data of a file to disk synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description          |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | Yes   | FD of the file.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fsyncSync(file.fd);
  fs.closeSync(file);
  ```

## fs.fdatasync

fdatasync(fd: number): Promise&lt;void&gt;

Flushes data of a file to disk. This API uses a promise to return the result. **fdatasync()** is similar to **fsync()**, but does not flush modified metadata unless that metadata is needed.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description          |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | Yes   | FD of the file.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fdatasync(file.fd).then((err) => {
    console.info("Data flushed");
    fs.closeSync(file);
  }).catch((err) => {
    console.info("sync data failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.fdatasync

fdatasync(fd: number, callback: AsyncCallback&lt;void&gt;): void

Flushes data of a file to disk. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                             | Mandatory  | Description               |
  | -------- | ------------------------------- | ---- | ----------------- |
  | fd       | number                          | Yes   | FD of the file.     |
  | callback | AsyncCallback&lt;void&gt; | Yes   | Callback invoked when the file data is synchronized in asynchronous mode.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fdatasync (file.fd, (err) => {
    if (err) {
      console.info("fdatasync failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("fdatasync success");
      fs.closeSync(file);
    }
  });
  ```

## fs.fdatasyncSync

fdatasyncSync(fd: number): void

Synchronizes data in a file synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description          |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | Yes   | FD of the file.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  let stat = fs.fdatasyncSync(file.fd);
  fs.closeSync(file);
  ```

## fs.listFile
listFile(path: string, options?: {
    recursion?: boolean;
    listNum?: number;
    filter?: Filter;
}): Promise<string[]>

Lists all files in a folder. This API uses a promise to return the result.<br>This API supports recursive listing of all files (including files in subfolders) and file filtering.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | path | string | Yes   | Application sandbox path of the folder.|
  | options | Object | No   | File filtering options. The files are not filtered by default.|

**options parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | recursion | boolean | No   | Whether to list all files in subfolders recursively. The default value is **false**.|
  | listNum | number | No   | Number of file names to list. The default value **0** means to list all files.|
  | filter | [Filter](#filter) | No   | File filtering options. Currently, only the match by file name extension, fuzzy search by file name, and filter by file size or latest modification time are supported.|

**Return value**

  | Type                  | Description        |
  | --------------------- | ---------- |
  | Promise&lt;string[]&gt; | Promise used to return the files names listed.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let options = {
    "recursion": false,
    "listNum": 0,
    "filter": {
      "suffix": [".png", ".jpg", ".jpeg"],
      "displayName": ["%abc", "efg%"],
      "fileSizeOver": 1024,
      "lastModifiedAfter": new Date().getTime(),
    }
  };
  fs.listFile(pathDir, options).then((filenames) => {
    console.info("listFile succeed");
    for (let i = 0; i < filenames.length; i++) {
      console.info("fileName: %s", filenames[i]);
    }
  }).catch((err) => {
      console.info("list file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.listFile
listFile(path: string, options?: {
    recursion?: boolean;
    listNum?: number;
    filter?: Filter;
}, callback: AsyncCallback<string[]>): void

Lists all files in a folder. This API uses an asynchronous callback to return the result.<br>This API supports recursive listing of all files (including files in subfolders) and file filtering.

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | path | string | Yes   | Application sandbox path of the folder.|
  | options | Object | No   | File filtering options. The files are not filtered by default.|
  | callback | AsyncCallback&lt;string[]&gt; | Yes   | Callback invoked to return the file names listed.             |

**options parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | recursion | boolean | No   | Whether to list all files in subfolders recursively. The default value is **false**.|
  | listNum | number | No   | Number of file names to list. The default value **0** means to list all files.|
  | filter | [Filter](#filter) | No   | File filtering options. Currently, only the match by file name extension, fuzzy search by file name, and filter by file size or latest modification time are supported.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let options = {
    "recursion": false,
    "listNum": 0,
    "filter": {
      "suffix": [".png", ".jpg", ".jpeg"],
      "displayName": ["%abc", "efg%"],
      "fileSizeOver": 1024,
      "lastModifiedAfter": new Date().getTime(),
    }
  };
  fs.listFile(pathDir, options, (err, filenames) => {
    if (err) {
      console.info("list file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("listFile succeed");
      for (let i = 0; i < filenames.length; i++) {
        console.info("filename: %s", filenames[i]);
      }
    }
  });
  ```

## fs.listFileSync

listFileSync(path: string, options?: {
    recursion?: boolean;
    listNum?: number;
    filter?: Filter;
}): string[]

Lists all files in a folder synchronously. This API supports recursive listing of all files (including files in subfolders) and file filtering.

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | path | string | Yes   | Application sandbox path of the folder.|
  | options | Object | No   | File filtering options. The files are not filtered by default.|

**options parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | recursion | boolean | No   | Whether to list all files in subfolders recursively. The default value is **false**.|
  | listNum | number | No   | Number of file names to list. The default value **0** means to list all files.|
  | filter | [Filter](#filter) | No   | File filtering options. Currently, only the match by file name extension, fuzzy search by file name, and filter by file size or latest modification time are supported.|

**Return value**

  | Type                  | Description        |
  | --------------------- | ---------- |
  | string[] | File names listed.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let options = {
    "recursion": false,
    "listNum": 0,
    "filter": {
      "suffix": [".png", ".jpg", ".jpeg"],
      "displayName": ["%abc", "efg%"],
      "fileSizeOver": 1024,
      "lastModifiedAfter": new Date().getTime(),
    }
  };
  let filenames = fs.listFileSync(pathDir, options);
  console.info("listFile succeed");
  for (let i = 0; i < filenames.length; i++) {
    console.info("filename: %s", filenames[i]);
  }
  ```

## fs.lseek<sup>20+</sup>

lseek(fd: number, offset: number, whence?: WhenceType): number

Adjusts the position of the file offset pointer.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | fd | number | Yes   | FD of the file.|
  | offset | number | Yes   | Relative offset, in bytes.|
  | whence | [WhenceType](#whencetype20) | No   | Where to start the offset. If this parameter is not specified, the file start position is used by default.|

**Return value**

  | Type                  | Description        |
  | --------------------- | ---------- |
  | number | Position of the current offset as measured from the beginning of the file, in bytes.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  console.info('The current offset is at ' + fs.lseek(file.fd, 5, fs.WhenceType.SEEK_SET));
  fs.closeSync(file);
  ```

## fs.moveDir<sup>20+</sup>

moveDir(src: string, dest: string, mode?: number): Promise\<void>

Moves the source directory to the destination directory. This API uses a promise to return the result.

> **NOTE**
>
> This API is not supported in a distributed directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | mode | number | No   | Move mode. The default value is **0**.<br>- **0**: Throw an exception if a directory conflict occurs.<br> An exception will be thrown if the destination directory contains a non-empty directory with the same name as the source directory.<br>- **1**: Throw an exception if a file conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory, and a file with the same name exists in the conflict directory. All the non-conflicting files in the source directory will be moved to the destination directory, and the non-conflicting files in the destination directory will be retained. The data attribute in the error returned provides information about the conflicting files in the Array\<[ConflictFiles](#conflictfiles20)> format.<br>- **2**: Forcibly overwrite the conflicting files in the destination directory.<br> When the destination directory contains a directory with the same name as the source directory, the files with the same names in the destination directory are overwritten forcibly; the files without conflicts in the destination directory are retained.<br>- **3**: Forcibly overwrite the conflicting directory.<br> The source directory is moved to the destination directory, and the content of the moved directory is the same as that of the source directory. If the destination directory contains a directory with the same name as the source directory, all original files in the directory will be deleted.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  // move directory from srcPath to destPath
  let srcPath = pathDir + "/srcDir/";
  let destPath = pathDir + "/destDir/";
  fs.moveDir(srcPath, destPath, 1).then(() => {
    console.info("move directory succeed");
  }).catch((err: BusinessError) => {
    console.error("move directory failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.moveDir<sup>20+</sup>

moveDir(src: string, dest: string, mode: number, callback: AsyncCallback\<void, Array\<ConflictFiles>>): void

Moves the source directory to the destination directory. You can set the move mode. This API uses an asynchronous callback to return the result.

> **NOTE**
>
> This API is not supported in a distributed directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | mode | number | Yes   | Move mode. The default value is **0**.<br>- **0**: Throw an exception if a directory conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory.<br>- **1**: Throw an exception if a file conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory, and a file with the same name exists in the conflict directory. All the non-conflicting files in the source directory will be moved to the destination directory, and the non-conflicting files in the destination directory will be retained. The data attribute in the error returned provides information about the conflicting files in the Array\<[ConflictFiles](#conflictfiles20)> format.<br>- **2**: Forcibly overwrite the conflicting files in the destination directory.<br> When the destination directory contains a directory with the same name as the source directory, the files with the same names in the destination directory are overwritten forcibly; the files without conflicts in the destination directory are retained.<br>- **3**: Forcibly overwrite the conflicting directory.<br> The source directory is moved to the destination directory, and the content of the moved directory is the same as that of the source directory. If the destination directory contains a directory with the same name as the source directory, all original files in the directory will be deleted.|
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | Yes   | Callback used to return the result.             |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ConflictFiles } from '@kit.CoreFileKit';
  // move directory from srcPath to destPath
  let srcPath = pathDir + "/srcDir/";
  let destPath = pathDir + "/destDir/";
  fs.moveDir(srcPath, destPath, 1, (err: BusinessError<Array<ConflictFiles>>) => {
    if (err && err.code == 13900015 && err.data?.length !== undefined) {
      for (let i = 0; i < err.data.length; i++) {
        console.error("move directory failed with conflicting files: " + err.data[i].srcFile + " " + err.data[i].destFile);
      }
    } else if (err) {
      console.error("move directory failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("move directory succeed");
    }
  });
  ```

## fs.moveDir<sup>20+</sup>

moveDir(src: string, dest: string, callback: AsyncCallback\<void, Array\<ConflictFiles>>): void

Moves the source directory to the destination directory. This API uses an asynchronous callback to return the result.

An exception will be thrown if a directory conflict occurs, that is, the destination directory contains a directory with the same name as the source directory.

> **NOTE**
>
> This API is not supported in a distributed directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | Yes   | Callback used to return the result.             |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ConflictFiles } from '@kit.CoreFileKit';
  // move directory from srcPath to destPath
  let srcPath = pathDir + "/srcDir/";
  let destPath = pathDir + "/destDir/";
  fs.moveDir(srcPath, destPath, (err: BusinessError<Array<ConflictFiles>>) => {
    if (err && err.code == 13900015 && err.data?.length !== undefined) {
      for (let i = 0; i < err.data.length; i++) {
        console.error("move directory failed with conflicting files: " + err.data[i].srcFile + " " + err.data[i].destFile);
      }
    } else if (err) {
      console.error("move directory failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("move directory succeed");
    }
  });
  ```

## fs.moveDirSync<sup>20+</sup>

moveDirSync(src: string, dest: string, mode?: number): void

Moves the source directory to the destination directory. This API returns the result synchronously.

> **NOTE**
>
> This API is not supported in a distributed directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source directory.|
  | dest | string | Yes   | Application sandbox path of the destination directory.|
  | mode | number | No   | Move mode. The default value is **0**.<br>- **0**: Throw an exception if a directory conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory.<br>- **1**: Throw an exception if a file conflict occurs.<br> An exception will be thrown if the destination directory contains a directory with the same name as the source directory, and a file with the same name exists in the conflict directory. All the non-conflicting files in the source directory will be moved to the destination directory, and the non-conflicting files in the destination directory will be retained. The data attribute in the error returned provides information about the conflicting files in the Array\<[ConflictFiles](#conflictfiles20)> format.<br>- **2**: Forcibly overwrite the conflicting files in the destination directory.<br> When the destination directory contains a directory with the same name as the source directory, the files with the same names in the destination directory are overwritten forcibly; the files without conflicts in the destination directory are retained.<br>- **3**: Forcibly overwrite the conflicting directory.<br> The source directory is moved to the destination directory, and the content of the moved directory is the same as that of the source directory. If the destination directory contains a directory with the same name as the source directory, all original files in the directory will be deleted.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
import { fileIo as fs, ConflictFiles } from '@kit.CoreFileKit';
// move directory from srcPath to destPath
let srcPath = pathDir + "/srcDir/";
let destPath = pathDir + "/destDir/";
try {
  fs.moveDirSync(srcPath, destPath, 1);
  console.info("move directory succeed");
} catch (error) {
  let err: BusinessError<Array<ConflictFiles>> = error as BusinessError<Array<ConflictFiles>>;
  if (err.code == 13900015 && err.data?.length !== undefined) {
    for (let i = 0; i < err.data.length; i++) {
      console.error("move directory failed with conflicting files: " + err.data[i].srcFile + " " + err.data[i].destFile);
    }
  } else {
    console.error("move directory failed with error message: " + err.message + ", error code: " + err.code);
  }
}
  ```

## fs.moveFile

moveFile(src: string, dest: string, mode?: number): Promise\<void>

Moves a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source file.|
  | dest | string | Yes   | Application sandbox path of the destination file.|
  | mode | number | No   | Whether to overwrite the file with the same name in the destination directory.<br>The value **0** means to overwrite the file with the same name in the destination directory; the value **1** means to throw an exception.<br>The default value is **0**.|

**Return value**

  | Type                 | Description                          |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcPath = pathDir + "/source.txt";
  let destPath = pathDir + "/dest.txt";
  fs.moveFile(srcPath, destPath, 0).then(() => {
      console.info("move file succeed");
  }).catch((err) => {
      console.info("move file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.moveFile

moveFile(src: string, dest: string, mode?: number, callback: AsyncCallback\<void>): void

Moves a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source file.|
  | dest | string | Yes   | Application sandbox path of the destination file.|
  | mode | number | No   | Whether to overwrite the file with the same name in the destination directory.<br>The value **0** means to overwrite the file with the same name in the destination directory; the value **1** means to throw an exception. <br>The default value is **0**.|
  | callback | AsyncCallback&lt;void&gt; | Yes   | Callback invoked when the file is moved.             |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcPath = pathDir + "/source.txt";
  let destPath = pathDir + "/dest.txt";
  fs.moveFile(srcPath, destPath, 0, (err) => {
    if (err) {
      console.info("move file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("move file succeed");
    }  
  });
  ```

## fs.moveFileSync

moveFile(src: string, dest: string, mode?: number): void

Moves a file synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | Yes   | Application sandbox path of the source file.|
  | dest | string | Yes   | Application sandbox path of the destination file.|
  | mode | number | No   | Whether to overwrite the file with the same name in the destination directory.<br>The value **0** means to overwrite the file with the same name in the destination directory; the value **1** means to throw an exception. <br>The default value is **0**.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let srcPath = pathDir + "/source.txt";
  let destPath = pathDir + "/dest.txt";
  fs.moveFileSync(srcPath, destPath, 0);
  console.info("move file succeed");
  ```

## fs.mkdtemp

mkdtemp(prefix: string): Promise&lt;string&gt;

Creates a temporary directory. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | prefix | string | Yes   | A randomly generated string used to replace "XXXXXX" in a directory.|

**Return value**

  | Type                  | Description        |
  | --------------------- | ---------- |
  | Promise&lt;string&gt; | Promise used to return the unique directory generated.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  fs.mkdtemp(pathDir + "/XXXXXX").then((pathDir) => {
      console.info("mkdtemp succeed:" + pathDir);
  }).catch((err) => {
      console.info("mkdtemp failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.mkdtemp

mkdtemp(prefix: string, callback: AsyncCallback&lt;string&gt;): void

Creates a temporary directory. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                         | Mandatory  | Description                         |
  | -------- | --------------------------- | ---- | --------------------------- |
  | prefix   | string                      | Yes   | A randomly generated string used to replace "XXXXXX" in a directory.|
  | callback | AsyncCallback&lt;string&gt; | Yes   | Callback invoked when a temporary directory is created asynchronously.             |

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  fs.mkdtemp(pathDir + "/XXXXXX", (err, res) => {
    if (err) {
      console.info("mkdtemp failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("mkdtemp success");
    }
  });
  ```

## fs.mkdtempSync

mkdtempSync(prefix: string): string

Synchronously creates a temporary directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | prefix | string | Yes   | A randomly generated string used to replace "XXXXXX" in a directory.|

**Return value**

  | Type   | Description        |
  | ------ | ---------- |
  | string | Unique path generated.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let res = fs.mkdtempSync(pathDir + "/XXXXXX");
  ```  

## fs.utimes<sup>20+</sup>

utimes(path: string, mtime: number): void

Updates the latest access timestamp of a file.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**
|    Name   | Type    | Mandatory  | Description                         |
| ------------ | ------ | ------ | ------------------------------------------------------------ |
| path  | string  |  Yes   | Application sandbox path of the file.|
| mtime  | number  |  Yes  | New timestamp. The value is the number of milliseconds elapsed since the Epoch time (00:00:00 UTC on January 1, 1970). Only the last access time of a file can be modified.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  fs.writeSync(file.fd, 'test data');
  fs.closeSync(file);
  fs.utimes(filePath, new Date().getTime());
  ```

## fs.createRandomAccessFile<sup>20+</sup>

createRandomAccessFile(file: string | File, mode?: number): Promise&lt;RandomAccessFile&gt;

Creates a **RandomAccessFile** instance based on the specified file path or file object. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**
|    Name   | Type    | Mandatory  | Description                         |
| ------------ | ------ | ------ | ------------------------------------------------------------ |
|     file     | string \| [File](#file) | Yes   | Application sandbox path of the file or an opened file object.|
|     mode     | number | No  | [Mode](#openmode) for creating the **RandomAccessFile** instance. This parameter is valid only when the application sandbox path of the file is passed in. One of the following options must be specified:<br>- **OpenMode.READ_ONLY(0o0)**: Create the file in read-only mode. This is the default value.<br>- **OpenMode.WRITE_ONLY(0o1)**: Create the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Create the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the **RandomAccessFile** object already exists and is created in write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Create the file in append mode. New data will be added to the end of the **RandomAccessFile** object. <br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the created file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception. The write permission is not allowed.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Create a **RandomAccessFile** instance in synchronous I/O mode.|

**Return value**

  | Type                               | Description       |
  | --------------------------------- | --------- |
  | Promise&lt;[RandomAccessFile](#randomaccessfile20)&gt; | Promise used to return the **RandomAccessFile** instance created.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  fs.createRandomAccessFile(file).then((randomAccessFile: fs.RandomAccessFile) => {
    console.info("randomAccessFile fd: " + randomAccessFile.fd);
    randomAccessFile.close();
  }).catch((err: BusinessError) => {
    console.error("create randomAccessFile failed with error message: " + err.message + ", error code: " + err.code);
  }).finally(() => {
    fs.closeSync(file);
  });
  ```

## fs.createRandomAccessFile<sup>20+</sup>

createRandomAccessFile(file: string | File, callback: AsyncCallback&lt;RandomAccessFile&gt;): void

Creates a **RandomAccessFile** object in read-only mode based on a file path or file object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

|  Name   | Type    | Mandatory  | Description                         |
| ------------ | ------ | ------ | ------------------------------------------------------------ |
|     file     | string \| [File](#file) | Yes   | Application sandbox path of the file or an opened file object.|
| callback | AsyncCallback&lt;[RandomAccessFile](#randomaccessfile20)&gt; | Yes  | Callback used to return the **RandomAccessFile** instance created.                                  |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**
  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  fs.createRandomAccessFile(file, (err: BusinessError, randomAccessFile: fs.RandomAccessFile) => {
    if (err) {
      console.error("create randomAccessFile failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("randomAccessFile fd: " + randomAccessFile.fd);
      randomAccessFile.close();
    }
    fs.closeSync(file);
  });
  ```

## fs.createRandomAccessFile<sup>20+</sup>

createRandomAccessFile(file: string | File, mode: number, callback: AsyncCallback&lt;RandomAccessFile&gt;): void

Creates a **RandomAccessFile** instance based on a file path or file object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

|  Name   | Type    | Mandatory  | Description                         |
| ------------ | ------ | ------ | ------------------------------------------------------------ |
|     file     | string \| [File](#file) | Yes   | Application sandbox path of the file or an opened file object.|
|     mode     | number | Yes  | [Mode](#openmode) for creating the **RandomAccessFile** instance. This parameter is valid only when the application sandbox path of the file is passed in. One of the following options must be specified:<br>- **OpenMode.READ_ONLY(0o0)**: Create the file in read-only mode. This is the default value.<br>- **OpenMode.WRITE_ONLY(0o1)**: Create the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Create the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the **RandomAccessFile** object already exists and is created in write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Create the file in append mode. New data will be added to the end of the **RandomAccessFile** object. <br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the created file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception. The write permission is not allowed.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Create a **RandomAccessFile** instance in synchronous I/O mode.|
| callback | AsyncCallback&lt;[RandomAccessFile](#randomaccessfile20)&gt; | Yes  | Callback used to return the **RandomAccessFile** instance created.                                  |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**
  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  fs.createRandomAccessFile(file, fs.OpenMode.READ_ONLY, (err: BusinessError, randomAccessFile: fs.RandomAccessFile) => {
    if (err) {
      console.error("create randomAccessFile failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("randomAccessFile fd: " + randomAccessFile.fd);
      randomAccessFile.close();
    }
    fs.closeSync(file);
  });
  ```

## fs.createRandomAccessFile<sup>20+</sup>

createRandomAccessFile(file: string | File, mode?: number, options?: RandomAccessFileOptions): Promise&lt;RandomAccessFile&gt;

Creates a **RandomAccessFile** instance based on the specified file path or file object. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

|  Name   | Type    | Mandatory  | Description                         |
| ------------ | ------ | ------ | ------------------------------------------------------------ |
|     file     | string \| [File](#file) | Yes   | Application sandbox path of the file or an opened file object.|
|     mode     | number | No  | [Mode](#openmode) for creating the **RandomAccessFile** instance. This parameter is valid only when the application sandbox path of the file is passed in. One of the following options must be specified:<br>- **OpenMode.READ_ONLY(0o0)**: Create the file in read-only mode. This is the default value.<br>- **OpenMode.WRITE_ONLY(0o1)**: Create the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Create the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the **RandomAccessFile** object already exists and is created in write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Create the file in append mode. New data will be added to the end of the **RandomAccessFile** object. <br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the created file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception. The write permission is not allowed.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Create a **RandomAccessFile** instance in synchronous I/O mode.|
|options|[RandomAccessFileOptions](#randomaccessfileoptions20)|No|The options are as follows:<br>- **start** (number): start position of the data to read in the file. This parameter is optional. By default, data is read from the current position.<br>- **end** (number): end position of the data to read in the file. This parameter is optional. The default value is the end of the file.|

**Return value**

  | Type                               | Description       |
  | --------------------------------- | --------- |
  | Promise&lt;[RandomAccessFile](#randomaccessfile20)&gt; | Promise used to return the **RandomAccessFile** instance created.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

```ts
import { BusinessError } from '@kit.BasicServicesKit';
let filePath = pathDir + "/test.txt";
fs.createRandomAccessFile(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE, { start: 10, end: 100 })
  .then((randomAccessFile: fs.RandomAccessFile) => {
    console.info("randomAccessFile fd: " + randomAccessFile.fd);
    randomAccessFile.close();
  })
  .catch((err: BusinessError) => {
    console.error("create randomAccessFile failed with error message: " + err.message + ", error code: " + err.code);
  });
```

## fs.createRandomAccessFileSync<sup>20+</sup>

createRandomAccessFileSync(file: string | File, mode?: number): RandomAccessFile

Creates a **RandomAccessFile** instance based on a file path or file object.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

|  Name   | Type    | Mandatory  | Description                         |
| ------------ | ------ | ------ | ------------------------------------------------------------ |
|     file     | string \| [File](#file) | Yes   | Application sandbox path of the file or an opened file object.|
|     mode     | number | No  | [Mode](#openmode) for creating the **RandomAccessFile** instance. This parameter is valid only when the application sandbox path of the file is passed in. One of the following options must be specified:<br>- **OpenMode.READ_ONLY(0o0)**: Create the file in read-only mode. This is the default value.<br>- **OpenMode.WRITE_ONLY(0o1)**: Create the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Create the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the **RandomAccessFile** object already exists and is created in write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Create the file in append mode. New data will be added to the end of the **RandomAccessFile** object. <br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the created file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception. The write permission is not allowed.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Create a **RandomAccessFile** instance in synchronous I/O mode.|

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [RandomAccessFile](#randomaccessfile20) | **RandomAccessFile** instance created.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  let randomAccessFile = fs.createRandomAccessFileSync(file);
  randomAccessFile.close();
  ```

## fs.createRandomAccessFileSync<sup>20+</sup>

createRandomAccessFileSync(file: string | File, mode?: number,
  options?: RandomAccessFileOptions): RandomAccessFile

Creates a **RandomAccessFile** instance based on a file path or file object.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

|  Name   | Type    | Mandatory  | Description                         |
| ------------ | ------ | ------ | ------------------------------------------------------------ |
|     file     | string \| [File](#file) | Yes   | Application sandbox path of the file or an opened file object.|
|     mode     | number | No  | [Mode](#openmode) for creating the **RandomAccessFile** instance. This parameter is valid only when the application sandbox path of the file is passed in. One of the following options must be specified:<br>- **OpenMode.READ_ONLY(0o0)**: Create the file in read-only mode. This is the default value.<br>- **OpenMode.WRITE_ONLY(0o1)**: Create the file in write-only mode.<br>- **OpenMode.READ_WRITE(0o2)**: Create the file in read/write mode.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the **RandomAccessFile** object already exists and is created in write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Create the file in append mode. New data will be added to the end of the **RandomAccessFile** object. <br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the created file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception. The write permission is not allowed.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Create a **RandomAccessFile** instance in synchronous I/O mode.|
|options|[RandomAccessFileOptions](#randomaccessfileoptions20)|No|The options are as follows:<br>- **start** (number): start position of the data to read in the file. This parameter is optional. By default, data is read from the current position.<br>- **end** (number): end position of the data to read in the file. This parameter is optional. The default value is the end of the file.|

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [RandomAccessFile](#randomaccessfile20) | **RandomAccessFile** instance created.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let randomAccessFile = fs.createRandomAccessFileSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE,
    { start: 10, end: 100 });
  randomAccessFile.close();
  ```

## fs.createStream<sup>20+</sup>

createStream(path: string, mode: string): Promise&lt;Stream&gt;

Creates a stream based on a file path. This API uses a promise to return the result. To close the stream, use **close()** of [Stream20](#stream).

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file.                                  |
| mode   | string | Yes  | - **r**: Open a file for reading. The file must exist.<br>- **r+**: Open a file for both reading and writing. The file must exist.<br>- **w**: Open a file for writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **w+**: Open a file for both reading and writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **a**: Open a file in append mode for writing at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).<br>- **a+**: Open a file in append mode for reading or updating at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).|

**Return value**

  | Type                               | Description       |
  | --------------------------------- | --------- |
  | Promise&lt;[Stream](#stream20)&gt; | Promise used to return the stream opened.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  fs.createStream(filePath, "a+").then((stream: fs.Stream) => {
    stream.closeSync();
    console.info("Stream created");
  }).catch((err: BusinessError) => {
    console.error("createStream failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.createStream<sup>20+</sup>

createStream(path: string, mode: string, callback: AsyncCallback&lt;Stream&gt;): void

Creates a stream based on a file path. This API uses an asynchronous callback to return the result. To close the stream, use **close()** of [Stream](#stream20).

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name  | Type                                   | Mandatory| Description                                                        |
| -------- | --------------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                                  | Yes  | Application sandbox path of the file.                                  |
| mode     | string                                  | Yes  | - **r**: Open a file for reading. The file must exist.<br>- **r+**: Open a file for both reading and writing. The file must exist.<br>- **w**: Open a file for writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **w+**: Open a file for both reading and writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **a**: Open a file in append mode for writing at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).<br>- **a+**: Open a file in append mode for reading or updating at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).|
| callback | AsyncCallback&lt;[Stream](#stream20)&gt; | Yes  | Callback used to return the result.                                  |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  fs.createStream(filePath, "r+", (err: BusinessError, stream: fs.Stream) => {
    if (err) {
      console.error("create stream failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("Stream created");
    }
    stream.closeSync();
  })
  ```

## fs.createStreamSync<sup>20+</sup>

createStreamSync(path: string, mode: string): Stream

Creates a stream based on a file path. This API returns the result synchronously. To close the stream, use **close()** of [Stream20](#stream).

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | Yes  | Application sandbox path of the file.                                  |
| mode   | string | Yes  | - **r**: Open a file for reading. The file must exist.<br>- **r+**: Open a file for both reading and writing. The file must exist.<br>- **w**: Open a file for writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **w+**: Open a file for both reading and writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **a**: Open a file in append mode for writing at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).<br>- **a+**: Open a file in append mode for reading or updating at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).|

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [Stream](#stream20) | Stream opened.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  console.info("Stream created");
  stream.closeSync();
  ```

## fs.fdopenStream<sup>20+</sup>

fdopenStream(fd: number, mode: string): Promise&lt;Stream&gt;

Opens a stream based on an FD. This API uses a promise to return the result. To close the stream, use **close()** of [Stream20](#stream).

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description                                      |
  | ---- | ------ | ---- | ---------------------------------------- |
  | fd   | number | Yes   | FD of the file.                            |
  | mode | string | Yes   | - **r**: Open a file for reading. The file must exist.<br>- **r+**: Open a file for both reading and writing. The file must exist.<br>- **w**: Open a file for writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **w+**: Open a file for both reading and writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **a**: Open a file in append mode for writing at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).<br>- **a+**: Open a file in append mode for reading or updating at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).|

**Return value**

  | Type                              | Description       |
  | --------------------------------- | --------- |
  | Promise&lt;[Stream](#stream20)&gt; | Promise used to return the stream opened.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fdopenStream(file.fd, "r+").then((stream: fs.Stream) => {
    console.info("Stream opened");
    stream.closeSync();
  }).catch((err: BusinessError) => {
    console.error("openStream failed with error message: " + err.message + ", error code: " + err.code);
    // If the file stream fails to be opened, the FD must be manually closed.
    fs.closeSync(file);
  });
  ```

> **NOTE**
>
> When a file stream created with an FD is used, the lifecycle of the FD will be managed by the file stream object. When **close()** is called to close the file stream, the FD is also closed.

## fs.fdopenStream<sup>20+</sup>

fdopenStream(fd: number, mode: string, callback: AsyncCallback&lt;Stream&gt;): void

Opens a stream based on an FD. This API uses an asynchronous callback to return the result. To close the stream, use **close()** of [Stream](#stream20).

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                                      | Mandatory  | Description                                      |
  | -------- | ---------------------------------------- | ---- | ---------------------------------------- |
  | fd       | number                                   | Yes   | FD of the file.                            |
  | mode     | string                                   | Yes   | - **r**: Open a file for reading. The file must exist.<br>- **r+**: Open a file for both reading and writing. The file must exist.<br>- **w**: Open a file for writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **w+**: Open a file for both reading and writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **a**: Open a file in append mode for writing at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).<br>- **a+**: Open a file in append mode for reading or updating at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).|
  | callback | AsyncCallback&lt;[Stream](#stream20)&gt; | Yes   | Callback used to return the result.                           |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_ONLY);
  fs.fdopenStream(file.fd, "r+", (err: BusinessError, stream: fs.Stream) => {
    if (err) {
      console.error("fdopen stream failed with error message: " + err.message + ", error code: " + err.code);
      stream.closeSync();
    } else {
      console.info("fdopen stream succeed");
      // If the file stream fails to be opened, the FD must be manually closed.
      fs.closeSync(file);
    }
  });
  ```

> **NOTE**
>
> If a file stream is created with an FD, the lifecycle of the FD is also transferred to the file stream object. When **close()** is called to close the file stream, the FD is also closed.

## fs.fdopenStreamSync<sup>20+</sup>

fdopenStreamSync(fd: number, mode: string): Stream

Opens a stream based on an FD. This API returns the result synchronously. To close the stream, use **close()** of [Stream](#stream20).

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description                                      |
  | ---- | ------ | ---- | ---------------------------------------- |
  | fd   | number | Yes   | FD of the file.                            |
  | mode | string | Yes   | - **r**: Open a file for reading. The file must exist.<br>- **r+**: Open a file for both reading and writing. The file must exist.<br>- **w**: Open a file for writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **w+**: Open a file for both reading and writing. If the file exists, clear its content. If the file does not exist, create a file.<br>- **a**: Open a file in append mode for writing at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).<br>- **a+**: Open a file in append mode for reading or updating at the end of the file. If the file does not exist, create a file. If the file exists, write data to the end of the file (the original content of the file is reserved).|

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [Stream](#stream20) | Stream opened.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_ONLY | fs.OpenMode.CREATE);
  let stream = fs.fdopenStreamSync(file.fd, "r+");
  stream.closeSync();
  ```

> **NOTE**
>
> If a file stream is created with an FD, the lifecycle of the FD is also transferred to the file stream object. When **close()** is called to close the file stream, the FD is also closed.

## fs.createReadStream<sup>20+</sup>

createReadStream(path: string, options?: ReadStreamOptions ): ReadStream

Creates a readable stream. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description                                      |
  | ---- | ------ | ---- | ---------------------------------------- |
  | path   | string | Yes   | Path of the file.                            |
  | options | [ReadStreamOptions](#readstreamoptions20) | No   | The options are as follows:<br>- **start** (number): start position of the data to read in the file. This parameter is optional. By default, data is read from the current position.<br>- **end** (number): end position of the data to read in the file. This parameter is optional. The default value is the end of the file.|

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [ReadStream](#readstream20) | **ReadStream** instance obtained.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
 // Create a readable stream.
  const rs = fs.createReadStream(`${pathDir}/read.txt`);
  // Create a writeable stream.
  const ws = fs.createWriteStream(`${pathDir}/write.txt`);
  // Copy files in paused mode.
  rs.on('readable', () => {
    const data = rs.read();
    if (!data) {
      return;
    }
    ws.write(data);
  });
  ```

## fs.createWriteStream<sup>20+</sup>

createWriteStream(path: string, options?: WriteStreamOptions): WriteStream

Creates a writeable stream. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description                                      |
  | ---- | ------ | ---- | ---------------------------------------- |
  | path   | string | Yes   | Path of the file.                            |
  | options | [WriteStreamOptions](#writestreamoptions20) | No   | The options are as follows:<br>- **start** (number): start position to write the data in the file. This parameter is optional. By default, data is written from the current position.<br>- **mode** (number): [mode](#openmode) for creating the writeable stream. This parameter is optional. The default value is the write-only mode.|

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [WriteStream](#writestream20) | **WriteStream** instance obtained.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
 // Create a readable stream.
  const rs = fs.createReadStream(`${pathDir}/read.txt`);
  // Create a writeable stream.
  const ws = fs.createWriteStream(`${pathDir}/write.txt`);
  // Copy files in paused mode.
  rs.on('readable', () => {
    const data = rs.read();
    if (!data) {
      return;
    }
    ws.write(data);
  });
  ```

## AtomicFile<sup>20+</sup>
AtomicFile is a class used to perform atomic read and write operations on files.

A temporary file is written and renamed to the original file location, which ensures file integrity. If the write operation fails, the temporary file is deleted without modifying the original file content.

You can call **finishWrite()** or **failWrite()** to write or roll back file content.

**System capability**: SystemCapability.FileManagement.File.FileIO

### constructor<sup>20+</sup>

constructor(path: string)

Creates an **AtomicFile** class for a file in a specified path.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description                              |
  | ------ | ------ | ---- | -------------------------------------- |
  | path   | string | Yes   | Application sandbox path of the file.                      |

### getBaseFile<sup>20+</sup>

getBaseFile(): File

Obtains the file object through the **AtomicFile** object.

The FD needs to be closed by calling **close()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

| Type               | Description       |
| ------------------ | --------- |
|  [File](#file) | open file object.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md) and [Universal Error Codes](../errorcodes/errorcode-universal.md).

**Example**

<!--code_no_check-->
```ts
import { hilog } from '@kit.PerformanceAnalysisKit';
import { common } from '@kit.AbilityKit';
import { fileIo as fs} from '@kit.CoreFileKit';

// Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
let pathDir = context.filesDir;

try {
  let atomicFile = new fs.AtomicFile(`${pathDir}/write.txt`);
  let writeSream = atomicFile.startWrite();
  writeSream.write("xxxxx","utf-8",()=> {
    atomicFile.finishWrite();
    let File = atomicFile.getBaseFile();
    hilog.info(0x0000, 'AtomicFile', 'getBaseFile File.fd is:%{public}d, path:%{public}s, name:%{public}s', File.fd, File.path, File.path);
  })
} catch (err) {
  hilog.error(0x0000, 'AtomicFile', 'failed! err :%{public}s', err.message);
}
```

### openRead<sup>20+</sup>

openRead(): ReadStream

Creates a **ReadStream** instance.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [ReadStream](#readstream20) | **ReadStream** instance obtained.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md) and [Universal Error Codes](../errorcodes/errorcode-universal.md).

**Example**

<!--code_no_check-->
```ts
import { hilog } from '@kit.PerformanceAnalysisKit';
import { common } from '@kit.AbilityKit';
import { fileIo as fs} from '@kit.CoreFileKit';
import { BusinessError } from '@kit.BasicServicesKit';

// Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
let pathDir = context.filesDir;

try {
  let file = new fs.AtomicFile(`${pathDir}/read.txt`);
  let writeSream = file.startWrite();
  writeSream.write("xxxxxx","utf-8",()=> {
    file.finishWrite();
    setTimeout(()=>{
      let readStream = file.openRead();
      readStream.on('readable', () => {
        const data = readStream.read();
        if (!data) {
          hilog.error(0x0000, 'AtomicFile', 'Read data is null');
          return;
        }
        hilog.info(0x0000, 'AtomicFile', 'Read data is:%{public}s!', data);
      });
    },1000);
  })
} catch (err) {
  hilog.error(0x0000, 'AtomicFile', 'failed!, err : %{public}s', err.message);
}
```

### readFully<sup>20+</sup>

readFully(): ArrayBuffer

Reads all content of a file.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | ArrayBuffer | Full content of a file.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md) and [Universal Error Codes](../errorcodes/errorcode-universal.md).

**Example**

<!--code_no_check-->
```ts
import { hilog } from '@kit.PerformanceAnalysisKit';
import { common } from '@kit.AbilityKit';
import { fileIo as fs} from '@kit.CoreFileKit';
import { util, buffer } from '@kit.ArkTS';
import { BusinessError } from '@kit.BasicServicesKit';

// Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
let pathDir = context.filesDir;

try {
  let file = new fs.AtomicFile(`${pathDir}/read.txt`);
  let writeSream = file.startWrite();
  writeSream.write("xxxxxxxxxxx","utf-8",()=> {
    file.finishWrite();
    setTimeout(()=>{
      let data = file.readFully();
      let decoder = util.TextDecoder.create('utf-8');
      let str = decoder.decodeToString(new Uint8Array(data));
      hilog.info(0x0000, 'AtomicFile', 'readFully str is :%{public}s!', str);
    },1000);
  })
} catch (err) {
  hilog.error(0x0000, 'AtomicFile', 'failed!, err : %{public}s', err.message);
}
```

### startWrite<sup>20+</sup>

startWrite(): WriteStream

Starts to write new file data in the **WriteStream** object returned.

If the file does not exist, create a file.

Call **finishWrite()** if the write operation is successful; call **failWrite()** if the write operation fails.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [WriteStream](#writestream20) | **WriteStream** instance obtained.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md) and [Universal Error Codes](../errorcodes/errorcode-universal.md).

**Example**

<!--code_no_check-->
```ts
import { hilog } from '@kit.PerformanceAnalysisKit';
import { common } from '@kit.AbilityKit';
import { fileIo as fs} from '@kit.CoreFileKit';

// Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
let pathDir = context.filesDir;

try {
  let file = new fs.AtomicFile(`${pathDir}/write.txt`);
  let writeSream = file.startWrite();
  hilog.info(0x0000, 'AtomicFile', 'startWrite end');
  writeSream.write("xxxxxxxx","utf-8",()=> {
    hilog.info(0x0000, 'AtomicFile', 'write end');
  })
} catch (err) {
  hilog.error(0x0000, 'AtomicFile', 'failed! err :%{public}s', err.message);
}
```

### finishWrite<sup>20+</sup>

finishWrite(): void

Finishes writing file data when the write operation is complete.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md) and [Universal Error Codes](../errorcodes/errorcode-universal.md).

**Example**

<!--code_no_check-->
```ts
import { hilog } from '@kit.PerformanceAnalysisKit';
import { common } from '@kit.AbilityKit';
import { fileIo as fs} from '@kit.CoreFileKit';

// Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
let pathDir = context.filesDir;

try {
  let file = new fs.AtomicFile(`${pathDir}/write.txt`);
  let writeSream = file.startWrite();
  writeSream.write("xxxxxxxxxxx","utf-8",()=> {
    file.finishWrite();
  })
} catch (err) {
  hilog.error(0x0000, 'AtomicFile', 'failed!, err : %{public}s', err.message);
}
```

### failWrite<sup>20+</sup>

failWrite(): void

Rolls back the file after the file fails to be written.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md) and [Universal Error Codes](../errorcodes/errorcode-universal.md).

**Example**

<!--code_no_check-->
```ts
import { hilog } from '@kit.PerformanceAnalysisKit';
import { common } from '@kit.AbilityKit';
import { fileIo as fs} from '@kit.CoreFileKit';
import { util, buffer } from '@kit.ArkTS';
import { BusinessError } from '@kit.BasicServicesKit';

// Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
let pathDir = context.filesDir;

let file = new fs.AtomicFile(`${pathDir}/write.txt`);
try {
  let writeSream = file.startWrite();
  writeSream.write("xxxxxxxxxxx","utf-8",()=> {
    hilog.info(0x0000, 'AtomicFile', 'write succeed!');
  })
} catch (err) {
  file.failWrite();
  hilog.error(0x0000, 'AtomicFile', 'failed!, err : %{public}s', err.message);
}
```

### delete<sup>20+</sup>

delete(): void

Deletes the **AtomicFile** class, including the original files and temporary files.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md) and [Universal Error Codes](../errorcodes/errorcode-universal.md).

**Example**

<!--code_no_check-->
```ts
import { common } from '@kit.AbilityKit';
import { fileIo as fs} from '@kit.CoreFileKit';
import { hilog } from '@kit.PerformanceAnalysisKit';
import { util } from '@kit.ArkTS';

// Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
let pathDir = context.filesDir;

try {
  let file = new fs.AtomicFile(`${pathDir}/read.txt`);
  let writeSream = file.startWrite();
  writeSream.write("xxxxxxxxxxx","utf-8",()=> {
    file.finishWrite();
    setTimeout(()=>{
      let data = file.readFully();
      let decoder = util.TextDecoder.create('utf-8');
      let str = decoder.decodeToString(new Uint8Array(data));
      hilog.info(0x0000, 'AtomicFile', 'readFully str is :%{public}s!', str);
      file.delete();
    },1000);
  })
} catch (err) {
  hilog.error(0x0000, 'AtomicFile', 'failed!, err : %{public}s', err.message);
}
```

## fs.createWatcher<sup>20+</sup>

createWatcher(path: string, events: number, listener: WatchEventListener): Watcher

Creates a **Watcher** object to listen for file or directory changes.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description                                      |
  | ---- | ------ | ---- | ---------------------------------------- |
  | path   | string | Yes   | Application sandbox path of the file or directory to observe.                            |
  | events | number | Yes   | Events to observe. Multiple events can be separated by a bitwise OR operator (\|).<br>- **0x1: IN_ACCESS**: A file is accessed.<br>- **0x2: IN_MODIFY**: The file content is modified.<br>- **0x4: IN_ATTRIB**: The file metadata is modified.<br>- **0x8: IN_CLOSE_WRITE**: A file is opened, written with data, and then closed.<br>- **0x10: IN_CLOSE_NOWRITE**: A file or directory is opened and then closed without data written.<br>- **0x20: IN_OPEN**: A file or directory is opened.<br>- **0x40: IN_MOVED_FROM**: A file in the observed directory is moved.<br>- **0x80: IN_MOVED_TO**: A file is moved to the observed directory.<br>- **0x100: IN_CREATE**: A file or directory is created in the observed directory.<br>- **0x200: IN_DELETE**: A file or directory is deleted from the observed directory.<br>- **0x400: IN_DELETE_SELF**: The observed directory is deleted. After the directory is deleted, the listening stops.<br>- **0x800: IN_MOVE_SELF**: The observed file or directory is moved. After the file or directory is moved, the listening continues.<br>- **0xfff: IN_ALL_EVENTS**: All events.|
  | listener   | [WatchEventListener](#watcheventlistener20) | Yes   | Callback invoked when an observed event occurs. The callback will be invoked each time an observed event occurs.                            |

**Return value**

  | Type               | Description       |
  | ------------------ | --------- |
  | [Watcher](#watcher20) | **Watcher** object created.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

<!--code_no_check-->
  ```ts
  import { common } from '@kit.AbilityKit';
  import { fileIo as fs, WatchEvent } from '@kit.CoreFileKit';

  // Obtain the context from the component and ensure that the return value of this.getUIContext().getHostContext() is UIAbilityContext.
  let context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  let pathDir = context.filesDir;
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  let watcher = fs.createWatcher(filePath, 0x2 | 0x10, (watchEvent: WatchEvent) => {
    if (watchEvent.event == 0x2) {
      console.info(watchEvent.fileName + 'was modified');
    } else if (watchEvent.event == 0x10) {
      console.info(watchEvent.fileName + 'was closed');
    }
  });
  watcher.start();
  fs.writeSync(file.fd, 'test');
  fs.closeSync(file);
  watcher.stop();
  ```

## WatchEventListener<sup>20+</sup>

(event: WatchEvent): void

Provides APIs for observing events.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name | Type    | Mandatory  | Description                                      |
  | ---- | ------ | ---- | ---------------------------------------- |
  | event   | [WatchEvent](#watchevent20) | Yes   | Event for the callback to invoke.                            |

## WatchEvent<sup>20+</sup>

Defines the event to observe.

**System capability**: SystemCapability.FileManagement.File.FileIO

### Properties

| Name  | Type  | Read-Only  | Optional  | Description     |
| ---- | ------ | ---- | ---- | ------- |
| fileName | string | Yes   | No   | Sandbox path of the file to observe. The sandbox path contains the file name.|
| event | number | Yes   | No   | Events to observe. Multiple events can be separated by a bitwise OR operator (\|).<br>- **0x1: IN_ACCESS**: A file is accessed.<br>- **0x2: IN_MODIFY**: The file content is modified.<br>- **0x4: IN_ATTRIB**: The file metadata is modified.<br>- **0x8: IN_CLOSE_WRITE**: A file is opened, written with data, and then closed.<br>- **0x10: IN_CLOSE_NOWRITE**: A file or directory is opened and then closed without data written.<br>- **0x20: IN_OPEN**: A file or directory is opened.<br>- **0x40: IN_MOVED_FROM**: A file in the observed directory is moved.<br>- **0x80: IN_MOVED_TO**: A file is moved to the observed directory.<br>- **0x100: IN_CREATE**: A file or directory is created in the observed directory.<br>- **0x200: IN_DELETE**: A file or directory is deleted from the observed directory.<br>- **0x400: IN_DELETE_SELF**: The observed directory is deleted. After the directory is deleted, the listening stops.<br>- **0x800: IN_MOVE_SELF**: The observed file or directory is moved. After the file or directory is moved, the listening continues.<br>- **0xfff: IN_ALL_EVENTS**: All events.|
| cookie | number | Yes   | No   | Cookie bound with the event.<br>Currently, only the **IN_MOVED_FROM** and **IN_MOVED_TO** events are supported. The **IN_MOVED_FROM** and **IN_MOVED_TO** events of the same file have the same **cookie** value.|

## Stat

Represents detailed file information. Before calling any API of the **Stat()** class, use [stat()](#fsstat) to create a **Stat** instance synchronously or asynchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

### Attributes

| Name    | Type  | Readable  | Writable  | Description                                      |
| ------ | ------ | ---- | ---- | ---------------------------------------- |                        
| ino    | number | Yes   | No   | File ID. Different files on the same device have different **ino**s.|                 |
| mode   | number | Yes   | No   | File permissions. The meaning of each bit is as follows:<br>- **0o400**: The owner has the read permission on a regular file or a directory entry.<br>- **0o200**: The owner has the permission to write a regular file or create and delete a directory entry.<br>- **0o100**: The owner has the permission to execute a regular file or search for the specified path in a directory.<br>- **0o040**: The user group has the read permission on a regular file or a directory entry.<br>- **0o020**: The user group has the permission to write a regular file or create and delete a directory entry.<br>- **0o010**: The user group has the permission to execute a regular file or search for the specified path in a directory.<br>- **0o004**: Other users have the permission to read a regular file or read a directory entry.<br>- **0o002**: Other users have the permission to write a regular file or create and delete a directory entry.<br>- **0o001**: Other users have the permission to execute a regular file or search for the specified path in a directory.|
| uid    | number | Yes   | No   | ID of the file owner.|
| gid    | number | Yes   | No   | ID of the user group of the file.|
| size   | number | Yes   | No   | File size, in bytes. This parameter is valid only for regular files. |
| atime  | number | Yes   | No   | Time of the last access to the file. The value is the number of seconds elapsed since 00:00:00 on January 1, 1970.       |
| mtime  | number | Yes   | No   | Time of the last modification to the file. The value is the number of seconds elapsed since 00:00:00 on January 1, 1970.       |
| ctime  | number | Yes   | No   | Time of the last status change of the file. The value is the number of seconds elapsed since 00:00:00 on January 1, 1970.     |

### isBlockDevice

isBlockDevice(): boolean

Checks whether this file is a block special file. A block special file supports access by block only, and it is cached when accessed.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type     | Description              |
  | ------- | ---------------- |
  | boolean | Whether the file is a block special file.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let isBLockDevice = fs.statSync(filePath).isBlockDevice();
  ```

### isCharacterDevice

isCharacterDevice(): boolean

Checks whether this file is a character special file. A character special file supports random access, and it is not cached when accessed.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type     | Description               |
  | ------- | ----------------- |
  | boolean | Whether the file is a character special file.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let isCharacterDevice = fs.statSync(filePath).isCharacterDevice();
  ```

### isDirectory

isDirectory(): boolean

Checks whether this file is a directory.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type     | Description           |
  | ------- | ------------- |
  | boolean | Whether the file is a directory.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let dirPath = pathDir + "/test";
  let isDirectory = fs.statSync(dirPath).isDirectory(); 
  ```

### isFIFO

isFIFO(): boolean

Checks whether this file is a named pipe (or FIFO). Named pipes are used for inter-process communication.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type     | Description                   |
  | ------- | --------------------- |
  | boolean | Whether the file is a FIFO.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let isFIFO = fs.statSync(filePath).isFIFO(); 
  ```

### isFile

isFile(): boolean

Checks whether this file is a regular file.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type     | Description             |
  | ------- | --------------- |
  | boolean | Whether the file is a regular file.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let isFile = fs.statSync(filePath).isFile();
  ```

### isSocket

isSocket(): boolean

Checks whether this file is a socket.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type     | Description            |
  | ------- | -------------- |
  | boolean | Whether the file is a socket.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](c).

**Example**

  ```js
  let filePath = pathDir + "/test.txt";
  let isSocket = fs.statSync(filePath).isSocket(); 
  ```

### isSymbolicLink

isSymbolicLink(): boolean

Checks whether this file is a symbolic link.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type     | Description             |
  | ------- | --------------- |
  | boolean | Whether the file is a symbolic link.|

**Error codes**

 For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```js
  let filePath = pathDir + "/test";
  let isSymbolicLink = fs.statSync(filePath).isSymbolicLink(); 
  ```

## Stream<sup>20+</sup>

Provides API for stream operations. Before calling any API of **Stream**, you need to create a **Stream** instance by using [fs.createStream](#fscreatestream20) or [fs.fdopenStream](#fsfdopenstream20).

### close

close(): Promise&lt;void&gt;

Closes the file stream. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type                 | Description           |
  | ------------------- | ------------- |
  | Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  stream.close().then(() => {
    console.info("File stream closed");
  }).catch((err: BusinessError) => {
    console.error("close fileStream  failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

### close

close(callback: AsyncCallback&lt;void&gt;): void

Closes the file stream. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                       | Mandatory  | Description           |
  | -------- | ------------------------- | ---- | ------------- |
  | callback | AsyncCallback&lt;void&gt; | Yes   | Callback invoked immediately after the stream is closed.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  stream.close((err: BusinessError) => {
    if (err) {
      console.error("close stream failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("close stream succeed");
    }
  });
  ```

### closeSync

closeSync(): void

Closes the file stream. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  stream.closeSync();
  ```

### flush

flush(): Promise&lt;void&gt;

Flushes the file stream. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type                 | Description           |
  | ------------------- | ------------- |
  | Promise&lt;void&gt; | Promise used to return the result.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  stream.flush().then(() => {
    console.info("Stream flushed");
    stream.close();
  }).catch((err: BusinessError) => {
    console.error("flush failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

### flush

flush(callback: AsyncCallback&lt;void&gt;): void

Flushes the file stream. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                       | Mandatory  | Description            |
  | -------- | ------------------------- | ---- | -------------- |
  | callback | AsyncCallback&lt;void&gt; | Yes   | Callback used to return the result.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  stream.flush((err: BusinessError) => {
    if (err) {
      console.error("flush stream failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("Stream flushed");
      stream.close();
    }
  });
  ```

### flushSync

flushSync(): void

Flushes the file stream. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  stream.flushSync();
  stream.close();
  ```

### write

write(buffer: ArrayBuffer | string, options?: WriteOptions): Promise&lt;number&gt;

Writes data to a stream file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type                             | Mandatory  | Description                                      |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer \| string | Yes   | Data to write. It can be a string or data from a buffer.                    |
  | options | [WriteOptions](#writeoptions20)                          | No   | The options are as follows:<br>- **length** (number): length of the data to write. The default value is the buffer length.<br>- **offset** (number): start position to write the data in the file. This parameter is optional. By default, data is written from the current position.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type                   | Description      |
  | --------------------- | -------- |
  | Promise&lt;number&gt; | Promise used to return the length of the data written.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, WriteOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  let writeOption: WriteOptions = {
    offset: 5,
    length: 5,
    encoding: 'utf-8'
  };
  stream.write("hello, world", writeOption).then((number: number) => {
    console.info("write succeed and size is:" + number);
    stream.close();
  }).catch((err: BusinessError) => {
    console.error("write failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

### write

write(buffer: ArrayBuffer | string, options?: WriteOptions, callback: AsyncCallback&lt;number&gt;): void

Writes data to a stream file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name  | Type                           | Mandatory| Description                                                        |
  | -------- | ------------------------------- | ---- | ------------------------------------------------------------ |
  | buffer   | ArrayBuffer \| string | Yes  | Data to write. It can be a string or data from a buffer.                    |
  | options  | [WriteOptions](#writeoptions20)                          | No  | The options are as follows:<br>- **length** (number): length of the data to write. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to write the data in the file. This parameter is optional. By default, data is written from the current position.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported.|
  | callback | AsyncCallback&lt;number&gt;     | Yes  | Callback used to return the result.                              |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, WriteOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  let writeOption: WriteOptions = {
    offset: 5,
    length: 5,
    encoding: 'utf-8'
  };
  stream.write("hello, world", writeOption, (err: BusinessError, bytesWritten: number) => {
    if (err) {
      console.error("write stream failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      if (bytesWritten) {
        console.info("write succeed and size is:" + bytesWritten);
        stream.close();
      }
    }
  });
  ```

### writeSync

writeSync(buffer: ArrayBuffer | string, options?: WriteOptions): number

Writes data to a stream file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type                             | Mandatory  | Description                                      |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer \| string | Yes   | Data to write. It can be a string or data from a buffer.                    |
  | options | [WriteOptions](#writeoptions20)                          | No   | The options are as follows:<br>- **length** (number): length of the data to write. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to write the data in the file. This parameter is optional. By default, data is written from the current position.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type    | Description      |
  | ------ | -------- |
  | number | Length of the data written in the file.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { fileIo as fs, WriteOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath,"r+");
  let writeOption: WriteOptions = {
    offset: 5,
    length: 5,
    encoding: 'utf-8'
  };
  let num = stream.writeSync("hello, world", writeOption);
  stream.close();
  ```

### read

read(buffer: ArrayBuffer, options?: ReadOptions): Promise&lt;number&gt;

Reads data from a stream file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type         | Mandatory  | Description                                      |
  | ------- | ----------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer | Yes   | Buffer used to store the file read.                             |
  | options | [ReadOptions](#readoptions20)      | No   | The options are as follows:<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.|

**Return value**

  | Type                                | Description    |
  | ---------------------------------- | ------ |
  | Promise&lt;number&gt; | Promise used to return the data read.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { buffer } from '@kit.ArkTS';
  import { fileIo as fs, ReadOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  let arrayBuffer = new ArrayBuffer(4096);
  let readOption: ReadOptions = {
    offset: 5,
    length: 5
  };
  stream.read(arrayBuffer, readOption).then((readLen: number) => {
    console.info("Read data successfully");
    let buf = buffer.from(arrayBuffer, 0, readLen);
    console.log(`The content of file: ${buf.toString()}`);
    stream.close();
  }).catch((err: BusinessError) => {
    console.error("read data failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

### read

read(buffer: ArrayBuffer, options?: ReadOptions, callback: AsyncCallback&lt;number&gt;): void

Reads data from a stream file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                                      | Mandatory  | Description                                      |
  | -------- | ---------------------------------------- | ---- | ---------------------------------------- |
  | buffer   | ArrayBuffer                              | Yes   | Buffer used to store the file read.                             |
  | options  | [ReadOptions](#readoptions20)                                   | No   | The options are as follows:<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.|
  | callback | AsyncCallback&lt;number&gt; | Yes   | Callback used to return the result.                        |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { buffer } from '@kit.ArkTS';
  import { fileIo as fs, ReadOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  let arrayBuffer = new ArrayBuffer(4096);
  let readOption: ReadOptions = {
    offset: 5,
    length: 5
  };
  stream.read(arrayBuffer, readOption, (err: BusinessError, readLen: number) => {
    if (err) {
      console.error("read stream failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("Read data successfully");
      let buf = buffer.from(arrayBuffer, 0, readLen);
      console.log(`The content of file: ${buf.toString()}`);
      stream.close();
    }
  });
  ```

### readSync

readSync(buffer: ArrayBuffer, options?: ReadOptions): number

Reads data from a stream file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type         | Mandatory  | Description                                      |
  | ------- | ----------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer | Yes   | Buffer used to store the file read.                             |
  | options | [ReadOptions](#readoptions20)      | No   | The options are as follows:<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to read the data. This parameter is optional. By default, data is read from the current position.<br> |

**Return value**

  | Type    | Description      |
  | ------ | -------- |
  | number | Length of the data read.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { fileIo as fs, ReadOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let stream = fs.createStreamSync(filePath, "r+");
  let readOption: ReadOptions = {
    offset: 5,
    length: 5
  };
  let buf = new ArrayBuffer(4096);
  let num = stream.readSync(buf, readOption);
  stream.close();
  ```

## File

Represents a **File** object opened by **open()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

### Attributes

| Name  | Type  | Readable  | Writable  | Description     |
| ---- | ------ | ---- | ---- | ------- |
| fd | number | Yes   | No   | FD of the file.|

### getParent<sup>20+</sup>

getParent(): string

Obtains the parent directory of this file object.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Return value**

  | Type                                | Description    |
  | ---------------------------------- | ------ |
  | string | Parent directory obtained.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  console.info('The parent path is: ' + file.getParent());
  fs.closeSync(file);
  ```

## RandomAccessFile<sup>20+</sup>

Provides APIs for randomly reading and writing a stream. Before invoking any API of **RandomAccessFile**, you need to use **createRandomAccessFile()** to create a **RandomAccessFile** instance synchronously or asynchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

### Properties

| Name        | Type  | Read-Only | Optional | Description             |
| ----------- | ------ | ----  | ----- | ---------------- |
| fd          | number | Yes   | No   | FD of the file.|
| filePointer | number | Yes   | No   | Offset pointer to the **RandomAccessFile** instance.|

### setFilePointer

setFilePointer(filePointer:number): void

Sets the file offset pointer.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type     | Mandatory  | Description        |
  | ------- | ----------- | ---- | ----------------------------- |
  | filePointer  | number | Yes  | Offset pointer to the **RandomAccessFile** instance. |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let randomAccessFile = fs.createRandomAccessFileSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  randomAccessFile.setFilePointer(1);
  randomAccessFile.close();
  ```

### close<

close(): void

Closes the **RandomAccessFile** instance. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let randomAccessFile = fs.createRandomAccessFileSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  randomAccessFile.close();
  ```

### write

write(buffer: ArrayBuffer | string, options?: WriteOptions): Promise&lt;number&gt;

Writes data into a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type                             | Mandatory  | Description                                      |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer \| string | Yes   | Data to write. It can be a string or data from a buffer.                    |
  | options | [WriteOptions](#writeoptions20)                          | No   | The options are as follows:<br>- **length** (number): length of the data to write. The default value is the buffer length.<br>- **offset** (number): start position to write the data (it is determined by **filePointer** plus **offset**). This parameter is optional. By default, data is written from the **filePointer**.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type                   | Description      |
  | --------------------- | -------- |
  | Promise&lt;number&gt; | Promise used to return the length of the data written.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, WriteOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  let randomAccessFile = fs.createRandomAccessFileSync(file);
  let bufferLength: number = 4096;
  let writeOption: WriteOptions = {
    offset: 1,
    length: 5,
    encoding: 'utf-8'
  };
  let arrayBuffer = new ArrayBuffer(bufferLength);
  randomAccessFile.write(arrayBuffer, writeOption).then((bytesWritten: number) => {
    console.info("randomAccessFile bytesWritten: " + bytesWritten);
  }).catch((err: BusinessError) => {
    console.error("create randomAccessFile failed with error message: " + err.message + ", error code: " + err.code);
  }).finally(() => {
    randomAccessFile.close();
    fs.closeSync(file);
  });

  ```

### write

write(buffer: ArrayBuffer | string, options?: WriteOptions, callback: AsyncCallback&lt;number&gt;): void

Writes data to a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name  | Type                           | Mandatory| Description                                                        |
  | -------- | ------------------------------- | ---- | ------------------------------------------------------------ |
  | buffer   | ArrayBuffer \| string | Yes  | Data to write. It can be a string or data from a buffer.                    |
  | options  | [WriteOptions](#writeoptions20)                          | No  | The options are as follows:<br>- **length** (number): length of the data to write. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to write the data (it is determined by **filePointer** plus **offset**). This parameter is optional. By default, data is written from the **filePointer**.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported.|
  | callback | AsyncCallback&lt;number&gt;     | Yes  | Callback used to return the result.                              |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, WriteOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  let randomAccessFile = fs.createRandomAccessFileSync(file);
  let bufferLength: number = 4096;
  let writeOption: WriteOptions = {
    offset: 1,
    length: bufferLength,
    encoding: 'utf-8'
  };
  let arrayBuffer = new ArrayBuffer(bufferLength);
  randomAccessFile.write(arrayBuffer, writeOption, (err: BusinessError, bytesWritten: number) => {
    if (err) {
      console.error("write failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      if (bytesWritten) {
        console.info("write succeed and size is:" + bytesWritten);
      }
    }
    randomAccessFile.close();
    fs.closeSync(file);
  });
  ```

### writeSync

writeSync(buffer: ArrayBuffer | string, options?: WriteOptions): number

Writes data to a file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type                             | Mandatory  | Description                                      |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer \| string | Yes   | Data to write. It can be a string or data from a buffer.                    |
  | options | [WriteOptions](#writeoptions20)                          | No   | The options are as follows:<br>- **length** (number): length of the data to write. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to write the data (it is determined by **filePointer** plus **offset**). This parameter is optional. By default, data is written from the **filePointer**.<br>- **encoding** (string): format of the data to be encoded when the data is a string. The default value is **'utf-8'**, which is the only value supported.|

**Return value**

  | Type    | Description      |
  | ------ | -------- |
  | number | Length of the data written in the file.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { fileIo as fs, WriteOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let randomAccessFile = fs.createRandomAccessFileSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  let writeOption: WriteOptions = {
    offset: 5,
    length: 5,
    encoding: 'utf-8'
  };
  let bytesWritten = randomAccessFile.writeSync("hello, world", writeOption);
  randomAccessFile.close();
  ```

### read

read(buffer: ArrayBuffer, options?: ReadOptions): Promise&lt;number&gt;

Reads data from a file. This API uses a promise to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type         | Mandatory  | Description                                      |
  | ------- | ----------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer | Yes   | Buffer used to store the file read.                             |
  | options | [ReadOptions](#readoptions20)      | No   | The options are as follows:<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to read the data (it is determined by **filePointer** plus **offset**). This parameter is optional. By default, data is read from the **filePointer**.|

**Return value**

  | Type                                | Description    |
  | ---------------------------------- | ------ |
  | Promise&lt;number&gt; | Promise used to return the data read.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ReadOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  let randomAccessFile = fs.createRandomAccessFileSync(file);
  let bufferLength: number = 4096;
  let readOption: ReadOptions = {
    offset: 1,
    length: 5
  };
  let arrayBuffer = new ArrayBuffer(bufferLength);
  randomAccessFile.read(arrayBuffer, readOption).then((readLength: number) => {
    console.info("randomAccessFile readLength: " + readLength);
  }).catch((err: BusinessError) => {
    console.error("create randomAccessFile failed with error message: " + err.message + ", error code: " + err.code);
  }).finally(() => {
    randomAccessFile.close();
    fs.closeSync(file);
  });
  ```

### read

read(buffer: ArrayBuffer, options?: ReadOptions, callback: AsyncCallback&lt;number&gt;): void

Reads data from a file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name     | Type                                      | Mandatory  | Description                                      |
  | -------- | ---------------------------------------- | ---- | ---------------------------------------- |
  | buffer   | ArrayBuffer                              | Yes   | Buffer used to store the file read.                             |
  | options  | [ReadOptions](#readoptions20)                                   | No   | The options are as follows:<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to read the data (it is determined by **filePointer** plus **offset**). This parameter is optional. By default, data is read from the **filePointer**.|
  | callback | AsyncCallback&lt;number&gt; | Yes   | Callback used to return the result.                        |

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ReadOptions } from '@kit.CoreFileKit';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  let randomAccessFile = fs.createRandomAccessFileSync(file);
  let length: number = 20;
  let readOption: ReadOptions = {
    offset: 1,
    length: 5
  };
  let arrayBuffer = new ArrayBuffer(length);
  randomAccessFile.read(arrayBuffer, readOption, (err: BusinessError, readLength: number) => {
    if (err) {
      console.error("read failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      if (readLength) {
        console.info("read succeed and size is:" + readLength);
      }
    }
    randomAccessFile.close();
    fs.closeSync(file);
  });
  ```

### readSync

readSync(buffer: ArrayBuffer, options?: ReadOptions): number

Reads data from a file. This API returns the result synchronously.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name    | Type         | Mandatory  | Description                                      |
  | ------- | ----------- | ---- | ---------------------------------------- |
  | buffer  | ArrayBuffer | Yes   | Buffer used to store the file read.                             |
  | options | [ReadOptions](#readoptions20)      | No   | The options are as follows:<br>- **length** (number): length of the data to read. This parameter is optional. The default value is the buffer length.<br>- **offset** (number): start position to read the data (it is determined by **filePointer** plus **offset**). This parameter is optional. By default, data is read from the **filePointer**.<br> |

**Return value**

  | Type    | Description      |
  | ------ | -------- |
  | number | Length of the data read.|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.CREATE | fs.OpenMode.READ_WRITE);
  let randomAccessFile = fs.createRandomAccessFileSync(file);
  let length: number = 4096;
  let arrayBuffer = new ArrayBuffer(length);
  let readLength = randomAccessFile.readSync(arrayBuffer);
  randomAccessFile.close();
  fs.closeSync(file);
  ```

## Watcher<sup>20+</sup>

Provides APIs for observing the changes of files or directories. Before using the APIs of **Watcher**, call **createWatcher()** to create a **Watcher** object.

### start<sup>20+</sup>

start(): void

Starts listening.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let watcher = fs.createWatcher(filePath, 0xfff, () => {});
  watcher.start();
  watcher.stop();
  ```

### stop<sup>20+</sup>

stop(): void

Stops listening and removes the **Watcher** object.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  let filePath = pathDir + "/test.txt";
  let watcher = fs.createWatcher(filePath, 0xfff, () => {});
  watcher.start();
  watcher.stop();
  ```

## OpenMode

Defines the constants of the **mode** parameter used in **open()**. It specifies the mode for opening a file.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name  | Type  | Value | Description     |
| ---- | ------ |---- | ------- |
| READ_ONLY | number |  0o0   | Open the file in read-only mode.|
| WRITE_ONLY | number | 0o1    | Open the file in write-only mode.|
| READ_WRITE | number | 0o2    | Open the file in read/write mode.|
| CREATE | number | 0o100    | Create a file if the specified file does not exist.|
| TRUNC | number | 0o1000    | If the file exists and is open in write-only or read/write mode, truncate the file length to 0.|
| APPEND | number | 0o2000   | Open the file in append mode. New data will be written to the end of the file.|
| NONBLOCK | number | 0o4000    | If **path** points to a named pipe (FIFO), block special file, or character special file, perform non-blocking operations on the open file and in subsequent I/Os.|
| DIR | number | 0o200000    | If **path** does not point to a directory, throw an exception.|
| NOFOLLOW | number | 0o400000    | If **path** points to a symbolic link, throw an exception.|
| SYNC | number | 0o4010000    | Open the file in synchronous I/O mode.|

## Filter

**System capability**: SystemCapability.FileManagement.File.FileIO

Defines the file filtering configuration, which can be used by **listFile()**.

| Name       | Type      | Description               |
| ----------- | --------------- | ------------------ |
| suffix | Array&lt;string&gt;     | Locate files that fully match the specified file name extensions, which are of the OR relationship.          |
| displayName    | Array&lt;string&gt;     | Locate files that fuzzy match the specified file names, which are of the OR relationship.|
| mimeType    | Array&lt;string&gt; | Locate files that fully match the specified MIME types, which are of the OR relationship.      |
| fileSizeOver    | number | Locate files that are greater than or equal to the specified size.      |
| lastModifiedAfter    | number | Locate files whose last modification time is the same or later than the specified time.      |
| excludeMedia    | boolean | Whether to exclude the files already in **Media**.      |

## ConflictFiles<sup>20+</sup>

Defines conflicting file information used in **copyDir()** or **moveDir()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Description               |
| ----------- | --------------- | ------------------ |
| srcFile | string     | Path of the source file.          |
| destFile    | string     | Path of the destination file.|

## Options<sup>20+</sup>

Defines the options used in **readLines()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Description               |
| ----------- | --------------- | ------------------ |
| encoding | string     | File encoding format. It is optional.          |

## WhenceType<sup>20+</sup>

Enumerates the types of the relative offset position used in **lseek()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Value      | Description               |
| ----------- | --------------- | ------------------ |
| SEEK_SET | 0     | Beginning of the file.          |
| SEEK_CUR    | 1     | Current offset position.|
| SEEK_END    | 2     | End of the file.|

## AccessModeType<sup>20+</sup>

Enumerates the access modes to verify. If this parameter is left blank, the system checks whether the file exists.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Value      | Description               |
| ----------- | --------------- | ------------------ |
| EXIST | 0     | Whether the file exists.          |
| WRITE    | 2     | Verify the write permission on the file.|
| READ    | 4     | Verify the read permission on the file.|
| READ_WRITE    | 6     | Verify the read/write permission on the file.|

## ReadOptions<sup>20+</sup>

Defines the options used in **read()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Mandatory      | Description               |
| ----------- | --------------- | ------------------ |------------------ |
| length | number     | No| Length of the data to read, in bytes. This parameter is optional. The default value is the buffer length.          |
|  offset    | number     | No| Start position of the file to read (current **filePointer** plus **offset**), in bytes. This parameter is optional. By default, data is read from the **filePointer**.|

## ReadTextOptions<sup>20+</sup>

Defines the options used in **readText()**. It inherits from [ReadOptions](#readoptions20).

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Mandatory      | Description               |
| ----------- | --------------- | ------------------ | ------------------ |
| length | number     | No| Length of the data to read, in bytes. This parameter is optional. The default value is the file length.          |
|  offset    | number     | No| Start position of the file to read, in bytes. This parameter is optional. By default, data is read from the current position.|
| encoding    | string | No| Format of the data to be encoded. This parameter is valid only when the data type is string. The default value is **'utf-8'**, which is the only value supported.<br> |

## WriteOptions<sup>20+</sup>

Defines the options used in **write()**. It inherits from [Options](#options20).

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Mandatory      | Description               |
| ----------- | --------------- | ------------------ | ------------------ |
| length | number     | No| Length of the data to write, in bytes. This parameter is optional. The default value is the buffer length. |
|  offset    | number     | No| Start position of the file to write (current **filePointer** plus **offset**), in bytes. This parameter is optional. By default, data is written from the **filePointer**.|
| encoding    | string | No| Format of the data to be encoded. This parameter is valid only when the data type is string. The default value is **'utf-8'**, which is the only value supported.      |

## ReadStream<sup>20+</sup>

Defines a readable stream. You need to use [fs.createReadStream](#fscreatereadstream20) to create a **ReadStream** instance, which is inherited from the [stream base class](../apis-arkts/js-apis-stream.md#readable).

The data obtained by **ReadStream** is a decoded string. Currently, only the UTF-8 format is supported.

### Properties

| Name    | Type  | Read-Only  | Optional  | Description                                      |
| ------ | ------ | ---- | ---- | ---------------------------------------- |
| bytesRead    | number | Yes   | No   | Number of bytes read by the readable stream.|
| path    | string | Yes   | No   | Path of the file corresponding to the readable stream.|

### Seek

seek(offset: number, whence?: WhenceType): number


Adjusts the position of the readable stream offset pointer.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | offset | number | Yes   | Relative offset, in bytes.|
  | whence | [WhenceType](#whencetype20) | No   | Where to start the offset. The default value is **SEEK_SET**, which indicates the beginning of the file.|

**Return value**

  | Type                  | Description        |
  | --------------------- | ---------- |
  | number | Position of the current offset pointer (offset relative to the file header, in bytes).|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  const filePath = pathDir + "/test.txt";
  const rs = fs.createReadStream(filePath);
  const curOff = rs.seek(5, fs.WhenceType.SEEK_SET);
  console.info(`current offset is ${curOff}`);
  rs.close();
  ```

### close

close(): void

Closes this readable stream.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  const filePath = pathDir + "/test.txt";
  const rs = fs.createReadStream(filePath);
  rs.close();
  ```

## WriteStream<sup>20+</sup>

Defines a writeable stream. You need to use [fs.createWriteStream](#fscreatewritestream20) to create a **WriteStream** instance, which is inherited from the [stream base class](../apis-arkts/js-apis-stream.md#writable).

### Properties

| Name    | Type  | Read-Only  | Optional  | Description                                      |
| ------ | ------ | ---- | ---- | ---------------------------------------- |
| bytesWritten    | number | Yes   | No   | Number of bytes written to the writable stream.|
| path    | string | Yes   | No   | Path of the file corresponding to the writeable stream.|

### Seek

seek(offset: number, whence?: WhenceType): number;

Adjusts the position of the writeable stream offset pointer.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Parameters**

  | Name   | Type    | Mandatory  | Description                         |
  | ------ | ------ | ---- | --------------------------- |
  | offset | number | Yes   | Relative offset, in bytes.|
  | whence | [WhenceType](#whencetype20) | No   | Where to start the offset. The default value is **SEEK_SET**, which indicates the beginning of the file.|

**Return value**

  | Type                  | Description        |
  | --------------------- | ---------- |
  | number | Position of the current offset pointer (offset relative to the file header, in bytes).|

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  const filePath = pathDir + "/test.txt";
  const ws = fs.createWriteStream(filePath);
  const curOff = ws.seek(5, fs.WhenceType.SEEK_SET);
  console.info(`current offset is ${curOff}`);
  ws.close();
  ```

### close

close(): void

Closes this writeable stream.

**System capability**: SystemCapability.FileManagement.File.FileIO

**Error codes**

For details about the error codes, see [Basic File IO Error Codes](../errorcodes/errorcode-filemanagement.md).

**Example**

  ```ts
  const filePath = pathDir + "/test.txt";
  const ws = fs.createWriteStream(filePath);
  ws.close();
  ```

## RandomAccessFileOptions<sup>20+</sup>

Defines the options used in **createRandomAccessFile()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Mandatory      |  Description               |
| ----------- | --------------- | ------------------ | ------------------ |
| start   | number     | No| Start position to read the data, in bytes. This parameter is optional. By default, data is read from the current position.          |
| end     | number     | No|  End position to read the data, in bytes. This parameter is optional. The default value is the end of the file.|

## ReadStreamOptions<sup>20+</sup>

Defines the options used in **createReadStream()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Mandatory      |  Description               |
| ----------- | --------------- | ------------------ | ------------------ |
| start   | number     | No| Start position to read the data, in bytes. This parameter is optional. By default, data is read from the current position.          |
| end     | number     | No|  End position to read the data, in bytes. This parameter is optional. The default value is the end of the file.|

## WriteStreamOptions<sup>20+</sup>

Defines the options used in **createWriteStream()**.

**System capability**: SystemCapability.FileManagement.File.FileIO

| Name       | Type      | Mandatory      |  Description               |
| ----------- | --------------- | ------------------ | ------------------ |
| start   | number     | No| Start position to write the data, in bytes. This parameter is optional. By default, data is written from the beginning of the file.          |
| mode     | number     | No| [Option](#openmode) for creating the writeable stream. You must specify one of the following options.<br>- **OpenMode.READ_ONLY(0o0)**: read-only, which is the default value.<br>- **OpenMode.WRITE_ONLY(0o1)**: write-only.<br>- **OpenMode.READ_WRITE(0o2)**: read/write.<br>You can also specify the following options, separated by a bitwise OR operator (&#124;). By default, no additional options are given.<br>- **OpenMode.CREATE(0o100)**: If the file does not exist, create it.<br>- **OpenMode.TRUNC(0o1000)**: If the file exists and is opened in write mode, truncate the file length to 0.<br>- **OpenMode.APPEND(0o2000)**: Open the file in append mode. New data will be added to the end of the file.<br>- **OpenMode.NONBLOCK(0o4000)**: If **path** points to a named pipe (also known as a FIFO), block special file, or character special file, perform non-blocking operations on the opened file and in subsequent I/Os.<br>- **OpenMode.DIR(0o200000)**: If **path** does not point to a directory, throw an exception. The write permission is not allowed.<br>- **OpenMode.NOFOLLOW(0o400000)**: If **path** points to a symbolic link, throw an exception.<br>- **OpenMode.SYNC(0o4010000)**: Open the file in synchronous I/O mode.|