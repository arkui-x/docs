# 插件子系统适配指南

本文档提供 ArkUI-X 插件子系统适配的完整指南，包括架构原则、实现规则、检查清单和参考实现。

## 目录

- [概述](#概述)
- [插件架构](#插件架构)
- [核心规则](#核心规则)
- [实现步骤](#实现步骤)
- [构建系统配置](#构建系统配置)
- [参考实现](#参考实现)
- [检查清单](#检查清单)
- [常见问题](#常见问题)

## 概述

### 什么是插件适配

插件适配是指将 OpenHarmony 的子系统 API（如 `@ohos.net.http`、`@ohos.data.preferences` 等）适配到 Android 和 iOS 平台的过程。

### 适配范围

| 模块类型 | 示例 API | 适配方式 |
|---------|---------|---------|
| **网络** | @ohos.net.http, @ohos.net.socket | 插件适配 |
| **存储** | @ohos.data.preferences, @ohos.data.relationalStore | 插件适配 |
| **文件** | @ohos.file.fs | 插件适配 |
| **多媒体** | @ohos.multimedia.image, @ohos.multimedia.player | 插件适配 |
| **传感器** | @ohos.sensor | 插件适配 |
| **设备** | @ohos.device.info | 插件适配 |

### 适用条件

**需要插件适配**：
- 子系统级别的功能模块
- 暴露 @ohos.* API 给应用层
- 需要调用平台原生能力（网络、存储、传感器等）

**需要框架适配**（不使用插件）：
- 窗口管理
- 输入处理
- 字体渲染
- 键盘管理

## 插件架构

### 5 层架构

```
┌─────────────────────────────────────────────────────────────────┐
│  第 1 层: ArkTS API 层                                           │
│  interface/sdk-js/api/@ohos.xxx.d.ts                            │
│  - TypeScript 类型定义                                           │
│  - 应用开发者使用                                                │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 2 层: NAPI 绑定层                                            │
│  plugins/xxx/napi/xxx_module.cpp                                │
│  - 参数解析                                                      │
│  - 类型转换                                                      │
│  - 异步处理                                                      │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 3 层: 业务逻辑层 (平台无关 C++)                               │
│  plugins/xxx/common/xxx_core.cpp                               │
│  - 核心算法                                                      │
│  - 状态管理                                                      │
│  - 平台无关逻辑                                                  │
└─────────────────────────────────────────────────────────────────┘
                           ↓
┌─────────────────────────────────────────────────────────────────┐
│  第 4 层: 平台接口层 (纯虚 C++ 接口)                              │
│  plugins/xxx/interface/xxx_interface.h                         │
│  - 纯虚接口定义                                                  │
│  - 平台抽象                                                      │
└─────────────────────────────────────────────────────────────────┘
                           ↓
        ┌─────────────────┴─────────────────┐
        ↓                                   ↓
┌─────────────────────┐         ┌─────────────────────┐
│  第 5 层: Android 实现 │         │  第 5 层: iOS 实现   │
│  - JNI Bridge        │         │  - ObjC++ Bridge     │
│  - Java/Kotlin Impl  │         │  - ObjC/Swift Impl   │
│  - AOSP APIs         │         │  - Apple Frameworks  │
└─────────────────────┘         └─────────────────────┘
```

### 目录结构

```
plugins/
├── plugin_lib.gni                    # 插件注册（必须更新）
├── http/                             # 示例：HTTP 插件
│   ├── BUILD.gn                      # 构建模板
│   ├── http.gni                      # 模块配置
│   ├── interface/                    # 平台无关接口
│   │   └── http_interface.h          # 纯虚接口
│   ├── napi/                         # NAPI 绑定层
│   │   └── http_module.cpp           # NAPI 实现
│   ├── common/                       # 共享 C++ 代码
│   │   └── http_core.cpp             # 业务逻辑
│   ├── android/                      # Android 平台
│   │   ├── BUILD.gn
│   │   ├── http_strategy_android.cpp # Android 策略
│   │   └── java/
│   │       └── jni/
│   │           └── http_jni.cpp      # JNI 桥接
│   └── ios/                          # iOS 平台
│       ├── BUILD.gn
│       ├── http_strategy_ios.mm      # iOS 策略
│       └── http_impl.mm              # iOS 实现
```

## 核心规则

### 13 条核心实现规则

#### 1. 分层架构

严格遵循 5 层架构，每层职责单一：

```
API 层 → NAPI 层 → 业务逻辑层 → 平台接口层 → 平台实现层
```

**正确示例**：
```cpp
// interface/http_interface.h - 平台接口层
class IHttpStrategy {
public:
    virtual void ExecuteRequest(const HttpRequest& request, Callback callback) = 0;
};

// android/http_strategy_android.cpp - Android 实现层
class AndroidHttpStrategy : public IHttpStrategy {
    void ExecuteRequest(const HttpRequest& request, Callback callback) override {
        // 调用 Java 实现
    }
};
```

**错误示例**：
```cpp
// ❌ NAPI 层直接调用 Java，跳过业务逻辑层
static napi_value Execute(napi_env env, napi_callback_info info) {
    // 直接调用 JNI，没有平台抽象
    jni_call_java();
}
```

#### 2. 平台隔离

**方式一：目录分离**（推荐）

```
plugins/xxx/
├── common/     # 平台无关代码
├── android/    # Android 特定代码
└── ios/        # iOS 特定代码
```

**方式二：条件编译**（用于小型差异）

```cpp
// common/xxx_core.cpp
#ifdef ANDROID_PLATFORM
    // Android 特定代码
#elif defined(IOS_PLATFORM)
    // iOS 特定代码
#endif
```

#### 3. 最小化 JNI 调用

**缓存 JNI 引用**：

```cpp
// ✅ 正确：缓存 JNI 引用
class JniCache {
private:
    static jmethodID midExecute_;
    static jobject httpService_;

public:
    static void Init(JNIEnv* env) {
        jclass clazz = env->FindClass("com/ohos/HttpService");
        midExecute_ = env->GetMethodID(clazz, "execute", "..."));
        httpService_ = env->NewGlobalRef(...);
    }
};

// ❌ 错误：每次都查找
void Execute() {
    jclass clazz = env->FindClass("com/ohos/HttpService");  // 性能差
    jmethodID mid = env->GetMethodID(clazz, "execute", "...");
}
```

**批量传递数据**：

```cpp
// ✅ 正确：一次性传递所有参数
struct RequestParams {
    std::string url;
    std::string method;
    std::map<std::string, std::string> headers;
    std::string body;
};
jni_execute(env, requestParams);

// ❌ 错误：多次 JNI 调用
jni_set_url(env, url);
jni_set_method(env, method);
jni_set_headers(env, headers);  // 多次跨语言调用
```

#### 4. 统一异步模型

**使用 Promise/Deferred 模式**：

```cpp
// ✅ 正确：统一异步处理
napi_value ExecuteAsync(napi_env env, napi_callback_info info) {
    napi_value promise;
    napi_deferred deferred;
    napi_create_promise(env, &promise, &deferred);

    // 异步执行
    auto callback = [env, deferred](Result result) {
        napi_value result_value;
        // 创建结果对象
        napi_resolve_deferred(env, deferred, result_value);
    };

    strategy_->Execute(request, callback);
    return promise;
}

// ❌ 错误：同步阻塞
napi_value Execute(napi_env env, napi_callback_info info) {
    Result result = strategy_->ExecuteSync(request);  // 阻塞线程
    return convert_to_napi(result);
}
```

#### 5. 模板化 GN 构建

**使用构建模板**：

```gni
# plugins/xxx/xxx.gni
template("xxx_plugin") {
  if (target_os == "android") {
    # Android 特定配置
  } else if (target_os == "ios") {
    # iOS 特定配置
  }

  # 通用配置
  sources = [
    "interface/xxx_interface.h",
    "common/xxx_core.cpp",
  ]
}
```

#### 6. 配置分离

**分离 .gni 和 BUILD.gn**：

```gni
# xxx.gni - 可重用配置
declare_args() {
  xxx_enable_feature = true
}

xxx_sources = [
  "interface/xxx_interface.h",
  "common/xxx_core.cpp",
]
```

```gni
# BUILD.gn - 构建规则
import("xxx.gni")

ohos_shared_library("xxx") {
  sources = xxx_sources
  deps = []
}
```

#### 7. 明确依赖声明

```gni
# BUILD.gn
ohos_shared_library("xxx") {
  deps = [
    "//foundation/arkui/napi:napi",           # 明确声明
    "//plugins/net/http:http_common",         # 明确声明
    "//third_party/curl:curl_shared",         # 明确声明
  ]

  external_deps = [
    "hilog_native:libhilog",                 # 外部依赖
  ]
}
```

#### 8. 接口抽象

**使用纯虚接口**：

```cpp
// ✅ 正确：纯虚接口
class IHttpStrategy {
public:
    virtual ~IHttpStrategy() = default;
    virtual void Execute(const Request& req, Callback cb) = 0;
    virtual void Cancel() = 0;
};

// ❌ 错误：具体实现作为接口
class HttpStrategy {
public:
    void Execute(const Request& req, Callback cb) {
        // 具体实现
    }
};
```

#### 9. 最小化条件编译

**优先使用文件分离**：

```
✅ 推荐：
plugins/xxx/
├── common/xxx_core.cpp      # 平台无关
├── android/xxx_android.cpp  # Android 特定
└── ios/xxx_ios.mm           # iOS 特定

❌ 避免：
plugins/xxx/common/xxx_core.cpp  # 包含大量 #ifdef ANDROID
```

#### 10. 统一错误处理

**定义平台无关错误码**：

```cpp
// common/error_codes.h
enum class ErrorCode {
    OK = 0,
    INVALID_PARAM = 401,
    PERMISSION_DENIED = 200,
    NETWORK_ERROR = 300,
};

// Android 实现
int AndroidMapToErrorCode(int javaError) {
    switch (javaError) {
        case JAVA_INVALID_PARAM: return static_cast<int>(ErrorCode::INVALID_PARAM);
        // ...
    }
}

// iOS 实现
int iOSMapToErrorCode(NSInteger nsError) {
    switch (nsError) {
        case NSURLErrorBadURL: return static_cast<int>(ErrorCode::INVALID_PARAM);
        // ...
    }
}
```

#### 11. 零拷贝数据传输

**使用 std::string_view 和移动语义**：

```cpp
// ✅ 正确：避免拷贝
void ProcessUrl(std::string_view url) {  // 不拷贝
    // 处理 URL
}

std::string BuildRequest() {
    Request req;
    // ...
    return req;  // RVO/移动语义
}

// ❌ 错误：不必要的拷贝
void ProcessUrl(std::string url) {  // 按值传递，拷贝
    // 处理 URL
}
```

#### 12. 智能指针生命周期

**使用 shared_ptr/unique_ptr**：

```cpp
// ✅ 正确：智能指针管理
class HttpClient {
    std::shared_ptr<IHttpStrategy> strategy_;
    std::unique_ptr<RequestContext> context_;
};

// ❌ 错误：手动管理
class HttpClient {
    IHttpStrategy* strategy_;  // 谁负责释放？
    RequestContext* context_;  // 内存泄漏风险
};
```

#### 13. 线程安全

**使用线程安全容器和互斥锁**：

```cpp
// ✅ 正确：线程安全
class RequestManager {
private:
    std::mutex mutex_;
    std::unordered_map<int, std::shared_ptr<Request>> requests_;

public:
    void AddRequest(int id, std::shared_ptr<Request> req) {
        std::lock_guard<std::mutex> lock(mutex_);
        requests_[id] = req;
    }
};

// ❌ 错误：非线程安全
class RequestManager {
private:
    std::unordered_map<int, Request*> requests_;  // 竞态条件
};
```

## 实现步骤

### 步骤 1: 创建插件目录结构

```bash
# 1. 创建插件目录
mkdir -p plugins/new_feature

# 2. 创建子目录
cd plugins/new_feature
mkdir -p interface      # 平台无关接口
mkdir -p napi          # NAPI 绑定
mkdir -p common        # 共享业务逻辑
mkdir -p android       # Android 实现
mkdir -p ios          # iOS 实现
```

### 步骤 2: 定义平台接口

```cpp
// plugins/new_feature/interface/new_feature_interface.h
#ifndef PLUGIN_NEW_FEATURE_INTERFACE_H
#define PLUGIN_NEW_FEATURE_INTERFACE_H

#include <functional>
#include <string>

struct Request {
    std::string data;
    int timeout;
};

struct Response {
    int code;
    std::string result;
};

using Callback = std::function<void(const Response&)>;

class INewFeatureStrategy {
public:
    virtual ~INewFeatureStrategy() = default;
    virtual void Execute(const Request& req, Callback cb) = 0;
    virtual void Cancel() = 0;
};

#endif  // PLUGIN_NEW_FEATURE_INTERFACE_H
```

### 步骤 3: 实现业务逻辑

```cpp
// plugins/new_feature/common/new_feature_core.cpp
#include "interface/new_feature_interface.h"
#include <memory>

class NewFeatureCore {
private:
    std::shared_ptr<INewFeatureStrategy> strategy_;

public:
    void SetStrategy(std::shared_ptr<INewFeatureStrategy> strategy) {
        strategy_ = strategy;
    }

    void Execute(const Request& req, Callback cb) {
        if (strategy_) {
            strategy_->Execute(req, cb);
        }
    }
};
```

### 步骤 4: 实现 Android 平台

```cpp
// plugins/new_feature/android/new_feature_android.cpp
#include "interface/new_feature_interface.h"
#include <jni.h>

class AndroidNewFeatureStrategy : public INewFeatureStrategy {
private:
    JNIEnv* env_;
    jobject java_obj_;

public:
    void Execute(const Request& req, Callback cb) override {
        // 调用 Java 实现
        jmethodID mid = /* 获取方法 ID */;
        jobject result = env_->CallObjectMethod(java_obj_, mid, /* 参数 */);

        // 转换结果并回调
        Response resp;
        cb(resp);
    }

    void Cancel() override {
        // 取消操作
    }
};
```

### 步骤 5: 实现 iOS 平台

```objc
// plugins/new_feature/ios/new_feature_ios.mm
#import "interface/new_feature_interface.h"
#import <Foundation/Foundation.h>

class IOSNewFeatureStrategy : public INewFeatureStrategy {
public:
    void Execute(const Request& req, Callback cb) override {
        // 调用 Objective-C 实现
        NSURLSession* session = [NSURLSession sharedSession];
        // ...

        dispatch_async(dispatch_get_main_queue(), ^{
            Response resp;
            cb(resp);
        });
    }

    void Cancel() override {
        // 取消操作
    }
};
```

### 步骤 6: 添加 NAPI 绑定

```cpp
// plugins/new_feature/napi/new_feature_module.cpp
#include "napi/native_api.h"
#include "interface/new_feature_interface.h"

static napi_value Execute(napi_env env, napi_callback_info info) {
    // 解析参数
    size_t argc = 1;
    napi_value args[1];
    napi_get_cb_info(env, info, &argc, args, nullptr, nullptr);

    // 创建 Promise
    napi_value promise;
    napi_deferred deferred;
    napi_create_promise(env, &promise, &deferred);

    // 调用业务逻辑
    Request req;
    auto callback = [env, deferred](const Response& resp) {
        napi_value result;
        // 转换为 NAPI value
        napi_resolve_deferred(env, deferred, result);
    };

    // 执行请求
    core_->Execute(req, callback);

    return promise;
}

static napi_value Init(napi_env env, napi_value exports) {
    napi_property_descriptor props[] = {
        {"execute", nullptr, Execute, nullptr, nullptr, nullptr, napi_default, nullptr}
    };
    napi_define_properties(env, exports, sizeof(props)/sizeof(props[0]), props);
    return exports;
}

NAPI_MODULE(addon, Init)
```

## 构建系统配置

### 配置文件清单

添加新插件时，必须更新以下 4 个配置文件：

| 配置文件 | 作用 | 优先级 |
|---------|-----|-------|
| `plugins/plugin_lib.gni` | 插件注册 | **必须** |
| `interface/sdk/plugins/api/apiConfig.json` | API 配置 | **必须** |
| `build_plugins/sdk/arkui_cross_sdk_description_std.json` | SDK 描述 | **必须** |
| `plugins/new_feature/BUILD.gn` | 构建规则 | **必须** |

### 1. 注册插件 (plugin_lib.gni)

**文件位置**: `plugins/plugin_lib.gni`

**添加位置**: 在 `common_plugin_libs` 数组中，按字母顺序插入

```gni
# plugins/plugin_lib.gni (约第 120 行)
common_plugin_libs = [
  # ... 现有插件 ...
  "pasteboard",
  "new_feature",  # ← 添加这一行，保持字母顺序
  "vibrator",
]
```

**验证命令**:
```bash
grep "new_feature" plugins/plugin_lib.gni
```

### 2. 配置 API (apiConfig.json)

**文件位置**: `interface/sdk/plugins/api/apiConfig.json`

**添加位置**: 在 JSON 数组末尾、闭合括号前

```json
{
    "module": "ohos.new_feature",
    "library": {
        "android": [
            "lib/new_feature/ace_new_feature_android.jar",
            "lib/new_feature/arch_type/libnew_feature.so"
        ],
        "ios": [ "xcframework/build_modes/libnew_feature.xcframework" ]
    },
    "deps": {
        "android": [],
        "ios": []
    }
}
```

**验证命令**:
```bash
# 验证 JSON 格式
python3 -m json.tool interface/sdk/plugins/api/apiConfig.json > /dev/null && echo "Valid" || echo "Invalid"

# 检查模块已配置
grep "ohos.new_feature" interface/sdk/plugins/api/apiConfig.json
```

### 3. 配置 SDK 描述 (arkui_cross_sdk_description_std.json)

**文件位置**: `build_plugins/sdk/arkui_cross_sdk_description_std.json`

**Android SO 配置** (添加到 `"android"` 数组):

```json
{
    "install_dir": "arkui-x/plugins/api/lib/new_feature/arch_type",
    "module_label": "//plugins/new_feature/android:new_feature_adapter_android",
    "target_os": ["linux", "windows", "darwin"]
}
```

**Android Java 配置** (如果有 Java 适配):

```json
{
    "install_dir": "arkui-x/plugins/api/lib/new_feature",
    "module_label": "//plugins/new_feature/frameworks/android/java:new_feature_plugin_java",
    "target_os": ["linux", "windows", "darwin"]
}
```

**iOS 配置** (添加到 `"ios"` 数组):

```json
{
    "install_dir": "arkui-x/plugins/api/framework/arch_type/libnew_feature.framework",
    "module_label": "//plugins/new_feature/ios:new_feature_adapter_ios",
    "target_os": ["darwin"]
}
```

**验证命令**:
```bash
# 验证 JSON 格式
python3 -m json.tool build_plugins/sdk/arkui_cross_sdk_description_std.json > /dev/null && echo "Valid" || echo "Invalid"

# 检查模块已配置
grep "new_feature" build_plugins/sdk/arkui_cross_sdk_description_std.json
```

### 4. 创建构建规则 (BUILD.gn)

**文件位置**: `plugins/new_feature/BUILD.gn`

```gni
import("//build/ohos.gni")

# 平台无关组件
ohos_source_set("new_feature_common") {
  sources = [
    "interface/new_feature_interface.h",
    "common/new_feature_core.cpp",
  ]

  include_dirs = [
    "interface",
    "//foundation/arkui/napi/native/common",
  ]

  external_deps = [ "hilog_native:libhilog" ]
}

# Android 实现
if (is_android) {
  ohos_shared_library("new_feature_adapter_android") {
    sources = [
      "android/new_feature_android.cpp",
    ]

    deps = [
      ":new_feature_common",
    ]

    include_dirs = [ "interface" ]
  }
}

# iOS 实现
if (is_ios) {
  ohos_shared_library("new_feature_adapter_ios") {
    sources = [
      "ios/new_feature_ios.mm",
    ]

    deps = [
      ":new_feature_common",
    ]

    include_dirs = [ "interface" ]

    frameworks = [ "Foundation" ]
  }
}
```

## 参考实现

### 最佳参考: plugins/net/http

| 方面 | 文件位置 |
|------|---------|
| API 定义 | `interface/sdk-js/api/@ohos.net.http.d.ts` |
| NAPI 绑定 | `plugins/net/http/http_module.cpp` |
| 平台接口 | `plugins/net/http/interface/http_exec_interface.h` |
| 业务逻辑 | `plugins/net/http/common/http_request_group.cpp` |
| Android 实现 | `plugins/net/http/android/http_exec.cpp` |
| iOS 实现 | `plugins/net/http/ios/http_exec.mm` |
| Java 实现 | `plugins/net/http/frameworks/android/java/` |
| 构建配置 | `plugins/net/http/BUILD.gn` |

### 代码示例

#### HTTP 请求流程

```cpp
// 1. NAPI 层接收请求
napi_value CreateHttpRequest(napi_env env, napi_callback_info info) {
    // 解析参数
    // 创建 HttpRequest 对象
}

// 2. 业务逻辑层处理
void HttpCore::ExecuteRequest(const HttpRequest& req, Callback cb) {
    strategy_->Execute(req, cb);
}

// 3. 平台接口层调度
class IHttpExec {
    virtual void Execute(const HttpRequest& req, Callback cb) = 0;
};

// 4. Android 实现
class AndroidHttpExec : public IHttpExec {
    void Execute(const HttpRequest& req, Callback cb) override {
        // 调用 Java HttpURLConnection
    }
};

// 5. iOS 实现
class IOSHttpExec : public IHttpExec {
    void Execute(const HttpRequest& req, Callback cb) override {
        // 调用 NSURLSession
    }
};
```

## 检查清单

### 实现前

- [ ] 确认需要插件适配（非框架能力）
- [ ] 查看 OHOS 是否有可复用的实现
- [ ] 确定适用模块列表

### 实现中

- [ ] 创建目录结构 (interface, napi, common, android, ios)
- [ ] 定义纯虚接口 (interface/)
- [ ] 实现业务逻辑 (common/)
- [ ] 实现 Android 平台 (android/)
- [ ] 实现 iOS 平台 (ios/)
- [ ] 添加 NAPI 绑定 (napi/)

### 构建配置

- [ ] 添加到 `plugins/plugin_lib.gni`
- [ ] 配置 `interface/sdk/plugins/api/apiConfig.json`
- [ ] 配置 `build_plugins/sdk/arkui_cross_sdk_description_std.json`
- [ ] 创建 `BUILD.gn`

### 测试

- [ ] 添加 XTS 测试 (test/xts/)
- [ ] 添加 API 文档 (docs/zh-cn/)
- [ ] 添加示例代码 (samples/CodeLab/)
- [ ] 在 Android 上测试
- [ ] 在 iOS 上测试

### 验证

- [ ] 编译通过
- [ ] 测试通过
- [ ] 文档完整
- [ ] 符合 13 条核心规则

## 常见问题

### Q1: 插件编译不通过？

**A**: 检查以下配置：
1. 是否已添加到 `plugins/plugin_lib.gni`
2. `BUILD.gn` 中的 `deps` 是否正确
3. `include_dirs` 是否包含头文件路径

### Q2: SDK 中找不到插件模块？

**A**: 检查：
1. `build_plugins/sdk/arkui_cross_sdk_description_std.json` 是否配置
2. `interface/sdk/plugins/api/apiConfig.json` 是否配置
3. 重新执行构建

### Q3: 运行时 API 调用失败？

**A**: 检查：
1. NAPI 绑定是否正确导出
2. JNI 引用是否已缓存
3. 异步回调是否在正确的线程执行

### Q4: 如何调试插件？

**A**:
```bash
# 1. 查看构建日志
./build.sh --product-name arkui-x --target-os android --log-level debug

# 2. 查看 NAPI 日志
hilog -T ace

# 3. 查看 JNI 日志
adb logcat | grep -i "new_feature"

# 4. 在代码中添加日志
HILOG_INFO("Tag", "Debug info: %{public}s", data);
```

### Q5: 何时使用目录分离 vs 条件编译？

**A**:
- **目录分离**: 平台逻辑差异大，代码超过 50 行
- **条件编译**: 小型差异，代码少于 20 行

### Q6: 如何处理平台特有功能？

**A**:
```cpp
// 在平台实现层处理，不影响业务逻辑
class AndroidStrategy : public IStrategy {
    void Execute() override {
        #ifdef ANDROID_ENABLE_FEATURE_X
            // Android 特有功能
        #endif
    }
};
```

## 参考文档

- [系统架构](../architecture/system_architecture.md)
- [开发最佳实践](../best_practices/README.md)
- [主 CLAUDE.md](../../CLAUDE.md)
- [OpenHarmony API 文档](https://docs.openharmony.cn/)
