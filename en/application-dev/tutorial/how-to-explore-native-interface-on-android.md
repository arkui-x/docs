# How to Extend Native APIs on Android

This tutorial describes how to call Android methods from ArkUI based on the [N-API](../quick-start/ffi-napi-introduction.md) mechanism and plug-in management mechanism on Android.

## Interface Definition

Implement the **log** method of the **testplugin** module to call the Android log system and print data from ArkUI.

| Name| Type| Return Type| Description|
| --- | --- | --- | --- |
| log | string | void | Print data from ArkUI on Android.|

## How to Develop

1. Use [ACE Tools](../quick-start/start-overview.md) or IDE to create a Native project.

```
// Use ACE Tools to create a Native project.
ace create project
? Please enter the project name: testplugin
? Please enter the bundle name (com.example.testplugin):
? Please enter the system (1: OpenHarmony, 2: HarmonyOS): 1
? Please enter the template (1: Empty Ability, 2: Native C++): 2
```

2. Import the Native .so file to ArkUI. For example, in the **testplugin\entry\src\main\ets\pages\Index.ets** file, **import testplugin from 'libtestplugin.so'** means to use the **libtestplugin.so** capability in ArkUI. Then, provide the ArkUI object named **testplugin** to the application. You can use this object to develop Java plug-ins based on the plug-in management mechanism and finally call the Java **log** method developed in the **`testplugin\.arkui-x\android\app\src\main\java\com\example\testplugin\TestPlugin.java**.

```typescript
// Index.ets

import testplugin from 'libtestplugin.so';

testplugin.log("log from ArkUI to Android");
```

3. Implement the **log** method of the **testplugin** module. After that, you can call the **log** method on ArkTS to invoke Android log system and print data from ArkUI.

## Implementing the Module

The essence of calling Android Java APIs is to enable ArkTS to call Java APIs. The recommended path for API calling is ArkTS -> C/C++ -> Java. Specifically, implement the Java method on Android, register the Java module using the Java Native Interface (JNI) mechanism, and then register the C/C++ module using the [N-API](../quick-start/ffi-napi-introduction.md#n-api) mechanism for the ArkTS to call.

### 1. Implementing the Java log Method

```Java
// android\app\src\main\java\com\example\testplugin\TestPlugin.java

package com.example.testplugin;

import android.content.Context;
import android.util.Log;

public class TestPlugin {
    private static final String LOG_TAG = "TestPlugin";

    // Plugin constructor, which is called by the plugin registration module.
    public TestPlugin(Context context) {
        // Call the initialization method of the plugin.
        nativeInit();
    }

    // Implement the log module.
    public void log(String log) {
        Log.i(LOG_TAG, log);
    }

    // Register the initialization method of the plugin for the plugin constructor to call.
    protected native void nativeInit();
}
```

### 2. Invoking the Java log Method Using JNI

1. Register a Java module on C/C++.<br>This operation enables C/C++ to call Java because JNI allows interaction between Java code and C/C++ code.

> **NOTE**
>
> In the Native template of ACE Tools or IDE, files in the SDK **arkui-x\engine\lib\include** directory are stored in the **android\app\src\main\cpp\include** directory.

```C++
// android\app\src\main\cpp\test_plugin_jni.cpp

#include "test_plugin_jni.h"

#include <jni.h>

// android\app\src\main\cpp\include\plugin_utils.h defines common APIs for plug-in registration.
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

    // Register the nativeInit function.
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

    // Obtain the method ID of the log method.
    g_pluginClass.log = env->GetMethodID(cls, METHOD_LOG, SIGNATURE_LOG);
    env->DeleteLocalRef(cls);
}

// Called by C++
void TestPluginJni::Log(std::string log)
{
    auto env = ARKUI_X_Plugin_GetJniEnv();
    jstring jLog = env->NewStringUTF(log.c_str());

    // Invoke the Java log method using JNI.
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

Encapsulate the JNI method.

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

### 3. Registering the Java Method and testplugin

Register the Java method using the plug-in mechanism and register the **testplugin** module using the N-API mechanism to provide the ArkTS log method.

Implement the C/C++ module corresponding to **testplugin.log**.

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
    // Create a TestPlugin instance.
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

    // Call the C++ API. The Log function finally calls TestPluginJni::Log.
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

Implement the module export entry function.

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

Implement the plug-in JNI registration function.

```C++
// android\app\src\main\cpp\js_test_plugin.cpp

// ANDROID_PLATFORM is a macro specific to Android.
#ifdef ANDROID_PLATFORM
static void TestPluginJniRegister()
{
    const char className[] = "com.example.testplugin.TestPlugin";

    // ARKUI_X_Plugin_RegisterPlugin provided by the framework registers the plugin JNI environment.
    ARKUI_X_Plugin_RegisterJavaPlugin(&TestPluginJni::Register, className);
}
#endif
```

Register the module.

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
    // Register the testPlugin module for ArkTS to call.
    napi_module_register(&testPluginModule);
#ifdef ANDROID_PLATFORM
    // The JNI plugin registration function runs in the Platform thread.
    ARKUI_X_Plugin_RunAsyncTask(&TestPluginJniRegister, ARKUI_X_PLUGIN_PLATFORM_THREAD);
#endif
}
```

### Appendix: Compilation Configuration Reference

#### 1. CMakeLists

```C++
// android\app\src\main\cpp\CMakeLists.txt

cmake_minimum_required(VERSION 3.18.1)

project("testplugin")

// Add the ANDROID_PLATFORM macro.
add_definitions(-DANDROID_PLATFORM)

// Default configuration of the Native template.
set(TESTPLUGIN_ROOT_PATH ${CMAKE_CURRENT_SOURCE_DIR})

// Default configuration of the Native template.
include_directories(
        ${TESTPLUGIN_ROOT_PATH}
        ${TESTPLUGIN_ROOT_PATH}/include
)

// Build libtestplugin.so.
add_library(testplugin SHARED test_plugin_impl.cpp test_plugin_jni.cpp js_test_plugin.cpp)

add_library(arkui_android SHARED IMPORTED GLOBAL)
set_target_properties(
        arkui_android
        PROPERTIES IMPORTED_LOCATION
        ${CMAKE_CURRENT_SOURCE_DIR}/../../../libs/${CMAKE_ANDROID_ARCH_ABI}/libarkui_android.so
)

target_link_libraries(testplugin PUBLIC arkui_android libc++.a)
```

#### 2. build.gradle

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
