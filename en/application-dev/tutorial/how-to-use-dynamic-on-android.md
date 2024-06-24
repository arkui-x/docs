# How to Use Dynamic Features on Android

## Overview

You can develop ArkUI-X dynamic features on the Android platform, including dynamic directory rules and constraints.

## When to Use

The dynamic features mainly involve two typical scenarios:

+ Dynamic framework, which reduces the ROM usage of applications and empowers dynamic framework upgrades.
+ Dynamic feature bundle, which decouples features from host applications.

## Development Guidelines

### Directory Structure

During dynamic loading, the directory architecture in the application sandbox must be as follows:

```
/data/data/*application*/files/arkui-x   
├── feature1                    # Cross-platform feature 1
│   ├── ets                     # ets directory
│   │   ├──sourceMaps.map
│   │   └──modules.abc
│   ├── resources.index         
│   ├── resources              
│   ├── module.json
│   └── libs                    # .so libraries contained in the feature bundle
│       ├── arm64-v8a
│       ├── armeabi-v7a
│       └── x86_64  
├── systemres                   # ArkUI public resources
└── libs                        # libs libraries in the root directory
    ├── arm64-v8a               
    │    └──libarkui_android.so    # ArkUI-X engine
    ├── armeabi-v7a           
    └── x86_64
```

- **/data/data/*application*/files/arkui-x** is the root directory of the application sandbox for ArkUI-X dynamic loading. The framework and feature bundles must be stored in this directory.

- The **libs** directory in the root directory stores the engine (**libarkui_android.so**) and other public libraries.

- The directories under the root directory must be organized by module and must be unique.

### Loading Priority

+ Engine .so library: The .so library in the **lib** directory of the application is preferentially loaded. If it is not found, the .so library in the root directory of the application sandbox is loaded.

+ Plugin. so library: The .so library in the **lib** directory of the application is preferentially loaded. If it is not found, the system attempts to load the .so library in the root directory of the application sandbox and then the .so library in the **libs** directory of the plugin.

+ Module loading: The system preferentially searches for the module in the **assets** directory of the application. If the module is not found, the system attempts to load the module in the application sandbox.

+ systemres loading: The system preferentially searches for **systemres** in the **assets** directory of the application. If **systemres** is not found, the system attempts to load **systemres** in the sandbox.

  **NOTE**: It is not recommended that the same module be used. That is, the module is pre-installed in the **assets** directory of the application and deployed in the sandbox.

### Framework Initialization

If the application uses the dynamic framework engine, place the engine library in **/data/data/*application*/files/arkui-x/libs/arm64-v8a** after downloading it.

Then open the corresponding cross-platform UI to initialize the framework.

```ts
appDelegate = new StageApplicationDelegate();
appDelegate.initApplication(this)
```

If you open the application later, you are advised to initialize the framework in the application according to the normal process and load the engine library in advance to speed up cross-platform module loading.
