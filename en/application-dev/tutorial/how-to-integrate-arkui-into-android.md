# How to Build an ArkUI Application on Android

## Overview

This tutorial describes how to use the ArkUI-X SDK to develop Android applications, thereby implementing the display of the ArkTS-based declarative development paradigm on Android. It covers the following topics:

* Integrating the ArkUI-X SDK into an Android project
* Integrating ArkUI-X building products into an Android project
* Using ACE Tools and DevEco Studio to integrate the ArkUI-X SDK for Android application development

### Creating an Android Project
Use ACE Tools or DevEco Studio to create an ArkUI-X application project (named **HelloWorld** in this example). An **.arkui-x/android** file is generated in the project directory. The entry classes **Application** and **Activity** of the Android application must inherit from the ArkUI base classes **StageApplication** and **StageActivity**, respectively. The **Application** class can also be used through the **StageApplicationDelegate** class. For details, see [ArkUI-X APIs for Android Integration](../reference/arkui-for-android).
* Activity class

  The class name is obtained by combining the module name and ability name. One ability corresponds to one **Activity** class of Android. For details, see [Developing an Android Application on Stage Model](../quick-start/start-with-ability-on-android.md).

    ```java
    package com.example.helloworld;

    import android.os.Bundle;
    import android.util.Log;

    import ohos.stage.ability.adapter.StageActivity;

    public class EntryEntryAbilityActivity extends StageActivity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            Log.e("HiHelloWorld", "EntryMainAbilityActivity");
            
            setInstanceName("com.example.helloworld:entry:EntryAbility:"); // Name of the directory (module instance name) for storing ArkUI-X build products in assets/js of the application project.
            super.onCreate(savedInstanceState);
        }
    }
    ```
* Application class
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


### Building an Android Project
ACE Tools or DevEco Studio performs the following steps when building an Android project:

* Integrate the ArkUI-X SDK.

    The integration complies with the JAR and dynamic library integration rules of Android application projects. Specifically, you need to copy the **arkui_android_adapter.jar** package in the SDK components to the **libs** directory. In this case, the dynamic libraries (**libarkui_android.so**, **libhilog_android.so**, **libhilog.so**, and **libresourcemanager.so**) are automatically copied to the **libs/arm64-v8a** directory.
* Integrate ArkUI-X build products.

  After ArkUI-X build products are generated, copy them to the **assets/arkui-x** (fixed) directory of the Android application project. For details, see [ArkUI-X Application Project Structure](../quick-start/package-structure-guide.md).

    ```
    src/main/assets/arkui-x
        ├── entry
        |   ├── ets
        |   |   ├── modules.abc
        |   |   └── sourceMaps.map
        |   ├── resouces.index
        |   ├── resouces
        |   └── module.json
        └── systemres
    ```



After the preceding steps are complete, you can build the ArkUI Android application according to the Android application building process. The application built will be able to be installed and run on Android phones.
### Reference

[ArkUI-X Samples](https://gitee.com/arkui-x/samples)
