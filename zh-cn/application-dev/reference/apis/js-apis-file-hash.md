# @ohos.file.hash (文件哈希处理)

该模块提供文件哈希处理能力，对文件内容进行哈希处理。

> **说明：**
>
> 本模块接口从API version 20开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { hash } from '@kit.CoreFileKit';
```

## 使用说明

使用该功能模块对文件/目录进行操作前，需要先获取其应用沙箱路径，获取方式及其接口用法请参考：

  ```ts
  import { UIAbility } from '@kit.AbilityKit';
  import { window } from '@kit.ArkUI';

  export default class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage: window.WindowStage) {
      let context = this.context;
      let pathDir = context.filesDir;
    }
  }
  ```

## hash.hash<sup>20+</sup>

hash(path: string, algorithm: string): Promise&lt;string&gt;

计算文件的哈希值，使用Promise异步回调。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名    | 类型   | 必填 | 说明                           |
| --------- | ------ | ---- | ------------------------------|
| path      | string | 是   | 待计算哈希值文件的应用沙箱路径。 |
| algorithm | string | 是   | 哈希计算采用的算法。可选&nbsp;"md5"、"sha1"&nbsp;或&nbsp;"sha256"。建议采用安全强度更高的&nbsp;"sha256"。 |

**返回值：**

  | 类型                    | 说明                         |
  | --------------------- | -------------------------- |
  | Promise&lt;string&gt; | Promise对象。返回文件的哈希值。表示为十六进制数字串，所有字母均大写。 |

**错误码：**

以下错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 13900020 | Invalid argument |
| 13900042 | Unknown error |

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  hash.hash(filePath, "sha256").then((str: string) => {
    console.info("calculate file hash succeed:" + str);
  }).catch((err: BusinessError) => {
    console.error("calculate file hash failed with error message: " + err.message + ", error code: " + err.code);
  });
  ```

## hash.hash<sup>20+</sup>

hash(path: string, algorithm: string, callback: AsyncCallback&lt;string&gt;): void

计算文件的哈希值，使用callback异步回调。


**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名    | 类型                        | 必填 | 说明                                                         |
| --------- | --------------------------- | ---- | ------------------------------------------------------------ |
| path      | string                      | 是   | 待计算哈希值文件的应用沙箱路径。                             |
| algorithm | string                      | 是   | 哈希计算采用的算法。可选&nbsp;"md5"、"sha1"&nbsp;或&nbsp;"sha256"。建议采用安全强度更高的&nbsp;"sha256"。 |
| callback  | AsyncCallback&lt;string&gt; | 是   | 异步计算文件哈希操作之后的回调函数（其中给定文件哈希值表示为十六进制数字串，所有字母均大写）。 |

**错误码：**

以下错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 13900020 | Invalid argument |
| 13900042 | Unknown error |

**示例：**

  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';
  let filePath = pathDir + "/test.txt";
  hash.hash(filePath, "sha256", (err: BusinessError, str: string) => {
    if (err) {
      console.error("calculate file hash failed with error message: " + err.message + ", error code: " + err.code);
    } else {
      console.info("calculate file hash succeed:" + str);
    }
  });
  ```
## hash.createHash<sup>20+</sup>

createHash(algorithm: string): HashStream;

创建并返回 HashStream 对象，该对象可用于使用给定的 algorithm 生成哈希摘要。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| algorithm | string | 是  | 哈希计算采用的算法。可选 "md5"、"sha1" 或 "sha256"。建议采用安全强度更高的 "sha256"。 |

**返回值：**

  | 类型            | 说明         |
  | ------------- | ---------- |
  | [HashStream](#HashStream20) | HashStream 类的实例。 |

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  // pages/xxx.ets
  import { fileIo as fs } from '@kit.CoreFileKit';

  function hashFileWithStream() {
    const filePath = pathDir + "/test.txt";
    // 创建文件可读流
    const rs = fs.createReadStream(filePath);
    // 创建哈希流
    const hs = hash.createHash('sha256');
    rs.on('data', (emitData) => {
      const data = emitData?.data;
      hs.update(new Uint8Array(data?.split('').map((x: string) => x.charCodeAt(0))).buffer);
    });
    rs.on('close', async () => {
      const hashResult = hs.digest();
      const fileHash = await hash.hash(filePath, 'sha256');
      console.info(`hashResult: ${hashResult}, fileHash: ${fileHash}`);
    });
  }
  ```

## HashStream<sup>20+</sup>

HashStream 类是用于创建数据的哈希摘要的实用工具。由 [createHash](#hash.createHash20) 接口获得。

### update<sup>20+</sup>

update(data: ArrayBuffer): void

使用给定的 data 更新哈希内容，可多次调用。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  // 创建哈希流
  const hs = hash.createHash('sha256');
  hs.update(new Uint8Array('1234567890'?.split('').map((x: string) => x.charCodeAt(0))).buffer);
  hs.update(new Uint8Array('abcdefg'?.split('').map((x: string) => x.charCodeAt(0))).buffer);
  const hashResult = hs.digest();
  // 88A00F46836CD629D0B79DE98532AFDE3AEAD79A5C53E4848102F433046D0106
  console.info(`hashResult: ${hashResult}`);
  ```

### digest<sup>20+</sup>

digest(): string

计算传给被哈希的所有数据的摘要。

**系统能力**：SystemCapability.FileManagement.File.FileIO

**错误码：**

接口抛出错误码的详细介绍请参见[基础文件IO错误码](../errorcodes/errorcode-filemanagement.md#文件管理错误码)。

**示例：**

  ```ts
  // 创建哈希流
  const hs = hash.createHash('sha256');
  hs.update(new Uint8Array('1234567890'?.split('').map((x: string) => x.charCodeAt(0))).buffer);
  hs.update(new Uint8Array('abcdefg'?.split('').map((x: string) => x.charCodeAt(0))).buffer);
  const hashResult = hs.digest();
  // 88A00F46836CD629D0B79DE98532AFDE3AEAD79A5C53E4848102F433046D0106
  console.info(`hashResult: ${hashResult}`);
  ```