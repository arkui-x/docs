# @ohos.data.relationalStore (RDB Store)

The relational database (RDB) store manages data based on relational models. The RDB store provides a complete mechanism for managing local databases based on the underlying SQLite. To satisfy different needs in complicated scenarios, the RDB store offers a series of APIs for performing operations such as adding, deleting, modifying, and querying data, and supports direct execution of SQL statements. The worker threads are not supported.

The **relationalStore** module provides the following functions:

- [RdbPredicates](#rdbpredicates): provides predicates indicating the nature, feature, or relationship of a data entity in an RDB store. It is used to define the operation conditions for an RDB store.
- [RdbStore](#rdbstore): provides APIs for managing data in an RDB store.
- [Resultset](#resultset): provides APIs for accessing the result set obtained from the RDB store. 

> **NOTE**
> 
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```ts
import relationalStore from '@ohos.data.relationalStore';
```

## relationalStore.getRdbStore

getRdbStore(context: Context, config: StoreConfig, callback: AsyncCallback&lt;RdbStore&gt;): void

Obtains an RDB store. This API uses an asynchronous callback to return the result. You can set parameters for the RDB store based on service requirements and call APIs to perform data operations.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                          | Mandatory| Description                                                        |
| -------- | ---------------------------------------------- | ---- | ------------------------------------------------------------ |
| context  | Context                                        | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| config   | [StoreConfig](#storeconfig)               | Yes  | Configuration of the RDB store.                               |
| callback | AsyncCallback&lt;[RdbStore](#rdbstore)&gt; | Yes  | Callback invoked to return the RDB store obtained.                  |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                               |
| ------------ | ----------------------------------------------------------- |
| 14800010     | Failed to open or delete database by invalid database path. |
| 14800011     | Failed to open database by database corrupted.              |
| 14800000     | Inner error.                                                |

Stage model:

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';
import { BusinessError } from "@ohos.base";

class EntryAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage) {
    const STORE_CONFIG: relationalStore.StoreConfig = {
      name: "RdbTest.db",
      securityLevel: relationalStore.SecurityLevel.S1
    };
        
    relationalStore.getRdbStore(this.context, STORE_CONFIG, (err: BusinessError, rdbStore: relationalStore.RdbStore) => {
      store = rdbStore;
      if (err) {
        console.error(`Get RdbStore failed, code is ${err.code},message is ${err.message}`);
        return;
      }
      console.info(`Get RdbStore successfully.`);
    })
  }
}
```

## relationalStore.getRdbStore

getRdbStore(context: Context, config: StoreConfig): Promise&lt;RdbStore&gt;

Obtains an RDB store. This API uses a promise to return the result. You can set parameters for the RDB store based on service requirements and call APIs to perform data operations.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name | Type                            | Mandatory| Description                                                        |
| ------- | -------------------------------- | ---- | ------------------------------------------------------------ |
| context | Context                          | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| config  | [StoreConfig](#storeconfig) | Yes  | Configuration of the RDB store.                               |

**Return value**

| Type                                     | Description                             |
| ----------------------------------------- | --------------------------------- |
| Promise&lt;[RdbStore](#rdbstore)&gt; | Promise used to return the **RdbStore** object.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                               |
| ------------ | ----------------------------------------------------------- |
| 14800010     | Failed to open or delete database by invalid database path. |
| 14800011     | Failed to open database by database corrupted.              |
| 14800000     | Inner error.                                                |

Stage model:

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';
import { BusinessError } from "@ohos.base";

class EntryAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage) {
    let store: relationalStore.RdbStore;
    const STORE_CONFIG: relationalStore.StoreConfig = {
      name: "RdbTest.db",
      securityLevel: relationalStore.SecurityLevel.S1
    };

    relationalStore.getRdbStore(this.context, STORE_CONFIG).then(async (rdbStore: relationalStore.RdbStore) => {
      store = rdbStore;
      console.info(`Get RdbStore successfully.`)
    }).catch((err: BusinessError) => {
      console.error(`Get RdbStore failed, code is ${err.code},message is ${err.message}`);
    })
  }
}
```

## relationalStore.deleteRdbStore

deleteRdbStore(context: Context, name: string, callback: AsyncCallback&lt;void&gt;): void

Deletes an RDB store. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                                                        |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| context  | Context                   | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| name     | string                    | Yes  | Name of the RDB store to delete.                                                |
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result.                                      |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                               |
| ------------ | ----------------------------------------------------------- |
| 14800010     | Failed to open or delete database by invalid database path. |
| 14800000     | Inner error.                                                |

Stage model:

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';
import { BusinessError } from "@ohos.base";

class EntryAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage){
    relationalStore.deleteRdbStore(this.context, "RdbTest.db", (err: BusinessError) => {
      if (err) {
        console.error(`Delete RdbStore failed, code is ${err.code},message is ${err.message}`);
        return;
      }
      store = undefined;
      console.info(`Delete RdbStore successfully.`);
    })
  }
}
```

## relationalStore.deleteRdbStore

deleteRdbStore(context: Context, name: string): Promise&lt;void&gt;

Deletes an RDB store. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name | Type   | Mandatory| Description                                                        |
| ------- | ------- | ---- | ------------------------------------------------------------ |
| context | Context | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| name    | string  | Yes  | Name of the RDB store to delete.                                                |

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                               |
| ------------ | ----------------------------------------------------------- |
| 14800010     | Failed to open or delete database by invalid database path. |
| 14800000     | Inner error.                                                |

Stage model:

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';
import { BusinessError } from "@ohos.base";

class EntryAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage){
    relationalStore.deleteRdbStore(this.context, "RdbTest.db").then(()=>{
      store = undefined;
      console.info(`Delete RdbStore successfully.`);
    }).catch((err: BusinessError) => {
      console.error(`Delete RdbStore failed, code is ${err.code},message is ${err.message}`);
    })
  }
}
```

## relationalStore.deleteRdbStore<sup>10+</sup>

deleteRdbStore(context: Context, config: StoreConfig, callback: AsyncCallback\<void>): void

Deletes an RDB store. This API uses an asynchronous callback to return the result.

After the deletion, you are advised to set the database object to null. If the database file is in the public sandbox directory, you must use this API to delete the database. If the database is accessed by multiple processes at the same time, you are advised to send a database deletion notification to other processes.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                       | Mandatory| Description                                                        |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| context  | Context                     | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| config   | [StoreConfig](#storeconfig) | Yes  | Configuration of the RDB store.                               |
| callback | AsyncCallback&lt;void&gt;   | Yes  | Callback invoked to return the result.                                      |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                               |
| ------------ | ----------------------------------------------------------- |
| 14800010     | Failed to open or delete database by invalid database path. |
| 14800000     | Inner error.                                                |
| 14801001     | Only supported in stage mode.                               |
| 14801002     | The data group id is not valid.                             |

**Example**

Stage model:

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';
import { BusinessError } from "@ohos.base";

class EntryAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage){
    const STORE_CONFIG: relationalStore.StoreConfig = {
      name: "RdbTest.db",
      securityLevel: relationalStore.SecurityLevel.S1
    };
    relationalStore.deleteRdbStore(this.context, STORE_CONFIG, (err: BusinessError) => {
      if (err) {
        console.error(`Delete RdbStore failed, code is ${err.code},message is ${err.message}`);
        return;
      }
      store = undefined;
      console.info(`Delete RdbStore successfully.`);
    })
  }
}
```

## relationalStore.deleteRdbStore<sup>10+</sup>

deleteRdbStore(context: Context, config: StoreConfig): Promise\<void>

Deletes an RDB store. This API uses a promise to return the result.

After the deletion, you are advised to set the database object to null. If the database file is in the public sandbox directory, you must use this API to delete the database. If the database is accessed by multiple processes at the same time, you are advised to send a database deletion notification to other processes.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name | Type                       | Mandatory| Description                                                        |
| ------- | --------------------------- | ---- | ------------------------------------------------------------ |
| context | Context                     | Yes  | Application context.<br>For details about the application context of the stage model, see [Context](js-apis-inner-application-uiAbilityContext.md).|
| config  | [StoreConfig](#storeconfig) | Yes  | Configuration of the RDB store.                               |

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                               |
| ------------ | ----------------------------------------------------------- |
| 14800010     | Failed to open or delete database by invalid database path. |
| 14800000     | Inner error.                                                |
| 14801001     | Only supported in stage mode.                               |
| 14801002     | The data group id is not valid.                             |

**Example**

Stage model:

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import window from '@ohos.window';
import { BusinessError } from "@ohos.base";

class EntryAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage){
    const STORE_CONFIG: relationalStore.StoreConfig = {
      name: "RdbTest.db",
      securityLevel: relationalStore.SecurityLevel.S1
    };
    relationalStore.deleteRdbStore(this.context, STORE_CONFIG).then(()=>{
      store = undefined;
      console.info(`Delete RdbStore successfully.`);
    }).catch((err: BusinessError) => {
      console.error(`Delete RdbStore failed, code is ${err.code},message is ${err.message}`);
    })
  }
}
```

## StoreConfig

Defines the RDB store configuration.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name       | Type         | Mandatory| Description                                                     |
| ------------- | ------------- | ---- | --------------------------------------------------------- |
| name          | string        | Yes  | Database file name.                                           |
| securityLevel | [SecurityLevel](#securitylevel) | Yes  | Security level of the RDB store.                                       |
| encrypt       | boolean       | No  | Whether to encrypt the RDB store.<br>The value **true** means to encrypt the RDB store; the value **false** (default) means the opposite.<br/>If this parameter is set to true, the cryptoParam parameter must also be configured to set the encrypted database key.|
| dataGroupId<sup>10+</sup> | string | No| Application group ID, which needs to be obtained from the AppGallery.<br>**Model restriction**: This attribute can be used only in the stage model.<br>This parameter is supported since API version 10. It specifies the **relationalStore** instance created in the sandbox directory corresponding to the **dataGroupId**. If this parameter is not specified, the **relationalStore** instance is created in the sandbox directory of the application.|
| isReadOnly<sup>12+</sup> | boolean | No| Whether the RDB store is read-only.<br>**true**: The RDB store is read-only. Writing data to the RDB store will result in error code 801.<br>**false** (default): The RDB store is readable and writeable.<br>This parameter is supported since API version 12.<br>**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core|
| rootDir<sup>18+</sup> | string | No| Root path of the database.<br>This parameter is supported since API version 18. The database in the **rootDir** + "/" + **customDir** directory will be opened or deleted. The database opened is read-only. Writing data to a read-only database will trigger error 801. If this parameter is set when you want to open or delete an RDB store, ensure that the database file exists in the corresponding path and the caller has the read permission. Otherwise, error 14800010 will be returned.<br>**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core|
| cryptoParam<sup>14+</sup> | [CryptoParam](#cryptoparam14) | No| Custom encryption parameters.<br>If this parameter is left empty, the default encryption parameters are used. For details, see default values of [CryptoParam](#cryptoparam14).<br>This parameter is valid only when **encrypt** is **true**.<br>This parameter is supported since API version 14.<br>**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core|
| persist<sup>18+</sup> | boolean | No| Whether to persist the RDB store. The value **true** means to persist the RDB store; the value **false** means the opposite (using an in-memory database). <br>Default value: **true**.<br>An in-memory database does not support encryption, backup, restore, cross-process access, and distributed capabilities, with the **securityLevel** property ignored.<br>**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core|

## SecurityLevel

Enumerates the RDB store security levels.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name| Value  | Description                                                        |
| ---- | ---- | ------------------------------------------------------------ |
| S1   | 1    | The RDB store security level is low. If data leakage occurs, minor impact will be caused on the database. For example, an RDB store that contains system data such as wallpapers.|
| S2   | 2    | The RDB store security level is medium. If data leakage occurs, moderate impact will be caused on the database. For example, an RDB store that contains information created by users or call records, such as audio or video clips.|
| S3   | 3    | The RDB store security level is high. If data leakage occurs, major impact will be caused on the database. For example, an RDB store that contains information such as user fitness, health, and location data.|
| S4   | 4    | The RDB store security level is critical. If data leakage occurs, severe impact will be caused on the database. For example, an RDB store that contains information such as authentication credentials and financial data.|

## CryptoParam<sup>14+</sup>

Represents the configuration of database encryption parameters. This parameter is valid only when **encrypt** in **StoreConfig** is **true**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name         | Type  | Mandatory| Description                                                        |
| ------------- | ------ | ---- | ------------------------------------------------------------ |
| encryptionKey | Uint8Array | Yes  | Key used for database encryption and decryption.<br>If the key is not required, you need to set the key to 0s.|
| iterationCount | number | No| Number of iterations of the PBKDF2 algorithm used in the RDB store. The value is an integer. <br>Default value: **10000**.<br>The value must be an integer greater than 0. If it is not an integer, the value is rounded down.<br>If this parameter is not specified or is set to **0**, the default value **10000** and the default encryption algorithm **AES_256_GCM** are used.|
| encryptionAlgo | [EncryptionAlgo](#encryptionalgo14) | No| Algorithm used for database encryption and decryption. <br>Default value: **AES_256_GCM**.|
| hmacAlgo | [HmacAlgo](#hmacalgo14) | No| HMAC algorithm used for database encryption and decryption. <br>Default value: **SHA256**.|
| kdfAlgo | [KdfAlgo](#kdfalgo14) | No| PBKDF2 algorithm used for database encryption and decryption. <br>Default value: the same as the HMAC algorithm used.|
| cryptoPageSize | number | No| Page size used for database encryption and decryption. <br>Default value: **1024** bytes.<br>The value must be an integer within the range of 1024 to 65536 and must be 2<sup>n</sup>. If the specified value is not an integer, the value is rounded down.|

## HmacAlgo<sup>14+</sup>

Enumerates the HMAC algorithms for the database. Use the enum name rather than the enum value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name| Value  | Description|
| ---- | ---- | ---- |
| SHA1 |  0    | HMAC_SHA1.    |
| SHA256 |  1    | HMAC_SHA256.    |
| SHA512 |  2    | HMAC_SHA512.   |

## KdfAlgo<sup>14+</sup>

Enumerates the PBKDF2 algorithms for the database. Use the enum name rather than the enum value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name| Value  | Description|
| ---- | ---- | ---- |
| KDF_SHA1 |  0    | PBKDF2_HMAC_SHA1.    |
| KDF_SHA256 |  1    | PBKDF2_HMAC_SHA256.    |
| KDF_SHA512 |  2    | PBKDF2_HMAC_SHA512.    |

## Field<sup>11+</sup>

Enumerates the special fields used in predicates. Use the enum name rather than the enum value.

**System capability**: SystemCapability.DistributedDataManager.CloudSync.Client

| Name          | Value  | Description                              |
| -------------- | ---- | ---------------------------------- |
| CURSOR_FIELD        | '#_cursor'     | Field name to be searched based on the cursor.|
| ORIGIN_FIELD        | '#_origin'     | Data source to be searched based on the cursor.   |
| DELETED_FLAG_FIELD  | '#_deleted_flag' | Whether the dirty data (data deleted from the cloud) is cleared from the local device. It fills in the result set returned upon the cursor-based search.<br>The value **true** means the dirty data is cleared; the value **false** means the opposite.|
| DATA_STATUS_FIELD<sup>12+</sup>   | '#_data_status' | Data status in the cursor-based search result set. The value **0** indicates normal data status; **1** indicates that data is retained after the account is logged out; **2** indicates that data is deleted from the cloud; **3** indicates that data is deleted after the account is logged out.|
| OWNER_FIELD  | '#_cloud_owner' | Party who shares the data. It fills in the result set returned when the owner of the shared data is searched.|
| PRIVILEGE_FIELD  | '#_cloud_privilege' | Operation permission on the shared data. It fills in the result set returned when the permission on the shared data is searched.|
| SHARING_RESOURCE_FIELD   | '#_sharing_resource_field' | Resource shared. It fills in the result set returned when the shared resource is searched.|

## AssetStatus<sup>10+</sup>

Enumerates the asset statuses. Use the enum names instead of the enum values.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name                             | Value  | Description            |
| ------------------------------- | --- | -------------- |
| ASSET_NORMAL     | -   | The asset is in normal status.     |
| ASSET_INSERT | - | The asset is to be inserted to the cloud.|
| ASSET_UPDATE | - | The asset is to be updated to the cloud.|
| ASSET_DELETE | - | The asset is to be deleted from the cloud.|
| ASSET_ABNORMAL    | -   | The asset is in abnormal status.     |
| ASSET_DOWNLOADING | -   | The asset is being downloaded to a local device.|

## Asset<sup>10+</sup>

Defines information about an asset (such as a document, image, and video). The asset APIs do not support **Datashare**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name         | Type                         | Mandatory | Description          |
| ----------- | --------------------------- | --- | ------------ |
| name        | string                      | Yes  | Asset name.      |
| uri         | string                      | Yes  | Asset URI, which is an absolute path in the system.      |
| path        | string                      | Yes  | Application sandbox path of the asset.      |
| createTime  | string                      | Yes  | Time when the asset was created.  |
| modifyTime  | string                      | Yes  | Time when the asset was last modified.|
| size        | string                      | Yes  | Size of the asset.   |
| status      | [AssetStatus](#assetstatus10) | No  | Asset status. The default value is **ASSET_NORMAL**.       |

## Assets<sup>10+</sup>

Defines an array of the [Asset](#asset10) type.

| Type   | Description                |
| ------- | -------------------- |
| [Asset](#asset10)[] | Array of assets.  |

## ValueType

Defines the data types allowed.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Type   | Description                |
| ------- | -------------------- |
| null<sup>10+</sup>    | Null.  |
| number  | Number.  |
| string  | String.  |
| boolean | Boolean.|
| Uint8Array<sup>10+</sup>           | Uint8 array.           |
| Asset<sup>10+</sup>  | [Asset](#asset10).    |
| Assets<sup>10+</sup> | [Assets](#assets10).|

## ValuesBucket

Defines the types of the key and value in a KV pair. This type is not multi-thread safe. If a **ValuesBucket** instance is operated by multiple threads at the same time in an application, use a lock for the instance.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Key Type| Value Type                  |
| ------ | ----------------------- |
| string | [ValueType](#valuetype) |

## SqlExecutionInfo<sup>12+</sup>

Represents statistics about SQL statements executed by the database.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name    | Type                                              | Read-Only| Optional |Description                                                        |
| -------- | ------------------------------------------------- | ---- | ---- | -------------------------------------------------------- |
| sql<sup>12+</sup>           | Array&lt;string&gt;            | Yes  |   No  | SQL statements executed. If the value of [batchInsert](#batchinsert) is too large, there may be multiple SQL statements.     |
| totalTime<sup>12+</sup>      | number                        | Yes  |   No  | Total time used to execute the SQL statements, in μs.                                   |
| waitTime<sup>12+</sup>       | number                        | Yes  |   No  | Time used to obtain the handle, in μs.                                        |
| prepareTime<sup>12+</sup>    | number                        | Yes  |   No  | Time used to get the SQL statements ready and bind parameters, in μs.                                |
| executeTime<sup>12+</sup>    | number                        | Yes  |   No  | Total time used to execute the SQL statements, in μs.|

## TransactionType<sup>14+</sup>

Enumerates the types of transaction objects that can be created. Use the enum name rather than the enum value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name            | Value  | Description                    |
| ---------------- | ---- | ------------------------ |
| DEFERRED       | 0    | Deferred transaction object. When a deferred transaction object is created, automatic commit is disabled and no transaction will start. A read or write transaction starts only when the first read or write operation is performed.  |
| IMMEDIATE | 1    | Immediate transaction object. When an immediate transaction object is created, a write transaction starts. If there is any uncommitted write transaction, the transaction object cannot be created and error 14800024 is returned.|
| EXCLUSIVE      | 2    | Exclusive transaction object. In WAL mode, the exclusive transaction object is the same as the immediate transaction object. In other log modes, this type of transaction can prevent the database from being read by other connections during the transaction.|

## TransactionOptions<sup>14+</sup>

Represents the configuration of a transaction object.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name       | Type         | Mandatory| Description                                                     |
| ------------- | ------------- | ---- | --------------------------------------------------------- |
| transactionType          | [TransactionType](#transactiontype14)        | No  | Transaction object type. <br>Default value: **DEFERRED**. |

## ConflictResolution<sup>10+</sup>

Defines the resolution to use when **insert()** and **update()** conflict.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name                | Value  | Description                                                        |
| -------------------- | ---- | ------------------------------------------------------------ |
| ON_CONFLICT_NONE | 0 | No operation is performed.|
| ON_CONFLICT_ROLLBACK | 1    | Abort the SQL statement and roll back the current transaction.               |
| ON_CONFLICT_ABORT    | 2    | Abort the current SQL statement and revert any changes made by the current SQL statement. However, the changes made by the previous SQL statement in the same transaction are retained and the transaction remains active.|
| ON_CONFLICT_FAIL     | 3    | Abort the current SQL statement. The **FAIL** resolution does not revert previous changes made by the failed SQL statement or end the transaction.|
| ON_CONFLICT_IGNORE   | 4    | Skip the rows that contain constraint violations and continue to process the subsequent rows of the SQL statement.|
| ON_CONFLICT_REPLACE  | 5    | Delete pre-existing rows that cause the constraint violation before inserting or updating the current row, and continue to execute the command normally.|

## RdbPredicates

Defines the predicates for an RDB store. This class determines whether the conditional expression for the RDB store is true or false. This type is not multi-thread safe. If an **RdbPredicates** instance is operated by multiple threads at the same time in an application, use a lock for the instance.

### constructor

constructor(name: string)

A constructor used to create an **RdbPredicates** object.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description        |
| ------ | ------ | ---- | ------------ |
| name   | string | Yes  | Database table name.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
```

### equalTo

equalTo(field: string, value: ValueType): RdbPredicates


Sets an **RdbPredicates** to match the field with data type **ValueType** and value equal to the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                  |
| ------ | ----------------------- | ---- | ---------------------- |
| field  | string                  | Yes  | Column name in the database table.    |
| value  | [ValueType](#valuetype) | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "lisi");
```


### notEqualTo

notEqualTo(field: string, value: ValueType): RdbPredicates


Sets an **RdbPredicates** to match the field with data type **ValueType** and value not equal to the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                  |
| ------ | ----------------------- | ---- | ---------------------- |
| field  | string                  | Yes  | Column name in the database table.    |
| value  | [ValueType](#valuetype) | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.notEqualTo("NAME", "lisi");
```


### beginWrap

beginWrap(): RdbPredicates


Adds a left parenthesis to the **RdbPredicates**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type                                | Description                     |
| ------------------------------------ | ------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** with a left parenthesis.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "lisi")
    .beginWrap()
    .equalTo("AGE", 18)
    .or()
    .equalTo("SALARY", 200.5)
    .endWrap()
```

### endWrap

endWrap(): RdbPredicates

Adds a right parenthesis to the **RdbPredicates**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type                                | Description                     |
| ------------------------------------ | ------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** with a right parenthesis.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "lisi")
    .beginWrap()
    .equalTo("AGE", 18)
    .or()
    .equalTo("SALARY", 200.5)
    .endWrap()
```

### or

or(): RdbPredicates

Adds the OR condition to the **RdbPredicates**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type                                | Description                     |
| ------------------------------------ | ------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** with the OR condition.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa")
    .or()
    .equalTo("NAME", "Rose")
```

### and

and(): RdbPredicates

Adds the AND condition to the **RdbPredicates**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type                                | Description                     |
| ------------------------------------ | ------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** with the AND condition.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa")
    .and()
    .equalTo("SALARY", 200.5)
```

### contains

contains(field: string, value: string): RdbPredicates

Sets an **RdbPredicates** to match a string containing the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                  |
| ------ | ------ | ---- | ---------------------- |
| field  | string | Yes  | Column name in the database table.    |
| value  | string | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.contains("NAME", "os");
```

### beginsWith

beginsWith(field: string, value: string): RdbPredicates

Sets an **RdbPredicates** to match a string that starts with the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                  |
| ------ | ------ | ---- | ---------------------- |
| field  | string | Yes  | Column name in the database table.    |
| value  | string | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.beginsWith("NAME", "os");
```

### endsWith

endsWith(field: string, value: string): RdbPredicates

Sets an **RdbPredicates** to match a string that ends with the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                  |
| ------ | ------ | ---- | ---------------------- |
| field  | string | Yes  | Column name in the database table.    |
| value  | string | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.endsWith("NAME", "se");
```

### isNull

isNull(field: string): RdbPredicates

Sets an **RdbPredicates** to match the field whose value is null.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| field  | string | Yes  | Column name in the database table.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.isNull("NAME");
```

### isNotNull

isNotNull(field: string): RdbPredicates

Sets an **RdbPredicates** to match the field whose value is not null.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| field  | string | Yes  | Column name in the database table.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.isNotNull("NAME");
```

### like

like(field: string, value: string): RdbPredicates

Sets an **RdbPredicates** to match a string that is similar to the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                  |
| ------ | ------ | ---- | ---------------------- |
| field  | string | Yes  | Column name in the database table.    |
| value  | string | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.like("NAME", "%os%");
```

### glob

glob(field: string, value: string): RdbPredicates

Sets an **RdbPredicates** to match the specified string.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| field  | string | Yes  | Column name in the database table.                                          |
| value  | string | Yes  | Value to match the **RdbPredicates**.<br><br>Wildcards are supported. * indicates zero, one, or multiple digits or characters. **?** indicates a single digit or character.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.glob("NAME", "?h*g");
```

### between

between(field: string, low: ValueType, high: ValueType): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **ValueType** and value within the specified range.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                      |
| ------ | ----------------------- | ---- | -------------------------- |
| field  | string                  | Yes  | Column name in the database table.        |
| low    | [ValueType](#valuetype) | Yes  | Minimum value to match the **RdbPredicates**.  |
| high   | [ValueType](#valuetype) | Yes  | Maximum value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.between("AGE", 10, 50);
```

### notBetween

notBetween(field: string, low: ValueType, high: ValueType): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **ValueType** and value out of the specified range.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                      |
| ------ | ----------------------- | ---- | -------------------------- |
| field  | string                  | Yes  | Column name in the database table.        |
| low    | [ValueType](#valuetype) | Yes  | Minimum value to match the **RdbPredicates**.  |
| high   | [ValueType](#valuetype) | Yes  | Maximum value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.notBetween("AGE", 10, 50);
```

### greaterThan

greaterThan(field: string, value: ValueType): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **ValueType** and value greater than the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                  |
| ------ | ----------------------- | ---- | ---------------------- |
| field  | string                  | Yes  | Column name in the database table.    |
| value  | [ValueType](#valuetype) | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.greaterThan("AGE", 18);
```

### lessThan

lessThan(field: string, value: ValueType): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **ValueType** and value less than the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                  |
| ------ | ----------------------- | ---- | ---------------------- |
| field  | string                  | Yes  | Column name in the database table.    |
| value  | [ValueType](#valuetype) | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.lessThan("AGE", 20);
```

### greaterThanOrEqualTo

greaterThanOrEqualTo(field: string, value: ValueType): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **ValueType** and value greater than or equal to the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                  |
| ------ | ----------------------- | ---- | ---------------------- |
| field  | string                  | Yes  | Column name in the database table.    |
| value  | [ValueType](#valuetype) | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.greaterThanOrEqualTo("AGE", 18);
```

### lessThanOrEqualTo

lessThanOrEqualTo(field: string, value: ValueType): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **ValueType** and value less than or equal to the specified value.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                   | Mandatory| Description                  |
| ------ | ----------------------- | ---- | ---------------------- |
| field  | string                  | Yes  | Column name in the database table.    |
| value  | [ValueType](#valuetype) | Yes  | Value to match the **RdbPredicates**.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.lessThanOrEqualTo("AGE", 20);
```

### orderByAsc

orderByAsc(field: string): RdbPredicates

Sets an **RdbPredicates** to match the column with values sorted in ascending order.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| field  | string | Yes  | Column name in the database table.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.orderByAsc("NAME");
```

### orderByDesc

orderByDesc(field: string): RdbPredicates

Sets an **RdbPredicates** to match the column with values sorted in descending order.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description              |
| ------ | ------ | ---- | ------------------ |
| field  | string | Yes  | Column name in the database table.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.orderByDesc("AGE");
```

### distinct

distinct(): RdbPredicates

Sets an **RdbPredicates** to filter out duplicate records.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type                                | Description                          |
| ------------------------------------ | ------------------------------ |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object that can filter out duplicate records.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose").distinct();
```

### limitAs

limitAs(value: number): RdbPredicates

Sets an **RdbPredicates** to specify the maximum number of records.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description            |
| ------ | ------ | ---- | ---------------- |
| value  | number | Yes  | Maximum number of records.|

**Return value**

| Type                                | Description                                |
| ------------------------------------ | ------------------------------------ |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object that specifies the maximum number of records.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose").limitAs(3);
```

### offsetAs

offsetAs(rowOffset: number): RdbPredicates

Sets an **RdbPredicates** to specify the start position of the returned result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name   | Type  | Mandatory| Description                              |
| --------- | ------ | ---- | ---------------------------------- |
| rowOffset | number | Yes  | Number of rows to offset from the beginning. The value is a positive integer.|

**Return value**

| Type                                | Description                                |
| ------------------------------------ | ------------------------------------ |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object that specifies the start position of the returned result.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose").offsetAs(3);
```

### groupBy

groupBy(fields: Array&lt;string&gt;): RdbPredicates

Sets an **RdbPredicates** to group rows that have the same value into summary rows.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type               | Mandatory| Description                |
| ------ | ------------------- | ---- | -------------------- |
| fields | Array&lt;string&gt; | Yes  | Names of columns to group.|

**Return value**

| Type                                | Description                  |
| ------------------------------------ | ---------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object that groups rows with the same value.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.groupBy(["AGE", "NAME"]);
```

### indexedBy

indexedBy(field: string): RdbPredicates

Sets an **RdbPredicates** object to specify the index column.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description          |
| ------ | ------ | ---- | -------------- |
| field  | string | Yes  | Name of the index column.|

**Return value**


| Type                                | Description                                 |
| ------------------------------------ | ------------------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object that specifies the index column.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.indexedBy("SALARY_INDEX");
```

### in

in(field: string, value: Array&lt;ValueType&gt;): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **Array&#60;ValueType&#62;** and value within the specified range.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                | Mandatory| Description                                   |
| ------ | ------------------------------------ | ---- | --------------------------------------- |
| field  | string                               | Yes  | Column name in the database table.                     |
| value  | Array&lt;[ValueType](#valuetype)&gt; | Yes  | Array of **ValueType**s to match.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.in("AGE", [18, 20]);
```

### notIn

notIn(field: string, value: Array&lt;ValueType&gt;): RdbPredicates

Sets an **RdbPredicates** to match the field with data type **Array&#60;ValueType&#62;** and value out of the specified range.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                | Mandatory| Description                                 |
| ------ | ------------------------------------ | ---- | ------------------------------------- |
| field  | string                               | Yes  | Column name in the database table.                   |
| value  | Array&lt;[ValueType](#valuetype)&gt; | Yes  | Array of **ValueType**s to match.|

**Return value**

| Type                                | Description                      |
| ------------------------------------ | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.notIn("NAME", ["Lisa", "Rose"]);
```

### notContains<sup>12+</sup>

notContains(field: string, value: string): RdbPredicates

Creates an **RdbPredicates** object to search for the records that do not contain the given value in the specified column.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                  |
| ------ | ------ | ---- | ---------------------- |
| field  | string | Yes  | Column name in the database table.    |
| value  | string | Yes  | Value to match.|

**Return value**

| Type                           | Description                      |
| ------------------------------- | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| **ID**| **Error Message**                                                                                                      |
| --------- |----------------------------------------------------------------------------------------------------------------|
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;  2. Incorrect parameter types. |

**Example**

```ts
// Find the records that do not contain the string "os" in the NAME column, for example, Lisa.
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.notContains("NAME", "os");
```

### notLike<sup>12+</sup>

notLike(field: string, value: string): RdbPredicates

Creates an **RdbPredicates** object to search for the records that are not similar to the given value in the specified column.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                  |
| ------ | ------ | ---- | ---------------------- |
| field  | string | Yes  | Column name in the database table.    |
| value  | string | Yes  | Value to match.|

**Return value**

| Type                           | Description                      |
| ------------------------------- | -------------------------- |
| [RdbPredicates](#rdbpredicates) | **RdbPredicates** object created.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md).

| **ID**| **Error Message**                                                                                                      |
| --------- |----------------------------------------------------------------------------------------------------------------|
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;  2. Incorrect parameter types. |

**Example**

```ts
// Find the records that are not "os" in the NAME column, for example, Rose.
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.notLike("NAME", "os");
```

## RdbStore

Provides APIs to manage an RDB store.

Before using the APIs of this class, use [executeSql](#executesql) to initialize the database table structure and related data.

### Attributes<sup>10+</sup>

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name        | Type           | Mandatory| Description                            |
| ------------ | ----------- | ---- | -------------------------------- |
| version<sup>10+</sup>  | number | Yes  | RDB store version, which is an integer greater than 0.      |

**Example**

```ts
// Set the RDB store version.
if(store != undefined) {
  (store as relationalStore.RdbStore).version = 3;
  // Obtain the RDB store version.
  console.info(`RdbStore version is ${store.version}`);
}
```

### insert

insert(table: string, values: ValuesBucket, callback: AsyncCallback&lt;number&gt;):void

Inserts a row of data into a table. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                         | Mandatory| Description                                                      |
| -------- | ----------------------------- | ---- | ---------------------------------------------------------- |
| table    | string                        | Yes  | Name of the target table.                                          |
| values   | [ValuesBucket](#valuesbucket) | Yes  | Row of data to insert.                                |
| callback | AsyncCallback&lt;number&gt;   | Yes  | Callback invoked to return the result. If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
if(store != undefined) {
  (store as relationalStore.RdbStore).insert("EMPLOYEE", valueBucket, (err: BusinessError, rowId: number) => {
    if (err) {
      console.error(`Insert is failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Insert is successful, rowId = ${rowId}`);
  })
}
```

### insert<sup>10+</sup>

insert(table: string, values: ValuesBucket,  conflict: ConflictResolution, callback: AsyncCallback&lt;number&gt;):void

Inserts a row of data into a table. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                       | Mandatory| Description                                                      |
| -------- | ------------------------------------------- | ---- | ---------------------------------------------------------- |
| table    | string                                      | Yes  | Name of the target table.                                          |
| values   | [ValuesBucket](#valuesbucket)               | Yes  | Row of data to insert.                                |
| conflict | [ConflictResolution](#conflictresolution10) | Yes  | Resolution used to resolve the conflict.                                        |
| callback | AsyncCallback&lt;number&gt;                 | Yes  | Callback invoked to return the result. If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
if(store != undefined) {
  (store as relationalStore.RdbStore).insert("EMPLOYEE", valueBucket, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE,
    (err: BusinessError, rowId: number) => {
      if (err) {
        console.error(`Insert is failed, code is ${err.code},message is ${err.message}`);
        return;
      }
      console.info(`Insert is successful, rowId = ${rowId}`);
  })
}
```

### insert

insert(table: string, values: ValuesBucket):Promise&lt;number&gt;

Inserts a row of data into a table. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                         | Mandatory| Description                      |
| ------ | ----------------------------- | ---- | -------------------------- |
| table  | string                        | Yes  | Name of the target table.          |
| values | [ValuesBucket](#valuesbucket) | Yes  | Row of data to insert.|

**Return value**

| Type                 | Description                                             |
| --------------------- | ------------------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the result. If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';
import { BusinessError } from "@ohos.base";

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
if(store != undefined) {
  (store as relationalStore.RdbStore).insert("EMPLOYEE", valueBucket).then((rowId: number) => {
    console.info(`Insert is successful, rowId = ${rowId}`);
  }).catch((err: BusinessError) => {
    console.error(`Insert is failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### insert<sup>10+</sup>

insert(table: string, values: ValuesBucket,  conflict: ConflictResolution):Promise&lt;number&gt;

Inserts a row of data into a table. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                       | Mandatory| Description                      |
| -------- | ------------------------------------------- | ---- | -------------------------- |
| table    | string                                      | Yes  | Name of the target table.          |
| values   | [ValuesBucket](#valuesbucket)               | Yes  | Row of data to insert.|
| conflict | [ConflictResolution](#conflictresolution10) | Yes  | Resolution used to resolve the conflict.        |

**Return value**

| Type                 | Description                                             |
| --------------------- | ------------------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the result. If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';
import { BusinessError } from "@ohos.base";

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
if(store != undefined) {
  (store as relationalStore.RdbStore).insert("EMPLOYEE", valueBucket, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE).then((rowId: number) => {
    console.info(`Insert is successful, rowId = ${rowId}`);
  }).catch((err: BusinessError) => {
    console.error(`Insert is failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### insertSync<sup>12+</sup>

insertSync(table: string, values: sendableRelationalStore.ValuesBucket, conflict?: ConflictResolution):number

Inserts a row of Sendable data into a table. This API returns the result synchronously. Due to the limit of the shared memory (max. 2 MB), a single data record cannot exceed 2 MB. Otherwise, the query operation will fail.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                                                                          | Mandatory| Description                                                                           |
| -------- | ---------------------------------------------------------------------------------------------- | ---- | ------------------------------------------------------------------------------- |
| table    | string                                                                                         | Yes  | Name of the target table.                                                               |
| values   | [sendableRelationalStore.ValuesBucket](./js-apis-data-sendableRelationalStore.md#valuesbucket) | Yes  | Sendable data to insert.                                           |
| conflict | [ConflictResolution](#conflictresolution10)                                                    | No  | Resolution used to resolve the conflict. <br>Default value: **relationalStore.ConflictResolution.ON_CONFLICT_NONE**.|

**Return value**

| Type  | Description                                |
| ------ | ------------------------------------ |
| number | If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000     | Inner error.                                                 |
| 14800011     | Failed to open the database because it is corrupted.                                          |
| 14800014     | The RdbStore or ResultSet is already closed.                                              |
| 14800015     | The database does not respond.                                        |
| 14800021     | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.                                       |
| 14800022     | SQLite: Callback routine requested an abort.                 |
| 14800023     | SQLite: Access permission denied.                            |
| 14800024     | SQLite: The database file is locked.                         |
| 14800025     | SQLite: A table in the database is locked.                   |
| 14800026     | SQLite: The database is out of memory.                       |
| 14800027     | SQLite: Attempt to write a readonly database.                |
| 14800028     | SQLite: Some kind of disk I/O error occurred.                |
| 14800029     | SQLite: The database is full.                                |
| 14800030     | SQLite: Unable to open the database file.                    |
| 14800031     | SQLite: TEXT or BLOB exceeds size limit.                     |
| 14800032     | SQLite: Abort due to constraint violation.                   |
| 14800033     | SQLite: Data type mismatch.                                  |
| 14800034     | SQLite: Library used incorrectly.                            |
| 14800047     | The WAL file size exceeds the default limit.                 |

**Example**

```ts
import { sendableRelationalStore } from '@kit.ArkData';

const valuesBucket: relationalStore.ValuesBucket = {
  "NAME": 'hangman',
  "AGE": 18,
  "SALARY": 100.5,
  "CODES": new Uint8Array([1, 2, 3])
};
const sendableValuesBucket = sendableRelationalStore.toSendableValuesBucket(valuesBucket);

if (store != undefined) {
  try {
    let rowId: number = (store as relationalStore.RdbStore).insertSync("EMPLOYEE", sendableValuesBucket, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE);
    console.info(`Insert is successful, rowId = ${rowId}`);
  } catch (error) {
    console.error(`Insert is failed, code is ${error.code},message is ${error.message}`);
  }
}
```

### insertSync<sup>12+</sup>

insertSync(table: string, values: sendableRelationalStore.ValuesBucket, conflict?: ConflictResolution):number

Inserts a row of Sendable data into a table. This API returns the result synchronously. Due to the limit of the shared memory (max. 2 MB), a single data record cannot exceed 2 MB. Otherwise, the query operation will fail.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                                                                          | Mandatory| Description                                                                           |
| -------- | ---------------------------------------------------------------------------------------------- | ---- | ------------------------------------------------------------------------------- |
| table    | string                                                                                         | Yes  | Name of the target table.                                                               |
| values   | [sendableRelationalStore.ValuesBucket](./js-apis-data-sendableRelationalStore.md#valuesbucket) | Yes  | Sendable data to insert.                                           |
| conflict | [ConflictResolution](#conflictresolution10)                                                    | No  | Resolution used to resolve the conflict. <br>Default value: **relationalStore.ConflictResolution.ON_CONFLICT_NONE**.|

**Return value**

| Type  | Description                                |
| ------ | ------------------------------------ |
| number | If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000     | Inner error.                                                 |
| 14800011     | Failed to open the database because it is corrupted.                                          |
| 14800014     | The RdbStore or ResultSet is already closed.                                              |
| 14800015     | The database does not respond.                                        |
| 14800021     | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.                                       |
| 14800022     | SQLite: Callback routine requested an abort.                 |
| 14800023     | SQLite: Access permission denied.                            |
| 14800024     | SQLite: The database file is locked.                         |
| 14800025     | SQLite: A table in the database is locked.                   |
| 14800026     | SQLite: The database is out of memory.                       |
| 14800027     | SQLite: Attempt to write a readonly database.                |
| 14800028     | SQLite: Some kind of disk I/O error occurred.                |
| 14800029     | SQLite: The database is full.                                |
| 14800030     | SQLite: Unable to open the database file.                    |
| 14800031     | SQLite: TEXT or BLOB exceeds size limit.                     |
| 14800032     | SQLite: Abort due to constraint violation.                   |
| 14800033     | SQLite: Data type mismatch.                                  |
| 14800034     | SQLite: Library used incorrectly.                            |
| 14800047     | The WAL file size exceeds the default limit.                 |

**Example**

```ts
import { sendableRelationalStore } from '@kit.ArkData';

const valuesBucket: relationalStore.ValuesBucket = {
  "NAME": 'hangman',
  "AGE": 18,
  "SALARY": 100.5,
  "CODES": new Uint8Array([1, 2, 3])
};
const sendableValuesBucket = sendableRelationalStore.toSendableValuesBucket(valuesBucket);

if (store != undefined) {
  try {
    let rowId: number = (store as relationalStore.RdbStore).insertSync("EMPLOYEE", sendableValuesBucket, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE);
    console.info(`Insert is successful, rowId = ${rowId}`);
  } catch (error) {
    console.error(`Insert is failed, code is ${error.code},message is ${error.message}`);
  }
}
```

### batchInsert

batchInsert(table: string, values: Array&lt;ValuesBucket&gt;, callback: AsyncCallback&lt;number&gt;):void

Batch inserts data into a table. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                      | Mandatory| Description                                                        |
| -------- | ------------------------------------------ | ---- | ------------------------------------------------------------ |
| table    | string                                     | Yes  | Name of the target table.                                            |
| values   | Array&lt;[ValuesBucket](#valuesbucket)&gt; | Yes  | An array of data to insert.                                |
| callback | AsyncCallback&lt;number&gt;                | Yes  | Callback invoked to return the result. If the operation is successful, the number of inserted data records is returned. Otherwise, **-1** is returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
let value5 = "Jack";
let value6 = 19;
let value7 = 101.5;
let value8 = new Uint8Array([6, 7, 8, 9, 10]);
let value9 = "Tom";
let value10 = 20;
let value11 = 102.5;
let value12 = new Uint8Array([11, 12, 13, 14, 15]);
const valueBucket1: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
const valueBucket2: ValuesBucket = {
  key1: value5,
  key2: value6,
  key3: value7,
  key4: value8,
};
const valueBucket3: ValuesBucket = {
  key1: value9,
  key2: value10,
  key3: value11,
  key4: value12,
};

let valueBuckets = new Array(valueBucket1, valueBucket2, valueBucket3);
if(store != undefined) {
  (store as relationalStore.RdbStore).batchInsert("EMPLOYEE", valueBuckets, (err, insertNum) => {
    if (err) {
      console.error(`batchInsert is failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`batchInsert is successful, the number of values that were inserted = ${insertNum}`);
  })
}
```

### batchInsert

batchInsert(table: string, values: Array&lt;ValuesBucket&gt;):Promise&lt;number&gt;

Batch inserts data into a table. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                      | Mandatory| Description                        |
| ------ | ------------------------------------------ | ---- | ---------------------------- |
| table  | string                                     | Yes  | Name of the target table.            |
| values | Array&lt;[ValuesBucket](#valuesbucket)&gt; | Yes  | An array of data to insert.|

**Return value**

| Type                 | Description                                                       |
| --------------------- | ----------------------------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the result. If the operation is successful, the number of inserted data records is returned. Otherwise, **-1** is returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';
import { BusinessError } from "@ohos.base";

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
let value5 = "Jack";
let value6 = 19;
let value7 = 101.5;
let value8 = new Uint8Array([6, 7, 8, 9, 10]);
let value9 = "Tom";
let value10 = 20;
let value11 = 102.5;
let value12 = new Uint8Array([11, 12, 13, 14, 15]);
const valueBucket1: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
const valueBucket2: ValuesBucket = {
  key1: value5,
  key2: value6,
  key3: value7,
  key4: value8,
};
const valueBucket3: ValuesBucket = {
  key1: value9,
  key2: value10,
  key3: value11,
  key4: value12,
};

let valueBuckets = new Array(valueBucket1, valueBucket2, valueBucket3);
if(store != undefined) {
  (store as relationalStore.RdbStore).batchInsert("EMPLOYEE", valueBuckets).then((insertNum: number) => {
    console.info(`batchInsert is successful, the number of values that were inserted = ${insertNum}`);
  }).catch((err: BusinessError) => {
    console.error(`batchInsert is failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### createTransaction<sup>14+</sup>

createTransaction(options?: TransactionOptions): Promise&lt;Transaction&gt;

Creates a transaction object and starts a transaction. This API uses a promise to return the result.

Different from [beginTransaction](#begintransaction), **createTransaction()** returns a transaction object, which is isolated from other transaction objects. When a transaction object is used to insert, delete, or update data, these changes cannot be detected by [on ('dataChange')](#ondatachange).

A store supports a maximum of four transaction objects at a time. If the number of transaction objects exceeds the upper limit, error 14800015 is returned. In this case, check whether transaction objects are being held too long or whether there are too many concurrent transactions. If the problem cannot be solved through these optimizations, you are advised to create transaction objects after existing transactions are released.

You are advised to use **createTransaction** instead of **beginTransaction**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type                         | Mandatory| Description                                                        |
| ----------- | ----------------------------- | ---- | ------------------------------------------------------------ |
| options       | [TransactionOptions](#transactionoptions14)           | No  | Configuration of the transaction object to create.                                |

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;[Transaction](#transaction14)&gt; | Promise used to return the transaction object created.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800015  | The database is busy.              |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800030  | SQLite: Unable to open the database file. |

**Example**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.execute("DELETE FROM test WHERE age = ? OR age = ?", [21, 20]).then(() => {
      transaction.commit();
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`execute sql failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction faided, code is ${err.code},message is ${err.message}`);
  });
}
```

### createTransaction<sup>14+</sup>

createTransaction(options?: TransactionOptions): Promise&lt;Transaction&gt;

Creates a transaction object and starts a transaction. This API uses a promise to return the result.

Different from [beginTransaction](#begintransaction), **createTransaction()** returns a transaction object, which is isolated from other transaction objects. When a transaction object is used to insert, delete, or update data, these changes cannot be detected by [on ('dataChange')](#ondatachange).

A store supports a maximum of four transaction objects at a time. If the number of transaction objects exceeds the upper limit, error 14800015 is returned. In this case, check whether transaction objects are being held too long or whether there are too many concurrent transactions. If the problem cannot be solved through these optimizations, you are advised to create transaction objects after existing transactions are released.

You are advised to use **createTransaction** instead of **beginTransaction**.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type                         | Mandatory| Description                                                        |
| ----------- | ----------------------------- | ---- | ------------------------------------------------------------ |
| options       | [TransactionOptions](#transactionoptions14)           | No  | Configuration of the transaction object to create.                                |

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;[Transaction](#transaction14)&gt; | Promise used to return the transaction object created.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800015  | The database is busy.              |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800030  | SQLite: Unable to open the database file. |

**Example**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.execute("DELETE FROM test WHERE age = ? OR age = ?", [21, 20]).then(() => {
      transaction.commit();
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`execute sql failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction faided, code is ${err.code},message is ${err.message}`);
  });
}
```

### update

update(values: ValuesBucket, predicates: RdbPredicates, callback: AsyncCallback&lt;number&gt;):void

Updates data in the RDB store based on the specified **RdbPredicates** object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                | Mandatory| Description                                                        |
| ---------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| values     | [ValuesBucket](#valuesbucket)        | Yes  | Rows of data to update in the RDB store. The key-value pair is associated with the column name in the target table.|
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Update conditions specified by the **RdbPredicates** object.                   |
| callback   | AsyncCallback&lt;number&gt;          | Yes  | Callback invoked to return the number of rows updated.                  |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Rose";
let value2 = 22;
let value3 = 200.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");
if(store != undefined) {
  (store as relationalStore.RdbStore).update(valueBucket, predicates,(err, rows) => {
    if (err) {
      console.error(`Updated failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Updated row count: ${rows}`);
  })
}
```

### update<sup>10+</sup>

update(values: ValuesBucket, predicates: RdbPredicates, conflict: ConflictResolution, callback: AsyncCallback&lt;number&gt;):void

Updates data in the RDB store based on the specified **RdbPredicates** object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                       | Mandatory| Description                                                        |
| ---------- | ------------------------------------------- | ---- | ------------------------------------------------------------ |
| values     | [ValuesBucket](#valuesbucket)               | Yes  | Rows of data to update in the RDB store. The key-value pair is associated with the column name in the target table.|
| predicates | [RdbPredicates](#rdbpredicates)            | Yes  | Update conditions specified by the **RdbPredicates** object.                     |
| conflict   | [ConflictResolution](#conflictresolution10) | Yes  | Resolution used to resolve the conflict.                                          |
| callback   | AsyncCallback&lt;number&gt;                 | Yes  | Callback invoked to return the number of rows updated.                  |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Rose";
let value2 = 22;
let value3 = 200.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");
if(store != undefined) {
  (store as relationalStore.RdbStore).update(valueBucket, predicates, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE, (err, rows) => {
    if (err) {
      console.error(`Updated failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Updated row count: ${rows}`);
  })
}
```

### update

update(values: ValuesBucket, predicates: RdbPredicates):Promise&lt;number&gt;

Updates data based on the specified **RdbPredicates** object. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name      | Type                                | Mandatory| Description                                                        |
| ------------ | ------------------------------------ | ---- | ------------------------------------------------------------ |
| values       | [ValuesBucket](#valuesbucket)        | Yes  | Rows of data to update in the RDB store. The key-value pair is associated with the column name in the target table.|
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Update conditions specified by the **RdbPredicates** object.                   |

**Return value**

| Type                 | Description                                     |
| --------------------- | ----------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the number of rows updated.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';
import { BusinessError } from "@ohos.base";

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Rose";
let value2 = 22;
let value3 = 200.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");
if(store != undefined) {
  (store as relationalStore.RdbStore).update(valueBucket, predicates).then(async (rows: Number) => {
    console.info(`Updated row count: ${rows}`);
  }).catch((err: BusinessError) => {
    console.error(`Updated failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### update<sup>10+</sup>

update(values: ValuesBucket, predicates: RdbPredicates, conflict: ConflictResolution):Promise&lt;number&gt;

Updates data based on the specified **RdbPredicates** object. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                       | Mandatory| Description                                                        |
| ---------- | ------------------------------------------- | ---- | ------------------------------------------------------------ |
| values     | [ValuesBucket](#valuesbucket)               | Yes  | Rows of data to update in the RDB store. The key-value pair is associated with the column name in the target table.|
| predicates | [RdbPredicates](#rdbpredicates)            | Yes  | Update conditions specified by the **RdbPredicates** object.                     |
| conflict   | [ConflictResolution](#conflictresolution10) | Yes  | Resolution used to resolve the conflict.                                          |

**Return value**

| Type                 | Description                                     |
| --------------------- | ----------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the number of rows updated.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';
import { BusinessError } from "@ohos.base";

let key1 = "NAME";
let key2 = "AGE";
let key3 = "SALARY";
let key4 = "CODES";
let value1 = "Rose";
let value2 = 22;
let value3 = 200.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
const valueBucket: ValuesBucket = {
  key1: value1,
  key2: value2,
  key3: value3,
  key4: value4,
};
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");
if(store != undefined) {
  (store as relationalStore.RdbStore).update(valueBucket, predicates, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE).then(async (rows: Number) => {
    console.info(`Updated row count: ${rows}`);
  }).catch((err: BusinessError) => {
    console.error(`Updated failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### delete

delete(predicates: RdbPredicates, callback: AsyncCallback&lt;number&gt;):void

Deletes data from the RDB store based on the specified **RdbPredicates** object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                | Mandatory| Description                                     |
| ---------- | ------------------------------------ | ---- | ----------------------------------------- |
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Conditions specified by the **RdbPredicates** object for deleting data.|
| callback   | AsyncCallback&lt;number&gt;          | Yes  | Callback invoked to return the number of rows deleted. |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");
if(store != undefined) {
  (store as relationalStore.RdbStore).delete(predicates, (err, rows) => {
    if (err) {
      console.error(`Delete failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Delete rows: ${rows}`);
  })
}
```

### delete

delete(predicates: RdbPredicates):Promise&lt;number&gt;

Deletes data from the RDB store based on the specified **RdbPredicates** object. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                | Mandatory| Description                                     |
| ---------- | ------------------------------------ | ---- | ----------------------------------------- |
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Conditions specified by the **RdbPredicates** object for deleting data.|

**Return value**

| Type                 | Description                           |
| --------------------- | ------------------------------- |
| Promise&lt;number&gt; | Promise used to return the number of rows deleted.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { BusinessError } from "@ohos.base";

let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");
if(store != undefined) {
  (store as relationalStore.RdbStore).delete(predicates).then((rows: Number) => {
    console.info(`Delete rows: ${rows}`);
  }).catch((err: BusinessError) => {
    console.error(`Delete failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### query<sup>10+</sup>

query(predicates: RdbPredicates, callback: AsyncCallback&lt;ResultSet&gt;):void

Queries data from the RDB store based on specified conditions. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                                        | Mandatory| Description                                                       |
| ---------- | ------------------------------------------------------------ | ---- | ----------------------------------------------------------- |
| predicates | [RdbPredicates](#rdbpredicates)                         | Yes  | Query conditions specified by the **RdbPredicates** object.                  |
| callback   | AsyncCallback&lt;[ResultSet](#resultset)&gt; | Yes  | Callback invoked to return the result. If the operation is successful, a **ResultSet** object will be returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose");
if(store != undefined) {
  (store as relationalStore.RdbStore).query(predicates, (err, resultSet) => {
    if (err) {
      console.error(`Query failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
    // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
    while (resultSet.goToNextRow()) {
      const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
      const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
      const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
      const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
      console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
    }
    // Release the dataset memory.
    resultSet.close();
  })
}
```

### query

query(predicates: RdbPredicates, columns: Array&lt;string&gt;, callback: AsyncCallback&lt;ResultSet&gt;):void

Queries data from the RDB store based on specified conditions. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                                        | Mandatory| Description                                                       |
| ---------- | ------------------------------------------------------------ | ---- | ----------------------------------------------------------- |
| predicates | [RdbPredicates](#rdbpredicates)                         | Yes  | Query conditions specified by the **RdbPredicates** object.                  |
| columns    | Array&lt;string&gt;                                          | Yes  | Columns to query. If this parameter is not specified, the query applies to all columns.           |
| callback   | AsyncCallback&lt;[ResultSet](#resultset)&gt; | Yes  | Callback invoked to return the result. If the operation is successful, a **ResultSet** object will be returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose");
if(store != undefined) {
  (store as relationalStore.RdbStore).query(predicates, ["ID", "NAME", "AGE", "SALARY", "CODES"], (err, resultSet) => {
    if (err) {
      console.error(`Query failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
    // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
    while (resultSet.goToNextRow()) {
      const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
      const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
      const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
      const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
      console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
    }
    // Release the dataset memory.
    resultSet.close();
  })
}
```

### query

query(predicates: RdbPredicates, columns?: Array&lt;string&gt;):Promise&lt;ResultSet&gt;

Queries data from the RDB store based on specified conditions. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                | Mandatory| Description                                            |
| ---------- | ------------------------------------ | ---- | ------------------------------------------------ |
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Query conditions specified by the **RdbPredicates** object.       |
| columns    | Array&lt;string&gt;                  | No  | Columns to query. If this parameter is not specified, the query applies to all columns.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Return value**

| Type                                                   | Description                                              |
| ------------------------------------------------------- | -------------------------------------------------- |
| Promise&lt;[ResultSet](#resultset)&gt; | Promise used to return the result. If the operation is successful, a **ResultSet** object will be returned.|

**Example**

```ts
import { BusinessError } from "@ohos.base";

let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose");
if(store != undefined) {
  (store as relationalStore.RdbStore).query(predicates, ["ID", "NAME", "AGE", "SALARY", "CODES"]).then((resultSet: relationalStore.ResultSet) => {
    console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
    // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
    while (resultSet.goToNextRow()) {
      const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
      const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
      const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
      const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
      console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
    }
    // Release the dataset memory.
    resultSet.close();
  }).catch((err: BusinessError) => {
    console.error(`Query failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### querySql<sup>10+</sup>

querySql(sql: string, callback: AsyncCallback&lt;ResultSet&gt;):void

Queries data using the specified SQL statement. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                        | Mandatory| Description                                                        |
| -------- | -------------------------------------------- | ---- | ------------------------------------------------------------ |
| sql      | string                                       | Yes  | SQL statement to run.                                       |
| callback | AsyncCallback&lt;[ResultSet](#resultset)&gt; | Yes  | Callback invoked to return the result. If the operation is successful, a **ResultSet** object will be returned.   |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
if(store != undefined) {
  (store as relationalStore.RdbStore).querySql("SELECT * FROM EMPLOYEE CROSS JOIN BOOK WHERE BOOK.NAME = 'sanguo'", (err, resultSet) => {
    if (err) {
      console.error(`Query failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
    // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
    while (resultSet.goToNextRow()) {
      const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
      const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
      const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
      const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
      console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
    }
    // Release the dataset memory.
    resultSet.close();
  })
}
```

### querySql

querySql(sql: string, bindArgs: Array&lt;ValueType&gt;, callback: AsyncCallback&lt;ResultSet&gt;):void

Queries data using the specified SQL statement. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                        | Mandatory| Description                                                        |
| -------- | -------------------------------------------- | ---- | ------------------------------------------------------------ |
| sql      | string                                       | Yes  | SQL statement to run.                                       |
| bindArgs | Array&lt;[ValueType](#valuetype)&gt;         | Yes  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If the SQL parameter statement is complete, the value of this parameter must be an empty array.|
| callback | AsyncCallback&lt;[ResultSet](#resultset)&gt; | Yes  | Callback invoked to return the result. If the operation is successful, a **ResultSet** object will be returned.   |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
if(store != undefined) {
  (store as relationalStore.RdbStore).querySql("SELECT * FROM EMPLOYEE CROSS JOIN BOOK WHERE BOOK.NAME = ?", ['sanguo'], (err, resultSet) => {
    if (err) {
      console.error(`Query failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
    // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
    while (resultSet.goToNextRow()) {
      const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
      const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
      const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
      const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
      console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
    }
    // Release the dataset memory.
    resultSet.close();
  })
}
```

### querySql

querySql(sql: string, bindArgs?: Array&lt;ValueType&gt;):Promise&lt;ResultSet&gt;

Queries data using the specified SQL statement. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                | Mandatory| Description                                                        |
| -------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql      | string                               | Yes  | SQL statement to run.                                       |
| bindArgs | Array&lt;[ValueType](#valuetype)&gt; | No  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If the SQL parameter statement is complete, leave this parameter blank.|

**Return value**

| Type                                                   | Description                                              |
| ------------------------------------------------------- | -------------------------------------------------- |
| Promise&lt;[ResultSet](#resultset)&gt; | Promise used to return the result. If the operation is successful, a **ResultSet** object will be returned.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
import { BusinessError } from "@ohos.base";

if(store != undefined) {
  (store as relationalStore.RdbStore).querySql("SELECT * FROM EMPLOYEE CROSS JOIN BOOK WHERE BOOK.NAME = 'sanguo'").then((resultSet: relationalStore.ResultSet) => {
    console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
    // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
    while (resultSet.goToNextRow()) {
      const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
      const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
      const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
      const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
      console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
    }
    // Release the dataset memory.
    resultSet.close();
  }).catch((err: BusinessError) => {
    console.error(`Query failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### executeSql<sup>10+</sup>

executeSql(sql: string, callback: AsyncCallback&lt;void&gt;):void

Executes an SQL statement that contains specified arguments but returns no value. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                | Mandatory| Description                                                        |
| -------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql      | string                               | Yes  | SQL statement to run.                                       |
| callback | AsyncCallback&lt;void&gt;            | Yes  | Callback invoked to return the result.                                      |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
const SQL_DELETE_TABLE = "DELETE FROM test WHERE name = 'zhangsan'"
if(store != undefined) {
  (store as relationalStore.RdbStore).executeSql(SQL_DELETE_TABLE, (err) => {
    if (err) {
      console.error(`ExecuteSql failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Delete table done.`);
  })
}
```

### executeSql

executeSql(sql: string, bindArgs: Array&lt;ValueType&gt;, callback: AsyncCallback&lt;void&gt;):void

Executes an SQL statement that contains specified arguments but returns no value. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                | Mandatory| Description                                                        |
| -------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql      | string                               | Yes  | SQL statement to run.                                       |
| bindArgs | Array&lt;[ValueType](#valuetype)&gt; | Yes  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If the SQL parameter statement is complete, the value of this parameter must be an empty array.|
| callback | AsyncCallback&lt;void&gt;            | Yes  | Callback invoked to return the result.                                      |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
const SQL_DELETE_TABLE = "DELETE FROM test WHERE name = ?"
if(store != undefined) {
  (store as relationalStore.RdbStore).executeSql(SQL_DELETE_TABLE, ['zhangsan'], (err) => {
    if (err) {
      console.error(`ExecuteSql failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Delete table done.`);
  })
}
```

### executeSql

executeSql(sql: string, bindArgs?: Array&lt;ValueType&gt;):Promise&lt;void&gt;

Executes an SQL statement that contains specified arguments but returns no value. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                | Mandatory| Description                                                        |
| -------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql      | string                               | Yes  | SQL statement to run.                                       |
| bindArgs | Array&lt;[ValueType](#valuetype)&gt; | No  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If the SQL parameter statement is complete, leave this parameter blank.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import { BusinessError } from "@ohos.base";

const SQL_DELETE_TABLE = "DELETE FROM test WHERE name = 'zhangsan'"
if(store != undefined) {
  (store as relationalStore.RdbStore).executeSql(SQL_DELETE_TABLE).then(() => {
    console.info(`Delete table done.`);
  }).catch((err: BusinessError) => {
    console.error(`ExecuteSql failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### beginTransaction

beginTransaction():void

Starts the transaction before executing an SQL statement.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                |
| ------------ | -------------------------------------------- |
| 14800047     | The WAL file size exceeds the default limit. |
| 14800000     | Inner error.                                 |

**Example**

```ts
import featureAbility from '@ohos.ability.featureAbility'
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let context = getContext(this);

let key1 = "name";
let key2 = "age";
let key3 = "SALARY";
let key4 = "blobType";
let value1 = "Lisi";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3]);
const STORE_CONFIG: relationalStore.StoreConfig = {
  name: "RdbTest.db",
  securityLevel: relationalStore.SecurityLevel.S1
};
relationalStore.getRdbStore(context, STORE_CONFIG, async (err, store) => {
  if (err) {
    console.error(`GetRdbStore failed, code is ${err.code},message is ${err.message}`);
    return;
  }
  store.beginTransaction();
  const valueBucket: ValuesBucket = {
    key1: value1,
    key2: value2,
    key3: value3,
    key4: value4,
  };
  await store.insert("test", valueBucket);
  store.commit();
})
```

### commit

commit():void

Commits the executed SQL statements.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let context = getContext(this);

let key1 = "name";
let key2 = "age";
let key3 = "SALARY";
let key4 = "blobType";
let value1 = "Lisi";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3]);
const STORE_CONFIG: relationalStore.StoreConfig = {
  name: "RdbTest.db",
  securityLevel: relationalStore.SecurityLevel.S1
};
relationalStore.getRdbStore(context, STORE_CONFIG, async (err, store) => {
  if (err) {
    console.error(`GetRdbStore failed, code is ${err.code},message is ${err.message}`);
    return;
  }
  store.beginTransaction();
  const valueBucket: ValuesBucket = {
    key1: value1,
    key2: value2,
    key3: value3,
    key4: value4,
  };
  await store.insert("test", valueBucket);
  store.commit();
})
```

### rollBack

rollBack():void

Rolls back the SQL statements that have been executed.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Example**

```ts
import { ValuesBucket } from '@ohos.data.ValuesBucket';

let context = getContext(this);

let key1 = "name";
let key2 = "age";
let key3 = "SALARY";
let key4 = "blobType";
let value1 = "Lisi";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3]);
const STORE_CONFIG: relationalStore.StoreConfig = {
  name: "RdbTest.db",
  securityLevel: relationalStore.SecurityLevel.S1
};
relationalStore.getRdbStore(context, STORE_CONFIG, async (err, store) => {
  if (err) {
    console.error(`GetRdbStore failed, code is ${err.code},message is ${err.message}`);
    return;
  }
  try {
    store.beginTransaction()
    const valueBucket: ValuesBucket = {
      key1: value1,
      key2: value2,
      key3: value3,
      key4: value4,
    };
    await store.insert("test", valueBucket);
    store.commit();
  } catch (err) {
    let code = (err as BusinessError).code;
    let message = (err as BusinessError).message
    console.error(`Transaction failed, code is ${code},message is ${message}`);
    store.rollBack();
  }
})
```

### backup

backup(destName:string, callback: AsyncCallback&lt;void&gt;):void

Backs up an RDB store. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                    |
| -------- | ------------------------- | ---- | ------------------------ |
| destName | string                    | Yes  | Name of the RDB store backup file.|
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result.  |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
if(store != undefined) {
  (store as relationalStore.RdbStore).backup("dbBackup.db", (err) => {
    if (err) {
      console.error(`Backup failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Backup success.`);
  })
}
```

### backup

backup(destName:string): Promise&lt;void&gt;

Backs up an RDB store. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type  | Mandatory| Description                    |
| -------- | ------ | ---- | ------------------------ |
| destName | string | Yes  | Name of the RDB store backup file.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
import { BusinessError } from "@ohos.base";

if(store != undefined) {
  let promiseBackup = (store as relationalStore.RdbStore).backup("dbBackup.db");
  promiseBackup.then(() => {
    console.info(`Backup success.`);
  }).catch((err: BusinessError) => {
    console.error(`Backup failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### restore

restore(srcName:string, callback: AsyncCallback&lt;void&gt;):void

Restores an RDB store from a backup file. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                     | Mandatory| Description                    |
| -------- | ------------------------- | ---- | ------------------------ |
| srcName  | string                    | Yes  | Name of the RDB store backup file.|
| callback | AsyncCallback&lt;void&gt; | Yes  | Callback invoked to return the result.  |

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
if(store != undefined) {
  (store as relationalStore.RdbStore).restore("dbBackup.db", (err) => {
    if (err) {
      console.error(`Restore failed, code is ${err.code},message is ${err.message}`);
      return;
    }
    console.info(`Restore success.`);
  })
}
```

### restore

restore(srcName:string): Promise&lt;void&gt;

Restores an RDB store from a backup file. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name | Type  | Mandatory| Description                    |
| ------- | ------ | ---- | ------------------------ |
| srcName | string | Yes  | Name of the RDB store backup file.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                |
| ------------ | ---------------------------- |
| 14800000     | Inner error.                 |

**Example**

```ts
import { BusinessError } from "@ohos.base";

if(store != undefined) {
  let promiseRestore = (store as relationalStore.RdbStore).restore("dbBackup.db");
  promiseRestore.then(() => {
    console.info(`Restore success.`);
  }).catch((err: BusinessError) => {
    console.error(`Restore failed, code is ${err.code},message is ${err.message}`);
  })
}
```

### on('statistics')<sup>12+</sup>

on(event: 'statistics', observer: Callback&lt;SqlExecutionInfo&gt;): void

Subscribes to SQL statistics.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name      | Type                             | Mandatory| Description                               |
| ------------ |---------------------------------| ---- |-----------------------------------|
| event        | string                          | Yes  | Event type. The value is **'statistics'**, which indicates the statistics of the SQL execution time.|
| observer     | Callback&lt;[SqlExecutionInfo](#sqlexecutioninfo12)&gt; | Yes  | Callback used to return the statistics about the SQL execution time in the database. |

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**   |
|-----------|--------|
| 401       | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified; 2.Incorrect parameter types. |
| 801       | Capability not supported.  |
| 14800000  | Inner error.  |
| 14800014  | The RdbStore or ResultSet is already closed.     |

**Example**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let sqlExecutionInfo = (sqlExecutionInfo: relationalStore.SqlExecutionInfo) => {
  console.info(`sql: ${sqlExecutionInfo.sql[0]}`);
  console.info(`totalTime: ${sqlExecutionInfo.totalTime}`);
  console.info(`waitTime: ${sqlExecutionInfo.waitTime}`);
  console.info(`prepareTime: ${sqlExecutionInfo.prepareTime}`);
  console.info(`executeTime: ${sqlExecutionInfo.executeTime}`);
};

try {
  if (store != undefined) {
    (store as relationalStore.RdbStore).on('statistics', sqlExecutionInfo);
  }
} catch (err) {
  let code = (err as BusinessError).code;
  let message = (err as BusinessError).message;
  console.error(`Register observer failed, code is ${code},message is ${message}`);
}

try {
  let value1 = "Lisa";
  let value2 = 18;
  let value3 = 100.5;
  let value4 = new Uint8Array([1, 2, 3, 4, 5]);

  const valueBucket: relationalStore.ValuesBucket = {
    'NAME': value1,
    'AGE': value2,
    'SALARY': value3,
    'CODES': value4
  };
  if (store != undefined) {
    (store as relationalStore.RdbStore).insert('test', valueBucket);
  }
} catch (err) {
  console.error(`insert fail, code:${err.code}, message: ${err.message}`);
}
```

### close<sup>12+</sup>

close(): Promise&lt;void&gt;

Closes this RDB store. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type               | Description         |
| ------------------- | ------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**                                   |
| ------------ | ----------------------------------------------- |
| 401          | Parameter error. The store must not be nullptr. |
| 14800000     | Inner error.                                    |

**Example**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

if (store != undefined) {
  (store as relationalStore.RdbStore).close().then(() => {
    console.info(`close succeeded`);
  }).catch((err: BusinessError) => {
    console.error(`close failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### on('statistics')<sup>12+</sup>

on(event: 'statistics', observer: Callback&lt;SqlExecutionInfo&gt;): void

Subscribes to SQL statistics.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name      | Type                             | Mandatory| Description                               |
| ------------ |---------------------------------| ---- |-----------------------------------|
| event        | string                          | Yes  | Event type. The value is **'statistics'**, which indicates the statistics of the SQL execution time.|
| observer     | Callback&lt;[SqlExecutionInfo](#sqlexecutioninfo12)&gt; | Yes  | Callback used to return the statistics about the SQL execution time in the database. |

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**   |
|-----------|--------|
| 401       | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified; 2.Incorrect parameter types. |
| 801       | Capability not supported.  |
| 14800000  | Inner error.  |
| 14800014  | The RdbStore or ResultSet is already closed.     |

**Example**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let sqlExecutionInfo = (sqlExecutionInfo: relationalStore.SqlExecutionInfo) => {
  console.info(`sql: ${sqlExecutionInfo.sql[0]}`);
  console.info(`totalTime: ${sqlExecutionInfo.totalTime}`);
  console.info(`waitTime: ${sqlExecutionInfo.waitTime}`);
  console.info(`prepareTime: ${sqlExecutionInfo.prepareTime}`);
  console.info(`executeTime: ${sqlExecutionInfo.executeTime}`);
};

try {
  if (store != undefined) {
    (store as relationalStore.RdbStore).on('statistics', sqlExecutionInfo);
  }
} catch (err) {
  let code = (err as BusinessError).code;
  let message = (err as BusinessError).message;
  console.error(`Register observer failed, code is ${code},message is ${message}`);
}

try {
  let value1 = "Lisa";
  let value2 = 18;
  let value3 = 100.5;
  let value4 = new Uint8Array([1, 2, 3, 4, 5]);

  const valueBucket: relationalStore.ValuesBucket = {
    'NAME': value1,
    'AGE': value2,
    'SALARY': value3,
    'CODES': value4
  };
  if (store != undefined) {
    (store as relationalStore.RdbStore).insert('test', valueBucket);
  }
} catch (err) {
  console.error(`insert fail, code:${err.code}, message: ${err.message}`);
}
```

### close<sup>12+</sup>

close(): Promise&lt;void&gt;

Closes this RDB store. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type               | Description         |
| ------------------- | ------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**                                   |
| ------------ | ----------------------------------------------- |
| 401          | Parameter error. The store must not be nullptr. |
| 14800000     | Inner error.                                    |

**Example**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

if (store != undefined) {
  (store as relationalStore.RdbStore).close().then(() => {
    console.info(`close succeeded`);
  }).catch((err: BusinessError) => {
    console.error(`close failed, code is ${err.code},message is ${err.message}`);
  });
}
```

## ResultSet

Provides APIs to access the result set obtained by querying the RDB store. A result set is a set of results returned after **query()** is called.

### Usage

Obtain the **resultSet** object first.

```ts
let resultSet: relationalStore.ResultSet | undefined = undefined;
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("AGE", 18);
if(store != undefined) {
  (store as relationalStore.RdbStore).query(predicates, ["ID", "NAME", "AGE", "SALARY", "CODES"]).then((result: relationalStore.ResultSet) => {
    resultSet = result;
    console.info(`resultSet columnNames: ${resultSet.columnNames}`);
    console.info(`resultSet columnCount: ${resultSet.columnCount}`);
  });
}
```

### Attributes

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

| Name        | Type           | Mandatory| Description                            |
| ------------ | ------------------- | ---- | -------------------------------- |
| columnNames  | Array&lt;string&gt; | Yes  | Names of all columns in the result set.      |
| columnCount  | number              | Yes  | Number of columns in the result set.            |
| rowCount     | number              | Yes  | Number of rows in the result set.            |
| rowIndex     | number              | Yes  | Index of the current row in the result set.        |
| isAtFirstRow | boolean             | Yes  | Whether the cursor is in the first row of the result set.      |
| isAtLastRow  | boolean             | Yes  | Whether the cursor is in the last row of the result set.    |
| isEnded      | boolean             | Yes  | Whether the cursor is after the last row of the result set.|
| isStarted    | boolean             | Yes  | Whether the cursor has been moved.            |
| isClosed     | boolean             | Yes  | Whether the result set is closed.        |

### getColumnIndex

getColumnIndex(columnName: string): number

Obtains the column index based on the column name.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type  | Mandatory| Description                      |
| ---------- | ------ | ---- | -------------------------- |
| columnName | string | Yes  | Column name.|

**Return value**

| Type  | Description              |
| ------ | ------------------ |
| number | Column index obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const id = (resultSet as relationalStore.ResultSet).getLong((resultSet as relationalStore.ResultSet).getColumnIndex("ID"));
const name = (resultSet as relationalStore.ResultSet).getString((resultSet as relationalStore.ResultSet).getColumnIndex("NAME"));
const age = (resultSet as relationalStore.ResultSet).getLong((resultSet as relationalStore.ResultSet).getColumnIndex("AGE"));
const salary = (resultSet as relationalStore.ResultSet).getDouble((resultSet as relationalStore.ResultSet).getColumnIndex("SALARY"));
}
```

### getColumnName

getColumnName(columnIndex: number): string

Obtains the column name based on the specified column index.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                      |
| ----------- | ------ | ---- | -------------------------- |
| columnIndex | number | Yes  | Column index.|

**Return value**

| Type  | Description              |
| ------ | ------------------ |
| string | Column name obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const id = (resultSet as relationalStore.ResultSet).getColumnName(0);
const name = (resultSet as relationalStore.ResultSet).getColumnName(1);
const age = (resultSet as relationalStore.ResultSet).getColumnName(2);
}
```

### goTo

goTo(offset:number): boolean

Moves the cursor to the row based on the specified offset.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type  | Mandatory| Description                        |
| ------ | ------ | ---- | ---------------------------- |
| offset | number | Yes  | Offset relative to the current position.|

**Return value**

| Type   | Description                                         |
| ------- | --------------------------------------------- |
| boolean | Returns **true** if the operation is successful; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800012     | The result set is empty or the specified location is invalid. |

**Example**

```ts
if(resultSet != undefined) {
(resultSet as relationalStore.ResultSet).goTo(1);
}
```

### goToRow

goToRow(position: number): boolean

Moves to the specified row in the result set.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type  | Mandatory| Description                    |
| -------- | ------ | ---- | ------------------------ |
| position | number | Yes  | Destination position to move to.|

**Return value**

| Type   | Description                                         |
| ------- | --------------------------------------------- |
| boolean | Returns **true** if the operation is successful; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800012     | The result set is empty or the specified location is invalid. |

**Example**

```ts
if(resultSet != undefined) {
(resultSet as relationalStore.ResultSet).goToRow(5);
}
```

### goToFirstRow

goToFirstRow(): boolean


Moves to the first row of the result set.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type   | Description                                         |
| ------- | --------------------------------------------- |
| boolean | Returns **true** if the operation is successful; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800012     | The result set is empty or the specified location is invalid. |

**Example**

```ts
if(resultSet != undefined) {
(resultSet as relationalStore.ResultSet).goToFirstRow();
}
```

### goToLastRow

goToLastRow(): boolean

Moves to the last row of the result set.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type   | Description                                         |
| ------- | --------------------------------------------- |
| boolean | Returns **true** if the operation is successful; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800012     | The result set is empty or the specified location is invalid. |

**Example**

```ts
if(resultSet != undefined) {
(resultSet as relationalStore.ResultSet).goToLastRow();
}
```

### goToNextRow

goToNextRow(): boolean

Moves to the next row in the result set.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type   | Description                                         |
| ------- | --------------------------------------------- |
| boolean | Returns **true** if the operation is successful; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800012     | The result set is empty or the specified location is invalid. |

**Example**

```ts
if(resultSet != undefined) {
(resultSet as relationalStore.ResultSet).goToNextRow();
}
```

### goToPreviousRow

goToPreviousRow(): boolean

Moves to the previous row in the result set.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type   | Description                                         |
| ------- | --------------------------------------------- |
| boolean | Returns **true** if the operation is successful; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800012     | The result set is empty or the specified location is invalid. |

**Example**

```ts
if(resultSet != undefined) {
(resultSet as relationalStore.ResultSet).goToPreviousRow();
}
```

### getBlob

getBlob(columnIndex: number): Uint8Array

Obtains the value in the form of a byte array based on the specified column and the current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                   |
| ----------- | ------ | ---- | ----------------------- |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type      | Description                            |
| ---------- | -------------------------------- |
| Uint8Array | Value obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const codes = (resultSet as relationalStore.ResultSet).getBlob((resultSet as relationalStore.ResultSet).getColumnIndex("CODES"));
}
```

### getValue<sup>12+</sup>

getValue(columnIndex: number): ValueType

Obtains the value from the specified column and current row. If the value type is any of **ValueType**, the value of the corresponding type will be returned. Otherwise, **14800000** will be returned.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                   |
| ----------- | ------ | ---- | ----------------------- |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type      | Description                            |
| ---------- | -------------------------------- |
| [ValueType](#valuetype) | Values of the specified type.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**    |
|-----------|---------|
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error.      |
| 14800011  | Failed to open the database because it is corrupted.        |
| 14800012  | ResultSet is empty or pointer index is out of bounds.       |
| 14800013  | Resultset is empty or column index is out of bounds.   |
| 14800014  | The RdbStore or ResultSet is already closed.       |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.    |
| 14800022  | SQLite: Callback routine requested an abort.     |
| 14800023  | SQLite: Access permission denied.    |
| 14800024  | SQLite: The database file is locked.    |
| 14800025  | SQLite: A table in the database is locked.  |
| 14800026  | SQLite: The database is out of memory.    |
| 14800027  | SQLite: Attempt to write a readonly database.    |
| 14800028  | SQLite: Some kind of disk I/O error occurred.    |
| 14800029  | SQLite: The database is full.   |
| 14800030  | SQLite: Unable to open the database file.    |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit.    |
| 14800032  | SQLite: Abort due to constraint violation.   |
| 14800033  | SQLite: Data type mismatch.      |
| 14800034  | SQLite: Library used incorrectly.    |

**Example**

```ts
if (resultSet != undefined) {
  const codes = (resultSet as relationalStore.ResultSet).getValue((resultSet as relationalStore.ResultSet).getColumnIndex("BIGINT_COLUMN"));
}
```

### getValue<sup>12+</sup>

getValue(columnIndex: number): ValueType

Obtains the value from the specified column and current row. If the value type is any of **ValueType**, the value of the corresponding type will be returned. Otherwise, **14800000** will be returned.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                   |
| ----------- | ------ | ---- | ----------------------- |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type      | Description                            |
| ---------- | -------------------------------- |
| [ValueType](#valuetype) | Values of the specified type.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**    |
|-----------|---------|
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error.      |
| 14800011  | Failed to open the database because it is corrupted.        |
| 14800012  | ResultSet is empty or pointer index is out of bounds.       |
| 14800013  | Resultset is empty or column index is out of bounds.   |
| 14800014  | The RdbStore or ResultSet is already closed.       |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.    |
| 14800022  | SQLite: Callback routine requested an abort.     |
| 14800023  | SQLite: Access permission denied.    |
| 14800024  | SQLite: The database file is locked.    |
| 14800025  | SQLite: A table in the database is locked.  |
| 14800026  | SQLite: The database is out of memory.    |
| 14800027  | SQLite: Attempt to write a readonly database.    |
| 14800028  | SQLite: Some kind of disk I/O error occurred.    |
| 14800029  | SQLite: The database is full.   |
| 14800030  | SQLite: Unable to open the database file.    |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit.    |
| 14800032  | SQLite: Abort due to constraint violation.   |
| 14800033  | SQLite: Data type mismatch.      |
| 14800034  | SQLite: Library used incorrectly.    |

**Example**

```ts
if (resultSet != undefined) {
  const codes = (resultSet as relationalStore.ResultSet).getValue((resultSet as relationalStore.ResultSet).getColumnIndex("BIGINT_COLUMN"));
}
```

### getString

getString(columnIndex: number): string

Obtains the value in the form of a string based on the specified column and the current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                   |
| ----------- | ------ | ---- | ----------------------- |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type  | Description                        |
| ------ | ---------------------------- |
| string | String obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const name = (resultSet as relationalStore.ResultSet).getString((resultSet as relationalStore.ResultSet).getColumnIndex("NAME"));
}
```

### getLong

getLong(columnIndex: number): number

Obtains the value of the Long type based on the specified column and the current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                   |
| ----------- | ------ | ---- | ----------------------- |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type  | Description                                                        |
| ------ | ------------------------------------------------------------ |
| number | Value obtained.<br>The value range supported by API is **Number.MIN_SAFE_INTEGER** to **Number.MAX_SAFE_INTEGER**. If the value is out of this range, use [getDouble](#getdouble).|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const age = (resultSet as relationalStore.ResultSet).getLong((resultSet as relationalStore.ResultSet).getColumnIndex("AGE"));
}
```

### getDouble

getDouble(columnIndex: number): number

Obtains the value of the double type based on the specified column and the current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                   |
| ----------- | ------ | ---- | ----------------------- |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type  | Description                        |
| ------ | ---------------------------- |
| number | Returns the value obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const salary = (resultSet as relationalStore.ResultSet).getDouble((resultSet as relationalStore.ResultSet).getColumnIndex("SALARY"));
}
```

### getAsset<sup>10+</sup>

getAsset(columnIndex: number): Asset

Obtains the value in the [Asset](#asset10) format based on the specified column and current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name        | Type    | Mandatory | Description          |
| ----------- | ------ | --- | ------------ |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type             | Description                        |
| --------------- | -------------------------- |
| [Asset](#asset10) | Returns the value obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                    |
| --------- | ------------------------------------------------------------ |
| 14800013  | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const doc = (resultSet as relationalStore.ResultSet).getAsset((resultSet as relationalStore.ResultSet).getColumnIndex("DOC"));
}
```

### getAssets<sup>10+</sup>

getAssets(columnIndex: number): Assets

Obtains the value in the [Assets](#assets10) format based on the specified column and current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name        | Type    | Mandatory | Description          |
| ----------- | ------ | --- | ------------ |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type             | Description                          |
| ---------------- | ---------------------------- |
| [Assets](#assets10)| Returns the value obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const docs = (resultSet as relationalStore.ResultSet).getAssets((resultSet as relationalStore.ResultSet).getColumnIndex("DOCS"));
}
```

### getRow<sup>11+</sup>

getRow(): ValuesBucket

Obtains the data in the current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type             | Description                          |
| ---------------- | ---------------------------- |
| [ValuesBucket](#valuesbucket) | Data obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800012  | ResultSet is empty or pointer index is out of bounds. |
| 14800013  | Resultset is empty or column index is out of bounds. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800022  | SQLite: Callback routine requested an abort. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800030  | SQLite: Unable to open the database file. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800032  | SQLite: Abort due to constraint violation. |
| 14800033  | SQLite: Data type mismatch. |
| 14800034  | SQLite: Library used incorrectly. |

**Example**

```ts
if (resultSet != undefined) {
  const row = (resultSet as relationalStore.ResultSet).getRow();
}
```

### getRow<sup>11+</sup>

getRow(): ValuesBucket

Obtains the data in the current row.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type             | Description                          |
| ---------------- | ---------------------------- |
| [ValuesBucket](#valuesbucket) | Data obtained.|

**Error codes**

For details about the error codes, see [RDB Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800012  | ResultSet is empty or pointer index is out of bounds. |
| 14800013  | Resultset is empty or column index is out of bounds. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800022  | SQLite: Callback routine requested an abort. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800030  | SQLite: Unable to open the database file. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800032  | SQLite: Abort due to constraint violation. |
| 14800033  | SQLite: Data type mismatch. |
| 14800034  | SQLite: Library used incorrectly. |

**Example**

```ts
if (resultSet != undefined) {
  const row = (resultSet as relationalStore.ResultSet).getRow();
}
```


### isColumnNull

isColumnNull(columnIndex: number): boolean

Checks whether the value in the specified column is null.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name     | Type  | Mandatory| Description                   |
| ----------- | ------ | ---- | ----------------------- |
| columnIndex | number | Yes  | Index of the target column, starting from 0.|

**Return value**

| Type   | Description                                                     |
| ------- | --------------------------------------------------------- |
| boolean | Returns **true** if the value is null; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800013     | The column value is null or the column type is incompatible. |

**Example**

```ts
if(resultSet != undefined) {
const isColumnNull = (resultSet as relationalStore.ResultSet).isColumnNull((resultSet as relationalStore.ResultSet).getColumnIndex("CODES"));
}
```

### close

close(): void

Closes this result set.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Example**

```ts
if(resultSet != undefined) {
(resultSet as relationalStore.ResultSet).close();
}
```

**Error codes**

For details about the error codes, see [RDB Error Codes](../errorcodes/errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 14800012     | The result set is empty or the specified location is invalid. |

## Transaction<sup>14+</sup>

Provides API for managing databases in transaction mode. A transaction object is created by using [createTransaction](#createtransaction14). Operations on different transaction objects are isolated. For details about the transaction types, see [TransactionType](#transactiontype14).

Currently, an RDB store supports only one write transaction at a time. If the current [RdbStore](#rdbstore) has a write transaction that is not released, creating an **IMMEDIATE** or **EXCLUSIVE** transaction object will return error 14800024. If a **DEFERRED** transaction object is created, error 14800024 may be returned when it is used to invoke a write operation for the first time. After a write transaction is created using **IMMEDIATE** or **EXCLUSIVE**, or a **DEFERRED** transaction is upgraded to a write transaction, write operations in the [RdbStore](#rdbstore) will also return error 14800024.

When the number of concurrent transactions is large and the write transaction duration is long, the frequency of returning error 14800024 may increase. You can reduce the occurrence of error 14800024 by shortening the transaction duration or by handling the error 14800024 through retries.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Example**

```ts
import { UIAbility } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';
import { window } from '@kit.ArkUI';

let store: relationalStore.RdbStore | undefined = undefined;

class EntryAbility extends UIAbility {
  async onWindowStageCreate(windowStage: window.WindowStage) {
    const STORE_CONFIG: relationalStore.StoreConfig = {
      name: "RdbTest.db",
      securityLevel: relationalStore.SecurityLevel.S3
    };

    await relationalStore.getRdbStore(this.context, STORE_CONFIG).then(async (rdbStore: relationalStore.RdbStore) => {
      store = rdbStore;
      console.info('Get RdbStore successfully.');
    }).catch((err: BusinessError) => {
      console.error(`Get RdbStore failed, code is ${err.code},message is ${err.message}`);
    });

    if (store != undefined) {
      (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
        console.info(`createTransaction success`);
        // Perform subsequent operations after the transaction instance is successfully obtained.
      }).catch((err: BusinessError) => {
        console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
      });
    }
  }
}
```

### commit<sup>14+</sup>

commit(): Promise&lt;void&gt;

Commits the executed SQL statements. When using asynchronous APIs to execute SQL statements, ensure that **commit()** is called after the asynchronous API execution is completed. Otherwise, the SQL operations may be lost. After **commit()** is called, the transaction object and the created **ResultSet** object will be closed.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |

**Example**

```ts
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3]);

if (store != undefined) {
  const valueBucket: relationalStore.ValuesBucket = {
    'NAME': value1,
    'AGE': value2,
    'SALARY': value3,
    'CODES': value4
  };
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.execute("DELETE FROM TEST WHERE age = ? OR age = ?", ["18", "20"]).then(() => {
      transaction.commit();
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`execute sql failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### rollback<sup>14+</sup>

rollback(): Promise&lt;void&gt;

Rolls back the executed SQL statement. After **rollback()** is called, the transaction object and the created **ResultSet** object will be closed.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise that returns no value.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |

**Example**

```ts
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.execute("DELETE FROM TEST WHERE age = ? OR age = ?", ["18", "20"]).then(() => {
      transaction.commit();
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`execute sql failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### insert<sup>14+</sup>

insert(table: string, values: ValuesBucket, conflict?: ConflictResolution): Promise&lt;number&gt;

Inserts a row of data into a table. This API uses a promise to return the result. Due to the limit of the shared memory (max. 2 MB), a single data record cannot exceed 2 MB. Otherwise, the query operation will fail.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                       | Mandatory| Description                      |
| -------- | ------------------------------------------- | ---- | -------------------------- |
| table    | string                                      | Yes  | Name of the target table.          |
| values   | [ValuesBucket](#valuesbucket)               | Yes  | Row of data to insert.|
| conflict | [ConflictResolution](#conflictresolution10) | No  | Resolution used to resolve the conflict. <br>Default value: **relationalStore.ConflictResolution.ON_CONFLICT_NONE**.        |

**Return value**

| Type                 | Description                                             |
| --------------------- | ------------------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the result. If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800033  | SQLite: Data type mismatch. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);

// You can use either of the following:
const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  NAME: value1,
  AGE: value2,
  SALARY: value3,
  CODES: value4
};
const valueBucket3: relationalStore.ValuesBucket = {
  "NAME": value1,
  "AGE": value2,
  "SALARY": value3,
  "CODES": value4
};

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.insert("EMPLOYEE", valueBucket1, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE).then((rowId: number) => {
      transaction.commit();
      console.info(`Insert is successful, rowId = ${rowId}`);
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`Insert is failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### insertSync<sup>14+</sup>

insertSync(table: string, values: ValuesBucket | sendableRelationalStore.ValuesBucket, conflict?: ConflictResolution): number

Inserts a row of data into a table. Due to the limit of the shared memory (max. 2 MB), a single data record cannot exceed 2 MB. Otherwise, the query operation will fail.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                       | Mandatory| Description                                                        |
| -------- | ------------------------------------------- | ---- | ------------------------------------------------------------ |
| table    | string                                      | Yes  | Name of the target table.                                            |
| values   | [ValuesBucket](#valuesbucket) \| [sendableRelationalStore.ValuesBucket](./js-apis-data-sendableRelationalStore.md#valuesbucket)   | Yes  | Row of data to insert.                                  |
| conflict | [ConflictResolution](#conflictresolution10) | No  | Resolution used to resolve the conflict. <br>Default value: **relationalStore.ConflictResolution.ON_CONFLICT_NONE**.|

**Return value**

| Type  | Description                                |
| ------ | ------------------------------------ |
| number | If the operation is successful, the row ID will be returned. Otherwise, **-1** will be returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000     | Inner error.                                                 |
| 14800011     | Failed to open the database because it is corrupted.                                          |
| 14800014     | The RdbStore or ResultSet is already closed.                                              |
| 14800021     | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.                                       |
| 14800023     | SQLite: Access permission denied.                            |
| 14800024     | SQLite: The database file is locked.                         |
| 14800025     | SQLite: A table in the database is locked.                   |
| 14800026     | SQLite: The database is out of memory.                       |
| 14800027     | SQLite: Attempt to write a readonly database.                |
| 14800028     | SQLite: Some kind of disk I/O error occurred.                |
| 14800029     | SQLite: The database is full.                                |
| 14800031     | SQLite: TEXT or BLOB exceeds size limit.                     |
| 14800033     | SQLite: Data type mismatch.                                  |
| 14800047     | The WAL file size exceeds the default limit.                 |

**Example**

```ts
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);

// You can use either of the following:
const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  NAME: value1,
  AGE: value2,
  SALARY: value3,
  CODES: value4
};
const valueBucket3: relationalStore.ValuesBucket = {
  "NAME": value1,
  "AGE": value2,
  "SALARY": value3,
  "CODES": value4
};

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    try {
      let rowId: number = (transaction as relationalStore.Transaction).insertSync("EMPLOYEE", valueBucket1, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE);
      transaction.commit();
      console.info(`Insert is successful, rowId = ${rowId}`);
    } catch (e) {
      transaction.rollback();
      console.error(`Insert is failed, code is ${e.code},message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### batchInsert<sup>14+</sup>

batchInsert(table: string, values: Array&lt;ValuesBucket&gt;): Promise&lt;number&gt;

Inserts a batch of data into a table. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                      | Mandatory| Description                        |
| ------ | ------------------------------------------ | ---- | ---------------------------- |
| table  | string                                     | Yes  | Name of the target table.            |
| values | Array&lt;[ValuesBucket](#valuesbucket)&gt; | Yes  | An array of data to insert.|

**Return value**

| Type                 | Description                                                       |
| --------------------- | ----------------------------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the result. If the operation is successful, the number of inserted data records is returned. Otherwise, **-1** is returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800033  | SQLite: Data type mismatch. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
let value5 = "Jack";
let value6 = 19;
let value7 = 101.5;
let value8 = new Uint8Array([6, 7, 8, 9, 10]);
let value9 = "Tom";
let value10 = 20;
let value11 = 102.5;
let value12 = new Uint8Array([11, 12, 13, 14, 15]);

const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  'NAME': value5,
  'AGE': value6,
  'SALARY': value7,
  'CODES': value8
};
const valueBucket3: relationalStore.ValuesBucket = {
  'NAME': value9,
  'AGE': value10,
  'SALARY': value11,
  'CODES': value12
};

let valueBuckets = new Array(valueBucket1, valueBucket2, valueBucket3);
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.batchInsert("EMPLOYEE", valueBuckets).then((insertNum: number) => {
      transaction.commit();
      console.info(`batchInsert is successful, the number of values that were inserted = ${insertNum}`);
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`batchInsert is failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### batchInsertSync<sup>14+</sup>

batchInsertSync(table: string, values: Array&lt;ValuesBucket&gt;): number

Inserts a batch of data into a table. This API returns the result synchronously.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                      | Mandatory| Description                        |
| ------ | ------------------------------------------ | ---- | ---------------------------- |
| table  | string                                     | Yes  | Name of the target table.            |
| values | Array&lt;[ValuesBucket](#valuesbucket)&gt; | Yes  | An array of data to insert.|

**Return value**

| Type  | Description                                          |
| ------ | ---------------------------------------------- |
| number | If the operation is successful, the number of inserted data records is returned. Otherwise, **-1** is returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000     | Inner error.                                                 |
| 14800011     | Failed to open the database because it is corrupted.                                          |
| 14800014     | The RdbStore or ResultSet is already closed.                                              |
| 14800021     | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.                                       |
| 14800023     | SQLite: Access permission denied.                            |
| 14800024     | SQLite: The database file is locked.                         |
| 14800025     | SQLite: A table in the database is locked.                   |
| 14800026     | SQLite: The database is out of memory.                       |
| 14800027     | SQLite: Attempt to write a readonly database.                |
| 14800028     | SQLite: Some kind of disk I/O error occurred.                |
| 14800029     | SQLite: The database is full.                                |
| 14800031     | SQLite: TEXT or BLOB exceeds size limit.                     |
| 14800033     | SQLite: Data type mismatch.                                  |
| 14800047     | The WAL file size exceeds the default limit.                 |

**Example**

```ts
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
let value5 = "Jack";
let value6 = 19;
let value7 = 101.5;
let value8 = new Uint8Array([6, 7, 8, 9, 10]);
let value9 = "Tom";
let value10 = 20;
let value11 = 102.5;
let value12 = new Uint8Array([11, 12, 13, 14, 15]);

const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  'NAME': value5,
  'AGE': value6,
  'SALARY': value7,
  'CODES': value8
};
const valueBucket3: relationalStore.ValuesBucket = {
  'NAME': value9,
  'AGE': value10,
  'SALARY': value11,
  'CODES': value12
};

let valueBuckets = new Array(valueBucket1, valueBucket2, valueBucket3);
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    try {
      let insertNum: number = (transaction as relationalStore.Transaction).batchInsertSync("EMPLOYEE", valueBuckets);
      transaction.commit();
      console.info(`batchInsert is successful, the number of values that were inserted = ${insertNum}`);
    } catch (e) {
      transaction.rollback();
      console.error(`batchInsert is failed, code is ${e.code},message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### batchInsertWithConflictResolution<sup>18+</sup>

batchInsertWithConflictResolution(table: string, values: Array&lt;ValuesBucket&gt;, conflict: ConflictResolution): Promise&lt;number&gt;

Inserts a batch of data into a table. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                      | Mandatory| Description                        |
| ------ | ------------------------------------------ | ---- | ---------------------------- |
| table  | string                                     | Yes  | Name of the target table.            |
| values | Array&lt;[ValuesBucket](#valuesbucket)&gt; | Yes  | An array of data to insert.|
| conflict | [ConflictResolution](#conflictresolution10) | Yes  | Resolution used to resolve the conflict. If **ON_CONFLICT_ROLLBACK** is used, the transaction will be rolled back when a conflict occurs.|

**Return value**

| Type                 | Description                                                       |
| --------------------- | ----------------------------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the result. If the operation is successful, the number of inserted data records is returned. Otherwise, **-1** is returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800022  | SQLite: Callback routine requested an abort. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800032  | SQLite: Abort due to constraint violation. |
| 14800033  | SQLite: Data type mismatch. |
| 14800034  | SQLite: Library used incorrectly. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
let value5 = "Jack";
let value6 = 19;
let value7 = 101.5;
let value8 = new Uint8Array([6, 7, 8, 9, 10]);
let value9 = "Tom";
let value10 = 20;
let value11 = 102.5;
let value12 = new Uint8Array([11, 12, 13, 14, 15]);

const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  'NAME': value5,
  'AGE': value6,
  'SALARY': value7,
  'CODES': value8
};
const valueBucket3: relationalStore.ValuesBucket = {
  'NAME': value9,
  'AGE': value10,
  'SALARY': value11,
  'CODES': value12
};

let valueBuckets = new Array(valueBucket1, valueBucket2, valueBucket3);
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.batchInsertWithConflictResolution("EMPLOYEE", valueBuckets, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE).then((insertNum: number) => {
      transaction.commit();
      console.info(`batchInsert is successful, the number of values that were inserted = ${insertNum}`);
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`batchInsert is failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### batchInsertWithConflictResolutionSync<sup>18+</sup>

batchInsertWithConflictResolutionSync(table: string, values: Array&lt;ValuesBucket&gt;, conflict: ConflictResolution): number

Inserts a batch of data into a table.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                      | Mandatory| Description                        |
| ------ | ------------------------------------------ | ---- | ---------------------------- |
| table  | string                                     | Yes  | Name of the target table.            |
| values | Array&lt;[ValuesBucket](#valuesbucket)&gt; | Yes  | An array of data to insert.|
| conflict | [ConflictResolution](#conflictresolution10) | Yes  | Resolution used to resolve the conflict. If **ON_CONFLICT_ROLLBACK** is used, the transaction will be rolled back when a conflict occurs.|

**Return value**

| Type  | Description                                          |
| ------ | ---------------------------------------------- |
| number | If the operation is successful, the number of inserted data records is returned. Otherwise, **-1** is returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800022  | SQLite: Callback routine requested an abort. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800032  | SQLite: Abort due to constraint violation. |
| 14800033  | SQLite: Data type mismatch. |
| 14800034  | SQLite: Library used incorrectly. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
let value1 = "Lisa";
let value2 = 18;
let value3 = 100.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);
let value5 = "Jack";
let value6 = 19;
let value7 = 101.5;
let value8 = new Uint8Array([6, 7, 8, 9, 10]);
let value9 = "Tom";
let value10 = 20;
let value11 = 102.5;
let value12 = new Uint8Array([11, 12, 13, 14, 15]);

const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  'NAME': value5,
  'AGE': value6,
  'SALARY': value7,
  'CODES': value8
};
const valueBucket3: relationalStore.ValuesBucket = {
  'NAME': value9,
  'AGE': value10,
  'SALARY': value11,
  'CODES': value12
};

let valueBuckets = new Array(valueBucket1, valueBucket2, valueBucket3);
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    try {
      let insertNum: number = (transaction as relationalStore.Transaction).batchInsertWithConflictResolutionSync("EMPLOYEE", valueBuckets, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE);
      transaction.commit();
      console.info(`batchInsert is successful, the number of values that were inserted = ${insertNum}`);
    } catch (e) {
      transaction.rollback();
      console.error(`batchInsert is failed, code is ${e.code},message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### update<sup>14+</sup>

update(values: ValuesBucket, predicates: RdbPredicates, conflict?: ConflictResolution): Promise&lt;number&gt;

Updates data based on the specified **RdbPredicates** object. This API uses a promise to return the result. Due to the limit of the shared memory (max. 2 MB), a single data record cannot exceed 2 MB. Otherwise, the query operation will fail.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                       | Mandatory| Description                                                        |
| ---------- | ------------------------------------------- | ---- | ------------------------------------------------------------ |
| values     | [ValuesBucket](#valuesbucket)               | Yes  | Rows of data to update in the RDB store. The key-value pair is associated with the column name in the target table.|
| predicates | [RdbPredicates](#rdbpredicates)            | Yes  | Update conditions specified by the **RdbPredicates** object.                     |
| conflict   | [ConflictResolution](#conflictresolution10) | No  | Resolution used to resolve the conflict. <br>Default value: **relationalStore.ConflictResolution.ON_CONFLICT_NONE**.                                         |

**Return value**

| Type                 | Description                                     |
| --------------------- | ----------------------------------------- |
| Promise&lt;number&gt; | Promise used to return the number of rows updated.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800033  | SQLite: Data type mismatch. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
let value1 = "Rose";
let value2 = 22;
let value3 = 200.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);

// You can use either of the following:
const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  NAME: value1,
  AGE: value2,
  SALARY: value3,
  CODES: value4
};
const valueBucket3: relationalStore.ValuesBucket = {
  "NAME": value1,
  "AGE": value2,
  "SALARY": value3,
  "CODES": value4
};

let predicates = new relationalStore.RdbPredicates('EMPLOYEE');
predicates.equalTo("NAME", "Lisa");

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.update(valueBucket1, predicates, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE).then(async (rows: Number) => {
      transaction.commit();
      console.info(`Updated row count: ${rows}`);
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`Updated failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### updateSync<sup>14+</sup>

updateSync(values: ValuesBucket, predicates: RdbPredicates, conflict?: ConflictResolution): number

Updates data in the database based on the specified **RdbPredicates** instance. Due to the limit of the shared memory (max. 2 MB), a single data record cannot exceed 2 MB. Otherwise, the query operation will fail.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                       | Mandatory| Description                                                        |
| ---------- | ------------------------------------------- | ---- | ------------------------------------------------------------ |
| values     | [ValuesBucket](#valuesbucket)               | Yes  | Rows of data to update in the RDB store. The key-value pair is associated with the column name in the target table.|
| predicates | [RdbPredicates](#rdbpredicates)             | Yes  | Update conditions specified by the **RdbPredicates** object.                     |
| conflict   | [ConflictResolution](#conflictresolution10) | No  | Resolution used to resolve the conflict. <br>Default value: **relationalStore.ConflictResolution.ON_CONFLICT_NONE**.|

**Return value**

| Type  | Description              |
| ------ | ------------------ |
| number | return the number of rows updated.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000     | Inner error.                                                 |
| 14800011     | Failed to open the database because it is corrupted.                                          |
| 14800014     | The RdbStore or ResultSet is already closed.                                              |
| 14800021     | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.                                       |
| 14800023     | SQLite: Access permission denied.                            |
| 14800024     | SQLite: The database file is locked.                         |
| 14800025     | SQLite: A table in the database is locked.                   |
| 14800026     | SQLite: The database is out of memory.                       |
| 14800027     | SQLite: Attempt to write a readonly database.                |
| 14800028     | SQLite: Some kind of disk I/O error occurred.                |
| 14800029     | SQLite: The database is full.                                |
| 14800031     | SQLite: TEXT or BLOB exceeds size limit.                     |
| 14800033     | SQLite: Data type mismatch.                                  |
| 14800047     | The WAL file size exceeds the default limit.                 |

**Example**

```ts
let value1 = "Rose";
let value2 = 22;
let value3 = 200.5;
let value4 = new Uint8Array([1, 2, 3, 4, 5]);

// You can use either of the following:
const valueBucket1: relationalStore.ValuesBucket = {
  'NAME': value1,
  'AGE': value2,
  'SALARY': value3,
  'CODES': value4
};
const valueBucket2: relationalStore.ValuesBucket = {
  NAME: value1,
  AGE: value2,
  SALARY: value3,
  CODES: value4
};
const valueBucket3: relationalStore.ValuesBucket = {
  "NAME": value1,
  "AGE": value2,
  "SALARY": value3,
  "CODES": value4
};

let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    try {
      let rows: Number = (transaction as relationalStore.Transaction).updateSync(valueBucket1, predicates, relationalStore.ConflictResolution.ON_CONFLICT_REPLACE);
      transaction.commit();
      console.info(`Updated row count: ${rows}`);
    } catch (e) {
      transaction.rollback();
      console.error(`Updated failed, code is ${e.code},message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### delete<sup>14+</sup>

delete(predicates: RdbPredicates):Promise&lt;number&gt;

Deletes data from the RDB store based on the specified **RdbPredicates** object. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                | Mandatory| Description                                     |
| ---------- | ------------------------------------ | ---- | ----------------------------------------- |
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Conditions specified by the **RdbPredicates** object for deleting data.|

**Return value**

| Type                 | Description                           |
| --------------------- | ------------------------------- |
| Promise&lt;number&gt; | Promise used to return the number of rows deleted.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800033  | SQLite: Data type mismatch. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.delete(predicates).then((rows: Number) => {
      transaction.commit();
      console.info(`Delete rows: ${rows}`);
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`Delete failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### deleteSync<sup>14+</sup>

deleteSync(predicates: RdbPredicates): number

Deletes data from the database based on the specified **RdbPredicates** object.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                           | Mandatory| Description                                   |
| ---------- | ------------------------------- | ---- | --------------------------------------- |
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Conditions specified by the **RdbPredicates** object for deleting data.|

**Return value**

| Type  | Description              |
| ------ | ------------------ |
| number | return the number of rows deleted.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800033  | SQLite: Data type mismatch. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Lisa");

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    try {
      let rows: Number = (transaction as relationalStore.Transaction).deleteSync(predicates);
      transaction.commit();
      console.info(`Delete rows: ${rows}`);
    } catch (e) {
      transaction.rollback();
      console.error(`Delete failed, code is ${e.code},message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### query<sup>14+</sup>

query(predicates: RdbPredicates, columns?: Array&lt;string&gt;): Promise&lt;ResultSet&gt;

Queries data from the RDB store based on specified conditions. This API uses a promise to return the result. Due to the limit of the shared memory (max. 2 MB), a single data record cannot exceed 2 MB. Otherwise, the query operation will fail.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                                | Mandatory| Description                                            |
| ---------- | ------------------------------------ | ---- | ------------------------------------------------ |
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Query conditions specified by the **RdbPredicates** object.       |
| columns    | Array&lt;string&gt;                  | No  | Columns to query. If this parameter is not specified, the query applies to all columns.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800047  | The WAL file size exceeds the default limit. |

**Return value**

| Type                                                   | Description                                              |
| ------------------------------------------------------- | -------------------------------------------------- |
| Promise&lt;[ResultSet](#resultset)&gt; | Promise used to return the result. If the operation is successful, a **ResultSet** object will be returned.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose");

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.query(predicates, ["ID", "NAME", "AGE", "SALARY", "CODES"]).then(async (resultSet: relationalStore.ResultSet) => {
      console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
      // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
      while (resultSet.goToNextRow()) {
        const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
        const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
        const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
        const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
        console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
      }
      // Release the memory of resultSet. If the memory is not released, FD or memory leaks may occur.
      resultSet.close();
      transaction.commit();
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`Query failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### querySync<sup>14+</sup>

querySync(predicates: RdbPredicates, columns?: Array&lt;string&gt;): ResultSet

Queries data in a database based on specified conditions. This API returns the result synchronously. If complex logic and a large number of loops are involved in the operations on the **resultSet** obtained by **querySync()**, the freeze problem may occur. You are advised to perform this operation in the [taskpool](../apis-arkts/js-apis-taskpool.md) thread.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name    | Type                           | Mandatory| Description                                                        |
| ---------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| predicates | [RdbPredicates](#rdbpredicates) | Yes  | Query conditions specified by the **RdbPredicates** object.                     |
| columns    | Array&lt;string&gt;             | No  | Columns to query. If this parameter is not specified, the query applies to all columns. <br>Default value: null.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800047  | The WAL file size exceeds the default limit. |

**Return value**

| Type                   | Description                               |
| ----------------------- | ----------------------------------- |
| [ResultSet](#resultset) | If the operation is successful, a **ResultSet** object will be returned.|

**Example**

```ts
let predicates = new relationalStore.RdbPredicates("EMPLOYEE");
predicates.equalTo("NAME", "Rose");

if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then(async (transaction: relationalStore.Transaction) => {
    try {
      let resultSet: relationalStore.ResultSet = (transaction as relationalStore.Transaction).querySync(predicates, ["ID", "NAME", "AGE", "SALARY", "CODES"]);
      console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
      // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
      while (resultSet.goToNextRow()) {
        const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
        const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
        const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
        const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
        console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
      }
      // Release the memory of resultSet. If the memory is not released, FD or memory leaks may occur.
      resultSet.close();
      transaction.commit();
    } catch (e) {
      transaction.rollback();
      console.error(`Query failed, code is ${e.code},message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### querySql<sup>14+</sup>

querySql(sql: string, args?: Array&lt;ValueType&gt;): Promise&lt;ResultSet&gt;

Queries data in the RDB store using the specified SQL statement. The number of relational operators between expressions and operators in the SQL statement cannot exceed 1000. This API uses a promise to return the result.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                | Mandatory| Description                                                        |
| -------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql      | string                               | Yes  | SQL statement to run.                                       |
| args | Array&lt;[ValueType](#valuetype)&gt; | No  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If the SQL parameter statement is complete, leave this parameter blank.|

**Return value**

| Type                                                   | Description                                              |
| ------------------------------------------------------- | -------------------------------------------------- |
| Promise&lt;[ResultSet](#resultset)&gt; | Promise used to return the result. If the operation is successful, a **ResultSet** object will be returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.querySql("SELECT * FROM EMPLOYEE CROSS JOIN BOOK WHERE BOOK.NAME = 'sanguo'").then(async (resultSet: relationalStore.ResultSet) => {
      console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
      // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
      while (resultSet.goToNextRow()) {
        const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
        const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
        const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
        const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
        console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
      }
      // Release the memory of resultSet. If the memory is not released, FD or memory leaks may occur.
      resultSet.close();
      transaction.commit();
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`Query failed, code is ${e.code},message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### querySqlSync<sup>14+</sup>

querySqlSync(sql: string, args?: Array&lt;ValueType&gt;): ResultSet

Queries data in the RDB store using the specified SQL statement. The number of relational operators between expressions and operators in the SQL statement cannot exceed 1000. If complex logic and a large number of loops are involved in the operations on the **resultSet** obtained by **querySync()**, the freeze problem may occur. You are advised to perform this operation in the [taskpool](../apis-arkts/js-apis-taskpool.md) thread.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                | Mandatory| Description                                                        |
| -------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql      | string                               | Yes  | SQL statement to run.                                       |
| args | Array&lt;[ValueType](#valuetype)&gt; | No  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If the SQL parameter statement is complete, leave this parameter blank. <br>Default value: null.|

**Return value**

| Type                   | Description                               |
| ----------------------- | ----------------------------------- |
| [ResultSet](#resultset) | If the operation is successful, a **ResultSet** object will be returned.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then(async (transaction: relationalStore.Transaction) => {
    try {
      let resultSet: relationalStore.ResultSet = (transaction as relationalStore.Transaction).querySqlSync("SELECT * FROM EMPLOYEE CROSS JOIN BOOK WHERE BOOK.NAME = 'sanguo'");
      console.info(`ResultSet column names: ${resultSet.columnNames}, column count: ${resultSet.columnCount}`);
      // resultSet is a cursor of a data set. By default, the cursor points to the -1st record. Valid data starts from 0.
      while (resultSet.goToNextRow()) {
        const id = resultSet.getLong(resultSet.getColumnIndex("ID"));
        const name = resultSet.getString(resultSet.getColumnIndex("NAME"));
        const age = resultSet.getLong(resultSet.getColumnIndex("AGE"));
        const salary = resultSet.getDouble(resultSet.getColumnIndex("SALARY"));
        console.info(`id=${id}, name=${name}, age=${age}, salary=${salary}`);
      }
      // Release the memory of resultSet. If the memory is not released, FD or memory leaks may occur.
      resultSet.close();
      transaction.commit();
    } catch (e) {
      transaction.rollback();
      console.error(`Query failed, code is ${e.code},message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### execute<sup>14+</sup>

execute(sql: string, args?: Array&lt;ValueType&gt;): Promise&lt;ValueType&gt;

Executes an SQL statement that contains specified arguments. The number of relational operators between expressions and operators in the statement cannot exceed 1000. This API uses a promise to return a value of the ValueType type.

This API can be used to add, delete, and modify data, run SQL statements of the PRAGMA syntax, and create, delete, and modify a table. The type of the return value varies, depending on the execution result.

This API does not support query, database attachment, and transaction operations. You can use [querySql](#querysql14) or [query](#query14) to query data, and use [attach](#attach12) to attach a database.

Statements separated by semicolons (\;) are not supported.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name  | Type                                | Mandatory| Description                                                        |
| -------- | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql      | string                               | Yes  | SQL statement to run.                                       |
| args | Array&lt;[ValueType](#valuetype)&gt; | No  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If the SQL parameter statement is complete, leave this parameter blank.|

**Return value**

| Type               | Description                     |
| ------------------- | ------------------------- |
| Promise&lt;[ValueType](#valuetype)&gt; | Promise used to return the SQL execution result.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
|-----------| ------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 801       | Capability not supported the sql(attach,begin,commit,rollback etc.). |
| 14800000  | Inner error. |
| 14800011  | Failed to open the database because it is corrupted. |
| 14800014  | The RdbStore or ResultSet is already closed. |
| 14800021  | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist. |
| 14800023  | SQLite: Access permission denied. |
| 14800024  | SQLite: The database file is locked. |
| 14800025  | SQLite: A table in the database is locked. |
| 14800026  | SQLite: The database is out of memory. |
| 14800027  | SQLite: Attempt to write a readonly database. |
| 14800028  | SQLite: Some kind of disk I/O error occurred. |
| 14800029  | SQLite: The database is full. |
| 14800031  | SQLite: TEXT or BLOB exceeds size limit. |
| 14800033  | SQLite: Data type mismatch. |
| 14800047  | The WAL file size exceeds the default limit. |

**Example**

```ts
// Delete all data from the table.
if (store != undefined) {
  const SQL_DELETE_TABLE = 'DELETE FROM test';
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    transaction.execute(SQL_DELETE_TABLE).then((data) => {
      transaction.commit();
      console.info(`delete result: ${data}`);
    }).catch((e: BusinessError) => {
      transaction.rollback();
      console.error(`delete failed, code is ${e.code}, message is ${e.message}`);
    });
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```

### executeSync<sup>14+</sup>

executeSync(sql: string, args?: Array&lt;ValueType&gt;): ValueType

Executes an SQL statement that contains specified arguments. The number of relational operators between expressions and operators in the statement cannot exceed 1000. This API returns a value of theValueType type.

This API can be used to add, delete, and modify data, run SQL statements of the PRAGMA syntax, and create, delete, and modify a table. The type of the return value varies, depending on the execution result.

This API does not support query, database attachment, and transaction operations. You can use [querySql](#querysql14) or [query](#query14) to query data, and use [attach](#attach12) to attach a database.

Statements separated by semicolons (\;) are not supported.

**System capability**: SystemCapability.DistributedDataManager.RelationalStore.Core

**Parameters**

| Name| Type                                | Mandatory| Description                                                        |
| ------ | ------------------------------------ | ---- | ------------------------------------------------------------ |
| sql    | string                               | Yes  | SQL statement to run.                                       |
| args   | Array&lt;[ValueType](#valuetype)&gt; | No  | Arguments in the SQL statement. The value corresponds to the placeholders in the SQL parameter statement. If this parameter is left blank or set to **null** or **undefined**, the SQL statement is complete. <br>Default value: null.|

**Return value**

| Type                   | Description               |
| ----------------------- | ------------------- |
| [ValueType](#valuetype) | SQL execution result.|

**Error codes**

For details about the error codes, see [Universal Error Codes](../errorcode-universal.md) and [RDB Store Error Codes](errorcode-data-rdb.md). For details about how to handle error 14800011, see [Database Backup and Restore](../../database/data-backup-and-restore.md).

| **ID**| **Error Message**                                                |
| ------------ | ------------------------------------------------------------ |
| 401          | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 801       | Capability not supported the sql(attach,begin,commit,rollback etc.). |
| 14800000     | Inner error.                                                 |
| 14800011     | Failed to open the database because it is corrupted.                                          |
| 14800014     | The RdbStore or ResultSet is already closed.                                              |
| 14800021     | SQLite: Generic error. Possible causes: Insert failed or the updated data does not exist.                                       |
| 14800023     | SQLite: Access permission denied.                            |
| 14800024     | SQLite: The database file is locked.                         |
| 14800025     | SQLite: A table in the database is locked.                   |
| 14800026     | SQLite: The database is out of memory.                       |
| 14800027     | SQLite: Attempt to write a readonly database.                |
| 14800028     | SQLite: Some kind of disk I/O error occurred.                |
| 14800029     | SQLite: The database is full.                                |
| 14800031     | SQLite: TEXT or BLOB exceeds size limit.                     |
| 14800033     | SQLite: Data type mismatch.                                  |
| 14800047     | The WAL file size exceeds the default limit.                 |

**Example**

```ts
// Delete all data from the table.
if (store != undefined) {
  (store as relationalStore.RdbStore).createTransaction().then((transaction: relationalStore.Transaction) => {
    const SQL_DELETE_TABLE = 'DELETE FROM test';
    try {
      let data = (transaction as relationalStore.Transaction).executeSync(SQL_DELETE_TABLE);
      transaction.commit();
      console.info(`delete result: ${data}`);
    } catch (e) {
      transaction.rollback();
      console.error(`delete failed, code is ${e.code}, message is ${e.message}`);
    };
  }).catch((err: BusinessError) => {
    console.error(`createTransaction failed, code is ${err.code},message is ${err.message}`);
  });
}
```
