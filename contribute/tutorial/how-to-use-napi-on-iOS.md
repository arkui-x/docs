TODO:补充在iOS平台如何使用NAPI机制扩展JS API和实现OpenHarmony API。
# ArkUI-CrossPlatform iOS端NAPI

## 说明
iOS端不支持动态加载静态和动态库，不能使用dlopen和dlsym相关动态加载库和符号的api，只能通过原生工程一起打包编译完成NAPI的相关功能的使用

## 步骤
1.编写相关的NAPI功能的c/cpp文件，做好安卓/iOS/OHOS相关终端的源码实现适配
比如拿`napi/sample/native_module_calc/napi_calc.cpp`做分析

2.改造`native_module_calc`源文件
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
针对改造后的`native_module_calc`源文件，需要打包进`IOS_PLATFORM`平台编译链接成对应的framework
```
/// 在ace/napi/BUILD.gn
if (target_os == "ios") {
    ......
    sources += [ "//foundation/ace/napi/sample/native_module_calc/napi_calc.cpp" ]
    ......

}

4.前端调用
```
这个napi实现的功能前端可以用"requireInternal('calc')"来使用其内部提供的'add'方法能力，也可以使用"requireNapi('calc')"来使用，这2种调用方式针对iOS都可以完成js调用原生api的能力。
```
const calc1 = requireNapi('calc')
const calc2 = requireInternal('calc')

const result1 = calc1.add(1, 2)
const result2 = calc2.add(1, 2)
```

