# N-API在Android平台应用的使用指导

在OpenHarmony中，N-API接口可以实现ArkTS/TS/JS与C/C++之间的交互。N-API提供的接口名与三方Node.js一致，目前支持部分接口，支持列表见[链接](https://gitee.com/openharmony/arkui_napi/blob/master/libnapi.ndk.json)。ArkUI-X在此基础上进行了跨平台扩展，N-API接口可以在OpenHarmony/HarmonyOS/Android/iOS多个平台上使用，本文档以[ACE Tools工具](../quick-start/start-overview.md)创建的native样例工程为例，介绍如何在Android平台上使用N-API接口。

## 开发流程

利用[ACE Tools工具](../quick-start/start-overview.md)创建native工程。
```
ace create project
? Please enter the project name: demo
? Please enter the bundle name (com.example.demo):
? Please enter the system (1: OpenHarmony, 2: HarmonyOS): 1
? Please enter the template (1: Empty Ability, 2: Native C++): 2
? Please enter the Ability Model Type (1: Stage, 2: FA): 1
```

ArkTS/TS/JS侧通过`import`引入native侧的so文件，在创建出的`demo\source\entry\src\main\ets\pages\Index.ets`中，`import entry from 'libentry.so'`，意为使用libentry.so的能力，并将名为`entry`的ArkTS/TS/JS对象给到应用的ArkTS/TS/JS侧，开发者可通过该对象，调用到在`demo\source\entry\src\main\cpp\hello.cpp`中开发的native方法。

## 基本功能
N-API接口可以实现ArkTS/TS/JS和C/C++之间的交互，这里以HelloWorld工程的两个例子来介绍N-API的典型使用场景：
1. 提供一个名为`Add`的native方法，ArkTS侧调用该方法并传入两个number，native方法将这两个number相加并返回到ArkTS侧。以此介绍：ArkTS侧如何调用到C++侧方法。
2. 提供一个名为`NativeCallArkTS`的native方法，ArkTS侧调用该方法并传入一个ArkTS function，native方法中调用这个ArkTS function，并将其结果返回ArkTS侧。以此介绍：C++侧如何调用到ArkTS侧方法。

下面给出了工程中的核心代码：
1. `source\entry\src\main\cpp\hello.cpp`, 包含native侧逻辑。
2. `source\entry\src\main\ets\pages\Index.ets`，包含ArkTS侧逻辑。
3. `source\entry\src\main\cpp\types\libentry\index.d.ts`，包含native侧暴露给ArkTS侧接口的声明。

同时给出了注解，工程中其余部分均与native默认工程相同。

```C++
// source\entry\src\main\cpp\hello.cpp
// 引入N-API相关头文件。
#include "napi/native_api.h"

// 开发者提供的native方法，入参有且仅有如下两个，开发者不需进行变更。
// napi_env 为当前运行的上下文。
// napi_callback_info 记录了一些信息，包括从ArkTS侧传递过来参数等。
static napi_value Add(napi_env env, napi_callback_info info)
{
    // 期望从ArkTS侧获取的参数的数量，napi_value可理解为ArkTS value在native方法中的表现形式。
    size_t argc = 2;
    napi_value args[2] = {nullptr};
    
    // 从info中，拿到从ArkTS侧传递过来的参数，此处获取了两个ArkTS参数，即arg[0]和arg[1]。
    napi_get_cb_info(env, info, &argc, args , nullptr, nullptr);

    // 将获取的ArkTS参数转换为native信息，此处ArkTS侧传入了两个number，这里将其转换为native侧可以操作的double类型。
    double value0;
    napi_get_value_double(env, args[0], &value0);

    double value1;
    napi_get_value_double(env, args[1], &value1);
    
    // native侧的业务逻辑，这里简单以两数相加为例。
    double nativeSum = value0 + value1;
    
    // 此处将native侧业务逻辑处理结果转换为ArkTS值，并返回给ArkTS。
    napi_value sum;
    napi_create_double(env, nativeSum , &sum);
    return sum;
}

static napi_value NativeCallArkTS(napi_env env, napi_callback_info info)
{
    // 期望从ArkTS侧获取的参数的数量，napi_value可理解为ArkTS value在native方法中的表现形式。
    size_t argc = 1;
    napi_value args[1] = {nullptr};
    
    // 从info中，拿到从ArkTS侧传递过来的参数，此处获取了一个ArkTS参数，即arg[0]。
    napi_get_cb_info(env, info, &argc, args , nullptr, nullptr);
    
    // 创建一个ArkTS string作为ArkTS function的入参。
    napi_value argv = nullptr;
    // 创建一个js string
    napi_create_string_utf8(env, "hello", NAPI_AUTO_LENGTH, &argv);
    
    napi_value result = nullptr;
    // native方法中调用ArkTS function，其返回值保存到result中并返到ArkTS侧。
    napi_call_function(env, nullptr, args[0], 1, &argv, &result);
    
    return result;
}

EXTERN_C_START
// Init将在exports上挂上Add/NativeCallArkTS这些native方法，此处的exports就是开发者import之后获取到的ArkTS对象。
static napi_value Init(napi_env env, napi_value exports)
{
    // 函数描述结构体，以Add为例，第三个参数"Add"为上述的native方法，
    // 第一个参数"add"为ArkTS侧对应方法的名称。
    napi_property_descriptor desc[] = {
        { "add", nullptr, Add, nullptr, nullptr, nullptr, napi_default, nullptr },
        { "nativeCallArkTS", nullptr, NativeCallArkTS, nullptr, nullptr, nullptr, napi_default, nullptr },
    };
    // 在exports这个ArkTS对象上，挂载native方法。
    napi_define_properties(env, exports, sizeof(desc) / sizeof(desc[0]), desc);
    return exports;
}
EXTERN_C_END

// 准备模块加载相关信息，将上述Init函数与本模块名等信息记录下来。
static napi_module demoModule = {
    .nm_version =1,
    .nm_flags = 0,
    .nm_filename = nullptr,
    .nm_register_func = Init,
    .nm_modname = "entry",
    .nm_priv = ((void*)0),
    .reserved = { 0 },
};

// 打开so时，该函数将自动被调用，使用上述demoModule模块信息，进行模块注册相关动作。
extern "C" __attribute__((constructor)) void RegisterHelloModule(void)
{
    napi_module_register(&demoModule);
}
```

```js
// source\entry\src\main\ets\pages\Index.ets
// 通过import的方式，引入native能力。
import entry from 'libentry.so'

function TestFunction(str) {
  let ret = str + ' world!'
  return ret;
}

@Entry
@Component
struct Index {
  @State message: string = 'Test native api: 2 + 3 = ?'
  @State message2: string = 'Click me'

  build() {
    Row() {
      Column() {
        // 第一个按钮，调用add方法，对应到native侧的Add方法，进行两数相加。
        Text(this.message)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.message = "Test native api: 2 + 3 = " + entry.add(2, 3);
            console.log("Test NAPI 2 + 3 = " + entry.add(2, 3));
          })
        // 第二个按钮，调用nativeCallArkTS方法，对应到native的NativeCallArkTS，在native中执行ArkTS function。
        Text(this.message2)
          .fontSize(50)
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.message2 = entry.nativeCallArkTS(TestFunction);
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

```js
// source\entry\src\main\cpp\types\libentry\index.d.ts
// native侧暴露给ArkTS侧接口的声明。
export const add: (a: number, b: number) => number;
export const nativeCallArkTS: (a: object) => string;
``` 

## 样例运行效果
利用[ACE Tools工具](../quick-start/start-overview.md)编译出apk包，并在Android平台上运行后，点击两个按钮，按钮文本随之改变。

## 开发建议

### 注册建议

* nm_register_func对应的函数（如上述Init函数）需要加上static，防止与其他so里的符号冲突。
* 模块注册的入口，即使用\_\_attribute\_\_((constructor))修饰的函数的函数名（如上述RegisterHelloModule函数）需要确保不与其他模块重复。

### so命名规则

**so命名必须符合以下规则：**

* 每个模块对应一个so。
* 如模块名为`entry`，则so的名字为`libentry.so`，`napi_module`中`nm_modname`字段应为`entry`，大小写与模块名保持一致，应用使用时写作：`import entry from 'libentry.so'`。

### JS对象线程限制

ArkCompiler会对JS对象线程进行保护，使用不当会引起应用crash，因此需要遵循如下原则：

* N-API接口只能在JS线程使用。
* env与线程绑定，不能跨线程使用。native侧JS对象只能在创建时的线程使用，即与线程所持有的env绑定。

### 头文件引入限制

在引入头文件时，需引入"napi/native_api.h"，否则会出现N-API接口无法找到的编译报错。
