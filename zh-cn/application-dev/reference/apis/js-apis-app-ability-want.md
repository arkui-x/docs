# @ohos.app.ability.Want (Want)

Want是对象间信息传递的载体，可以用于应用组件间的信息传递。Want的使用场景之一是作为startAbility的参数，其包含了指定的启动目标，以及启动时需携带的相关数据，例如bundleName和abilityName字段分别指明目标Ability所在应用的包名以及对应包内的Ability名称。当UIAbilityA需要启动UIAbilityB并传入一些数据时，可使用Want作为载体将这些数据传递给UIAbilityB。

> **说明：**
>
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import Want from '@ohos.app.ability.Want';
```

## 属性

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityBase

| 名称        | 类型                 | 必填 | 说明                                                         |
| ----------- | -------------------- | ---- | ------------------------------------------------------------ |
| bundleName   | string               | 是   | 表示待启动Ability所在的应用Bundle名称。 |
| moduleName | string | 是 | 表示待启动的Ability所属的模块名称。 |
| abilityName  | string               | 是   | 表示待启动Ability名称。 |
| parameters   | {[key: string]: any} | 否   | 表示WantParams描述，由开发者自行决定传入的键值对。|

**示例：**

- 基础用法：在UIAbility对象中调用，示例中的context的获取方式请参见[获取UIAbility的上下文信息](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/application-models/uiability-usage.md#%E8%8E%B7%E5%8F%96uiability%E7%9A%84%E4%B8%8A%E4%B8%8B%E6%96%87%E4%BF%A1%E6%81%AF)。

  ```ts
  import common from '@ohos.app.ability.common';
  let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
  let want = {
    'bundleName': 'com.example.myapplication',
    'abilityName': 'FuncAbility',
    'moduleName': 'entry' // moduleName非必选
  };
  
  context.startAbility(want, (err) => {
    // 显式拉起Ability，通过bundleName、abilityName和moduleName可以唯一确定一个Ability
    console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
  });
  ```

- 目前parameters支持的数据类型有：字符串、数字、布尔等。

    * 字符串（String）
        ```ts
        import common from '@ohos.app.ability.common';
        let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
        let want = {
          bundleName: 'com.example.myapplication',
          abilityName: 'FuncAbility',
          parameters: {
            keyForString: 'str',
          },
        };
        
        context.startAbility(want, (err) => {
          console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
        });
        ```
    * 数字（Number）
        ```ts
        import common from '@ohos.app.ability.common';
        let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
        let want = {
          bundleName: 'com.example.myapplication',
          abilityName: 'FuncAbility',
          parameters: {
            keyForInt: 100,
            keyForDouble: 99.99,
          },
        };
        
        context.startAbility(want, (err) => {
          console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
        });
        ```
    * 布尔（Boolean）
        ```ts
        import common from '@ohos.app.ability.common';
        let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
        let want = {
          bundleName: 'com.example.myapplication',
          abilityName: 'FuncAbility',
          parameters: {
            keyForBool: true,
          },
        };
        
        context.startAbility(want, (err) => {
          console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
        });
        ```
