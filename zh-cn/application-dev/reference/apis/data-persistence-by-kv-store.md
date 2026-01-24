# 通过键值型数据库实现数据持久化 (ArkTS)

## 场景介绍

键值型数据库存储键值对形式的数据，当需要存储的数据没有复杂的关系模型，比如存储商品名称及对应价格、员工工号及今日是否已出勤等，由于数据复杂度低，更容易兼容不同数据库版本和设备类型，因此推荐使用键值型数据库持久化此类数据。


## 约束限制

- 单版本数据库，针对每条记录，Key的长度≤1 KB，Value的长度&lt;4 MB。

- 每个应用程序最多支持同时打开16个键值型分布式数据库。

- 键值型数据库事件回调方法中不允许进行阻塞操作，例如修改UI组件。


## 接口说明

以下是键值型数据库持久化功能的相关接口，大部分为异步接口。异步接口均有callback和Promise两种返回形式，下表均以callback形式为例，更多接口及使用方式请见[分布式键值数据库](./js-apis-distributedKVStore.md)。

| 接口名称 | 描述 |
| -------- | -------- |
| createKVManager(config: KVManagerConfig): KVManager | 创建一个KVManager对象实例，用于管理数据库对象。 |
| getKVStore&lt;T&gt;(storeId: string, options: Options, callback: AsyncCallback&lt;T&gt;): void | 指定options和storeId，创建并得到指定类型的KVStore数据库。 |
| put(key: string, value: Uint8Array \| string \| number \| boolean, callback: AsyncCallback&lt;void&gt;): void | 添加指定类型的键值对到数据库。 |
| get(key: string, callback: AsyncCallback&lt;boolean \| string \| number \| Uint8Array&gt;): void | 获取指定键的值。 |
| delete(key: string, callback: AsyncCallback&lt;void&gt;): void | 从数据库中删除指定键值的数据。 |
| closeKVStore(appId: string, storeId: string, callback: AsyncCallback&lt;void&gt;): void | 通过storeId的值关闭指定的分布式键值数据库。 |
| deleteKVStore(appId: string, storeId: string, callback: AsyncCallback&lt;void&gt;): void | 通过storeId的值删除指定的分布式键值数据库。 |


## 开发步骤

1. 若要使用键值型数据库，首先要使用createKVManager()方法获取一个KVManager实例，用于管理数据库对象。示例代码如下所示：

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

2. 使用getKVStore()方法创建并获取键值数据库。示例代码如下所示：

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

3. 调用put()方法向键值数据库中插入数据。示例代码如下所示：

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

   > **说明：**
   >
   > 当Key值存在时，put()方法会修改其值，否则新增一条数据。

4. 调用get()方法获取指定键的值。示例代码如下所示：

   ``` TypeScript
   public Get = (() => {
     console.info('Get start');
     if (kvStore === undefined) {
       console.info('Get: kvStore not initialized');
       return;
     }
     const KEY_TEST_STRING_ELEMENT = 'key_test_string';
     try {
       kvStore.get(KEY_TEST_STRING_ELEMENT, (err, data) => {
         if (err != undefined) {
           console.error(`Failed to get data. Code:${err.code},message:${err.message}`);
           return;
         }
         console.info(`Succeeded in getting data. Data:${data}`);
       });
     } catch (e) {
       let error = e as BusinessError;
       console.error(`Failed to get data. Code:${error.code},message:${error.message}`);
     }
   })
   ```

5. 调用delete()方法删除指定键值的数据。示例代码如下所示：

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

6. 调用closeKVStore()方法通过storeId的值关闭指定的分布式键值数据库。示例代码如下所示：

    ``` TypeScript
    public CloseKVStore = (()=>{
      console.info('CloseKVStore start');
      if (kvManager === undefined) {
        console.info('KvManager not initialized');
        return;
      }
      try {
        // appId为应用的bundleName
        kvStore = undefined;
        kvManager.closeKVStore(appId, storeId, (err: BusinessError)=> {
          if (err) {
            console.error(`Failed to close KVStore.code is ${err.code},message is ${err.message}`);
            return;
          }
          console.info('Succeeded in closing KVStore');
        });
      } catch (e) {
        let error = e as BusinessError;
        console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
      }
    })
    ```

7. 调用deleteKVStore()方法通过storeId的值删除指定的分布式键值数据库。示例代码如下所示：

    ``` TypeScript
    public DeleteKvStore = (()=>{
      console.info('DeleteKvStore start');
      if (kvManager === undefined) {
        console.info('KvManager not initialized');
        return;
      }
      try {
        // appId为应用的bundleName
        kvStore = undefined;
        kvManager.deleteKVStore(appId, storeId, (err: BusinessError)=> {
          if (err) {
            console.error(`Failed to delete KVStore.code is ${err.code},message is ${err.message}`);
            return;
          }
          console.info('Succeeded in deleting KVStore');
        });
      } catch (e) {
        let error = e as BusinessError;
        console.error(`An unexpected error occurred. Code:${error.code},message:${error.message}`);
      }
    })
    ```
