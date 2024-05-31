# Android平台扩展@ohos接口

> **说明：**
>
> Android平台上扩展@ohos接口，以testplugin.hello接口为例（假定@ohos.testplugin是OpenHarmony跨平台API）。

## 导入模块
```typescript
// xxx.ets

import testplugin from '@ohos.testplugin';
```

## 接口
| 名称 | 参数 | 返回类型 | 描述 |
| --- | --- | --- | --- |
| hello | void | void | 在Android侧打印日志"hello from java" |

```typescript
// xxx.ets

testplugin.hello();
```

## 步骤
为了调用Android Java API，本质是要实现JS调用Java的能力。跨平台推荐JS -> C/C++ -> Java的调用路径，实现JS调用Android Java API。即在Android侧实现Java接口，再通过JNI机制注册Java模块；通过NAPI机制实现C/C++模块注册，供应用侧JS调用。

### 一、Android侧实现hello方法

```Java
// plugins/test_plugin/android/java/src/TestPlugin.java

package ohos.ace.plugin.testplugin;

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

### 二、通过JNI调用Java hello方法

#### 1、JNI（Java Native Interface）允许Java代码和C/C++代码交互，通过在C/C++侧注册Java模块，实现C/C++调用Java。

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

    // 注册nativeInit函数
    return jniEnv->RegisterNatives(cls, METHODS, sizeof(METHODS) / sizeof(METHODS[0]));
}

// Called by Java
void TestPluginJni::NativeInit(JNIEnv* env, jobject jobj) {
    jclass cls = env->GetObjectClass(jobj);

    // 获取hello方法的Method ID
    g_pluginClass.hello = env->GetMethodID(cls, "hello", "()V");
}

// Called by C++
void TestPluginJni::Hello()
{
    auto env = OH_Plugin_GetJniEnv();

    // 通过JNI调用Java hello方法
    env->CallVoidMethod(g_pluginClass.globalRef, g_pluginClass.hello);
}
```

#### 2、对JNI方法进行封装。

```C++
// plugins/test_plugin/android/java/jni/test_plugin_impl.cpp

// 定义了插件注册常用的接口
#include "plugin_utils.h"

// RunTaskOnPlatform是框架提供的接口，将JNI方法抛到Platform线程异步执行
void TestPluginImpl::Hello()
{
    PluginUtilsInner::RunTaskOnPlatform([]() { TestPluginJni::Hello(); });
}
```

### 三、通过插件机制注册Java方法，通过NAPI机制暴露JS hello方法

#### 1、实现testplugin.hello对应的C/C++模块。

```C++
// plugins/test_plugin/js_test_plugin.cpp
static napi_value JSTestPluginHello(napi_env env, napi_callback_info info)
{
    // 创建TestPlugin实例
    auto plugin = TestPlugin::Create();

    // 调用C++接口，Hello函数最终会调用TestPluginJni::Hello
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
#### 2、实现模块导出入口函数

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

#### 3、实现插件JNI注册函数

```C++
// plugins/test_plugin/js_test_plugin.cpp

// ANDROID_PLATFORM是针对Android平台特有的宏
#ifdef ANDROID_PLATFORM
static void TestPluginJniRegister()
{
    // OH_Plugin_RegisterPlugin是框架提供的接口，实现插件JNI环境的注册
    OH_Plugin_RegisterJavaPlugin(&TestPluginJni::Register, "ohos.ace.plugin.testplugin.TestPlugin");
}
#endif
```

#### 4、注册模块

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
    // 注册testPlugin模块，供JS调用
    napi_module_register(&testPluginModule);
#ifdef ANDROID_PLATFORM
    // JNI插件注册函数需要在Platform线程运行
    OH_Plugin_RunAsyncTask(&TestPluginJniRegister, OH_PLUGIN_PLATFORM_THREAD);
#endif
}
```

### 附：GN配置

#### 1、增加模块

```C++
// plugins/plugin_lib.gni
common_plugin_libs = [
  "test_plugin",
]
```

#### 2、编译js_test_plugin模块

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

#### 3、编译Android平台特有代码

```C++
// plugins/test_plugin/android/java/BUILD.gn

import("//build/ohos.gni")

java_library("test_plugin_android_java") {
  java_files = [ "src/TestPlugin.java" ]
  subsystem_name = "plugins"
  part_name = "test_plugin"
}

// 编译jar包
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
