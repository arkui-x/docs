# How to Use Dynamic Features on iOS

## Overview

You can develop ArkUI-X dynamic features on the iOS platform, including dynamic directory rules and constraints.

## When to Use

AppStore of the iOS platform does not allow applications to dynamically update .so libraries. Therefore, ArkUI-X can dynamically update UI pages and .abc files on iOS devices, but not dynamically loading .so libraries. The ArkUI-X basic library, plugins, and service .so libraries must be packed in the application and released in advance.

## Development Guidelines

### Directory Structure

The directory structure of the ArkUI-X sandbox on the iOS platform is as follows:

```
/Data/Application/*application*/Documents/files/arkui-x   
├── feature1                    # Cross-platform feature 1
│   ├── ets                     # ets directory
│   │   ├──sourceMaps.map
│   │   └──modules.abc
│   ├── resources.index         
│   ├── resources              
│   └── module.json
└── systemres                   # ArkUI public resources
```

- **/Data/Application/*application*/Documents/files/arkui-x** is the root directory of the application sandbox for ArkUI-X dynamic loading. The feature bundles must be stored in this directory.
- The directories under the root directory must be organized by module and must be unique.

### Loading Priority

+ Module loading: The system preferentially searches for the module in the root directory of the application. If the module is not found, the system attempts to load the module in the application sandbox.

+ systemres loading: The system preferentially searches for **systemres** in the root directory of the application. If **systemres** is not found, the system attempts to load **systemres** in the sandbox.

  **NOTE**: It is not recommended that the same module be used. That is, the module is pre-installed in the application and deployed in the sandbox.
