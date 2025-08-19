# @ohos.app.ability.common

本模块提供了UIAbility的相关配置信息以及操作UIAbility的方法，如启动UIAbility，停止当前UIAbilityContext所属的UIAbility，启动、停止、连接等。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 本模块接口仅可在Stage模型下使用。

依赖权限

iOS平台

**说明**: Xcode项目里拉起图片选择器，需要配置权限，没有权限将导致运行异常。

**配置方法**: 在Xcode中右击项目中的info.plist，选择Open As -> Source Code，在plist标签中加入NSPhotoLibraryUsageDescription。

示例如下:

```js
<plist version="1.0">
<dict>
    <key>NSPhotoLibraryUsageDescription</key>
    <string>获取相册权限描述文案</string>
</dict>
</plist>
```

## 导入模块

```ts
import common from '@ohos.app.ability.common';
```

## **属性**

**系统能力：** SystemCapability.Ability.AbilityRuntime.Core

| 名称      | 类型                                                                                                                                                                                                | 可读 | 可写 | 说明                                         | Android平台 | IOS平台 |
| --------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ---- | -------------------------------------------- | ---- | ---- |
| windowStage<sup>21+</sup>     | window.WindowStage         | 是   | 否   | 当前WindowStage对象。仅支持在主线程调用。| 支持   | 支持   |

## **UIAbilityContext.startAbility**

startAbility(want: Want, callback: AsyncCallback&lt;void&gt;): void;

启动Ability（callback形式）。

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                | 必填 | 说明                     | Android平台 | iOS平台 |
| -------- | ----------------------------------- | ---- | ------------------------ | ----------- | ------- |
| want     | [Want](js-apis-app-ability-want.md) | 是   | 启动Ability的want信息    | 支持        | 支持    |
| callback | AsyncCallback&lt;void&gt;           | 是   | callback形式返回启动结果 | 支持        | 支持    |

**错误码**：

以下错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist.           |
| 16000050 | Internal error.                       |

> **说明：**
>
> 当在iOS平台上使用此接口时，无法从返回结果中判断参数[Want](js-apis-app-ability-want.md)中moduleName或者abilityName是否错误，需要参考[ArkUI应用实现页面跳转](../../quick-start/start-with-ability-on-ios.md#arkui应用实现页面跳转)识别错误。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import Want from '@ohos.app.ability.Want';
import { BusinessError } from '@ohos.base';

export default class EntryAbility extends UIAbility {

  onForeground() {
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'EntryAbility'
    };

    try {
      this.context.startAbility(want, (err: BusinessError) => {
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
      let code = (err as BusinessError).code;
      let message = (err as BusinessError).message;
      console.error(`startAbility failed, code is ${code}, message is ${message}`);
    }
  }
}
```

## **UIAbilityContext.startAbility<sup>18+</sup>**

startAbility(want: Want, options?: StartOptions): Promise&lt;void&gt;;

启动Ability（promise形式）。

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                | 必填 | 说明                                        | Android平台 | iOS平台 |
| ------- | ----------------------------------- | ---- | ------------------------------------------- | ----------- | ------- |
| want    | [Want](js-apis-app-ability-want.md) | 是   | 启动Ability的want信息                       | 支持        | 支持    |
| options | StartOptions                        | 否   | 启动Ability所携带的参数。目前不支持跨平台。 | 不支持      | 不支持  |

**错误码**：

以下错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist.           |
| 16000050 | Internal error.                       |

> **说明：**
>
> 当在iOS平台上使用此接口时，无法从返回结果中判断参数[Want](js-apis-app-ability-want.md)中moduleName或者abilityName是否错误，需要参考[ArkUI应用实现页面跳转](../../quick-start/start-with-ability-on-ios.md#arkui应用实现页面跳转)识别错误。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import Want from '@ohos.app.ability.Want';
import StartOptions from '@ohos.app.ability.StartOptions';
import { BusinessError } from '@ohos.base';

export default class EntryAbility extends UIAbility {

  onForeground() {
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'EntryAbility'
    };
    let options: StartOptions = {
      windowMode: 0,
    };

    try {
      this.context.startAbility(want, options)
        .then(() => {
          // 执行正常业务
          console.info('startAbility succeed');
        })
        .catch((err: BusinessError) => {
          // 处理业务逻辑错误
          console.error(`startAbility failed, code is ${err.code}, message is ${err.message}`);
        });
    } catch (err) {
      // 处理入参错误异常
      let code = (err as BusinessError).code;
      let message = (err as BusinessError).message;
      console.error(`startAbility failed, code is ${code}, message is ${message}`);
    }
  }
}
```

## **UIAbilityContext.startAbility<sup>18+</sup>**

startAbility(want: Want, options: StartOptions, callback: AsyncCallback `<void>`): void

启动Ability（callback形式）。

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                | 必填 | 说明                                        | Android平台 | iOS平台 |
| ------- | ----------------------------------- | ---- | ------------------------------------------- | ----------- | ------- |
| want    | [Want](js-apis-app-ability-want.md) | 是   | 启动Ability的want信息                       | 支持        | 支持    |
| options | StartOptions                        | 否   | 启动Ability所携带的参数。目前不支持跨平台。 | 不支持      | 不支持  |

**错误码**：

以下错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist.           |
| 16000050 | Internal error.                       |

> **说明：**
>
> 当在iOS平台上使用此接口时，无法从返回结果中判断参数[Want](js-apis-app-ability-want.md)中moduleName或者abilityName是否错误，需要参考[ArkUI应用实现页面跳转](../../quick-start/start-with-ability-on-ios.md#arkui应用实现页面跳转)识别错误。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import Want from '@ohos.app.ability.Want';
import StartOptions from '@ohos.app.ability.StartOptions';
import { BusinessError } from '@ohos.base';

export default class EntryAbility extends UIAbility {

  onForeground() {
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'EntryAbility'
    };
    let options: StartOptions = {
      windowMode: 0,
    };

    try {
      this.context.startAbility(want, options, (err: BusinessError) => {
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
      let code = (err as BusinessError).code;
      let message = (err as BusinessError).message;
      console.error(`startAbility failed, code is ${code}, message is ${message}`);
    }
  }
}
```

## **UIAbilityContext.startAbilityForResult<sup>18+</sup>**

startAbilityForResult(want: Want, callback: AsyncCallback `<AbilityResult>`): void

启动Ability（callback形式）。

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                | 必填 | 说明                     | Android平台 | iOS平台 |
| -------- | ----------------------------------- | ---- | ------------------------ | ----------- | ------- |
| want     | [Want](js-apis-app-ability-want.md) | 是   | 启动Ability的want信息    | 支持        | 支持    |
| callback | AsyncCallback&lt;void&gt;           | 是   | callback形式返回启动结果 | 支持        | 支持    |

**错误码**：

以下错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist.           |
| 16000050 | Internal error.                       |

> **说明：**
>
> 当在iOS平台上使用此接口时，无法从返回结果中判断参数[Want](js-apis-app-ability-want.md)中moduleName或者abilityName是否错误，需要参考[ArkUI应用实现页面跳转](../../quick-start/start-with-ability-on-ios.md#arkui应用实现页面跳转)识别错误。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import Want from '@ohos.app.ability.Want';
import { BusinessError } from '@ohos.base';

export default class EntryAbility extends UIAbility {

  onForeground() {
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'EntryAbility'
    };

    try {
      this.context.startAbilityForResult(want, (err: BusinessError, result: common.AbilityResult) => {
        if (err.code) {
          // 处理业务逻辑错误
          console.error(`startAbilityForResult failed, code is ${err.code}, message is ${err.message}`);
          return;
        }
        // 执行正常业务
        console.info('startAbilityForResult succeed');
      });
    } catch (err) {
      // 处理入参错误异常
      let code = (err as BusinessError).code;
      let message = (err as BusinessError).message;
      console.error(`startAbilityForResult failed, code is ${code}, message is ${message}`);
    }
  }
}
```

## **UIAbilityContext.startAbilityForResult<sup>18+</sup>**

startAbilityForResult(want: Want, options?: StartOptions): Promise `<AbilityResult>`

启动Ability（promise形式）。

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                | 必填 | 说明                                        | Android平台 | iOS平台 |
| ------- | ----------------------------------- | ---- | ------------------------------------------- | ----------- | ------- |
| want    | [Want](js-apis-app-ability-want.md) | 是   | 启动Ability的want信息                       | 支持        | 支持    |
| options | StartOptions                        | 否   | 启动Ability所携带的参数。目前不支持跨平台。 | 不支持      | 不支持  |

**错误码**：

以下错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist.           |
| 16000050 | Internal error.                       |

> **说明：**
>
> 当在iOS平台上使用此接口时，无法从返回结果中判断参数[Want](js-apis-app-ability-want.md)中moduleName或者abilityName是否错误，需要参考[ArkUI应用实现页面跳转](../../quick-start/start-with-ability-on-ios.md#arkui应用实现页面跳转)识别错误。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import Want from '@ohos.app.ability.Want';
import StartOptions from '@ohos.app.ability.StartOptions';
import { BusinessError } from '@ohos.base';

export default class EntryAbility extends UIAbility {

  onForeground() {
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'EntryAbility'
    };
    let options: StartOptions = {
      windowMode: 0,
    };

    try {
      this.context.startAbilityForResult(want, options)
        .then((result: common.AbilityResult) => {
          // 执行正常业务
          console.info('startAbilityForResult succeed');
        })
        .catch((err: BusinessError) => {
          // 处理业务逻辑错误
          console.error(`startAbilityForResult failed, code is ${err.code}, message is ${err.message}`);
        });
    } catch (err) {
      // 处理入参错误异常
      let code = (err as BusinessError).code;
      let message = (err as BusinessError).message;
      console.error(`startAbilityForResult failed, code is ${code}, message is ${message}`);
    }
  }
}
```

## **UIAbilityContext.startAbilityForResult<sup>18+</sup>**

startAbilityForResult(want: Want, options: StartOptions, callback: AsyncCallback `<AbilityResult>`): void

启动Ability（callback形式）。

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                                                                                                            | 必填 | 说明                                        | Android平台 | iOS平台 |
| -------- | ----------------------------------------------------------------------------------------------------------------------------------------------- | ---- | ------------------------------------------- | ----------- | ------- |
| want     | [Want](js-apis-app-ability-want.md)                                                                                                             | 是   | 启动Ability的want信息                       | 支持        | 支持    |
| options  | StartOptions                                                                                                                                    | 否   | 启动Ability所携带的参数。目前不支持跨平台。 | 不支持      | 不支持  |
| callback | [AsyncCallback](https://docs.openharmony.cn/pages/v5.0/zh-cn/application-dev/reference/apis-ability-kit/js-apis-inner-ability-abilityResult.md) | 是   | callback形式返回启动结果                    | 支持        | 支持    |

**错误码**：

以下错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)

| 错误码ID | 错误信息                              |
| -------- | ------------------------------------- |
| 16000001 | The specified ability does not exist. |
| 16000011 | The context does not exist.           |
| 16000050 | Internal error.                       |

> **说明：**
>
> 当在iOS平台上使用此接口时，无法从返回结果中判断参数[Want](js-apis-app-ability-want.md)中moduleName或者abilityName是否错误，需要参考[ArkUI应用实现页面跳转](../../quick-start/start-with-ability-on-ios.md#arkui应用实现页面跳转)识别错误。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import Want from '@ohos.app.ability.Want';
import StartOptions from '@ohos.app.ability.StartOptions';
import { BusinessError } from '@ohos.base';

export default class EntryAbility extends UIAbility {

  onForeground() {
    let want: Want = {
      bundleName: 'com.example.myapplication',
      abilityName: 'EntryAbility'
    };
    let options: StartOptions = {
      windowMode: 0,
    };

    try {
      this.context.startAbilityForResult(want, options, (err: BusinessError, result: common.AbilityResult) => {
        if (err.code) {
          // 处理业务逻辑错误
          console.error(`startAbilityForResult failed, code is ${err.code}, message is ${err.message}`);
          return;
        }
        // 执行正常业务
        console.info('startAbilityForResult succeed');
      });
    } catch (err) {
      // 处理入参错误异常
      let code = (err as BusinessError).code;
      let message = (err as BusinessError).message;
      console.error(`startAbilityForResult failed, code is ${code}, message is ${message}`);
    }
  }
}
```