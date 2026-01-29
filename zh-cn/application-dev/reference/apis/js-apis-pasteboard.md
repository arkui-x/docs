# @ohos.pasteboard (剪贴板)
本模块提供管理系统剪贴板的能力，支持系统复制、粘贴功能。系统剪贴板支持对文本、HTML、URI、Want、PixelMap等内容的操作。

> **说明：**
>
> 本模块首批接口从API version 24开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { pasteboard } from '@kit.BasicServicesKit';
```

## 常量

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

| 名称 | 类型 | 值 | 说明 | Android平台 | iOS平台 |
| -------- | -------- |--------------|------------------------| -------- | -------- |
| MIMETYPE_TEXT_HTML | string | 'text/html'  | HTML内容的MIME类型定义。 | 支持 | 支持 |
| MIMETYPE_TEXT_WANT  | string | 'text/want'  | Want内容的MIME类型定义。 | 支持 | 支持 |
| MIMETYPE_TEXT_PLAIN | string | 'text/plain' | 纯文本内容的MIME类型定义。 | 支持 | 支持 |
| MIMETYPE_TEXT_URI | string | 'text/uri'   | URI内容的MIME类型定义。 | 支持 | 支持 |
| MIMETYPE_PIXELMAP | string | 'pixelMap'   | PixelMap内容的MIME类型定义。 | 不支持 | 支持 |

## ValueType

type ValueType = string | image.PixelMap | Want | ArrayBuffer

用于表示允许的数据字段类型。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

| 类型 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- |
| string | 表示string的类型。 | 支持 | 支持 |
| image.PixelMap | 表示[image.PixelMap](js-apis-image.md#pixelmap7)的类型。 | 不支持 | 支持 |
| Want | 表示[Want](js-apis-app-ability-want.md)的类型。**注意** ：剪贴板仅作为数据传输媒介，仅负责完整传递Want内的字段数据，不解析、不执行Want的跳转/启动能力；Want数据的跨平台兼容性限制（字段支持范围、数据格式）请参考[Want专属文档](js-apis-app-ability-want.md)。 | 支持 | 支持 |
| ArrayBuffer | 表示ArrayBuffer的类型。 | 不支持 | 支持 |

## pasteboard.createData

createData(mimeType: string, value: ValueType): PasteData

构建一个指定类型的剪贴板内容对象。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明  |
| -------- | -------- | -------- |-------------------------|
| mimeType | string | 是 | 剪贴板数据对应的MIME类型，可以是[常量](#常量)中已定义的类型，包括HTML类型，WANT类型，纯文本类型，URI类型，PIXELMAP类型；也可以是自定义的MIME类型，开发者可自定义此参数值, mimeType长度不能超过1024字节。**注意** ：Android平台不支持PIXELMAP类型和自定义MIME类型。 |
| value | [ValueType](#valuetype) | 是 | 自定义数据内容。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [PasteData](#pastedata) |  剪贴板内容对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types; 3. Parameter verification failed. |

**示例：**

  ```ts
  let dataText = 'hello';
  let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, dataText);
  ```

## pasteboard.createData

createData(data: Record&lt;string, ValueType&gt;): PasteData

构建一个包含多个类型数据的剪贴板内容对象。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| ------ | ----------------- | ---- | ------------------------ |
| data   | Record&lt;string, [ValueType](#valuetype)&gt; | 是   | Record的key为剪贴板数据对应的MIME类型。可以是[常量](#常量)中已定义的类型，包括HTML类型，WANT类型，纯文本类型，URI类型，PIXELMAP类型。也可以是自定义的MIME类型，可自定义此参数值，mimeType长度不能超过1024字节。**注意** ：Android平台不支持PIXELMAP类型和自定义的MIME类型。<br/>Record的value为key中指定MIME类型对应的数据。<br/>Record中的首个key-value指定的MIME类型，会作为剪贴板内容对象中首个PasteDataRecord的默认MIME类型，非默认类型的数据在粘贴时只能使用[getData](#getdata)接口读取。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [PasteData](#pastedata) |  剪贴板内容对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types; 3. Parameter verification failed. |

**示例：**

```ts
let record: Record<string, pasteboard.ValueType> = {};
record[pasteboard.MIMETYPE_TEXT_PLAIN] = 'hello';
record[pasteboard.MIMETYPE_TEXT_URI] = 'dataability:///com.example.myapplication1/user.txt';
let pasteData: pasteboard.PasteData = pasteboard.createData(record);
```

## pasteboard.createRecord

createRecord(mimeType: string, value: ValueType): PasteDataRecord

创建一条指定类型的数据内容条目。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明                |
| -------- | -------- | -------- |-------------------|
| mimeType | string | 是 | 剪贴板数据对应的MIME类型，可以是[常量](#常量)中已定义的类型，包括HTML类型，WANT类型，纯文本类型，URI类型，PIXELMAP类型；也可以是自定义的MIME类型，开发者可自定义此参数值，mimeType长度不能超过1024字节。**注意** ：Android平台不支持PIXELMAP类型和自定义的MIME类型。 |
| value | [ValueType](#valuetype) | 是 | 指定类型对应的数据内容。          |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [PasteDataRecord](#pastedatarecord) | 一条新建的指定类型的数据内容条目。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types;  3. Parameter verification failed. |

**示例：**

  ```ts
  let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
  let record: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_URI, 'dataability:///com.example.myapplication1/user.txt');
  pasteData.replaceRecord(0, record);
  ```

## pasteboard.getSystemPasteboard

getSystemPasteboard(): SystemPasteboard

获取系统剪贴板对象。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [SystemPasteboard](#systempasteboard) | 系统剪贴板对象。 |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
```


## PasteDataProperty

定义剪贴板中所有内容条目的属性，包含时间戳、数据类型以及一些附加数据等，该属性必须通过[setProperty](#setproperty)方法，才能设置到剪贴板中。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- |-------------------------------|-------------------------------|-------------------------------|
| additions | {[key:string]:object} | 否 | 否 | 设置额外附加属性数据。不支持动态追加属性，只能通过重新赋值的方式修改附加属性，具体见相关示例setProperty， 默认为空。**注意** ：additions中不支持数组中嵌套数组及Object对象。 | 支持 | 支持 |
| mimeTypes | Array&lt;string&gt; | 是 | 否 | 剪贴板内容条目的数据类型。 | 支持 | 支持 |
| tag | string | 否 | 否 | 用户自定义标签，默认为空。 | 支持 | 支持 |
| timestamp | number | 是 | 否 | 剪贴板数据的写入时间戳（单位：ms）。**注意** ：Android平台为UNIX时间戳。 | 支持 | 支持 |

## PasteDataRecord

对于剪贴板中内容记录的抽象定义，称之为条目。剪贴板内容部分由一个或者多个条目构成，例如一条文本内容、一份HTML、一个URI或者一个Want。

### 属性

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| htmlText | string | 否 | 否 | HTML内容。**注意** ：在PasteDataRecord中若只包含htmlText数据内容时，在Android平台会自动生成一个htmlText数据的纯文本内容作为plainText数据。 | 支持 | 支持 |
| want | [Want](js-apis-app-ability-want.md) | 否 | 否 | Want内容。 | 支持 | 支持 |
| mimeType | string | 否 | 否 | 默认数据类型。 | 不支持 | 支持 |
| plainText | string | 否 | 否 | 纯文本内容。 | 支持 | 支持 |
| uri | string | 否 | 否 | URI内容。**注意**  ：Android平台需考虑设备限制，如不支持“file://”等开头的URI写入系统剪切板。 | 支持 | 支持 |
| pixelMap | [image.PixelMap](js-apis-image.md#pixelmap7) | 否 | 否 | PixelMap内容。**注意** ： pixelMap跟原生APP交互只支持JPEG和PNG格式。 | 不支持 | 支持 |
| data | {[mimeType:&nbsp;string]:&nbsp;ArrayBuffer} | 否 | 否 | 自定义数据内容。 | 不支持 | 支持 |

### toPlainText

toPlainText(): string

将一个PasteDataRecord中的html、plain、uri内容强制转换为文本内容。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| string | 纯文本内容。 |

**示例：**

```ts
let record: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_HTML, '<html>hello</html>');
let text: string = record.toPlainText();
console.info(`Succeeded in converting to text. Text: ${text}`);
```

### addEntry

addEntry(type: string, value: ValueType): void

往一个PasteDataRecord中额外添加一种样式的自定义数据。此方式添加的MIME类型都不是Record的默认类型，粘贴时只能使用[getData](#getdata)接口读取对应数据。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型 | 必填 | 说明                |
|-------| -------- | -------- |-------------------|
| type  | string | 是 | 剪贴板数据对应的MIME类型，可以是[常量](#常量)中已定义的类型，包括HTML类型，WANT类型，纯文本类型，URI类型，PIXELMAP类型；也可以是自定义的MIME类型，开发者可自定义此参数值，mimeType长度不能超过1024字节。**注意**  ：Android平台不支持PIXELMAP类型和自定义的MIME类型。 |
| value | [ValueType](#valuetype) | 是 | 自定义数据内容。          |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types; 3. Parameter verification failed. |

**示例：**

```ts
let html = "<!DOCTYPE html>\n" + "<html>\n" + "<head>\n" + "<meta charset=\"utf-8\">\n" + "<title>HTML-PASTEBOARD_HTML</title>\n" + "</head>\n" + "<body>\n" + "    <h1>HEAD</h1>\n" + "    <p></p>\n" + "</body>\n" + "</html>";
let record: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_URI, 'dataability:///com.example.myapplication1/user.txt');
record.addEntry(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
record.addEntry(pasteboard.MIMETYPE_TEXT_HTML, html);
```

### getValidTypes

getValidTypes(types: Array&lt;string&gt;): Array&lt;string&gt;

根据传入的MIME类型，返回传入的MIME类型和剪贴板中数据的MIME类型的交集。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型 | 必填 | 说明             |
|-------| -------- | -------- |----------------|
| types | Array&lt;string&gt; | 是 | MIME类型列表。 |

**返回值：**

| 类型 | 说明                                   |
| -------- |--------------------------------------|
| Array&lt;string&gt; | 传入的MIME类型和剪贴板中数据的MIME类型的交集。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types; 3. Parameter verification failed. |

**示例：**

```ts
let html = "<!DOCTYPE html>\n" + "<html>\n" + "<head>\n" + "<meta charset=\"utf-8\">\n" + "<title>HTML-PASTEBOARD_HTML</title>\n" + "</head>\n" + "<body>\n" + "    <h1>HEAD</h1>\n" + "    <p></p>\n" + "</body>\n" + "</html>";
let record: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_URI, 'dataability:///com.example.myapplication1/user.txt');
record.addEntry(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
record.addEntry(pasteboard.MIMETYPE_TEXT_HTML, html);
let types: string[] = record.getValidTypes([
    pasteboard.MIMETYPE_TEXT_PLAIN,
    pasteboard.MIMETYPE_TEXT_HTML,
    pasteboard.MIMETYPE_TEXT_URI,
    pasteboard.MIMETYPE_TEXT_WANT
]);
```

### getData

getData(type: string): Promise&lt;ValueType&gt;

从PasteDataRecord中获取指定MIME类型的自定义数据。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型     |必填 | 说明       |
|------|--------|-------- |----------|
| type | string |是 | MIME类型。 |

**返回值：**

| 类型   | 说明    |
| ---------- | --------------- |
| Promise&lt;[ValueType](#valuetype)&gt; | Promise对象，返回PasteDataRecord中指定MIME类型的自定义数据。<br/>PasteDataRecord中包含多个MIME类型数据时，非PasteDataRecord的默认MIME类型的数据只能通过本接口获取。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types; 3. Parameter verification failed. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let html = "<!DOCTYPE html>\n" + "<html>\n" + "<head>\n" + "<meta charset=\"utf-8\">\n" + "<title>HTML-PASTEBOARD_HTML</title>\n" + "</head>\n" + "<body>\n" + "    <h1>HEAD</h1>\n" + "    <p></p>\n" + "</body>\n" + "</html>";
let record: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_URI, 'dataability:///com.example.myapplication1/user.txt');
record.addEntry(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
record.addEntry(pasteboard.MIMETYPE_TEXT_HTML, html);
record.getData(pasteboard.MIMETYPE_TEXT_PLAIN).then((value: pasteboard.ValueType) => {
    let textPlainContent = value as string;
    console.info('Success to get text/plain value. value is: ' + textPlainContent);
}).catch((err: BusinessError) => {
    console.error('Failed to get text/plain value. Cause: ' + err.message);
});
record.getData(pasteboard.MIMETYPE_TEXT_URI).then((value: pasteboard.ValueType) => {
    let uri = value as string;
    console.info('Success to get text/uri value. value is: ' + uri);
}).catch((err: BusinessError) => {
    console.error('Failed to get text/uri value. Cause: ' + err.message);
});
```

## PasteData

剪贴板内容对象。剪贴板内容包含一个或者多个内容条目（[PasteDataRecord](#pastedatarecord)）以及属性描述对象（[PasteDataProperty](#pastedataproperty)）。

在调用PasteData的接口前，需要先通过[createData()](#pasteboardcreatedata)或[getData()](#getdata)获取一个PasteData对象。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

> **说明：** Android 平台无统一的系统级别 PasteData 数据大小限制，但不同厂商的定制化系统会对 PasteData 的总数据量、单条 PasteDataRecord 数据量设置上限（通常一个PasteData的数据不建议超过1MB），超出上限时，数据可能被系统截断或整个数据无法写入剪贴板。

### getPrimaryText

getPrimaryText(): string

获取第一条纯文本内容。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| string | 纯文本内容。剪贴板内容对象中没有纯文本内容时，默认返回为undefined。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getData().then((pasteData: pasteboard.PasteData) => {
    let text: string = pasteData.getPrimaryText();
}).catch((err: BusinessError) => {
    console.error('Failed to get PasteData. Cause: ' + err.message);
});
```

### getPrimaryHtml

getPrimaryHtml(): string

获取第一条的HTML内容。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| string | HTML内容。剪贴板内容对象中没有HTML内容时，默认返回为undefined。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getData().then((pasteData: pasteboard.PasteData) => {
    let htmlText: string = pasteData.getPrimaryHtml();
}).catch((err: BusinessError) => {
    console.error('Failed to get PasteData. Cause: ' + err.message);
});
```

### getPrimaryWant

getPrimaryWant(): Want

获取第一条的Want对象内容。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [Want](js-apis-app-ability-want.md) | Want对象内容。剪贴板内容对象中没有Want内容时，默认返回为undefined。 |

**示例：**

```ts
import { Want } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getData().then((pasteData: pasteboard.PasteData) => {
    let want: Want = pasteData.getPrimaryWant();
}).catch((err: BusinessError) => {
    console.error('Failed to get PasteData. Cause: ' + err.message);
});
```

### getPrimaryUri

getPrimaryUri(): string

获取第一条的URI内容。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| string | URI内容。剪贴板内容对象中没有URI内容时，默认返回为undefined。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getData().then((pasteData: pasteboard.PasteData) => {
    let uri: string = pasteData.getPrimaryUri();
}).catch((err: BusinessError) => {
    console.error('Failed to get PasteData. Cause: ' + err.message);
});
```

### getPrimaryPixelMap

getPrimaryPixelMap(): image.PixelMap

获取第一条的PixelMap内容。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** iOS

**返回值：**

| 类型 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- |
| [image.PixelMap](js-apis-image.md#pixelmap7) | PixelMap内容。剪贴板内容对象中没有PixelMap内容时，默认返回为undefined。 | 不支持 | 支持 |

**示例：**

```ts
import { image } from '@kit.ImageKit';

let buffer = new ArrayBuffer(128);
let realSize: image.Size = { height: 3, width: 5 };
let opt: image.InitializationOptions = {
    size: realSize,
    pixelFormat: 3,
    editable: true,
    alphaType: 1,
    scaleMode: 1
};
image.createPixelMap(buffer, opt).then((pixelMap: image.PixelMap) => {
    let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_PIXELMAP, pixelMap);
    let PixelMap: image.PixelMap = pasteData.getPrimaryPixelMap();
});
```

### addRecord

addRecord(record: PasteDataRecord): void

向当前剪贴板内容中添加一条条目，同时也会将条目类型添加到[PasteDataProperty](#pastedataproperty)的mimeTypes中。入参均不能为空，否则添加失败。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| record | [PasteDataRecord](#pastedatarecord) | 是 | 待添加的条目。 |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_URI, 'dataability:///com.example.myapplication1/user.txt');
let textRecord: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let html: string = "<!DOCTYPE html>\n" + "<html>\n" + "<head>\n" + "<meta charset=\"utf-8\">\n" + "<title>HTML-PASTEBOARD_HTML</title>\n" + "</head>\n" + "<body>\n" + "    <h1>HEAD</h1>\n" + "    <p></p>\n" + "</body>\n" + "</html>";
let htmlRecord: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_HTML, html);
pasteData.addRecord(textRecord);
pasteData.addRecord(htmlRecord);
```
### addRecord

addRecord(mimeType: string, value: ValueType): void

向当前剪贴板内容中添加一条数据内容条目，同时也会将数据类型添加到[PasteDataProperty](#pastedataproperty)的mimeTypes中。入参均不能为空，否则添加失败。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| mimeType | string | 是 | 数据的MIME类型， 其长度不能超过1024字节。**注意** ：Android不支持PIXELMAP类型和自定义的MIME类型。 |
| value | [ValueType](#valuetype) | 是 | 数据内容。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types; 3. Parameter verification failed. |

**示例：**

  ```ts
  let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_URI, 'dataability:///com.example.myapplication1/user.txt');
  let text: string = "hello";
  pasteData.addRecord(pasteboard.MIMETYPE_TEXT_PLAIN, text);
  ```

### getMimeTypes

getMimeTypes(): Array&lt;string&gt;

获取剪贴板中内容条目的数据类型，非重复的类型列表，接口调用异常时返回undefined。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Array&lt;string&gt; | 剪贴板内容条目的数据类型，非重复的类型列表。 |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let types: string[] = pasteData.getMimeTypes();
```

### getPrimaryMimeType

getPrimaryMimeType(): string

获取剪贴板内容中第一个条目的数据类型。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** iOS

**返回值：**

| 类型 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- |
| string | 第一个条目的数据类型。 | 不支持 | 支持 |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let type: string = pasteData.getPrimaryMimeType();
```

### getProperty

getProperty(): PasteDataProperty

获取剪贴板内容的属性描述对象。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [PasteDataProperty](#pastedataproperty) | 属性描述对象。 |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let property: pasteboard.PasteDataProperty = pasteData.getProperty();
```

### setProperty

setProperty(property: PasteDataProperty): void

设置剪贴板内容的属性描述对象[PasteDataProperty](#pastedataproperty)。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| property | [PasteDataProperty](#pastedataproperty) | 是 | 属性描述对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
type AdditionType = Record<string, Record<string, Object>>;

let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_HTML, 'application/xml');
let prop: pasteboard.PasteDataProperty = pasteData.getProperty();
// 需要注意，不支持对addition进行追加属性的操作，只能通过重新赋值的方式达到追加属性的目的。
prop.additions = { 'TestOne': { 'Test': 123 }, 'TestTwo': { 'Test': 'additions' } } as AdditionType;
prop.tag = 'TestTag';
pasteData.setProperty(prop);
```
### getRecord

getRecord(index: number): PasteDataRecord

获取剪贴板内容中指定下标的条目。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| index | number | 是 | 指定条目的下标。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [PasteDataRecord](#pastedatarecord) | 指定下标的条目。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12900001 | The index is out of the record. |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let record: pasteboard.PasteDataRecord = pasteData.getRecord(0);
```

### getRecordCount

getRecordCount(): number

获取剪贴板内容中条目的个数。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| number | 条目的个数。 |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let count: number = pasteData.getRecordCount();
```

### getTag

getTag(): string

获取剪贴板内容中用户自定义的标签内容，如果没有设置用户自定义的标签内容将返回空。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| string | 返回用户自定义的标签内容，如果没有设置用户自定义的标签内容，将返回空。 |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let tag: string = pasteData.getTag();
```

### hasType

hasType(mimeType: string): boolean

检查剪贴板内容中是否有指定的MIME数据类型。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| mimeType | string | 是 | 待查询的数据类型。可以是[常量](#常量)中已定义的类型，包括HTML类型，WANT类型，纯文本类型，URI类型，PIXELMAP类型；也可以是自定义的MIME类型。**注意** ：Android平台不支持PIXELMAP类型和自定义的MIME类型。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| boolean | 有指定的数据类型返回true，否则返回false。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let hasType: boolean = pasteData.hasType(pasteboard.MIMETYPE_TEXT_PLAIN);
```

### removeRecord

removeRecord(index: number): void

移除剪贴板内容中指定下标的条目。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**说明：** Android 平台上一个 PasteData 需至少包含一个 PasteDataRecord 条目，若通过 removeRecord 删除某个 PasteData 对象的所有 PasteDataRecord 条目（即 PasteData 内无任何内容条目）,将无法将该 PasteData 写入系统剪贴板，若需清空剪贴板，建议使用[clearData](#clearData)接口，而非删除所有 PasteDataRecord 后写入。

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| index | number | 是 | 指定的下标。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12900001 | The index is out of the record. |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
pasteData.removeRecord(0);
```

### replaceRecord

replaceRecord(index: number, record: PasteDataRecord): void

替换剪贴板内容中指定下标的条目。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| index | number | 是 | 指定的下标。 |
| record | [PasteDataRecord](#pastedatarecord) | 是 | 被替换后的条目数据内容。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12900001 | The index is out of the record. |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
let record: pasteboard.PasteDataRecord = pasteboard.createRecord(pasteboard.MIMETYPE_TEXT_URI, 'dataability:///com.example.myapplication1/user.txt');
pasteData.replaceRecord(0, record);
```

## SystemPasteboard

系统剪贴板对象。

在调用SystemPasteboard的接口前，需要先通过[getSystemPasteboard](#pasteboardgetsystempasteboard)获取系统剪贴板。

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
```

### on('update')

on(type: 'update', callback: () =&gt;void): void

订阅系统剪贴板内容变化事件，当系统剪贴板中内容变化时触发用户程序的回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| type | string | 是 | 取值为'update'，表示系统剪贴板内容变化事件。 |
| callback | function | 是 | 剪贴板中内容变化时触发的用户程序的回调。 |

**注**：为避免内存泄露，在监听页面退出时，务必调用 [off](#offupdate) 接口注销监听，否则将导致内存无法释放。

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
let listener = () => {
    console.info('The system pasteboard has changed.');
};
systemPasteboard.on('update', listener);
```

### off('update')

off(type: 'update', callback?: () =&gt;void): void

取消订阅系统剪贴板内容变化事件。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- |------------------------|
| type | string | 是 | 取值为'update'，表示系统剪贴板内容变化事件。                              |
| callback | function | 否 | 剪贴板中内容变化时触发的用户程序的回调。如果此参数未填，表明清除本应用的所有监听回调，否则表示清除指定监听回调。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
let listener = () => {
    console.info('The system pasteboard has changed.');
};
systemPasteboard.off('update', listener);
```

### clearData

clearData(callback: AsyncCallback&lt;void&gt;): void

清空系统剪贴板内容，使用callback异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**说明：** Android API level 28（对应 Android 9.0）以下版本，无法彻底清空系统剪贴板内容：调用此接口后，系统不会移除剪贴板中的所有条目，而是会写入一条“内容为空字符串的纯文本的剪切板数据”将原有数据完全覆盖（即剪贴板仍保留 1 条空文本条目，而非完全空白）；

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;void&gt; | 是 | 回调函数。当成功清空时，err为undefined；否则为错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.clearData((err, data) => {
    if (err) {
        console.error(`Failed to clear the pasteboard. Cause: ${err.message}`);
        return;
    }
    console.info('Succeeded in clearing the pasteboard.');
});
```

### clearData

clearData(): Promise&lt;void&gt;

清空系统剪贴板内容，使用Promise异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

> **说明：** Android API level 28（对应 Android 9.0）以下版本，无法彻底清空系统剪贴板内容：调用此接口后，系统不会移除剪贴板中的所有条目，而是会写入一条"内容为空字符串的纯文本的剪切板数据”将原有数据全部覆盖（即剪贴板仍保留 1 条空文本条目，而非完全空白）。

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.clearData().then((data: void) => {
    console.info('Succeeded in clearing the pasteboard.');
}).catch((err: BusinessError) => {
    console.error(`Failed to clear the pasteboard. Cause: ${err.message}`);
});
```

### setData

setData(data: PasteData, callback: AsyncCallback&lt;void&gt;): void

将数据写入系统剪贴板，使用callback异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| data | [PasteData](#pastedata) | 是 | PasteData对象。 |
| callback | AsyncCallback&lt;void> | 是 | 回调函数。当写入成功，err为undefined，否则为错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 27787277 | Another copy or paste operation is in progress. |
| 27787278 | Replication is prohibited. |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'content');
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.setData(pasteData, (err, data) => {
    if (err) {
        console.error('Failed to set PasteData. Cause: ' + err.message);
        return;
    }
    console.info('Succeeded in setting PasteData.');
});
```

### setData

setData(data: PasteData): Promise&lt;void&gt;

将数据写入系统剪贴板，使用Promise异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| data | [PasteData](#pastedata) | 是 | PasteData对象。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 27787277 | Another copy or paste operation is in progress. |
| 27787278 | Replication is prohibited. |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'content');
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.setData(pasteData).then((data: void) => {
    console.info('Succeeded in setting PasteData.');
}).catch((err: BusinessError) => {
    console.error('Failed to set PasteData. Cause: ' + err.message);
});
```

### getData

getData(callback: AsyncCallback&lt;PasteData&gt;): void

读取系统剪贴板内容，使用callback异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;[PasteData](#pastedata)&gt; | 是 | 回调函数。当读取成功，err为undefined，data为返回的系统剪贴板数据；否则返回错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 27787277 | Another copy or paste operation is in progress. |
| 201      | Permission verification failed. The application does not have the permission required to call the API. |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getData((err: BusinessError, pasteData: pasteboard.PasteData) => {
    if (err) {
        console.error('Failed to get PasteData. Cause: ' + err.message);
        return;
    }
    let text: string = pasteData.getPrimaryText();
});
```

### getData

getData(): Promise&lt;PasteData&gt;

读取系统剪贴板内容，使用Promise异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;[PasteData](#pastedata)&gt; | Promise对象，返回系统剪贴板数据。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 27787277 | Another copy or paste operation is in progress. |
| 201      | Permission verification failed. The application does not have the permission required to call the API. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getData().then((pasteData: pasteboard.PasteData) => {
    let text: string = pasteData.getPrimaryText();
}).catch((err: BusinessError) => {
    console.error('Failed to get PasteData. Cause: ' + err.message);
});
```

### hasData

hasData(callback:  AsyncCallback&lt;boolean&gt;): void

判断系统剪贴板中是否有内容，使用callback异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;boolean&gt; | 是 | 返回true表示系统剪贴板中有内容，返回false表示系统剪贴板中没有内容。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.hasData((err: BusinessError, data: boolean) => {
    if (err) {
        console.error(`Failed to check the PasteData. Cause: ${err.message}`);
        return;
    }
    console.info(`Succeeded in checking the PasteData. Data: ${data}`);
});
```

### hasData

hasData(): Promise&lt;boolean&gt;

判断系统剪贴板中是否有内容，使用Promise异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;boolean&gt; | 返回true表示系统剪贴板中有内容，返回false表示系统剪贴板中没有内容。 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.hasData().then((data: boolean) => {
    console.info(`Succeeded in checking the PasteData. Data: ${data}`);
}).catch((err: BusinessError) => {
    console.error(`Failed to check the PasteData. Cause: ${err.message}`);
});
```

### hasDataType

hasDataType(mimeType: string): boolean

检查剪贴板内容中是否有指定类型的数据。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型   | 必填 | 说明               |
| -------- | ------ | ---- | ------------------ |
| mimeType | string | 是   | 数据类型。 |

**返回值：**

| 类型    | 说明                                        |
| ------- | ------------------------------------------- |
| boolean | 有指定类型的数据返回true，否则返回false。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401 | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |
| 12900005 | Excessive processing time for internal data. |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    let result: boolean = systemPasteboard.hasDataType(pasteboard.MIMETYPE_TEXT_PLAIN);
    console.info(`Succeeded in checking the DataType. Result: ${result}`);
} catch (err) {
    console.error('Failed to check the DataType. Cause:' + err.message);
};
```

### clearDataSync

clearDataSync(): void

清空系统剪贴板内容, 此接口为同步接口。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**说明：** Android API level 28（对应 Android 9.0）以下版本，无法彻底清空系统剪贴板内容：调用此接口后，系统不会移除剪贴板中的所有条目，而是会写入一条“内容为空字符串的纯文本的剪切板数据”将原有数据全部覆盖（即剪贴板仍保留 1 条空文本条目，而非完全空白）；

**错误码：**

以下错误码的详细介绍请参见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12900005 | Excessive processing time for internal data. |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    systemPasteboard.clearDataSync();
    console.info('Succeeded in clearing the pasteboard.');
} catch (err) {
    console.error('Failed to clear the pasteboard. Cause:' + err.message);
};
```

### getDataSync

getDataSync(): PasteData

读取系统剪贴板内容, 此接口为同步接口。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型                    | 说明                 |
| ----------------------- | -------------------- |
| [PasteData](#pastedata) | 返回系统剪贴板数据。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12900005 | Excessive processing time for internal data. |
| 201      | Permission verification failed. The application does not have the permission required to call the API. |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    let result: pasteboard.PasteData = systemPasteboard.getDataSync();
    console.info('Succeeded in getting PasteData.');
} catch (err) {
    console.error('Failed to get PasteData. Cause:' + err.message);
};   
```

### setDataSync

setDataSync(data: PasteData): void

将数据写入系统剪贴板, 此接口为同步接口。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                    | 必填 | 说明             |
| ------ | ----------------------- | ---- | ---------------- |
| data   | [PasteData](#pastedata) | 是   | 需要写入剪贴板中的数据。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |
| 12900005 | Excessive processing time for internal data. |

**示例：**

```ts
let pasteData: pasteboard.PasteData = pasteboard.createData(pasteboard.MIMETYPE_TEXT_PLAIN, 'hello');
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    systemPasteboard.setDataSync(pasteData);
    console.info('Succeeded in setting PasteData.');
} catch (err) {
    console.error('Failed to set PasteData. Cause:' + err.message);
};  
```

### hasDataSync

hasDataSync(): boolean

判断系统剪贴板中是否有内容, 此接口为同步接口。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 返回true表示系统剪贴板中有内容，返回false表示系统剪贴板中没有内容。 |

**错误码：**

以下错误码的详细介绍请参见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12900005 | Excessive processing time for internal data. |

**示例：**

```ts
const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    let result: boolean = systemPasteboard.hasDataSync();
    console.info(`Succeeded in checking the PasteData. Result: ${result}`);
} catch (err) {
    console.error('Failed to check the PasteData. Cause:' + err.message);
};    
```

### getUnifiedData

getUnifiedData(): Promise&lt;unifiedDataChannel.UnifiedData&gt;

读取系统剪贴板内容，使用Promise异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;[unifiedDataChannel.UnifiedData](js-apis-data-unifiedDataChannel.md#unifieddata)&gt; | Promise对象，返回系统剪贴板数据。 |

**错误码：**

以下错误码的详细介绍请参见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 201      | Permission verification failed. The application does not have the permission required to call the API. |
| 27787277 | Another copy or paste operation is in progress. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { unifiedDataChannel, uniformDataStruct, uniformTypeDescriptor } from '@kit.ArkData';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getUnifiedData().then((data) => {
    let records: Array<unifiedDataChannel.UnifiedRecord> = data.getRecords();
    for (let j = 0; j < records.length; j++) {
        if (records[j].getType() === uniformTypeDescriptor.UniformDataType.PLAIN_TEXT) {
            let text = records[j].getValue() as uniformDataStruct.PlainText;
            console.info(`${j + 1}.${text.textContent}`);
        }
    }
}).catch((err: BusinessError) => {
    console.error('Failed to get UnifiedData. Cause: ' + err.message);
});
```

### getUnifiedDataSync

getUnifiedDataSync(): unifiedDataChannel.UnifiedData

读取系统剪贴板内容, 此接口为同步接口。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型                                                         | 说明                 |
| ------------------------------------------------------------ | -------------------- |
| [unifiedDataChannel.UnifiedData](js-apis-data-unifiedDataChannel.md#unifieddata) | 返回系统剪贴板数据。 |

**错误码：**

以下错误码的详细介绍请参见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 201      | Permission verification failed. The application does not have the permission required to call the API. |
| 12900005 | Excessive processing time for internal data. |

**示例：**

```ts
import { unifiedDataChannel } from '@kit.ArkData';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    let result: unifiedDataChannel.UnifiedData = systemPasteboard.getUnifiedDataSync();
    console.info('Succeeded in getting UnifiedData.');
} catch (err) {
    console.error('Failed to get UnifiedData. Cause:' + err.message);
};   
```

### setUnifiedData

setUnifiedData(data: unifiedDataChannel.UnifiedData): Promise&lt;void&gt;

将数据写入系统剪贴板，使用Promise异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| data | [unifiedDataChannel.UnifiedData](js-apis-data-unifiedDataChannel.md#unifieddata) | 是 | 需要写入剪贴板中的数据。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;void&gt; | 无返回结果的Promise对象。 |

**错误码：**

以下错误码的详细介绍请参见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |
| 27787277 | Another copy or paste operation is in progress. |
| 27787278 | Replication is prohibited. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { unifiedDataChannel, uniformDataStruct, uniformTypeDescriptor } from '@kit.ArkData';

let plainText : uniformDataStruct.PlainText = {
    uniformDataType: uniformTypeDescriptor.UniformDataType.PLAIN_TEXT,
    textContent : 'PLAINTEXT_CONTENT',
    abstract : 'PLAINTEXT_ABSTRACT',
}
let record = new unifiedDataChannel.UnifiedRecord(uniformTypeDescriptor.UniformDataType.PLAIN_TEXT, plainText);
let data = new unifiedDataChannel.UnifiedData();
data.addRecord(record);

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.setUnifiedData(data).then((data: void) => {
    console.info('Succeeded in setting UnifiedData.');
}).catch((err: BusinessError) => {
    console.error('Failed to setUnifiedData. Cause: ' + err.message);
});
```

### setUnifiedDataSync

setUnifiedDataSync(data: unifiedDataChannel.UnifiedData): void

将数据写入系统剪贴板, 此接口为同步接口。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型        | 必填 | 说明             |
| ------ | ----------- | ---- | ---------------- |
| data   | [unifiedDataChannel.UnifiedData](js-apis-data-unifiedDataChannel.md#unifieddata) | 是   | 需要写入剪贴板中的数据。 |

**错误码：**

以下错误码的详细介绍请参见[剪贴板错误码](../errorcodes/errorcode-pasteboard.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 401      | Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types. |
| 12900005 | Excessive processing time for internal data. |

**示例：**

```ts
import { unifiedDataChannel } from '@kit.ArkData';

let plainTextData = new unifiedDataChannel.UnifiedData();
let plainText = new unifiedDataChannel.PlainText();
plainText.details = {
    Key: 'delayPlaintext',
    Value: 'delayPlaintext',
};
plainText.textContent = 'delayTextContent';
plainText.abstract = 'delayTextContent';
plainTextData.addRecord(plainText);

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    systemPasteboard.setUnifiedDataSync(plainTextData);
    console.info('Succeeded in setting UnifiedData.');
} catch (err) {
    console.error('Failed to set UnifiedData. Cause:' + err.message);
};
```

### getMimeTypes

getMimeTypes(): Promise&lt;Array&lt;string&gt;&gt;

读取剪贴板中存在的MIME类型，使用Promise异步回调。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;Array&lt;string&gt;&gt; | Promise对象，返回读取到的MIME类型。 |

**示例：**

```ts
import { pasteboard, BusinessError } from '@kit.BasicServicesKit'

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
systemPasteboard.getMimeTypes().then((data: Array<String>) => {
    console.info('Succeeded in getting mimeTypes. mimeTypes: ' + data.sort().join(','));
}).catch((err: BusinessError) => {
    console.error('Failed to get mimeTypes. Cause:' + err.message);
});
```

### getChangeCount

getChangeCount(): number

获取剪贴板内容的变化次数。

执行成功时返回剪贴板内容的变化次数，否则返回0。

当剪贴板内容过期或调用[clearDataSync](#cleardatasync)等接口导致剪贴板内容为空时，内容变化次数不会因此改变。

系统重启或剪贴板服务异常重启时，剪贴板内容变化次数重新从0开始计数。对同一内容连续多次复制会被视作多次更改，每次复制均会导致内容变化次数增加。

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** iOS

**说明：** iOS端调用[clearDataSync](#cleardatasync)等接口导致剪贴板内容为空时，内容变化次数也会改变，系统重启或剪贴板服务异常重启时，剪贴板内容变化次数不会重新从0开始计数。

**返回值：**

| 类型 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- |
| number | 返回读取到的剪贴板内容变化次数。 | 不支持 | 支持 |

**示例：**

```ts
import { BusinessError, pasteboard } from '@kit.BasicServicesKit';

const systemPasteboard: pasteboard.SystemPasteboard = pasteboard.getSystemPasteboard();
try {
    let result : number = systemPasteboard.getChangeCount();
    console.info(`Succeeded in getting the ChangeCount. Result: ${result}`);
} catch (err) {
    console.error(`Failed to get the ChangeCount. Cause: ${err.message}`);
};
```
### UpdateCallback 

type UpdateCallback = () => void

表示剪贴板内容变更的回调

**系统能力：** SystemCapability.MiscServices.Pasteboard

**支持平台：** Android、iOS