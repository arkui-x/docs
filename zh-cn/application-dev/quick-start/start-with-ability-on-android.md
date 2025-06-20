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

### 1. 通过继承StageApplication的方式进行初始化

```
import ohos.stage.ability.adapter.StageApplication;

public class HiStageApplication extends StageApplication {
    @Override
    public void onCreate() {
        super.onCreate();
    }
}
```

### 2. 继承Android原生Application方式，在onCreate方法中创建StageApplicationDelegate实例进行初始化

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

### 3. 在Activity中创建StageApplicationDelegate实例进行初始化

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

使用原生Activity拉起Ability时，需使用原生应用的startActivity方法，参数的传递需要通过Intent中的putExtra()进行设置，目前有两种方式进行参数的传递，具体如下：

### 1. 使用手动方式
#### 参数格式
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

#### 示例代码

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
#### 支持的参数类型列表

| 参数类型 | 参数类型值 |
| ------- | --------- |
| boolean |     1     |
| int     |     5     |
| double  |     9     |
| string  |    10     |

### 2. WantParams工具类
推荐使用。
#### 参数格式
putExtra中的key值为"params",对应的value为WantParams字符串。
```
    WantParams wantParams = new WantParams();
    wantParams.addValue("key1", "value")
            .addValue("key2", 123)
            .addValue("key3", false)
            ...
```

#### 示例代码
* Java
此示例展示了在启动一个新的 Ability 时，如何通过 WantParams 实现参数传递。具体涉及使用 WantParams 中的 addValue、getValue 和 toWantParamsString 接口，完成数据的自定义、获取和修改。
```
public class MainAbilityActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        Intent intent = new Intent();
        intent.setClass(this, EntryEntryAbilityActivity.class);
        // 设置自定义数据
        WantParams wantParams = new WantParams();
        wantParams.addValue("stringKey", "normal")
                .addValue("intKey", -2147483648)
                .addValue("doubleKey", -6.9)
                .addValue("boolKey", true)
                .addValue("arrayKey", new boolean[] { false, true })
                .addValue("wantParamsKey",
                        new WantParams()
                                .addValue("stringKey2", "It's me."));

        // 获取指定的键对应的值并修改
        Object obj = wantParams.getValue("stringKey");
        if (obj instanceof String) {
            wantParams.setValue("stringKey", "new string value");
        }
        obj = wantParams.getValue("intKey");
        if (obj instanceof Integer) {
            wantParams.setValue("intKey", 123);
        }
        obj = wantParams.getValue("arrayKey");
        if (obj instanceof boolean[]) {
            wantParams.setValue("arrayKey", new boolean[] { false, false });
        }
        obj = wantParams.getValue("wantParamsKey");
        if (obj instanceof WantParams) {
            wantParams.setValue("wantParamsKey", new WantParams().addValue("intKey", 999));
        }
        intent.putExtra("params", wantParams.toWantParamsString());
        startActivity(intent);
    }
}
```
* ArkTS
```
# xxx.ets
export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    console.log("value = " + want.parameters?.stringKey)
    console.log("value = " + want.parameters?.intKey)
    console.log("value = " + want.parameters?.doubleKey)
    console.log("value = " + want.parameters?.boolKey)
    console.log("value = " + JSON.stringify(want.parameters?.arrayKey))
    console.log("value = " + JSON.stringify(want.parameters?.wantParamsKey))
  }
}
```

#### 支持的参数类型
WantParams支持的类型有：
    boolean、int、float、double、String、WantParams、boolean[]、int[]、float[]、double[]、String[]。
WantParams提供的接口详细如下：
| 接口 | 返回值 | 参数 | 功能 |
| ------- | --------- | --------- | --------- |
| addValue | WantParams | String key, boolean value | 为WantParams添加"String"类型的key，"boolean"类型的值value。 |
| addValue | WantParams | String key, int  value | 为WantParams添加"String"类型的key，"int"类型的值value。 |
| addValue | WantParams | String key, double value | 为WantParams添加"String"类型的key，"double"类型的值value。 |
| addValue | WantParams | String key, String value | 为WantParams添加"String"类型的key，"String"类型的值value。 |
| addValue | WantParams | String key, boolean[] value | 为WantParams添加"String"类型的key，"boolean[]"类型的值value。 |
| addValue | WantParams | String key, int[] value | 为WantParams添加"String"类型的key，"int[]"类型的值value。 |
| addValue | WantParams | String key, double[] value | 为WantParams添加"String"类型的key，"double[]"类型的值value。 |
| addValue | WantParams | String key, String[] value | 为WantParams添加"String"类型的key，"String[]"类型的值value。 |
| addValue | WantParams | String key, WantParams value | 为WantParams添加"String"类型的key，"WantParams"类型的值value。 |
| getValue | Object | String key | 获取键值为key的属性值，如果键值不存在则返回null。 |
| toWantParamsString | String | - | 将WantParams对象转换为Json字符串。 |


### 注意事项
  * addValue和getValue中的key不能包含特殊字符；如\t、\r、\n等。
  * 在使用手动方式(非WantParams)自定义字符串时，key和value均不能包含特殊字符。
  * array和object不支持使用手动方式进行使用。
  * double的小数点后有效小数位为6位。

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