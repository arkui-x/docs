# Android平台构建ArkUI应用

## 简介

本教程主要讲述如何利用ArkUI-X SDK完成Android应用开发，实现基于ArkTS的声明式开发范式在Android平台显示。包括：

* Android应用工程集成ArkUI-X SDK
* Android应用工程集成ArkUI-X应用编译产物
* 使用ACE Tools和DevEco Studio集成ArkUI-X SDK进行Android应用开发

### Android 工程创建
通过ACE Tools或DevEco Studio创建一个ArkUI-X应用工程（示例工程名为HelloWorld），其工程目录下的.arkui-x/android文件代表对应的Android工程。Android应用的入口Application和Activity类，这两个类需要继承自ArkUI提供的基类，Activity继承StageActivity类，Application则会继承StageApplication类，Application也可以通过代理类StageApplicationDelegate使用，详情参见[使用说明](https://gitee.com/arkui-x/docs/tree/master/zh-cn/application-dev/reference/arkui-for-android):
* Activity类\
该类名通过通过module名和ability名拼接而得，一个ability对应一个Android工程侧的Activity类。详情参见[Ability使用说明](https://gitee.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/start-with-ability-on-android.md):
    ```java
    package com.example.helloworld;

    import android.os.Bundle;
    import android.util.Log;

    import ohos.stage.ability.adapter.StageActivity;

    public class EntryEntryAbilityActivity extends StageActivity {
        @Override
        protected void onCreate(Bundle savedInstanceState) {
            Log.e("HiHelloWorld", "EntryMainAbilityActivity");
            
            setInstanceName("com.example.helloworld:entry:EntryAbility:");// ArkUI-X应用编译产物在应用工程assets/js中存放的目录名（即模块实例名）。
            super.onCreate(savedInstanceState);
        }
    }
    ```
* Application类
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


### Android 工程编译
对Android工程编译时，ACE Tools或DevEco Studio会完成两个步骤：

* 集成ArkUI-X SDK \
    Android工程集成ArkUI-X SDK遵循Android应用工程集成Jar和动态库规则，即SDK组成清单中的arkui_android_adapter.jar包拷贝到libs目录，动态库（libarkui_android.so\libhilog_android.so\libhilog.so\libresourcemanager.so）会自动拷贝到libs/arm64-v8a目录。
* 集成ArkUI-X应用编译产物 \
ArkUI-X编译产物生成后，拷贝到Android应用工程assets/arkui-x目录下。这里“arkui-x”目录名称是固定的，不能更改；详情参见[ArkUI-X应用工程结构说明](https://gitee.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/package-structure-guide.md)

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



完成上述步骤后即可按照Android应用构建流程，构建ArkUI Android应用，并且可以安装至Android手机后运行。
### 参考

【1】[ArkUI-X Samples仓](https://gitee.com/arkui-x/samples)

