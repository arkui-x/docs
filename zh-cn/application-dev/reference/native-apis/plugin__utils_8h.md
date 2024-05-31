# plugin_utils.h


## 概述

声明用于ArkUI-X Plugin开发的工具API。

**起始版本：**

10

**相关模块：**

[Plugin Utils](_plugin_utils.md)


## 汇总


### 类型定义

| 类型定义名称                                                 | 描述                                |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Task](_plugin_utils.md#arkui_x_plugin_task)          | 提供线程调度的任务类型。              |


### 枚举
| 枚举名称                                                      | 描述                                |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Thread_Mode](_plugin_utils.md#arkui_x_plugin_thread_mode) { <br/>ARKUI_X_PLUGIN_PLATFORM_THREAD = 1, <br/>ARKUI_X_PLUGIN_JS_THREAD = 2 }         | 枚举线程类型。                       |


### 函数

| 函数名称                                                     | 描述                                               |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [ARKUI_X_Plugin_GetJniEnv](_plugin_utils.md#arkui_x_plugin_getjnienv) () | 在Android平台上，获取JNI环境。      |
| [ARKUI_X_Plugin_RegisterJavaPlugin](_plugin_utils.md#arkui_x_plugin_registerjavaplugin) (bool (\*func)(void\*), const char\* name) | 在Android平台上，注册给定的Plugin。      |
| [ARKUI_X_Plugin_RunAsyncTask](_plugin_utils.md#arkui_x_plugin_runasynctask) ([ARKUI_X_Plugin_Task](_plugin_utils.md#arkui_x_plugin_task) task, [ARKUI_X_Plugin_Thread_Mode](_plugin_utils.md#arkui_x_plugin_thread_mode) mode) | 将传入的任务抛到对应线程异步执行。  |
