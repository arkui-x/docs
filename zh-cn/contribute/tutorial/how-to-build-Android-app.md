## Android平台构建ArkUI应用

#### 总览

本教程主要讲述如何利用ArkUI-X SDK完成Android应用开发，实现ArkUI开发范式在安卓平台显示。包括：

* ArkUI-X SDK获取方法
* ArkUI-X SDK内容组成
* Android应用工程集成ArkUI-X SDK
* Android应用工程集成ArkUI JSBundle实例

##### 开发准备

**方式一**

* 通过镜像站点获取ArkUI-X SDK，适合ArkUI跨平台应用初学者。

| 版本源码                             | **版本信息** | **下载站点** | **SHA256校验码** |
| ------------------------------------ | ------------ | ------------ | ---------------- |
| ArkUI-X SDK包（Android） | 0.1.0 Beta    | [站点]()     | [SHA256校验码]() |

**方式二**

* 通过ArkUI-X跨平台项目源码编译出SDK，适用于ArkUI跨平台应用高阶开发者。完成跨平台项目[代码下载](../../application-dev/quick-start/README.md)后，执行如下命令编译ArkUI-X Android平台SDK。

```
./build.sh --product-name arkui-cross --target-os android --ccache
```

##### SDK组成清单

ArkUI跨平台Android侧SDK主要有Java层和Native层两部分组成，Java层主要提供基于Activity和Application的ArkUI启动和JSBundle加载入口，Native层主要提供ArkUI渲染和NAPI实现等。

```
ArkUI_Android_SDK
    ├── ace_android_adapter.jar              // ArkUI Android平台启动入口
    ├── arkui
    │   ├── ace_engine_cross
    │   │   ├── libace_android.so            // ArkUI引擎及Android适配层
    │   │   ├── libace_ndk.so                // ArkUI XComponent NDK接口
    │   │   ├── libanimator.so
    │   │   ├── libpromptaction.so
    │   │   ├── libmediaquery.so             // ArkUI媒体查询接口实现
    │   │   ├── libdevice.so
    │   │   ├── libconfiguration.so
    │   │   ├── libgrid.so
    │   │   └── librouter.so                 // ArkUI页面跳转路由接口实现
    │   └── napi
    │       ├── libace_napi.so               // napi接口实现层
    │       ├── libace_container_scope.so
    │       └── libace_napi_ark.so           // napi适配quickjs引擎
    └── lib.unstripped                       // 带符号动态库
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
                └── libace_napi_quickjs.so
```

##### Android工程集成ArkUI跨平台SDK

* Android工程集成ArkUI跨平台SDK遵循Android应用工程集成Jar和动态库规则，即SDK中ace_android_adapter.jar包拷贝到libs目录，动态库（libace_android.so\libace_napi.so\libace_napi_quickjs.so等）拷贝到libs/arm64-v8a目录。
* Android应用的入口Application和Activity需要继承自ArkUI提供的基类，详情参见[使用说明](https://gitee.com/arkui-x/android#使用说明)，比如:

**Activity部分**

```
package com.example.helloworld;

import android.os.Bundle;
import android.os.PersistableBundle;
import android.util.Log;

import ohos.ace.adapter.AceActivity;

import androidx.annotation.Nullable;

public class HiHelloWorldActivity extends AceActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        Log.d("HiHelloWorld", "HiHelloWorldActivity");
        setVersion(VERSION_ETS); // ArkUI开发范式选择，VERSION_JS:ArkUI类Web范式，VERSION_ETS:ArkUI声明式范式。
        setInstanceName("MainAbility"); // ArkUI JSBundle在应用工程assets中存放的目录名（即模块实例名）。
        super.onCreate(savedInstanceState);
    }
}
```

**Application部分**

```
package com.example.helloworld;

import android.util.Log;

import ohos.ace.adapter.AceApplication;

public class HiHelloWorldApplication extends AceApplication {
    private static final String LOG_TAG = "HiHelloWorld";

    private static final String RES_NAME = "res";

    @Override
    public void onCreate() {
        Log.d(LOG_TAG, "HiHelloWorldApplication");
        super.onCreate();
        Log.d(LOG_TAG, "HiHelloWorldApplication is onCreate");
    }
}
```

##### Android工程集成ArkUI JSBundle实例

ArkUI JSBundle生成后，拷贝到Android应用工程assets/js目录下，比如：assets/js/MainAbility。这里“js”目录名称是固定的，不能更改，MainAbility为模块实例名称，定义在config.json清单文件中。

```
src/main/assets/js/MainAbility
    ├── app.js
    ├── manifest.json
    └── pages
        └── index
            └── index.js
```

实际上，上述所有步骤完成后即可按照Android应用构建流程，构建ArkUI Android应用。

##### 参考

【1】[ArkUI跨平台应用Android侧示例](https://gitee.com/arkui-x/samples)

【2】[ArkUI JSBundle获取指南]()

