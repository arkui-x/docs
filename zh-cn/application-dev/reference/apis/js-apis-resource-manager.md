# @ohos.resourceManager (资源管理)

资源管理模块，根据当前configuration：语言、区域、横竖屏、Mcc（移动国家码）和Mnc（移动网络码）、Device capability（设备类型）、Density（分辨率）提供获取应用资源信息读取接口。

> **说明：**
>
> 本模块首批接口从API version 6开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 使用说明

从API Version9开始，Stage模型通过context获取resourceManager对象的方式后，可直接调用其内部获取资源的接口，无需再导入包。

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class EntryAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
        let context = this.context;
        let resourceManager = context.resourceManager;
    }
}
```

## Direction

用于表示设备屏幕方向。

**系统能力**：SystemCapability.Global.ResourceManager

| 名称                   | 值  | 说明   |
| -------------------- | ---- | ---- |
| DIRECTION_VERTICAL   | 0    | 竖屏   |
| DIRECTION_HORIZONTAL | 1    | 横屏   |


## DeviceType

用于表示当前设备类型。

**系统能力**：SystemCapability.Global.ResourceManager

| 名称                   | 值  | 说明   |
| -------------------- | ---- | ---- |
| DEVICE_TYPE_PHONE    | 0x00 | 手机   |
| DEVICE_TYPE_TABLET   | 0x01 | 平板   |
| DEVICE_TYPE_CAR      | 0x02 | 汽车   |
| DEVICE_TYPE_PC       | 0x03 | 电脑   |
| DEVICE_TYPE_TV       | 0x04 | 电视   |
| DEVICE_TYPE_WEARABLE | 0x06 | 穿戴   |


## ScreenDensity

用于表示当前设备屏幕密度。

**系统能力**：SystemCapability.Global.ResourceManager

| 名称             | 值  | 说明         |
| -------------- | ---- | ---------- |
| SCREEN_SDPI    | 120  | 小规模的屏幕密度   |
| SCREEN_MDPI    | 160  | 中规模的屏幕密度   |
| SCREEN_LDPI    | 240  | 大规模的屏幕密度   |
| SCREEN_XLDPI   | 320  | 特大规模的屏幕密度  |
| SCREEN_XXLDPI  | 480  | 超大规模的屏幕密度  |
| SCREEN_XXXLDPI | 640  | 超特大规模的屏幕密度 |


## Configuration

表示当前设备的状态。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 名称        | 类型                    | 可读   | 可写   | 说明       |
| --------- | ----------------------- | ---- | ---- | -------- |
| direction | [Direction](#direction) | 是    | 否    | 当前设备屏幕方向 |
| locale    | string                  | 是    | 否    | 当前系统语言   |

**示例：**

  ```js
  this.context.resourceManager.getConfiguration((error, value) => {
    let direction = value.direction;
    let locale = value.locale;
  });
  ```

## DeviceCapability

表示设备支持的能力。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：**

| 名称            | 类型                            | 可读   | 可写   | 说明       |
| ------------- | ------------------------------- | ---- | ---- | -------- |
| screenDensity | [ScreenDensity](#screendensity) | 是    | 否    | 当前设备屏幕密度 |
| deviceType    | [DeviceType](#devicetype)       | 是    | 否    | 当前设备类型   |

**示例：**

  ```js
  this.context.resourceManager.getDeviceCapability((error, value) => {
    let screenDensity = value.screenDensity;
    let deviceType = value.deviceType;
  });
  ```

## RawFileDescriptor<sup>8+</sup>

表示rawfile的descriptor信息。

**系统能力：** SystemCapability.Global.ResourceManager

**参数：**

| 名称     | 类型    | 可读   | 可写  | 说明           |
| ------ | ------  | ---- | ---- | ------------------ |
| fd     | number  | 是    | 否 | rawfile所在hap的文件描述符 |
| offset | number  | 是    | 否 | rawfile的起始偏移量      |
| length | number  | 是    | 否 | rawfile的文件长度       |

## Resource<sup>9+</sup>

表示的资源信息。

**系统能力：** 以下各项对应的系统能力均为SystemCapability.Global.ResourceManager

**参数：**

| 名称         | 类型     | 可读   | 可写  |说明          |
| ---------- | ------ | ----- | ----  | ---------------|
| bundleName | string | 是    | 否 | 应用的bundle名称 |
| moduleName | string | 是    | 否 | 应用的module名称 |
| id         | number | 是    | 否 | 资源的id值      |

## ResourceManager

提供访问应用资源的能力。

> **说明：**
>
> - ResourceManager涉及到的方法，仅限基于TS扩展的声明式开发范式使用。
>
> - 资源文件在工程的resources目录中定义，id可通过$r(资源地址).id的方式获取，例如$r('app.string.test').id。

### getStringValue<sup>9+</sup>

getStringValue(resId: number, callback: AsyncCallback&lt;string&gt;): void

用户获取指定资源ID对应的字符串，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明              |
| -------- | --------------------------- | ---- | --------------- |
| resId    | number                      | 是    | 资源ID值           |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the module resId invalid.             |
| 9001002  | If the resource not found by resId.      |
| 9001006  | If the resource re-ref too much.         |

**示例Stage：** 
  ```ts
    try {
        this.context.resourceManager.getStringValue($r('app.string.test').id, (error, value) => {
          if (error != null) {
              console.log("error is " + error);
          } else {
              let str = value;
          }
      });
    } catch (error) {
        console.error(`callback getStringValue failed, error code: ${error.code}, message: ${error.message}.`)
    }
  ```


### getStringValue<sup>9+</sup>

getStringValue(resId: number): Promise&lt;string&gt;

用户获取指定资源ID对应的字符串，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |

**返回值：**

| 类型                    | 说明          |
| --------------------- | ----------- |
| Promise&lt;string&gt; | 资源ID值对应的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringValue($r('app.string.test').id).then(value => {
        let str = value;
    }).catch(error => {
        console.log("getStringValue promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getStringValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```


### getStringValue<sup>9+</sup>

getStringValue(resource: Resource, callback: AsyncCallback&lt;string&gt;): void

用户获取指定resource对象对应的字符串，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明              |
| -------- | --------------------------- | ---- | --------------- |
| resource | [Resource](#resource9)      | 是    | 资源信息            |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.string.test').id
  };
  try {
    this.context.resourceManager.getStringValue(resource, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let str = value;
        }
    });
  } catch (error) {
    console.error(`callback getStringValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  
  ```


### getStringValue<sup>9+</sup>

getStringValue(resource: Resource): Promise&lt;string&gt;

用户获取指定resource对象对应的字符串，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：**

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |

**返回值：**

| 类型                    | 说明               |
| --------------------- | ---------------- |
| Promise&lt;string&gt; | resource对象对应的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.string.test').id
  };
  try {
    this.context.resourceManager.getStringValue(resource).then(value => {
      let str = value;
    }).catch(error => {
      console.log("getStringValue promise error is " + error);
    });
  } catch (error) {
    console.error(`callback getStringValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```


### getStringArrayValue<sup>9+</sup>

getStringArrayValue(resId: number, callback: AsyncCallback&lt;Array&lt;string&gt;&gt;): void

用户获取指定资源ID对应的字符串数组，使用callback形式返回字符串数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                                       | 必填   | 说明                |
| -------- | ---------------------------------------- | ---- | ----------------- |
| resId    | number                                   | 是    | 资源ID值             |
| callback | AsyncCallback&lt;Array&lt;string&gt;&gt; | 是    | 异步回调，用于返回获取的字符串数组 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringArrayValue($r('app.strarray.test').id, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let strArray = value;
        }
    });
  } catch (error) {
    console.error(`callback getStringArrayValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```


### getStringArrayValue<sup>9+</sup>

getStringArrayValue(resId: number): Promise&lt;Array&lt;string&gt;&gt;

用户获取指定资源ID对应的字符串数组，使用Promise形式返回字符串数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |

**返回值：**

| 类型                                 | 说明            |
| ---------------------------------- | ------------- |
| Promise&lt;Array&lt;string&gt;&gt; | 资源ID值对应的字符串数组 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringArrayValue($r('app.strarray.test').id).then(value => {
        let strArray = value;
    }).catch(error => {
        console.log("getStringArrayValue promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getStringArrayValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringArrayValue<sup>9+</sup>

getStringArrayValue(resource: Resource, callback: AsyncCallback&lt;Array&lt;string&gt;&gt;): void

用户获取指定resource对象对应的字符串数组，使用callback形式返回回字符串数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                                       | 必填   | 说明                |
| -------- | ---------------------------------------- | ---- | ----------------- |
| resource | [Resource](#resource9)                   | 是    | 资源信息              |
| callback | AsyncCallback&lt;Array&lt;string&gt;&gt; | 是    | 异步回调，用于返回获取的字符串数组 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.strarray.test').id
  };
  try {
    this.context.resourceManager.getStringArrayValue(resource, (error, value) => {
      if (error != null) {
          console.log("error is " + error);
      } else {
          let strArray = value;
      }
    });
  } catch (error) {
    console.error(`callback getStringArrayValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringArrayValue<sup>9+</sup>

getStringArrayValue(resource: Resource): Promise&lt;Array&lt;string&gt;&gt;

用户获取指定resource对象对应的字符串数组，使用Promise形式返回字符串数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |

**返回值：**

| 类型                                 | 说明                 |
| ---------------------------------- | ------------------ |
| Promise&lt;Array&lt;string&gt;&gt; | resource对象对应的字符串数组 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.strarray.test').id
  };
  try {
    this.context.resourceManager.getStringArrayValue(resource).then(value => {
      let strArray = value;
    }).catch(error => {
        console.log("getStringArray promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getStringArrayValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>9+</sup>

getMediaContent(resId: number, callback: AsyncCallback&lt;Uint8Array&gt;): void

用户获取指定资源ID对应的媒体文件内容，使用callback形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                              | 必填   | 说明                 |
| -------- | ------------------------------- | ---- | ------------------ |
| resId    | number                          | 是    | 资源ID值              |
| callback | AsyncCallback&lt;Uint8Array&gt; | 是    | 异步回调，用于返回获取的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaContent($r('app.media.test').id, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>10+</sup>

getMediaContent(resId: number, density: number, callback: AsyncCallback&lt;Uint8Array&gt;): void

用户获取指定资源ID对应的指定屏幕密度媒体文件内容，使用callback形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                              | 必填   | 说明                 |
| -------- | ------------------------------- | ---- | ------------------ |
| resId    | number                          | 是    | 资源ID值              |
| [density](#screendensity)  | number                          | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |
| callback | AsyncCallback&lt;Uint8Array&gt; | 是    | 异步回调，用于返回获取的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaContent($r('app.media.test').id, 120, (error, value) => {
        if (error != null) {
            console.error(`callback getMediaContent failed, error code: ${error.code}, message: ${error.message}.`);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>9+</sup>

getMediaContent(resId: number): Promise&lt;Uint8Array&gt;

用户获取指定资源ID对应的媒体文件内容，使用Promise形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |

**返回值：**

| 类型                        | 说明             |
| ------------------------- | -------------- |
| Promise&lt;Uint8Array&gt; | 资源ID值对应的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
      this.context.resourceManager.getMediaContent($r('app.media.test').id).then(value => {
          let media = value;
      }).catch(error => {
          console.log("getMediaContent promise error is " + error);
      });
  } catch (error) {
    console.error(`promise getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>10+</sup>

getMediaContent(resId: number, density: number): Promise&lt;Uint8Array&gt;

用户获取指定资源ID对应的指定屏幕密度媒体文件内容，使用Promise形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |
| [density](#screendensity)  | number                          | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |

**返回值：**

| 类型                        | 说明             |
| ------------------------- | -------------- |
| Promise&lt;Uint8Array&gt; | 资源ID值对应的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
      this.context.resourceManager.getMediaContent($r('app.media.test').id, 120).then(value => {
          let media = value;
      }).catch(error => {
          console.error(`promise getMediaContent failed, error code: ${error.code}, message: ${error.message}.`);
      });
  } catch (error) {
    console.error(`promise getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>9+</sup>

getMediaContent(resource: Resource, callback: AsyncCallback&lt;Uint8Array&gt;): void

用户获取指定resource对象对应的媒体文件内容，使用callback形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                              | 必填   | 说明                 |
| -------- | ------------------------------- | ---- | ------------------ |
| resource | [Resource](#resource9)          | 是    | 资源信息               |
| callback | AsyncCallback&lt;Uint8Array&gt; | 是    | 异步回调，用于返回获取的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContent(resource, (error, value) => {
        if (error != null) {
          console.log("error is " + error);
        } else {
          let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>10+</sup>

getMediaContent(resource: Resource, density: number, callback: AsyncCallback&lt;Uint8Array&gt;): void

用户获取指定resource对象对应的指定屏幕密度媒体文件内容，使用callback形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                              | 必填   | 说明                 |
| -------- | ------------------------------- | ---- | ------------------ |
| resource | [Resource](#resource9)          | 是    | 资源信息               |
| [density](#screendensity)  | number        | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |
| callback | AsyncCallback&lt;Uint8Array&gt; | 是    | 异步回调，用于返回获取的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContent(resource, 120, (error, value) => {
        if (error != null) {
          console.error(`callback getMediaContent failed, error code: ${error.code}, message: ${error.message}.`);
        } else {
          let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>9+</sup>

getMediaContent(resource: Resource): Promise&lt;Uint8Array&gt;

用户获取指定resource对象对应的媒体文件内容，使用Promise形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |

**返回值：**

| 类型                        | 说明                  |
| ------------------------- | ------------------- |
| Promise&lt;Uint8Array&gt; | resource对象对应的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContent(resource).then(value => {
      let media = value;
    }).catch(error => {
      console.log("getMediaContent promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContent<sup>10+</sup>

getMediaContent(resource: Resource, density: number): Promise&lt;Uint8Array&gt;

用户获取指定resource对象对应的指定屏幕密度媒体文件内容，使用Promise形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |
| [density](#screendensity)  | number                          | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |

**返回值：**

| 类型                        | 说明                  |
| ------------------------- | ------------------- |
| Promise&lt;Uint8Array&gt; | resource对象对应的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContent(resource, 120).then(value => {
      let media = value;
    }).catch(error => {
      console.error(`promise getMediaContent failed, error code: ${error.code}, message: ${error.message}.`);
    });
  } catch (error) {
    console.error(`promise getMediaContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>9+</sup>

getMediaContentBase64(resId: number, callback: AsyncCallback&lt;string&gt;): void

用户获取指定资源ID对应的图片资源Base64编码，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明                       |
| -------- | --------------------------- | ---- | ------------------------ |
| resId    | number                      | 是    | 资源ID值                    |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaContentBase64($r('app.media.test').id, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let media = value;
        }
    });       
  } catch (error) {
    console.error(`callback getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>10+</sup>

getMediaContentBase64(resId: number, density: number, callback: AsyncCallback&lt;string&gt;): void

用户获取指定资源ID对应的指定屏幕密度图片资源Base64编码，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明                       |
| -------- | --------------------------- | ---- | ------------------------ |
| resId    | number                      | 是    | 资源ID值                    |
| [density](#screendensity)  | number        | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaContentBase64($r('app.media.test').id, 120, (error, value) => {
        if (error != null) {
            console.error(`callback getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`);
        } else {
            let media = value;
        }
    });       
  } catch (error) {
    console.error(`callback getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>9+</sup>

getMediaContentBase64(resId: number): Promise&lt;string&gt;

用户获取指定资源ID对应的图片资源Base64编码，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |

**返回值：**

| 类型                    | 说明                   |
| --------------------- | -------------------- |
| Promise&lt;string&gt; | 资源ID值对应的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaContentBase64($r('app.media.test').id).then(value => {
        let media = value;
    }).catch(error => {
        console.log("getMediaContentBase64 promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>10+</sup>

getMediaContentBase64(resId: number, density: number): Promise&lt;string&gt;

用户获取指定资源ID对应的指定屏幕密度图片资源Base64编码，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |
| [density](#screendensity)  | number                          | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |

**返回值：**

| 类型                    | 说明                   |
| --------------------- | -------------------- |
| Promise&lt;string&gt; | 资源ID值对应的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaContentBase64($r('app.media.test').id, 120).then(value => {
        let media = value;
    }).catch(error => {
        console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`);
    });
  } catch (error) {
    console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>9+</sup>

getMediaContentBase64(resource: Resource, callback: AsyncCallback&lt;string&gt;): void

用户获取指定resource对象对应的图片资源Base64编码，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：**

| 参数名      | 类型                          | 必填   | 说明                       |
| -------- | --------------------------- | ---- | ------------------------ |
| resource | [Resource](#resource9)      | 是    | 资源信息                     |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContentBase64(resource, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>10+</sup>

getMediaContentBase64(resource: Resource, density: number, callback: AsyncCallback&lt;string&gt;): void

用户获取指定resource对象对应的指定屏幕密度图片资源Base64编码，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：**

| 参数名      | 类型                          | 必填   | 说明                       |
| -------- | --------------------------- | ---- | ------------------------ |
| resource | [Resource](#resource9)      | 是    | 资源信息                     |
| [density](#screendensity)  | number        | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContentBase64(resource, 120, (error, value) => {
        if (error != null) {
            console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>9+</sup>

getMediaContentBase64(resource: Resource): Promise&lt;string&gt;

用户获取指定resource对象对应的图片资源Base64编码，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |

**返回值：**

| 类型                    | 说明                        |
| --------------------- | ------------------------- |
| Promise&lt;string&gt; | resource对象对应的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContentBase64(resource).then(value => {
        let media = value;
    }).catch(error => {
        console.log("getMediaContentBase64 promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaContentBase64<sup>10+</sup>

getMediaContentBase64(resource: Resource, density: number): Promise&lt;string&gt;

用户获取指定resource对象对应的指定屏幕密度图片资源Base64编码，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |
| [density](#screendensity)  | number                          | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |

**返回值：**

| 类型                    | 说明                        |
| --------------------- | ------------------------- |
| Promise&lt;string&gt; | resource对象对应的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.media.test').id
  };
  try {
    this.context.resourceManager.getMediaContentBase64(resource, 120).then(value => {
        let media = value;
    }).catch(error => {
        console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`);
    });
  } catch (error) {
    console.error(`promise getMediaContentBase64 failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getConfiguration

getConfiguration(callback: AsyncCallback&lt;Configuration&gt;): void

用户获取设备的Configuration，使用callback形式返回Configuration对象。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                                       | 必填   | 说明                        |
| -------- | ---------------------------------------- | ---- | ------------------------- |
| callback | AsyncCallback&lt;[Configuration](#configuration)&gt; | 是    | 异步回调，用于返回设备的Configuration |

**示例：** 
  ```ts
  resourceManager.getResourceManager((error, mgr) => {
      mgr.getConfiguration((error, value) => {
          if (error != null) {
              console.log("error is " + error);
          } else {
              let direction = value.direction;
              let locale = value.locale;
          }
      });
  });
  ```


### getConfiguration

getConfiguration(): Promise&lt;Configuration&gt;

用户获取设备的Configuration，使用Promise形式返回Configuration对象。

**系统能力**：SystemCapability.Global.ResourceManager

**返回值：**

| 类型                                       | 说明               |
| ---------------------------------------- | ---------------- |
| Promise&lt;[Configuration](#configuration)&gt; | 设备的Configuration |

**示例：** 
  ```ts
  resourceManager.getResourceManager((error, mgr) => {
      mgr.getConfiguration().then(value => {
          let direction = value.direction;
          let locale = value.locale;
      }).catch(error => {
          console.log("getConfiguration promise error is " + error);
      });
  });
  ```


### getDeviceCapability

getDeviceCapability(callback: AsyncCallback&lt;DeviceCapability&gt;): void

用户获取设备的DeviceCapability，使用callback形式返回DeviceCapability对象。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                                       | 必填   | 说明                           |
| -------- | ---------------------------------------- | ---- | ---------------------------- |
| callback | AsyncCallback&lt;[DeviceCapability](#devicecapability)&gt; | 是    | 异步回调，用于返回设备的DeviceCapability |

**示例：** 
  ```ts
  resourceManager.getResourceManager((error, mgr) => {
      mgr.getDeviceCapability((error, value) => {
          if (error != null) {
              console.log("error is " + error);
          } else {
              let screenDensity = value.screenDensity;
              let deviceType = value.deviceType;
          }
      });
  });
  ```


### getDeviceCapability

getDeviceCapability(): Promise&lt;DeviceCapability&gt;

用户获取设备的DeviceCapability，使用Promise形式返回DeviceCapability对象。

**系统能力**：SystemCapability.Global.ResourceManager

**返回值：**

| 类型                                       | 说明                  |
| ---------------------------------------- | ------------------- |
| Promise&lt;[DeviceCapability](#devicecapability)&gt; | 设备的DeviceCapability |

**示例：** 
  ```ts
  resourceManager.getResourceManager((error, mgr) => {
      mgr.getDeviceCapability().then(value => {
          let screenDensity = value.screenDensity;
          let deviceType = value.deviceType;
      }).catch(error => {
          console.log("getDeviceCapability promise error is " + error);
      });
  });
  ```


### getPluralStringValue<sup>9+</sup>

getPluralStringValue(resId: number, num: number, callback: AsyncCallback&lt;string&gt;): void

根据指定数量获取指定ID字符串表示的单复数字符串，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明                              |
| -------- | --------------------------- | ---- | ------------------------------- |
| resId    | number                      | 是    | 资源ID值                           |
| num      | number                      | 是    | 数量值                             |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，返回根据指定数量获取指定ID字符串表示的单复数字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getPluralStringValue($r("app.plural.test").id, 1, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let str = value;
        }
    });
  } catch (error) {
    console.error(`callback getPluralStringValue failed, error code: ${error.code}, message: ${error.message}.`)
  }   
  ```


### getPluralStringValue<sup>9+</sup>

getPluralStringValue(resId: number, num: number): Promise&lt;string&gt;

根据指定数量获取对指定ID字符串表示的单复数字符串，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |
| num   | number | 是    | 数量值   |

**返回值：**

| 类型                    | 说明                        |
| --------------------- | ------------------------- |
| Promise&lt;string&gt; | 根据提供的数量获取对应ID字符串表示的单复数字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getPluralStringValue($r("app.plural.test").id, 1).then(value => {
        let str = value;
    }).catch(error => {
        console.log("getPluralStringValue promise error is " + error);
    });
  } catch (error) {
    console.error(`callback getPluralStringValue failed, error code: ${error.code}, message: ${error.message}.`)
  }  
  ```

### getPluralStringValue<sup>9+</sup>

getPluralStringValue(resource: Resource, num: number, callback: AsyncCallback&lt;string&gt;): void

根据指定数量获取指定resource对象表示的单复数字符串，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明                                   |
| -------- | --------------------------- | ---- | ------------------------------------ |
| resource | [Resource](#resource9)      | 是    | 资源信息                                 |
| num      | number                      | 是    | 数量值                                  |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，返回根据指定数量获取指定resource对象表示的单复数字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.plural.test').id
  };
  try {
    this.context.resourceManager.getPluralStringValue(resource, 1, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let str = value;
        }
    });
  } catch (error) {
    console.error(`callback getPluralStringValue failed, error code: ${error.code}, message: ${error.message}.`)
  }  
  
  ```

### getPluralStringValue<sup>9+</sup>

getPluralStringValue(resource: Resource, num: number): Promise&lt;string&gt;

根据指定数量获取对指定resource对象表示的单复数字符串，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |
| num      | number                 | 是    | 数量值  |

**返回值：**

| 类型                    | 说明                             |
| --------------------- | ------------------------------ |
| Promise&lt;string&gt; | 根据提供的数量获取对应resource对象表示的单复数字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.plural.test').id
  };
  try {
    this.context.resourceManager.getPluralStringValue(resource, 1).then(value => {
        let str = value;
    }).catch(error => {
        console.log("getPluralStringValue promise error is " + error);
    });
  } catch (error) {
    console.error(`callback getPluralStringValue failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```


### getRawFileContent<sup>9+</sup>

getRawFileContent(path: string, callback: AsyncCallback&lt;Uint8Array&gt;): void

用户获取resources/rawfile目录下对应的rawfile文件内容，使用callback形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                              | 必填   | 说明                      |
| -------- | ------------------------------- | ---- | ----------------------- |
| path     | string                          | 是    | rawfile文件路径             |
| callback | AsyncCallback&lt;Uint8Array&gt; | 是    | 异步回调，用于返回获取的rawfile文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001005  | If the resource not found by path.          |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getRawFileContent("test.xml", (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let rawFile = value;
        }
    });
  } catch (error) {
    console.error(`callback getRawFileContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
      
  ```

### getRawFileContent<sup>9+</sup>

getRawFileContent(path: string): Promise&lt;Uint8Array&gt;

用户获取resources/rawfile目录下对应的rawfile文件内容，使用Promise形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名  | 类型     | 必填   | 说明          |
| ---- | ------ | ---- | ----------- |
| path | string | 是    | rawfile文件路径 |

**返回值：**

| 类型                        | 说明          |
| ------------------------- | ----------- |
| Promise&lt;Uint8Array&gt; | rawfile文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001005  | If the resource not found by path.          |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getRawFileContent("test.xml").then(value => {
        let rawFile = value;
    }).catch(error => {
        console.log("getRawFileContent promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getRawFileContent failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```


### getRawFd<sup>9+</sup>

getRawFd(path: string, callback: AsyncCallback&lt;RawFileDescriptor&gt;): void

用户获取resources/rawfile目录下对应rawfile文件的descriptor，使用callback形式返回。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                                       | 必填   | 说明                               |
| -------- | ---------------------------------------- | ---- | -------------------------------- |
| path     | string                                   | 是    | rawfile文件路径                      |
| callback | AsyncCallback&lt;[RawFileDescriptor](#rawfiledescriptor8)&gt; | 是    | 异步回调，用于返回获取的rawfile文件的descriptor |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001005  | If the resource not found by path.          |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getRawFd("test.xml", (error, value) => {
        if (error != null) {
            console.log(`callback getRawFd failed error code: ${error.code}, message: ${error.message}.`);
        } else {
            let fd = value.fd;
            let offset = value.offset;
            let length = value.length;
        }
    });
  } catch (error) {
      console.error(`callback getRawFd failed, error code: ${error.code}, message: ${error.message}.`)
  };
  ```

### getRawFd<sup>9+</sup>

getRawFd(path: string): Promise&lt;RawFileDescriptor&gt;

用户获取resources/rawfile目录下对应rawfile文件的descriptor，使用Promise形式返回。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名  | 类型     | 必填   | 说明          |
| ---- | ------ | ---- | ----------- |
| path | string | 是    | rawfile文件路径 |

**返回值：**

| 类型                                       | 说明                  |
| ---------------------------------------- | ------------------- |
| Promise&lt;[RawFileDescriptor](#rawfiledescriptor8)&gt; | rawfile文件descriptor |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001005  | If the resource not found by path.          |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getRawFd("test.xml").then(value => {
        let fd = value.fd;
        let offset = value.offset;
        let length = value.length;
    }).catch(error => {
        console.log(`promise getRawFd error error code: ${error.code}, message: ${error.message}.`);
    });
  } catch (error) {
    console.error(`promise getRawFd failed, error code: ${error.code}, message: ${error.message}.`);
  };
  ```

### closeRawFileDescriptor<sup>8+</sup>

closeRawFileDescriptor(path: string, callback: AsyncCallback&lt;void&gt;): void

用户关闭resources/rawfile目录下rawfile文件的descriptor，使用callback形式返回。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                        | 必填   | 说明          |
| -------- | ------------------------- | ---- | ----------- |
| path     | string                    | 是    | rawfile文件路径 |
| callback | AsyncCallback&lt;void&gt; | 是    | 异步回调        |

**示例：** 
  ```ts
  resourceManager.getResourceManager((error, mgr) => {
      mgr.closeRawFileDescriptor("test.xml", (error, value) => {
          if (error != null) {
              console.log("error is " + error);
          }
      });
  });
  ```

### closeRawFileDescriptor<sup>8+</sup>

closeRawFileDescriptor(path: string): Promise&lt;void&gt;

用户关闭resources/rawfile目录下rawfile文件的descriptor，使用Promise形式返回。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名  | 类型     | 必填   | 说明          |
| ---- | ------ | ---- | ----------- |
| path | string | 是    | rawfile文件路径 |

**返回值：**

| 类型                  | 说明   |
| ------------------- | ---- |
| Promise&lt;void&gt; | 无返回值 |

**示例：** 
  ```ts
  resourceManager.getResourceManager((error, mgr) => {
      mgr.closeRawFileDescriptor("test.xml").then(value => {
          let result = value;
      }).catch(error => {
          console.log("closeRawFileDescriptor promise error is " + error);
      });
  });
  ```


### closeRawFd<sup>9+</sup>

closeRawFd(path: string, callback: AsyncCallback&lt;void&gt;): void

用户关闭resources/rawfile目录下rawfile文件的descriptor，使用callback形式返回。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                        | 必填   | 说明          |
| -------- | ------------------------- | ---- | ----------- |
| path     | string                    | 是    | rawfile文件路径 |
| callback | AsyncCallback&lt;void&gt; | 是    | 异步回调        |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001005  | The resource not found by path.          |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.closeRawFd("test.xml", (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        }
    });
  } catch (error) {
    console.error(`callback closeRawFd failed, error code: ${error.code}, message: ${error.message}.`)
  }
      
  ```

### closeRawFd<sup>9+</sup>

closeRawFd(path: string): Promise&lt;void&gt;

用户关闭resources/rawfile目录下rawfile文件的descriptor，使用Promise形式返回。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名  | 类型     | 必填   | 说明          |
| ---- | ------ | ---- | ----------- |
| path | string | 是    | rawfile文件路径 |

**返回值：**

| 类型                  | 说明   |
| ------------------- | ---- |
| Promise&lt;void&gt; | 无返回值 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001005  | If the resource not found by path.          |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.closeRawFd("test.xml").then(value => {
        let result = value;
    }).catch(error => {
        console.log("closeRawFd promise error is " + error);
    });
  } catch (error) {
    console.error(`promise closeRawFd failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### release<sup>7+</sup>

release()

用户释放创建的resourceManager。

**系统能力**：SystemCapability.Global.ResourceManager

**示例：** 
  ```ts
  resourceManager.getResourceManager((error, mgr) => {
      mgr.release();
  });
  ```

### getStringByName<sup>9+</sup>

getStringByName(resName: string, callback: AsyncCallback&lt;string&gt;): void

用户获取指定资源名称对应的字符串，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：**

| 参数名      | 类型                          | 必填   | 说明              |
| -------- | --------------------------- | ---- | --------------- |
| resName  | string                      | 是    | 资源名称            |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：**
  ```ts
  try {
    this.context.resourceManager.getStringByName("test", (error, value) => {
        if (error != null) {
             console.log("error is " + error);
        } else {
            let string = value;
        }
    });
  } catch (error) {
    console.error(`callback getStringByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  
  ```

### getStringByName<sup>9+</sup>

getStringByName(resName: string): Promise&lt;string&gt;

用户获取指定资源名称对应的字符串，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：**

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |

**返回值：**

| 类型                    | 说明         |
| --------------------- | ---------- |
| Promise&lt;string&gt; | 资源名称对应的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：**
  ```ts
  try {
    this.context.resourceManager.getStringByName("test").then(value => {
        let string = value;
    }).catch(error => {
        console.log("getStringByName promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getStringByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringArrayByName<sup>9+</sup>

getStringArrayByName(resName: string, callback: AsyncCallback&lt;Array&lt;string&gt;&gt;): void

用户获取指定资源名称对应的字符串数组，使用callback形式返回字符串数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                |
| -------- | ---------------------------------------- | ---- | ----------------- |
| resName  | string                                   | 是    | 资源名称              |
| callback | AsyncCallback&lt;Array&lt;string&gt;&gt; | 是    | 异步回调，用于返回获取的字符串数组 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringArrayByName("test", (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let strArray = value;
        }
    });
  } catch (error) {
    console.error(`callback getStringArrayByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringArrayByName<sup>9+</sup>

getStringArrayByName(resName: string): Promise&lt;Array&lt;string&gt;&gt;

用户获取指定资源名称对应的字符串数组，使用Promise形式返回字符串数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |

**返回值：**

| 类型                                 | 说明           |
| ---------------------------------- | ------------ |
| Promise&lt;Array&lt;string&gt;&gt; | 资源名称对应的字符串数组 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringArrayByName("test").then(value => {
        let strArray = value;
    }).catch(error => {
        console.log("getStringArrayByName promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getStringArrayByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaByName<sup>9+</sup>

getMediaByName(resName: string, callback: AsyncCallback&lt;Uint8Array&gt;): void

用户获取指定资源ID对应的媒体文件内容，使用callback形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                              | 必填   | 说明                 |
| -------- | ------------------------------- | ---- | ------------------ |
| resName  | string                          | 是    | 资源名称               |
| callback | AsyncCallback&lt;Uint8Array&gt; | 是    | 异步回调，用于返回获取的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaByName("test", (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaByName<sup>10+</sup>

getMediaByName(resName: string, density: number, callback: AsyncCallback&lt;Uint8Array&gt;): void

用户获取指定资源ID对应的指定屏幕密度媒体文件内容，使用callback形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                              | 必填   | 说明                 |
| -------- | ------------------------------- | ---- | ------------------ |
| resName  | string                          | 是    | 资源名称               |
| [density](#screendensity)  | number        | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |
| callback | AsyncCallback&lt;Uint8Array&gt; | 是    | 异步回调，用于返回获取的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaByName("test", 120, (error, value) => {
        if (error != null) {
            console.error(`callback getMediaByName failed, error code: ${error.code}, message: ${error.message}.`);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaByName<sup>9+</sup>

getMediaByName(resName: string): Promise&lt;Uint8Array&gt;

用户获取指定资源名称对应的媒体文件内容，使用Promise形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |

**返回值：**

| 类型                        | 说明            |
| ------------------------- | ------------- |
| Promise&lt;Uint8Array&gt; | 资源名称对应的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaByName("test").then(value => {
        let media = value;
    }).catch(error => {
        console.log("getMediaByName promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getMediaByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaByName<sup>10+</sup>

getMediaByName(resName: string, density: number): Promise&lt;Uint8Array&gt;

用户获取指定资源名称对应的指定屏幕密度媒体文件内容，使用Promise形式返回字节数组。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |
| [density](#screendensity)  | number                          | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |

**返回值：**

| 类型                        | 说明            |
| ------------------------- | ------------- |
| Promise&lt;Uint8Array&gt; | 资源名称对应的媒体文件内容 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaByName("test", 120).then(value => {
        let media = value;
    }).catch(error => {
        console.error(`promise getMediaByName failed, error code: ${error.code}, message: ${error.message}.`);
    });
  } catch (error) {
    console.error(`promise getMediaByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaBase64ByName<sup>9+</sup>

getMediaBase64ByName(resName: string, callback: AsyncCallback&lt;string&gt;): void

用户获取指定资源名称对应的图片资源Base64编码，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明                       |
| -------- | --------------------------- | ---- | ------------------------ |
| resName  | string                      | 是    | 资源名称                     |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaBase64ByName("test", (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaBase64ByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaBase64ByName<sup>10+</sup>

getMediaBase64ByName(resName: string, density: number, callback: AsyncCallback&lt;string&gt;): void

用户获取指定资源名称对应的指定屏幕密度图片资源Base64编码，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明                       |
| -------- | --------------------------- | ---- | ------------------------ |
| resName  | string                      | 是    | 资源名称                     |
| [density](#screendensity)  | number        | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，用于返回获取的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaBase64ByName("test", 120, (error, value) => {
        if (error != null) {
            console.error(`callback getMediaBase64ByName failed, error code: ${error.code}, message: ${error.message}.`);
        } else {
            let media = value;
        }
    });
  } catch (error) {
    console.error(`callback getMediaBase64ByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaBase64ByName<sup>9+</sup>

getMediaBase64ByName(resName: string): Promise&lt;string&gt;

用户获取指定资源名称对应的图片资源Base64编码，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |

**返回值：**

| 类型                    | 说明                  |
| --------------------- | ------------------- |
| Promise&lt;string&gt; | 资源名称对应的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaBase64ByName("test").then(value => {
        let media = value;
    }).catch(error => {
        console.log("getMediaBase64ByName promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getMediaBase64ByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getMediaBase64ByName<sup>10+</sup>

getMediaBase64ByName(resName: string, density: number): Promise&lt;string&gt;

用户获取指定资源名称对应的指定屏幕密度图片资源Base64编码，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |
| [density](#screendensity)  | number                          | 是    | 资源获取需要的屏幕密度，0表示默认屏幕密度    |

**返回值：**

| 类型                    | 说明                  |
| --------------------- | ------------------- |
| Promise&lt;string&gt; | 资源名称对应的图片资源Base64编码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getMediaBase64ByName("test", 120).then(value => {
        let media = value;
    }).catch(error => {
        console.error(`promise getMediaBase64ByName failed, error code: ${error.code}, message: ${error.message}.`);
    });
  } catch (error) {
    console.error(`promise getMediaBase64ByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getPluralStringByName<sup>9+</sup>

getPluralStringByName(resName: string, num: number, callback: AsyncCallback&lt;string&gt;): void

根据传入的数量值，获取资源名称对应的字符串资源，使用callback形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                          | 必填   | 说明                            |
| -------- | --------------------------- | ---- | ----------------------------- |
| resName  | string                      | 是    | 资源名称                          |
| num      | number                      | 是    | 数量值                           |
| callback | AsyncCallback&lt;string&gt; | 是    | 异步回调，返回根据传入的数量值获取资源名称对应的字符串资源 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getPluralStringByName("test", 1, (error, value) => {
        if (error != null) {
            console.log("error is " + error);
        } else {
            let str = value;
        }
    });
  } catch (error) {
    console.error(`callback getPluralStringByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  
  ```

### getPluralStringByName<sup>9+</sup>

getPluralStringByName(resName: string, num: number): Promise&lt;string&gt;

根据传入的数量值，获取资源名称对应的字符串资源，使用Promise形式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |
| num     | number | 是    | 数量值  |

**返回值：**

| 类型                    | 说明                     |
| --------------------- | ---------------------- |
| Promise&lt;string&gt; | 根据传入的数量值获取资源名称对应的字符串资源 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getPluralStringByName("test", 1).then(value => {
      let str = value;
    }).catch(error => {
      console.log("getPluralStringByName promise error is " + error);
    });
  } catch (error) {
    console.error(`promise getPluralStringByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringSync<sup>9+</sup>

getStringSync(resId: number): string

用户获取指定资源ID对应的字符串，使用同步方式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |

**返回值：**

| 类型     | 说明          |
| ------ | ----------- |
| string | 资源ID值对应的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringSync($r('app.string.test').id);
  } catch (error) {
    console.error(`getStringSync failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringSync<sup>10+</sup>

getStringSync(resId: number, ...args: Array<string | number>): string

用户获取指定资源ID对应的字符串，根据args参数进行格式化，使用同步方式返回相应字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |
| args | Array<string \| number> | 否    | 格式化字符串资源参数 <br> 支持参数类型：<br /> -%d、%f、%s、%% <br> 说明：%%转译符，转译%<br>举例：%%d格式化后为%d字符串|

**返回值：**

| 类型     | 说明          |
| ------ | ---------------------------- |
| string | 资源ID值对应的格式化字符串|

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ----------------------------------------------- |
| 9001001  | If the resId invalid.                               |
| 9001002  | If the resource not found by resId.                 |
| 9001006  | If the resource re-ref too much.                    |
| 9001007  | If the resource obtained by resId formatting error. |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringSync($r('app.string.test').id, "format string", 10, 98.78);
  } catch (error) {
    console.error(`getStringSync failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringSync<sup>9+</sup>

getStringSync(resource: Resource): string

用户获取指定resource对象对应的字符串，使用同步方式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |

**返回值：**

| 类型     | 说明               |
| ------ | ---------------- |
| string | resource对象对应的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.string.test').id
  };
  try {
    this.context.resourceManager.getStringSync(resource);
  } catch (error) {
    console.error(`getStringSync failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringSync<sup>10+</sup>

getStringSync(resource: Resource, ...args: Array<string | number>): string

用户获取指定resource对象对应的字符串，根据args参数进行格式化，使用同步方式返回相应字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |
| args | Array<string \| number> | 否    | 格式化字符串资源参数 <br> 支持参数类型：<br /> -%d、%f、%s、%% <br> 说明：%%转译符，转译%<br>举例：%%d格式化后为%d字符串|

**返回值：**

| 类型     | 说明          |
| ------ | ---------------------------- |
| string | resource对象对应的格式化字符串|

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |
| 9001007  | If the resource obtained by resId formatting error. |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.string.test').id
  };
  try {
    this.context.resourceManager.getStringSync(resource, "format string", 10, 98.78);
  } catch (error) {
    console.error(`getStringSync failed, error code: ${error.code}, message: ${error.message}.`)
  }
 ```

### getStringByNameSync<sup>9+</sup>

getStringByNameSync(resName: string): string

用户获取指定资源名称对应的字符串，使用同步方式返回字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |

**返回值：**

| 类型     | 说明         |
| ------ | ---------- |
| string | 资源名称对应的字符串 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringByNameSync("test");
  } catch (error) {
    console.error(`getStringByNameSync failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getStringByNameSync<sup>10+</sup>

getStringByNameSync(resName: string, ...args: Array<string | number>): string

用户获取指定资源名称对应的字符串，根据args参数进行格式化，使用同步方式返回相应字符串。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |
| args | Array<string \| number> | 否    | 格式化字符串资源参数 <br> 支持参数类型：<br /> -%d、%f、%s、%% <br> 说明：%%转译符，转译%<br>举例：%%d格式化后为%d字符串|

**返回值：**

| 类型     | 说明          |
| ------ | ---------------------------- |
| string | 资源名称对应的格式化字符串|

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |
| 9001008  | If the resource obtained by resName formatting error. |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getStringByNameSync("test", "format string", 10, 98.78);
  } catch (error) {
    console.error(`getStringByNameSync failed, error code: ${error.code}, message: ${error.message}.`)
  }
 ```

### getBoolean<sup>9+</sup>

getBoolean(resId: number): boolean

使用同步方式，返回获取指定资源ID对应的布尔结果。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |

**返回值：**

| 类型      | 说明           |
| ------- | ------------ |
| boolean | 资源ID值对应的布尔结果 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getBoolean($r('app.boolean.boolean_test').id);
  } catch (error) {
    console.error(`getBoolean failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```
### getBoolean<sup>9+</sup>

getBoolean(resource: Resource): boolean

使用同步方式，返回获取指定resource对象对应的布尔结果。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |

**返回值：**

| 类型      | 说明                |
| ------- | ----------------- |
| boolean | resource对象对应的布尔结果 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.boolean.boolean_test').id
  };
  try {
    this.context.resourceManager.getBoolean(resource);
  } catch (error) {
    console.error(`getBoolean failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getBooleanByName<sup>9+</sup>

getBooleanByName(resName: string): boolean

使用同步方式，返回获取指定资源名称对应的布尔结果

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |

**返回值：**

| 类型      | 说明          |
| ------- | ----------- |
| boolean | 资源名称对应的布尔结果 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getBooleanByName("boolean_test");
  } catch (error) {
    console.error(`getBooleanByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getNumber<sup>9+</sup>

getNumber(resId: number): number

用户获取指定资源ID对应的integer数值或者float数值，使用同步方式返回资源对应的数值。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名   | 类型     | 必填   | 说明    |
| ----- | ------ | ---- | ----- |
| resId | number | 是    | 资源ID值 |

**返回值：**

| 类型     | 说明         |
| ------ | ---------- | 
| number | 资源ID值对应的数值。Integer对应的是原数值，float对应的是真实像素点值，具体参考示例代码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getNumber($r('app.integer.integer_test').id); // integer对应返回的是原数值
  } catch (error) {
    console.error(`getNumber failed, error code: ${error.code}, message: ${error.message}.`)
  }

  try {
    this.context.resourceManager.getNumber($r('app.float.float_test').id); // float对应返回的是真实像素点值
  } catch (error) {
    console.error(`getNumber failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getNumber<sup>9+</sup>

getNumber(resource: Resource): number

用户获取指定resource对象对应的integer数值或者float数值，使用同步方式返回资源对应的数值。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名      | 类型                     | 必填   | 说明   |
| -------- | ---------------------- | ---- | ---- |
| resource | [Resource](#resource9) | 是    | 资源信息 |

**返回值：**

| 类型     | 说明              |
| ------ | --------------- |
| number | resource对象对应的数值。Integer对应的是原数值，float对应的是真实像素点值, 具体参考示例代码 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001001  | If the resId invalid.                       |
| 9001002  | If the resource not found by resId.         |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  let resource = {
      bundleName: "com.example.myapplication",
      moduleName: "entry",
      id: $r('app.integer.integer_test').id
  };
  try {
    this.context.resourceManager.getNumber(resource);// integer对应返回的是原数值, float对应返回的是真实像素点值
  } catch (error) {
    console.error(`getNumber failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```

### getNumberByName<sup>9+</sup>

getNumberByName(resName: string): number

用户获取指定资源名称对应的integer数值或者float数值，使用同步方式资源对应的数值。

**系统能力**：SystemCapability.Global.ResourceManager

**参数：** 

| 参数名     | 类型     | 必填   | 说明   |
| ------- | ------ | ---- | ---- |
| resName | string | 是    | 资源名称 |

**返回值：**

| 类型     | 说明        |
| ------ | --------- |
| number | 资源名称对应的数值 |

**错误码：**

以下错误码的详细介绍请参见[资源管理错误码](../errorcodes/errorcode-resource-manager.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------------------- |
| 9001003  | If the resName invalid.                     |
| 9001004  | If the resource not found by resName.       |
| 9001006  | If the resource re-ref too much.            |

**示例：** 
  ```ts
  try {
    this.context.resourceManager.getNumberByName("integer_test");
  } catch (error) {
    console.error(`getNumberByName failed, error code: ${error.code}, message: ${error.message}.`)
  }

  try {
    this.context.resourceManager.getNumberByName("float_test");
  } catch (error) {
    console.error(`getNumberByName failed, error code: ${error.code}, message: ${error.message}.`)
  }
  ```