# Stage模型Android端开发文档

## 简介

本文介绍将OpenHarmony开发框架扩展到Android平台所需要的必要的类及其使用说明，开发者基于OpenHarmony，可复用大部分的应用代码（生命周期等）并可以部署到Android平台，降低跨平台应用开发成本。

## 应用工程OpenHarmony逻辑部分的StageApplication

应用需要继承继承存于ArkUI-X跨平台SDK中的arkui_android_adapter.jar包所提供的StageApplication，例如：

```
package com.example.myapplication;
import ohos.stage.ability.adapter.StageApplication;

public class HiStageApplication extends StageApplication {

}
```

## 应用工程Android逻辑部分的StageActivity

Activity需要继承存于ArkUI-X跨平台SDK中的arkui_android_adapter.jar包所提供的StageActivity，例如：

```
package com.example.myapplication;
import ohos.stage.ability.adapter.StageActivity;

public class EntryMainAbilityActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.setInstanceName("com.example.myapplication:entry:MainAbility:");
        super.onCreate(savedInstanceState);
    }
}
```

需要重写StageActivity中的onCreate事件，在super.onCreate(savedInstanceState)之前设置instanceName，规则如下

“bundleName:moduleName:abilityName:”，其中bundleName、moduleName、abilityName来自OpenHarmony应用的module.json5配置文件中对应字段的值

## Ability与Activity对应规则
  ![stage_android](figures/stage_android.png)




