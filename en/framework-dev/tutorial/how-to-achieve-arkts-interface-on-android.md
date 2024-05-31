# How to Extend @ohos APIs on Android

> **NOTE**
>
> This tutorial uses **testplugin.hello** as an example based on the assumption that **@ohos.testplugin** is an OpenHarmony cross-platform API.

## Modules to Import
```typescript
// xxx.ets

import testplugin from '@ohos.testplugin';
```

## API
| Name| Parameter| Return Type| Description|
| --- | --- | --- | --- |
| hello | void | void | Prints the log "hello from java" on Android.|

```typescript
// xxx.ets

testplugin.hello();
```

## Procedure
The essence of calling Android Java APIs is to enable JS to call Java APIs. The recommended path for API calling is JS -> C/C++ -> Java. Specifically, implement Java APIs on Android and register them as Java modules through Java Native Interface (JNI), and then register C/C++ modules through Native API (NAPI) for JS API calling on the app side.

### Implementing the hello Method on Android

```Java
// plugins/test_plugin/android/java/src/TestPlugin.java

package ohos.ace.plugin.testplugin;

import android.content.Context;
import android.util.Log;

public class TestPlugin {
    private static final String LOG_TAG = "TestPlugin";

    // Plug-in constructor, which is called by the plug-in registration module.
    public TestPlugin(Context context) {
        // Call the plug-in initialization method.
        nativeInit();
    }

    // Implement the hello module.
    public void hello() {
        Log.i(LOG_TAG, "TestPlugin: hello from java");
    }

    // Register the plug-in initialization method for the plug-in constructor to call.
    protected native void nativeInit();
}
```

### Calling the Java hello Method Through JNI

#### 1. Register a Java module on C/C++.<br>This operation enables C/C++ to call Java because JNI allows interaction between Java code and C/C++ code.

```C++
// plugins/test_plugin/android/java/jni/test_plugin_jni.cpp

static const JNINativeMethod METHODS[] = {
    { "nativeInit", "()V", reinterpret_cast<void*>(TestPluginJni::NativeInit) },
};

struct {
    jmethodID hello;
    jobject globalRef;
} g_pluginClass;

bool TestPluginJni::Register(void* env) {
    auto* jniEnv = static_cast<JNIEnv*>(env);
    jclass cls = jniEnv->FindClass("ohos/ace/plugin/testplugin/TestPlugin");

    // Register the nativeInit function.
    return jniEnv->RegisterNatives(cls, METHODS, sizeof(METHODS) / sizeof(METHODS[0]));
}

// Called by Java
void TestPluginJni::NativeInit(JNIEnv* env, jobject jobj) {
    jclass cls = env->GetObjectClass(jobj);

    // Obtain the method ID of the hello method.
    g_pluginClass.hello = env->GetMethodID(cls, "hello", "()V");
}

// Called by C++
void TestPluginJni::Hello()
{
    auto env = OH_Plugin_GetJniEnv();

    // Call the Java hello method through JNI.
    env->CallVoidMethod(g_pluginClass.globalRef, g_pluginClass.hello);
}
```

#### 2. Encapsulate the JNI method.

```C++
// plugins/test_plugin/android/java/jni/test_plugin_impl.cpp

// Define the common APIs for plug-in registration.
#include "plugin_utils.h"

// RunTaskOnPlatform provided by the framework throws the JNI method to the Platform thread for asynchronous execution.
void TestPluginImpl::Hello()
{
    PluginUtilsInner::RunTaskOnPlatform([]() { TestPluginJni::Hello(); });
}
```

### Registering the Java Method Through the Plug-in and Exposing the JS hello Method Through NAPI

#### 1. Implement the C/C++ module corresponding to testplugin.hello.

```C++
// plugins/test_plugin/js_test_plugin.cpp
static napi_value JSTestPluginHello(napi_env env, napi_callback_info info)
{
    // Create a TestPlugin instance.
    auto plugin = TestPlugin::Create();

    // Call the C++ API. The Hello function finally calls TestPluginJni::Hello.
    plugin->Hello();
    return nullptr;
}

// plugins/test_plugin/test_plugin.h
class TestPlugin {
public:
    TestPlugin() = default;
    virtual ~TestPlugin() = default;

    static std::unique_ptr<TestPlugin> Create();

    virtual void Hello() = 0;
};

// plugins/test_plugin/android/java/jni/test_plugin_impl.cpp
std::unique_ptr<TestPlugin> TestPlugin::Create()
{
    return std::make_unique<TestPluginImpl>();
}
```
#### 2. Implement the module export entry function.

```C++
// plugins/test_plugin/js_test_plugin.cpp
static napi_value TestPluginExport(napi_env env, napi_value exports)
{
    static napi_property_descriptor desc[] = {
        DECLARE_NAPI_FUNCTION("hello", JSTestPluginHello),
    };
    NAPI_CALL(env, napi_define_properties(env, exports, sizeof(desc) / sizeof(desc[0]), desc));
    return exports;
}
```

#### 3. Implement the plug-in JNI registration function.

```C++
// plugins/test_plugin/js_test_plugin.cpp

// ANDROID_PLATFORM is a macro specific to Android.
#ifdef ANDROID_PLATFORM
static void TestPluginJniRegister()
{
    // OH_Plugin_RegisterPlugin provided by the framework registers the plug-in JNI environment.
    OH_Plugin_RegisterJavaPlugin(&TestPluginJni::Register, "ohos.ace.plugin.testplugin.TestPlugin");
}
#endif
```

#### 4. Register the module.

```C++
// plugins/test_plugin/js_test_plugin.cpp

static napi_module testPluginModule = {
    .nm_version = 1,
    .nm_flags = 0,
    .nm_filename = nullptr,
    .nm_register_func = TestPluginExport,
    .nm_modname = "testPlugin",
    .nm_priv = ((void*)0),
    .reserved = { 0 },
};

extern "C" __attribute__((constructor)) void TestPluginRegister()
{
    // Register the testPlugin module for JS to call.
    napi_module_register(&testPluginModule);
#ifdef ANDROID_PLATFORM
    // The JNI plug-in registration function runs in the Platform thread.
    OH_Plugin_RunAsyncTask(&TestPluginJniRegister, OH_PLUGIN_PLATFORM_THREAD);
#endif
}
```

### Appendix: GN Configuration

#### 1. Add a module.

```C++
// plugins/plugin_lib.gni
common_plugin_libs = [
  "test_plugin",
]
```

#### 2. Build the js_test_plugin module.

```C++
// plugins/test_plugin/BUILD.gn
  ohos_source_set(target_name) {
    defines += invoker.defines
    cflags_cc += invoker.cflags_cc

    sources = [ "js_test_plugin.cpp" ]

    deps = [
      "//plugins/interfaces/native:ace_plugin_util_${platform}",
      "//plugins/libs/napi:napi_${target_os}",
    ]

    if (platform == "android") {
      deps += [ "android/java:test_plugin_android_jni" ]
    }

    subsystem_name = "plugins"
    part_name = "test_plugin"
  }
```

#### 3. Build the Android-specific code.

```C++
// plugins/test_plugin/android/java/BUILD.gn

import("//build/ohos.gni")

java_library("test_plugin_android_java") {
  java_files = [ "src/TestPlugin.java" ]
  subsystem_name = "plugins"
  part_name = "test_plugin"
}

// Compile the JAR package.
ohos_combine_jars("test_plugin_java") {
  deps = [ ":test_plugin_android_java" ]

  subsystem_name = "plugins"
  part_name = "test_plugin"
  jar_path = "${root_out_dir}/plugins/test_plugin/ace_test_plugin_android.jar"
}

ohos_source_set("test_plugin_android_jni") {
  sources = [
    "jni/test_plugin_impl.cpp",
    "jni/test_plugin_jni.cpp",
  ]

  defines = [ "ANDROID_PLATFORM" ]

  deps = [
    ":test_plugin_java",
    "//plugins/interfaces/native:ace_plugin_util_android",
  ]

  subsystem_name = "plugins"
  part_name = "test_plugin"
}
```
