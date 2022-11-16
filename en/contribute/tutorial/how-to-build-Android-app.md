## Building ArkUI Applications on Android

#### Introduction

This tutorial describes how to use the ArkUI-X SDK to develop Android applications, thereby implementing the display of the ArkTS-based declarative development paradigm on Android. It covers the following contents:

* Obtaining the ArkUI-X SDK
* ArkUI-X SDK Components
* Integrating the ArkUI-X SDK into an Android Project
* Integrating an ArkUI JS Bundle Instance into an Android Project

##### Obtaining the ArkUI-X SDK

**Method 1**

* Obtain the ArkUI-X SDK from the mirror site. This method is suitable for beginners of ArkUI cross-platform applications.

| Source Code                | Version| Mirror| SHA-256 Checksum|
| ------------------------ | ------------ | ------------ | ---------------- |
| ArkUI-X SDK (Android)| 0.1.0 Beta   | [Download]()    | [Download]()|

**Method 2**

* Build the SDK using the source code of the ArkUI-X project. This method is applicable to high-level developers of ArkUI cross-platform applications. After [downloading the source code](../../framework-dev/quick-start/readme.md), run the following command to build the Android-based ArkUI-X SDK:

```
./build.sh --product-name arkui-cross --target-os android --ccache
```

##### ArkUI-X SDK Components

The Android-based ArkUI-X SDK consists of the Java layer and native layer. The Java layer provides the ArkUI startup and JS bundle loading entry based on activities and applications. The native layer provides ArkUI rendering and Native API (NAPI) implementation.

```
ArkUI_Android_SDK
    ├── ace_android_adapter.jar              // Entry for starting the ArkUI Android platform
    ├── arkui
    │   ├── ace_engine_cross
    │   │   ├── libace_android.so            // ArkUI engine and Android adaptation layer
    │   │   ├── libace_ndk.so                // ArkUI XComponent NDK interface
    │   │   ├── libanimator.so
    │   │   ├── libpromptaction.so
    │   │   ├── libmediaquery.so             // ArkUI media query interface
    │   │   ├── libdevice.so
    │   │   ├── libconfiguration.so
    │   │   ├── libgrid.so
    │   │   └── librouter.so                 // ArkUI page redirection interface
    │   └── napi
    │       ├── libace_napi.so               // NAPI implementation layer
    │       ├── libace_container_scope.so
    │       └── libace_napi_ark.so           // NAPI adaptation to the Ark JS engine.
    └── lib.unstripped                       // Signed dynamic library
        └── arkui
            ├── ace_engine_cross
            │   ├── libace_android.so
            │   ├── libace_ndk.so
            │   ├── libmediaquery.so
            │   ├── libdevice.so
            │   ├── libconfiguration.so
            │   ├── libgrid.so
            │   ├── libmediaquery.so
            │   └── librouter.so
            └── napi
                ├── libace_napi.so
                └── libace_napi_ark.so
```

##### Integrating the ArkUI-X SDK into an Android Project

* The integration complies with the JAR and dynamic library integration rules of Android application projects. Specifically, you need to copy the **ace_android_adapter.jar** package in the SDK to the **libs** directory and the dynamic libraries (such as **libace_android.so**, **libace_napi.so**, and **libace_napi_ark.so**) to the **libs/arm64-v8a** directory.
* The entry application and activity of the Android application must be inherited from the base class provided by ArkUI. For details, see [How to Use](https://gitee.com/arkui-x/android#how-to-use). For example:

**Activity part**

```
package com.example.helloworld;

import android.os.Bundle;
import android.os.PersistableBundle;
import android.util.Log;

import ohos.ace.adapter.AceActivity;

import androidx.annotation.Nullable;

public class MainActivity extends AceActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        Log.d("HiHelloWorld", "MainActivity");
        setVersion(VERSION_ETS);            // ArkUI development paradigm: VERSION_JS (JS-compatible web-like development paradigm) or VERSION_ETS (ArkTS-based declarative development paradigm).
        setInstanceName("MainAbility");     // Name of the directory (module instance name) where the ArkUI JS bundle is stored in assets/js of the application project.
        super.onCreate(savedInstanceState);
    }
}
```

**Application part**

```
package com.example.helloworld;

import android.util.Log;

import ohos.ace.adapter.AceApplication;

public class MyApplication extends AceApplication {
    private static final String LOG_TAG = "HiHelloWorld";

    private static final String RES_NAME = "res";

    @Override
    public void onCreate() {
        Log.d(LOG_TAG, "MyApplication");
        super.onCreate();
        Log.d(LOG_TAG, "MyApplication is onCreate");
    }
}
```

##### Integrating an ArkUI JS Bundle Instance into an Android Project

Copy the generated ArkUI JS bundle to the **assets/js** directory of the Android project, for example, **assets/js/MainAbility**. The **js** directory name is fixed and cannot be changed. **MainAbility** specifies the JS bundle module instance name, which can be customized.

```
src/main/assets/js/MainAbility
    ├── app.js
    ├── manifest.json
    └── pages
        └── index
            └── index.js
```

Now you can build an ArkUI Android application according to the Android application building process.

##### References

[1] [ArkUI-X App Samples](https://gitee.com/arkui-x/samples)

[2] [Obtaining ArkUI JS Bundle]()
