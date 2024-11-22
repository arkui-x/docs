# iOS平台接口扩展指南

本文介绍在iOS平台上基于[N-API](../quick-start/ffi-napi-introduction.md)机制和插件管理机制，实现ArkUI侧调用iOS侧方法。

## 接口定义

本文实现如下testplugin模块的log方法，功能为调用iOS日志系统，打印来自ArkUI侧的数据。

| 名称 | 参数类型 | 返回类型 | 描述 |
| --- | --- | --- | --- |
| log | string | void | 在iOS侧打印来自ArkUI侧的数据。|

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

2、通过Ace Tools或IDE创建Native模板后，ArkUI侧通过`import`引入Native侧的so文件，在创建出的`testplugin\source\entry\src\main\ets\pages\Index.ets`中，`import testplugin from 'libtestplugin.so'`，意为使用libtestplugin.so的能力，并将名为`testplugin`的ArkUI侧对象给到应用侧，开发者可通过该对象，基于插件管理机制开发Objective-C插件，最终调用到在`testplugin\ios\app\ios_test_plugin.m`中开发的Objective-C `log`方法。

```typescript
// Index.ets

import testplugin from 'libtestplugin.so';

testplugin.log("log from ArkUI to iOS");
```

3、按如下步骤实现testplugin模块的log方法，即可实现ArkTS调用iOS日志系统，打印来自ArkUI侧的数据。

## 模块实现步骤

ArkTS调用iOS Objective-C（OC）API，本质上是要实现ArkTS调用OC的能力。跨平台推荐ArkTS -> C/C++ -> OC的调用路径，实现ArkTS调用iOS OC API。即在iOS侧实现OC接口，通过C/C++直接封装OC接口，再通过[N-API](../quick-start/ffi-napi-introduction.md#n-api)机制实现C/C++模块注册，供应用侧ArkTS调用。

### 1、实现Objective-C log方法

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

// 实现log模块
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

### 2、实现C/C++调用OC log方法

> **说明：**
>
> Ace Tools或IDE的Native模板中，SDK libarkui_ios中的头文件能被索引到。

```C++
// ios\app\test_plugin_impl.mm

#include "test_plugin_impl.h"

#include <memory>

// libarkui_ios中的plugin_utils.h定义了插件注册常用的接口
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

### 3、通过N-API机制注册testplugin模块，提供ArkTS log方法

实现testplugin.log对应的C/C++模块。

```C++
// ios\app\js_test_plugin.cpp

#include <cstddef>

// libarkui_ios的include中定义了NAPI的接口
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

    // 调用C++接口，Log函数最终会调用iOSTestPlugin的log方法
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

实现模块导出入口函数。

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

注册模块。

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
    // 注册testPlugin模块，供ArkTS调用
    napi_module_register(&testPluginModule);
}
```
