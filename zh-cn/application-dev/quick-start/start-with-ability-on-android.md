# 通过Stage模型开发Android端应用指南

## 简介

本文介绍将ArkUI框架扩展到Android平台所需要的必要的类及其使用说明，开发者基于OpenHarmony，可复用大部分的应用代码（生命周期等）并可以部署到Android平台，降低跨平台应用开发成本。

## AndroidStudio配置

使用AndroidStudio所创建Android工程的PackageName需要与OpenHarmony工程的BundleName一致。

**注:** AndroidStudio:Android应用的开发工具。

## ArkUI-X和Android平台集成所用关键类

### 应用工程Android逻辑部分的StageApplication

应用需要继承arkui_android_adapter.jar包所提供的StageApplication。StageApplication用于初始化资源路径以及加载配置信息，例如：

```
package com.example.myapplication;
import ohos.stage.ability.adapter.StageApplication;

public class HiStageApplication extends StageApplication {

}
```

### 应用工程Android逻辑部分的StageActivity

Activity需要继承arkui_android_adapter.jar包所提供的StageActivity，StageActivity主要功能是将Android中Activity的生命周期与OpenHarmony中Ability的生命周期进行映射，例如：

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

为了将Activity和ability进行关联，需要重写StageActivity中的onCreate事件，在super.onCreate(savedInstanceState)之前设置instanceName，规则如下：

```
bundleName:moduleName:abilityName:
```

其中bundleName的值来自于OpenHarmony应用中app.json5配置文件，moduleName、abilityName的值来自于OpenHarmony应用中的module.json5配置文件。

## Ability与Activity对应规则

Android端应用内的Activity的packageName需要与Ability的bundleName一致。

Android端应用内的Activity的activityName组成规则：Ability的moduleName + Ability的abilityName + “Activity”。

示例如图：
  ![stage_android](figures/stage_android.png)

## StageApplication初始化支持以下三种方式

### 通过继承StageApplication的方式进行初始化

```
import ohos.stage.ability.adapter.StageApplication;

public class HiStageApplication extends StageApplication {
    @Override
    public void onCreate() {
        super.onCreate();
    }
}
```

### 继承Android原生Application方式，在onCreate方法中创建StageApplicationDelegate实例进行初始化

```
import android.app.Application;
import ohos.stage.ability.adapter.StageApplicationDelegate;

public class HiStageApplication extends Application {
    private StageApplicationDelegate appDelegate_ = null;

    @Override
    public void onCreate() {
        super.onCreate();
        appDelegate_ = new StageApplicationDelegate();
        appDelegate_.initApplication(this);
    }
}
```

### 在Activity中创建StageApplicationDelegate实例进行初始化

```
import android.app.Activity;
import ohos.stage.ability.adapter.StageApplicationDelegate;

public class EntryEntryAbilityActivity extends Activity {

    private StageApplicationDelegate appDelegate_ = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        appDelegate_ = new StageApplicationDelegate();
        appDelegate_.initApplication(this.getApplication());
        super.onCreate(savedInstanceState);
    }
}
```

## 通过原生Activity拉起Ability并传递参数

使用原生Activity拉起Ability时，需使用原生应用的startActivity方法，参数的传递需要通过Intent中的putExtra()进行设置，规则如下：

key值为params  
value为json格式

```
{
    "params":[
        {
            "key":键,
            "type":参数类型值,
            "value":值
        },
        {
            ...
        }
    ]
}
```

### 示例代码

* Java

```
public class EntryEntryAbilityActivity extends AppCompatActivity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Intent intent = new Intent();
        intent.setClass(this, EntryEntryAbilityTwoActivity.class);
        intent.putExtra("params",
                "{\"params\":[{\"key\":\"keyfirst\",\"type\":1,\"value\":\"keyvalue\"}," +
                "{\"key\":\"keysecond\",\"type\":9,\"value\":\"2.3\"}," +
                "{\"key\":\"keythird\",\"type\":5,\"value\":\"2\"}," +
                "{\"key\":\"keyfourth\",\"type\":10,\"value\":\"test\"}]}");
        startActivity(intent);
    }
}
```

* ArkTS

```
# xxx.ets
export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    console.log("value = " + want.parameters?.keyfirst)
    console.log("value = " + want.parameters?.keysecond)
    console.log("value = " + want.parameters?.keythird)
    console.log("value = " + want.parameters?.keyfourth)
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    ...
  }
...
}
```
### 支持的参数类型列表

| 参数类型 | 参数类型值 |
| ------- | --------- |
| boolean |     1     |
| int     |     5     |
| double  |     9     |
| string  |    10     |

### 示例代码

```
public class EntryEntryAbilityActivity extends AppCompatActivity {

    @Override
    public void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Intent intent = new Intent();
        intent.setClass(this, EntryEntryAbilityTwoActivity.class);
        intent.putExtra("params",
                "{\"params\":[{\"key\":\"keyfirst",\"type\":1,\"value\":\"valuefirst"}," +
                "{\"key\":\"keysecond\",\"type\":9,\"value\":\"2.3\"}," +
                "{\"key\":\"keythird",\"type\":5,\"value\":\"2\"}," +
                "{\"key\":\"keyfourth",\"type\":10,\"value\":\"test\"}]}");
        startActivity(intent);
    }
}
```

## 用启动Ability的方式拉起原生Activity

每一个Ability对应一个StageActivity，启动Ability实际是拉起对应的StageActivity。

所以将原生Activity按照上文中Ability对应StageActivity的规则命名，可以用启动Ability的方式拉起原生Activity。

```javascript
// xxx.ets
 let want: Want = {
    bundleName: 'com.example.helloworld',
    moduleName: 'entry', //小写
    abilityName: 'Jump', //首字母大写
    parameters:{id:1,name:'ArkUI-X'} //可选参数
    };
    let context = getContext(this) as common.UIAbilityContext;
    context.startAbility(want, (err, data) => {
    }); 
```

```java
// xxx.java
public class EntryJumpActivity extends AppCompatActivity { //命名：moduleName + abilityName + “Activity”
    private static final String WANT_PARAMS = "params";
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_jump);
        Intent intent = getIntent();
        String params = "";
        if (intent != null) {
            params = intent.getStringExtra(WANT_PARAMS);
        }
    }
}
```