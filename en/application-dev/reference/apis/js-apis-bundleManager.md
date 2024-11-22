# @ohos.bundle.bundleManager (bundleManager)

The **bundleManager** module provides APIs for querying information about bundles.

> **NOTE**
>
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```ts
import bundleManager from '@ohos.bundle.bundleManager';
```

### LaunchType

Enumerates the launch types of the ability.

 **System capability**: SystemCapability.BundleManager.BundleFramework.Core

| Name| Value| Description|
|:----------------:|:---:|:---:|
| SINGLETON        | 0   | The ability can have only one instance.|
| MULTITON         | 1   | The ability can have multiple instances.|