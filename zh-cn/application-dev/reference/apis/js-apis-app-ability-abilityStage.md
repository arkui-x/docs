# @ohos.app.ability.AbilityStage (AbilityStage)

AbilityStage是HAP的运行时类。

AbilityStage类提供在HAP加载的时候，通知开发者，可以在此进行该HAP的初始化（如资源预加载，线程创建等）能力。

> **说明：**
> 
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。  
> 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import AbilityStage from '@ohos.app.ability.AbilityStage';
```

## AbilityStage.onCreate

onCreate(): void

当应用创建时调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**示例：**
    
```ts
import AbilityStage from '@ohos.app.ability.AbilityStage';

class MyAbilityStage extends AbilityStage {
    onCreate() {
        console.log('MyAbilityStage.onCreate is called');
    }
}
```


## AbilityStage.onConfigurationUpdate

onConfigurationUpdate(newConfig: Configuration): void;

环境变化通知接口，发生全局配置变更时回调。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**参数：**

  | 参数名 | 类型 | 必填 | 说明 | 
  | -------- | -------- | -------- | -------- |
  | newConfig | [Configuration](js-apis-app-ability-configuration.md) | 是 | 发生全局配置变更时触发回调，当前全局配置包括系统语言、深浅色模式。 | 

**示例：**
    
```ts
import AbilityStage from '@ohos.app.ability.AbilityStage';

class MyAbilityStage extends AbilityStage {
    onConfigurationUpdate(config) {
        console.log('onConfigurationUpdate, language: ${config.language}');
    }
}
```

## AbilityStage.context

context: AbilityStageContext;

指示AbilityStage的上下文。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

| 属性名      | 类型                        | 说明                                                         |
| ----------- | --------------------------- | ------------------------------------------------------------ |
| context  | [AbilityStageContext](js-apis-inner-application-abilityStageContext.md) | 在Ability启动阶段进行初始化时回调，获取到该Ability的context值。 |
