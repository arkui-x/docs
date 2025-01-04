# **代码工程及构建介绍**

## 背景

ArkUI作为OpenHarmony的默认开发框架，在本项目（ArkUI-X）中需要做到一套代码同时支持多平台构建，所以会采取共仓开发的方式，部分仓直接指向OpenHarmony相关开源仓。

##  代码结构及仓库结构

代码工程的目录结构如下：
```
├── arkcompiler                 // 方舟编译器
├── base                        // 基础能力
├── build                       // 项目构建和配置脚本
├── build_plugins               // 跨平台构建插件
├── commonlibrary               // 公共基础库
├── community                   // 社区相关
├── developtools                // 开发者工具
├── docs                        // 配套文档
├── foundation
│   ├── appframework            // 应用框架兼容适配层
│   ├── arkui                   // ArkUI引擎
│   ├── communication           // 通信能力
│   ├── distributeddatamgr      // 分布式数据管理
│   ├── filemanagement          // 文件管理
│   ├── graphic                 // 图形引擎
│   └── multimedia              // 多媒体
├── interface                   // 接口声明
├── plugins                     // 插件管理与实现
├── prebuilts                   // 预编译目录
├── productdefine               // 产品形态配置
├── samples                     // 示例代码
├── test                        // 测试框架与用例
└── third_party                 // 三方库
```

具体的代码结构及指向，见下表：

| 目录路径                                  | 描述                                                  | 代码仓位置                                                   |
| ----------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| build                                     | 项目构建和配置脚本                         | [OpenHarmony/build](https://gitcode.com/openharmony/build) |
| build_plugins                             | 跨平台构建插件                        | [ArkUI-X/build_plugins](https://gitcode.com/arkui-x/build_plugins) |
| samples                                   | 应用程序样例                                          | [ArkUI-X/samples](https://gitcode.com/arkui-x/samples) |
| community                                 | 社区运作管理                                          | [ArkUI-X/community](https://gitcode.com/arkui-x/community) |
| docs                                      | 说明文档                                              | [ArkUI-X/docs](https://gitcode.com/arkui-x/docs) |
| interface/sdk                             | ArkUI-X SDK配置                                     | [ArkUI-X/interface_sdk](https://gitcode.com/arkui-x/interface_sdk) |
| plugins                                   | API插件管理，OpenHarmony API插件实现                   | [ArkUI-X/plugins](https://gitcode.com/arkui-x/plugins) |
| test/xts                                  | ArkUI-X跨平台应用测试套件                   | [ArkUI-X/xts](https://gitcode.com/arkui-x/xts) |
| test/testfwk/arkxtest                     | ArkUI-X测试框架                   | [ArkUI-X/arkxtest](https://gitcode.com/arkui-x/arkxtest) |
| developtools/ace_tools                    | 跨平台命令行工具                                       | [ArkUI-X/cli](https://gitcode.com/arkui-x/cli) |
| foundation/appframework | 应用框架兼容适配层    | [ArkUI-X/app_framework](https://gitcode.com/arkui-x/app_framework) |
| foundation/arkui/ace_engine/adapter/android | Android平台适配代码                                   | [ArkUI-X/arkui_for_android](https://gitcode.com/arkui-x/arkui_for_android) |
| foundation/arkui/ace_engine/adapter/ios     | iOS平台适配代码                                       | [ArkUI-X/arkui_for_ios](https://gitcode.com/arkui-x/arkui_for_ios) |
| foundation/arkui/ace_engine                 | ArkUI 引擎核心代码 | [OpenHarmony/arkui_ace_engine](https://gitcode.com/openharmony/arkui_ace_engine) |
| foundation/arkui/napi                       | Native API扩展机制                                    | [OpenHarmony/arkui_napi](https://gitcode.com/openharmony/arkui_napi) |
| foundation/communication/netmanager_base    | 网络管理模块               | [OpenHarmony/communication_netmanager_base](https://gitcode.com/openharmony/communication_netmanager_base) |
| foundation/communication/netstack           | 网络协议栈               | [OpenHarmony/communication_netstack](https://gitcode.com/openharmony/communication_netstack) |
| foundation/graphic/graphic_2d               | 2D图形基础库                                    | [OpenHarmony/graphic_graphic_2d](https://gitcode.com/openharmony/graphic_graphic_2d) |
| foundation/filemanagement/file_api          | 提供目录和文件的访问操作接口               | [OpenHarmony/filemanagement_file_api](https://gitcode.com/openharmony/filemanagement_file_api) |
| foundation/multimedia/image_framework       | 图片编解码功能实现               | [OpenHarmony/multimedia_image_framework](https://gitcode.com/openharmony/multimedia_image_framework) |
| developtools/ace_ets2bundle               | 基于ArkTS的声明式开发范式编译转换工具和跨平台应用构建工具      | [OpenHarmony/ace_ets2bundle](https://gitcode.com/openharmony/developtools_ace_ets2bundle) |
| developtools/ace_js2bundle                | 兼容JS的类Web开发范式编译转换工具和跨平台应用构建工具       | [OpenHarmony/ace_js2bundle](https://gitcode.com/openharmony/developtools_ace_js2bundle) |
| arkcompiler/ets_frontend                          | 方舟前端工具                        | [OpenHarmony/arkcompiler_ets_frontend](https://gitcode.com/openharmony/arkcompiler_ets_frontend) |
| arkcompiler/ets_runtime                          | 方舟ArkTS运行时                        | [OpenHarmony/arkcompiler_ets_runtime](https://gitcode.com/openharmony/arkcompiler_ets_runtime) |
| arkcompiler/runtime_core                          | 方舟编译器运行时                        | [OpenHarmony/arkcompiler_runtime_core](https://gitcode.com/openharmony/arkcompiler_runtime_core) |
| arkcompiler/toolchain                          | 调试调优工具                        | [OpenHarmony/arkcompiler_toolchain](https://gitcode.com/openharmony/arkcompiler_toolchain) |
| prebuilts                                 | 预编译目录，python，nodejs，clang和cmake等            | 通过脚本预下载                                               |
| third_party                               | 开源第三方组件（统一复用OpenHarmony代码仓）           | 引用开源三方库集合 |
| commonlibrary/c_utils                     | 通用的C++功能函数和类                 | [OpenHarmony/commonlibrary_c_utils](https://gitcode.com/openharmony/commonlibrary_c_utils) |
| commonlibrary/ets_utils                   | 用于存放基础类库JSAPI，比如url、uri等  | [OpenHarmony/commonlibrary_ets_utils](https://gitcode.com/openharmony/commonlibrary_ets_utils) |
| base/hiviewdfx/hilog                      | 系统日志功能                        | [OpenHarmony/hiviewdfx_hilog](https://gitcode.com/openharmony/hiviewdfx_hilog) |
| base/web/webview                          | WebView组件的Native引擎               | [OpenHarmony/web_webview](https://gitcode.com/openharmony/web_webview) |
| base/global/resource_management           | 全球化资源管理                  | [OpenHarmony/global_resource_management](https://gitcode.com/openharmony/global_resource_management) |

## 分支同步策略

OpenHarmony相关代码仓，指向OpenHarmony master分支的固定tag点，定期同步，默认按照OpenHarmony的Weekly分支频率进行同步。


## ArkUI引擎核心代码仓目录结构

ArkUI引擎核心代码仓 `ace_engine` 的目录结构以及每个目录的内容如下：

```
foundation/arkui/ace_engine
├── ace_config.gni      // 全局配置文件
├── adapter             // 平台适配层
│   ├── android         // Android平台适配，独立仓
│   │   ├── build
│   │   ├── capability
│   │   ├── entrance
│   │   ├── stage
│   │   └── osal
│   ├── ios             // iOS平台适配，独立仓
│   │   ├── build
│   │   ├── capability
│   │   ├── entrance
│   │   ├── stage
│   │   └── osal
│   ├── ohos            // OpenHarmony平台适配
│   └── preview         // 预览器平台适配
├── build               // 编译配置
│   ├── ace_gen_obj.gni
│   ├── ace_lib.gni
│   ├── BUILD.gn
│   ├── search.py
│   └── tools
├── BUILD.gn            // 全局编译配置
├── frameworks          // 引擎框架层
│   ├── base            // base库
│   ├── bridge          // 前端桥接
│   └── core            // 引擎核心实现
├── interfaces          // 通用对外接口
│   └── napi
│       └── kits
├── LICENSE
├── OAT.xml
├── README.md
├── README_zh.md
└── test                // 测试相关
```

## 编译构建流程

为了支持一套代码在OpenHarmony和其它平台同时构建，需要根据代码结构动态对编译配置进行调整，因为对外依赖可能不同，并且不同环境下可能依赖的外部源码都有差异，所以必须对编译目标(targets)都进行动态的定义，否则无法保持一致。

-  **编译入口：** OpenHarmony的编译入口为bundle.json，这里定义了子系统的部件以及对外的接口，放在对应子系统根目录下，ArkUI的其它平台构建暂不需要，使用固定模块名“ace_packages”作为入口。

-  **全局配置：** 根目录下的BUILD.gn以及ace_config.gni为全局的配置，其中BUILD.gn下定义了全局使用的“ace_config”、“ace_test_config”，其中ace_config.gni分别定义了以下配置：
    * 全局的变量配置，如“enable_ace_debug”，通过开关进行编译控制。
    * 全局的路径定义，如“ace_napi“，配置路径的变量。
    * 工具链相关的配置，如“windows_buildtool”。
    * 在“ace_config”中使用的全局宏的定义，如“ace_common_defines”。
    * 生成平台相关的配置项“ace_platforms”，这一步是动态定义目标的关键，见如下代码：
    ```bash
    ace_platforms = []
    
    # 通过搜索adapter下的目录，生成所有的adapter目录名
    _ace_adapter_dir = rebase_path("$ace_root/adapter", root_build_dir)
    _adapters = exec_script("build/search.py", [ _ace_adapter_dir ], "list lines")
    
    # 导入每个adapter下的platform.gni，生成platform的定义配置，加入到ace_platforms中
    foreach(item, _adapters) {
    import_var = {}
    import_var = {
      import("$ace_root/adapter/$item/build/platform.gni")
    }
    
    if (defined(import_var.platforms)) {
      foreach(platform, import_var.platforms) {
      if (defined(platform.name)) {
        ace_platforms += [ platform ]
      }
      }
    }
    }
    ```

-  **平台配置：** 每个adapter下的build目录存放平台相关的配置
    ```
    ├── BUILD.gn      // 入口目标定义，如"ace_packages"
    ├── config.gni    // 平台配置
    ├── bundle.json   // 部件配置，非OpenHarmony平台暂不需要
    └── platform.gni  // 平台定义
    ```
    其中，单个config.gni的配置类似如下：
  
    ```bash
    defines = [
    "OHOS_PLATFORM",
    "OHOS_STANDARD_SYSTEM",
    ]
    
    js_engines = []
    ark_engine = {
    engine_name = "ark"
    engine_path = "jsi"
    engine_defines = [ "USE_ARK_ENGINE" ]
    }
    js_engines += [ ark_engine ]
    
    disable_gpu = true
    use_external_icu = "shared"
    use_curl_download = true
    ohos_standard_fontmgr = true
    sk_use_hilog = true
    accessibility_support = false
    rich_components_support = true
    advance_components_support = false
    form_components_support = false
    
    if (disable_gpu) {
    defines += [ "GPU_DISABLED" ]
    }
    
    cflags_cc = [
    "-Wno-thread-safety-attributes",
    "-Wno-thread-safety-analysis",
    ]
    
    platform_deps = [
    "//foundation/arkui/ace_engine/adapter/ohos/entrance:ace_ohos_standard_entrance",
    "//foundation/arkui/ace_engine/adapter/ohos/osal:ace_osal_ohos",
    ]
    ```
  
    这个文件定义了该平台的所有差异化配置，如宏、cflags、平台特有的依赖，以及一些功能性的开关。在具体的模块定义中，宏、cflags这一类编译配置直接从config引入，其它差异需要定义为功能性的开关，如上述的“use_curl_download”，使模块gn及特性的定义更清晰。
  
-  **动态目标定义：** 基于上述平台的配置，就可以实现动态的定义目标，以base模块为例：

    ```bash
    import("//build/ohos.gni")
    import("//foundation/arkui/ace_engine/ace_config.gni")
    
    template("ace_base_source_set") {
      forward_variables_from(invoker, "*")
    
      ohos_source_set(target_name) {
        # 引入平台config中的define和cflags
        defines += invoker.defines
        cflags_cc = []
        cflags_cc += invoker.cflags_cc
        
        deps = [
          "$ace_root/build/third_party/cJSON:third_party_cjson",
          "i18n:ace_base_i18n_$platform",
          "resource:ace_resource",
        ]
    
        configs = [ "$ace_root:ace_config" ]
    
        # add base source file here
        sources = [
          "geometry/animatable_dimension.cpp",
          "geometry/animatable_matrix4.cpp",
          "geometry/matrix4.cpp",
          "geometry/quaternion.cpp",
          "geometry/transform_util.cpp",
          "json/json_util.cpp",
          "log/ace_trace.cpp",
          "log/dump_log.cpp",
          "memory/memory_monitor.cpp",
          "thread/background_task_executor.cpp",
          "utils/base_id.cpp",
          "utils/date_util.cpp",
          "utils/resource_configuration.cpp",
          "utils/string_utils.cpp",
          "utils/time_util.cpp",
        ]
    
      # 通过platform变量来进行区别配置
        if (platform != "windows") {
          # add secure c API
          include_dirs = [ "//utils/native/base/include" ]
    
          sources += [
            "//utils/native/base/src/securec/memset_s.c",
            "//utils/native/base/src/securec/securecutil.c",
            "//utils/native/base/src/securec/secureprintoutput_a.c",
            "//utils/native/base/src/securec/snprintf_s.c",
            "//utils/native/base/src/securec/sprintf_s.c",
            "//utils/native/base/src/securec/strcat_s.c",
            "//utils/native/base/src/securec/strcpy_s.c",
            "//utils/native/base/src/securec/vsnprintf_s.c",
            "//utils/native/base/src/securec/vsprintf_s.c",
          ]
        }
    
        # 通过平台的config决定是否依赖和编译curl
        if (defined(config.use_curl_download) && config.use_curl_download) {
          configs += [ "//third_party/curl:curl_config" ]
          sources += [ "$ace_root/frameworks/base/network/download_manager.cpp" ]
          deps += [ "//third_party/curl:curl" ]
        }
      }
    }
    
    # 根据ace_platforms动态定义目标：假设里面包含三个平台"ohos"、"windows"、"mac",则会定义三个target，
    # 分别为：ace_base_ohos,ace_base_windows,ace_base_mac
    foreach(item, ace_platforms) {
      ace_base_source_set("ace_base_" + item.name) {
        # 从platform中导入变量
        platform = item.name
        defines = []
        cflags_cc = []
        config = {
        }
    
        if (defined(item.config)) {
          config = item.config
        }
    
        if (defined(config.defines)) {
          defines = config.defines
        }
    
        if (defined(config.cflags_cc)) {
          cflags_cc = config.cflags_cc
        }
      }
    }
    ```

-  **模块定义：** 根据目录结构，尽量分成一个个小模块来定义，每个模块定义明确，如果涉及平台差异，使用上述方法定义，内部配置差异尽量使用特性来区分

-  **依赖关系：** 依赖关系尽量清楚，按层次依赖，不可反向依赖


## 开发原则

- 所有功能实现遵循分层设计
- frameworks目录下的所有模块都是平台无关的，不能依赖adapter目录下的模块，不能依赖除JS引擎相关的其它子系统
- 平台相关的代码都必须放到adapter目录对应的平台下，如果需要在frameworks中引用相关，需要进行适当的接口抽象
- 非OpenHarmony的adapter中不能依赖OpenHarmony其它子系统的模块（例外：该子系统有跨平台计划并且通用）
- frameworks/core不允许依赖frameworks/bridge下的模块
- 新增代码不可直接依赖skia接口，使用graphic_2d提供的drawing接口
- 修改OpenHarmony仓中的内容要保证各平台都能编译通过，功能正常
