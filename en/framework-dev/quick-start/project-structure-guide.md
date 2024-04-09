# ArkUI-X Project Code Structure and Building

## Background

ArkUI is the default development framework of OpenHarmony. In the ArkUI-X project, a set of code needs to support build across different OS platforms. Therefore, the ArkUI-X project shares code repositories with OpenHarmony, and some repositories directly point to OpenHarmony related repositories.

##  Code and Repository Structure

The directory structure of the code project is as follows:
```
├── arkcompiler                 // Ark compiler
├── base                        // Basic capabilities
├── build                       // Project build and configuration scripts
├── build_plugins               // Cross-platform build plug-ins
├── commonlibrary               // Common libraries
├── community                   // Community-related information
├── developtools                // Developer tools
├── docs                        // Related documents
├── foundation
│   ├── appframework            // Application framework compatibility adaptation layer
│   ├── arkui                   // ArkUI engines
│   ├── communication           // Communication capability
│   ├── distributeddatamgr      // Distributed data management
│   ├── filemanagement          // File management
│   ├── graphic                 // Graphics engine
│   └── multimedia              // Multimedia
├── interface                   // Interface declaration
├── plugins                     // Plug-in management and implementation
├── prebuilts                   // Directory for prebuilts
├── productdefine               // Product configuration
├── samples                     // Sample code
├── samples                     // Test framework and cases
└── third_party                 // Third-party libraries
```

The table below lists the code repositories.

| Directory                                   | Description                                                  | Code Repository                                              |
| ------------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| build                                       | Project build and configuration scripts                      | [OpenHarmony/build](https://gitee.com/openharmony/build)     |
| build_plugins                               | Cross-platform build plug-ins                                | [ArkUI-X/build_plugins](https://gitee.com/arkui-x/build_plugins) |
| samples                                     | Application samples                                          | [ArkUI-X/samples](https://gitee.com/arkui-x/samples)         |
| community                                   | Community operations and management                          | [ArkUI-X/community](https://gitee.com/arkui-x/community)     |
| docs                                        | Reference documents                                          | [ArkUI-X/docs](https://gitee.com/arkui-x/docs)               |
| interface/sdk                               | ArkUI-X SDK configuration                                    | [ArkUI-X/interface_sdk](https://gitee.com/arkui-x/interface_sdk) |
| plugins                                     | API plug-in management and OpenHarmony API plug-in implementation | [ArkUI-X/plugins](https://gitee.com/arkui-x/plugins)         |
| test/xts                                    | ArkUI-X cross-platform application test suites               | [ArkUI-X/xts](https://gitee.com/arkui-x/xts)                 |
| test/testfwk/arkxtest                       | ArkUI-X test framework                                       | [ArkUI-X/arkxtest](https://gitee.com/arkui-x/arkxtest)       |
| developtools/ace_tools                      | Cross-platform CLI tool                                      | [ArkUI-X/cli](https://gitee.com/arkui-x/cli)                 |
| foundation/appframework                     | Application framework compatibility adaptation               | [ArkUI-X/app_framework](https://gitee.com/arkui-x/app_framework) |
| foundation/arkui/ace_engine/adapter/android | Android platform adaptation code                             | [ArkUI-X/arkui_for_android](https://gitee.com/arkui-x/arkui_for_android) |
| foundation/arkui/ace_engine/adapter/ios     | iOS platform adaptation code                                 | [ArkUI-X/arkui_for_ios](https://gitee.com/arkui-x/arkui_for_ios) |
| foundation/arkui/ace_engine                 | Core code of the ArkUI engine                                | [OpenHarmony/arkui_ace_engine](https://gitee.com/openharmony/arkui_ace_engine) |
| foundation/arkui/napi                       | Native API extension mechanism                               | [OpenHarmony/arkui_napi](https://gitee.com/openharmony/arkui_napi) |
| foundation/communication/netmanager_base    | Network management module                                    | [OpenHarmony/communication_netmanager_base](https://gitee.com/openharmony/communication_netmanager_base) |
| foundation/communication/netstack           | Network protocol stack                                       | [OpenHarmony/communication_netstack](https://gitee.com/openharmony/communication_netstack) |
| foundation/graphic/graphic_2d               | Basic 2D graphics library                                    | [OpenHarmony/graphic_graphic_2d](https://gitee.com/openharmony/graphic_graphic_2d) |
| foundation/filemanagement/file_api          | APIs for accessing folders and files                         | [OpenHarmony/filemanagement_file_api](https://gitee.com/openharmony/filemanagement_file_api) |
| foundation/multimedia/image_framework       | Code for implementing image encoding and decoding            | [OpenHarmony/multimedia_image_framework](https://gitee.com/openharmony/multimedia_image_framework) |
| developtools/ace_ets2bundle                 | Compilation tool and cross-platform application build tool using the ArkTS-based declarative development paradigm | [OpenHarmony/ace_ets2bundle](https://gitee.com/openharmony/developtools_ace_ets2bundle) |
| developtools/ace_js2bundle                  | Compilation tool and cross-platform application building tool using the JS-compatible web-like development paradigm | [OpenHarmony/ace_js2bundle](https://gitee.com/openharmony/developtools_ace_js2bundle) |
| arkcompiler/ets_frontend                    | Ark frontend tool                                            | [OpenHarmony/arkcompiler_ets_frontend](https://gitee.com/openharmony/arkcompiler_ets_frontend) |
| arkcompiler/ets_runtime                     | Ark ArkTS runtime                                            | [OpenHarmony/arkcompiler_ets_runtime](https://gitee.com/openharmony/arkcompiler_ets_runtime) |
| arkcompiler/runtime_core                    | Ark compiler runtime                                         | [OpenHarmony/arkcompiler_runtime_core](https://gitee.com/openharmony/arkcompiler_runtime_core) |
| arkcompiler/toolchain                       | Debugging and tuning tool                                    | [OpenHarmony/arkcompiler_toolchain](https://gitee.com/openharmony/arkcompiler_toolchain) |
| prebuilts                                   | Directory for prebuilts (Python, nodejs, clang, and cmake)   | Pre-download the software using a script.                    |
| third_party                                 | Open-source third-party components (reuse the OpenHarmony code repositories) | Open-source third-party library collection referenced.       |
| commonlibrary/c_utils                       | Common C++ functions and classes                             | [OpenHarmony/commonlibrary_c_utils](https://gitee.com/openharmony/commonlibrary_c_utils) |
| commonlibrary/ets_utils                     | Basic JS APIs, such as APIs for managing URLs and URIs       | [OpenHarmony/commonlibrary_ets_utils](https://gitee.com/openharmony/commonlibrary_ets_utils) |
| base/hiviewdfx/hilog                        | System logging                                               | [OpenHarmony/hiviewdfx_hilog](https://gitee.com/openharmony/hiviewdfx_hilog) |
| base/web/webview                            | Native engine of the WebView component                       | [OpenHarmony/web_webview](https://gitee.com/openharmony/web_webview) |
| base/global/resource_management             | Global resource management                                   | [OpenHarmony/global_resource_management](https://gitee.com/openharmony/global_resource_management) |

## Release Synchronization Policy

The OpenHarmony-related code repositories point to the fixed tag points of the OpenHarmony master repository and are synchronized periodically. By default, the synchronization is performed based on the weekly branch frequency of the OpenHarmony.


## ace_engine Directory Structure

The directory structure of the ArkUI engine core code repository **ace_engine** is as follows:

```
foundation/arkui/ace_engine
├── ace_config.gni      // Global configuration file
├── adapter             // Platform adaptation layer
│   ├── android         // Independent repository for Android adaptation
│   │   ├── build
│   │   ├── capability
│   │   ├── entrance
│   │   ├── stage
│   │   └── osal
│   ├── ios             // Independent repository for iOS adaptation
│   │   ├── build
│   │   ├── capability
│   │   ├── entrance
│   │   ├── stage
│   │   └── osal
│   ├── ohos            // OpenHarmony platform adaptation
│   └── preview         // Previewer adaptation with the platform
├── build               // Build configuration
│   ├── ace_gen_obj.gni
│   ├── ace_lib.gni
│   ├── BUILD.gn
│   ├── search.py
│   └── tools
├── BUILD.gn            // Global build configuration
├── frameworks          // Engine framework layer
│   ├── base            // base library
│   ├── bridge          // Frontend bridging
│   └── core            // Engine core implementation
├── interfaces          // Universal external interfaces
│   └── napi
│       └── kits
├── LICENSE
├── OAT.xml
├── README.md
├── README_zh.md
└── test                // Test code
```

## Compilation and Build Process

To support cross-platform build with a set of code, the compilation configuration needs to be dynamically adjusted based on the code structure. Since different OS platforms have different dependencies and the source code of the dependencies may vary with the environment, the build targets must be dynamically defined.

-  Build entry: The build entry of OpenHarmony is **bundle.json**. The **bundle.json** file defines the subsystem components and external interfaces and stored in the root directory of the subsystem. The other OS platforms use the module name **ace_packages** as the build entry.

-  Global configuration: The **BUILD.gn** and **ace_config.gni** files in the root directory contain global configurations. **BUILD.gn** defines **ace_config** and **ace_test_config** used globally. The **ace_config.gni** file defines the following configurations:
    * Configuration of global variables, for example, **enable_ace_debug**. 
    * Global path definition, for example, **ace_napi**.
    * Configuration of the toolchain, for example, **windows_buildtool**.
    * Definition of global macros used in **ace_config**, for example, **ace_common_defines**.
    * Platform-related configuration **ace_platforms**, which is the key to dynamic definition of the build targets. The code is as follows:
    ```bash
    ace_platforms = []
    
    # Search for directories under adapter and generate all adapter directory names.
    _ace_adapter_dir = rebase_path("$ace_root/adapter", root_build_dir)
    _adapters = exec_script("build/search.py", [ _ace_adapter_dir ], "list lines")
    
    # Import the platform.gni file of each adapter, generate the platform definition configuration, and add the configuration to the ace_platforms file.
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

-  **Platform configuration**: The **build** directory of each **adapter** contains platform-related configuration.
    ```
    ├── BUILD.gn      // Definition of the entry target, for example, "ace_packages"
    ├── config.gni    // Platform configuration
    ├── bundle.json // Component configuration, which is required only for OpenHarmony
    └── platform.gni  // Platform definition
    ```
    The content of a **config.gni** file is similar to the following:
  
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
  
    This file defines all differentiated configurations of the platform, such as macros, cflags, platform-specific dependencies, and some functional switches. In specific module definitions, the configuration such as macros and cflags are directly imported from **config.gni**. Other differences must be defined as functional switches, such as **use_curl_download**, to make the definitions of module gn and features clearer.
  
-  **Dynamic target definition**: Dynamic target definition can be implemented based on the previous platform configuration. The following uses the **base** module as an example:

    ```bash
    import("//build/ohos.gni")
    import("//foundation/arkui/ace_engine/ace_config.gni")
    
    template("ace_base_source_set") {
      forward_variables_from(invoker, "*")
    
      ohos_source_set(target_name) {
        # Import define and cflags from the platform config.
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
    
      # Perform differentiated configuration via platform variables.
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
    
        # Determine whether to depend on and compile curl based on the platform config.
        if (defined(config.use_curl_download) && config.use_curl_download) {
          configs += [ "//third_party/curl:curl_config" ]
          sources += [ "$ace_root/frameworks/base/network/download_manager.cpp" ]
          deps += [ "//third_party/curl:curl" ]
        }
      }
    }
    
    # Dynamically define targets based on ace_platforms. For example, if ace_platforms are ohos, windows, and mac, three targets,
    # namely ace_base_ohos, ace_base_windows, and ace_base_mac, will be defined.
    foreach(item, ace_platforms) {
      ace_base_source_set("ace_base_" + item.name) {
        # Import variables from platform
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

-  **Module definition**: Divide modules based on the directory structure and define each module clearly. Use features to differentiate internal differences of a platform. For inter-platform differences, use the preceding method to define modules.

-  **Dependencies**: Clearly define dependencies by layer. Reverse dependency is not allowed.


## Development Guidelines

- Apply the layered design for implementation of all functions.
- All modules in the **frameworks** directory are platform-irrelevant. They cannot depend on modules in the **adapter** directory or other subsystems related to the JS engine.
- Platform-related code must be stored in the **adapter** directory of the related platform. If the code needs to be referenced in frameworks, proper interface abstraction is required.
- Non-OpenHarmony adapters cannot depend on modules of other OpenHarmony subsystems (except that the subsystem is designed for cross-platform purposes and is universal).
- **frameworks/core** cannot depend on the modules in **frameworks/bridge**.
- The new code cannot directly depend on the Skia interfaces. Use the drawing interfaces provided by **graphic_2d**.
- After the code in the OpenHarmony repository is modified, ensure that the build on each platform is successful and the functions are normal.
