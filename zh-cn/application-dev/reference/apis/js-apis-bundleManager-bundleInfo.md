# BundleInfo

应用包信息，三方应用可以通过[bundleManager.getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)获取自身的应用包信息，其中入参[bundleFlags](js-apis-bundleManager.md#bundleflag)指定所返回的[BundleInfo](js-apis-bundleManager-bundleInfo.md)中所包含的信息。

> **说明：**
>
> 本模块首批接口从API version 20 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## BundleInfo

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core

| 名称                              | 类型                                                         | 只读 | 可选 | 说明                                                         | Android平台                                           | iOS平台                                                    |
| --------------------------------- | ------------------------------------------------------------ | ---- | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| name                              | string                                                       | 是   | 否   | 应用包的名称。 | 支持 | 支持 |
| vendor                            | string                                                       | 是   | 否   | 应用包的供应商。 | 支持 | 支持 |
| versionCode                       | number                                                       | 是   | 否   | 应用包的版本号。 | 支持 | 支持 |
| versionName                       | string                                                       | 是   | 否   | 应用包的版本文本描述信息。 | 支持 | 支持 |
| minCompatibleVersionCode          | number                                                       | 是   | 否   | 分布式场景下的应用包兼容的最低版本。 | 支持 | 支持 |
| targetVersion                     | number                                                       | 是   | 否   | 该标签标识应用运行目标版本。 | 支持 | 支持 |
| appInfo                           | [ApplicationInfo](js-apis-bundleManager-applicationInfo.md)         | 是   | 否   | 应用程序的配置信息，通过调用[getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)接口，bundleFlags参数传入GET_BUNDLE_INFO_WITH_APPLICATION获取。 | 支持 | 支持 |
| hapModulesInfo                    | Array\<[HapModuleInfo](js-apis-bundleManager-hapModuleInfo.md)>     | 是   | 否   | 模块的配置信息，通过调用[getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)接口，bundleFlags参数传入GET_BUNDLE_INFO_WITH_HAP_MODULE获取。 | 支持 | 支持 |
| reqPermissionDetails     | Array\<[ReqPermissionDetail](#reqpermissiondetail)>   | 是   | 否   | 应用运行时需向系统申请的权限集合的详细信息，通过调用[getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)接口，bundleFlags参数传入GET_BUNDLE_INFO_WITH_REQUESTED_PERMISSION获取。 | 支持 | 支持 |
| permissionGrantStates        | Array\<[bundleManager.PermissionGrantState](js-apis-bundleManager.md#permissiongrantstate)> | 是   | 否   | 申请权限的授予状态，通过调用[getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)接口，bundleFlags参数传入GET_BUNDLE_INFO_WITH_REQUESTED_PERMISSION获取。 | 支持 | 支持 |
| signatureInfo          | [SignatureInfo](#signatureinfo)                                          | 是   | 否   | 应用包的签名信息，通过调用[getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)接口，bundleFlags参数传入GET_BUNDLE_INFO_WITH_SIGNATURE_INFO获取。 | 支持 | 支持 |


## ReqPermissionDetail

应用运行时需向系统申请的权限集合的详细信息。
> **说明：**
>
> - 如果应用内多包申请的权限名称一样，但是权限申请理由不一致，系统只会返回一个权限申请理由，优先级从高到低顺序为entry类型HAP、feature类型HAP、应用内HSP。

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core

| 名称                  | 类型                    | 只读 | 可选 | 说明                 | Android平台        | iOS平台            |
| --------------------- | ----------------------- | ---- | ---- | --------------------- | --------------------- | --------------------- |
| name                  | string                  | 否   | 否   | 需要使用的权限名称。   | 支持 | 支持 |
| reason                | string                  | 否   | 否   | 描述申请权限的原因。  | 支持 | 支持 |
| reasonId              | number                  | 否   | 否  | 描述申请权限的原因ID。 | 支持 | 支持 |
| usedScene             | [UsedScene](#usedscene) | 否   | 否   | 权限使用的场景和时机。 | 支持 | 支持 |

## UsedScene

描述权限使用的场景和时机。

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core

| 名称      | 类型           | 只读 | 可选 | 说明                        | Android平台          | iOS平台                   |
| --------- | -------------- | ---- | ---- | --------------------------- | --------------------------- | --------------------------- |
| abilities | Array\<string> | 否   | 否   | 使用到该权限的Ability集合。   | 支持 | 支持 |
| when      | string         | 否   | 否   | 使用该权限的时机。支持的取值有inuse（使用时）、always（始终）。          | 支持        | 支持        |

## SignatureInfo

描述应用包的签名信息。

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core

| 名称      | 类型           | 只读 | 可选 | 说明                        | Android平台               | iOS平台                   |
| --------- | -------------- | ---- | ---- | --------------------------- | --------------------------- | --------------------------- |
| appId     | string         | 是   | 否   | 应用的appId。                 | 支持               | 支持               |
|fingerprint| string         | 是   | 否   | 应用包的指纹信息。使用的签名证书发生变化，该字段会发生变化。            | 支持          | 支持          |
