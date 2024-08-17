# @ohos.data.unifiedDataChannel (标准化数据通路)

本模块为统一数据管理框架（Unified Data Management Framework,UDMF）的组成部分，针对多对多跨应用数据共享的不同业务场景提供了标准化的数据通路，提供了标准化的数据接入与读取接口。同时对文本、图片等数据类型提供了标准化定义，方便不同应用间进行数据交互，减少数据类型适配的工作量。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import unifiedDataChannel from '@ohos.data.unifiedDataChannel';
```


## ValueType<sup>12+</sup>

type ValueType = number | string | boolean | image.PixelMap | Want | ArrayBuffer | object | null | undefined

用于表示统一数据记录允许的数据字段类型。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

| 类型 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- |
| number | 表示number的类型。 | 支持 | 支持 |
| string | 表示string的类型。 | 支持 | 支持 |
| boolean | 表示boolean的类型。 | 支持 | 支持 |
| image.PixelMap | 表示[image.PixelMap](js-apis-image.md#pixelmap7)的类型。 | 支持 | 支持 |
| Want | 表示[Want](js-apis-app-ability-want.md)的类型。 | 支持 | 支持 |
| ArrayBuffer | 表示ArrayBuffer的类型。 | 支持 | 支持 |
| object | 表示object的类型。 | 支持 | 支持 |
| null | 表示null。 | 支持 | 支持 |
| undefined | 表示undefined。 | 支持 | 支持 |

## UnifiedData

表示UDMF统一数据对象，提供封装一组数据记录的方法。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

### constructor<sup>12+</sup>

constructor()

用于创建统一数据对象。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**示例：**

```ts
let unifiedData = new unifiedDataChannel.UnifiedData();
```

### constructor

constructor(record: UnifiedRecord)

用于创建带有一条数据记录的统一数据对象。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明                                      |
| ------ | ------------------------------- | ---- |-----------------------------------------|
| record | [UnifiedRecord](#unifiedrecord) | 是   | 要添加到统一数据对象中的数据记录，该记录为UnifiedRecord或其子类对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
let text = new unifiedDataChannel.PlainText();
text.textContent = 'this is textContent of text';
let unifiedData = new unifiedDataChannel.UnifiedData(text);
```

### addRecord

addRecord(record: UnifiedRecord): void

在当前统一数据对象中添加一条数据记录。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明                                          |
| ------ | ------------------------------- | ---- |---------------------------------------------|
| record | [UnifiedRecord](#unifiedrecord) | 是   | 要添加到统一数据对象中的数据记录，该记录为UnifiedRecord子类对象。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
let text1 = new unifiedDataChannel.PlainText();
text1.textContent = 'this is textContent of text1';
let unifiedData = new unifiedDataChannel.UnifiedData(text1);

let text2 = new unifiedDataChannel.PlainText();
text2.textContent = 'this is textContent of text2';
unifiedData.addRecord(text2);
```

### getRecords

getRecords(): Array\<UnifiedRecord\>

将当前统一数据对象中的所有数据记录取出。通过本接口取出的数据为UnifiedRecord类型，需通过[getType](#gettype)获取数据类型后转为子类再使用。

**系统能力** ：SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                                     | 说明                      |
| ---------------------------------------- |-------------------------|
| Array\<[UnifiedRecord](#unifiedrecord)\> | 当前统一数据对象内所添加的记录。 |

**示例：**

```ts
import { uniformTypeDescriptor, unifiedDataChannel } from '@kit.ArkData';

let text = new unifiedDataChannel.PlainText();
text.textContent = 'this is textContent of text';
let unifiedData = new unifiedDataChannel.UnifiedData(text);

let link = new unifiedDataChannel.Hyperlink();
link.url = 'www.XXX.com';
unifiedData.addRecord(link);

let records = unifiedData.getRecords();
for (let i = 0; i < records.length; i++) {
  let record = records[i];
  if (record.getType() == uniformTypeDescriptor.UniformDataType.PLAIN_TEXT) {
    let plainText = record as unifiedDataChannel.PlainText;
    console.info(`textContent: ${plainText.textContent}`);
  } else if (record.getType() == uniformTypeDescriptor.UniformDataType.HYPERLINK) {
    let hyperlink = record as unifiedDataChannel.Hyperlink;
    console.info(`linkUrl: ${hyperlink.url}`);
  }
}
```

### hasType<sup>12+</sup>

hasType(type: string): boolean

检查当前统一数据对象中是否有指定的数据类型。

**系统能力** ：SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明                                          |
| ------ | ------------------------------- | ---- |---------------------------------------------|
| type | string | 是   | 要查询的数据类型，见[UniformDataType](js-apis-data-uniformTypeDescriptor.md#uniformdatatype)。|

**返回值：**

| 类型                                     | 说明                      |
| ---------------------------------------- |-------------------------|
| boolean | 有指定的数据类型返回true，否则返回false。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor, unifiedDataChannel } from '@kit.ArkData';

let text = new unifiedDataChannel.PlainText();
text.textContent = 'this is textContent of text';
let unifiedData = new unifiedDataChannel.UnifiedData(text);

let link = new unifiedDataChannel.Hyperlink();
link.url = 'www.XXX.com';
unifiedData.addRecord(link);

let hasPlainText = unifiedData.hasType(uniformTypeDescriptor.UniformDataType.PLAIN_TEXT);
let hasLink = unifiedData.hasType(uniformTypeDescriptor.UniformDataType.HYPERLINK);
```

### getTypes<sup>12+</sup>

getTypes(): Array\<string\>

获取当前统一数据对象所有数据记录的类型。

**系统能力** ：SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                                     | 说明                      |
| ---------------------------------------- |-------------------------|
| Array\<string\> | [UniformDataType](js-apis-data-uniformTypeDescriptor.md#uniformdatatype)类型的数组，表示当前统一数据对象所有数据记录对应的数据类型。 |

**示例：**

```ts
import { unifiedDataChannel } from '@kit.ArkData';

let text = new unifiedDataChannel.PlainText();
text.textContent = 'this is textContent of text';
let unifiedData = new unifiedDataChannel.UnifiedData(text);

let link = new unifiedDataChannel.Hyperlink();
link.url = 'www.XXX.com';
unifiedData.addRecord(link);

let types = unifiedData.getTypes();
```

## UnifiedRecord

对UDMF支持的数据内容的抽象定义，称为数据记录。一个统一数据对象内包含一条或多条数据记录，例如一条文本记录、一条图片记录、一条HTML记录等。

### constructor<sup>12+</sup>

constructor()

用于创建数据记录。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**示例：**

```ts
let unifiedRecord = new unifiedDataChannel.UnifiedRecord();
```

### constructor<sup>12+</sup>

constructor(type: string, value: ValueType)

用于创建指定类型和值的数据记录。<br />当参数value为[image.PixelMap](js-apis-image.md#pixelmap7)类型时，参数type必须对应为[UniformDataType](js-apis-data-uniformTypeDescriptor.md#uniformdatatype)中OPENHARMONY_PIXEL_MAP的值;<br />

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明                                      |
| ------ | ------------------------------- | ---- |-----------------------------------------|
| type | string | 是   | 要创建的数据记录的类型。 |
| value | [ValueType](#valuetype12) | 是   | 要创建的数据记录的值。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types; 3. Parameter verification failed.  |

**示例：**

```ts
import { image } from '@kit.ImageKit';
import { uniformDataStruct, uniformTypeDescriptor, unifiedDataChannel } from '@kit.ArkData';
import { Want } from '@kit.AbilityKit';

let text = new unifiedDataChannel.UnifiedRecord(uniformTypeDescriptor.UniformDataType.PLAIN_TEXT, 'this is value of text');
let link = new unifiedDataChannel.UnifiedRecord(uniformTypeDescriptor.UniformDataType.HYPERLINK, 'www.XXX.com');
let object: Want = {
  bundleName: 'com.example.myapplication',
  abilityName: 'entryAbility',
};

const color = new ArrayBuffer(96);
let opts: image.InitializationOptions = { editable: true, pixelFormat: 3, size: { height: 4, width: 6 } };
let pixelMap = image.createPixelMapSync(color, opts);
let pixelMapRecord = new unifiedDataChannel.UnifiedRecord(uniformTypeDescriptor.UniformDataType.OPENHARMONY_PIXEL_MAP, pixelMap);

let hyperlinkDetails : Record<string, string> = {
  'attr1': 'value1',
  'attr2': 'value2',
}
let hyperlink : uniformDataStruct.Hyperlink = {
  uniformDataType:'general.hyperlink',
  url : 'www.XXX.com',
  description : 'This is the description of this hyperlink',
  details : hyperlinkDetails,
}
let hyperlinkRecord = new unifiedDataChannel.UnifiedRecord(uniformTypeDescriptor.UniformDataType.HYPERLINK, hyperlink);
```

### getType

getType(): string

获取当前数据记录的类型。由于从统一数据对象中调用[getRecords](#getrecords)所取出的数据是UnifiedRecord对象，因此需要通过本接口查询此记录的具体类型，再将该UnifiedRecord对象转换为其子类，调用子类接口。

**系统能力** ：SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                                                   |
| ------ |------------------------------------------------------|
| string | 当前数据记录对应的具体数据类型，见[UniformDataType](js-apis-data-uniformTypeDescriptor.md#uniformdatatype)。|

**示例：**

```ts
import { uniformTypeDescriptor, unifiedDataChannel } from '@kit.ArkData';

let text = new unifiedDataChannel.PlainText();
text.textContent = 'this is textContent of text';
let unifiedData = new unifiedDataChannel.UnifiedData(text);

let records = unifiedData.getRecords();
if (records[0].getType() == uniformTypeDescriptor.UniformDataType.PLAIN_TEXT) {
  let plainText = records[0] as unifiedDataChannel.PlainText;
  console.info(`textContent: ${plainText.textContent}`);
}
```

### getValue<sup>12+</sup>

getValue(): ValueType

获取当前数据记录的值。

**系统能力** ：SystemCapability.DistributedDataManager.UDMF.Core

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                                                   |
| ------ |------------------------------------------------------|
| [ValueType](#valuetype12) | 当前数据记录对应的值。 |

**示例：**

```ts
import { uniformDataStruct, uniformTypeDescriptor, unifiedDataChannel } from '@kit.ArkData';

let text = new unifiedDataChannel.UnifiedRecord(uniformTypeDescriptor.UniformDataType.PLAIN_TEXT, 'this is value of text');
let value = text.getValue();

let hyperlinkDetails : Record<string, string> = {
  'attr1': 'value1',
  'attr2': 'value2',
}
let hyperlink : uniformDataStruct.Hyperlink = {
  uniformDataType:'general.hyperlink',
  url : 'www.XXX.com',
  description : 'This is the description of this hyperlink',
  details : hyperlinkDetails,
}
let hyperlinkRecord = new unifiedDataChannel.UnifiedRecord(uniformTypeDescriptor.UniformDataType.HYPERLINK, hyperlink);
let hyperlinkValue = hyperlinkRecord.getValue();
```

## Text

文本类型数据，是[UnifiedRecord](#unifiedrecord)的子类，也是文本类型数据的基类，用于描述文本类数据，推荐开发者优先使用Text的子类描述数据，如[PlainText](#plaintext)、[Hyperlink](#hyperlink)、[HTML](#html)等具体子类。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| details | Record<string, string> | 否 | 是 | 是一个字典类型对象，key和value都是string类型，用于描述文本内容。例如，可生成一个details内容为<br />{<br />"title":"标题",<br />"content":"内容"<br />}<br />的数据对象，用于描述一篇文章。非必填字段，默认值为空字典对象。 | 支持 | 支持 |

**示例：**

```ts
let text = new unifiedDataChannel.Text();
text.details = {
  title: 'MyTitle',
  content: 'this is content',
};
let unifiedData = new unifiedDataChannel.UnifiedData(text);
```

## PlainText

纯文本类型数据，是[Text](#text)的子类，用于描述纯文本类数据。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| textContent | string | 否 | 否 | 纯文本内容。                | 支持              | 支持              |
| abstract    | string | 否 | 是 | 纯文本摘要，非必填字段，默认值为空字符串。 | 支持 | 支持 |

**示例：**

```ts
let text = new unifiedDataChannel.PlainText();
text.textContent = 'this is textContent';
text.abstract = 'this is abstract';
```

## Hyperlink

超链接类型数据，是[Text](#text)的子类，用于描述超链接类型数据。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| url         | string | 否 | 否 | 链接url。       | 支持     | 支持     |
| description | string | 否 | 是 | 链接内容描述，非必填字段，默认值为空字符串。 | 支持 | 支持 |

**示例：**

```ts
let link = new unifiedDataChannel.Hyperlink();
link.url = 'www.XXX.com';
link.description = 'this is description';
```

## HTML

HTML类型数据，是[Text](#text)的子类，用于描述超文本标记语言数据。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| htmlContent  | string | 否 | 否 | html格式内容。             | 支持           | 支持           |
| plainContent | string | 否 | 是 | 去除html标签后的纯文本内容，非必填字段，默认值为空字符串。 | 支持 | 支持 |

**示例：**

```ts
let html = new unifiedDataChannel.HTML();
html.htmlContent = '<div><p>标题</p></div>';
html.plainContent = 'this is plainContent';
```

## File

File类型数据，是[UnifiedRecord](#unifiedrecord)的子类，也是文件类型数据的基类，用于描述文件类型数据，推荐开发者优先使用File的子类描述数据，如[Image](#image)、[Video](#video)、[Folder](#folder)等具体子类。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| details | Record<string, string> | 否 | 是 | 是一个字典类型对象，key和value都是string类型，用于描述文件相关信息。例如，可生成一个details内容为<br />{<br />"name":"文件名",<br />"type":"文件类型"<br />}<br />的数据对象，用于描述一个文件。非必填字段，默认值为空字典对象。 | 支持 | 支持 |
| uri     | string                    | 否 | 否 | 文件数据uri。                                                                                                                                             | 支持                                                                                                                                           | 支持                                                                                                                                           |

**示例：**

```ts
let file = new unifiedDataChannel.File();
file.details = {
    name: 'test',
    type: 'txt',
};
file.uri = 'schema://com.samples.test/files/test.txt';
```

## Image

图片类型数据，是[File](#file)的子类，用于描述图片文件。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| imageUri | string | 否 | 否 | 图片数据uri。 | 支持 | 支持 |

**示例：**

```ts
let image = new unifiedDataChannel.Image();
image.imageUri = 'schema://com.samples.test/files/test.jpg';
```

## Video

视频类型数据，是[File](#file)的子类，用于描述视频文件。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| videoUri | string | 否 | 否 | 视频数据uri。 | 支持 | 支持 |

**示例：**

```ts
let video = new unifiedDataChannel.Video();
video.videoUri = 'schema://com.samples.test/files/test.mp4';
```

## Audio

音频类型数据，是[File](#file)的子类，用于描述音频文件。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| audioUri | string | 否 | 否 | 音频数据uri。 | 支持 | 支持 |

**示例：**

```ts
let audio = new unifiedDataChannel.Audio();
audio.audioUri = 'schema://com.samples.test/files/test.mp3';
```

## Folder

文件夹类型数据，是[File](#file)的子类，用于描述文件夹。

**系统能力**：SystemCapability.DistributedDataManager.UDMF.Core

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| folderUri | string | 否 | 否 | 文件夹uri。 | 支持 | 支持 |

**示例：**

```ts
let folder = new unifiedDataChannel.Folder();
folder.folderUri = 'schema://com.samples.test/files/folder/';
```
