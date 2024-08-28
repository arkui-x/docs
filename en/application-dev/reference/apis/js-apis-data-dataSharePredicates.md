# @ohos.data.dataSharePredicates (DataShare Predicates)

**DataSharePredicates** provides a filter object to query data in a database by using **DataShare** APIs. It is often used to update, delete, and query data.

The APIs provided by **DataSharePredicates** correspond to the filter criteria of the database. Before using the APIs, you need to have basic database knowledge.

> **NOTE**
>
> - The initial APIs of this module are supported to cross platform since API version 12. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>
> - The APIs of this module can be used only in the stage model.



## Modules to Import

```ts
import { dataSharePredicates } from '@kit.ArkData';
```

## DataSharePredicates
Provides methods for setting different **DataSharePredicates** objects. This type is not multi-thread safe. If a **DataSharePredicates** instance is operated by multiple threads at the same time in an application, use a lock for the instance.

### equalTo<sup>12+</sup>

equalTo(field: string, value: ValueType): DataSharePredicates

Sets a **DataSharePredicates** object to match the data that is equal to the specified value.

Currently, only the relational database (RDB) and key-value database (KVDB, schema) support this **DataSharePredicates** object.

**System capability**: SystemCapability.DistributedDataManager.DataShare.Core

**Parameters**

| Name| Type                                               | Mandatory| Description                  |
| ------ | --------------------------------------------------- | ---- | ---------------------- |
| field  | string                                              | Yes  | Column name in the database table.    |
| value  | [ValueType](js-apis-data-valuesBucket.md#valuetype) | Yes  | Value to match.|

**Return value**

| Type                                       | Description                      |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | **DataSharePredicates** object created.|

**Example**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.equalTo("NAME", "Rose")
```


### and<sup>12+</sup>

and(): DataSharePredicates

Adds the AND condition to this **DataSharePredicates** object.

Currently, only the RDB and KVDB (schema) support this **DataSharePredicates** object.

**System capability**: SystemCapability.DistributedDataManager.DataShare.Core

**Return value**

| Type                                       | Description                  |
| ------------------------------------------- | ---------------------- |
| [DataSharePredicates](#datasharepredicates) | **DataSharePredicates** object with the AND condition.|

**Example**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.equalTo("NAME", "lisi")
    .and()
    .equalTo("SALARY", 200.5)
```

### orderByAsc<sup>12+</sup>

orderByAsc(field: string): DataSharePredicates

Sets a **DataSharePredicates** object that sorts data in ascending order.

Currently, only the RDB and KVDB (schema) support this **DataSharePredicates** object.

**System capability**: SystemCapability.DistributedDataManager.DataShare.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| field  | string | Yes  | Column name in the database table.|

**Return value**

| Type                                       | Description                      |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | **DataSharePredicates** object created.|

**Example**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.orderByAsc("AGE")
```

### orderByDesc<sup>12+</sup>

orderByDesc(field: string): DataSharePredicates

Sets a **DataSharePredicates** object that sorts data in descending order.

Currently, only the RDB and KVDB (schema) support this **DataSharePredicates** object.

**System capability**: SystemCapability.DistributedDataManager.DataShare.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| field  | string | Yes  | Column name in the database table.|

**Return value**

| Type                                       | Description                      |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | **DataSharePredicates** object created.|

**Example**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.orderByDesc("AGE")
```

### limit<sup>12+</sup>

limit(total: number, offset: number): DataSharePredicates

Sets a **DataSharePredicates** object to specify the number of results and the start position.

Currently, only the RDB and KVDB (schema) support this **DataSharePredicates** object.

**System capability**: SystemCapability.DistributedDataManager.DataShare.Core

**Parameters**

| Name  | Type  | Mandatory| Description          |
| -------- | ------ | ---- | -------------- |
| total    | number | Yes  | Number of results.  |
| offset | number | Yes  | Start position.|

**Return value**

| Type                                       | Description                      |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | **DataSharePredicates** object created.|

**Example**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.equalTo("NAME", "Rose").limit(10, 3)
```

### in<sup>12+</sup>

in(field: string, value: Array&lt;ValueType&gt;): DataSharePredicates

Sets a **DataSharePredicates** object to match the data that is within the specified value.

Currently, only the RDB and KVDB (schema) support this **DataSharePredicates** object.

**System capability**: SystemCapability.DistributedDataManager.DataShare.Core

**Parameters**

| Name | Type            | Mandatory| Description                                   |
| ------- | ---------------- | ---- | --------------------------------------- |
| field   | string           | Yes| Column name in the database table.                     |
| value | Array&lt;[ValueType](js-apis-data-valuesBucket.md#valuetype)&gt; | Yes  | Array of the values to match.|

**Return value**

| Type                                       | Description                      |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | **DataSharePredicates** object created.|

**Example**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.in("AGE", [18, 20])
```
