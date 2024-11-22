# HapModuleInfo

The **HapModuleInfo** module defines the HAP module information. 

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## HapModuleInfo

**System capability**: SystemCapability.BundleManager.BundleFramework.Core

| Name                             | Type                                                        | Readable| Writable| Description                |
| --------------------------------- | ------------------------------------------------------------ | ---- | ---- | -------------------- |
| name                              | string                                                       | Yes  | No  | Module name.            |
| icon                              | string                                                       | Yes  | No  | Module icon.            |
| iconId                            | number                                                       | Yes  | No  | ID of the module icon.      |
| label                             | string                                                       | Yes  | No  | Module label.            |
| labelId                           | number                                                       | Yes  | No  | ID of the module label.      |
| description                       | string                                                       | Yes  | No  | Module description.        |
| descriptionId                     | number                                                       | Yes  | No  | ID of the module description.      |
| mainElementName                   | string                                                       | Yes  | No  | Name of the main ability.     |
| abilitiesInfo                     | Array\<[AbilityInfo](js-apis-bundleManager-abilityInfo.md)>         | Yes  | No  | Ability information.         |
| metadata                          | Array\<[Metadata](js-apis-bundleManager-metadata.md)>               | Yes  | No  | Metadata of the ability.     |
