# Android平台接口扩展指南

本文介绍在Android平台上基于[N-API](../quick-start/ffi-napi-introduction.md)机制和插件管理机制，实现ArkUI侧调用Android侧方法。

## 接口定义

本文实现如下testplugin模块的log方法，功能为调用Android日志系统，打印来自ArkUI侧的数据。

| 名称 | 参数类型 | 返回类型 | 描述 |
| --- | --- | --- | --- |
| log | string | void | 在Android侧打印来自ArkUI侧的数据。|

## 开发流程

1、利用[ACE Tools工具](../quick-start/start-overview.md)或IDE创建Native工程。

```
// Ace Tools 创建Native工程
ace create demo -t plugin_napi
? Please enter the project name(demo)
? Please enter the bundleName (com.example.demo):
? Please enter the runtimeOS (1: OpenHarmony, 2: HarmonyOS): 1

Project created successfully! Target directory: ${当前目录}/demo.

In order to run your application, type:
    
    $ cd demo
    $ ace run
      
Your application code is in demo/entry.
```

2、通过Ace Tools或IDE创建Native模板后，ArkUI侧通过`import`引入Native侧的so文件，在创建出的`testplugin\entry\src\main\ets\pages\Index.ets`中，`import testplugin from 'libtestplugin.so'`，意为使用libtestplugin.so的能力，并将名为`testplugin`的ArkUI侧对象给到应用侧，开发者可通过该对象，基于插件管理机制开发Java插件，最终调用到在`testplugin\.arkui-x\android\app\src\main\java\com\example\testplugin\TestPlugin.java`中开发的Java `log`方法。

```typescript
// Index.ets

import testplugin from 'libtestplugin.so';

testplugin.log("log from ArkUI to Android");
```

3、按如下步骤实现testplugin模块的log方法，即可实现ArkTS调用Android日志系统，打印来自ArkUI侧的数据。

## 模块实现步骤

ArkTS调用Android Java API，本质上是要实现ArkTS调用Java的能力。跨平台推荐ArkTS -> C/C++ -> Java的调用路径，实现ArkTS调用Android Java API。即在Android侧实现Java接口，再通过JNI机制注册Java模块；通过[N-API](../quick-start/ffi-napi-introduction.md#n-api)机制实现C/C++模块注册，供应用侧ArkTS调用。

### 1、实现Java log方法

```Java
// android\app\src\main\java\com\example\testplugin\TestPlugin.java

package com.example.testplugin;

import android.content.Context;
import android.util.Log;

public class TestPlugin {
    private static final String LOG_TAG = "TestPlugin";

    // 插件构造函数，供插件注册模块调用
    public TestPlugin(Context context) {
        // 调用注册插件的初始化方法
        nativeInit();
    }

    // 实现log模块
    public void log(String log) {
        Log.i(LOG_TAG, log);
    }

    // 注册插件的初始化方法，供插件构造函数调用
    protected native void nativeInit();
}
```

### 2、通过JNI调用Java log方法

JNI（Java Native Interface）允许Java代码和C/C++代码交互，通过在C/C++侧注册Java模块，实现C/C++调用Java。

> **说明：**
>
> Ace Tools或IDE的Native模板中，SDK arkui-x\engine\lib\include路径下的文件会放在android\app\src\main\cpp\include目录中。

```C++
// android\app\src\main\cpp\test_plugin_jni.cpp

#include "test_plugin_jni.h"

#include <jni.h>

// android\app\src\main\cpp\include\plugin_utils.h定义了插件注册常用的接口
#include "plugin_utils.h"

namespace {
    const char TEST_PLUGIN_CLASS_NAME[] = "com/example/testplugin/TestPlugin";

    static const JNINativeMethod METHODS[] = {
            { "nativeInit", "()V", reinterpret_cast<void*>(TestPluginJni::NativeInit) },
    };

    const char METHOD_LOG[] = "log";
    const char SIGNATURE_LOG[] = "(Ljava/lang/String;)V";

    struct {
        jmethodID log;
        jobject globalRef;
    } g_pluginClass;

}

bool TestPluginJni::Register(void* env)
{
    auto* jniEnv = static_cast<JNIEnv*>(env);
    jclass cls = jniEnv->FindClass(TEST_PLUGIN_CLASS_NAME);

    // 注册nativeInit函数
    bool ret = jniEnv->RegisterNatives(cls, METHODS, sizeof(METHODS) / sizeof(METHODS[0])) == 0;
    jniEnv->DeleteLocalRef(cls);
    if (!ret) {
        return false;
    }
    return true;
}

// Called by Java
void TestPluginJni::NativeInit(JNIEnv* env, jobject jobj)
{
    g_pluginClass.globalRef = env->NewGlobalRef(jobj);
    jclass cls = env->GetObjectClass(jobj);

    // 获取log方法的Method ID
    g_pluginClass.log = env->GetMethodID(cls, METHOD_LOG, SIGNATURE_LOG);
    env->DeleteLocalRef(cls);
}

// Called by C++
void TestPluginJni::Log(std::string log)
{
    auto env = ARKUI_X_Plugin_GetJniEnv();
    jstring jLog = env->NewStringUTF(log.c_str());

    // 通过JNI调用Java log方法
    env->CallVoidMethod(g_pluginClass.globalRef, g_pluginClass.log, jLog);

    if (jLog != nullptr) {
        env->DeleteLocalRef(jLog);
    }

    if (env->ExceptionCheck()) {
        env->ExceptionDescribe();
        env->ExceptionClear();
    }
}
```

```C++
// android\app\src\main\cpp\test_plugin_jni.h

#ifndef ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_JNI_H
#define ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_JNI_H

#include <jni.h>
#include <memory>
#include <string>

class TestPluginJni {
public:
    TestPluginJni() = delete;
    ~TestPluginJni() = delete;
    static bool Register(void* env);
    // Called by Java
    static void NativeInit(JNIEnv* env, jobject jobj);
    // Called by C++
    static void Log(std::string log);
};

#endif
```

对JNI方法进行封装。

```C++
// android\app\src\main\cpp\test_plugin_impl.cpp

#include "test_plugin_impl.h"

#include <memory>

#include "plugin_utils.h"
#include "test_plugin_jni.h"

std::unique_ptr<TestPlugin> TestPlugin::Create()
{
    return std::make_unique<TestPluginImpl>();
}

void TestPluginImpl::Log(std::string log)
{
    TestPluginJni::Log(log);
}
```

```C++
// android\app\src\main\cpp\test_plugin_impl.h

#ifndef ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_IMPL_H
#define ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_IMPL_H

#include <memory>

#include "test_plugin.h"

class TestPluginImpl final : public TestPlugin {
public:
    TestPluginImpl() = default;
    ~TestPluginImpl() override = default;

    void Log(std::string log) override;
};

#endif
```

### 3、通过插件机制注册Java方法，通过N-API机制注册testplugin模块，提供ArkTS log方法

实现testplugin.log对应的C/C++模块。

```C++
// android\app\src\main\cpp\js_test_plugin.cpp

#include <cstddef>

#include "napi/native_api.h"
#include "node_api.h"
#include "plugin_utils.h"

#include "test_plugin.h"

#ifdef ANDROID_PLATFORM
#include "test_plugin_jni.h"
#endif

#define NAPI_CALL_BASE(env, theCall, retVal) \
    do {                                     \
        if ((theCall) != napi_ok) {          \
            return retVal;                   \
        }                                    \
    } while (0)

#define NAPI_CALL(env, theCall) NAPI_CALL_BASE(env, theCall, nullptr)

static napi_value JSTestPluginLog(napi_env env, napi_callback_info info)
{
    // 创建TestPlugin实例
    auto plugin = TestPlugin::Create();
    if (!plugin) {
        return nullptr;
    }

    size_t argc = 1;
    napi_value args[1];
    NAPI_CALL(env, napi_get_cb_info(env, info, &argc, args, NULL, NULL));
    char buffer[128];
    size_t buffer_size = 128;
    size_t copied;
    NAPI_CALL(env, napi_get_value_string_utf8(env, args[0], buffer, buffer_size, &copied));

    std::string log;
    log = buffer;

    // 调用C++接口，Log函数最终会调用TestPluginJni::Log
    plugin->Log(log);
    return nullptr;
}
```

```C++
// android\app\src\main\cpp\test_plugin.h

#ifndef ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_H
#define ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_H

#include <memory>
#include <string>

class TestPlugin {
public:
    TestPlugin() = default;
    virtual ~TestPlugin() = default;

    static std::unique_ptr<TestPlugin> Create();

    virtual void Log(std::string log) = 0;
};

#endif
```

实现模块导出入口函数。

```C++
// android\app\src\main\cpp\js_test_plugin.cpp

#define DECLARE_NAPI_FUNCTION(name, func)                                         \
    {                                                                             \
        (name), nullptr, (func), nullptr, nullptr, nullptr, napi_default, nullptr \
    }

static napi_value TestPluginExport(napi_env env, napi_value exports)
{
    static napi_property_descriptor desc[] = {
        DECLARE_NAPI_FUNCTION("log", JSTestPluginLog),
    };
    NAPI_CALL(env, napi_define_properties(env, exports, sizeof(desc) / sizeof(desc[0]), desc));
    return exports;
}
```

实现插件JNI注册函数。

```C++
// android\app\src\main\cpp\js_test_plugin.cpp

// ANDROID_PLATFORM是针对Android平台特有的宏
#ifdef ANDROID_PLATFORM
static void TestPluginJniRegister()
{
    const char className[] = "com.example.testplugin.TestPlugin";

    // ARKUI_X_Plugin_RegisterPlugin是框架提供的接口，实现插件JNI环境的注册
    ARKUI_X_Plugin_RegisterJavaPlugin(&TestPluginJni::Register, className);
}
#endif
```

注册模块。

```C++
// android\app\src\main\cpp\js_test_plugin.cpp

static napi_module testPluginModule = {
        .nm_version = 1,
        .nm_flags = 0,
        .nm_filename = nullptr,
        .nm_register_func = TestPluginExport,
        .nm_modname = "testplugin",
        .nm_priv = ((void*)0),
        .reserved = { 0 },
};

extern "C" __attribute__((constructor)) void TestPluginRegister()
{
    // 注册testPlugin模块，供ArkTS调用
    napi_module_register(&testPluginModule);
#ifdef ANDROID_PLATFORM
    // JNI插件注册函数需要在Platform线程运行
    ARKUI_X_Plugin_RunAsyncTask(&TestPluginJniRegister, ARKUI_X_PLUGIN_PLATFORM_THREAD);
#endif
}
```

### 附：编译配置参考

#### 1、CMakeLists

```C++
// android\app\src\main\cpp\CMakeLists.txt

cmake_minimum_required(VERSION 3.18.1)

project("testplugin")

// 增加ANDROID_PLATFORM宏
add_definitions(-DANDROID_PLATFORM)

// Native模板默认配置
set(TESTPLUGIN_ROOT_PATH ${CMAKE_CURRENT_SOURCE_DIR})

// Native模板默认配置
include_directories(
        ${TESTPLUGIN_ROOT_PATH}
        ${TESTPLUGIN_ROOT_PATH}/include
)

// 构建libtestplugin.so
add_library(testplugin SHARED test_plugin_impl.cpp test_plugin_jni.cpp js_test_plugin.cpp)

add_library(arkui_android SHARED IMPORTED GLOBAL)
set_target_properties(
        arkui_android
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/../../../libs/${CMAKE_ANDROID_ARCH_ABI}/libarkui_android.so
)

target_link_libraries(testplugin PUBLIC arkui_android libc++.a)
```

#### 2、build.gradle

```C++
// android\app\build.gradle

android {
    defaultConfig {
        ......

        ndk {
            abiFilters "arm64-v8a"
        }
        externalNativeBuild {
            cmake {
                cppFlags ''
            }
        }
    }

    packagingOptions {
        pickFirst 'lib/arm64-v8a/libarkui_android.so'
    }

    externalNativeBuild {
        cmake {
            path file('src/main/cpp/CMakeLists.txt')
            version '3.18.1'
        }
    }
}
```
