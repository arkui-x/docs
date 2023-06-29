# Plugin Utils


描述ArkUI-X Plugin开发涉及的工具库，包括插件注册、任务执行等。

**起始版本：**


10


## 汇总


### 文件

| 文件名称                                                     | 描述                                 |
| ------------------------------------------------------------ | ------------------------------------ |
| [plugin_utils.h](plugin__utils_8h.md)                  | 声明用于ArkUI-X Plugin开发的工具API。   |


### 类型定义

| 类型定义名称                                                 | 描述                                |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Task](#arkui_x_plugin_task)                           | 提供线程调度的任务类型。               |

### 枚举
| 枚举名称                                                      | 描述                                |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Thread_Mode](_plugin_utils.md#arkui_x_plugin_thread_mode) { <br/>ARKUI_X_PLUGIN_PLATFORM_THREAD = 1, <br/>ARKUI_X_PLUGIN_JS_THREAD = 2 }         | 枚举线程类型。                       |

### 函数

| 函数名称                                                     | 描述                                               |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [ARKUI_X_Plugin_GetJniEnv](#arkui_x_plugin_getjnienv) () | 在Android平台上，获取JNI环境。      |
| [ARKUI_X_Plugin_RegisterJavaPlugin](#arkui_x_plugin_registerjavaplugin) (bool (\*func)(void\*), const char\* name) | 在Android平台上，注册给定的Plugin。      |
| [ARKUI_X_Plugin_RunAsyncTask](#arkui_x_plugin_runasynctask) ([ARKUI_X_Plugin_Task](#arkui_x_plugin_task) task, [ARKUI_X_Plugin_Thread_Mode](#arkui_x_plugin_thread_mode) mode) | 将传入的任务抛到指定线程异步执行。  |


## 详细描述


## 类型定义说明


### ARKUI_X_Plugin_Task


```
typedef void (*ARKUI_X_Plugin_Task)();
```

**描述：**

提供线程调度的任务类型。

**起始版本：**

10


## 枚举定义说明


### ARKUI_X_Plugin_Thread_Mode


```
enum ARKUI_X_Plugin_Thread_Mode
```

**描述：**

枚举线程类型。

| 枚举值                      | 描述         |
| -------------------------- | ------------- |
| ARKUI_X_PLUGIN_PLATFORM_THREAD  | Platform线程。|
| ARKUI_X_PLUGIN_JS_THREAD        | JS线程。      |

**起始版本：**

10


## 函数说明

### ARKUI_X_Plugin_GetJniEnv()


```
JNIEnv* ARKUI_X_Plugin_GetJniEnv();
```

**描述：**

在Android平台上，获取JNI环境。

**参数：**

无参数。

**返回：**

| 类型       | 描述                                    |
| ---------- | --------------------------------------- |
| JNIEnv*       | Android平台上，JNI环境的指针。                |

**起始版本：**

10


### ARKUI_X_Plugin_RegisterJavaPlugin()


```
void ARKUI_X_Plugin_RegisterJavaPlugin(bool (*func)(void*), const char* name);
```

**描述：**

在Android平台上，注册给定的Plugin。

**参数：**

| Name       | 描述                                    |
| ---------- | --------------------------------------- |
| func       | 待注册plugin的初始化函数。                |
| name       | 待注册plugin的包名。                     |

**返回：**

无返回值。

**起始版本：**

10


### ARKUI_X_Plugin_RunAsyncTask()


```
void ARKUI_X_Plugin_RunAsyncTask(ARKUI_X_Plugin_Task task, ARKUI_X_Plugin_Thread_Mode mode);
```

**描述：**

将传入的任务抛到指定线程异步执行。

**参数：**

| Name       | 描述                                    |
| ---------- | --------------------------------------- |
| task       | 表示待抛到指定线程执行的任务。             |
| mode       | 表示指定的线程类型。                      |

**返回：**

无返回值。

**起始版本：**

10
