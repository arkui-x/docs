# 数据库备份与恢复 (ArkTS)
## 场景介绍

如果操作或存储的过程中出现问题，开发者可以使用恢复功能，将数据库恢复到之前的状态，重新对数据库进行操作。

在数据库被篡改、删除、或者设备断电场景下，数据库可能会因为数据丢失、数据损坏、脏数据等而不可用，可以通过数据库的备份恢复能力将数据库恢复至可用状态。

键值型数据库和关系型数据库均支持对数据库的备份和恢复。另外，键值型数据库还支持删除数据库备份，以释放本地存储空间。

## 键值型数据库备份、恢复与删除

键值型数据库，通过backup接口实现数据库备份，通过restore接口实现数据库恢复，通过deletebackup接口删除数据库备份。具体接口及功能，可见[分布式键值数据库](./js-apis-distributedKVStore.md)。

1. 创建数据库。

   (1) 创建kvManager。

   (2) 配置数据库参数。

   (3) 创建kvStore。

   EntryAbility.ets

   ```ts
   export default class EntryAbility extends UIAbility {
       onCreate(want: Want, launchParam: AbilityConstant.LaunchParam) {
           // 创建名称为EntryAbilityContext的context
           AppStorage.setOrCreate('EntryAbilityContext', this.context);
       }
   }
   ```

   KvStoreInterface.ets

   ```ts
   // 导入模块
   // 在pages目录下新建KvStoreInterface.ets
   import { distributedKVStore } from '@kit.ArkData';
   import { BusinessError } from '@kit.BasicServicesKit';
   import common from '@ohos.app.ability.common';
   
   let kvManager: distributedKVStore.KVManager | undefined = undefined;
   let kvStore: distributedKVStore.SingleKVStore | undefined = undefined;
   let appId: string = 'com.example.kvstoresamples';
   let storeId: string = 'storeId';
   // 引用名称为EntryAbilityContext的context
   const context = AppStorage.get<common.UIAbilityContext>("EntryAbilityContext") as common.UIAbilityContext;
   
   // 下面所有接口的代码都实现在KvInterface中
   export class KvInterface {
   }
   ```
   ``` TypeScript
   public CreateKvManager = (() => {
     console.info('CreateKvManager start');
     if (typeof (kvManager) === 'undefined') {
       const kvManagerConfig: distributedKVStore.KVManagerConfig = {
         bundleName: appId,
         context: this.context
       };
       try {
         // 创建KVManager实例
         kvManager = distributedKVStore.createKVManager(kvManagerConfig);
         console.info('Succeeded in creating KVManager.');
       } catch (err) {
         console.error(`Failed to create KVManager. Code:${err.code},message:${err.message}`);
       }
     } else {
       console.info ('KVManager has created');
     }
   })
   ```
   

   ``` TypeScript
   public GetKvStore = (() => {
     console.info('GetKvStore start');
     if (kvManager === undefined) {
       console.info('KvManager not initialized');
       return;
     }
     try {
       let child1 = new distributedKVStore.FieldNode('id');
       child1.type = distributedKVStore.ValueType.INTEGER;
       child1.nullable = false;
       child1.default = '1';
       let child2 = new distributedKVStore.FieldNode('name');
       child2.type = distributedKVStore.ValueType.STRING;
       child2.nullable = false;
       child2.default = 'zhangsan';
   
       let schema = new distributedKVStore.Schema();
       schema.root.appendChild(child1);
       schema.root.appendChild(child2);
       schema.indexes = ['$.id', '$.name'];
       // 0表示COMPATIBLE模式，1表示STRICT模式。
       schema.mode = 1;
       // 支持在检查Value时，跳过skip指定的字节数，且取值范围为[0,4M-2]。
       schema.skip = 0;
   
       const options: distributedKVStore.Options = {
         createIfMissing: true,
         // kvStoreType 需要创建单版本数据库。
         kvStoreType: distributedKVStore.KVStoreType.SINGLE_VERSION,
         schema: schema,
         // schema未定义可以不填，定义方法请参考上方schema示例。
         securityLevel: distributedKVStore.SecurityLevel.S3
       };
       kvManager.getKVStore<distributedKVStore.SingleKVStore>(storeId, options,
         (err, store: distributedKVStore.SingleKVStore) => {
           if (err) {
             console.error(`Failed to get KVStore: Code:${err.code},message:${err.message}`);
             return;
           }
           console.info('Succeeded in getting KVStore.');
           kvStore = store;
           // 请确保获取到键值数据库实例后，再进行相关数据操作
         });
     } catch (e) {
       let error = e as BusinessError;
       console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
     }
   })
   ```

2. 使用put()方法插入数据。

   ``` TypeScript
   public Put = (() => {
     console.info('Put start');
     if (kvStore === undefined) {
       console.info('Put: kvStore not initialized');
       return;
     }
     const KEY_TEST_STRING_ELEMENT = 'key_test_string';
     const VALUE_TEST_STRING_ELEMENT = '{"id":0, "name":"lisi"}';
     try {
       kvStore.put(KEY_TEST_STRING_ELEMENT, VALUE_TEST_STRING_ELEMENT, (err) => {
         if (err !== undefined) {
           console.error(`Failed to put data. Code:${err.code},message:${err.message}`);
           return;
         }
         console.info('Succeeded in putting data.');
       });
     } catch (e) {
       let error = e as BusinessError;
       console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
     }
   })
   ```
   
3. 使用backup()方法备份数据。

   ``` TypeScript
   public Backup = (() => {
     console.info('Backup start');
     if (kvStore === undefined) {
       console.info('Backup: kvStore not initialized');
       return;
     }
     let backupFile = 'BK001';
     try {
       kvStore.backup(backupFile, (err) => {
         if (err) {
           console.error(`Fail to backup data.code:${err.code},message:${err.message}`);
         } else {
           console.info('Succeeded in backing up data.');
         }
       });
     } catch (e) {
       let error = e as BusinessError;
       console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
     }
   })
   ```
   
4. 使用delete()方法删除数据（模拟意外删除、篡改场景）。

   ``` TypeScript
   public Delete = (() => {
     console.info('DeleteData start');
     if (kvStore === undefined) {
       console.info('DeleteData: kvStore not initialized');
       return;
     }
     const KEY_TEST_STRING_ELEMENT = 'key_test_string';
     try {
       kvStore.delete(KEY_TEST_STRING_ELEMENT, (err) => {
         if (err !== undefined) {
           console.error(`Failed to delete data. Code:${err.code},message:${err.message}`);
           return;
         }
         console.info('Succeeded in deleting data.');
       });
     } catch (e) {
       let error = e as BusinessError;
       console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
     }
   })
   ```
   
5. 使用restore()方法恢复数据。

   ``` TypeScript
   public Restore = (() => {
     console.info('Restore start');
     if (kvStore === undefined) {
       console.info('Restore: kvStore not initialized');
       return;
     }
     let backupFile = 'BK001';
     try {
       kvStore.restore(backupFile, (err) => {
         if (err) {
           console.error(`Fail to restore data. Code:${err.code},message:${err.message}`);
         } else {
           console.info('Succeeded in restoring data.');
         }
       });
     } catch (e) {
       let error = e as BusinessError;
       console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
     }
   })
   ```
   
6. 当本地设备存储空间有限或需要重新备份时，还可使用deleteBackup()方法删除备份，释放存储空间。

   ``` TypeScript
   public DeleteBackup = (() => {
     console.info('DeleteBackup start');
     if (kvStore === undefined) {
       console.info('DeleteBackup: kvStore not initialized');
       return;
     }
     let backupFile = 'BK001';
     let files = [backupFile];
     try {
       kvStore.deleteBackup(files, (err: BusinessError, data: [string, number][]) => {
         if (err) {
           console.error(`Failed to delete Backup.code is ${err.code},message is ${err.message}`);
         } else {
           console.info(`Succeed in deleting Backup.data=${data}`);
         }
       });
     } catch (e) {
       let error = e as BusinessError;
       console.error(`An unexpected error occurred.code is ${error.code},message is ${error.message}`);
     }
   })
   ```
