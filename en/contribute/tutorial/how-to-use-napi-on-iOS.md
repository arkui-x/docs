# @ohos API Extension on iOS

## Introduction
This tutorial describes how to extend @ohos JS APIs on iOS. It uses the **testplugin.hello** API as an example.

## Modules to Import
```typescript
// Method 1
import testplugin from 'libtestplugin.so';

// Method 2 (not recommended). Add @ts-ignore for non-OpenHarmony APIs to ignore build errors.
// @ts-ignore
import testplugin from '@ohos.testplugin';
```

**NOTE**

Method 1 is recommended for introducing non-OpenHarmony APIs, and method 2 is recommended for introducing extended OpenHarmony APIs. (If method 2 is used for introducing non-OpenHarmony APIs, build errors will be reported.)

## APIs
| API| Parameter| Return Type| Description|
| --- | --- | --- | --- |
| hello | void | void | Prints the log "hello from ios" on iOS.|

```typescript
// ets
testplugin.hello();
```

## Procedure
The essence of calling iOS Objective-C (OC) APIs is to enable JS to call OC APIs. The invoking path JS -> C/C++ -> OC is recommended. Specifically, the OC APIs are implemented on iOS and directly encapsulated through C/C++, and the C/C++ module is registered through the Native API (NAPI) mechanism for JS invoking on the application side.

### 1. Implementing the hello Method on iOS

```C++
// plugins/test_plugin/ios/ios_test_plugin.m

@implementation iOSTestPlugin

+ (instancetype)shareinstance{
    static dispatch_once_t onceToken;
    static iOSTestPlugin *instance = nil;
    dispatch_once(&onceToken, ^{
        instance = [iOSTestPlugin new];
    });
    return instance;
}

// Implement the hello module.
-(void)hello{
    NSLog(@"TestPlugin: Hello from ios");
}

@end
```

### 2. Calling the OC hello Method in C/C++

```C++
// plugins/test_plugin/ios/test_plugin_impl.mm
void TestPluginImpl::Hello()
{
    PluginUtils::RunTaskOnPlatform([]() {
        // Call the OC hello method.
        [[iOSTestPlugin shareinstance] hello];
    });
}
```

### 3. Exposing the JS hello Method Through the NAPI Mechanism

#### 1. Implementing the C/C++ Module Corresponding to testplugin.hello

```C++
// plugins/test_plugin/js_test_plugin.cpp
static napi_value JSTestPluginHello(napi_env env, napi_callback_info info)
{
    // Create a TestPlugin instance.
    auto plugin = TestPlugin::Create();

    // Call the C++ API. The Hello function finally calls the hello method of iOSTestPlugin.
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

// plugins/test_plugin/ios/test_plugin_impl.mm
std::unique_ptr<TestPlugin> TestPlugin::Create()
{
    return std::make_unique<TestPluginImpl>();
}

```
#### 2. Implementing the Module Export Entry Function

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

#### 3. Registering the Module

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
}
```

### Appendix: GN Configuration

#### 1. Adding a Module

```C++
// plugins/plugin_lib.gni
common_plugin_libs = [
  "test_plugin",
]
```

#### 2. Building the js_test_plugin Module

```C++
// plugins/test_plugin/BUILD.gn
  ohos_source_set(target_name) {
    defines += invoker.defines
    cflags_cc += invoker.cflags_cc

    sources = [ "js_test_plugin.cpp" ]

    deps = [
      "//foundation/arkui/napi:ace_napi",
      "//plugins/interfaces/native:ace_plugin_util_${platform}",
    ]

    if (platform == "ios") {
      deps += [ "ios:test_plugin_ios" ]
    }

    subsystem_name = "plugins"
    part_name = "test_plugin"
  }
```

#### 3. Building iOS-specific Code

```C++
// plugins/test_plugin/ios/BUILD.gn

import("//build/ohos.gni")
import("//foundation/arkui/ace_engine/ace_config.gni")

ohos_source_set("test_plugin_ios") {
  sources = [
    "ios_test_plugin.m",
    "test_plugin_impl.mm",
  ]

  defines = [ "IOS_PLATFORM" ]

  if (target_cpu == "arm64") {
    defines += [ "_ARM64_" ]
  }

  configs = [ "$ace_root:ace_config" ]

  cflags_objc = [
    "-fvisibility=default",
    "-fobjc-arc",
    "-fobjc-weak",
  ]

  cflags_objcc = cflags_objc

  deps = [ "//plugins/interfaces/native:ace_plugin_util_ios" ]

  subsystem_name = "plugins"
  part_name = "test_plugin"
}
```
