# ApplicationContext

ApplicationContext模块提供开发者应用级别的的上下文的能力，包括提供注册及取消注册应用内组件生命周期的监听接口。

> **说明：**
>
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
> 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import common from '@ohos.app.ability.common';
```

## 使用说明

在使用ApplicationContext的功能前，需要通过context的实例获取。

```ts
let applicationContext: common.ApplicationContext = this.context.getApplicationContext();
```

## ApplicationContext.on(type: 'abilityLifecycle', callback: AbilityLifecycleCallback)

on(type: 'abilityLifecycle', callback: AbilityLifecycleCallback): **number**;

注册监听应用内生命周期

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名                   | 类型     | 必填 | 说明                           |
| ------------------------ | -------- | ---- | ------------------------------ |
| type | 'abilityLifecycle' | 是   | 监听事件的类型。 |
| callback | [AbilityLifecycleCallback](js-apis-app-ability-abilityLifecycleCallback.md) | 是   | 回调方法，返回注册监听事件的ID。 |

**返回值：**

| 类型   | 说明                           |
| ------ | ------------------------------ |
| number | 返回的此次注册监听生命周期的ID（每次注册该ID会自增+1，当超过监听上限数量2^63-1时，返回-1）。|

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import AbilityLifecycleCallback from '@ohos.app.ability.AbilityLifecycleCallback';

let lifecycleId: number;

export default class EntryAbility extends UIAbility {
    onCreate() {
        console.log('MyAbility onCreate');
        let AbilityLifecycleCallback: AbilityLifecycleCallback = {
            onAbilityCreate(ability) {
                console.log(`AbilityLifecycleCallback onAbilityCreate ability: ${ability}`);
            },
            onWindowStageCreate(ability, windowStage) {
                console.log(`AbilityLifecycleCallback onWindowStageCreate ability: ${ability}`);
                console.log(`AbilityLifecycleCallback onWindowStageCreate windowStage: ${windowStage}`);
            },
            onWindowStageDestroy(ability, windowStage) {
                console.log(`AbilityLifecycleCallback onWindowStageDestroy ability: ${ability}`);
                console.log(`AbilityLifecycleCallback onWindowStageDestroy windowStage: ${windowStage}`);
            },
            onAbilityDestroy(ability) {
                console.log(`AbilityLifecycleCallback onAbilityDestroy ability: ${ability}`);
            },
            onAbilityForeground(ability) {
                console.log(`AbilityLifecycleCallback onAbilityForeground ability: ${ability}`);
            },
            onAbilityBackground(ability) {
                console.log(`AbilityLifecycleCallback onAbilityBackground ability: ${ability}`);
            },
            onAbilityContinue(ability) {
                console.log(`AbilityLifecycleCallback onAbilityContinue ability: ${ability}`);
            }
        }
        // 1.通过context属性获取applicationContext
        let applicationContext = this.context.getApplicationContext();
        // 2.通过applicationContext注册监听应用内生命周期
        lifecycleId = applicationContext.on('abilityLifecycle', AbilityLifecycleCallback);
        console.log(`registerAbilityLifecycleCallback lifecycleId: ${lifecycleId}`);
    }
}
```

## ApplicationContext.off(type: 'abilityLifecycle', callbackId: number, callback: AsyncCallback\<void>)

off(type: 'abilityLifecycle', callbackId: **number**,  callback: AsyncCallback<**void**>): **void**;

取消监听应用内生命周期

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 参数名        | 类型     | 必填 | 说明                       |
| ------------- | -------- | ---- | -------------------------- |
| type | 'abilityLifecycle' | 是   | 取消监听事件的类型。 |
| callbackId    | number   | 是   | 注册监听应用内生命周期的ID。 |
| callback | AsyncCallback\<void> | 是   | 回调方法。                   |

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

let lifecycleId: number;

export default class EntryAbility extends UIAbility {
    onDestroy() {
        let applicationContext = this.context.getApplicationContext();
        console.log(`stage applicationContext: ${applicationContext}`);
        applicationContext.off('abilityLifecycle', lifecycleId, (error, data) => {
            if (error) {
                console.error(`unregisterAbilityLifecycleCallback fail, err: ${JSON.stringify(error)}`);
            } else {
                console.log(`unregisterAbilityLifecycleCallback success, data: ${JSON.stringify(data)}`);
            }
        });
    }
}
```

## ApplicationContext.on('applicationStateChange')<sup>16+</sup>

on(type: 'applicationStateChange', callback: ApplicationStateChangeCallback): void

注册对当前应用前后台变化的监听。使用callback异步回调。仅支持主线程调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台**：Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         | Android平台 | iOS平台 |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| type     | 'applicationStateChange'                                     | 是   | 监听事件类型。                                               | 支持        | 支持    |
| callback | [ApplicationStateChangeCallback](js-apis-app-ability-applicationStateChangeCallback.md) | 是   | 回调函数。可以对应用从后台切换到前台，以及前台切换到后台分别定义回调。 | 支持        | 支持    |

**错误码**：

以下错误码详细介绍请参考[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types. |

**示例：**

```
import { UIAbility, ApplicationStateChangeCallback } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

export default class MyAbility extends UIAbility {
  onCreate() {
    console.log('MyAbility onCreate');
    let applicationStateChangeCallback: ApplicationStateChangeCallback = {
      onApplicationForeground() {
        console.info('applicationStateChangeCallback onApplicationForeground');
      },
      onApplicationBackground() {
        console.info('applicationStateChangeCallback onApplicationBackground');
      }
    }

    // 1.获取applicationContext
    let applicationContext = this.context.getApplicationContext();
    try {
      // 2.通过applicationContext注册应用前后台状态监听
      applicationContext.on('applicationStateChange', applicationStateChangeCallback);
    } catch (paramError) {
      console.error(`error: ${(paramError as BusinessError).code}, ${(paramError as BusinessError).message}`);
    }
    console.log('Resgiter applicationStateChangeCallback');
  }
}
```

## ApplicationContext.off('applicationStateChange')<sup>16+</sup>

off(type: 'applicationStateChange', callback?: ApplicationStateChangeCallback): void

取消当前应用注册的前后台变化的全部监听。使用callback异步回调。仅支持主线程调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台**：Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         | Android平台 | iOS平台 |
| -------- | :----------------------------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| type     | 'applicationStateChange'                                     | 是   | 取消监听事件的类型。                                         | 支持        | 支持    |
| callback | [ApplicationStateChangeCallback](js-apis-app-ability-applicationStateChangeCallback.md) | 否   | 回调函数。可以对应用从后台切换到前台，以及前台切换到后台分别定义回调。 | 支持        | 支持    |

**错误码**：

以下错误码详细介绍请参考[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types. |

**示例：**

```
import { UIAbility } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

export default class MyAbility extends UIAbility {
  onDestroy() {
    let applicationContext = this.context.getApplicationContext();
    try {
      applicationContext.off('applicationStateChange');
    } catch (paramError) {
      console.error(`error: ${(paramError as BusinessError).code}, ${(paramError as BusinessError).message}`);
    }
  }
}
```

## ApplicationContext.getRunningProcessInformation<sup>9+</sup>

getRunningProcessInformation(): Promise\<Array\<ProcessInformation>>;

获取有关运行进程的信息。

**需要权限**：ohos.permission.GET_RUNNING_INFO

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**系统API**: 此接口为系统接口，三方应用不支持调用。

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise\<Array\<[ProcessInformation](js-apis-inner-application-processInformation.md)>> | 以Promise方式返回接口运行结果及有关运行进程的信息，可进行错误处理或其他自定义处理。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
import { BusinessError } from '@ohos.base';

export default class MyAbility extends UIAbility {
    onForeground() {
        let applicationContext = this.context.getApplicationContext();
        applicationContext.getRunningProcessInformation().then((data) => {
            console.log(`The process running information is: ${JSON.stringify(data)}`);
        }).catch((error: BusinessError) => {
            console.error(`error: ${JSON.stringify(error)}`);
        });
    }
}
```

## ApplicationContext.getRunningProcessInformation<sup>9+</sup>

getRunningProcessInformation(callback: AsyncCallback\<Array\<ProcessInformation>>): void;

获取有关运行进程的信息。

**需要权限**：ohos.permission.GET_RUNNING_INFO

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**系统API**: 此接口为系统接口，三方应用不支持调用。

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
|AsyncCallback\<Array\<[ProcessInformation](js-apis-inner-application-processInformation.md)>> | 以回调方式返回接口运行结果及有关运行进程的信息，可进行错误处理或其他自定义处理。 |

**错误码**：

| 错误码ID | 错误信息 |
| ------- | -------- |
| 16000011 | The context does not exist. |
| 16000050 | Internal error. |

以上错误码详细介绍请参考[errcode-ability](../errorcodes/errorcode-ability.md)。

**示例：**

```ts
import UIAbility from '@ohos.app.ability.UIAbility';

export default class MyAbility extends UIAbility {
    onForeground() {
        let applicationContext = this.context.getApplicationContext();
        applicationContext.getRunningProcessInformation((err, data) => {
            if (err) {
                console.error(`getRunningProcessInformation faile, err: ${JSON.stringify(err)}`);
            } else {
                console.log(`The process running information is: ${JSON.stringify(data)}`);
            }
        })
    }
}
```

## ApplicationContext.setColorMode<sup>16+</sup>

setColorMode(colorMode: ConfigurationConstant.ColorMode): void

设置应用的颜色模式。仅支持主线程调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台**：Android、iOS

**参数：**

| 参数名    | 类型                                                         | 必填 | 说明                                                         | Android平台 | iOS平台 |
| --------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| colorMode | [ColorMode](js-apis-app-ability-configurationConstant.md#colormode) | 是   | 设置颜色模式，包括：深色模式、浅色模式、不设置（跟随系统）。 | 支持        | 支持    |

**错误码**：

以下错误码详细介绍请参考[通用错误码](../errorcodes/errorcode-universal.md)和[元能力子系统错误码](../errorcodes/errorcode-ability.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified. 2.Incorrect parameter types. |
| 16000011 | The context does not exist.                                  |

**示例：**

```
import { UIAbility, ConfigurationConstant } from '@kit.AbilityKit';

export default class MyAbility extends UIAbility {
  onForeground() {
    let applicationContext = this.context.getApplicationContext();
    applicationContext.setColorMode(ConfigurationConstant.ColorMode.COLOR_MODE_DARK);
  }
}
```

## ApplicationContext.setFont<sup>21+</sup>

setFont(font: string): void

设置应用的字体类型。仅支持主线程调用。

> **说明：**
>
> 调用该接口前，需要确保窗口已完成创建、且UIAbility对应的页面已完成加载，即在[onWindowStageCreate()](js-apis-app-ability-uiAbility.md#onwindowstagecreate)生命周期中通过[loadContent](js-apis-window.md#loadcontent9)方法加载页面之后调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台**：Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| font   | string | 是   | 设置字体类型，字体可以通过[UIContext.registerFont](js-apis-font.md#registerfont)方法进行注册使用。 |

**错误码**：

以下错误码详细介绍请参考[通用错误码](../errorcodes/errorcode-universal.md)和[元能力子系统错误码](../errorcodes/errorcode-ability.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 16000011 | The context does not exist.                                  |
| 16000050 | Internal error.                                              |

**示例：**

```ts
import { font } from '@kit.ArkUI';
import { common } from '@kit.AbilityKit';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';
  context = this.getUIContext().getHostContext() as common.UIAbilityContext;

  aboutToAppear() {
    this.getUIContext().getFont().registerFont({
      familyName: 'fontName',
      familySrc: $rawfile('font/medium.ttf')
    })

    this.context.getApplicationContext().setFont("fontName");
  }

  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize(50)
          .fontWeight(50)
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

## ApplicationContext.setFontSizeScale<sup>21+</sup>

setFontSizeScale(fontSizeScale: number): void

设置应用字体大小缩放比例。仅支持主线程调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台**：Android、iOS

**参数：**

| 参数名        | 类型   | 必填 | 说明                                                         |
| ------------- | ------ | ---- | ------------------------------------------------------------ |
| fontSizeScale | number | 是   | 表示字体缩放比例，取值为非负数。当应用字体[跟随系统](https://docs.openharmony.cn/pages/v5.0/zh-cn/application-dev/quick-start/app-configuration-file.md#configuration标签)且该字段取值超过[fontSizeMaxScale](https://docs.openharmony.cn/pages/v5.0/zh-cn/application-dev/quick-start/app-configuration-file.md#configuration标签)取值时，实际生效值为[fontSizeMaxScale](https://docs.openharmony.cn/pages/v5.0/zh-cn/application-dev/quick-start/app-configuration-file.md#configuration标签)取值。 |

**示例：**

```ts
import { UIAbility } from '@kit.AbilityKit';
import { window } from '@kit.ArkUI';

export default class MyAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage) {
    windowStage.loadContent('pages/Index', (err, data) => {
      if (err.code) {
        return;
      }
      let applicationContext = this.context.getApplicationContext();
      applicationContext.setFontSizeScale(2);
    });
  }
}
```

## ApplicationContext.setLanguage<sup>21+</sup>

setLanguage(language: string): void

设置应用的语言。仅支持主线程调用。

> **说明：**
>
> 调用该接口前，需要确保窗口已完成创建、且UIAbility对应的页面已完成加载，即在[onWindowStageCreate()](js-apis-app-ability-uiAbility.md#onwindowstagecreate)生命周期中通过[loadContent](js-apis-window.md#loadcontent9)方法加载页面之后调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台**：Android、iOS

**参数：**

| 参数名   | 类型   | 必填 | 说明                                                         |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| language | string | 是   | 设置语言，当前支持的语言列表可以通过[getSystemLanguages()](js-apis-i18n.md#getsystemlanguages9)获取。 |

**错误码**：

以下错误码详细介绍请参考[通用错误码](../errorcodes/errorcode-universal.md)和[元能力子系统错误码](../errorcodes/errorcode-ability.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 16000011 | The context does not exist.                                  |

**示例：**

```ts
import { UIAbility } from '@kit.AbilityKit';
import { window } from '@kit.ArkUI';

export default class MyAbility extends UIAbility {
  onWindowStageCreate(windowStage: window.WindowStage) {
    console.info("Ability onWindowStageCreate");
    windowStage.loadContent('pages/Index', (err, data) => {
      if (err.code) {
        console.error(`Failed to load the content. Code: ${err.code}, message: ${err.message}`);
        return;
      }
      console.info(`Succeeded in loading the content. Data: ${JSON.stringify(data)}`);
    });
    let applicationContext = this.context.getApplicationContext();
    applicationContext.setLanguage('zh-cn');
  }
}
```