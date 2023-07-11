# 通过Stage模型开发Android端应用指南

## 简介

本文介绍将OpenHarmony开发框架扩展到Android平台所需要的必要的类及其使用说明，开发者基于OpenHarmony，可复用大部分的应用代码（生命周期等）并可以部署到Android平台，降低跨平台应用开发成本。

## 关键依赖集成

* ArkUI源码和OpenHarmony资源编译后的JSBundle和资源文件，作为平台资源存放在assets中。
* ArkUI-X编译SDK时生成的arkui_android_adapter.jar包。
**注:** AndroidStudio:Android应用的开发工具。

## AndroidStudio配置

* 使用AndroidStudio所创建Android工程的PackageName需要与OpenHarmony工程的BundleName一致。

## ArkUI-X和Android平台集成所用关键类

### 应用工程Android逻辑部分的StageApplication

应用需要继承arkui_android_adapter.jar包所提供的StageApplication，例如：

```
package com.example.myapplication;
import ohos.stage.ability.adapter.StageApplication;

public class HiStageApplication extends StageApplication {

}
```

### 应用工程Android逻辑部分的StageActivity

Activity需要继承arkui_android_adapter.jar包所提供的StageActivity，例如：

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

需要重写StageActivity中的onCreate事件，在super.onCreate(savedInstanceState)之前设置instanceName，规则如下：

```
bundleName:moduleName:abilityName:
```

其中bundleName、moduleName、abilityName的值来自于OpenHarmony应用中的module.json5配置文件。

## Ability与Activity对应规则
  ![stage_android](figures/stage_android.png)
