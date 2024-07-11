# Fragment跨平台开发指南

## 简介

本文介绍将ArkUI框架的UIAbility跨平台部署至Android平台Fragment的使用说明，实现Android原生Fragment和ArkUI跨平台Fragment的混合开发，方便开发者灵活部署跨平台界面。

## Android工程配置

- Android工程的PackageName需要与OpenHarmony工程的BundleName一致；

- 请在Android应用的gradle.properties文件，使能AndroidX：

```
android.useAndroidX=true
```

- 请在Android应用的build.gradle文件增加AndroidX Fragment库的依赖项：

```
dependencies {
    ...
    implementation  'androidx.appcompat:appcompat:1.4.1'
    ...
}
```

## ArkUI-X和Android平台集成所用关键类

### 应用工程Android逻辑部分的StageApplication

应用需要继承arkui_android_adapter.jar包所提供的StageApplication。StageApplication用于初始化资源路径以及加载配置信息，例如：

```java
package com.example.myapplication;
import ohos.stage.ability.adapter.StageApplication;

public class MyApplication extends StageApplication {
	...
}
```

### 应用工程Android逻辑部分Fragment的宿主Activity

原生Activity需要继承androidx.fragment.app.FragmentActivity，绑定StageFragment示例如下：

```java
package com.example.myapplication;

import android.os.Bundle;

import androidx.fragment.app.FragmentActivity;
import androidx.fragment.app.FragmentManager;

import ohos.stage.ability.adapter.StageFragment;

public class MainActivity extends FragmentActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        StageFragment fragment = new HiFragment();
        FragmentManager manager = getSupportFragmentManager();
        manager.beginTransaction().add(R.id.frag,fragment).commit();
    }
}
```

其中activity_main.xml文件示例如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <LinearLayout
        android:id="@+id/frag"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal">
    </LinearLayout>

</LinearLayout>
```

StageFragment支持传递参数，需使用关键字"params"，示例如下：

```java
StageFragment fragment = new HiFragment();
Bundle args = new Bundle();
args.putString("params", "{\"params\":[{\"key\":\"path\",\"type\":10}]}");
fragment.setArguments(args);
```

### 应用工程Android逻辑部分的StageFragment

Fragment需要继承arkui_android_adapter.jar包所提供的StageFragment，StageFragment主要功能是将Android中Fragment的生命周期与OpenHarmony中UIAbility的生命周期进行映射，例如：

```java
package com.example.myapplication;
import ohos.stage.ability.adapter.StageFragment;

public class HiFragment extends StageFragment {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.setInstanceName("com.example.myapplication:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```

为了将Fragment和UIAbility进行关联，需要重写StageFragment中的onCreate事件，在super.onCreate(savedInstanceState)之前设置instanceName，规则如下：

```
bundleName:moduleName:abilityName:
```

其中bundleName的值来自于OpenHarmony应用中app.json5配置文件，moduleName、abilityName的值来自于OpenHarmony应用中的module.json5配置文件。
