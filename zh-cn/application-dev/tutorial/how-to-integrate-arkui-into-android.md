## Android平台构建ArkUI应用

#### 总览

本教程主要讲述如何利用ArkUI-X SDK完成Android应用开发，实现基于ArkTS的声明式开发范式在Android平台显示。包括：

* Android应用工程集成ArkUI-X SDK
* Android应用工程集成ArkUI JSBundle实例
* 使用ace tool和DevEco Studio IDE集成ArkUI-X SDK进行Android应用开发


##### Android应用工程集成ArkUI跨平台SDK

* Android工程集成ArkUI跨平台SDK遵循Android应用工程集成Jar和动态库规则，即SDK组成清单中的arkui_android_adapter.jar包拷贝到libs目录，动态库（libarkui_android.so\libace_napi.so\libace_napi_ark.so等）拷贝到libs/arm64-v8a目录。
* Android应用的入口Application和Activity，其中Activity需要继承自ArkUI提供的基类，Application可以通过代理类使用，详情参见[使用说明](https://gitee.com/arkui-x/android#使用说明)，比如:

**Activity部分** 
* 注意该Activity类名EntryMainAbilityActivity通过module名和ability名拼接规则命名，不能随意改动，详情参见[多module]()
```
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

```
package com.example.helloworld;

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
* 代理类方式

```
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
##### Android应用工程集成ArkUI JSBundle实例

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

实际上，上述所有步骤完成后即可按照Android应用构建流程，构建ArkUI Android应用。
##### 使用ace tool和DevEco Studio IDE集成ArkUI-X SDK进行Android应用开发
上述步骤我们可以很方便地通过ace tool或DevEco Studio IDE完成
* ace tool
1. ace create 命令创建一个跨平台应用工程
2. 进入工程目录，执行ace build apk命令，就会执行上述步骤。对jsbundle的生成与拷贝，以及其所依赖的动态库文件和jar包的拷贝，并且最后构建出一个Android应用包。
* DevEco Studio IDE
1. 创建跨平台工程
2. 通过执行build app选项，即可执行上述步骤，并且构建出Android应用
##### 参考

【1】[ArkUI跨平台应用示例](https://gitee.com/arkui-x/samples)

