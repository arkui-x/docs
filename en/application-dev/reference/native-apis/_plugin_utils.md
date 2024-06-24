# Plugin Utils


Describes the utils library involved in ArkUI-X plugin development, including plugin registration and task execution.

**Since**


10


## Summary


### Files

| Name                                                    | Description                                |
| ------------------------------------------------------------ | ------------------------------------ |
| [plugin_utils.h](plugin__utils_8h.md)                  | Declares the utils APIs used for ArkUI-X plugin development.  |


### Types

| Name                                                | Description                               |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Task](#arkui_x_plugin_task)                           | Defines the task type of thread scheduling.              |

### Enums
| Name                                                     | Description                               |
| ------------------------------------------------------------ | ----------------------------------- |
| [ARKUI_X_Plugin_Thread_Mode](_plugin_utils.md#arkui_x_plugin_thread_mode) { <br>ARKUI_X_PLUGIN_PLATFORM_THREAD = 1, <br>ARKUI_X_PLUGIN_JS_THREAD = 2 }         | Enumerates the thread types.                      |

### Functions

| Name                                                    | Description                                              |
| ------------------------------------------------------------ | -------------------------------------------------- |
| [ARKUI_X_Plugin_GetJniEnv](#arkui_x_plugin_getjnienv) () | Obtains the JNI environment on the Android platform.     |
| [ARKUI_X_Plugin_RegisterJavaPlugin](#arkui_x_plugin_registerjavaplugin) (bool (\*func)(void\*), const char\* name) | Registers a plugin on the Android platform.     |
| [ARKUI_X_Plugin_RunAsyncTask](#arkui_x_plugin_runasynctask) ([ARKUI_X_Plugin_Task](#arkui_x_plugin_task) task, [ARKUI_X_Plugin_Thread_Mode](#arkui_x_plugin_thread_mode) mode) | Dispatches a task to the thread for asynchronous execution. |


## Description


## Type Description


### ARKUI_X_Plugin_Task


```
typedef void (*ARKUI_X_Plugin_Task)();
```

**Description**

Defines the task type of thread scheduling.

**Since**

10


## Enum Description


### ARKUI_X_Plugin_Thread_Mode


```
enum ARKUI_X_Plugin_Thread_Mode
```

**Description**

Enumerates the thread types.

| Value                     | Description        |
| -------------------------- | ------------- |
| ARKUI_X_PLUGIN_PLATFORM_THREAD  | Platform thread.|
| ARKUI_X_PLUGIN_JS_THREAD        | JS thread.     |

**Since**

10


## Function Description

### ARKUI_X_Plugin_GetJniEnv()


```
JNIEnv* ARKUI_X_Plugin_GetJniEnv();
```

**Description**

Obtains the JNI environment on the Android platform.

**Parameters**

None

**Returns**

| Type      | Description                                   |
| ---------- | --------------------------------------- |
| JNIEnv*       | Pointer to the JNI environment on the Android platform.               |

**Since**

10


### ARKUI_X_Plugin_RegisterJavaPlugin()


```
void ARKUI_X_Plugin_RegisterJavaPlugin(bool (*func)(void*), const char* name);
```

**Description**

Registers a plugin on the Android platform.

**Parameters**

| Name       | Description                                   |
| ---------- | --------------------------------------- |
| func       | Initialization function of the plugin to be registered.               |
| name       | Package name of the plugin to be registered.                    |

**Returns**

No return value.

**Since**

10


### ARKUI_X_Plugin_RunAsyncTask()


```
void ARKUI_X_Plugin_RunAsyncTask(ARKUI_X_Plugin_Task task, ARKUI_X_Plugin_Thread_Mode mode);
```

**Description**

Dispatches a task to the thread for asynchronous execution.

**Parameters**

| Name       | Description                                   |
| ---------- | --------------------------------------- |
| task       | Task to be dispatched.            |
| mode       | Thread type.                     |

**Returns**

No return value.

**Since**

10
