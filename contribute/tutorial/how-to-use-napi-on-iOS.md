TODO:补充在iOS平台如何使用NAPI机制扩展JS API和实现OpenHarmony API。
# ArkUI-X iOS端NAPI

## 说明
iOS端不能使用dlopen和dlsym加载相关动态so库和符号，只能通过源码一起打进ios的动态库里

## 步骤
1.编写相关的NAPI功能的c/cpp文件，做好Android/iOS/OHOS相关终端的源码实现适配
比如拿`napi/sample/native_module_calc/napi_calc.cpp`做分析

2.修改`napi_calc`源文件
针对`_binary_calc_js_start`和`_binary_calc_js_end`相关代码做好iOS平台编译区分
```
#ifndef IOS_PLATFORM
extern const char _binary_calc_js_start[];
extern const char _binary_calc_js_end[];
#endif

#ifndef IOS_PLATFORM
extern "C" __attribute__((visibility("default"))) void NAPI_calc_GetJSCode(const char** buf, int* bufLen)
{
    if (buf != nullptr) {
        *buf = _binary_calc_js_start;
    }

    if (bufLen != nullptr) {
        *bufLen = _binary_calc_js_end - _binary_calc_js_start;
    }
}
#endif
```
针对IOS_PLATFORM终端而言，不支持动态加载符号表的功能，这块的代码需要做编译区分

3.gn配置
对应的napi_calc.cpp文件的BUILD.gn，可参考https://gitee.com/openharmony/arkui_napi/blob/master/sample/native_module_calc/BUILD.gn
核心代码如下：
```
import("//build/ohos.gni")
import("//foundation/ace/ace_engine/ace_config.gni")
  config("ace_napi_config") {
    include_dirs = [
      "//third_party/node/src",
      "//foundation/ace/napi/interfaces/kits",
    ]
  }

  ohos_source_set("calc") {
    defines = [ "IOS_PLATFORM" ]
    public_configs = [ ":ace_napi_config" ]
    deps = [ "//foundation/ace/napi/:ace_napi" ]

    sources = [ "napi_calc.cpp" ]

    if (current_cpu == "arm64") {
      defines += [ "_ARM64_" ]
    }

    cflags_cc = [ "-Wno-missing-braces" ]

    subsystem_name = "ace"
    part_name = "napi"
  }
  ```

4.前端调用
```
这个napi实现的功能前端可以用"requireInternal('calc')"来使用其内部提供的'add'方法能力，也可以使用"requireNapi('calc')"来使用，这2种调用方式针对iOS都可以完成js调用原生api的能力。
```
const calc1 = requireNapi('calc')
const calc2 = requireInternal('calc')

const result1 = calc1.add(1, 2)
const result2 = calc2.add(1, 2)
```

