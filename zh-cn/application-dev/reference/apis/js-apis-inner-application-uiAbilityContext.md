# UIAbilityContext

UIAbilityContext是[UIAbility](js-apis-app-ability-uiAbility.md)的上下文环境，继承自[Context](js-apis-inner-application-context.md)，提供UIAbility的相关配置信息以及操作UIAbility的方法，如启动UIAbility，停止当前UIAbilityContext所属的UIAbility。

> **说明：**
>
>  - 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>  - 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import common from '@ohos.app.ability.common';
```

## 属性

| 名称 | 类型 | 可读 | 可写 | 说明 |
| -------- | -------- | -------- | -------- | -------- |
| abilityInfo | [AbilityInfo](js-apis-bundleManager-abilityInfo.md) | 是 | 否 | UIAbility的相关信息。 |
| currentHapModuleInfo | [HapModuleInfo](js-apis-bundleManager-hapModuleInfo.md) | 是 | 否 | 当前HAP的信息。 |
| config | [Configuration](js-apis-app-ability-configuration.md) | 是 | 否 | 与UIAbility相关的配置信息，如语言、颜色模式等。 |

## UIAbilityContext.startAbility

startAbility(want: Want, callback: AsyncCallback&lt;void&gt;): void;

启动Ability（callback形式）。

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-application-want.md) | 是 | 启动Ability的want信息。 |
| callback | AsyncCallback&lt;void&gt; | 是 | callback形式返回启动结果。 |

**示例：**

  ```ts
let want = {
  bundleName: 'com.example.myapplication',
  abilityName: 'EntryAbility'
};

try {
  this.context.startAbility(want, (err) => {
    if (err.code) {
      // 处理业务逻辑错误
      console.error(`startAbility failed, code is ${err.code}, message is ${err.message}`);
      return;
    }
    // 执行正常业务
    console.info('startAbility succeed');
  });
} catch (err) {
  // 处理入参错误异常
  console.error(`startAbility failed failed, code is ${err.code}, message is ${err.message}`);
}
  ```

## UIAbilityContext.startAbility

startAbility(want: Want): Promise&lt;void&gt;;

启动Ability（promise形式）。

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-application-want.md) | 是 | 启动Ability的want信息。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;void&gt; | Promise形式返回启动结果。 |

**示例：**

  ```ts
let want = {
  bundleName: 'com.example.myapplication',
  abilityName: 'EntryAbility'
};

try {
  this.context.startAbility(want)
    .then(() => {
      // 执行正常业务
      console.info('startAbility succeed');
    })
    .catch((err) => {
      // 处理业务逻辑错误
      console.error(`startAbility failed, code is ${err.code}, message is ${err.message}`);
    });
} catch (err) {
  // 处理入参错误异常
  console.error(`startAbility failed, code is ${err.code}, message is ${err.message}`);
}
  ```

## UIAbilityContext.terminateSelf

terminateSelf(callback: AsyncCallback&lt;void&gt;): void;

停止Ability自身（callback形式）。

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| callback | AsyncCallback&lt;void&gt; | 是 | 停止Ability自身的回调函数。 |

**示例：**

  ```ts
  try {
    this.context.terminateSelf((err) => {
      if (err.code) {
        // 处理业务逻辑错误
        console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
        return;
      }
      // 执行正常业务
      console.info('terminateSelf succeed');
    });
  } catch (err) {
    // 捕获同步的参数错误
    console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
  }
  ```


## UIAbilityContext.terminateSelf

terminateSelf(): Promise&lt;void&gt;;

停止Ability自身（promise形式）。

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;void&gt; | 停止Ability自身的回调函数。 |

**示例：**

  ```ts
  try {
    this.context.terminateSelf()
      .then(() => {
        // 执行正常业务
        console.info('terminateSelf succeed');
      })
      .catch((err) => {
        // 处理业务逻辑错误
        console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
      });
  } catch (error) {
    // 捕获同步的参数错误
    console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
  }
  ```
