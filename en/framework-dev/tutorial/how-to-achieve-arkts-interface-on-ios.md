# How to Extend @ohos APIs on iOS

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
| hello | void | void | Prints the log "hello from ios" on iOS.|

```typescript
// xxx.ets

testplugin.hello();
```

## Procedure

The essence of calling iOS Objective-C (OC) APIs is to enable JS to call OC APIs. The recommended path for API calling is JS -> C/C++ -> OC. Specifically, implement OC APIs on iOS and directly encapsulate them through C/C++, and then register C/C++ modules through Native API (NAPI) for JS API calling on the app side.

### Implementing the hello Method on iOS

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

### Calling the OC hello Method in C/C++

```C++
// plugins/test_plugin/ios/test_plugin_impl.mm
void TestPluginImpl::Hello()
{
    PluginUtilsInner::RunTaskOnPlatform([]() {
        // Call the OC hello method.
        [[iOSTestPlugin shareinstance] hello];
    });
}
```

### Exposing the JS hello Method Through NAPI

#### 1. Implement the C/C++ module corresponding to testplugin.hello.

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

#### 3. Register the module.

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

    if (platform == "ios") {
      deps += [ "ios:test_plugin_ios" ]
    }

    subsystem_name = "plugins"
    part_name = "test_plugin"
  }
```

#### 3. Build the iOS-specific code.

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
    "-fobjc-weak",
  ]

  cflags_objcc = cflags_objc

  deps = [ "//plugins/interfaces/native:ace_plugin_util_ios" ]

  subsystem_name = "plugins"
  part_name = "test_plugin"
}
```
