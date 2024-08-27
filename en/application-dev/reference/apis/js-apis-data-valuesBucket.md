# @ohos.data.ValuesBucket (Data Set)

The **ValuesBucket** module defines a data set in key-value (KV) format for inserting data into an RDB store.

> **NOTE**
>
> - The initial APIs of this module are supported to cross platform since API version 12. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>
> - The APIs of this module can be used only in the stage model.


## Modules to Import

```ts
import { ValueType } from '@kit.ArkData';
```

## ValueType

type ValueType = number | string | boolean

Enumerates the value types allowed by the database.

**System capability**: SystemCapability.DistributedDataManager.DataShare.Core

| Type   | Description                |
| ------- | -------------------- |
| number  | The value is a number.  |
| string  | The value is a string.|
| boolean | The value is **true** or **false**.|
