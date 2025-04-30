# HapModuleInfo

> **说明：**
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

HAP信息。

## HapModuleInfo

**系统能力**: SystemCapability.BundleManager.BundleFramework.Core

| 名称                              | 类型                                                         | 可读 | 可写 | 说明                 | Android平台  | iOS平台            |
| --------------------------------- | ------------------------------------------------------------ | ---- | ---- | -------------------- | --------------------------------- | --------------------------------- |
| name                              | string                                                       | 是   | 否   | 模块名称。             | 支持           | 支持           |
| icon                              | string                                                       | 是   | 否   | 模块图标。             | 支持           | 支持           |
| iconId                            | number                                                       | 是   | 否   | 模块图标的资源id值。       | 支持     | 支持     |
| label                             | string                                                       | 是   | 否   | 模块标签。             | 支持           | 支持           |
| labelId                           | number                                                       | 是   | 否   | 模块标签的资源id值。       | 支持     | 支持     |
| description                       | string                                                       | 是   | 否   | 模块描述信息。         | 支持       | 支持       |
| descriptionId                     | number                                                       | 是   | 否   | 描述信息的资源id值。       | 支持     | 支持     |
| mainElementName                   | string                                                       | 是   | 否   | 入口ability信息。      | 支持    | 支持    |
| abilitiesInfo                     | Array\<[AbilityInfo](js-apis-bundleManager-abilityInfo.md)>         | 是   | 否   | Ability信息。          | 支持        | 支持        |
| metadata                          | Array\<[Metadata](js-apis-bundleManager-metadata.md)>               | 是   | 否   | Ability的元信息。      | 支持    | 支持    |
| deviceTypes<sup>20+</sup> | Array\<string> | 是 | 否 | 可以运行模块的设备类型。 | 支持 | 支持 |
| installationFree<sup>20+</sup> | boolean | 是 | 否 | 模块是否支持免安装，取值为true表示支持免安装，取值为false表示不支持免安装。 | 支持 | 支持 |
| hashValue<sup>20+</sup> | string | 是 | 否 | 模块的Hash值。 | 支持 | 支持 |
| type<sup>20+</sup> | [bundleManager.ModuleType](js-apis-bundleManager.md#moduletype20) | 是 | 否 | 标识当前模块的类型。 | 支持 | 支持 |