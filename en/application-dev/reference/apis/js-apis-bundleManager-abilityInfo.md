# AbilityInfo

The **AbilityInfo** module defines the ability information. 

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## AbilityInfo

**System capability**: SystemCapability.BundleManager.BundleFramework.Core

| Name                 | Type                                                    | Readable| Writable| Description                                     |
| --------------------- | -------------------------------------------------------- | ---- | ---- | ------------------------------------------ |
| bundleName            | string                                                   | Yes  | No  | Bundle name.                           |
| moduleName            | string                                                   | Yes  | No  | Name of the HAP file to which the ability belongs.                   |
| name                  | string                                                   | Yes  | No  | Ability name.                              |
| label                 | string                                                   | Yes  | No  | Ability name visible to users.                  |
| labelId               | number                                                   | Yes  | No  | ID of the ability label.                      |
| description           | string                                                   | Yes  | No  | Ability description.                            |
| descriptionId         | number                                                   | Yes  | No  | ID of the ability description.                      |
| icon                  | string                                                   | Yes  | No  | Index of the ability icon resource file.                |
| iconId                | number                                                   | Yes  | No  | ID of the ability icon.                      |
| launchType            | [LaunchType](js-apis-bundleManager.md#launchtype)        | Yes  | No  | Ability launch mode.                        |
| applicationInfo       | [ApplicationInfo](js-apis-bundleManager-applicationInfo.md)     | Yes  | No  | Application information. |
| metadata              | Array\<[Metadata](js-apis-bundleManager-metadata.md)>           | Yes  | No  | Metadata of the ability. |