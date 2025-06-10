# @ohos.file.fs (文件管理)

该模块为基础文件操作API，提供基础文件操作能力，包括文件基本管理、文件目录管理、文件信息统计、文件流式读写等常用功能。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import fs from '@ohos.file.fs';
```

## 使用说明

使用该功能模块对文件/目录进行操作前，需要先获取其应用沙箱路径，获取方式及其接口用法请参考：

**Stage模型**

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

stat(file: string | number): Promise&lt;Stat&gt;

获取文件详细属性信息，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                       |
| ------ | ------ | ---- | -------------------------- |
| file   | string\|number | 是   | 文件应用沙箱路径path或已打开的文件描述符fd。 |

**返回值：**

  | 类型                           | 说明         |
  | ---------------------------- | ---------- |
  | Promise&lt;[Stat](#stat)&gt; | Promise对象。返回文件的具体信息。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.stat(filePath).then((stat) => {
      console.info("get file info succeed, the size of file is " + stat.size);
  }).catch((err) => {
      console.info("get file info failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.stat

stat(file: string | number, callback: AsyncCallback&lt;Stat&gt;): void

获取文件详细属性信息，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                               | 必填 | 说明                           |
| -------- | ---------------------------------- | ---- | ------------------------------ |
| file     | string\|number                            | 是   | 文件应用沙箱路径path或已打开的文件描述符fd。     |
| callback | AsyncCallback&lt;[Stat](#stat)&gt; | 是   | 异步获取文件的信息之后的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

statSync(file: string | number): Stat

以同步方法获取文件详细属性信息。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                       |
| ------ | ------ | ---- | -------------------------- |
| file   | string\|number | 是   | 文件应用沙箱路径path或已打开的文件描述符fd。 |

**返回值：**

  | 类型            | 说明         |
  | ------------- | ---------- |
  | [Stat](#stat) | 表示文件的具体信息。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let stat = fs.statSync(pathDir);
  console.info("get file info succeed, the size of file is " + stat.size);
  ```

## fs.access

access(path: string, mode?: AccessModeType): Promise&lt;boolean&gt;

检查文件或目录是否存在，或校验操作权限，使用promise异步回调。<br>校验读、写或读写权限不通过会抛出13900012（Permission denied）错误码。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 文件应用沙箱路径。                                   |
| mode<sup>20+</sup>   | [AccessModeType](#accessmodetype20) | 否   | 文件校验的权限。不填该参数则默认校验文件是否存在。|

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;boolean&gt; | Promise对象。返回布尔值。返回true，表示文件存在；返回false，表示文件不存在。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

| 错误码ID                     | 错误信息        |
| ---------------------------- | ---------- |
| 401 | 1. 未指定必须的参数；2. 参数类型与接口定义不匹配。 |
| 13900020 | 非法参数值。 |

**示例：**

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

检查文件是否存在，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                         |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                    | 是   | 文件应用沙箱路径。                                   |
| callback | AsyncCallback&lt;boolean&gt; | 是   | 异步检查文件是否存在的回调，如果存在，回调返回true，否则返回false。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.access(filePath, (err, res) => {
    if (err) {
      console.info("access failed with error message: " + err.message + ", error code: " + err.code);
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

以同步方法检查文件或目录是否存在，或校验操作权限。<br>校验读、写或读写权限不通过会抛出13900012（Permission denied）错误码。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 文件应用沙箱路径。                                   |
| mode<sup>20+</sup>   | [AccessModeType](#accessmodetype20) | 否   | 文件校验的权限。不填该参数则默认校验文件是否存在。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | boolean | 返回true，表示文件存在；返回false，表示文件不存在。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

| 错误码ID                     | 错误信息        |
| ---------------------------- | ---------- |
| 401 | 1. 未指定必须的参数；2. 参数类型与接口定义不匹配。 |
| 13900020 | 非法参数值。 |

**示例：**

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

close(file: number | File): Promise&lt;void&gt;

关闭文件，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型     | 必填   | 说明           |
  | ---- | ------ | ---- | ------------ |
  | file   | number\|[File](#file) | 是    | 已打开的File对象或已打开的文件描述符fd，关闭后file对象或文件描述符不再具备实际意义，不可再用于进行读写等操作。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.close(file).then(() => {
      console.info("close file succeed");
  }).catch((err) => {
      console.info("close file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.close

close(file: number | File, callback: AsyncCallback&lt;void&gt;): void

关闭文件，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                        | 必填   | 说明           |
  | -------- | ------------------------- | ---- | ------------ |
  | file       | number\|[File](#file)                  | 是    | 已打开的File对象或已打开的文件描述符fd，关闭后file对象或文件描述符不再具备实际意义，不可再用于进行读写等操作。 |
  | callback | AsyncCallback&lt;void&gt; | 是    | 异步关闭文件之后的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.close(file, (err) => {
    if (err) {
      console.info("close file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("close file succeed");
    }
  });
  ```

## fs.closeSync

closeSync(file: number | File): void

以同步方法关闭文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型     | 必填   | 说明           |
  | ---- | ------ | ---- | ------------ |
  | file   | number\|[File](#file) | 是    | 已打开的File对象或已打开的文件描述符fd，关闭后file对象或文件描述符不再具备实际意义，不可再用于进行读写等操作。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.closeSync(file);
  ```

## fs.copyFile

copyFile(src: string | number, dest: string | number, mode?: number): Promise&lt;void&gt;

复制文件，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型                         | 必填   | 说明                                       |
  | ---- | -------------------------- | ---- | ---------------------------------------- |
  | src  | string\|number | 是    | 待复制文件的路径或待复制文件的文件描述符。                      |
  | dest | string\|number | 是    | 目标文件路径或目标文件的文件描述符。                          |
  | mode | number                     | 否    | mode提供覆盖文件的选项，当前仅支持0，且默认为0。<br/>0：完全覆盖目标文件，未覆盖部分将被裁切掉。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

copyFile(src: string | number, dest: string | number, mode: number, callback: AsyncCallback&lt;void&gt;): void

复制文件，可设置覆盖文件的方式，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                         | 必填   | 说明                                       |
  | -------- | -------------------------- | ---- | ---------------------------------------- |
  | src      | string\|number | 是    | 待复制文件的路径或待复制文件的文件描述符。                      |
  | dest     | string\|number | 是    | 目标文件路径或目标文件的文件描述符。                          |
  | mode     | number                     | 是    | mode提供覆盖文件的选项，当前仅支持0，且默认为0。<br/>0：完全覆盖目标文件，未覆盖部分将被裁切掉。 |
  | callback | AsyncCallback&lt;void&gt;  | 是    | 异步复制文件之后的回调。                             |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let srcPath = pathDir + "/srcDir/test.txt";
  let dstPath = pathDir + "/dstDir/test.txt";
  fs.copyFile(srcPath, dstPath, 0, (err) => {
    if (err) {
      console.info("copy file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("copy file succeed");
    }
  });
  ```

## fs.copyFile

copyFile(src: string | number, dest: string | number, callback: AsyncCallback&lt;void&gt;): void

复制文件，覆盖方式为完全覆盖目标文件，未覆盖部分将被裁切。使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                         | 必填   | 说明                                       |
  | -------- | -------------------------- | ---- | ---------------------------------------- |
  | src      | string\|number | 是    | 待复制文件的路径或待复制文件的文件描述符。                      |
  | dest     | string\|number | 是    | 目标文件路径或目标文件的文件描述符。                          |
  | callback | AsyncCallback&lt;void&gt;  | 是    | 异步复制文件之后的回调。                             |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let srcPath = pathDir + "/srcDir/test.txt";
  let dstPath = pathDir + "/dstDir/test.txt";
  fs.copyFile(srcPath, dstPath, (err) => {
    if (err) {
      console.info("copy file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("copy file succeed");
    }
  });
  ```

## fs.copyFileSync

copyFileSync(src: string | number, dest: string | number, mode?: number): void

以同步方法复制文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型                         | 必填   | 说明                                       |
  | ---- | -------------------------- | ---- | ---------------------------------------- |
  | src  | string\|number | 是    | 待复制文件的路径或待复制文件的文件描述符。                      |
  | dest | string\|number | 是    | 目标文件路径或目标文件的文件描述符。                          |
  | mode | number                     | 否    | mode提供覆盖文件的选项，当前仅支持0，且默认为0。<br/>0：完全覆盖目标文件，未覆盖部分将被裁切掉。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let srcPath = pathDir + "/srcDir/test.txt";
  let dstPath = pathDir + "/dstDir/test.txt";
  fs.copyFileSync(srcPath, dstPath);
  ```

## fs.copyDir<sup>20+</sup>

copyDir(src: string, dest: string, mode?: number): Promise\<void>

复制源目录至目标路径下，使用promise异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | mode | number | 否    | 复制模式，默认值为0。<br/>-&nbsp; mode为0，文件级别抛异常。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则抛出异常。源目录下未冲突的文件全部移动至目标目录下，目标目录下未冲突文件将继续保留，且冲突文件信息将在抛出异常的data属性中以Array\<[ConflictFiles](#conflictfiles20)>形式提供。<br/>-&nbsp; mode为1，文件级别强制覆盖。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则强制覆盖冲突目录下所有同名文件，未冲突文件将继续保留。|

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  // copy directory from srcPath to destPath
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

复制源目录至目标路径下，可设置复制模式。使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | mode | number | 是    | 复制模式，默认值为0。<br/>-&nbsp; mode为0，文件级别抛异常。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则抛出异常。源目录下未冲突的文件全部移动至目标目录下，目标目录下未冲突文件将继续保留，且冲突文件信息将在抛出异常的data属性中以Array\<[ConflictFiles](#conflictfiles20)>形式提供。<br/>-&nbsp; mode为1，文件级别强制覆盖。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则强制覆盖冲突目录下所有同名文件，未冲突文件将继续保留。|
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | 是    | 异步复制目录之后的回调。              |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ConflictFiles } from '@kit.CoreFileKit';
  // copy directory from srcPath to destPath
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

复制源目录至目标路径下，使用callback异步回调。

如果目标目录下有与源目录名冲突的目录，且冲突目录下有同名文件，则抛出异常。源目录下未冲突的文件全部移动至目标目录下，目标目录下未冲突文件将继续保留，且冲突文件信息将在抛出异常的data属性中以Array\<[ConflictFiles](#conflictfiles20)>形式提供。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | 是    | 异步复制目录之后的回调。              |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { fileIo as fs, ConflictFiles } from '@kit.CoreFileKit';
  // copy directory from srcPath to destPath
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

以同步方法复制源目录至目标路径下。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | mode | number | 否    | 复制模式，默认值为0。<br/>-&nbsp; mode为0，文件级别抛异常。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则抛出异常。源目录下未冲突的文件全部移动至目标目录下，目标目录下未冲突文件将继续保留，且冲突文件信息将在抛出异常的data属性中以Array\<[ConflictFiles](#conflictfiles20)>形式提供。<br/>-&nbsp; mode为1，文件级别强制覆盖。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则强制覆盖冲突目录下所有同名文件，未冲突文件将继续保留。|

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  // copy directory from srcPath to destPath
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

设置文件或目录的扩展属性。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |
| key   | string | 是   | 扩展属性的key。仅支持前缀为“user.”的字符串，且长度需小于256字节。  |
| value   | string | 是   | 扩展属性的value。                                   |

**返回值：**

  | 类型     | 说明                                       |
  | ------ | ---------------------------------------- |
  | Promise&lt;void&gt;| Promise对象。无返回值。                             |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

设置文件或目录的扩展属性。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |
| key   | string | 是   | 扩展属性的key。仅支持前缀为“user.”的字符串，且长度需小于256字节。   |
| value   | string | 是   | 扩展属性的value。                                   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

获取文件或目录的扩展属性。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |
| key   | string | 是   | 扩展属性的key。                                   |

**返回值：**

  | 类型     | 说明                                       |
  | ------ | ---------------------------------------- |
  | Promise&lt;string&gt;| Promise对象。返回扩展属性的value。    |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

使用同步接口获取文件或目录的扩展属性。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |
| key   | string | 是   | 扩展属性的key。                                   |

**返回值：**

  | 类型     | 说明                                       |
  | ------ | ---------------------------------------- |
  | key| string对象。返回扩展属性的value。      |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

创建目录，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.mkdir(dirPath).then(() => {
      console.info("mkdir succeed");
  }).catch((err) => {
      console.info("mkdir failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.mkdir<sup>20+</sup>

mkdir(path: string, recursion: boolean): Promise\<void>

创建目录，使用promise异步回调。当recursion指定为true时，可递归创建目录。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |
| recursion   | boolean | 是   | 是否递归创建目录。recursion指定为true时，可递归创建目录。recursion指定为false时，仅可创建单层目录。   |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let dirPath = pathDir + "/testDir1/testDir2/testDir3";
  fs.mkdir(dirPath, true).then(() => {
    console.info("mkdir succeed");
  }).catch((err: BusinessError) => {
    console.error("mkdir failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.mkdir

mkdir(path: string, callback: AsyncCallback&lt;void&gt;): void

创建目录，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                         |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                    | 是   | 目录的应用沙箱路径。                                   |
| callback | AsyncCallback&lt;void&gt; | 是   | 异步创建目录操作完成之后的回调。                             |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.mkdir(dirPath, (err) => {
    if (err) {
      console.info("mkdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("mkdir succeed");
    }
  });
  ```

## fs.mkdir<sup>20+</sup>

mkdir(path: string, recursion: boolean, callback: AsyncCallback&lt;void&gt;): void

创建目录，使用callback异步回调。当recursion指定为true，可递归创建目录。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                         |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                    | 是   | 目录的应用沙箱路径。                                   |
| recursion   | boolean | 是   | 是否递归创建目录。recursion指定为true时，可递归创建目录。recursion指定为false时，仅可创建单层目录。   |
| callback | AsyncCallback&lt;void&gt; | 是   | 异步创建目录操作完成之后的回调。                             |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let dirPath = pathDir + "/testDir1/testDir2/testDir3";
  fs.mkdir(dirPath, true, (err: BusinessError) => {
    if (err) {
      console.error("mkdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("mkdir succeed");
    }
  });
  ```

## fs.mkdirSync

mkdirSync(path: string): void

以同步方法创建目录。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.mkdirSync(dirPath);
  ```

## fs.mkdirSync<sup>20+</sup>

mkdirSync(path: string, recursion: boolean): void

以同步方法创建目录。当recursion指定为true，可递归创建目录。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 目录的应用沙箱路径。                                   |
| recursion   | boolean | 是   | 是否递归创建目录。recursion指定为true时，可递归创建目录。recursion指定为false时，仅可创建单层目录。   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  let dirPath = pathDir + "/testDir1/testDir2/testDir3";
  fs.mkdirSync(dirPath, true);
  ```

## fs.open

open(path: string, mode?: number): Promise&lt;File&gt;

打开文件，使用Promise异步返回。支持使用URI打开文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 文件的应用沙箱路径或文件URI。                                   |
| mode  | number | 否   | 打开文件的[选项](#openmode)，必须指定如下选项中的一个，默认以只读方式打开：<br/>-&nbsp;OpenMode.READ_ONLY(0o0)：只读打开。<br/>-&nbsp;OpenMode.WRITE_ONLY(0o1)：只写打开。<br/>-&nbsp;OpenMode.READ_WRITE(0o2)：读写打开。<br/>给定如下功能选项，以按位或的方式追加，默认不给定任何额外选项：<br/>-&nbsp;OpenMode.CREATE(0o100)：若文件不存在，则创建文件。<br/>-&nbsp;OpenMode.TRUNC(0o1000)：如果文件存在且以只写或读写的方式打开文件，则将其长度裁剪为零。<br/>-&nbsp;OpenMode.APPEND(0o2000)：以追加方式打开，后续写将追加到文件末尾。<br/>-&nbsp;OpenMode.NONBLOCK(0o4000)：如果path指向FIFO、块特殊文件或字符特殊文件，则本次打开及后续&nbsp;IO&nbsp;进行非阻塞操作。<br/>-&nbsp;OpenMode.DIR(0o200000)：如果path不指向目录，则出错。<br/>-&nbsp;OpenMode.NOFOLLOW(0o400000)：如果path指向符号链接，则出错。<br/>-&nbsp;OpenMode.SYNC(0o4010000)：以同步IO的方式打开文件。 |

**返回值：**

  | 类型                    | 说明          |
  | --------------------- | ----------- |
  | Promise&lt;[File](#file)&gt; | Promise对象。返回File对象。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.open(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE).then((file) => {
      console.info("file fd: " + file.fd);
  }).catch((err) => {
      console.info("open file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```


## fs.open

open(path: string, mode: number, callback: AsyncCallback&lt;File&gt;): void

打开文件，可设置打开文件的选项。使用callback异步回调。

支持使用URI打开文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                            | 必填 | 说明                                                         |
| -------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                          | 是   | 文件的应用沙箱路径或URI。                                   |
| mode  | number | 否   | 打开文件的[选项](#openmode)，必须指定如下选项中的一个，默认以只读方式打开：<br/>-&nbsp;OpenMode.READ_ONLY(0o0)：只读打开。<br/>-&nbsp;OpenMode.WRITE_ONLY(0o1)：只写打开。<br/>-&nbsp;OpenMode.READ_WRITE(0o2)：读写打开。<br/>给定如下功能选项，以按位或的方式追加，默认不给定任何额外选项：<br/>-&nbsp;OpenMode.CREATE(0o100)：若文件不存在，则创建文件。<br/>-&nbsp;OpenMode.TRUNC(0o1000)：如果文件存在且以只写或读写的方式打开文件，则将其长度裁剪为零。<br/>-&nbsp;OpenMode.APPEND(0o2000)：以追加方式打开，后续写将追加到文件末尾。<br/>-&nbsp;OpenMode.NONBLOCK(0o4000)：如果path指向FIFO、块特殊文件或字符特殊文件，则本次打开及后续&nbsp;IO&nbsp;进行非阻塞操作。<br/>-&nbsp;OpenMode.DIR(0o200000)：如果path不指向目录，则出错。<br/>-&nbsp;OpenMode.NOFOLLOW(0o400000)：如果path指向符号链接，则出错。<br/>-&nbsp;OpenMode.SYNC(0o4010000)：以同步IO的方式打开文件。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

## fs.open

open(path: string, callback: AsyncCallback&lt;File&gt;): void

打开文件，使用callback异步回调。支持使用URI打开文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                            | 必填 | 说明                                                         |
| -------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| path     | string                          | 是   | 文件的应用沙箱路径或URI。                                   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.open(filePath, (err, file) => {
    if (err) {
      console.info("mkdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("file fd: " + file.fd);
    }
  });
  ```

## fs.openSync

openSync(path: string, mode?: number): File

以同步方法打开文件。支持使用URI打开文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| path   | string | 是   | 打开文件的应用沙箱路径或URI。                                   |
| mode  | number | 否   | 打开文件的[选项](#openmode)，必须指定如下选项中的一个，默认以只读方式打开：<br/>-&nbsp;OpenMode.READ_ONLY(0o0)：只读打开。<br/>-&nbsp;OpenMode.WRITE_ONLY(0o1)：只写打开。<br/>-&nbsp;OpenMode.READ_WRITE(0o2)：读写打开。<br/>给定如下功能选项，以按位或的方式追加，默认不给定任何额外选项：<br/>-&nbsp;OpenMode.CREATE(0o100)：若文件不存在，则创建文件。<br/>-&nbsp;OpenMode.TRUNC(0o1000)：如果文件存在且以只写或读写的方式打开文件，则将其长度裁剪为零。<br/>-&nbsp;OpenMode.APPEND(0o2000)：以追加方式打开，后续写将追加到文件末尾。<br/>-&nbsp;OpenMode.NONBLOCK(0o4000)：如果path指向FIFO、块特殊文件或字符特殊文件，则本次打开及后续&nbsp;IO&nbsp;进行非阻塞操作。<br/>-&nbsp;OpenMode.DIR(0o200000)：如果path不指向目录，则出错。<br/>-&nbsp;OpenMode.NOFOLLOW(0o400000)：如果path指向符号链接，则出错。<br/>-&nbsp;OpenMode.SYNC(0o4010000)：以同步IO的方式打开文件。 |

**返回值：**

  | 类型     | 说明          |
  | ------ | ----------- |
  | [File](#file) | 打开的File对象。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  console.info("file fd: " + file.fd);
  fs.closeSync(file);
  ```

## fs.read

read(fd: number, buffer: ArrayBuffer, options?: ReadOptions): Promise&lt;number&gt;

读取文件数据，使用promise异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名  | 类型        | 必填 | 说明                                                         |
| ------- | ----------- | ---- | ------------------------------------------------------------ |
| fd      | number      | 是   | 已打开的文件描述符。                                     |
| buffer  | ArrayBuffer | 是   | 用于保存读取到的文件数据的缓冲区。                           |
| options | [ReadOptions](#readoptions20)      | 否   | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望读取文件的位置。可选，默认从当前位置开始读。<br/>-&nbsp;length，number类型，表示期望读取数据的长度。可选，默认缓冲区长度。|

**返回值：**

  | 类型                                 | 说明     |
  | ---------------------------------- | ------ |
  | Promise&lt;number&gt; | Promise对象。返回实际读取的数据长度（单位：字节）。|

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  import { buffer } from '@kit.ArkTS';
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE);
  let arrayBuffer = new ArrayBuffer(4096);
  fs.read(file.fd, arrayBuffer).then((readLen: number) => {
    console.info("read file data succeed");
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

从文件读取数据，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                                       | 必填   | 说明                                       |
  | -------- | ---------------------------------------- | ---- | ---------------------------------------- |
  | fd       | number                                   | 是    | 已打开的文件描述符。                             |
  | buffer   | ArrayBuffer                              | 是    | 用于保存读取到的文件数据的缓冲区。                        |
  | options | [ReadOptions](#readoptions20)      | 否   | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望读取文件的位置。可选，默认从当前位置开始读。<br/>-&nbsp;length，number类型，表示期望读取数据的长度。可选，默认缓冲区长度。|
  | callback | AsyncCallback&lt;number&gt; | 是    | 异步读取数据之后的回调。返回实际读取的数据长度，单位字节。                             |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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
      console.info("read file data succeed");
      let buf = buffer.from(arrayBuffer, 0, readLen);
      console.info(`The content of file: ${buf.toString()}`);
    }
    fs.closeSync(file);
  });
  ```

## fs.readSync

readSync(fd: number, buffer: ArrayBuffer, options?: ReadOptions): number

以同步方法从文件读取数据。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名     | 类型          | 必填   | 说明                                       |
  | ------- | ----------- | ---- | ---------------------------------------- |
  | fd      | number      | 是    | 已打开的文件描述符。                             |
  | buffer  | ArrayBuffer | 是    | 用于保存读取到的文件数据的缓冲区。                        |
  | options | [ReadOptions](#readoptions20)      | 否   | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望读取文件的位置。可选，默认从当前位置开始读。<br/>-&nbsp;length，number类型，表示期望读取数据的长度。可选，默认缓冲区长度。|

**返回值：**

  | 类型     | 说明       |
  | ------ | -------- |
  | number | 返回实际读取的数据长度（单位：字节）。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE);
  let buf = new ArrayBuffer(4096);
  fs.readSync(file.fd, buf);
  fs.closeSync(file);
  ```

## fs.rmdir

rmdir(path: string): Promise&lt;void&gt;

删除整个目录，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                       |
| ------ | ------ | ---- | -------------------------- |
| path   | string | 是   | 目录的应用沙箱路径。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.rmdir(dirPath).then(() => {
      console.info("rmdir succeed");
  }).catch((err) => {
      console.info("rmdir failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.rmdir

rmdir(path: string, callback: AsyncCallback&lt;void&gt;): void

删除整个目录，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                      | 必填 | 说明                       |
| -------- | ------------------------- | ---- | -------------------------- |
| path     | string                    | 是   | 目录的应用沙箱路径。 |
| callback | AsyncCallback&lt;void&gt; | 是   | 异步删除目录之后的回调。   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.rmdir(dirPath, (err) => {
    if (err) {
      console.info("rmdir failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("rmdir succeed");
    }
  });
  ```

## fs.rmdirSync

rmdirSync(path: string): void

以同步方法删除目录。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                       |
| ------ | ------ | ---- | -------------------------- |
| path   | string | 是   | 目录的应用沙箱路径。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let dirPath = pathDir + "/testDir";
  fs.rmdirSync(dirPath);
  ```

## fs.unlink

unlink(path: string): Promise&lt;void&gt;

删除单个文件，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                       |
| ------ | ------ | ---- | -------------------------- |
| path   | string | 是   | 文件的应用沙箱路径。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.unlink(filePath).then(() => {
      console.info("remove file succeed");
  }).catch((err) => {
      console.info("remove file failed with error message: " + err.message + ", error code: " + err.codeor);
  });
  ```

## fs.unlink

unlink(path: string, callback: AsyncCallback&lt;void&gt;): void

删除文件，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                      | 必填 | 说明                       |
| -------- | ------------------------- | ---- | -------------------------- |
| path     | string                    | 是   | 文件的应用沙箱路径。 |
| callback | AsyncCallback&lt;void&gt; | 是   | 异步删除文件之后的回调。   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.unlink(filePath, (err) => {
    if (err) {
      console.info("remove file failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("remove file succeed");
    }
  });
  ```

## fs.unlinkSync

unlinkSync(path: string): void

以同步方法删除文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                       |
| ------ | ------ | ---- | -------------------------- |
| path   | string | 是   | 文件的应用沙箱路径。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  fs.unlinkSync(filePath);
  ```

## fs.write

write(fd: number, buffer: ArrayBuffer | string, options?: WriteOptions): Promise&lt;number&gt;

将数据写入文件，使用promise异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名     | 类型                              | 必填   | 说明                                       |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | fd      | number                          | 是    | 已打开的文件描述符。                             |
  | buffer  | ArrayBuffer \| string | 是    | 待写入文件的数据，可来自缓冲区或字符串。                     |
  | options | [WriteOptions](#writeoptions20)                          | 否    | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望写入文件的位置。可选，默认从当前位置开始写入。<br/>-&nbsp;length，number类型，表示期望写入数据的长度。可选，默认缓冲区长度。<br/>-&nbsp;encoding，string类型，当数据是string类型时有效，表示数据的编码方式，默认&nbsp;'utf-8'。当前仅支持&nbsp;'utf-8'。|

**返回值：**

  | 类型                    | 说明       |
  | --------------------- | -------- |
  | Promise&lt;number&gt; | Promise对象。返回实际写入的数据长度（单位：字节）。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

将数据写入文件，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                              | 必填   | 说明                                       |
  | -------- | ------------------------------- | ---- | ---------------------------------------- |
  | fd       | number                          | 是    | 已打开的文件描述符。                             |
  | buffer   | ArrayBuffer \| string | 是    | 待写入文件的数据，可来自缓冲区或字符串。                     |
  | options | [WriteOptions](#writeoptions20)                          | 否    | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望写入文件的位置。可选，默认从当前位置开始写。<br/>-&nbsp;length，number类型，表示期望写入数据的长度。可选，默认缓冲区长度。<br/>-&nbsp;encoding，string类型，当数据是string类型时有效，表示数据的编码方式，默认&nbsp;'utf-8'。当前仅支持&nbsp;'utf-8'。|
  | callback | AsyncCallback&lt;number&gt;     | 是    | 异步将数据写入完成后执行的回调函数。                       |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

以同步方法将数据写入文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名     | 类型                              | 必填   | 说明                                       |
  | ------- | ------------------------------- | ---- | ---------------------------------------- |
  | fd      | number                          | 是    | 已打开的文件描述符。                             |
  | buffer  | ArrayBuffer \| string | 是    | 待写入文件的数据，可来自缓冲区或字符串。                     |
  | options | [WriteOptions](#writeoptions20)                          | 否    | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望写入文件的位置。可选，默认从当前位置开始写。<br/>-&nbsp;length，number类型，表示期望写入数据的长度。可选，默认缓冲区长度。<br/>-&nbsp;encoding，string类型，当数据是string类型时有效，表示数据的编码方式，默认&nbsp;'utf-8'。当前仅支持&nbsp;'utf-8'。|

**返回值：**

  | 类型     | 说明       |
  | ------ | -------- |
  | number | 返回实际写入的数据长度（单位：字节）。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath, fs.OpenMode.READ_WRITE | fs.OpenMode.CREATE);
  let str: string = "hello, world";
  let writeLen = fs.writeSync(file.fd, str);
  console.info("write data to file succeed and size is:" + writeLen);
  fs.closeSync(file);
  ```

## fs.truncate

truncate(file: string | number, len?: number): Promise&lt;void&gt;

截断文件，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                             |
| ------ | ------ | ---- | -------------------------------- |
| file   | string\|number | 是   | 文件的应用沙箱路径或已打开的文件描述符fd。       |
| len    | number | 否   | 文件截断后的长度，以字节为单位。默认为0。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let len = 5;
  fs.truncate(filePath, len).then(() => {
      console.info("truncate file succeed");
  }).catch((err) => {
      console.info("truncate file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.truncate

truncate(file: string | number, len?: number, callback: AsyncCallback&lt;void&gt;): void

截断文件，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                      | 必填 | 说明                             |
| -------- | ------------------------- | ---- | -------------------------------- |
| file     | string\|number                    | 是   | 文件的应用沙箱路径或已打开的文件描述符fd。       |
| len      | number                    | 否   | 文件截断后的长度，以字节为单位。默认为0。 |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数，本调用无返回值。   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let len = 5;
  fs.truncate(filePath, len, (err) => {
    if (err) {
      console.info("truncate failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("truncate succeed");
    }
  });
  ```

## fs.truncateSync

truncateSync(file: string | number, len?: number): void

以同步方法截断文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                             |
| ------ | ------ | ---- | -------------------------------- |
| file   | string\|number | 是   | 文件的应用沙箱路径或已打开的文件描述符fd。       |
| len    | number | 否   | 文件截断后的长度，以字节为单位。默认为0。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let len = 5;
  fs.truncateSync(filePath, len);
  ```

## fs.readText

readText(filePath: string, options?: ReadTextOptions): Promise&lt;string&gt;

基于文本方式读取文件（即直接读取文件的文本内容），使用promise异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型   | 必填 | 说明                                                         |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | string | 是   | 文件的应用沙箱路径。                                   |
| options  | [ReadTextOptions](#readtextoptions20) | 否   | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望读取文件的位置。可选，默认从当前位置开始读取。<br/>-&nbsp;length，number类型，表示期望读取数据的长度。可选，默认文件长度。<br/>-&nbsp;encoding，string类型，当数据是&nbsp;string&nbsp;类型时有效，表示数据的编码方式，默认&nbsp;'utf-8'，仅支持&nbsp;'utf-8'。 |

**返回值：**

  | 类型                    | 说明         |
  | --------------------- | ---------- |
  | Promise&lt;string&gt; | Promise对象。返回读取文件的内容。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

基于文本方式读取文件内容，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                                         |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| filePath | string                      | 是   | 文件的应用沙箱路径。                                   |
| options  | [ReadTextOptions](#readtextoptions20)                      | 否   | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望读取文件的位置。可选，默认从当前位置开始读取。<br/>-&nbsp;length，number类型，表示期望读取数据的长度。可选，默认文件长度。<br/>-&nbsp;encoding，string类型，表示数据的编码方式，默认&nbsp;'utf-8'，仅支持&nbsp;'utf-8'。 |
| callback | AsyncCallback&lt;string&gt; | 是   | 回调函数，返回读取文件的内容。                         |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

以同步方法基于文本方式读取文件（即直接读取文件的文本内容）。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型   | 必填 | 说明                                                         |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | string | 是   | 文件的应用沙箱路径。                                   |
| options  | [ReadTextOptions](#readtextoptions20) | 否   | 支持如下选项：<br/>-&nbsp;offset，number类型，表示期望读取文件的位置。可选，默认从当前位置开始读取。<br/>-&nbsp;length，number类型，表示期望读取数据的长度。可选，默认文件长度。<br/>-&nbsp;encoding，string类型，当数据是&nbsp;string&nbsp;类型时有效，表示数据的编码方式，默认&nbsp;'utf-8'，仅支持&nbsp;'utf-8'。 |

**返回值：**

  | 类型   | 说明                 |
  | ------ | -------------------- |
  | string | 返回读取文件的内容。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

## fs.lstat<sup>20+</sup>

lstat(path: string): Promise&lt;Stat&gt;

获取链接文件信息，使用promise异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                   |
| ------ | ------ | ---- | -------------------------------------- |
| path   | string | 是   | 文件的应用沙箱路径。 |

**返回值：**

  | 类型                           | 说明         |
  | ---------------------------- | ---------- |
  | Promise&lt;[Stat](#stat)&gt; | Promise对象。返回Stat对象，表示文件的具体信息，详情见Stat。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/linkToFile";
  fs.lstat(filePath).then((stat: fs.Stat) => {
    console.info("lstat succeed, the size of file is " + stat.size);
  }).catch((err: BusinessError) => {
    console.error("lstat failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.lstat<sup>20+</sup>

lstat(path: string, callback: AsyncCallback&lt;Stat&gt;): void

获取链接文件信息，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                               | 必填 | 说明                                   |
| -------- | ---------------------------------- | ---- | -------------------------------------- |
| path     | string                             | 是   | 文件的应用沙箱路径。 |
| callback | AsyncCallback&lt;[Stat](#stat)&gt; | 是   | 异步获取文件具体信息之后的回调。       |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/linkToFile";
  fs.lstat(filePath, (err: BusinessError, stat: fs.Stat) => {
    if (err) {
      console.error("lstat failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("lstat succeed, the size of file is" + stat.size);
    }
  });
  ```

## fs.lstatSync<sup>20+</sup>

lstatSync(path: string): Stat

以同步方法获取链接文件信息。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                   |
| ------ | ------ | ---- | -------------------------------------- |
| path   | string | 是   | 文件的应用沙箱路径。 |

**返回值：**

  | 类型            | 说明         |
  | ------------- | ---------- |
  | [Stat](#stat) | 表示文件的具体信息。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  let filePath = pathDir + "/linkToFile";
  let fileStat = fs.lstatSync(filePath);
  console.info("lstat succeed, the size of file is" + fileStat.size);
  ```

## fs.rename

rename(oldPath: string, newPath: string): Promise&lt;void&gt;

重命名文件或文件夹，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名  | 类型   | 必填 | 说明                         |
| ------- | ------ | ---- | ---------------------------- |
| oldPath | string | 是   | 文件的应用沙箱原路径。 |
| newPath | string | 是   | 文件的应用沙箱新路径。   |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let srcFile = pathDir + "/test.txt";
  let dstFile = pathDir + "/new.txt";
  fs.rename(srcFile, dstFile).then(() => {
      console.info("rename succeed");
  }).catch((err) => {
      console.info("rename failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.rename

rename(oldPath: string, newPath: string, callback: AsyncCallback&lt;void&gt;): void

重命名文件或文件夹，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名   | 类型                      | 必填 | 说明                         |
| -------- | ------------------------- | ---- | ---------------------------- |
| oldPath | string | 是   | 文件的应用沙箱原路径。 |
| newPath | string | 是   | 文件的应用沙箱新路径。   |
| callback | AsyncCallback&lt;void&gt; | 是   | 异步重命名文件之后的回调。   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let srcFile = pathDir + "/test.txt";
  let dstFile = pathDir + "/new.txt";
  fs.rename(srcFile, dstFile, (err) => {
    if (err) {
      console.info("rename failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("rename succeed");
    }
  });
  ```

## fs.renameSync

renameSync(oldPath: string, newPath: string): void

以同步方法重命名文件或文件夹。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名  | 类型   | 必填 | 说明                         |
| ------- | ------ | ---- | ---------------------------- |
| oldPath | string | 是   | 文件的应用沙箱原路径。 |
| newPath | string | 是   | 文件的应用沙箱新路径。   |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let srcFile = pathDir + "/test.txt";
  let dstFile = pathDir + "/new.txt";
  fs.renameSync(srcFile, dstFile);
  ```

## fs.fsync

fsync(fd: number): Promise&lt;void&gt;

同步文件数据，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型     | 必填   | 说明           |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | 是    | 已打开的文件描述符。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fsync(file.fd).then(() => {
      console.info("sync data succeed");
  }).catch((err) => {
      console.info("sync data failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.fsync

fsync(fd: number, callback: AsyncCallback&lt;void&gt;): void

同步文件数据，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                        | 必填   | 说明              |
  | -------- | ------------------------- | ---- | --------------- |
  | fd       | number                    | 是    | 已打开的文件描述符。    |
  | Callback | AsyncCallback&lt;void&gt; | 是    | 异步将文件数据同步之后的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fsync(file.fd, (err) => {
    if (err) {
      console.info("fsync failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("fsync succeed");
      fs.closeSync(file);
    }
  });
  ```


## fs.fsyncSync

fsyncSync(fd: number): void

以同步方法同步文件数据。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型     | 必填   | 说明           |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | 是    | 已打开的文件描述符。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fsyncSync(file.fd);
  fs.closeSync(file);
  ```

## fs.fdatasync

fdatasync(fd: number): Promise&lt;void&gt;

实现文件内容数据同步，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型     | 必填   | 说明           |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | 是    | 已打开的文件描述符。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fdatasync(file.fd).then((err) => {
    console.info("sync data succeed");
    fs.closeSync(file);
  }).catch((err) => {
    console.info("sync data failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.fdatasync

fdatasync(fd: number, callback: AsyncCallback&lt;void&gt;): void

实现文件内容数据同步，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                              | 必填   | 说明                |
  | -------- | ------------------------------- | ---- | ----------------- |
  | fd       | number                          | 是    | 已打开的文件描述符。      |
  | callback | AsyncCallback&lt;void&gt; | 是    | 异步将文件内容数据同步之后的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let file = fs.openSync(filePath);
  fs.fdatasync (file.fd, (err) => {
    if (err) {
      console.info("fdatasync failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("fdatasync succeed");
      fs.closeSync(file);
    }
  });
  ```

## fs.fdatasyncSync

fdatasyncSync(fd: number): void

以同步方法实现文件内容数据同步。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名  | 类型     | 必填   | 说明           |
  | ---- | ------ | ---- | ------------ |
  | fd   | number | 是    | 已打开的文件描述符。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

列出文件夹下所有文件名，支持递归列出所有文件名（包含子目录下），支持文件过滤，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | path | string | 是    | 文件夹的应用沙箱路径。 |
  | options | Object | 否    | 文件过滤选项。默认不进行过滤。 |

**options参数说明：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | recursion | boolean | 否    | 是否递归子目录下文件名，默认为false。 |
  | listNum | number | 否    | 列出文件名数量。当设置0时，列出所有文件，默认为0。 |
  | filter | [Filter](#filter) | 否    | 文件过滤选项。当前仅支持后缀名匹配、文件名模糊查询、文件大小过滤、最近修改时间过滤。 |

**返回值：**

  | 类型                   | 说明         |
  | --------------------- | ---------- |
  | Promise&lt;string[]&gt; | Promise对象。返回文件名数组。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

列出文件夹下所有文件名，支持递归列出所有文件名（包含子目录下），支持文件过滤，使用Callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | path | string | 是    | 文件夹的应用沙箱路径。 |
  | options | Object | 否    | 文件过滤选项。默认不进行过滤。 |
  | callback | AsyncCallback&lt;string[]&gt; | 是    | 异步列出文件名数组之后的回调。              |

**options参数说明：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | recursion | boolean | 否    | 是否递归子目录下文件名，默认为false。 |
  | listNum | number | 否    | 列出文件名数量。当设置0时，列出所有文件，默认为0。 |
  | filter | [Filter](#filter) | 否    | 文件过滤选项。当前仅支持后缀名匹配、文件名模糊查询、文件大小过滤、最近修改时间过滤。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

以同步方式列出文件夹下所有文件名，支持递归列出所有文件名（包含子目录下），支持文件过滤。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | path | string | 是    | 文件夹的应用沙箱路径。 |
  | options | Object | 否    | 文件过滤选项。默认不进行过滤。 |

**options参数说明：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | recursion | boolean | 否    | 是否递归子目录下文件名，默认为false。 |
  | listNum | number | 否    | 列出文件名数量。当设置0时，列出所有文件，默认为0。 |
  | filter | [Filter](#filter) | 否    | 文件过滤选项。当前仅支持后缀名匹配、文件名模糊查询、文件大小过滤、最近修改时间过滤。 |

**返回值：**

  | 类型                   | 说明         |
  | --------------------- | ---------- |
  | string[] | 返回文件名数组。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

## fs.moveDir<sup>20+</sup>

moveDir(src: string, dest: string, mode?: number): Promise\<void>

移动源目录至目标路径下，使用promise异步回调。

> **说明：**
>
> 该接口不支持在分布式文件路径下操作。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | mode | number | 否    | 移动模式，默认值为0。<br/>-&nbsp;mode为0，目录级别抛异常。若目标目录下存在与源目录名冲突的非空目录，则抛出异常。<br/>-&nbsp;mode为1，文件级别抛异常。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则抛出异常。源目录下未冲突的文件全部移动至目标目录下，目标目录下未冲突文件将继续保留，且冲突文件信息将在抛出异常的data属性中以Array\<[ConflictFiles](#conflictfiles20)>形式提供。<br/>-&nbsp; mode为2，文件级别强制覆盖。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则强制覆盖冲突目录下所有同名文件，未冲突文件将继续保留。<br/>-&nbsp; mode为3，目录级别强制覆盖。移动源目录至目标目录下，目标目录下移动的目录内容与源目录完全一致。若目标目录下存在与源目录名冲突的目录，该目录下的所有原始文件将被删除。|

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

移动源目录至目标路径下，支持设置移动模式。使用callback异步回调。

> **说明：**
>
> 该接口不支持在分布式文件路径下操作。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | mode | number | 是    | 移动模式，默认值为0。<br/>-&nbsp;mode为0，目录级别抛异常。若目标目录下存在与源目录名冲突的目录，则抛出异常。<br/>-&nbsp;mode为1，文件级别抛异常。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则抛出异常。源目录下未冲突的文件全部移动至目标目录下，目标目录下未冲突文件将继续保留，且冲突文件信息将在抛出异常的data属性中以Array\<[ConflictFiles](#conflictfiles20)>形式提供。<br/>-&nbsp; mode为2，文件级别强制覆盖。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则强制覆盖冲突目录下所有同名文件，未冲突文件将继续保留。<br/>-&nbsp; mode为3，目录级别强制覆盖。移动源目录至目标目录下，目标目录下移动的目录内容与源目录完全一致。若目标目录下存在与源目录名冲突的目录，该目录下所有原始文件将被删除。|
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | 是    | 异步移动目录之后的回调。              |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

移动源目录至目标路径下。使用callback异步回调。

移动模式为目录级别抛异常。当目标目录下存在与源目录名冲突的目录，则抛出异常。

> **说明：**
>
> 该接口不支持在分布式文件路径下操作。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | callback | AsyncCallback&lt;void, Array&lt;[ConflictFiles](#conflictfiles20)&gt;&gt; | 是    | 异步移动目录之后的回调。              |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

以同步方法移动源目录至目标路径下。

> **说明：**
>
> 该接口不支持在分布式文件路径下操作。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源目录的应用沙箱路径。 |
  | dest | string | 是    | 目标目录的应用沙箱路径。 |
  | mode | number | 否    | 移动模式，默认值为0。<br/>-&nbsp;mode为0，目录级别抛异常。若目标目录下存在与源目录名冲突的目录，则抛出异常。<br/>-&nbsp;mode为1，文件级别抛异常。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则抛出异常。源目录下未冲突的文件全部移动至目标目录下，目标目录下未冲突文件将继续保留，且冲突文件信息将在抛出异常的data属性中以Array\<[ConflictFiles](#conflictfiles20)>形式提供。<br/>-&nbsp; mode为2，文件级别强制覆盖。目标目录下存在与源目录名冲突的目录，若冲突目录下存在同名文件，则强制覆盖冲突目录下所有同名文件，未冲突文件将继续保留。<br/>-&nbsp; mode为3，目录级别强制覆盖。移动源目录至目标目录下，目标目录下移动的目录内容与源目录完全一致。若目标目录下存在与源目录名冲突的目录，该目录下所有原始文件将被删除。|

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

## fs.moveFile<sup>20+</sup>

moveFile(src: string, dest: string, mode?: number): Promise\<void>

移动文件，使用promise异步回调。

> **说明：**
>
> 该接口不支持在分布式文件路径下操作。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源文件的应用沙箱路径。 |
  | dest | string | 是    | 目标文件的应用沙箱路径。 |
  | mode | number | 否    | 移动模式。若mode为0，移动位置存在同名文件时，强制移动覆盖。若mode为1，移动位置存在同名文件时，抛出异常。默认为0。 |

**返回值：**

  | 类型                  | 说明                           |
  | ------------------- | ---------------------------- |
  | Promise&lt;void&gt; | Promise对象。无返回值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let srcPath = pathDir + "/source.txt";
  let destPath = pathDir + "/dest.txt";
  fs.moveFile(srcPath, destPath, 0).then(() => {
    console.info("move file succeed");
  }).catch((err: BusinessError) => {
    console.error("move file failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.moveFile

moveFile(src: string, dest: string, mode?: number, callback: AsyncCallback\<void>): void

移动文件，使用Callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源文件的应用沙箱路径。 |
  | dest | string | 是    | 目的文件的应用沙箱路径。 |
  | mode | number | 否    | 移动模式。若mode为0，移动位置存在同名文件时，强制移动覆盖。若mode为1，移动位置存在同名文件时，抛出异常。默认为0。 |
  | callback | AsyncCallback&lt;void&gt; | 是    | 异步移动文件之后的回调。              |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

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

moveFileSync(src: string, dest: string, mode?: number): void

以同步方式移动文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | src | string | 是    | 源文件的应用沙箱路径。 |
  | dest | string | 是    | 目的文件的应用沙箱路径。 |
  | mode | number | 否    | 移动模式。若mode为0，移动位置存在同名文件时，强制移动覆盖。若mode为1，移动位置存在同名文件时，抛出异常。默认为0。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let srcPath = pathDir + "/source.txt";
  let destPath = pathDir + "/dest.txt";
  fs.moveFileSync(srcPath, destPath, 0);
  console.info("move file succeed");
  ```

## fs.mkdtemp

mkdtemp(prefix: string): Promise&lt;string&gt;

创建临时目录，使用Promise异步返回。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | prefix | string | 是    | 以“XXXXXX”结尾目录路径。路径末尾的 "XXXXXX" 字符将被替换为随机字符，以创建一个唯一的目录名。 |

**返回值：**

  | 类型                   | 说明         |
  | --------------------- | ---------- |
  | Promise&lt;string&gt; | Promise对象。返回生成的唯一目录路径。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  fs.mkdtemp(pathDir + "/XXXXXX").then((pathDir) => {
      console.info("mkdtemp succeed:" + pathDir);
  }).catch((err) => {
      console.info("mkdtemp failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## fs.mkdtemp

mkdtemp(prefix: string, callback: AsyncCallback&lt;string&gt;): void

创建临时目录，使用callback异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名      | 类型                          | 必填   | 说明                          |
  | -------- | --------------------------- | ---- | --------------------------- |
  | prefix   | string                      | 是    | 用随机产生的字符串替换以“XXXXXX”结尾目录路径。 |
  | callback | AsyncCallback&lt;string&gt; | 是    | 异步创建临时目录之后的回调。              |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  fs.mkdtemp(pathDir + "/XXXXXX", (err, res) => {
    if (err) {
      console.info("mkdtemp failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("mkdtemp succeed");
    }
  });
  ```

## fs.mkdtempSync

mkdtempSync(prefix: string): string

以同步的方法创建临时目录。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

  | 参数名    | 类型     | 必填   | 说明                          |
  | ------ | ------ | ---- | --------------------------- |
  | prefix | string | 是    | 用随机产生的字符串替换以“XXXXXX”结尾目录路径。 |

**返回值：**

  | 类型    | 说明         |
  | ------ | ---------- |
  | string | 产生的唯一目录路径。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let res = fs.mkdtempSync(pathDir + "/XXXXXX");
  ```  

## Stat

文件具体信息，在调用Stat的方法前，需要先通过[stat()](#fsstat)方法（同步或异步）来构建一个Stat实例。

**系统能力**：SystemCapability.FileManagement.File.FileIO

### 属性

| 名称     | 类型   | 可读   | 可写   | 说明                                       |
| ------ | ------ | ---- | ---- | ---------------------------------------- |                        
| ino    | bigint | 是    | 否    | 标识该文件。通常同设备上的不同文件的INO不同。|                 |
| mode   | number | 是    | 否    | 表示文件权限，各特征位的含义如下：<br/>-&nbsp;0o400：用户读，对于普通文件，所有者可读取文件；对于目录，所有者可读取目录项。<br/>-&nbsp;0o200：用户写，对于普通文件，所有者可写入文件；对于目录，所有者可创建/删除目录项。<br/>-&nbsp;0o100：用户执行，对于普通文件，所有者可执行文件；对于目录，所有者可在目录中搜索给定路径名。<br/>-&nbsp;0o040：用户组读，对于普通文件，所有用户组可读取文件；对于目录，所有用户组可读取目录项。<br/>-&nbsp;0o020：用户组写，对于普通文件，所有用户组可写入文件；对于目录，所有用户组可创建/删除目录项。<br/>-&nbsp;0o010：用户组执行，对于普通文件，所有用户组可执行文件；对于目录，所有用户组是否可在目录中搜索给定路径名。<br/>-&nbsp;0o004：其他读，对于普通文件，其余用户可读取文件；对于目录，其他用户组可读取目录项。<br/>-&nbsp;0o002：其他写，对于普通文件，其余用户可写入文件；对于目录，其他用户组可创建/删除目录项。<br/>-&nbsp;0o001：其他执行，对于普通文件，其余用户可执行文件；对于目录，其他用户组可在目录中搜索给定路径名。 |
| uid    | number | 是    | 否    | 文件所有者的ID。|
| gid    | number | 是    | 否    | 文件所有组的ID。|
| size   | number | 是    | 否    | 文件的大小，以字节为单位。仅对普通文件有效。  |
| atime  | number | 是    | 否    | 上次访问该文件的时间，表示距1970年1月1日0时0分0秒的秒数。        |
| mtime  | number | 是    | 否    | 上次修改该文件的时间，表示距1970年1月1日0时0分0秒的秒数。        |
| ctime  | number | 是    | 否    | 最近改变文件状态的时间，表示距1970年1月1日0时0分0秒的秒数。      |

### isBlockDevice

isBlockDevice(): boolean

用于判断文件是否是块特殊文件。一个块特殊文件只能以块为粒度进行访问，且访问的时候带缓存。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**返回值：**

  | 类型      | 说明               |
  | ------- | ---------------- |
  | boolean | 表示文件是否是块特殊设备。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let isBLockDevice = fs.statSync(filePath).isBlockDevice();
  ```

### isCharacterDevice

isCharacterDevice(): boolean

用于判断文件是否是字符特殊文件。一个字符特殊设备可进行随机访问，且访问的时候不带缓存。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**返回值：**

  | 类型      | 说明                |
  | ------- | ----------------- |
  | boolean | 表示文件是否是字符特殊设备。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let isCharacterDevice = fs.statSync(filePath).isCharacterDevice();
  ```

### isDirectory

isDirectory(): boolean

用于判断文件是否是目录。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**返回值：**

  | 类型      | 说明            |
  | ------- | ------------- |
  | boolean | 表示文件是否是目录。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let dirPath = pathDir + "/test";
  let isDirectory = fs.statSync(dirPath).isDirectory(); 
  ```

### isFIFO

isFIFO(): boolean

用于判断文件是否是命名管道（有时也称为FIFO）。命名管道通常用于进程间通信。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**返回值：**

  | 类型      | 说明                    |
  | ------- | --------------------- |
  | boolean | 表示文件是否是&nbsp;FIFO。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let isFIFO = fs.statSync(filePath).isFIFO(); 
  ```

### isFile

isFile(): boolean

用于判断文件是否是普通文件。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**返回值：**

  | 类型      | 说明              |
  | ------- | --------------- |
  | boolean | 表示文件是否是普通文件。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let isFile = fs.statSync(filePath).isFile();
  ```

### isSocket

isSocket(): boolean

用于判断文件是否是套接字。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**返回值：**

  | 类型      | 说明             |
  | ------- | -------------- |
  | boolean | 表示文件是否是套接字。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test.txt";
  let isSocket = fs.statSync(filePath).isSocket(); 
  ```

### isSymbolicLink

isSymbolicLink(): boolean

用于判断文件是否是符号链接。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**返回值：**

  | 类型      | 说明              |
  | ------- | --------------- |
  | boolean | 表示文件是否是符号链接。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```js
  let filePath = pathDir + "/test";
  let isSymbolicLink = fs.statSync(filePath).isSymbolicLink(); 
  ```

## File

由open接口打开的File对象。

**系统能力**：SystemCapability.FileManagement.File.FileIO

### 属性

| 名称   | 类型   | 可读   | 可写   | 说明      |
| ---- | ------ | ---- | ---- | ------- |
| fd | number | 是    | 否    | 打开的文件描述符。 |

## OpenMode

open接口flags参数常量。文件打开标签。

**系统能力**：SystemCapability.FileManagement.File.FileIO

| 名称   | 类型   | 值  | 说明      |
| ---- | ------ |---- | ------- |
| READ_ONLY | number |  0o0   | 只读打开。 |
| WRITE_ONLY | number | 0o1    | 只写打开。 |
| READ_WRITE | number | 0o2    | 读写打开。 |
| CREATE | number | 0o100    | 若文件不存在，则创建文件。 |
| TRUNC | number | 0o1000    | 如果文件存在且以只写或读写的方式打开文件，则将其长度裁剪为零。 |
| APPEND | number | 0o2000   | 以追加方式打开，后续写将追加到文件末尾。 |
| NONBLOCK | number | 0o4000    | 如果path指向FIFO、块特殊文件或字符特殊文件，则本次打开及后续 IO 进行非阻塞操作。 |
| DIR | number | 0o200000    | 如果path不指向目录，则出错。 |
| NOFOLLOW | number | 0o400000    | 如果path指向符号链接，则出错。 |
| SYNC | number | 0o4010000    | 以同步IO的方式打开文件。 |

## Filter

**系统能力**：SystemCapability.FileManagement.File.FileIO

文件过滤配置项类型，支持listFile接口使用。

| 名称        | 类型       | 说明                |
| ----------- | --------------- | ------------------ |
| suffix | Array&lt;string&gt;     | 文件后缀名完全匹配，各个关键词OR关系。           |
| displayName    | Array&lt;string&gt;     | 文件名模糊匹配，各个关键词OR关系。 |
| mimeType    | Array&lt;string&gt; | mime类型完全匹配，各个关键词OR关系。       |
| fileSizeOver    | number | 文件大小匹配，大于等于指定大小的文件。       |
| lastModifiedAfter    | number | 文件最近修改时间匹配，在指定时间点及之后的文件。       |
| excludeMedia    | boolean | 是否排除Media中已有的文件。       |

## ConflictFiles<sup>20+</sup>

冲突文件信息，支持copyDir及moveDir接口使用。

**系统能力**：SystemCapability.FileManagement.File.FileIO

| 名称        | 类型       | 说明                |
| ----------- | --------------- | ------------------ |
| srcFile | string     | 源冲突文件路径。           |
| destFile    | string     | 目标冲突文件路径。 |

## Options<sup>20+</sup>

可选项类型，支持readLines接口使用。

**系统能力**：SystemCapability.FileManagement.File.FileIO

| 名称        | 类型       | 说明                |
| ----------- | --------------- | ------------------ |
| encoding | string     | 文件编码方式。可选项。           |

## AccessModeType<sup>20+</sup>

枚举，表示需要校验的具体权限。若不填，默认校验文件是否存在。

**系统能力**：SystemCapability.FileManagement.File.FileIO

| 名称        | 值       | 说明                |
| ----------- | --------------- | ------------------ |
| EXIST | 0     | 文件是否存在。           |
| WRITE    | 2     | 文件是否具有写入权限。 |
| READ    | 4     | 文件是否具有读取权限。 |
| READ_WRITE    | 6     | 文件是否具有读写权限。 |

## ReadOptions<sup>20+</sup>

可选项类型，支持read接口使用。

**系统能力**：SystemCapability.FileManagement.File.FileIO

| 名称        | 类型       | 必选       | 说明                |
| ----------- | --------------- | ------------------ |------------------ |
| length | number     | 否 | 期望读取数据的长度，单位为字节。可选，默认缓冲区长度。           |
|  offset    | number     | 否 | 期望读取文件位置，单位为字节（基于当前filePointer加上offset的位置）。可选，默认从偏移指针（filePointer）开始读。 |

## ReadTextOptions<sup>20+</sup>

可选项类型，支持readText接口使用，ReadTextOptions继承至[ReadOptions](#readoptions20)。

**系统能力**：SystemCapability.FileManagement.File.FileIO

| 名称        | 类型       | 必选       | 说明                |
| ----------- | --------------- | ------------------ | ------------------ |
| length | number     | 否 | 期望读取数据的长度，单位为字节。可选，默认文件长度。           |
|  offset    | number     | 否 | 期望读取文件的位置，单位为字节。可选，默认从当前位置开始读取。 |
| encoding    | string | 否 | 当数据是 string 类型时有效，表示数据的编码方式，默认 'utf-8'，仅支持 'utf-8'。 |

## WriteOptions<sup>20+</sup>

可选项类型，支持write接口使用，WriteOptions继承至[Options](#options20)。

**系统能力**：SystemCapability.FileManagement.File.FileIO

| 名称        | 类型       | 必选       | 说明                |
| ----------- | --------------- | ------------------ | ------------------ |
| length | number     | 否 | 期望写入数据的长度，单位为字节。可选，默认缓冲区长度。 |
|  offset    | number     | 否 | 期望写入文件位置，单位为字节（基于当前filePointer加上offset的位置）。可选，默认从偏移指针（filePointer）开始写。 |
| encoding    | string | 否 | 当数据是string类型时有效，表示数据的编码方式。默认 'utf-8'。仅支持 'utf-8'。       |
