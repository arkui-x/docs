# Android 原生页面与跨平台页面跳转开发指南

## 概述

本文档介绍如何在现有 Android 原生工程中集成 ArkUI-X，并实现以下两类页面跳转：

- Android 原生页面跳转到跨平台页面
- 跨平台页面跳转到 Android 原生页面

## 整体架构

Android 与 ArkUI-X 混合开发采用双框架共存模式。宿主应用仍使用 Android 原生 `Application` 和 `Activity` 体系，ArkUI-X 则通过适配层完成运行时初始化和页面承载。

核心组件如下：

- `StageApplication`：ArkUI-X 应用级适配器，负责初始化 ArkUI-X 运行时环境
- `StageActivity`：跨平台页面承载类，负责映射 Android Activity 生命周期并加载跨平台页面
- `arkui_android_adapter.jar`：Android 侧 ArkUI-X 适配层核心库
- `.so` 动态库：ArkUI-X 运行时底层实现
- `assets/arkui-x/`：跨平台页面产物与系统资源目录

页面跳转链路如下：

1. Android 系统启动应用并加载宿主 `Application`
2. 宿主 `Application` 初始化 ArkUI-X 运行时
3. 原生页面通过 `Intent` 跳转到跨平台页面承载页
4. 承载页通过 `setInstanceName()` 绑定目标 Ability 并加载跨平台页面
5. 满足命名映射规则时，跨平台页面也可通过 `startAbility()` 拉起原生 Activity

## 前置条件

在开始接入前，请先在 DevEco Studio 中完成一次 ArkUI-X 工程构建，确保生成以下 Android 集成产物：

- `libs` 目录，包含 `arkui_android_adapter.jar` 和各架构 `.so` 库
- `assets` 目录，例如 `~/.arkui-x/android/app/src/main/assets/`
- 自动生成的 `EntryEntryAbilityActivity.java`

建议在接入前先检查以下约束：

- Android 工程的 `packageName` 与 ArkUI-X 工程的 `bundleName` 保持一致
- ArkUI-X 承载页类以 DevEco Studio 自动生成代码为准
- 相关 Activity 已在 `AndroidManifest.xml` 中注册

## 接入步骤

### 步骤 1：拷贝 ArkUI-X 库文件

将 ArkUI-X 工程构建生成的整个 `libs` 目录拷贝到 Android 工程的 `app/libs` 下。

目录示例如下：

```shell
app/libs/
├── arkui_android_adapter.jar
├── arm64-v8a/
│   ├── libarkui_android.so
│   └── ...
└── armeabi-v7a/
    ├── libarkui_android.so
    └── ...
```

### 步骤 2：拷贝跨平台页面产物

将 `~/.arkui-x/android/app/src/main/assets/` 下的全部文件和子目录完整拷贝到 Android 工程的 `app/src/main/assets/` 下，建议直接覆盖更新。

`assets` 是跨平台页面运行时资源根目录，`StageActivity` 会从该目录查找页面字节码和系统资源。

常见目录如下：

- `arkui-x/entry/`：业务模块产物，通常包含 `ark_module.json`、`module.json`、`ets/modules.abc`、`resources/`
- `arkui-x/systemres/`：ArkUI-X 运行时系统资源

如果 `assets` 拷贝不完整，常见现象包括黑屏、资源加载失败或运行时异常。

### 步骤 3：配置 `build.gradle`

在 `app/build.gradle` 中配置本地动态库目录和 jar 依赖。

配置 `sourceSets`：

```gradle
android {
    sourceSets {
        main() {
            jniLibs.srcDirs = ['libs']
        }
    }
}
```

添加依赖：

```gradle
dependencies {
    implementation files('libs\\arkui_android_adapter.jar')
}
```

### 步骤 4：修改宿主 `Application`

将宿主工程中的 `Application` 修改为继承 `StageApplication`。原有业务初始化逻辑可以保留，但需确保调用 `super.onCreate()`。

参考实现：

```java
package com.example.myapplication;

import android.util.Log;
import ohos.stage.ability.adapter.StageApplication;

public class MyApplication extends StageApplication {
    private static final String LOG_TAG = "HiHelloWorld";

    @Override
    public void onCreate() {
        Log.e(LOG_TAG, "MyApplication");
        super.onCreate();
        Log.e(LOG_TAG, "MyApplication onCreate");
    }
}
```

### 步骤 5：接入跨平台页面承载页

不要手动创建该类，建议直接使用 DevEco Studio 自动生成的 `EntryEntryAbilityActivity.java`。

该类需要继承 `StageActivity`，并在 `onCreate()` 中通过 `setInstanceName()` 指定目标 Ability。

参考实现：

```java
package com.example.myapplication;

import android.os.Bundle;
import android.util.Log;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        Log.e("HiHelloWorld", "EntryEntryAbilityActivity");
        setInstanceName("com.example.myapplication:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```

`setInstanceName()` 的值需要与 ArkUI-X 工程实际生成的模块信息保持一致，否则页面可能无法正确加载。

### 步骤 6：配置 `AndroidManifest.xml`

在 `AndroidManifest.xml` 中声明宿主 `Application` 和跨平台页面承载页。

`application` 节点示例：

```xml
<application
    android:name=".MyApplication"
    android:extractNativeLibs="true"
    ... >
```

`activity` 节点示例：

```xml
<activity
    android:name=".EntryEntryAbilityActivity"
    android:windowSoftInputMode="adjustResize|stateHidden"
    android:configChanges="orientation|keyboard|layoutDirection|screenSize|uiMode|smallestScreenSize|density"
    android:exported="true">
</activity>
```

## 页面跳转开发

### Android 原生页面跳转到跨平台页面

在原生 Activity 中，通常通过 `Intent` 跳转到跨平台页面承载页，例如 `EntryEntryAbilityActivity`。

#### 场景 1：无参跳转

如果只需要完成页面切换、不涉及参数传递，可直接使用以下方式。

```java
package com.example.myapplication;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btn = findViewById(R.id.buttonNavigate);
        btn.setOnClickListener(view -> {
            Intent intent = new Intent();
            intent.setClass(this, EntryEntryAbilityActivity.class);
            startActivity(intent);
        });
    }
}
```

如果跳转时还需要向跨平台页面传递参数，可继续使用同一跳转方式，并通过 `Intent` 的 `params` 字段传参。

#### 场景 2：带参跳转

##### 方式 1：手动拼接 `params`

手动传参时，`putExtra()` 的键固定为 `params`，值为 JSON 字符串。

参数格式和支持的参数类型可参考：

- [参数格式](../quick-start/start-with-ability-on-android.md#参数格式)
- [支持的参数类型列表](../quick-start/start-with-ability-on-android.md#支持的参数类型列表)

Java 示例：

```java
package com.example.myapplication;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btn = findViewById(R.id.buttonNavigate);
        btn.setOnClickListener(view -> {
            Intent intent = new Intent();
            intent.setClass(this, EntryEntryAbilityActivity.class);
            intent.putExtra("params",
                "{\"params\":[{\"key\":\"keyfirst\",\"type\":1,\"value\":\"keyvalue\"}," +
                "{\"key\":\"keysecond\",\"type\":9,\"value\":\"2.3\"}," +
                "{\"key\":\"keythird\",\"type\":5,\"value\":\"2\"}," +
                "{\"key\":\"keyfourth\",\"type\":10,\"value\":\"test\"}]}");
            startActivity(intent);
        });
    }
}
```

ArkTS 接收示例：

```ts
export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    console.log("value = " + want.parameters?.keyfirst)
    console.log("value = " + want.parameters?.keysecond)
    console.log("value = " + want.parameters?.keythird)
    console.log("value = " + want.parameters?.keyfourth)
  }
}
```

##### 方式 2：使用 `WantParams`（推荐）

推荐优先使用 `WantParams` 传递参数。该方式可读性更好，也更适合后续扩展复杂参数。

参数格式、支持类型和注意事项可参考：

- [参数格式](../quick-start/start-with-ability-on-android.md#参数格式-1)
- [支持的参数类型](../quick-start/start-with-ability-on-android.md#支持的参数类型)
- [注意事项](../quick-start/start-with-ability-on-android.md#注意事项)

Java 示例：

```java
package com.example.myapplication;

import android.content.Intent;
import android.os.Bundle;
import android.widget.Button;
import androidx.appcompat.app.AppCompatActivity;
import ohos.stage.ability.adapter.WantParams;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        Button btn = findViewById(R.id.buttonNavigate);
        btn.setOnClickListener(view -> {
            Intent intent = new Intent();
            intent.setClass(this, EntryEntryAbilityActivity.class);
            WantParams wantParams = new WantParams();
            wantParams.addValue("stringKey", "normal")
                    .addValue("intKey", -2147483648)
                    .addValue("doubleKey", -6.9)
                    .addValue("boolKey", true)
                    .addValue("arrayKey", new boolean[] { false, true })
                    .addValue("wantParamsKey", new WantParams().addValue("stringKey2", "It's me."));
            intent.putExtra("params", wantParams.toWantParamsString());
            startActivity(intent);
        });
    }
}
```

ArkTS 接收示例：

```ts
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

无参跳转适合验证集成链路，实际业务开发优先推荐使用 `WantParams` 传参。

### 跨平台页面跳转到 Android 原生页面

跨平台页面可以通过 `startAbility()` 拉起符合映射规则的 Android 原生 Activity。

命名规则如下：

- 原生 Activity 的 `packageName` 需要与应用 `bundleName` 一致
- 原生 Activity 的类名规则为 `moduleName + abilityName + Activity`

例如：

- `moduleName` 为 `entry`
- `abilityName` 为 `Jump`
- 对应原生 Activity 类名为 `EntryJumpActivity`

ArkTS 示例：

```ts
let want: Want = {
  bundleName: 'com.example.helloworld',
  moduleName: 'entry',
  abilityName: 'Jump',
  parameters: { id: 1, name: 'ArkUI-X' }
};

let context = getContext(this) as common.UIAbilityContext;
context.startAbility(want, (err, data) => {
});
```

Android 原生页示例：

```java
public class EntryJumpActivity extends AppCompatActivity {
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

## 快速检查清单

如果页面无法正常跳转，建议优先检查以下内容：

- `packageName` 是否与 ArkUI-X 工程的 `bundleName` 一致
- `EntryEntryAbilityActivity` 是否来自自动生成代码
- `setInstanceName()` 是否与实际模块信息一致
- `app/libs` 中是否已包含 `arkui_android_adapter.jar` 和所需 `.so` 库
- `app/src/main/assets/arkui-x/` 是否完整
- `AndroidManifest.xml` 中是否已注册 `MyApplication` 和相关 Activity
- 原生与 ArkUI-X 两侧的参数名是否一致

## 常见问题

### Q1：应用启动时崩溃，提示找不到 ArkUI-X 相关类

可能原因：

- `MyApplication` 未继承 `StageApplication`
- `AndroidManifest.xml` 中未配置正确的 `android:name`

处理建议：

- 检查 `application` 标签中的 `android:name` 是否正确设置为 `.MyApplication`
- 检查宿主 `Application` 是否正确调用 `super.onCreate()`

### Q2：跳转到跨平台页面后黑屏或崩溃

可能原因：

- 未调用 `setInstanceName()`
- `setInstanceName()` 格式错误
- `assets/arkui-x/` 目录缺少对应的 `.abc` 或资源文件

处理建议：

- 确保在 `onCreate()` 中调用 `setInstanceName()`
- 对照构建产物检查实例名是否正确
- 检查 `assets/arkui-x/entry/ets/modules.abc` 等文件是否完整

### Q3：跨平台页面无法拉起原生页面

可能原因：

- `packageName` 与 `bundleName` 不一致
- 原生 Activity 类名不符合映射规则
- 目标 Activity 未在 `AndroidManifest.xml` 中注册

处理建议：

- 检查应用包名是否一致
- 检查目标 Activity 是否符合 `moduleName + abilityName + Activity` 命名规则
- 检查目标 Activity 是否已正确注册

### Q4：参数传递失败

可能原因：

- `params` 内容格式不正确
- 手动方式传入了不支持的复杂类型
- ArkUI-X 侧读取的字段名与原生侧传入字段名不一致

处理建议：

- 检查 `putExtra("params", ...)` 的内容是否符合约定格式
- 复杂类型优先使用 `WantParams`
- 检查两侧参数键名是否完全一致

## 项目结构示例

```shell
app/
├── build.gradle
├── src/
│   ├── main/
│   │   ├── java/com/example/myapplication/
│   │   │   ├── MainActivity.java
│   │   │   ├── MyApplication.java
│   │   │   ├── EntryEntryAbilityActivity.java
│   │   │   └── EntryJumpActivity.java
│   │   ├── res/
│   │   │   ├── layout/
│   │   │   │   ├── activity_main.xml
│   │   │   │   └── activity_jump.xml
│   │   │   └── ...
│   │   └── assets/arkui-x/
│   │       ├── entry/
│   │       │   ├── ark_module.json
│   │       │   ├── module.json
│   │       │   ├── ets/modules.abc
│   │       │   └── resources/
│   │       └── systemres/
│   ├── test/
│   └── androidTest/
└── libs/
    ├── arkui_android_adapter.jar
    ├── arm64-v8a/
    │   └── *.so
    └── armeabi-v7a/
        └── *.so
```

## 参考资料

- [ArkUI-X Android集成官方文档](./how-to-integrate-arkui-into-android.md)
- [通过Stage模型开发Android端应用指南](../quick-start/start-with-ability-on-android.md)
- [Android开发官方文档](https://developer.android.com/guide)
