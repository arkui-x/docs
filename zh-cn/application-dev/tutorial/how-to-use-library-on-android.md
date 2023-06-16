# Android平台构建ArkUI AAR与使用

## 总览
本教程主要讲述如何利用ArkUI-X SDK完成Android AAR开发，实现基于ArkTS的声明式开发范式在framework平台显示。包括：

* Android AAR工程集成ArkUI-X SDK
* Android AAR工程集成ArkUI JSBundle实例
* 使用ace tool和DevEco Studio集成ArkUI-X SDK进行Android AAR开发
* AAR在android应用工程的使用
### Android AAR工程集成ArkUI跨平台SDK
* Android AAR工程集成ArkUI跨平台SDK遵循Android应用工程集成Jar和动态库规则，即SDK组成清单中的arkui_android_adapter.jar包拷贝到libs目录，动态库（libarkui_android.so\libace_napi.so\libace_napi_ark.so等）拷贝到libs/arm64-v8a目录。

**Activity部分**
* 注意该Activity类名EntryMainAbilityActivity通过module名和ability名拼接规则命名，不能随意改动，详情参见[多module]()
```java
package com.example.myaar;

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
``` java
package com.example.myaar;

import android.util.Log;

import ohos.stage.ability.adapter.StageApplication;

public class MyApplication extends StageApplication {
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
### Android AAR程集成ArkUI JSBundle实例
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

### 使用ACE tools和DevEco Studio集成ArkUI-X SDK进行Android AAR开发
上述步骤均可通过ACE tools或DevEco Studio完成
* ACE tools
1. ace create 命令创建一个跨平台应用工程
2. 进入工程后，通过ace create aar命令创建一个aar工程
3. 执行ace build aar命令，就会执行上述步骤,并且构建出Android aar包。
* DevEco Studio
1. 创建跨平台library工程
2. 通过执行build app选项，即可执行上述步骤，并且构建出Android aar包
### AAR在应用工程的使用

通过Android studio 创建一个应用工程，将我们上述的aar包添加到工程目录下的libs目录中
**Application部分**
* 继承调用
```java
package com.example.helloworld;

import com.example.myaar.MyApplication;

public class MainApplication extends MyApplication { //

}
```
* 代理类调用
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
**AndroidManifest.xml**
```xml
<?xml version="1.0" encoding="utf-8"?>
 <manifest xmlns:android="http://schemas.android.com/apk/res/android"
     package="com.example.test_aar_demo">

 <uses-permission android:name="android.permission.INTERNET"/>
     <application
         android:name="com.example.test_aar_demo.MainApplication"
         android:allowBackup="true"
         android:icon="@drawable/hihelloworld"
         android:label="@string/app_name"
         android:roundIcon="@mipmap/ic_launcher_round"
         android:supportsRtl="true"
         android:theme="@style/Theme.Helloworld">
     <activity android:name="com.example.myaar.EntryMainAbilityActivity" 
         android:windowSoftInputMode="adjustResize |stateHidden"
         android:configChanges="orientation|keyboard|layoutDirection|screenSize|uiMode|smallestScreenSize"
         ><!-- 只需将name设为aar中的EntryMainAbilityActivity -->
             <intent-filter>
                 <action android:name="android.intent.action.MAIN" />

                 <category android:name="android.intent.category.LAUNCHER" />
             </intent-filter>
         </activity>
     </application>

 </manifest>
```
完成上述步骤后即可按照Android应用构建流程，构建ArkUI Android应用。