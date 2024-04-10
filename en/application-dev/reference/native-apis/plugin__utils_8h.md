# plugin_utils.h


## Overview

Declares the utils APIs used for ArkUI-X plugin development.

**Since**

10

**Related module**

[Plugin Utils](_plugin_utils.md)


## Summary


### Types

| Name                                                | Description                               |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Task](_plugin_utils.md#arkui_x_plugin_task)          | Defines the task type of thread scheduling.             |


### Enums
| Name                                                     | Description                               |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Thread_Mode](_plugin_utils.md#arkui_x_plugin_thread_mode) { <br>ARKUI_X_PLUGIN_PLATFORM_THREAD = 1, <br>ARKUI_X_PLUGIN_JS_THREAD = 2 }         | Enumerates the thread types.                      |


### Functions

| Name                                                    | Description                                              |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [ARKUI_X_Plugin_GetJniEnv](_plugin_utils.md#arkui_x_plugin_getjnienv) () | Obtains the JNI environment on the Android platform.     |
| [ARKUI_X_Plugin_RegisterJavaPlugin](_plugin_utils.md#arkui_x_plugin_registerjavaplugin) (bool (\*func)(void\*), const char\* name) | Registers a plugin on the Android platform.     |
| [ARKUI_X_Plugin_RunAsyncTask](_plugin_utils.md#arkui_x_plugin_runasynctask) ([ARKUI_X_Plugin_Task](_plugin_utils.md#arkui_x_plugin_task) task, [ARKUI_X_Plugin_Thread_Mode](_plugin_utils.md#arkui_x_plugin_thread_mode) mode) | Dispatches a task to the thread for asynchronous execution. |
