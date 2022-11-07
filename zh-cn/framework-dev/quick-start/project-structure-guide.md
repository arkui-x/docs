# **代码工程及构建介绍**

## 背景

ArkUI作为OpenHarmony的默认开发框架，在本项目（ArkUI-X）中需要做到一套代码同时支持多平台构建，所以会采取共仓开发的方式，部分仓直接指向OpenHarmony的仓。


##  代码结构及仓库结构

具体的代码结构及指向，见下表：

| 目录路径                                  | 描述                                                  | 代码仓位置                                                   |
| ----------------------------------------- | ----------------------------------------------------- | ------------------------------------------------------------ |
| build                                     | ArkUI跨平台项目构建和配置脚本                         | [arkui-x/build](https://gitee.com/arkui-x/build) |
| samples                                   | 应用程序样例                                          | [arkui-x/samples](https://gitee.com/arkui-x/samples) |
| community                                 | 社区运作管理                                          | [arkui-x/community](https://gitee.com/arkui-x/community) |
| docs                                      | 说明文档                                              | [arkui-x/docs](https://gitee.com/arkui-x/docs) |
| developtools/ace_tools                    | 跨平台命令行工具                                      | [arkui-x/cli](https://gitee.com/arkui-x/cli) |
| foundation/arkui/ace_engine/adapter/android | Android平台适配代码                                   | [arkui-x/arkui_for_android](https://gitee.com/arkui-x/arkui_for_android) |
| foundation/arkui/ace_engine/adapter/ios     | iOS平台适配代码                                       | [arkui-x/arkui_for_ios](https://gitee.com/arkui-x/arkui_for_ios) |
| foundation/arkui/ace_engine                 | ArkUI 引擎核心代码 | [openharmony/arkui_ace_engine](https://gitee.com/openharmony/arkui_ace_engine) |
| foundation/arkui/napi                       | Native API扩展机制                                    | [openharmony/arkui_napi](https://gitee.com/openharmony/arkui_napi) |
| developtools/ace_ets2bundle               | ArkUI-声明式范式编译转换工具和跨平台应用构建工具      | [openharmony/ace_ets2bundle](https://gitee.com/openharmony/developtools_ace_ets2bundle) |
| developtools/ace_js2bundle                | ArkUI-类Web范式编译转换工具和跨平台应用构建工具       | [openharmony/ace_js2bundle](https://gitee.com/openharmony/developtools_ace_js2bundle) |
| interface/sdk-js                          | OpenHarmony API和ArkUI组件接口                        | [openharmony/interface_sdk-js](https://gitee.com/openharmony/interface_sdk-js) |
| prebuilts                                 | 预编译目录，python，nodejs，clang和cmake等            | 通过脚本预下载                                               |
| third_party                               | 开源第三方组件（统一复用OpenHarmony代码仓）           | [openharmony/third_party](https://gitee.com/organizations/openharmony/projects) |
| utils/native                              | 常用的工具集                                          | [openharmony/utils_native](https://gitee.com/openharmony/utils_native) |


## 分支同步策略

OpenHarmony相关代码仓，指向OpenHarmony master分支的固定tag点，定期同步，暂定每个月末前一周进行同步


## ArkUI目录结构

ArkUI引擎核心代码仓 `ace_engine` 的目录结构以及每个目录的内容如下：

```
foundation/arkui/ace_engine
├── ace_config.gni      // 全局配置文件
├── adapter             // 平台适配层
│   ├── android         // Android平台适配
│   │   ├── build
│   │   ├── capability
│   │   ├── entrance
│   │   └── osal
│   ├── ios             // iOS平台适配
│   │   ├── build
│   │   ├── capability
│   │   ├── entrance
│   │   └── osal
│   ├── ohos            // OpenHarmony平台适配
│   │   ├── build
│   │   ├── capability
│   │   ├── entrance
│   │   └── osal
│   └── preview         // 预览器平台适配
│       ├── build
│       ├── entrance
│       ├── inspector
│       └── osal
├── build               // 编译配置
│   ├── ace_gen_obj.gni
│   ├── ace_lib.gni
│   ├── BUILD.gn
│   ├── search.py
│   ├── external_config     // 三方库编译配置
│   │   ├── cJSON
│   │   ├── expat
│   │   ├── flutter
│   │   └── ...
│   └── tools
├── BUILD.gn            // 全局编译配置
├── frameworks          // 引擎框架层
│   ├── base            // base库
│   │   ├── BUILD.gn
│   │   ├── geometry
│   │   ├── i18n
│   │   ├── image
│   │   ├── json
│   │   ├── log
│   │   ├── memory
│   │   ├── network
│   │   ├── resource
│   │   ├── test
│   │   ├── thread
│   │   └── utils
│   ├── bridge          // 前端桥接
│   │   ├── BUILD.gn
│   │   ├── card_frontend
│   │   ├── codec
│   │   ├── common
│   │   ├── declarative_frontend
│   │   ├── js_frontend
│   │   └── test
│   └── core            // 引擎核心实现
│       ├── accessibility
│       ├── animation
│       ├── BUILD.gn
│       ├── common
│       ├── components
│       ├── components_v2
│       ├── event
│       ├── focus
│       ├── gestures
│       ├── image
│       ├── mock
│       └── pipeline
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

为了支持一套代码在OpenHarmony和其它平台同时构建，需要根据代码结构动态对编译配置进行调整，因为对外依赖可能不同，并且不同环境下可能依赖的外部源码都有差异，所以必须对编译目标(targets)都进行动态的定义，否则无法保持一致

-  **编译入口：** OpenHarmony的编译入口为ohos.build/bundle.json，这里定义了子系统的部件以及对外的接口，分别放在对应adapter的build目录下，ArkUI的其它平台构建暂不需要，使用固定模块名“ace_packages”作为入口

-  **全局配置：** 根目录下的BUILD.gn以及ace_config.gni为全局的配置。

  其中BUILD.gn下定义了全局使用的“ace_config”， “ace_test_config”

  其中ace_config.gni分别定义了以下配置：

  - 全局的变量配置，如“enable_ace_debug”，通过开关进行编译控制

  - 全局的路径定义，如“flutter_root”，依赖三方库的路径

  - 工具链相关的配置，如“windows_buildtool”

  - 在“ace_config”中使用的全局宏的定义，如“ace_common_defines”

  - 生成平台相关的配置项“ace_platforms”，这一步是动态定义目标的关键，见如下代码：

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
  ├── ohos.build    // 部件配置，非OH平台暂不需要
  └── platform.gni  // 平台定义
  ```

  其中，单个config.gni的配置类似如下：
  
  ```bash
  defines = [
   "OHOS_PLATFORM",
   "OHOS_STANDARD_SYSTEM",
  ]
  
  js_engines = []
  qjs_engine = {
   engine_name = "qjs"
   engine_path = "quickjs"
   engine_defines = [ "USE_QUICKJS_ENGINE" ]
   have_debug = true
   declarative_default = true
  }
  js_engines += [ qjs_engine ]
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
- 非OH的adapter中不能依赖OpenHarmony其它子系统的模块（例外：该子系统有跨平台计划并且通用）
- frameworks/core不允许依赖frameworks/bridge下的模块
- 只有entrance下或flutter_xxx开头的文件可以依赖flutter和skia的接口
- 修改OH仓中的内容要保证各平台都能编译通过，功能正常