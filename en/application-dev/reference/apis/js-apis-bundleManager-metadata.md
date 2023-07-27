# Metadata

The **Metadata** module provides a metadata object. This object is contained in [HapModuleInfo](js-apis-bundleManager-hapModuleInfo.md) and [AbilityInfo](js-apis-bundleManager-abilityInfo.md).

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>
> The **Metadata** module provides the configuration about the module, UIAbility, and ExtensionAbility. The value is of the array type. The configuration is valid only for the current module, UIAbility, or ExtensionAbility.

## Metadata

**System capability**: SystemCapability.BundleManager.BundleFramework.Core
| Name    | Type  | Readable| Writable| Description      |
| -------- | ------ | ---- | ---- | ---------- |
| name     | string | Yes  | Yes  | Metadata name.|
| value    | string | Yes  | Yes  | Metadata value.  |
| resource | string | Yes  | Yes  | Metadata resource.|
