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

## TypeDescriptor<sup>11+</sup> 

标准化数据类型的描述类，它包含了一些属性和方法用于描述标准化数据类型自身以及和其他标准化数据类型之间的归属与层级关系。

### 属性

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

| 名称    | 类型                    | 只读 | 可选 | 说明                                                       |
| ------- | ----------------------- | ---- | ---- |----------------------------------------------------------|
| typeId<sup>11+</sup>     | string | 是   | 否   | 标准化数据类型的ID（即[UTD列表](#uniformdatatype)中对应的枚举值），也可以是自定义UTD。 |
| belongingToTypes<sup>11+</sup>  | Array\<string>          | 是   | 否   | 标准化数据类型所归属的类型typeId列表。                                   |
| description<sup>11+</sup>     | string                  | 是   | 否   | 标准化数据类型的简要说明。                                            |
| referenceURL<sup>11+</sup>     | string                  | 是   | 否   | 标准化数据类型的参考链接URL，用于描述类型的详细信息。                            |
| iconFile<sup>11+</sup>     | string                  | 是   | 否   | 标准化数据类型的默认图标文件路径，可能为空字符串（即没有默认图标），应用可以自行决定是否使用该默认图标。                                   |
| filenameExtensions<sup>12+</sup>  | Array\<string>          | 是   | 否   | 标准化数据类型所关联的文件名后缀列表。                                   |
| mimeTypes<sup>12+</sup>  | Array\<string>          | 是   | 否   | 标准化数据类型所关联的多用途互联网邮件扩展类型列表。                                   |

### belongsTo<sup>11+</sup> 

belongsTo(type: string): boolean

判断当前标准化数据类型是否归属于指定的标准化数据类型。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| type    | string  | 是    |所指定的标准化数据类型（即[UniformDataType](#uniformdatatype)中对应的枚举值）。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 返回true表示当前的标准化数据类型归属于所指定的标准化数据类型，包括所指定类型与当前类型相同的情况；返回false则表示无归属关系。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try{
    let typeObj : uniformTypeDescriptor.TypeDescriptor = uniformTypeDescriptor.getTypeDescriptor('general.type-script');
    let ret = typeObj.belongsTo('general.source-code');
    if(ret) {
        console.info('type general.type-script belongs to type general.source-code');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`belongsTo throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

### isLowerLevelType<sup>11+</sup> 

isLowerLevelType(type: string): boolean

判断当前标准化数据类型是否是指定标准化数据类型的低层级类型。例如TYPE_SCRIPT为SOURCE_CODE的低层级类型，TYPE_SCRIPT和SOURCE_CODE为TEXT的低层级类型。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| type    | string  | 是    |所指定的标准化数据类型（即[UniformDataType](#uniformdatatype)中对应的枚举值）。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 返回true表示当前的标准化数据类型是所指定标准化数据类型的低层级类型，否则返回false。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try{
    let typeObj : uniformTypeDescriptor.TypeDescriptor = uniformTypeDescriptor.getTypeDescriptor('general.type-script');
    let ret = typeObj.isLowerLevelType('general.source-code');
    if(ret) {
        console.info('type general.type-script is lower level type of type general.source-code');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`isLowerLevelType throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

### isHigherLevelType<sup>11+</sup> 

isHigherLevelType(type: string): boolean

判断当前标准化数据类型是否是指定标准化数据类型的高层级类型。例如SOURCE_CODE为TYPE_SCRIPT的高层级类型，TEXT为SOURCE_CODE和TYPE_SCRIPT的高层级类型。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| type    | string  | 是    |所指定的标准化数据类型（即[UniformDataType](#uniformdatatype)中对应的枚举值）。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 返回true表示当前的标准化数据类型是所指定标准化数据类型的高层级类型，否则返回false。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try{
    let typeObj : uniformTypeDescriptor.TypeDescriptor = uniformTypeDescriptor.getTypeDescriptor('general.source-code');
    let ret = typeObj.isHigherLevelType('general.type-script');
    if(ret) {
        console.info('type general.source-code is higher level type of type general.type-script');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`isHigherLevelType throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

### equals<sup>11+</sup> 

equals(typeDescriptor: TypeDescriptor): boolean

判断指定的标准化数据类型描述类对象的类型ID和当前标准化数据类型描述类对象的类型ID是否相同，即[TypeDescriptor](#typedescriptor11)对象的typeId。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| typeDescriptor    | [TypeDescriptor](#typedescriptor11)  | 是    |待比较的标准化数据类型描述类对象。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 返回true表示所比较的标准化数据类型相同；返回false则表示不同。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try{
    let typeA : uniformTypeDescriptor.TypeDescriptor = uniformTypeDescriptor.getTypeDescriptor('general.type-script');
    let typeB : uniformTypeDescriptor.TypeDescriptor = uniformTypeDescriptor.getTypeDescriptor('general.python-script');
    if(!typeA.equals(typeB)) {
      console.info('typeA is not equal to typeB');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

## uniformTypeDescriptor.getTypeDescriptor<sup>11+</sup>

getTypeDescriptor(typeId: string): TypeDescriptor

按给定的标准化数据类型ID查询并返回对应的标准化数据类型描述类对象。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| typeId    | string  | 是    |[标准化数据类型ID](../../database/uniform-data-type-list.md#基础类型)。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| [TypeDescriptor](#typedescriptor11) | 返回标准化数据类型描述类对象，如果要查询的标准化数据类型不存在则返回null。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try {
    let typeObj : uniformTypeDescriptor.TypeDescriptor = uniformTypeDescriptor.getTypeDescriptor('com.adobe.photoshop-image');
    if (typeObj) {
        let typeId = typeObj.typeId;
        let belongingToTypes = typeObj.belongingToTypes;
        let description = typeObj.description;
        let referenceURL = typeObj.referenceURL;
        let iconFile = typeObj.iconFile;
        let filenameExtensions = typeObj.filenameExtensions;
        let mimeTypes = typeObj.mimeTypes;
        console.info(`typeId: ${typeId}, belongingToTypes: ${belongingToTypes}, description: ${description}, referenceURL: ${referenceURL}, iconFile: ${iconFile}, filenameExtensions: ${filenameExtensions}, mimeTypes: ${mimeTypes}`);
    } else {
        console.info('type com.adobe.photoshop-image does not exist');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getTypeDescriptor throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

## uniformTypeDescriptor.getUniformDataTypeByFilenameExtension<sup>11+</sup>

getUniformDataTypeByFilenameExtension(filenameExtension: string, belongsTo?: string): string

根据给定的文件后缀名和所归属的标准化数据类型查询标准化数据类型ID，若有多个符合条件的标准化数据类型ID，则返回第一个。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| filenameExtension    | string  | 是    |文件后缀名称。   |
| belongsTo    | string  | 否    |要查询的标准化数据类型所归属类型ID，无默认值，若不传入此参数则只按照文件后缀名称查询[标准化数据类型ID](../../database/uniform-data-type-list.md#基础类型)。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| string | 返回与给定文件后缀名以及归属类型ID（如果设置了belongsTo参数）匹配的标准化数据类型ID，如果要查询的标准化数据类型不存在则返回根据入参按指定规则生成的动态类型。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try {
    let typeId = uniformTypeDescriptor.getUniformDataTypeByFilenameExtension('.ts', 'general.source-code');
    if(typeId) {
        console.info('typeId is general.type-script');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypeByFilenameExtension throws an exception. code is ${error.code}, message is ${error.message} `);
}

// 根据“.myts”，“general.plain-text”查不到预置数据类型则按返回根据入参信息生成的动态类型。
try {
    let typeId = uniformTypeDescriptor.getUniformDataTypeByFilenameExtension('.myts', 'general.plain-text');
    if(typeId) {
        console.info('typeId is flex.************');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypeByFilenameExtension throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

## uniformTypeDescriptor.getUniformDataTypeByMIMEType<sup>11+</sup>

getUniformDataTypeByMIMEType(mimeType: string, belongsTo?: string): string

根据给定的MIME类型和所归属的标准化数据类型查询标准化数据类型ID，若有多个符合条件的标准化数据类型ID，则返回第一个。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| mimeType    | string  | 是    |MIME类型名称。   |
| belongsTo    | string  | 否    |要查询的标准化数据类型所归属类型ID。无默认值，若不传入此参数则只按照MIME类型名称查询[标准化数据类型ID](../../database/uniform-data-type-list.md#基础类型)。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| string | 返回与MIME类型名称以及归属类型ID（如果设置了belongsTo参数）匹配的标准化数据类型ID，如果要查询的标准化数据类型不存在则返回根据入参按指定规则生成的动态类型。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try {
    let typeId = uniformTypeDescriptor.getUniformDataTypeByMIMEType('image/jpeg', 'general.image');
    if(typeId) {
        console.info('typeId is general.jpeg');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypeByMIMEType throws an exception. code is ${error.code}, message is ${error.message} `);
}

// 根据“image/myimage”, “general.image”查不到预置数据类型则按返回根据入参信息生成的动态类型。
try {
    let typeId = uniformTypeDescriptor.getUniformDataTypeByMIMEType('image/myimage', 'general.image');
    if(typeId) {
        console.info('typeId is flex.************');
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypeByMIMEType throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

## uniformTypeDescriptor.getUniformDataTypesByFilenameExtension<sup>13+</sup>

getUniformDataTypesByFilenameExtension(filenameExtension: string, belongsTo?: string): Array\<string>

根据给定的文件后缀名和所归属的标准化数据类型查询标准化数据类型ID列表。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| filenameExtension    | string  | 是    |文件后缀名称。   |
| belongsTo    | string  | 否    |要查询的标准化数据类型所归属类型ID，无默认值，若不传入此参数则只按照文件后缀名称查询[标准化数据类型ID](../../database/uniform-data-type-list.md#基础类型)。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| Array\<string> | 返回与给定文件后缀名以及归属类型ID（如果设置了belongsTo参数）匹配的标准化数据类型ID列表，如果要查询的标准化数据类型不存在则返回根据入参按指定规则生成的动态类型列表。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try {
    let typeIds = uniformTypeDescriptor.getUniformDataTypesByFilenameExtension('.ts', 'general.source-code');
    for (let typeId of typeIds) {
        console.info(`typeId is ${typeId}`);
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypesByFilenameExtension throws an exception. code is ${error.code}, message is ${error.message} `);
}

// 根据“.myts”，“general.plain-text”查不到预置数据类型则按返回根据入参信息生成的动态类型列表。
try {
    let flexTypeIds = uniformTypeDescriptor.getUniformDataTypesByFilenameExtension('.myts', 'general.plain-text');
    for (let flexTypeId of flexTypeIds) {
        console.info(`typeId is flex type, flex typeId is ${flexTypeId}`);
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypesByFilenameExtension throws an exception. code is ${error.code}, message is ${error.message} `);
}
```

## uniformTypeDescriptor.getUniformDataTypesByMIMEType<sup>13+</sup>

getUniformDataTypesByMIMEType(mimeType: string, belongsTo?: string): Array\<string>

根据给定的MIME类型和所归属的标准化数据类型查询标准化数据类型ID列表。

**系统能力：** SystemCapability.DistributedDataManager.UDMF.Core

**参数：**

| 参数名  | 类型 | 必填  | 说明                    |
| -----  | ------  | ----  | ----------------------- |
| mimeType    | string  | 是    |MIME类型名称。   |
| belongsTo    | string  | 否    |要查询的标准化数据类型所归属类型ID。无默认值，若不传入此参数则只按照MIME类型名称查询[标准化数据类型ID](../../database/uniform-data-type-list.md#基础类型)。   |

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| Array\<string> | 返回与MIME类型名称以及归属类型ID（如果设置了belongsTo参数）匹配的标准化数据类型ID列表，如果要查询的标准化数据类型不存在则返回根据入参按指定规则生成的动态类型列表。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcode-universal.md)。

| **错误码ID** | **错误信息**                                |
| ------------ | ------------------------------------------- |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameters types.  |

**示例：**

```ts
import { uniformTypeDescriptor } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

try {
    let typeIds = uniformTypeDescriptor.getUniformDataTypesByMIMEType('text/plain', 'general.text');
    for (let typeId of typeIds) {
        console.info(`typeId is ${typeId}`);
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypesByMIMEType throws an exception. code is ${error.code}, message is ${error.message} `);
}

// 根据“image/myimage”, “general.image”查不到预置数据类型则按返回根据入参信息生成的动态类型列表。
try {
    let flexTypeIds = uniformTypeDescriptor.getUniformDataTypesByMIMEType('image/myimage', 'general.image');
    for (let flexTypeId of flexTypeIds) {
        console.info(`typeId is flex type, flex typeId is ${flexTypeId}`);
    }
} catch(e) {
    let error: BusinessError = e as BusinessError;
    console.error(`getUniformDataTypesByMIMEType throws an exception. code is ${error.code}, message is ${error.message} `);
}
```
