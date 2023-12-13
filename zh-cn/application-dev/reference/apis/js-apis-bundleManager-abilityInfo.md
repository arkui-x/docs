# AbilityInfo

> **说明：**
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

Ability信息。

## AbilityInfo

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core。

| 名称                  | 类型                                                     | 可读 | 可写 | 说明                                      |
| --------------------- | -------------------------------------------------------- | ---- | ---- | ------------------------------------------ |
| bundleName            | string                                                   | 是   | 否   | 应用Bundle名称。                            |
| moduleName            | string                                                   | 是   | 否   | Ability所属的HAP的名称。                    |
| name                  | string                                                   | 是   | 否   | Ability名称。                               |
| label                 | string                                                   | 是   | 否   | Ability对用户显示的名称。                   |
| labelId               | number                                                   | 是   | 否   | Ability的标签资源id。                       |
| description           | string                                                   | 是   | 否   | Ability的描述。                             |
| descriptionId         | number                                                   | 是   | 否   | Ability的描述资源id。                       |
| icon                  | string                                                   | 是   | 否   | Ability的图标资源文件索引。                 |
| iconId                | number                                                   | 是   | 否   | Ability的图标资源id。                       |
| launchType            | [LaunchType](js-apis-bundleManager.md#launchtype)        | 是   | 否   | Ability的启动模式。                         |
| applicationInfo       | [ApplicationInfo](js-apis-bundleManager-applicationInfo.md)     | 是   | 否   | 应用程序的配置信息。 |
| metadata              | Array\<[Metadata](js-apis-bundleManager-metadata.md)>           | 是   | 否   | Ability的元信息。 |