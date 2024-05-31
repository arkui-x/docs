# ApplicationInfo

The **ApplicationInfo** module defines the application information. 

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## ApplicationInfo

**System capability**: SystemCapability.BundleManager.BundleFramework.Core
| Name                      | Type                                                        | Readable| Writable| Description                                                        |
| -------------------------- | ------------------------------------------------------------ | ---- | ---- | ------------------------------------------------------------ |
| name                       | string                                                       | Yes  | No  | Application name.                                                |
| description                | string                                                       | Yes  | No  | Description of the application, for example, "description": $string: mainability_description".                                                |
| descriptionId              | number                                                       | Yes  | No  | ID of the application description.                                              |
| label                      | string                                                       | Yes  | No  | Application name, for example, "label": "$string: mainability_description".|
| labelId                    | number                                                       | Yes  | No  | ID of the application label.                                              |
| icon                       | string                                                       | Yes  | No  | Application icon, for example, "icon": "$media:icon".                                                |
| iconId                     | number                                                       | Yes  | No  | ID of the application icon.                                              |
| codePath                   | string                                                       | Yes  | No  | Installation directory of the application.                                            |