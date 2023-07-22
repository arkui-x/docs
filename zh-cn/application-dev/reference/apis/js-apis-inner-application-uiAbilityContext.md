# UIAbilityContext

UIAbilityContext是[UIAbility](js-apis-app-ability-uiAbility.md)的上下文环境，继承自[Context](js-apis-inner-application-context.md)，提供UIAbility的相关配置信息以及操作UIAbility的方法，如启动UIAbility，停止当前UIAbilityContext所属的UIAbility，启动、停止、连接等。

> **说明：**
>
>  - 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>  - 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import common from '@ohos.app.ability.common';
```

## 属性

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityRuntime.Core

| 名称 | 类型 | 可读 | 可写 | 说明 |
| -------- | -------- | -------- | -------- | -------- |
| abilityInfo | [AbilityInfo](js-apis-bundleManager-abilityInfo.md) | 是 | 否 | UIAbility的相关信息。 |
| currentHapModuleInfo | [HapModuleInfo](js-apis-bundleManager-hapModuleInfo.md) | 是 | 否 | 当前HAP的信息。 |
| config | [Configuration](js-apis-app-ability-configuration.md) | 是 | 否 | 与UIAbility相关的配置信息，如语言、颜色模式等。 |

> **关于示例代码的说明：**
>
> 在本文档的示例中，通过`this.context`来获取`UIAbilityContext`，其中`this`代表继承自`UIAbility`的`UIAbility`实例。如需要在页面中使用`UIAbilityContext`提供的能力，请参见[获取UIAbility的上下文信息](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/application-models/uiability-usage.md#%E8%8E%B7%E5%8F%96uiability%E7%9A%84%E4%B8%8A%E4%B8%8B%E6%96%87%E4%BF%A1%E6%81%AF)

## UIAbilityContext.startAbility

startAbility(want: Want, callback: AsyncCallback&lt;void&gt;): void;

启动Ability（callback形式）。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-app-ability-want.md) | 是 | 启动Ability的want信息。 |
| callback | AsyncCallback&lt;void&gt; | 是 | callback形式返回启动结果。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist. |

错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

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

startAbility(want: Want, options?: StartOptions): Promise&lt;void&gt;;

启动Ability（promise形式）。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-app-ability-want.md) | 是 | 启动Ability的want信息。 |
| options | [StartOptions] | 否 | 启动Ability所携带的参数。目前不支持跨平台。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;void&gt; | Promise形式返回启动结果。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist. |

错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

**示例：**

  ```ts
let want = {
  bundleName: 'com.example.myapplication',
  abilityName: 'EntryAbility'
};
let options = {
  windowMode: 0,
};

try {
  this.context.startAbility(want, options)
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

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

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

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

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
  } catch (err) {
    // 捕获同步的参数错误
    console.error(`terminateSelf failed, code is ${err.code}, message is ${err.message}`);
  }
  ```