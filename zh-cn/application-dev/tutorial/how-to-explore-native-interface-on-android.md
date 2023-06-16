# Android平台接口扩展指南

本文介绍在Android平台上基于NAPI机制和插件管理机制编写平台接口。

> **说明：**
>
> Android平台上扩展平台接口，以testplugin.hello接口为例。

## ArkUI侧接口使用
```typescript
// xxx.ets

import testplugin from 'libtestplugin.so';
```

## 接口定义
| 名称 | 参数 | 返回类型 | 描述 |
| --- | --- | --- | --- |
| hello | void | void | 在Android侧打印日志"hello from java" |

```typescript
// xxx.ets

testplugin.hello();
```

## 步骤

为了调用Android Java API，本质是要实现JS调用Java的能力。跨平台推荐JS -> C/C++ -> Java的调用路径，实现JS调用Android Java API。即在Android侧实现Java接口，再通过JNI机制注册Java模块；通过NAPI机制实现C/C++模块注册，供应用侧JS调用。

### 1、Android侧实现hello方法

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

    // 实现hello模块
    public void hello() {
        Log.i(LOG_TAG, "TestPlugin: hello from java");
    }

    // 注册插件的初始化方法，供插件构造函数调用
    protected native void nativeInit();
}
```

### 2、通过JNI调用Java hello方法

JNI（Java Native Interface）允许Java代码和C/C++代码交互，通过在C/C++侧注册Java模块，实现C/C++调用Java。

> **说明：**
>
> SDK arkui-x\engine\lib\include路径下的文件需放在android\app\src\main\cpp\include目录中。

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

    const char METHOD_HELLO[] = "hello";
    const char SIGNATURE_HELLO[] = "()V";

    struct {
        jmethodID hello;
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

    // 获取hello方法的Method ID
    g_pluginClass.hello = env->GetMethodID(cls, METHOD_HELLO, SIGNATURE_HELLO);
    env->DeleteLocalRef(cls);
}

// Called by C++
void TestPluginJni::Hello()
{
    auto env = ARKUI_X_Plugin_GetJniEnv();

    // 通过JNI调用Java hello方法
    env->CallVoidMethod(g_pluginClass.globalRef, g_pluginClass.hello);
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

class TestPluginJni {
public:
    TestPluginJni() = delete;
    ~TestPluginJni() = delete;
    static bool Register(void* env);
    // Called by Java
    static void NativeInit(JNIEnv* env, jobject jobj);
    // Called by C++
    static void Hello();
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

void HelloTask()
{
    TestPluginJni::Hello();
}

// ARKUI_X_Plugin_RunAsyncTask是框架提供的接口，将JNI方法抛到Platform线程异步执行
void TestPluginImpl::Hello()
{
    ARKUI_X_Plugin_RunAsyncTask(&HelloTask, ARKUI_X_PLUGIN_PLATFORM_THREAD);
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

    void Hello() override;
};

#endif
```

### 3、通过插件机制注册Java方法，通过NAPI机制暴露JS hello方法

实现testplugin.hello对应的C/C++模块。

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

static napi_value JSTestPluginHello(napi_env env, napi_callback_info info)
{
    // 创建TestPlugin实例
    auto plugin = TestPlugin::Create();
    if (!plugin) {
        return nullptr;
    }
    // 调用C++接口，Hello函数最终会调用TestPluginJni::Hello
    plugin->Hello();
    return nullptr;
}
```

```C++
// android\app\src\main\cpp\test_plugin.h

#ifndef ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_H
#define ANDROID_APP_SRC_MAIN_CPP_TEST_PLUGIN_H

#include <memory>

class TestPlugin {
public:
    TestPlugin() = default;
    virtual ~TestPlugin() = default;

    static std::unique_ptr<TestPlugin> Create();

    virtual void Hello() = 0;
};

#endif
```

实现模块导出入口函数。

```C++
// android\app\src\main\cpp\js_test_plugin.cpp

#define NAPI_CALL_BASE(env, theCall, retVal) \
    do {                                     \
        if ((theCall) != napi_ok) {          \
            return retVal;                   \
        }                                    \
    } while (0)

#define NAPI_CALL(env, theCall) NAPI_CALL_BASE(env, theCall, nullptr)

#define DECLARE_NAPI_FUNCTION(name, func)                                         \
    {                                                                             \
        (name), nullptr, (func), nullptr, nullptr, nullptr, napi_default, nullptr \
    }

static napi_value TestPluginExport(napi_env env, napi_value exports)
{
    static napi_property_descriptor desc[] = {
        DECLARE_NAPI_FUNCTION("hello", JSTestPluginHello),
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
    // 注册testPlugin模块，供JS调用
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

set(TESTPLUGIN_ROOT_PATH ${CMAKE_CURRENT_SOURCE_DIR})

include_directories(
        ${TESTPLUGIN_ROOT_PATH}
        ${TESTPLUGIN_ROOT_PATH}/include
)

// 构建libtestplugin.so
add_library(testplugin SHARED test_plugin_impl.cpp test_plugin_jni.cpp js_test_plugin.cpp )

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
