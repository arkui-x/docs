# @ohos.app.ability.UIAbility (UIAbility)

UIAbility是包含UI界面的应用组件，提供组件创建、销毁、前后台切换等生命周期回调，同时也具备组件协同的能力，组件协同主要提供如下常用功能：

- [Caller](#caller)：由[startAbilityByCall](js-apis-inner-application-uiAbilityContext.md#uiabilitycontextstartabilitybycall)接口返回，CallerAbility(调用者)可使用Caller与CalleeAbility(被调用者)进行通信。
- [Callee](#callee)：UIAbility的内部对象，CalleeAbility(被调用者)可以通过Callee与Caller进行通信。

> **说明：**
> 
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。  
> 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
```

## 属性

| 名称 | 类型 | 可读 | 可写 | 说明 |
| -------- | -------- | -------- | -------- | -------- |
| context | [UIAbilityContext](js-apis-inner-application-uiAbilityContext.md) | 是 | 否 | 上下文。 |

## UIAbility.onCreate

onCreate(want: Want, param: AbilityConstant.LaunchParam): void;

UIAbility创建时回调，执行初始化业务逻辑操作。

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-app-ability-want.md) | 是 | 当前UIAbility的Want类型信息，包括ability名称、bundle名称等。 |
| param | [AbilityConstant.LaunchParam](js-apis-app-ability-abilityConstant.md#abilityconstantlaunchparam) | 是 | 创建&nbsp;ability、上次异常退出的原因信息。 |

**示例：**

  ```ts
  class MyUIAbility extends UIAbility {
      onCreate(want, param) {
          console.log('onCreate, want: ${want.abilityName}');
      }
  }
  ```


## UIAbility.onWindowStageCreate

onWindowStageCreate(windowStage: window.WindowStage): void

当WindowStage创建后调用。

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| windowStage | [window.WindowStage](js-apis-window.md#windowstage9) | 是 | WindowStage相关信息。 |

**示例：**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onWindowStageCreate(windowStage) {
          console.log('onWindowStageCreate');
      }
  }
  ```


## UIAbility.onWindowStageDestroy

onWindowStageDestroy(): void

当WindowStage销毁后调用。

**示例：**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onWindowStageDestroy() {
          console.log('onWindowStageDestroy');
      }
  }
  ```


## UIAbility.onDestroy

onDestroy(): void | Promise&lt;void&gt;;

UIAbility生命周期回调，在销毁时回调，执行资源清理等操作。

**示例：**
    

  ```ts
  class MyUIAbility extends UIAbility {
      onDestroy() {
          console.log('onDestroy');
      }
  }
  ```

在执行完onDestroy生命周期回调后，应用可能会退出，从而可能导致onDestroy中的异步函数未能正确执行，比如异步写入数据库。可以使用异步生命周期，以确保异步onDestroy完成后再继续后续的生命周期。

  ```ts
class MyUIAbility extends UIAbility {
    async onDestroy() {
        console.log('onDestroy');
        // 调用异步函数...
    }
}
  ```

## UIAbility.onForeground

onForeground(): void;

UIAbility生命周期回调，当应用从后台转到前台时触发。

**示例：**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onForeground() {
          console.log('onForeground');
      }
  }
  ```


## UIAbility.onBackground

onBackground(): void;

UIAbility生命周期回调，当应用从前台转到后台时触发。

**示例：**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onBackground() {
          console.log('onBackground');
      }
  }
  ```


## UIAbility.onNewWant

onNewWant(want: Want, launchParams: AbilityConstant.LaunchParam): void;

当传入新的Want，ability再次被拉起时会回调执行该方法。

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-app-ability-want.md) | 是 | Want类型参数，如ability名称，包名等。 |
| launchParams | [AbilityConstant.LaunchParam](js-apis-app-ability-abilityConstant.md#abilityconstantlaunchparam) | 是 | UIAbility启动的原因、上次异常退出的原因信息。 |

**示例：**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onNewWant(want, launchParams) {
          console.log('onNewWant, want: ${want.abilityName}');
          console.log('onNewWant, launchParams: ${JSON.stringify(launchParams)}');
      }
  }
  ```
