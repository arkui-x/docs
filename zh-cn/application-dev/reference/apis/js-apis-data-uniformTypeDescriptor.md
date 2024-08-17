# @ohos.data.uniformTypeDescriptor (标准化数据定义与描述)

本模块对标准化数据类型进行了抽象定义与描述。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import uniformTypeDescriptor from '@ohos.data.uniformTypeDescriptor';
```

## UniformDataType

标准化数据类型的枚举定义。标准化数据类型之间存在归属关系，例如JPEG图片类型归属于IMAGE类型。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

| 名称                         | 值                            | 说明                                 | Android平台                        | iOS平台                   |
|----------------------------|------------------------------|------------------------------------|------------------------------------|------------------------------------|
| TEXT                       | 'general.text'                   | 所有文本的基类型，归属类型为OBJECT。                          | 支持                        | 支持                        |
| PLAIN_TEXT                | 'general.plain-text'             | 未指定编码的文本类型，没有标识符，归属类型为TEXT。        | 支持      | 支持      |
| HTML                  | 'general.html'                   | HTML文本类型，归属类型为TEXT。               | 支持             | 支持             |
| HYPERLINK         | 'general.hyperlink'              | 超链接类型，归属类型为TEXT。                  | 支持                | 支持                |
| IMAGE        | 'general.image'          | 所有图片的基类型，归属类型为MEDIA。              | 支持            | 支持            |
| VIDEO       | 'general.video'           | 所有视频的基类型，归属类型为MEDIA。              | 支持            | 支持            |
| AUDIO       | 'general.audio'          | 所有音频的基类型，归属类型为MEDIA。              | 支持            | 支持            |
| FILE                       | 'general.file'                 | 所有文件的基类型，归属类型为ENTITY。                          | 支持                        | 支持                        |
| FOLDER        | 'general.folder'           | 所有文件夹的基类型，归属类型为DIRECTORY。                         | 支持                       | 支持                       |
| OPENHARMONY_PIXEL_MAP                        | 'openharmony.pixel-map'                  | 系统定义的像素图类型，归属类型为IMAGE。             | 支持           | 支持           |


