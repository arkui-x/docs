# AbilityInfo

> **说明：**
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

Ability信息。

## AbilityInfo

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core。

| 名称                             | 类型                                                         | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| -------------------------------- | ------------------------------------------------------------ | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| bundleName                       | string                                                       | 是   | 否   | 应用Bundle名称。                                             | 支持        | 支持    |
| moduleName                       | string                                                       | 是   | 否   | Ability所属的HAP的名称。                                     | 支持        | 支持    |
| name                             | string                                                       | 是   | 否   | Ability名称。                                                | 支持        | 支持    |
| label                            | string                                                       | 是   | 否   | Ability对用户显示的名称。                                    | 支持        | 支持    |
| labelId                          | number                                                       | 是   | 否   | Ability的标签资源id。                                        | 支持        | 支持    |
| description                      | string                                                       | 是   | 否   | Ability的描述。                                              | 支持        | 支持    |
| descriptionId                    | number                                                       | 是   | 否   | Ability的描述资源id。                                        | 支持        | 支持    |
| icon                             | string                                                       | 是   | 否   | Ability的图标资源文件索引。                                  | 支持        | 支持    |
| iconId                           | number                                                       | 是   | 否   | Ability的图标资源id。                                        | 支持        | 支持    |
| launchType                       | [LaunchType](js-apis-bundleManager.md#launchtype)            | 是   | 否   | Ability的启动模式。                                          | 支持        | 支持    |
| applicationInfo                  | [ApplicationInfo](js-apis-bundleManager-applicationInfo.md)  | 是   | 否   | 应用程序的配置信息。                                         | 支持        | 支持    |
| metadata                         | Array\<[Metadata](js-apis-bundleManager-metadata.md)>        | 是   | 否   | Ability的元信息。                                            | 支持        | 支持    |
| process<sup>20+</sup>            | string                                                       | 是   | 否   | Ability的进程名称。                                          | 支持        | 支持    |
| orientation<sup>20+</sup>        | [bundleManager.DisplayOrientation](js-apis-bundleManager.md#displayorientation) | 是   | 否   | Ability的显示模式。                                          | 支持        | 支持    |
| deviceTypes<sup>20+</sup>        | Array\<string>                                               | 是   | 否   | Ability支持的设备类型。                                      | 支持        | 支持    |
| enabled<sup>20+</sup>            | boolean                                                      | 是   | 否   | Ability是否可用，取值为true表示Ability可用，取值为false表示Ability不可用。 | 支持        | 支持    |
| supportWindowModes<sup>20+</sup> | Array\<[bundleManager.SupportWindowMode](js-apis-bundleManager.md#supportwindowmode)> | 是   | 否   | Ability支持的窗口模式。                                      | 支持        | 支持    |
| windowSize<sup>20+</sup>         | [WindowSize](#windowsize20)                                  | 是   | 否   | Ability窗口尺寸。                                            | 支持        | 支持    |

## WindowSize<sup>20+</sup>

描述窗口尺寸。

 **系统能力:** 以下各项对应的系统能力均为SystemCapability.BundleManager.BundleFramework.Core

| 名称                          | 类型   | 只读 | 可选 | 说明                                              | Android平台 | iOS平台 |
| ----------------------------- | ------ | ---- | ---- | ------------------------------------------------- | ----------- | ------- |
| maxWindowRatio<sup>20+</sup>  | number | 是   | 否   | 表示自由窗口状态下窗口的最大宽高比；取值范围0-1。 | 支持        | 支持    |
| minWindowRatio<sup>20+</sup>  | number | 是   | 否   | 表示自由窗口状态下窗口的最小宽高比；取值范围0-1。 | 支持        | 支持    |
| maxWindowWidth<sup>20+</sup>  | number | 是   | 否   | 表示自由窗口状态下窗口的最大宽度，宽度单位为vp。  | 支持        | 支持    |
| minWindowWidth<sup>20+</sup>  | number | 是   | 否   | 表示自由窗口状态下窗口的最小宽度，宽度单位为vp。  | 支持        | 支持    |
| maxWindowHeight<sup>20+</sup> | number | 是   | 否   | 表示自由窗口状态下窗口的最大高度，宽度单位为vp。  | 支持        | 支持    |
| minWindowHeight<sup>20+</sup> | number | 是   | 否   | 表示自由窗口状态下窗口的最小高度，宽度单位为vp。  | 支持        | 支持    |