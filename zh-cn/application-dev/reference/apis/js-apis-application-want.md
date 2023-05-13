# @ohos.application.Want (Want)

Want是对象间信息传递的载体, 可以用于应用组件间的信息传递。 Want的使用场景之一是作为startAbility的参数, 其包含了指定的启动目标, 以及启动时需携带的相关数据, 如bundleName和abilityName字段分别指明目标Ability所在应用Bundle名称以及对应包内的Ability名称。当Ability A需要启动Ability B并传入一些数据时, 可使用Want作为载体将这些数据传递给Ability B。

> **说明：**
>
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import Want from '@ohos.application.Want';
```

## 属性

| 名称        | 类型                 | 必填 | 说明                                                         |
| ----------- | -------------------- | ---- | ------------------------------------------------------------ |
| bundleName   | string               | 是   | 表示Bundle名称。 |
| moduleName   | string               | 是   | 表示Module名称。 |
| abilityName  | string               | 是   | 表示待启动的Ability名称。 |
| parameters   | {[key: string]: any} | 否   | 表示WantParams描述，由开发者自行决定传入的键值对。 |

**示例：**

- 基础用法(在UIAbility对象中调用，其中示例中的context为UIAbility的上下文对象)

  ```ts
    let want = {
        'bundleName': 'com.example.myapplication',
        'abilityName': 'EntryAbility',
        'moduleName': 'entry' // moduleName非必选
    };
    this.context.startAbility(want, (error) => {
        // 显式拉起Ability，通过bundleName、abilityName和moduleName可以唯一确定一个Ability
        console.error('error.code = ${error.code}');
    });
  ```

- 通过自定字段传递数据, 以下为当前支持类型。(在UIAbility对象中调用，其中示例中的context为UIAbility的上下文对象)

    * 字符串（String）
        ```ts
        let want = {
            bundleName: 'com.example.myapplication',
            abilityName: 'EntryAbility',
            parameters: {
                keyForString: 'str',
            },
        };
        ```
    * 数字（Number）
        ```ts
        let want = {
            bundleName: 'com.example.myapplication',
            abilityName: 'EntryAbility',
            parameters: {
                keyForInt: 100,
                keyForDouble: 99.99,
            },
        };
        ```
    * 布尔（Boolean）
        ```ts
        let want = {
            bundleName: 'com.example.myapplication',
            abilityName: 'EntryAbility',
            parameters: {
                keyForBool: true,
            },
        };
        ```