# **@ohos.app.ability.Want (Want)**

Want是对象间信息传递的载体，可以用于应用组件间的信息传递。Want的使用场景之一是作为startAbility的参数，其包含了指定的启动目标，以及启动时需携带的相关数据，例如bundleName和abilityName字段分别指明目标Ability所在应用的包名以及对应包内的Ability名称。当UIAbilityA需要启动UIAbilityB并传入一些数据时，可使用Want作为载体将这些数据传递给UIAbilityB。

> **说明：**
>
> 本模块首批接口从API version 16 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import Want from '@ohos.app.ability.Want';
```

## 属性

**参数：**

| 名称        | 类型                 | 必填 | 说明                                               |
| ----------- | -------------------- | ---- | -------------------------------------------------- |
| bundleName  | string               | 是   | 表示待启动Ability所在的应用Bundle名称。            |
| moduleName  | string               | 是   | 表示待启动的Ability所属的模块名称。                |
| abilityName | string               | 是   | 表示待启动Ability名称。                            |
| parameters  | {[key: string]: any} | 否   | 表示WantParams描述，由开发者自行决定传入的键值对。 |
**支持平台：** Android、iOS

| 名称        | 类型                 | 必填 | 说明                                               | Android平台 | iOS平台 |
| ----------- | -------------------- | ---- | -------------------------------------------------- | ----------- | ------- |
| bundleName  | string               | 是   | 表示待启动Ability所在的应用Bundle名称。            | 支持        | 支持    |
| moduleName  | string               | 是   | 表示待启动的Ability所属的模块名称。                | 支持        | 支持    |
| abilityName | string               | 是   | 表示待启动Ability名称。                            | 支持        | 支持    |
| type        | string               | 否   | 表示MIME type类型描述，打开文件的类型。            | 支持        | 支持    |
| parameters  | {[key: string]: any} | 否   | 表示WantParams描述，由开发者自行决定传入的键值对。 | 支持        | 支持    |

> **说明：**
>
> bundleName传入"com.ohos.filepicker"拉起文件选择器，传入"com.ohos.photos"拉起图片选择器。
>
> 选择器默认打开全部文件，可以通过type指定要打开文件的类型。
>
> 选择器默认单选。parameters传入的key为"uri"能控制单选多选，当value为"singleselect"时单选，"multipleselect"时多选。

**示例：**

基础用法：在UIAbility对象中调用，示例中的context的获取方式请参见[获取UIAbility的上下文信息](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/application-models/uiability-usage.md#获取uiability的上下文信息)。

```ts
import common from '@ohos.app.ability.common';
import Want from '@ohos.app.ability.Want';

let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
let want: Want = {
    bundleName: 'com.example.myapplication',
    abilityName: 'FuncAbility',
    moduleName: 'entry'
};

context.startAbility(want, (err) => {
    // 显式拉起Ability，通过bundleName、abilityName和moduleName可以唯一确定一个Ability
    console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
});
```

目前parameters支持的数据类型有：字符串、数字、布尔等。

字符串（String）

```ts
import common from '@ohos.app.ability.common';
import Want from '@ohos.app.ability.Want';

let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
let want: Want = {
    bundleName: 'com.example.myapplication',
    abilityName: 'FuncAbility',
    moduleName: 'entry',
    parameters: {
        keyForString: 'str',
    },
};

context.startAbility(want, (err) => {
    console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
});
```

数字（Number）

```ts
import common from '@ohos.app.ability.common';
import Want from '@ohos.app.ability.Want';

let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
let want: Want = {
	bundleName: 'com.example.myapplication',
	abilityName: 'FuncAbility',
  moduleName: 'entry',
	parameters: {
		keyForInt: 100,
		keyForDouble: 99.99,
	},
};
```

布尔（Boolean）

```ts
import Want from '@ohos.app.ability.Want';

let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
let want: Want = {
	bundleName: 'com.example.myapplication',
	abilityName: 'FuncAbility',
  moduleName: 'entry',
	parameters: {
		keyForBool: true,
	},
};

context.startAbility(want, (err) => {
	console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
});
```

拉起文件选择器，设置文件类型，设置单选多选：

```ts
import common from '@ohos.app.ability.common';
import Want from '@ohos.app.ability.Want';

let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
let want: Want = {
    bundleName: 'com.ohos.filepicker',
    abilityName: 'MainAbility',
    moduleName: 'entry',
    type: 'text/xml', // MIME类型为xml
    parameters: {
        uri: 'multipleselect', // 多选
    },
};

context.startAbility(want, (err) => {
    // 显式拉起Ability，通过bundleName、abilityName和moduleName可以唯一确定一个Ability
    console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
});
```

拉起图片选择器，设置文件类型，设置单选多选：

```ts
import common from '@ohos.app.ability.common';
import Want from '@ohos.app.ability.Want';

let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
let want: Want = {
    bundleName: 'com.ohos.photos',
    abilityName: 'com.ohos.photos.MainAbility',
    moduleName: 'entry',
    type: 'image/*', // 只能选择图片
    parameters: {
        uri: 'multipleselect', // 多选
    },
};

context.startAbility(want, (err) => {
    // 显式拉起Ability，通过bundleName、abilityName和moduleName可以唯一确定一个Ability
    console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
  });
  ```
- 目前parameters支持的数据类型有：字符串、数字、布尔、数组、对象。

  * 字符串（String）

    ```ts
    import common from '@ohos.app.ability.common';
    import Want from '@ohos.app.ability.Want';

    let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'FuncAbility',
      moduleName: 'entry',
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
    import Want from '@ohos.app.ability.Want';

    let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'FuncAbility',
      moduleName: 'entry',
      parameters: {
        keyForInt: 100,
        keyForDouble: 99.99,
      },
    };
    ```
  * 布尔（Boolean）

    ```ts
    import Want from '@ohos.app.ability.Want';

    let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'FuncAbility',
      moduleName: 'entry',
      parameters: {
        keyForBool: true,
      },
    };

    context.startAbility(want, (err) => {
      console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
    });
    ```
  * 对象（Object）

    ```ts
    import { common, Want } from '@kit.AbilityKit';
    import { BusinessError } from '@kit.BasicServicesKit';

    let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'FuncAbility',
      moduleName: 'entry',
      parameters: {
        keyForObject: {
          keyForObjectString: 'str',
          keyForObjectInt: -200,
          keyForObjectDouble: 35.5,
          keyForObjectBool: false,
        },
      },
    };

    context.startAbility(want, (err: BusinessError) => {
      if (err.code) {
        console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
      }
    });
    ```
  * 数组（Array）

    ```ts
    import { common, Want } from '@kit.AbilityKit';
    import { BusinessError } from '@kit.BasicServicesKit';

    let context = getContext(this) as common.UIAbilityContext; // UIAbilityContext
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'FuncAbility',
      moduleName: 'entry',
      parameters: {
        keyForArrayString: ['str1', 'str2', 'str3'],
        keyForArrayInt: [100, 200, 300, 400],
        keyForArrayDouble: [0.1, 0.2],
      },
    };

    context.startAbility(want, (err: BusinessError) => {
      if (err.code) {
        console.error(`Failed to startAbility. Code: ${err.code}, message: ${err.message}`);
      }
    });
    ```
    不支持数组中嵌套数组以及Object对象。
