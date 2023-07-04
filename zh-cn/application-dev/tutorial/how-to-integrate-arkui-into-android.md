# Android平台构建ArkUI应用

## 总览

本教程主要讲述如何利用ArkUI-X SDK完成Android应用开发，实现基于ArkTS的声明式开发范式在Android平台显示。包括：

* Android应用工程集成ArkUI-X SDK
* Android应用工程集成ArkUI JSBundle实例
* 使用ace tool和DevEco Studio集成ArkUI-X SDK进行Android应用开发


### Android应用工程集成ArkUI跨平台SDK

* Android工程集成ArkUI跨平台SDK遵循Android应用工程集成Jar和动态库规则，即SDK组成清单中的arkui_android_adapter.jar包拷贝到libs目录，动态库（libarkui_android.so\libace_napi.so\libace_napi_ark.so等）拷贝到libs/arm64-v8a目录。
* Android应用的入口Application和Activity，其中Activity需要继承自ArkUI提供的基类，Application可以通过代理类使用，详情参见[使用说明](https://gitee.com/arkui-x/android#使用说明)，比如:

**Activity部分** 
* 一个ability对应一个Android工程侧的Activity类。
* 该Activity类名EntryMainAbilityActivity是通过module名加ability名拼接而得，不能随意改动。
```java
package com.example.helloworld;

import android.os.Bundle;
import android.util.Log;

import ohos.stage.ability.adapter.StageActivity;

public class EntryMainAbilityActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        Log.e("HiHelloWorld", "EntryMainAbilityActivity");
        
        setInstanceName("com.example.helloworld:entry:MainAbility:");// ArkUI JSBundle在应用工程assets/js中存放的目录名（即模块实例名）。
        super.onCreate(savedInstanceState);
    }
}
```

**Application部分**
* 继承方式

```java
package com.example.helloworld;

import android.util.Log;

import ohos.stage.ability.adapter.StageApplication;

public class MainApplication extends StageApplication {
    private static final String LOG_TAG = "HiHelloWorld";

    private static final String RES_NAME = "res";

    @Override
    public void onCreate() {
        Log.e(LOG_TAG, "MyApplication");
        super.onCreate();
        Log.e(LOG_TAG, "MyApplication onCreate");
    }
}
```
* 代理类方式

```java
package com.example.helloworld;


import android.app.Application;
import android.content.res.Configuration;
import android.util.Log;

import ohos.stage.ability.adapter.StageApplicationDelegate;

public class MainApplication extends Application {
    private StageApplicationDelegate appDelegate = null;

    public void onCreate() {
        super.onCreate();
        this.appDelegate = new StageApplicationDelegate();
        this.appDelegate.initApplication(this);
    }
    public void onConfigurationChanged(Configuration newConfig) {
        super.onConfigurationChanged(newConfig);
        if (this.appDelegate == null) {
            Log.e("StageApplication", "appDelegate is null");
        } else {
            this.appDelegate.onConfigurationChanged(newConfig);
        }
    }
}
```
### Android应用工程集成ArkUI JSBundle实例

ArkUI JSBundle生成后，拷贝到Android应用工程assets/arkui-x目录下。这里“arkui-x”目录名称是固定的，不能更改；entry为模块实例名称，可以自定义名称；entryTest为测试模块，可按照实际需求拷贝；systemres是JSBundle必须的系统资源，需从ArkUI-X SDK中拷贝。

```
src/main/assets/arkui-x
    ├── entry
    |   └── ets
    |       ├── modules.abc
    |       └── sourceMaps.map
    ├── entryTest
    └── systemres
```
**AndroidManifest.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.demo">

<uses-permission android:name="android.permission.INTERNET"/>
    <application
        android:name=".MainApplication"
        android:allowBackup="true"
        android:icon="@drawable/arkuix"
        android:label="@string/app_name"
        android:supportsRtl="true"
        android:extractNativeLibs="true"
        android:theme="@android:style/Theme.Light.NoTitleBar"><!-- 将name设为MainApplication-->
	<activity android:exported="true" android:name=".EntryMainAbilityActivity"
		android:windowSoftInputMode="adjustResize |stateHidden"
		android:configChanges="orientation|keyboard|layoutDirection|screenSize|uiMode|smallestScreenSize|density"
		>
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity><!-- 将name设为EntryMainAbilityActivity-->
    </application>

</manifest>

```
**build.gradle**
* 添加ndk和编译依赖目录
```gradle
plugins {
    id 'com.android.application'
}

android {
    compileSdkVersion 33
    buildToolsVersion "30.0.3"

    defaultConfig {
        applicationId "com.example.demo"
        minSdkVersion 26
        targetSdkVersion 33
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"

        ndk {
            abiFilters "arm64-v8a", "armeabi-v7a"
        }//ndk
    }

    buildTypes {
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    dynamicFeatures = []

    sourceSets {
        main {
            jniLibs.srcDirs = ['libs']
        }
    }
}

dependencies {
    implementation fileTree(dir: 'libs', include: ['*.jar', '*.aar'])//编译依赖目录
    testImplementation 'junit:junit:4.+'
    androidTestImplementation 'com.android.support.test:runner:1.0.2'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.2'
}

```

完成上述步骤后即可按照Android应用构建流程，构建ArkUI Android应用。
### 使用ace tool和DevEco Studio集成ArkUI-X SDK进行Android应用开发
上述步骤均可通过ACE tools或DevEco Studio完成
* ACE tools
1. ace create 命令创建一个跨平台应用工程
2. 进入工程目录，执行ace build apk命令，就会执行上述步骤，并且构建出一个Android应用包。
* DevEco Studio
1. 创建跨平台工程
2. 通过执行build app选项，即可执行上述步骤，并且构建出Android应用
### 参考

【1】[ArkUI跨平台应用示例](https://gitee.com/arkui-x/samples)

