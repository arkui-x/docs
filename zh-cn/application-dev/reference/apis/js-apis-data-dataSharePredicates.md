# @ohos.data.dataSharePredicates (数据共享谓词)

**谓词(DataSharePredicates)** 是开发者通过DataShare查询数据库中的数据所使用的筛选条件，经常被应用在更新数据、删除数据和查询数据中。

谓词的接口函数与数据库的筛选条件一一对应，开发者在使用前需了解数据库相关知识。

> **说明：** 
>
> - 本模块首批接口从API version 12开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> - 本模块接口仅可在Stage模型下使用。



## 导入模块

```ts
import { dataSharePredicates } from '@kit.ArkData';
```

## DataSharePredicates
提供用于不同实现不同查询方法的数据共享谓词。该类型不是多线程安全的，如果应用中存在多线程同时操作该类派生出的实例，注意加锁保护。

### equalTo

equalTo(field: string, value: ValueType): DataSharePredicates

该接口用于配置谓词以匹配值等于指定值的字段。

目前仅RDB及KVDB(schema)支持该谓词。

**系统能力：**  SystemCapability.DistributedDataManager.DataShare.Core

**参数：**

| 参数名 | 类型                                                | 必填 | 说明                   |
| ------ | --------------------------------------------------- | ---- | ---------------------- |
| field  | string                                              | 是   | 数据库表中的列名。     |
| value  | [ValueType](js-apis-data-valuesBucket.md#valuetype) | 是   | 指示要与谓词匹配的值。 |

**返回值：**

| 类型                                        | 说明                       |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | 返回与指定字段匹配的谓词。 |

**示例：**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.equalTo("NAME", "Rose")
```


### and

and(): DataSharePredicates

该接口用于将和条件添加到谓词中。

目前仅RDB及KVDB(schema)支持该谓词。

**系统能力：**  SystemCapability.DistributedDataManager.DataShare.Core

**返回值：**

| 类型                                        | 说明                   |
| ------------------------------------------- | ---------------------- |
| [DataSharePredicates](#datasharepredicates) | 返回带有和条件的谓词。 |

**示例：**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.equalTo("NAME", "lisi")
    .and()
    .equalTo("SALARY", 200.5)
```

### orderByAsc

orderByAsc(field: string): DataSharePredicates

该接口用于配置谓词以匹配其值按升序排序的列。

目前仅RDB及KVDB(schema)支持该谓词。

**系统能力：**  SystemCapability.DistributedDataManager.DataShare.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| field  | string | 是   | 数据库表中的列名。 |

**返回值：**

| 类型                                        | 说明                       |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | 返回与指定字段匹配的谓词。 |

**示例：**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.orderByAsc("AGE")
```

### orderByDesc

orderByDesc(field: string): DataSharePredicates

该接口用于配置谓词以匹配其值按降序排序的列。

目前仅RDB及KVDB(schema)支持该谓词。

**系统能力：**  SystemCapability.DistributedDataManager.DataShare.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| field  | string | 是   | 数据库表中的列名。 |

**返回值：**

| 类型                                        | 说明                       |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | 返回与指定字段匹配的谓词。 |

**示例：**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.orderByDesc("AGE")
```

### limit

limit(total: number, offset: number): DataSharePredicates

该接口用于配置谓词以指定结果数和起始位置。

目前仅RDB及KVDB(schema)支持该谓词。

**系统能力：**  SystemCapability.DistributedDataManager.DataShare.Core

**参数：**

| 参数名   | 类型   | 必填 | 说明           |
| -------- | ------ | ---- | -------------- |
| total    | number | 是   | 指定结果数。   |
| offset | number | 是   | 指示起始位置。 |

**返回值：**

| 类型                                        | 说明                       |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | 返回与指定字段匹配的谓词。 |

**示例：**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.equalTo("NAME", "Rose").limit(10, 3)
```

### in

in(field: string, value: Array&lt;ValueType&gt;): DataSharePredicates

该接口用于配置谓词以匹配值在指范围内的字段。

目前仅RDB及KVDB(schema)支持该谓词。

**系统能力：**  SystemCapability.DistributedDataManager.DataShare.Core

**参数：**

| 参数名  | 类型             | 必填 | 说明                                    |
| ------- | ---------------- | ---- | --------------------------------------- |
| field   | string           | 是 | 数据库表中的列名。                      |
| value | Array&lt;[ValueType](js-apis-data-valuesBucket.md#valuetype)&gt; | 是   | 以ValueType型数组形式指定的要匹配的值。 |

**返回值：**

| 类型                                        | 说明                       |
| ------------------------------------------- | -------------------------- |
| [DataSharePredicates](#datasharepredicates) | 返回与指定字段匹配的谓词。 |

**示例：**

```ts
let predicates = new dataSharePredicates.DataSharePredicates()
predicates.in("AGE", [18, 20])
```