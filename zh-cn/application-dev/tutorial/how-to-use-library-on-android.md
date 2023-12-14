# Android平台基于ArkUI-X的AAR的构建与使用

## 简介
本教程主要讲述如何利用ArkUI-X SDK完成Android AAR开发，实现基于ArkTS的声明式开发范式在android平台显示。包括：

* 跨平台Library工程开发介绍
* AAR在Android应用工程的集成方式


### 使用ACE Tools和DevEco Studio集成ArkUI-X SDK进行Android AAR开发
可以通过通过ACE Tools或DevEco Studio完成
* ACE Tools
1. ace create 命令创建一个跨平台的library模版工程：
    ```
    ace create [project] -t library
    ```
2. 执行ace build aar命令，构建Android aar包。
    ```
    ace build aar
    ```
* DevEco Studio
1. 导入跨平台的Sample工程Library
2. 通过执行Build APP(s)选项，构建出Android aar包
### AAR在应用工程的使用

通过Android studio 创建一个应用工程，将我们上述的aar包添加到工程目录下的libs目录中
**Application部分**

* 继承调用
```java
package com.example.helloworld;

import com.example.myaar.MyApplication;

public class MainApplication extends MyApplication { 

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
         android:name=".MainApplication"
         android:theme="@style/Theme.Helloworld"><!-- 将name设为MainApplication-->
     <activity android:name="com.example.myaar.EntryMainAbilityActivity" 
         android:windowSoftInputMode="adjustResize |stateHidden"
         android:configChanges="orientation|keyboard|layoutDirection|screenSize|uiMode|smallestScreenSize"
         ><!-- 将name设为aar中的EntryMainAbilityActivity -->
             <intent-filter>
                 <action android:name="android.intent.action.MAIN" />
                 <category android:name="android.intent.category.LAUNCHER" />
             </intent-filter>
         </activity>
     </application>

 </manifest>
```
**build.gradle**
* 添加ndk和编译依赖目录,这部分配置项与Android平台构建ArkUI应用内容一致。

完成上述步骤后即可按照Android应用构建流程，构建ArkUI Android应用。