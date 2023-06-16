# iOS平台扩展@ohos接口

> **说明：**
>
> iOS平台上扩展@ohos接口，以testplugin.hello接口为例（假定@ohos.testplugin是OpenHarmony跨平台API）。

## 导入模块
```typescript
// xxx.ets

import testplugin from '@ohos.testplugin';
```

## 接口
| 名称 | 参数 | 返回类型 | 描述 |
| --- | --- | --- | --- |
| hello | void | void | 在iOS侧打印日志"hello from ios" |

```typescript
// xxx.ets

testplugin.hello();
```

## 步骤
为了调用iOS Objective-C（OC） API，本质是要实现JS调用OC的能力。跨平台推荐JS -> C/C++ -> OC的调用路径，实现JS调用iOS OC API。即在iOS侧实现OC接口，通过C/C++直接封装OC接口，再通过NAPI机制实现C/C++模块注册，供应用侧JS调用。

### 一、iOS侧实现hello方法

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

// 实现hello模块
-(void)hello{
    NSLog(@"TestPlugin: Hello from ios");
}

@end
```

### 二、C/C++调用OC hello方法

```C++
// plugins/test_plugin/ios/test_plugin_impl.mm
void TestPluginImpl::Hello()
{
    PluginUtilsInner::RunTaskOnPlatform([]() {
        // 直接调用OC hello方法
        [[iOSTestPlugin shareinstance] hello];
    });
}
```

### 三、通过NAPI机制暴露JS hello方法

#### 1、实现testplugin.hello对应的C/C++模块。

```C++
// plugins/test_plugin/js_test_plugin.cpp
static napi_value JSTestPluginHello(napi_env env, napi_callback_info info)
{
    // 创建TestPlugin实例
    auto plugin = TestPlugin::Create();

    // 调用C++接口，Hello函数最终会调用iOSTestPlugin的hello方法
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

#### 3、注册模块

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

    if (platform == "ios") {
      deps += [ "ios:test_plugin_ios" ]
    }

    subsystem_name = "plugins"
    part_name = "test_plugin"
  }
```

#### 3、编译iOS平台特有代码

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
