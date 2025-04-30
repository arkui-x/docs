# ApplicationInfo

> **说明：**
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

应用程序信息。

## ApplicationInfo

**系统能力**: 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core

| 名称                              | 类型                                        | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| --------------------------------- | ------------------------------------------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| name                              | string                                      | 是   | 否   | 应用程序的名称。                                             | 支持        | 支持    |
| description                       | string                                      | 是   | 否   | 标识应用的描述信息，使用示例："description": $string: mainability_description"。 | 支持        | 支持    |
| descriptionId                     | number                                      | 是   | 否   | 标识应用的描述信息的资源id。                                 | 支持        | 支持    |
| label                             | string                                      | 是   | 否   | 标识应用的名称，使用示例："label": "$string: mainability_description"。 | 支持        | 支持    |
| labelId                           | number                                      | 是   | 否   | 标识应用名称的资源id。                                       | 支持        | 支持    |
| icon                              | string                                      | 是   | 否   | 应用程序的图标，使用示例："icon": "$media:icon"。            | 支持        | 支持    |
| iconId                            | number                                      | 是   | 否   | 应用程序图标的资源id。                                       | 支持        | 支持    |
| codePath                          | string                                      | 是   | 否   | 应用程序的安装目录。                                         | 支持        | 支持    |
| enabled<sup>20+</sup>             | boolean                                     | 是   | 否   | 判断应用程序是否可以使用，取值为true表示可以使用，取值为false表示不可使用。 | 支持        | 支持    |
| process<sup>20+</sup>             | string                                      | 是   | 否   | 应用程序的进程名称。                                         | 支持        | 支持    |
| permissions<sup>20+</sup>         | Array\<string>                              | 是   | 否   | 访问应用程序所需的权限，通过调用[getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)接口，bundleFlags参数传入GET_BUNDLE_INFO_WITH_APPLICATION和GET_BUNDLE_INFO_WITH_REQUESTED_PERMISSION获取。 | 支持        | 支持    |
| metadataArray<sup>20+</sup>       | Array\<[ModuleMetadata](#modulemetadata20)> | 是   | 否   | 应用程序的元信息，通过调用[getBundleInfoForSelf](js-apis-bundleManager.md#bundlemanagergetbundleinfoforself)接口，bundleFlags参数传入GET_BUNDLE_INFO_WITH_APPLICATION和GET_BUNDLE_INFO_WITH_METADATA获取。 | 支持        | 支持    |
| removable<sup>20+</sup>           | boolean                                     | 是   | 否   | 应用程序是否可以被移除，取值为true表示可以被移除，取值为false表示不可以被移除。 | 支持        | 支持    |
| accessTokenId<sup>20+</sup>       | number                                      | 是   | 否   | 应用程序的accessTokenId。                                    | 支持        | 支持    |
| uid<sup>20+</sup>                 | number                                      | 是   | 否   | 应用程序的uid。                                              | 支持        | 支持    |
| iconResource<sup>20+</sup>        | Resource                                    | 是   | 否   | 应用程序的图标资源信息，通过ApplicationInfo 获取的resource 包含了该资源的信息的bundleName、moduleName 和 id，可以调用全球化的接口getMediaContent来获取详细的资源数据信息。 | 支持        | 支持    |
| labelResource<sup>20+</sup>       | Resource                                    | 是   | 否   | 应用程序的标签资源信息，通过ApplicationInfo 获取的resource 包含了该资源的信息的bundleName、moduleName 和 id，可以调用全球化的接口getMediaContent来获取详细的资源数据信息。 | 支持        | 支持    |
| descriptionResource<sup>20+</sup> | Resource                                    | 是   | 否   | 应用程序的描述资源信息，通过ApplicationInfo 获取的resource 包含了该资源的信息的bundleName、moduleName 和 id，可以调用全球化的接口getMediaContent来获取详细的资源数据信息。 | 支持        | 支持    |
| appDistributionType<sup>20+</sup> | string                                      | 是   | 否   | 应用程序签名证书的分发类型，分为：app_gallery、enterprise、os_integration和crowdtesting。 | 支持        | 支持    |
| appProvisionType<sup>20+</sup>    | string                                      | 是   | 否   | 应用程序签名证书文件的类型，分为debug和release两种类型。     | 支持        | 支持    |
| systemApp<sup>20+</sup>           | boolean                                     | 是   | 否   | 标识应用是否为系统应用，取值为true表示系统应用，取值为false表示非系统应用。 | 支持        | 支持    |
| debug<sup>20+</sup>               | boolean                                     | 是   | 否   | 标识应用是否处于调试模式，取值为true表示应用处于调试模式，取值为false表示应用处于非调试模式。 | 支持        | 支持    |
| dataUnclearable<sup>20+</sup>     | boolean                                     | 是   | 否   | 标识应用数据是否可被删除。true表示不可删除，false表示可以删除。 | 支持        | 支持    |
| nativeLibraryPath<sup>20+</sup>   | string                                      | 是   | 否   | 应用程序的本地库文件路径。                                   | 支持        | 支持    |
| releaseType<sup>20+</sup>         | string                                      | 是   | 否   | 标识应用打包时使用的SDK的发布类型。当前SDK的发布类型可能为Canary、Beta、Release，其中Canary和Beta可能通过序号进一步细分，例如Canary1、Canary2、Beta1、Beta2等。开发者可通过对比应用打包依赖的SDK发布类型来判断兼容性。 | 支持        | 支持    |

## ModuleMetadata<sup>20+</sup>

描述模块的元数据信息。

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core

| 名称                     | 类型                                                  | 只读 | 可选 | 说明                       | Android平台 | iOS平台 |
| ------------------------ | ----------------------------------------------------- | ---- | ---- | -------------------------- | ----------- | ------- |
| moduleName<sup>20+</sup> | string                                                | 是   | 否   | 模块名。                   | 支持        | 支持    |
| metadata<sup>20+</sup>   | Array\<[Metadata](js-apis-bundleManager-metadata.md)> | 是   | 否   | 该模块下的元数据信息列表。 | 支持        | 支持    |