# How to Use the ArkUI-X SDK for Android AAR Development

## Overview
This tutorial describes how to use the ArkUI-X SDK for Android AAR development, thereby implementing the display of the ArkTS-based declarative development paradigm on Android. It covers the following topics:

* Cross-platform library project development
* AAR integration in Android app projects


### Integrating the ArkUI-X SDK for Android AAR Development
You can use ACE Tools or DevEco Studio to integrate the ArkUI-X SDK for Android AAR development.
* ACE Tools
1. Run **ace create** to create a cross-platform app project. In this example, the project name is **demo** and the project type is **library**.
    ```
    ace create [project]
    ```
2. Run **ace build aar** to build the Android AAR library.
    ```
    ace build aar
    ```
* DevEco Studio
1. Create a cross-platform library project.
2. Run **build app** to build the Android AAR library.
### Using the Android AAR in App Projects

Use Android Studio to create an app project and add the Android AAR library to the **libs** directory of the project.
**Application**
* Inherited Class Invoking
```java
package com.example.helloworld;

import com.example.myaar.MyApplication;

public class MainApplication extends MyApplication { 

}
```
* Proxy Class Invoking
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
         android:theme="@style/Theme.Helloworld"><!-- Set name to MainApplication. -->
     <activity android:name="com.example.myaar.EntryMainAbilityActivity" 
         android:windowSoftInputMode="adjustResize |stateHidden"
         android:configChanges="orientation|keyboard|layoutDirection|screenSize|uiMode|smallestScreenSize"
         ><!-- Set name to EntryMainAbilityActivity in the AAR library. -->
             <intent-filter>
                 <action android:name="android.intent.action.MAIN" />
                 <category android:name="android.intent.category.LAUNCHER" />
             </intent-filter>
         </activity>
     </application>

 </manifest>
```
**build.gradle**
* Add the NDK and build dependency directory. The configuration items are the same as those for ArkUI app building on Android.

Now you can build an ArkUI app according to the Android app building process.
