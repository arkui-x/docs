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
| bundleName   | string               | 否   | 表示待启动Ability所在的应用Bundle名称。 |
| moduleName | string | 否 | 表示待启动的Ability所属的模块名称。 |
| abilityName  | string               | 否   | 表示待启动Ability名称。如果在Want中该字段同时指定了BundleName和AbilityName，则Want可以直接匹配到指定的Ability。AbilityName需要在一个应用的范围内保证唯一。 |
| parameters   | {[key: string]: any} | 否   | 表示WantParams描述，由开发者自行决定传入的键值对。|

**示例：**

- 基础用法：在UIAbility对象中调用。

  ```ts
  import common from '@ohos.app.ability.common';
  let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
  let want = {
    'deviceId': '', // deviceId为空表示本设备
    'bundleName': 'com.example.myapplication',
    'abilityName': 'FuncAbility',
    'moduleName': 'entry' // moduleName非必选
  };
  
  context.startAbility(want, (err) => {
    // 显式拉起Ability，通过bundleName、abilityName和moduleName可以唯一确定一个Ability
    console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
  });
  ```

- 目前支持的数据类型有：字符串、数字、布尔等。

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
    - parameter参数用法：以ability.params.backToOtherMissionStack为例，ServiceExtension在拉起UIAbility的时候，可以支持跨任务链返回。

    ```ts
        // (1) UIAbility1启动一个ServiceExtension
        let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
        let want = {
          bundleName: 'com.example.myapplication1',
          abilityName: 'ServiceExtensionAbility',
        };
        context.startAbility(want, (err) => {
          console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
        });
      ```
    ```ts

        // (2) 该ServiceExtension去启动另一个UIAbility2，并在启动的时候携带参数ability.params.backToOtherMissionStack为true
        let context ; // ServiceExtensionContext
        let want = {
          bundleName: 'com.example.myapplication2',
          abilityName: 'MainAbility',
          parameters: {
            "ability.params.backToOtherMissionStack": true,
          },
        };

        context.startAbility(want, (err) => {
          console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
        });
    ```

    说明：上例中，如果ServiceExtension启动UIAbility2时不携带ability.params.backToOtherMissionStack参数，或者携带的ability.params.backToOtherMissionStack参数为false，则UIAbility1和UIAbility2不在同一个任务栈里面，在UIAbility2的界面点back键，不会回到UIAbility1的界面。如果携带的ability.params.backToOtherMissionStack参数为true，则表示支持跨任务链返回，此时在UIAbility2的界面点back键，会回到UIAbility1的界面。