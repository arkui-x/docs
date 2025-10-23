# @ohos.file.photoAccessHelper (相册管理模块)

该模块提供相册管理模块能力，包括创建相册以及访问、修改相册中的媒体数据信息等。

> **说明：**
>
> 本模块首批接口从API version 12开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。

iOS中使用此模块，需要在XCode工程的info.plist配置中增加以下配置项，以说明访问相册的目的和功能

- Privacy - Photo Library Usage Description


## 导入模块

```ts
import { photoAccessHelper } from '@kit.MediaLibraryKit';
```

## photoAccessHelper.getPhotoAccessHelper

getPhotoAccessHelper(context: Context): PhotoAccessHelper

获取相册管理模块的实例，用于访问和修改相册中的媒体文件。

**模型约束**： 此接口仅可在Stage模型下使用。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名  | 类型    | 必填 | 说明                       |
| ------- | ------- | ---- | -------------------------- |
| context | [Context](js-apis-inner-application-context.md) | 是   | 传入Ability实例的Context。 |

**返回值：**

| 类型                            | 说明    |
| ----------------------------- | :---- |
| [PhotoAccessHelper](#photoaccesshelper) | 相册管理模块的实例。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 

**示例：**

```ts
//此处获取的phAccessHelper实例为全局对象，后续使用到phAccessHelper的地方默认为使用此处获取的对象，如未添加此段代码报phAccessHelper未定义的错误请自行添加
let context = getContext(this);
let phAccessHelper = photoAccessHelper.getPhotoAccessHelper(context);
```

## PhotoAccessHelper

### getAssets

getAssets(options: FetchOptions, callback: AsyncCallback&lt;FetchResult&lt;PhotoAsset&gt;&gt;): void

获取图片和视频资源，使用callback方式返回结果。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.READ_IMAGEVIDEO

**参数：**

| 参数名   | 类型                     | 必填 | 说明                      |
| -------- | ------------------------ | ---- | ------------------------- |
| options  | [FetchOptions](#fetchoptions)        | 是   | 图片和视频检索选项。              |
| callback |  AsyncCallback&lt;[FetchResult](#fetchresult)&lt;[PhotoAsset](#photoasset)&gt;&gt; | 是   | callback返回图片和视频检索结果集。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900012     | Permission denied.         |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getAssets');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOptions: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };

  phAccessHelper.getAssets(fetchOptions, async (err, fetchResult) => {
    if (fetchResult !== undefined) {
      console.info('fetchResult success');
      let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getFirstObject();
      if (photoAsset !== undefined) {
        console.info('photoAsset.displayName : ' + photoAsset.displayName);
      }
    } else {
      console.error(`fetchResult fail with error: ${err.code}, ${err.message}`);
    }
  });
}
```

### getAssets

getAssets(options: FetchOptions): Promise&lt;FetchResult&lt;PhotoAsset&gt;&gt;

获取图片和视频资源，使用Promise方式返回结果。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.READ_IMAGEVIDEO

**参数：**

| 参数名  | 类型                | 必填 | 说明             |
| ------- | ------------------- | ---- | ---------------- |
| options | [FetchOptions](#fetchoptions)   | 是   | 图片和视频检索选项。     |

**返回值：**

| 类型                        | 说明           |
| --------------------------- | -------------- |
| Promise&lt;[FetchResult](#fetchresult)&lt;[PhotoAsset](#photoasset)&gt;&gt; | Promise对象，返回图片和视频数据结果集。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900012     | Permission denied.         |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getAssets');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOptions: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  try {
    let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOptions);
    if (fetchResult !== undefined) {
      console.info('fetchResult success');
      let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getFirstObject();
      if (photoAsset !== undefined) {
        console.info('photoAsset.displayName :' + photoAsset.displayName);
      }
    }
  } catch (err) {
    console.error(`getAssets failed, error: ${err.code}, ${err.message}`);
  }
}
```
### createAsset<sup>22+</sup>

createAsset(photoType: PhotoType, extension: string, options: CreateOptions, callback: AsyncCallback\<string\>): void

指定文件类型、后缀和创建选项，创建图片或视频资源。使用callback方式返回结果。

**支持平台**：Android

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.WRITE_IMAGEVIDEO

**参数：**

| 参数名    | 类型                                                        | 必填 | 说明                                                         | Android平台 | iOS平台 |
| :-------- | :---------------------------------------------------------- | :--- | :----------------------------------------------------------- | ----------- | ------- |
| photoType | [PhotoType](#phototype)                                     | 是   | 创建的文件类型，IMAGE或者VIDEO类型。                         | 支持        | 不支持  |
| extension | string                                                      | 是   | 文件名后缀参数，例如：'jpg'。**注意：** 需传入目标操作系统（Android）所支持和识别的标准文件扩展名,若传入"."则只会解析最后一个"."之后的字符，如传入".jpg"则最终会解析为jpg | 支持        | 不支持  |
| options   | [CreateOptions<sup>22+</sup>](#CreateOptions22+) | 是   | 创建选项，当前仅支持'title'，例如{title: 'testPhoto'}。**注意：** 传入'title'之外的选项，配置不生效。文件名中不允许出现非法英文字符，包括： . .. \ / : * ? " ' ` < > \| { } [ ] | 支持        | 不支持  |
| callback  | AsyncCallback\<string\>                                       | 是   | callback返回创建的图片和视频的uri。                          | 支持        | 不支持  |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息                                                     |
| :------- | :----------------------------------------------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |
| 201      | Permission denied.                                           |
| 13900020 | Invalid argument.                                            |
| 14000011 | System inner fail.                                           |

**示例：**

phAccessHelper的创建请参考[photoAccessHelper.getPhotoAccessHelper](#photoaccesshelpergetphotoaccesshelper)的示例使用。
```ts
async function example(phAccessHelper: photoAccessHelper.PhotoAccessHelper) {
    console.info('createAssetDemo');
    let photoType: photoAccessHelper.PhotoType = photoAccessHelper.PhotoType.IMAGE;
    let extension:string = 'jpg';
    let options: photoAccessHelper.CreateOptions = {
        title: 'testPhoto'
    }
    phAccessHelper.createAsset(photoType, extension, options, (err, uri) => {
        if (uri !== undefined) {
            console.info('createAsset uri' + uri);
            console.info('createAsset successfully');
        } else {
            console.error(`createAsset failed, error: ${err.code}, ${err.message}`);
        }
    });
}
```

### createAsset<sup>22+</sup>

createAsset(photoType: PhotoType, extension: string, callback: AsyncCallback\<string\>): void

指定文件类型和后缀，创建图片或视频资源，使用callback方式返回结果。

**支持平台**：Android

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.WRITE_IMAGEVIDEO

**参数：**

| 参数名    | 类型                    | 必填 | 说明                                                         | Android平台 | iOS平台 |
| :-------- | :---------------------- | :--- | :----------------------------------------------------------- | ----------- | ------- |
| photoType | [PhotoType](#phototype) | 是   | 创建的文件类型，IMAGE或者VIDEO类型。                         | 支持        | 不支持  |
| extension | string                  | 是   | 文件名后缀参数，例如：'jpg'。**注意：** 需传入目标操作系统（Android）所支持和识别的标准文件扩展名,若传入"."则只会解析最后一个"."之后的字符，如传入".jpg"则最终会解析为jpg | 支持        | 不支持  |
| callback  | AsyncCallback\<string\>  | 是   | callback返回创建的图片和视频的uri。                          | 支持        | 不支持  |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息                                                     |
| :------- | :----------------------------------------------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |
| 201      | Permission denied.                                           |
| 13900020 | Invalid argument.                                            |
| 14000011 | System inner fail.                                           |

**示例：**

phAccessHelper的创建请参考[photoAccessHelper.getPhotoAccessHelper](#photoaccesshelpergetphotoaccesshelper)的示例使用。
```ts
async function example(phAccessHelper: photoAccessHelper.PhotoAccessHelper) {  
    console.info('createAssetDemo');
    let photoType: photoAccessHelper.PhotoType = photoAccessHelper.PhotoType.IMAGE;
    let extension: string = 'jpg';
    phAccessHelper.createAsset(photoType, extension, (err, uri) => {
        if (uri !== undefined) {
            console.info('createAsset uri' + uri);
            console.info('createAsset successfully');
        } else {
            console.error(`createAsset failed, error: ${err.code}, ${err.message}`);
        }
    });
}
```

### createAsset<sup>22+</sup>

createAsset(photoType: PhotoType, extension: string, options?: CreateOptions): Promise\<string\>

指定文件类型、后缀和创建选项，创建图片或视频资源，以Promise方式返回结果。

**支持平台**：Android

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.WRITE_IMAGEVIDEO

**参数：**

| 参数名    | 类型                                                        | 必填 | 说明                                                         | Android平台 | iOS平台 |
| :-------- | :---------------------------------------------------------- | :--- | :----------------------------------------------------------- | ----------- | ------- |
| photoType | [PhotoType](#phototype)                                     | 是   | 创建的文件类型，IMAGE或者VIDEO类型。                         | 支持        | 不支持  |
| extension | string                                                      | 是   | 文件名后缀参数，例如：'jpg'。**注意：** 需传入目标操作系统（Android）所支持和识别的标准文件扩展名,若传入"."则只会解析最后一个"."之后的字符，如传入".jpg"则最终会解析为jpg | 支持        | 不支持  |
| options   | [CreateOptions<sup>22+</sup>](#CreateOptions22+) | 否   | 创建选项，当前仅支持'title'，例如{title: 'testPhoto'}。**注意：** 传入'title'之外的选项，配置不生效。文件名中不允许出现非法英文字符，包括： . .. \ / : * ? " ' ` < > \| { } [ ] | 支持        | 不支持  |

**返回值：**

| 类型            | 说明                                     |
| :-------------- | :--------------------------------------- |
| Promise\<string\> | Promise对象，返回创建的图片和视频的uri。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息                                                     |
| :------- | :----------------------------------------------------------- |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |
| 201      | Permission denied.                                           |
| 13900020 | Invalid argument.                                            |
| 14000011 | System inner fail.                                           |

**示例：**

phAccessHelper的创建请参考[photoAccessHelper.getPhotoAccessHelper](#photoaccesshelpergetphotoaccesshelper)的示例使用。
```ts
async function example(phAccessHelper: photoAccessHelper.PhotoAccessHelper) {
    console.info('createAssetDemo');
    try {
        let photoType: photoAccessHelper.PhotoType = photoAccessHelper.PhotoType.IMAGE;
        let extension: string = 'jpg';
        let options: photoAccessHelper.CreateOptions = {
            title: 'testPhoto'
        }
        let uri: string = await phAccessHelper.createAsset(photoType, extension, options);
        console.info('createAsset uri' + uri);
        console.info('createAsset successfully');
    } catch (err) {
        console.error(`createAsset failed, error: ${err.code}, ${err.message}`);
    }
}
```

### getAlbums

getAlbums(type: AlbumType, subtype: AlbumSubtype, options: FetchOptions, callback: AsyncCallback&lt;FetchResult&lt;Album&gt;&gt;): void

根据检索选项和相册类型获取相册，使用callback方式返回结果。

获取相册前需先保证相册存在。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.READ_IMAGEVIDEO

**参数：**

| 参数名   | 类型                     | 必填 | 说明                      |
| -------- | ------------------------ | ---- | ------------------------- |
| type  | [AlbumType](#albumtype)         | 是   | 相册类型。              |
| subtype  | [AlbumSubtype](#albumsubtype)         | 是   | 相册子类型。              |
| options  | [FetchOptions](#fetchoptions)         | 是   |  检索选项。              |
| callback |  AsyncCallback&lt;[FetchResult](#fetchresult)&lt;[Album](#album)&gt;&gt; | 是   | callback返回获取相册的结果集。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900012     | Permission denied.         |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  // 示例代码中为获取相册名为newAlbumName的相册。
  console.info('getAlbumsDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  predicates.equalTo('album_name', 'newAlbumName');
  let fetchOptions: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  phAccessHelper.getAlbums(photoAccessHelper.AlbumType.USER, photoAccessHelper.AlbumSubtype.USER_GENERIC, fetchOptions, async (err, fetchResult) => {
    if (err) {
      console.error(`getAlbumsCallback failed with err: ${err.code}, ${err.message}`);
      return;
    }
    if (fetchResult === undefined) {
      console.error('getAlbumsCallback fetchResult is undefined');
      return;
    }
    let album = await fetchResult.getFirstObject();
    console.info('getAlbumsCallback successfully, albumName: ' + album.albumName);
    fetchResult.close();
  });
}
```

### getAlbums

getAlbums(type: AlbumType, subtype: AlbumSubtype, callback: AsyncCallback&lt;FetchResult&lt;Album&gt;&gt;): void

根据相册类型获取相册，使用callback方式返回结果。

获取相册前需先保证相册存在。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.READ_IMAGEVIDEO

**参数：**

| 参数名   | 类型                     | 必填 | 说明                      |
| -------- | ------------------------ | ---- | ------------------------- |
| type  | [AlbumType](#albumtype)         | 是   | 相册类型。              |
| subtype  | [AlbumSubtype](#albumsubtype)         | 是   | 相册子类型。              |
| callback |  AsyncCallback&lt;[FetchResult](#fetchresult)&lt;[Album](#album)&gt;&gt; | 是   | callback返回获取相册的结果集。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900012     | Permission denied.         |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
async function example() {
  // 示例代码中为获取统相册VIDEO，默认已预置。
  console.info('getAlbumsDemo');
  phAccessHelper.getAlbums(photoAccessHelper.AlbumType.SYSTEM, photoAccessHelper.AlbumSubtype.VIDEO, async (err, fetchResult) => {
    if (err) {
      console.error(`getAlbumsCallback failed with err: ${err.code}, ${err.message}`);
      return;
    }
    if (fetchResult === undefined) {
      console.error('getAlbumsCallback fetchResult is undefined');
      return;
    }
    let album: photoAccessHelper.Album = await fetchResult.getFirstObject();
    console.info('getAlbumsCallback successfully, albumUri: ' + album.albumUri);
    fetchResult.close();
  });
}
```

### getAlbums

getAlbums(type: AlbumType, subtype: AlbumSubtype, options?: FetchOptions): Promise&lt;FetchResult&lt;Album&gt;&gt;

根据检索选项和相册类型获取相册，使用Promise方式返回结果。

获取相册前需先保证相册存在。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**需要权限**：ohos.permission.READ_IMAGEVIDEO

**参数：**

| 参数名   | 类型                     | 必填 | 说明                      |
| -------- | ------------------------ | ---- | ------------------------- |
| type  | [AlbumType](#albumtype)         | 是   | 相册类型。              |
| subtype  | [AlbumSubtype](#albumsubtype)         | 是   | 相册子类型。              |
| options  | [FetchOptions](#fetchoptions)         | 否   |  检索选项，不填时默认根据相册类型检索。              |

**返回值：**

| 类型                        | 说明           |
| --------------------------- | -------------- |
| Promise&lt;[FetchResult](#fetchresult)&lt;[Album](#album)&gt;&gt; | Promise对象，返回获取相册的结果集。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900012     | Permission denied.         |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

async function example() {
  // 示例代码中为获取相册名为newAlbumName的相册。
  console.info('getAlbumsDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  predicates.equalTo('album_name', 'newAlbumName');
  let fetchOptions: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  phAccessHelper.getAlbums(photoAccessHelper.AlbumType.USER, photoAccessHelper.AlbumSubtype.USER_GENERIC, fetchOptions).then( async (fetchResult) => {
    if (fetchResult === undefined) {
      console.error('getAlbumsPromise fetchResult is undefined');
      return;
    }
    let album: photoAccessHelper.Album = await fetchResult.getFirstObject();
    console.info('getAlbumsPromise successfully, albumName: ' + album.albumName);
    fetchResult.close();
  }).catch((err: BusinessError) => {
    console.error(`getAlbumsPromise failed with err: ${err.code}, ${err.message}`);
  });
}
```

## PhotoAsset

提供封装文件属性的方法。

### 属性

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称                      | 类型                     | 可读 | 可写 | 说明                                                   |
| ------------------------- | ------------------------ | ---- | ---- | ------------------------------------------------------ |
| uri                       | string                   | 是   | 否   | 媒体文件资源uri（如：file://media/Photo/1/IMG_datetime_0001/displayName.jpg）        |
| photoType   | [PhotoType](#phototype) | 是   | 否   | 媒体文件类型                                               |
| displayName               | string                   | 是   | 否   | 显示文件名，包含后缀名。                                 |

### get

get(member: string): MemberType

获取PhotoAsset成员参数。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名      | 类型                        | 必填   | 说明    |
| -------- | ------------------------- | ---- | ----- |
| member | string | 是    | 成员参数名称，在get时，除了'uri'、'media_type'、'subtype'和'display_name'四个属性之外，其他的属性都需要在fetchColumns中填入需要get的[PhotoKeys](#photokeys)，例如：get title属性fetchColumns: ['title']。 |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [MemberType](#membertype) | 获取PhotoAsset成员参数的值。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900020     | Invalid argument.         |
| 14000014     | Member is not a valid PhotoKey.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('photoAssetGetDemo');
  try {
    let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
    let fetchOption: photoAccessHelper.FetchOptions = {
      fetchColumns: ['title'],
      predicates: predicates
    };
    let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
    let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getFirstObject();
    let title: photoAccessHelper.PhotoKeys = photoAccessHelper.PhotoKeys.TITLE;
    let photoAssetTitle: photoAccessHelper.MemberType = photoAsset.get(title.toString());
    console.info('photoAsset Get photoAssetTitle = ', photoAssetTitle);
  } catch (err) {
    console.error(`release failed. error: ${err.code}, ${err.message}`);
  }
}
```

## PhotoViewPicker

图库选择器对象，用来支撑选择图片/视频等用户场景。在使用前，需要先创建PhotoViewPicker实例。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**示例：**

```ts
let photoPicker = new photoAccessHelper.PhotoViewPicker();
```

### select

select(option?: PhotoSelectOptions) : Promise&lt;PhotoSelectResult&gt;

通过选择模式拉起photoPicker界面，用户可以选择一个或多个图片/视频。接口采用promise异步返回形式，传入可选参数PhotoSelectOptions对象，返回PhotoSelectResult对象。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名  | 类型    | 必填 | 说明                       |
| ------- | ------- | ---- | -------------------------- |
| option | [PhotoSelectOptions](#photoselectoptions) | 否   | photoPicker选择选项，若无此参数，则默认选择媒体文件类型为图片和视频类型，默认选择媒体文件数量的最大值为50 |

**返回值：**

| 类型                            | 说明    |
| ----------------------------- | :---- |
| Promise&lt;[PhotoSelectResult](#photoselectresult)&gt; | Promise对象。返回photoPicker选择后的结果集 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900042      | Unknown error.         |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
async function example01() {
  try {
    let PhotoSelectOptions = new photoAccessHelper.PhotoSelectOptions();
    PhotoSelectOptions.MIMEType = photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE;
    let photoPicker = new photoAccessHelper.PhotoViewPicker();
    photoPicker.select(PhotoSelectOptions).then((PhotoSelectResult: photoAccessHelper.PhotoSelectResult) => {
      console.info('PhotoViewPicker.select successfully, PhotoSelectResult uri: ' + JSON.stringify(PhotoSelectResult));
    }).catch((err: BusinessError) => {
      console.error(`PhotoViewPicker.select failed with err: ${err.code}, ${err.message}`);
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`PhotoViewPicker failed with err: ${err.code}, ${err.message}`);
  }
}
```

### select

select(option: PhotoSelectOptions, callback: AsyncCallback&lt;PhotoSelectResult&gt;) : void

通过选择模式拉起photoPicker界面，用户可以选择一个或多个图片/视频。接口采用callback异步返回形式，传入参数PhotoSelectOptions对象，返回PhotoSelectResult对象。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名  | 类型    | 必填 | 说明                       |
| ------- | ------- | ---- | -------------------------- |
| option | [PhotoSelectOptions](#photoselectoptions) | 是   | photoPicker选择选项 |
| callback | AsyncCallback&lt;[PhotoSelectResult](#photoselectresult)&gt;      | 是   | callback 返回photoPicker选择后的结果集 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900042      | Unknown error.         |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
async function example02() {
  try {
    let PhotoSelectOptions = new photoAccessHelper.PhotoSelectOptions();
    PhotoSelectOptions.MIMEType = photoAccessHelper.PhotoViewMIMETypes.IMAGE_TYPE;
    let photoPicker = new photoAccessHelper.PhotoViewPicker();
    photoPicker.select(PhotoSelectOptions, (err: BusinessError, PhotoSelectResult: photoAccessHelper.PhotoSelectResult) => {
      if (err) {
        console.error(`PhotoViewPicker.select failed with err: ${err.code}, ${err.message}`);
        return;
      }
      console.info('PhotoViewPicker.select successfully, PhotoSelectResult uri: ' + JSON.stringify(PhotoSelectResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`PhotoViewPicker failed with err: ${err.code}, ${err.message}`);
  }
}
```

### select

select(callback: AsyncCallback&lt;PhotoSelectResult&gt;) : void

通过选择模式拉起photoPicker界面，用户可以选择一个或多个图片/视频。接口采用callback异步返回形式，返回PhotoSelectResult对象。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名  | 类型    | 必填 | 说明                       |
| ------- | ------- | ---- | -------------------------- |
| callback | AsyncCallback&lt;[PhotoSelectResult](#photoselectresult)&gt;      | 是   | callback 返回photoPicker选择后的结果集 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900042      | Unknown error.         |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
async function example03() {
  try {
    let photoPicker = new photoAccessHelper.PhotoViewPicker();
    photoPicker.select((err: BusinessError, PhotoSelectResult: photoAccessHelper.PhotoSelectResult) => {
      if (err) {
        console.error(`PhotoViewPicker.select failed with err: ${err.code}, ${err.message}`);
        return;
      }
      console.info('PhotoViewPicker.select successfully, PhotoSelectResult uri: ' + JSON.stringify(PhotoSelectResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`PhotoViewPicker failed with err: ${err.code}, ${err.message}`);
  }
}
```

## FetchResult

文件检索结果集。

### getCount

getCount(): number

获取文件检索结果中的文件总数。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**返回值：**

| 类型     | 说明       |
| ------ | -------- |
| number | 检索到的文件总数。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getCountDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  let fetchCount = fetchResult.getCount();
  console.info('fetchCount = ', fetchCount);
}
```

### isAfterLast

isAfterLast(): boolean

检查结果集是否指向最后一行。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**返回值：**

| 类型      | 说明                                 |
| ------- | ---------------------------------- |
| boolean | 当读到最后一条记录后，后续没有记录返回true，否则返回false。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  let fetchCount = fetchResult.getCount();
  console.info('count:' + fetchCount);
  let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getLastObject();
  if (fetchResult.isAfterLast()) {
    console.info('photoAsset isAfterLast displayName = ', photoAsset.displayName);
  } else {
    console.info('photoAsset not isAfterLast.');
  }
}
```

### close

close(): void

释放FetchResult实例并使其失效。无法调用其他方法。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('fetchResultCloseDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  try {
    let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
    fetchResult.close();
    console.info('close succeed.');
  } catch (err) {
    console.error(`close fail. error: ${err.code}, ${err.message}`);
  }
}
```

### getFirstObject

getFirstObject(callback: AsyncCallback&lt;T&gt;): void

获取文件检索结果中的第一个文件资产。此方法使用callback形式返回结果。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                                        |
| -------- | --------------------------------------------- | ---- | ------------------------------------------- |
| callback | AsyncCallback&lt;T&gt; | 是   | 异步获取结果集中的第一个完成后的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getFirstObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  fetchResult.getFirstObject((err, photoAsset) => {
    if (photoAsset !== undefined) {
      console.info('photoAsset displayName: ', photoAsset.displayName);
    } else {
      console.error(`photoAsset failed with err:${err.code}, ${err.message}`);
    }
  });
}
```

### getFirstObject

getFirstObject(): Promise&lt;T&gt;

获取文件检索结果中的第一个文件资产。此方法使用promise方式来异步返回。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**返回值：**

| 类型                                    | 说明                       |
| --------------------------------------- | -------------------------- |
| Promise&lt;T&gt; | Promise对象，返回结果集中第一个对象。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getFirstObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getFirstObject();
  console.info('photoAsset displayName: ', photoAsset.displayName);
}
```

### getNextObject

getNextObject(callback: AsyncCallback&lt;T&gt;): void

获取文件检索结果中的下一个文件资产。此方法使用callback形式返回结果。
在调用此方法之前，必须使用[isAfterLast()](#isafterlast)来检查当前位置是否为最后一行。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名    | 类型                                          | 必填 | 说明                                      |
| --------- | --------------------------------------------- | ---- | ----------------------------------------- |
| callback | AsyncCallback&lt;T&gt; | 是   | 异步返回结果集中下一个之后的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getNextObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  await fetchResult.getFirstObject();
  if (!fetchResult.isAfterLast()) {
    fetchResult.getNextObject((err, photoAsset) => {
      if (photoAsset !== undefined) {
        console.info('photoAsset displayName: ', photoAsset.displayName);
      } else {
        console.error(`photoAsset failed with err: ${err.code}, ${err.message}`);
      }
    });
  }
}
```

### getNextObject

getNextObject(): Promise&lt;T&gt;

获取文件检索结果中的下一个文件资产。此方法使用promise方式来异步返回。
在调用此方法之前，必须使用[isAfterLast()](#isafterlast)来检查当前位置是否为最后一行。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**返回值：**

| 类型                                    | 说明              |
| --------------------------------------- | ----------------- |
| Promise&lt;T&gt; | Promise对象，返回结果集中下一个对象。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getNextObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  await fetchResult.getFirstObject();
  if (!fetchResult.isAfterLast()) {
    let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getNextObject();
    console.info('photoAsset displayName: ', photoAsset.displayName);
  }
}
```

### getLastObject

getLastObject(callback: AsyncCallback&lt;T&gt;): void

获取文件检索结果中的最后一个文件资产。此方法使用callback回调来返回。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                        |
| -------- | --------------------------------------------- | ---- | --------------------------- |
| callback | AsyncCallback&lt;T&gt; | 是   | 异步返回结果集中最后一个的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getLastObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  fetchResult.getLastObject((err, photoAsset) => {
    if (photoAsset !== undefined) {
      console.info('photoAsset displayName: ', photoAsset.displayName);
    } else {
      console.error(`photoAsset failed with err: ${err.code}, ${err.message}`);
    }
  });
}
```

### getLastObject

getLastObject(): Promise&lt;T&gt;

获取文件检索结果中的最后一个文件资产。此方法使用Promise方式来返回。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**返回值：**

| 类型                                    | 说明              |
| --------------------------------------- | ----------------- |
| Promise&lt;T&gt; | Promise对象，返回结果集中最后一个对象。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getLastObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getLastObject();
  console.info('photoAsset displayName: ', photoAsset.displayName);
}
```

### getObjectByPosition

getObjectByPosition(index: number, callback: AsyncCallback&lt;T&gt;): void

获取文件检索结果中具有指定索引的文件资产。此方法使用callback来返回。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名       | 类型                                       | 必填   | 说明                 |
| -------- | ---------------------------------------- | ---- | ------------------ |
| index    | number                                   | 是    | 要获取的文件的索引，从0开始。     |
| callback | AsyncCallback&lt;T&gt; | 是    | 异步返回指定索引的文件资产的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getObjectByPositionDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  fetchResult.getObjectByPosition(0, (err, photoAsset) => {
    if (photoAsset !== undefined) {
      console.info('photoAsset displayName: ', photoAsset.displayName);
    } else {
      console.error(`photoAsset failed with err: ${err.code}, ${err.message}`);
    }
  });
}
```

### getObjectByPosition

getObjectByPosition(index: number): Promise&lt;T&gt;

获取文件检索结果中具有指定索引的文件资产。此方法使用Promise形式返回文件Asset。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名    | 类型     | 必填   | 说明             |
| ----- | ------ | ---- | -------------- |
| index | number | 是    | 要获取的文件的索引，从0开始。 |

**返回值：**

| 类型                                    | 说明              |
| --------------------------------------- | ----------------- |
| Promise&lt;T&gt; | Promise对象，返回结果集中指定索引的一个对象。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getObjectByPositionDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  let photoAsset: photoAccessHelper.PhotoAsset = await fetchResult.getObjectByPosition(0);
  console.info('photoAsset displayName: ', photoAsset.displayName);
}
```

### getAllObjects

getAllObjects(callback: AsyncCallback&lt;Array&lt;T&gt;&gt;): void

获取文件检索结果中的所有文件资产。此方法使用callback形式返回结果。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                                        |
| -------- | --------------------------------------------- | ---- | ------------------------------------------- |
| callback | AsyncCallback&lt;Array&lt;T&gt;&gt; | 是   | 异步获取结果集中的所有文件资产完成后的回调。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getAllObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  fetchResult.getAllObjects((err, photoAssetList) => {
    if (photoAssetList !== undefined) {
      console.info('photoAssetList length: ', photoAssetList.length);
    } else {
      console.error(`photoAssetList failed with err:${err.code}, ${err.message}`);
    }
  });
}
```

### getAllObjects

getAllObjects(): Promise&lt;Array&lt;T&gt;&gt;

获取文件检索结果中的所有文件资产。此方法使用promise方式来异步返回。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**返回值：**

| 类型                                    | 说明                       |
| --------------------------------------- | -------------------------- |
| Promise&lt;Array&lt;T&gt;&gt; | Promise对象，返回结果集中所有文件资产数组。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401    | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('getAllObjectDemo');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchResult: photoAccessHelper.FetchResult<photoAccessHelper.PhotoAsset> = await phAccessHelper.getAssets(fetchOption);
  let photoAssetList: Array<photoAccessHelper.PhotoAsset> = await fetchResult.getAllObjects();
  console.info('photoAssetList length: ', photoAssetList.length);
}
```

## Album

实体相册

### 属性

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称           | 类型    | 可读   | 可写  | 说明   |
| ------------ | ------ | ---- | ---- | ------- |
| albumType | [AlbumType](#albumtype) | 是    | 否    | 相册类型。    |
| albumSubtype | [AlbumSubtype](#albumsubtype) | 是    | 否   | 相册子类型。    |
| albumName | string | 是    | 用户相册可写，预置相册不可写   | 相册名称。    |
| albumUri | string | 是    | 否    | 相册Uri。   |
| count | number | 是    | 否    |  相册中文件数量。 |
| imageCount | number | 是   | 否   | 相册中图片数量。|
| videoCount | number | 是   | 否   | 相册中视频数量。|

### getAssets

getAssets(options: FetchOptions, callback: AsyncCallback&lt;FetchResult&lt;PhotoAsset&gt;&gt;): void

获取相册中的文件。该方法使用callback形式来返回文件。

**需要权限**：ohos.permission.READ_IMAGEVIDEO

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明       |
| -------- | ------------------------- | ---- | ---------- |
| options | [FetchOptions](#fetchoptions) | 是   | 检索选项。 |
| callback | AsyncCallback&lt;[FetchResult](#fetchresult)&lt;[PhotoAsset](#photoasset)&gt;&gt; | 是   | callback返回图片和视频数据结果集。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900012     | Permission denied.         |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';

async function example() {
  console.info('albumGetAssetsDemoCallback');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let albumFetchOptions: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let albumList: photoAccessHelper.FetchResult<photoAccessHelper.Album> = await phAccessHelper.getAlbums(photoAccessHelper.AlbumType.USER, photoAccessHelper.AlbumSubtype.USER_GENERIC, albumFetchOptions);
  let album: photoAccessHelper.Album = await albumList.getFirstObject();
  album.getAssets(fetchOption, (err, albumFetchResult) => {
    if (albumFetchResult !== undefined) {
      console.info('album getAssets successfully, getCount: ' + albumFetchResult.getCount());
    } else {
      console.error(`album getAssets failed with error: ${err.code}, ${err.message}`);
    }
  });
}
```

### getAssets

getAssets(options: FetchOptions): Promise&lt;FetchResult&lt;PhotoAsset&gt;&gt;

获取相册中的文件。该方法使用Promise来返回文件。

**需要权限**：ohos.permission.READ_IMAGEVIDEO

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明       |
| -------- | ------------------------- | ---- | ---------- |
| options | [FetchOptions](#fetchoptions) | 是   | 检索选项。 |

**返回值：**

| 类型                                    | 说明              |
| --------------------------------------- | ----------------- |
| Promise&lt;[FetchResult](#fetchresult)&lt;[PhotoAsset](#photoasset)&gt;&gt; | Promise对象，返回图片和视频数据结果集。 |

**错误码：**

接口抛出错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[文件管理错误码](../errorcodes/errorcode-filemanagement.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. | 
| 13900012     | Permission denied.         |
| 13900020     | Invalid argument.         |
| 14000011       | System inner fail.         |

**示例：**

```ts
import { dataSharePredicates } from '@kit.ArkData';
import { BusinessError } from '@kit.BasicServicesKit';

async function example() {
  console.info('albumGetAssetsDemoPromise');
  let predicates: dataSharePredicates.DataSharePredicates = new dataSharePredicates.DataSharePredicates();
  let albumFetchOptions: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let fetchOption: photoAccessHelper.FetchOptions = {
    fetchColumns: [],
    predicates: predicates
  };
  let albumList: photoAccessHelper.FetchResult<photoAccessHelper.Album> = await phAccessHelper.getAlbums(photoAccessHelper.AlbumType.USER, photoAccessHelper.AlbumSubtype.USER_GENERIC, albumFetchOptions);
  let album: photoAccessHelper.Album = await albumList.getFirstObject();
  album.getAssets(fetchOption).then((albumFetchResult) => {
    console.info('album getAssets successfully, getCount: ' + albumFetchResult.getCount());
  }).catch((err: BusinessError) => {
    console.error(`album getAssets failed with error: ${err.code}, ${err.message}`);
  });
}
```

## MemberType

type MemberType = number | string | boolean

PhotoAsset的成员类型。

成员类型为下表类型中的并集。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 类型 | 说明 |
| ---- | ---- |
| number | 表示值类型为数字，可取任意值。 |
| string | 表示值类型为字符，可取任意值。|
| boolean | 表示值类型为布尔类型。 |

## PhotoType

枚举，媒体文件类型。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称  |  值 |  说明 |
| ----- |  ---- |  ---- |
| IMAGE |  1 |  图片。 |
| VIDEO |  2 |  视频。 |

## AlbumType

枚举，相册类型，表示是用户相册还是系统预置相册。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称                  | 值    | 说明                        |
| ------------------- | ---- | ------------------------- |
| USER                | 0    | 用户相册。                     |
| SYSTEM              | 1024 | 系统预置相册。                   |

## AlbumSubtype

枚举，相册子类型，表示具体的相册类型。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称                                | 值          | 说明                              |
| --------------------------------- | ---------- | ------------------------------- |
| USER\_GENERIC                     | 1          | 用户相册。                           |
| FAVORITE                          | 1025       | 收藏夹。                            |
| VIDEO                             | 1026       | 视频相册。                           |
| ANY                               | 2147483647 | 任意相册。                           |

## PhotoKeys

枚举，图片和视频文件关键信息。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称          | 值              | 说明                                                       |
| ------------- | ------------------- | ---------------------------------------------------------- |
| URI           | 'uri'                 | 文件uri。<br>注意：不支持基于该字段查询、排序。           |
| PHOTO_TYPE    | 'media_type'           | 媒体文件类型。<br>注意：Android不支持基于该字段排序。                                              |
| DISPLAY_NAME  | 'display_name'        | 显示名字。<br>注意：iOS不支持基于该字段查询、排序。                                                   |
| SIZE          | 'size'                | 文件大小（单位：字节）。<br>注意：iOS不支持基于该字段查询、排序。                                                   |
| DATE_ADDED    | 'date_added'          | 添加日期（添加文件时间距1970年1月1日的秒数值）。<br>注意：iOS不支持基于该字段查询。             |
| DATE_MODIFIED | 'date_modified'       | 修改日期（修改文件时间距1970年1月1日的秒数值，修改文件名不会改变此值，当文件内容发生修改时才会更新）。<br>注意：iOS不支持基于该字段查询。 |
| DURATION      | 'duration'            | 持续时间（单位：毫秒）。                                    |
| WIDTH         | 'width'               | 图片宽度（单位：像素）。                                    |
| HEIGHT        | 'height'              | 图片高度（单位：像素）。                                      |
| DATE_TAKEN    | 'date_taken'          | 拍摄日期（文件拍照时间距1970年1月1日的秒数值）。<br>注意：iOS不支持基于该字段查询。                |
| ORIENTATION   | 'orientation'         | 文件的旋转角度，单位为度。<br>注意：iOS不支持基于该字段查询、排序。                                             |
| FAVORITE      | 'is_favorite'            | 收藏。                                                    |
| TITLE         | 'title'               | 文件标题。<br>注意：iOS不支持基于该字段查询、排序。                                                   |

## AlbumKeys

枚举，相册关键信息。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称          | 值              | 说明                                                       |
| ------------- | ------------------- | ---------------------------------------------------------- |
| URI           | 'uri'                 | 相册uri。                                                   |
| ALBUM_NAME    | 'album_name'          | 相册名字。                                                   |

## CreateOptions<sup>22+</sup>

图片或视频的创建选项。

title参数的规格如下：

- 不应包含扩展名。
- 文件名字符串长度为1~255。

**支持平台**：Android

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称  | 类型   | 必填 | 说明                 | Android平台 | iOS平台 |
| :---- | :----- | :--- | :------------------- | ----------- | ------- |
| title | string | 否   | 图片或者视频的标题。 | 支持        | 不支持  |

## FetchOptions

检索条件。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称                   | 类型                | 可读 | 可写 | 说明                                              |
| ---------------------- | ------------------- | ---- |---- | ------------------------------------------------ |
| fetchColumns           | Array&lt;string&gt; | 是   | 是   | 检索条件，指定列名查询。<br>对于照片，如果该参数为空，默认查询'uri'、'media_type'、'subtype'和'display_name'，使用[get](#get)接口获取当前对象的其他属性时将会报错。示例：fetchColumns: ['uri', 'title']。<br>对于相册，如果该参数为空，默认查询'uri'和'album_name'。 |
| predicates           | [dataSharePredicates.DataSharePredicates](js-apis-data-dataSharePredicates.md#datasharepredicates) | 是   | 是   | 谓词查询，显示过滤条件。 |

## PhotoViewMIMETypes

枚举，可选择的媒体文件类型。

**系统能力：** SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称                                    |  值 | 说明       |
|---------------------------------------|  ---- |----------|
| IMAGE_TYPE                            |  'image/*' | 图片类型。    |
| VIDEO_TYPE                            |  'video/*' | 视频类型。    |
| IMAGE_VIDEO_TYPE                      |  '\*/*' | 图片和视频类型。 |

## BaseSelectOptions

图库选择选项基类。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称                    | 类型                | 必填 | 说明                          |
| ----------------------- | ------------------- | ---- | -------------------------------- |
| MIMEType    | [PhotoViewMIMETypes](#photoviewmimetypes)   | 否   | 可选择的媒体文件类型，若无此参数，则默认为图片和视频类型。 |

## PhotoSelectOptions

图库选择选项子类，继承于BaseSelectOptions。

## PhotoSelectResult

返回图库选择后的结果集。

**系统能力**：SystemCapability.FileManagement.PhotoAccessHelper.Core

| 名称                    | 类型                | 可读 | 可写 | 说明                           |
| ----------------------- | ------------------- | ---- | ---- | ------------------------------ |
| photoUris        | Array&lt;string&gt;    | 是   | 是   | 返回图库选择后的媒体文件的uri数组。 |
| isOriginalPhoto        | boolean    | 是   | 是   | 返回图库选择后的媒体文件是否为原图。 |
