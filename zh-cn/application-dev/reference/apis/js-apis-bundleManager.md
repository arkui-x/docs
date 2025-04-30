# @ohos.bundle.bundleManager (bundleManager模块)

本模块提供应用信息查询能力。

> **说明：**
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import bundleManager from '@ohos.bundle.bundleManager';
```

## LaunchType

指示组件的启动方式。

 **系统能力：** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core。

| 名称 | 值 | 说明 | Android平台 | iOS平台 |
|:----------------:|:---:|:---:|------------------|------------------|
| SINGLETON        | 0   | ability的启动模式，表示单实例。 | 支持 | 支持 |
| MULTITON         | 1   | ability的启动模式，表示普通多实例。 | 支持 | 支持 |
| SPECIFIED<sup>20+</sup> | 2 | ability的启动模式，表示该ability内部根据业务自己指定多实例。 | 支持 | 支持 |

## BundleFlag<sup>20+</sup>

包信息标志，指示需要获取的包信息的内容。

 **系统能力：** SystemCapability.BundleManager.BundleFramework.Core

| 名称                                                    | 值         | 说明                                                         | Android平台 | iOS平台 |
| ------------------------------------------------------- | ---------- | ------------------------------------------------------------ | ----------- | ------- |
| GET_BUNDLE_INFO_DEFAULT<sup>20+</sup>                   | 0x00000000 | 用于获取默认bundleInfo，获取的bundleInfo不包含applicationInfo、hapModuleInfo、ability和permission的信息。 | 支持        | 支持    |
| GET_BUNDLE_INFO_WITH_APPLICATION<sup>20+</sup>          | 0x00000001 | 用于获取包含applicationInfo的bundleInfo，获取的bundleInfo不包含signatureInfo、hapModuleInfo、ability、extensionAbility和permission的信息。 | 支持        | 支持    |
| GET_BUNDLE_INFO_WITH_HAP_MODULE<sup>20+</sup>           | 0x00000002 | 用于获取包含hapModuleInfo的bundleInfo，获取的bundleInfo不包含signatureInfo、applicationInfo、ability、extensionAbility和permission的信息。 | 支持        | 支持    |
| GET_BUNDLE_INFO_WITH_ABILITY<sup>20+</sup>              | 0x00000004 | 用于获取包含ability的bundleInfo，获取的bundleInfo不包含signatureInfo、applicationInfo、extensionAbility和permission的信息。它不能单独使用，需要与GET_BUNDLE_INFO_WITH_HAP_MODULE一起使用。 | 支持        | 支持    |
| GET_BUNDLE_INFO_WITH_REQUESTED_PERMISSION<sup>20+</sup> | 0x00000010 | 用于获取包含permission的bundleInfo。获取的bundleInfo不包含signatureInfo、applicationInfo、hapModuleInfo、extensionAbility和ability的信息。 | 支持        | 支持    |
| GET_BUNDLE_INFO_WITH_METADATA<sup>20+</sup>             | 0x00000020 | 用于获取applicationInfo、moduleInfo和abilityInfo中包含的metadata。它不能单独使用，它需要与GET_BUNDLE_INFO_WITH_APPLICATION、GET_BUNDLE_INFO_WITH_HAP_MODULE、GET_BUNDLE_INFO_WITH_ABILITY、GET_BUNDLE_INFO_WITH_EXTENSION_ABILITY一起使用。 | 支持        | 支持    |
| GET_BUNDLE_INFO_WITH_DISABLE<sup>20+</sup>              | 0x00000040 | 用于获取application被禁用的BundleInfo和被禁用的Ability信息。获取的bundleInfo不包含signatureInfo、applicationInfo、hapModuleInfo、ability、extensionAbility和permission的信息。 | 支持        | 支持    |
| GET_BUNDLE_INFO_WITH_SIGNATURE_INFO<sup>20+</sup>       | 0x00000080 | 用于获取包含signatureInfo的bundleInfo。获取的bundleInfo不包含applicationInfo、hapModuleInfo、extensionAbility、ability和permission的信息。 | 支持        | 支持    |

## PermissionGrantState<sup>20+</sup>

指示权限授予状态。

 **系统能力：** SystemCapability.BundleManager.BundleFramework.Core

|               名称               |  值  |      说明      | Android平台 | iOS平台 |
| :------------------------------: | :--: | :------------: | ----------- | ------- |
| PERMISSION_DENIED<sup>20+</sup>  |  -1  | 拒绝授予权限。 | 支持        | 支持    |
| PERMISSION_GRANTED<sup>20+</sup> |  0   |   授予权限。   | 支持        | 支持    |

## SupportWindowMode<sup>20+</sup>

标识该组件所支持的窗口模式。

 **系统能力：** SystemCapability.BundleManager.BundleFramework.Core

|           名称            |  值  |        说明        | Android平台 | iOS平台 |
| :-----------------------: | :--: | :----------------: | ----------- | ------- |
| FULL_SCREEN<sup>20+</sup> |  0   | 窗口支持全屏显示。 | 支持        | 支持    |
|    SPLIT<sup>20+</sup>    |  1   | 窗口支持分屏显示。 | 支持        | 支持    |
|  FLOATING<sup>20+</sup>   |  2   |  支持窗口化显示。  | 支持        | 支持    |

## DisplayOrientation<sup>20+</sup>

标识该Ability的显示模式。该标签仅适用于page类型的Ability。

 **系统能力：** SystemCapability.BundleManager.BundleFramework.Core

| 名称                                             | 值   | 说明                               | Android平台 | iOS平台 |
| :----------------------------------------------- | ---- | ---------------------------------- | ----------- | ------- |
| UNSPECIFIED<sup>20+</sup>                        | 0    | 表示未定义方向模式，由系统判定。   | 支持        | 支持    |
| LANDSCAPE<sup>20+</sup>                          | 1    | 表示横屏显示模式。                 | 支持        | 支持    |
| PORTRAIT<sup>20+</sup>                           | 2    | 表示竖屏显示模式。                 | 支持        | 支持    |
| FOLLOW_RECENT<sup>20+</sup>                      | 3    | 表示跟随上一个显示模式。           | 支持        | 支持    |
| LANDSCAPE_INVERTED<sup>20+</sup>                 | 4    | 表示反向横屏显示模式。             | 支持        | 支持    |
| PORTRAIT_INVERTED<sup>20+</sup>                  | 5    | 表示反向竖屏显示模式。             | 支持        | 支持    |
| AUTO_ROTATION<sup>20+</sup>                      | 6    | 表示传感器自动旋转模式。           | 支持        | 支持    |
| AUTO_ROTATION_LANDSCAPE<sup>20+</sup>            | 7    | 表示传感器自动横向旋转模式。       | 支持        | 支持    |
| AUTO_ROTATION_PORTRAIT<sup>20+</sup>             | 8    | 表示传感器自动竖向旋转模式。       | 支持        | 支持    |
| AUTO_ROTATION_RESTRICTED<sup>20+</sup>           | 9    | 表示受开关控制的自动旋转模式。     | 支持        | 支持    |
| AUTO_ROTATION_LANDSCAPE_RESTRICTED<sup>20+</sup> | 10   | 表述受开关控制的自动横向旋转模式。 | 支持        | 支持    |
| AUTO_ROTATION_PORTRAIT_RESTRICTED<sup>20+</sup>  | 11   | 表示受开关控制的自动竖向旋转模式。 | 支持        | 支持    |
| LOCKED<sup>20+</sup>                             | 12   | 表示锁定模式。                     | 支持        | 支持    |

## ModuleType<sup>20+</sup>

标识模块类型。

 **系统能力:** SystemCapability.BundleManager.BundleFramework.Core

| 名称                  | 值   | 说明                   | Android平台 | iOS平台 |
| --------------------- | ---- | ---------------------- | ----------- | ------- |
| ENTRY<sup>20+</sup>   | 1    | 应用的主模块。         | 支持        | 支持    |
| FEATURE<sup>20+</sup> | 2    | 应用的动态特性模块。   | 支持        | 支持    |
| SHARED<sup>20+</sup>  | 3    | 应用的动态共享库模块。 | 支持        | 支持    |

## bundleManager.getBundleInfoForSelf<sup>20+</sup>

getBundleInfoForSelf(bundleFlags: number): Promise\<BundleInfo>

根据给定的bundleFlags获取当前应用的BundleInfo，使用Promise异步回调。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

**参数：**

| 参数名                                             | 类型   | 必填 | 说明                               |
| -------------------------------------------------- | ------ | ---- | ---------------------------------- |
| [bundleFlags](js-apis-bundleManager.md#bundleflag) | number | 是   | 指定返回的BundleInfo所包含的信息。 |

**返回值：**

| 类型                                                        | 说明                                    |
| ----------------------------------------------------------- | --------------------------------------- |
| Promise\<[BundleInfo](js-apis-bundleManager-bundleInfo.md)> | Promise对象，返回当前应用的BundleInfo。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
// 额外获取带有metadataArray信息的appInfo
import { bundleManager } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';
import { hilog } from '@kit.PerformanceAnalysisKit';

let bundleFlags = bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_APPLICATION | bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_METADATA;

try {
  bundleManager.getBundleInfoForSelf(bundleFlags).then((data) => {
    hilog.info(0x0000, 'testTag', 'getBundleInfoForSelf successfully. Data: %{public}s', JSON.stringify(data));
  }).catch((err: BusinessError) => {
    hilog.error(0x0000, 'testTag', 'getBundleInfoForSelf failed. Cause: %{public}s', err.message);
  });
} catch (err) {
  let message = (err as BusinessError).message;
  hilog.error(0x0000, 'testTag', 'getBundleInfoForSelf failed: %{public}s', message);
}
```

## bundleManager.getBundleInfoForSelf<sup>20+</sup>

getBundleInfoForSelf(bundleFlags: number, callback: AsyncCallback\<BundleInfo>): void

根据给定的bundleFlags获取当前应用的BundleInfo，使用callback异步回调。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

**参数：**

| 参数名                                             | 类型                                                         | 必填 | 说明                                                         |
| -------------------------------------------------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| [bundleFlags](js-apis-bundleManager.md#bundleflag) | number                                                       | 是   | 指定返回的BundleInfo所包含的信息。                           |
| callback                                           | AsyncCallback\<[BundleInfo](js-apis-bundleManager-bundleInfo.md)> | 是   | 回调函数，当获取成功时，err为null，data为获取到的当前应用的BundleInfo；否则为错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
// 额外获取带有permissions信息的abilitiesInfo
import { bundleManager } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';
import { hilog } from '@kit.PerformanceAnalysisKit';

let bundleFlags = bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_HAP_MODULE | bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_ABILITY | bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_REQUESTED_PERMISSION;

try {
  bundleManager.getBundleInfoForSelf(bundleFlags, (err, data) => {
    if (err) {
      hilog.error(0x0000, 'testTag', 'getBundleInfoForSelf failed: %{public}s', err.message);
    } else {
      hilog.info(0x0000, 'testTag', 'getBundleInfoForSelf successfully: %{public}s', JSON.stringify(data));
    }
  });
} catch (err) {
  let message = (err as BusinessError).message;
  hilog.error(0x0000, 'testTag', 'getBundleInfoForSelf failed: %{public}s', message);
}
```

## bundleManager.getBundleInfoForSelfSync<sup>20+</sup>

getBundleInfoForSelfSync(bundleFlags: number): BundleInfo

以同步方法根据给定的bundleFlags获取当前应用的BundleInfo。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

**参数：**

| 参数名                                             | 类型   | 必填 | 说明                               |
| -------------------------------------------------- | ------ | ---- | ---------------------------------- |
| [bundleFlags](js-apis-bundleManager.md#bundleflag) | number | 是   | 指定返回的BundleInfo所包含的信息。 |

**返回值：**

| 类型                                              | 说明                 |
| ------------------------------------------------- | -------------------- |
| [BundleInfo](js-apis-bundleManager-bundleInfo.md) | 返回BundleInfo对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |

**示例：**

```ts
import { bundleManager } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';
import { hilog } from '@kit.PerformanceAnalysisKit';

let bundleFlags = bundleManager.BundleFlag.GET_BUNDLE_INFO_WITH_REQUESTED_PERMISSION;

try {
  let data = bundleManager.getBundleInfoForSelfSync(bundleFlags);
  hilog.info(0x0000, 'testTag', 'getBundleInfoForSelfSync successfully: %{public}s', JSON.stringify(data));
} catch (err) {
  let message = (err as BusinessError).message;
  hilog.error(0x0000, 'testTag', 'getBundleInfoForSelfSync failed: %{public}s', message);
}
```

## ApplicationInfo<sup>20+</sup>

type ApplicationInfo = _ApplicationInfo

应用程序信息。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明           |
| ------------------------------------------------------------ | -------------- |
| [_ApplicationInfo](js-apis-bundleManager-applicationInfo.md#applicationinfo-1) | 应用程序信息。 |

## ModuleMetadata<sup>20+</sup>

type ModuleMetadata = _ModuleMetadata

模块的元数据信息。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明               |
| ------------------------------------------------------------ | ------------------ |
| [_ModuleMetadata](js-apis-bundleManager-applicationInfo.md#ModuleMetadata10) | 模块的元数据信息。 |

## Metadata<sup>20+</sup>

type Metadata = _Metadata

元数据信息。

**系统能力**: SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                    | 说明         |
| ------------------------------------------------------- | ------------ |
| [_Metadata](js-apis-bundleManager-metadata.md#metadata) | 元数据信息。 |

## BundleInfo<sup>20+</sup>

type BundleInfo = _BundleInfo.BundleInfo

应用包信息。

**系统能力**: SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明         |
| ------------------------------------------------------------ | ------------ |
| [_BundleInfo.BundleInfo](js-apis-bundleManager-bundleInfo.md#bundleinfo) | 应用包信息。 |

## UsedScene<sup>20+</sup>

type UsedScene = _BundleInfo.UsedScene

权限使用的场景和时机。

**系统能力:** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明                   |
| ------------------------------------------------------------ | ---------------------- |
| [_BundleInfo.UsedScene](js-apis-bundleManager-bundleInfo.md#usedscene) | 权限使用的场景和时机。 |

## ReqPermissionDetail<sup>20+</sup>

type ReqPermissionDetail = _BundleInfo.ReqPermissionDetail

应用运行时需向系统申请的权限集合的详细信息。

**系统能力:** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明                                         |
| ------------------------------------------------------------ | -------------------------------------------- |
| [_BundleInfo.ReqPermissionDetail](js-apis-bundleManager-bundleInfo.md#reqpermissiondetail) | 应用运行时需向系统申请的权限集合的详细信息。 |

## SignatureInfo<sup>20+</sup>

type SignatureInfo = _BundleInfo.SignatureInfo

应用包的签名信息。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明               |
| ------------------------------------------------------------ | ------------------ |
| [_BundleInfo.SignatureInfo](js-apis-bundleManager-bundleInfo.md#signatureinfo) | 应用包的签名信息。 |

## HapModuleInfo<sup>20+</sup>

type HapModuleInfo = _HapModuleInfo.HapModuleInfo

HAP信息。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明      |
| ------------------------------------------------------------ | --------- |
| [_HapModuleInfo.HapModuleInfo](js-apis-bundleManager-hapModuleInfo.md#hapmoduleinfo-1) | HAP信息。 |

## AbilityInfo<sup>20+</sup>

type AbilityInfo = _AbilityInfo.AbilityInfo

Ability信息。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明          |
| ------------------------------------------------------------ | ------------- |
| [_AbilityInfo.AbilityInfo](js-apis-bundleManager-abilityInfo.md#abilityinfo-1) | Ability信息。 |

## WindowSize<sup>20+</sup>

type WindowSize = _AbilityInfo.WindowSize

窗口尺寸。

**系统能力：** SystemCapability.BundleManager.BundleFramework.Core

**支持平台：** Android、iOS

| 类型                                                         | 说明       |
| ------------------------------------------------------------ | ---------- |
| [_AbilityInfo.WindowSize](js-apis-bundleManager-abilityInfo.md#windowsize) | 窗口尺寸。 |
