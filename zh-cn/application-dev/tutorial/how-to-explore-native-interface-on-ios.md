# iOS平台接口扩展指南

本文介绍在iOS平台上基于NAPI机制和插件管理机制编写平台接口。

> **说明：**
>
> iOS平台上扩展平台接口，以testplugin.hello接口为例。

## ArkUI侧接口使用
```typescript
// xxx.ets

import testplugin from 'libtestplugin.so';
```

## 接口定义
| 名称 | 参数 | 返回类型 | 描述 |
| --- | --- | --- | --- |
| hello | void | void | 在iOS侧打印日志"hello from ios" |

```typescript
// ets
testplugin.hello();
```

## 步骤
为了调用iOS Objective-C（OC）API，本质是要实现JS调用OC的能力。跨平台推荐JS -> C/C++ -> OC的调用路径，实现JS调用iOS OC API。即在iOS侧实现OC接口，通过C/C++直接封装OC接口，再通过NAPI机制实现C/C++模块注册，供应用侧JS调用。

### 一、iOS侧实现hello方法

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

// 实现hello模块
-(void)hello{
    NSLog(@"TestPlugin: hello from ios");
}

@end
```

```Objective-C
#import <Foundation/Foundation.h>

NS_ASSUME_NONNULL_BEGIN

@interface iOSTestPlugin : NSObject

+(instancetype)shareinstance;
-(void)hello;

@end

NS_ASSUME_NONNULL_END
```

### 二、C/C++调用OC hello方法

> **说明：**
>
> 确保SDK libarkui_ios中的头文件能被索引。

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

void TestPluginImpl::Hello()
{
    ARKUI_X_Plugin_RunAsyncTask([]() {
        // 直接调用OC hello方法
        [[iOSTestPlugin shareinstance] hello];
    }, ARKUI_X_PLUGIN_PLATFORM_THREAD);
}
```

```C++
// ios\app\test_plugin_impl.h

#ifndef IOS_APP_TEST_PLUGIN_IMPL_H
#define IOS_APP_TEST_PLUGIN_IMPL_H

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

### 三、通过NAPI机制暴露JS hello方法

#### 1、实现testplugin.hello对应的C/C++模块。

```C++
// ios\app\js_test_plugin.cpp

#include <cstddef>

// libarkui_ios的include中定义了NAPI的接口
#include <libarkui_ios/include/napi/native_api.h>
#include <libarkui_ios/include/node_api.h>

#include <libarkui_ios/include/plugin_utils.h>

#include "test_plugin.h"

static napi_value JSTestPluginHello(napi_env env, napi_callback_info info)
{
    // 创建TestPlugin实例
    auto plugin = TestPlugin::Create();
    if (!plugin) {
        return nullptr;
    }

    // 调用C++接口，Hello函数最终会调用iOSTestPlugin的hello方法
    plugin->Hello();
    return nullptr;
}
```

```C++
// ios\app\test_plugin.h

#ifndef IOS_APP_TEST_PLUGIN_H
#define IOS_APP_TEST_PLUGIN_H

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

#### 2、实现模块导出入口函数

```C++
// ios\app\js_test_plugin.cpp

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

#### 3、注册模块

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
    // 注册testPlugin模块，供JS调用
    napi_module_register(&testPluginModule);
}
```
