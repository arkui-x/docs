## Android平台构建ArkUI应用

#### 总览

本教程主要讲述如何利用ArkUI-X SDK完成Android应用开发，实现基于ArkTS的声明式开发范式在Android平台显示。包括：

* ArkUI-X SDK获取方法
* ArkUI-X SDK内容组成
* Android应用工程集成ArkUI-X SDK
* Android应用工程集成ArkUI JSBundle实例

##### 开发准备

**方式一**

* 通过镜像站点获取ArkUI-X SDK，适合ArkUI跨平台应用初学者。

| 版本源码                             | **版本信息** | **下载站点** | **SHA256校验码** |
| ------------------------------------ | ------------ | ------------ | ---------------- |
| ArkUI-X SDK包  | 0.1.0 Beta    | [站点]()     | [SHA256校验码]() |

**方式二**

* 通过ArkUI-X跨平台项目源码编译出SDK，适用于ArkUI跨平台应用高阶开发者。完成跨平台项目[代码下载](../../framework-dev/quick-start/readme.md)后，执行如下命令编译ArkUI-X Android平台SDK。

```
./build.sh --product-name arkui-cross --target-os android --ccache
```

##### SDK组成清单

ArkUI跨平台Android侧SDK主要有Java层和Native层两部分组成，Java层主要提供基于Activity和Application的ArkUI启动和JSBundle加载入口，Native层主要提供ArkUI渲染和NAPI实现等。

```
ArkUI_Android_SDK
    ├── engine
    │   ├── ace_android_adapter.jar              // ArkUI Android平台启动入口
    │   └── arm64-v8a
    │       ├── libace_android.so                // ArkUI引擎及Android适配层
    │       ├── libace_container_scope.so
    │       ├── libace_napi.so
    │       ├── libace_napi_ark.so
    │       ├── libace_ndk.so
    │       ├── libark_jsruntime.so
    │       ├── libglobal_resmgr.so
    │       ├── libhmicui18n.so
    │       ├── libhmicuuc.so
    │       └── libsec_shared.so
    └── plugins                                  // OpenHarmony接口定义实现
        ├── libanimator.so
        ├── libmediaquery.so
        ├── libpromptaction.so
        └── librouter.so
```

##### Android工程集成ArkUI跨平台SDK

* Android工程集成ArkUI跨平台SDK遵循Android应用工程集成Jar和动态库规则，即SDK组成清单中的ace_android_adapter.jar包拷贝到libs目录，动态库（libace_android.so\libace_napi.so\libace_napi_ark.so等）拷贝到libs/arm64-v8a目录。
* Android应用的入口Application和Activity需要继承自ArkUI提供的基类，详情参见[使用说明](https://gitee.com/arkui-x/android#使用说明)，比如:

**Activity部分**

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
        setVersion(VERSION_ETS);            // ArkUI开发范式类型，VERSION_JS:兼容JS的类Web开发范式，VERSION_ETS:基于ArkTS的声明式开发范式。
        setInstanceName("MainAbility");     // ArkUI JSBundle在应用工程assets/js中存放的目录名（即模块实例名）。
        super.onCreate(savedInstanceState);
    }
}
```

**Application部分**

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

##### Android工程集成ArkUI JSBundle实例

ArkUI JSBundle生成后，拷贝到Android应用工程assets/js目录下，比如：assets/js/MainAbility。这里“js”目录名称是固定的，不能更改；MainAbility为JSBundle模块实例名称，可以自定义名称。

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

【1】[ArkUI跨平台应用示例](https://gitee.com/arkui-x/samples)

