# Extending Native APIs on iOS

This tutorial describes how to use [Node-API](../quick-start/ffi-napi-introduction.md) to call iOS APIs using ArkUI.

## Interface Definition

Implement the **log** method of the **testplugin** module to call the iOS log system to print ArkUI data.

| Name| Type| Return Type| Description|
| --- | --- | --- | --- |
| log | string | void | Print data from ArkUI on iOS.|

## How to Develop

1. Use [ACE Tools](../quick-start/start-overview.md) or IDE to create a native project.

```
// Use ACE Tools to create a native project.
ace create demo -t plugin_napi
? Please enter the project name(demo)
? Please enter the bundleName (com.example.demo):
? Please enter the runtimeOS (1: OpenHarmony, 2: HarmonyOS): 1

Project created successfully! Target directory: ${current_directory}/demo.

In order to run your application, type:
    
    $ cd demo
    $ ace run
      
Your application code is in demo/entry.
```

2. Import the native .so file to ArkUI. For example, in the **testplugin\source\entry\src\main\ets\pages\Index.ets** file, **import testplugin from 'libtestplugin.so'** means to use the **libtestplugin.so** capability in ArkUI. Then, provide the ArkUI object named **testplugin** to the application. You can use this object to develop Objective-C (OC) plug-ins based on the plug-in management mechanism and finally call the OC **log** method developed in the **testplugin\ios\app\ios_test_plugin.m**.

```typescript
// Index.ets

import testplugin from 'libtestplugin.so';

testplugin.log("log from ArkUI to iOS");
```

3. Implement the **log** method of the **testplugin** module. After that, you can call the **log** method on ArkTS to invoke iOS log system and print data from ArkUI.

## Implementing the Module

The essence of calling iOS OC APIs is to enable ArkTS to call OC APIs. The recommended path for API calling is ArkTS -> C/C++ -> OC. Specifically, implement the OC API on iOS, encapsulate the OC API in C/C++, and then register the C/C++ module using the [Node-API](../quick-start/ffi-napi-introduction.md) mechanism for the ArkTS to call.

### 1. Implementing the OC log Method

```Objective-C
// ios\app\ios_test_plugin.m

#import "ios_test_plugin.h"

@implementation iOSTestPlugin

+ (instancetype)shareinstance{
    static dispatch_once_t onceToken;
    static iOSTestPlugin *instance = nil;
    dispatch_once(&onceToken, ^{
        instance = [iOSTestPlugin new];
    });
    return instance;
}

// Implement the log module.
-(void)log: (NSString*) log{
    NSLog(@"%@", log);
}

@end
```

```Objective-C
// ios\app\ios_test_plugin.h

#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface iOSTestPlugin : NSObject

+(instancetype)shareinstance;
-(void)log: (NSString*) log;

@end

NS_ASSUME_NONNULL_END
```

### 2. Implementing the OC log Method in C/C++

> **NOTE**
>
> In the native template of ACE Tools or IDE, ensure that the header file in SDK libarkui_ios is included.

```C++
// ios\app\test_plugin_impl.mm

#include "test_plugin_impl.h"

#include <memory>

// plugin_utils.h in libarkui_ios defines common interfaces for plug-in registration.
#include <libarkui_ios/include/plugin_utils.h>

#import "ios_test_plugin.h"

std::unique_ptr<TestPlugin> TestPlugin::Create()
{
    return std::make_unique<TestPluginImpl>();
}

void TestPluginImpl::log(std::string log)
{
    NSString* ocLog = [NSString stringWithCString:log.c_str() encoding:NSUTF8StringEncoding];
    [[iOSTestPlugin shareinstance] log: ocLog];
}
```

```C++
// ios\app\test_plugin_impl.h

#ifndef IOS_APP_TEST_PLUGIN_IMPL_H
#define IOS_APP_TEST_PLUGIN_IMPL_H

#include <memory>
#include <string>

#include "test_plugin.h"

class TestPluginImpl final : public TestPlugin {
public:
    TestPluginImpl() = default;
    ~TestPluginImpl() override = default;

    void Log(std::string log) override;
};

#endif
```

### 3. Registering the testplugin module 

Register the **testplugin** module using Node-API to provide the ArkTS log method.

Implement the C/C++ module corresponding to **testplugin.log**.

```C++
// ios\app\js_test_plugin.cpp

#include <cstddef>

// The include in libarkui_ios defines the Node-API interfaces.
#include <libarkui_ios/include/napi/native_api.h>
#include <libarkui_ios/include/node_api.h>

#include <libarkui_ios/include/plugin_utils.h>

#include "test_plugin.h"

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

    // Invoke the C++ interface. The Log function finally calls the log method of iOSTestPlugin.
    plugin->Log(log);
    return nullptr;
}
```

```C++
// ios\app\test_plugin.h

#ifndef IOS_APP_TEST_PLUGIN_H
#define IOS_APP_TEST_PLUGIN_H

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
// ios\app\js_test_plugin.cpp

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

Register the module.

```C++
// ios\app\js_test_plugin.cpp

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
}
```
