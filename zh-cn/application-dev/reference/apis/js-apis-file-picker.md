# @ohos.file.picker (选择器)

> **说明：**
>
> 该模块接口从API version 9开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。

选择器(Picker)是一个封装PhotoViewPicker、DocumentViewPicker、AudioViewPicker等API模块，具有选择与保存的能力。应用可以自行选择使用哪种API实现文件选择和文件保存的功能。该类接口，需要应用在界面UIAbility中调用，否则无法拉起photoPicker应用或FilePicker应用。

## 导入模块

```ts
import  { picker } from '@kit.CoreFileKit';
```

## DocumentViewPicker

文件选择器对象，用来支撑选择和保存各种格式文档。在使用前，需要先创建DocumentViewPicker实例。

**系统能力**：SystemCapability.FileManagement.UserFileService

### select

select(option?: DocumentSelectOptions): Promise<Array\<string\>>

通过选择模式拉起documentPicker界面，用户可以选择一个或多个文件。接口采用Promise异步返回形式，传入可选参数DocumentSelectOptions对象，返回选择文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。

**原子化服务API:** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名 | 类型 | 必填 | 说明                                                               |
| -------- | ------ | ------ | -------------------------------------------------------------------- |
| option | [DocumentSelectOptions](#documentselectoptions)     | 否   | documentPicker选择选项，若无此参数，则默认拉起documentPicker主界面 |

**返回值：**

| 类型                   | 说明                                          |
| ------------------------ | :---------------------------------------------- |
| Promise<Array\<string\>> | Promise对象。返回documentPicker选择后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example07(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let documentSelectOptions = new picker.DocumentSelectOptions();
    let documentPicker = new picker.DocumentViewPicker(context);
    documentPicker.select(documentSelectOptions).then((documentSelectResult: Array<string>) => {
      console.info('DocumentViewPicker.select successfully, documentSelectResult uri: ' + JSON.stringify(documentSelectResult));
    }).catch((err: BusinessError) => {
      console.error('DocumentViewPicker.select failed with err: ' + JSON.stringify(err));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('DocumentViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### select

select(option: DocumentSelectOptions, callback: AsyncCallback<Array\<string\>>): void

通过选择模式拉起documentPicker界面，用户可以选择一个或多个文件。接口采用callback异步返回形式，传入参数DocumentSelectOptions对象，返回选择文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。

**原子化服务API:** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                      |
| ---------- | ------------------------------ | ------ | ------------------------------------------- |
| option   | [DocumentSelectOptions](#documentselectoptions)                             | 是   | documentPicker选择选项                    |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回documentPicker选择后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example08(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let documentSelectOptions = new picker.DocumentSelectOptions();
    let documentPicker = new picker.DocumentViewPicker(context);
    documentPicker.select(documentSelectOptions, (err: BusinessError, documentSelectResult: Array<string>) => {
      if (err) {
        console.error('DocumentViewPicker.select failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('DocumentViewPicker.select successfully, documentSelectResult uri: ' + JSON.stringify(documentSelectResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('DocumentViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### select

select(callback: AsyncCallback<Array\<string\>>): void

通过选择模式拉起documentPicker界面，用户可以选择一个或多个文件。接口采用callback异步返回形式，返回选择文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。

**原子化服务API:** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                      |
| ---------- | ------------------------------ | ------ | ------------------------------------------- |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回documentPicker选择后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example09(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let documentPicker = new picker.DocumentViewPicker(context);
    documentPicker.select((err: BusinessError, documentSelectResult: Array<string>) => {
      if (err) {
        console.error('DocumentViewPicker.select failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('DocumentViewPicker.select successfully, documentSelectResult uri: ' + JSON.stringify(documentSelectResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('DocumentViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### save

save(option?: DocumentSaveOptions): Promise<Array\<string\>>

通过保存模式拉起documentPicker界面，用户可以保存一个或多个文件。接口采用Promise异步返回形式，传入可选参数DocumentSaveOptions对象，返回保存文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。（仅支持安卓跨平台）

**原子化服务API:** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名 | 类型 | 必填 | 说明                                                                                     |
| -------- | ------ | ------ | ------------------------------------------------------------------------------------------ |
| option | [DocumentSaveOptions](#documentsaveoptions)     | 否   | documentPicker保存选项，若无此参数，则拉起documentPicker界面后需用户自行输入保存的文件名 |

**返回值：**

| 类型                   | 说明                                          |
| ------------------------ | :---------------------------------------------- |
| Promise<Array\<string\>> | Promise对象。返回documentPicker保存后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example10(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let documentSaveOptions = new picker.DocumentSaveOptions();
    documentSaveOptions.newFileNames = ['DocumentViewPicker01.txt'];
    let documentPicker = new picker.DocumentViewPicker(context);
    documentPicker.save(documentSaveOptions).then((documentSaveResult: Array<string>) => {
      console.info('DocumentViewPicker.save successfully, documentSaveResult uri: ' + JSON.stringify(documentSaveResult));
    }).catch((err: BusinessError) => {
      console.error('DocumentViewPicker.save failed with err: ' + JSON.stringify(err));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('DocumentViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### save

save(option: DocumentSaveOptions, callback: AsyncCallback<Array\<string\>>): void

通过保存模式拉起documentPicker界面，用户可以保存一个或多个文件。接口采用callback异步返回形式，传入参数DocumentSaveOptions对象，返回保存文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。（仅支持安卓跨平台）

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                      |
| ---------- | ------------------------------ | ------ | ------------------------------------------- |
| option   | [DocumentSaveOptions](#documentsaveoptions)                             | 是   | documentPicker保存选项                    |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回documentPicker保存后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example11(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let documentSaveOptions = new picker.DocumentSaveOptions();
    documentSaveOptions.newFileNames = ['DocumentViewPicker02.txt'];
    let documentPicker = new picker.DocumentViewPicker(context);
    documentPicker.save(documentSaveOptions, (err: BusinessError, documentSaveResult: Array<string>) => {
      if (err) {
        console.error('DocumentViewPicker.save failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('DocumentViewPicker.save successfully, documentSaveResult uri: ' + JSON.stringify(documentSaveResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('DocumentViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### save

save(callback: AsyncCallback<Array\<string\>>): void

通过保存模式拉起documentPicker界面，用户可以保存一个或多个文件。接口采用callback异步返回形式，返回保存文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。（仅支持安卓跨平台）

**原子化服务API:** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                      |
| ---------- | ------------------------------ | ------ | ------------------------------------------- |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回documentPicker保存后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example12(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let documentPicker = new picker.DocumentViewPicker(context);
    documentPicker.save((err: BusinessError, documentSaveResult: Array<string>) => {
      if (err) {
        console.error('DocumentViewPicker.save failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('DocumentViewPicker.save successfully, documentSaveResult uri: ' + JSON.stringify(documentSaveResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('DocumentViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

## AudioViewPicker

音频选择器对象，用来支撑选择和保存音频类文件等用户场景。在使用前，需要先创建AudioViewPicker实例。

**系统能力**：SystemCapability.FileManagement.UserFileService

### select

select(option?: AudioSelectOptions): Promise<Array\<string\>>

通过选择模式拉起audioPicker界面，用户可以选择一个或多个音频文件。接口采用Promise异步返回形式，传入可选参数AudioSelectOptions对象，返回选择音频文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[媒体类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名 | 类型 | 必填 | 说明                                                             |
| -------- | ------ | ------ | ------------------------------------------------------------------ |
| option | [AudioSelectOptions](#audioselectoptions)     | 否   | audioPicker音频选择选项，若无此参数，则默认拉起audioPicker主界面 |

**返回值：**

| 类型                   | 说明                                           |
| ------------------------ | :----------------------------------------------- |
| Promise<Array\<string\>> | Promise对象。返回audioPicker选择音频后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example13(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let audioSelectOptions = new picker.AudioSelectOptions();
    let audioPicker = new picker.AudioViewPicker(context);
    audioPicker.select(audioSelectOptions).then((audioSelectResult: Array<string>) => {
      console.info('AudioViewPicker.select successfully, audioSelectResult uri: ' + JSON.stringify(audioSelectResult));
    }).catch((err: BusinessError) => {
      console.error('AudioViewPicker.select failed with err: ' + JSON.stringify(err));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('AudioViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### select

select(option: AudioSelectOptions, callback: AsyncCallback<Array\<string\>>): void

通过选择模式拉起audioPicker界面，用户可以选择一个或多个音频文件。接口采用callback异步返回形式，传入参数AudioSelectOptions对象，返回选择音频文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[媒体类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                       |
| ---------- | ------------------------------ | ------ | -------------------------------------------- |
| option   | [AudioSelectOptions](#audioselectoptions)                             | 是   | audioPicker音频选择选项                    |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回audioPicker选择音频后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example14(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let audioSelectOptions = new picker.AudioSelectOptions();
    let audioPicker = new picker.AudioViewPicker(context);
    audioPicker.select(audioSelectOptions, (err: BusinessError, audioSelectResult: Array<string>) => {
      if (err) {
        console.error('AudioViewPicker.select failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('AudioViewPicker.select successfully, audioSelectResult uri: ' + JSON.stringify(audioSelectResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('AudioViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### select

select(callback: AsyncCallback<Array\<string\>>): void

通过选择模式拉起audioPicker界面，用户可以选择一个或多个音频文件。接口采用callback异步返回形式，返回选择音频文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[媒体类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                       |
| ---------- | ------------------------------ | ------ | -------------------------------------------- |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回audioPicker选择音频后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example15(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let audioPicker = new picker.AudioViewPicker(context);
    audioPicker.select((err: BusinessError, audioSelectResult: Array<string>) => {
      if (err) {
        console.error('AudioViewPicker.select failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('AudioViewPicker.select successfully, audioSelectResult uri: ' + JSON.stringify(audioSelectResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('AudioViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### save

save(option?: AudioSaveOptions): Promise<Array\<string\>>

通过保存模式拉起audioPicker界面（目前拉起的是documentPicker，audioPicker在规划中），用户可以保存一个或多个音频文件。接口采用Promise异步返回形式，传入可选参数AudioSaveOptions对象，返回保存音频文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。（仅支持安卓跨平台）

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名 | 类型 | 必填 | 说明                                                                                       |
| -------- | ------ | ------ | -------------------------------------------------------------------------------------------- |
| option | [AudioSaveOptions](#audiosaveoptions)     | 否   | audioPicker保存音频文件选项，若无此参数，则拉起audioPicker界面后需用户自行输入保存的文件名 |

**返回值：**

| 类型                   | 说明                                               |
| ------------------------ | ---------------------------------------------------- |
| Promise<Array\<string\>> | Promise对象。返回audioPicker保存音频文件后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example16(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let audioSaveOptions = new picker.AudioSaveOptions();
    audioSaveOptions.newFileNames = ['AudioViewPicker01.mp3'];
    let audioPicker = new picker.AudioViewPicker(context);
    audioPicker.save(audioSaveOptions).then((audioSaveResult: Array<string>) => {
      console.info('AudioViewPicker.save successfully, audioSaveResult uri: ' + JSON.stringify(audioSaveResult))
    }).catch((err: BusinessError) => {
      console.error('AudioViewPicker.save failed with err: ' + JSON.stringify(err));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('AudioViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### save

save(option: AudioSaveOptions, callback: AsyncCallback<Array\<string\>>): void

通过保存模式拉起audioPicker界面（目前拉起的是documentPicker，audioPicker在规划中），用户可以保存一个或多个音频文件。接口采用callback异步返回形式，传入参数AudioSaveOptions对象，返回保存音频文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。（仅支持安卓跨平台）

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                           |
| ---------- | ------------------------------ | ------ | ------------------------------------------------ |
| option   | [AudioSaveOptions](#audiosaveoptions)                             | 是   | audioPicker保存音频文件选项                    |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回audioPicker保存音频文件后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example17(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let audioSaveOptions = new picker.AudioSaveOptions();
    audioSaveOptions.newFileNames = ['AudioViewPicker02.mp3'];
    let audioPicker = new picker.AudioViewPicker(context);
    audioPicker.save(audioSaveOptions, (err: BusinessError, audioSaveResult: Array<string>) => {
      if (err) {
        console.error('AudioViewPicker.save failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('AudioViewPicker.save successfully, audioSaveResult uri: ' + JSON.stringify(audioSaveResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('AudioViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

### save

save(callback: AsyncCallback<Array\<string\>>): void

通过保存模式拉起audioPicker界面（目前拉起的是documentPicker，audioPicker在规划中），用户可以保存一个或多个音频文件。接口采用callback异步返回形式，返回保存音频文件的uri数组。

**注意**：此接口返回的uri数组的具体使用方式参见用户文件uri介绍中的[文档类uri的使用方式](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/file-management/user-file-uri-intro.md#文档类uri的使用方式)。（仅支持安卓跨平台）

**系统能力**：SystemCapability.FileManagement.UserFileService

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                           |
| ---------- | ------------------------------ | ------ | ------------------------------------------------ |
| callback | AsyncCallback<Array\<string\>> | 是   | callback 返回audioPicker保存音频文件后的结果集 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { common } from '@kit.AbilityKit';
import  { picker } from '@kit.CoreFileKit';
async function example18(context: common.Context) { // 需确保 context 由 UIAbilityContext 转换而来
  try {
    let audioPicker = new picker.AudioViewPicker(context);
    audioPicker.save((err: BusinessError, audioSaveResult: Array<string>) => {
      if (err) {
        console.error('AudioViewPicker.save failed with err: ' + JSON.stringify(err));
        return;
      }
      console.info('AudioViewPicker.save successfully, audioSaveResult uri: ' + JSON.stringify(audioSaveResult));
    });
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error('AudioViewPicker failed with err: ' + JSON.stringify(err));
  }
}
```

## DocumentSelectMode<sup>11+</sup>

枚举，picker选择的文档类型。

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力：** SystemCapability.FileManagement.UserFileService.FolderSelection

| 名称   | 值 | 说明       |
| -------- | ---- | ------------ |
| FILE   | 0  | 文件类型   |
| FOLDER | 1  | 文件夹类型 |

## DocumentSelectOptions

文档选择选项。

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力：** SystemCapability.FileManagement.UserFileService

| 名称               | 类型          | 必填 | 说明                                                                                                                                                                                                                                                                                                                                                                                                            |
| :------------------- | --------------- | ------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| maxSelectNumber<sup>10+</sup>    | number        | 否   | 选择文件最大个数，上限500，有效值范围1-500（选择目录仅对具有该系统能力的设备开放。且目录选择的最大个数为1）。默认值是1。**系统能力：** SystemCapability.FileManagement.UserFileService                                                                                                                                                                                                                          |
| defaultFilePathUri<sup>10+</sup> | string        | 否   | 指定选择的文件或者目录路径。（仅支持安卓跨平台）                                                                                                                                                                                                                                                                                                                                                                |
| fileSuffixFilters<sup>10+</sup>  | Array\<string\> | 否   | 选择文件的后缀类型，传入字符串数组，每一项代表一个后缀选项，每一项内部用"\|\"分为两部分，第一部分为描述，第二部分为过滤后缀。没有"\|\"则没有描述，该项整体是一个过滤后缀。每项过滤后缀可以存在多个后缀名，则每一个后缀名之间用英文逗号进行分隔，传入数组长度不能超过100。仅对具有该系统能力的设备开放。默认全部过滤，即显示所有文件。**系统能力：** SystemCapability.FileManagement.UserFileService |
| selectMode<sup>11+</sup>         | [DocumentSelectMode](#documentselectmode11)              | 否   | 支持选择的资源类型，比如：文件、文件夹和二者混合，仅对具有该系统能力的设备开放，默认值是文件类型。**系统能力：** SystemCapability.FileManagement.UserFileService.FolderSelection。（仅支持安卓跨平台）                                                                                                                                                                                                          |

## DocumentSaveOptions

文档保存选项。（仅支持安卓跨平台）

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力：** SystemCapability.FileManagement.UserFileService

| 名称               | 类型          | 必填 | 说明                                                                   |
| -------------------- | --------------- | ------ | ------------------------------------------------------------------------ |
| newFileNames       | Array\<string\> | 否   | 拉起documentPicker进行保存的文件名，若无此参数，则默认需要用户自行输入 |
| defaultFilePathUri<sup>10+</sup> | string        | 否   | 指定保存的文件或者目录路径                                             |

## AudioSelectOptions

音频选择选项。（仅支持安卓跨平台）

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力：** SystemCapability.FileManagement.UserFileService

| 名称            | 类型   | 必填 | 说明                                                  |
| :---------------- | -------- | ------ | ------------------------------------------------------- |
| maxSelectNumber<sup>12+</sup> | number | 否   | 选择文件最大个数，默认值为1，上限500，有效值范围1-500 |

## AudioSaveOptions

音频的保存选项。（仅支持安卓跨平台）

**原子化服务API：** 从API version 12开始，该接口支持在原子化服务中使用。

**系统能力：** SystemCapability.FileManagement.UserFileService

| 名称         | 类型          | 必填 | 说明                                                                        |
| -------------- | --------------- | ------ | ----------------------------------------------------------------------------- |
| newFileNames | Array\<string\> | 否   | 拉起audioPicker进行保存音频资源的文件名，若无此参数，则默认需要用户自行输入 |
