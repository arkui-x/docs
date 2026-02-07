# StageApplicationDelegate

```
java.lang.Object
    └── StageApplicationDelegate
```

```
public class StageApplicationDelegate
```

StageApplicationDelegate是ArkUI-X跨平台上Stage模型Application的代理类，是启动跨平台应用的入口。跨平台的应用必须在其Application的onCreate里创建StageApplicationDelegate，并调用其初始化方法，完成js runtime创建、JsBundle资源解析等工作。

## 方法概要

| 类型 | 方法                                         | 描述             |
| ---- | -------------------------------------------- | ---------------- |
| void | init(Application application)                | 初始化           |
| void | onConfigurationChanged(Configuration newCfg) | 系统环境变化通知 |
| void | setLogInterface(ILogger log)                 | 注册LogInterface截断日志 。该接口将在后续版本废弃，建议使用setLogger代替                                |
| void | setLogLevel(int logLevel)                    | 设置输出日志的最低打印级别，低于该等级的日志被拦截，不再输出。该接口将在后续版本废弃，建议使用setLogger代替 |
| void | initApplication(Application application, boolean shouldLoadUI) | 应用初始化ArkUI-X框架，设置框架是否加载ArkUI |
| void | loadModule(String moduleName, String entryFile) | 加载指定的Hap模块，需要与ArkTS侧[ModuleLoader](../apis/js-apis-ModuleLoader.md)结合使用。 |
| void | setLogger(ILogger logger, int level)         | 注册截断日志并设置日志的最低打印级别，低于该等级的日志被拦截，不再输出。|

## 方法说明

### init

```
/**
* init js environment, should called in application onCreate()
* 
* @param application the target application
*/
public void init(Application application);
```

### onConfigurationChanged

```
/**
* Called by the system when the device configuration changes while your component is running.
* 
* @param newConfig current configuration of environment.
*/
public void onConfigurationChanged(Configuration newConfig);
```

### setLogInterface

```java
/**
* Set log interface.
*
* @param logger the log interface.
*/
public void setLogInterface(ILogger logger);
```

**描述：**

设置日志接口。

**参数：** 

| Name   | 类型                  | 必填 | 描述                    |
| ------ | --------------------- | ---- | ----------------------- |
| logger | [ILogger](ILogger.md) | 是   | 实现ILogger接口的对象。 |

**返回值：** 

无

**示例：** 

```java
LogInterface logInterface = new LogInterface();
this.appDelegate = new StageApplicationDelegate();
this.appDelegate.setLogInterface(logInterface);
```

### setLogLevel

```java
/**
* Set log level.
*
* @param logLevel the log level.
*/
public void setLogLevel(int logLevel);
```

**描述：**

设置日志拦截级别。

**参数：** 

| Name     | 类型 | 必填 | 描述     |
| -------- | ---- | ---- | -------- |
| logLevel | int  | 是   | 日志级别 |

**返回值：** 

无

**示例：** 

```java
LogInterface logInterface = new LogInterface();
this.appDelegate = new StageApplicationDelegate();
this.appDelegate.setLogLevel(ILogger.LOG_DEBUG);
```

### initApplication

```java
/**
 * Initialize stage application.
 *
 * @param application the stage application.
 * @param shouldLoadUI should load UI.
 */
public void initApplication(Application application, boolean shouldLoadUI);
```

**描述：**

应用初始化ArkUI-X框架，控制是否加载ArkUI，如不加载，应用启动占用内存降低。

**参数：** 

| Name         | 类型        | 必填 | 描述                                              |
| ------------ | ----------- | ---- | ------------------------------------------------- |
| application  | Application | 是   | 应用的全局管理者。                                |
| shouldLoadUI | boolean     | 是   | true表示需要加载ArkUI，false表示不需要加载ArkUI。 |

**返回值：** 

无

**示例：** 

```java
StageApplicationDelegate stageApplicationDelegate = new StageApplicationDelegate();
stageApplicationDelegate.initApplication(this, false);
```

### loadModule

```java
/**
 * Load module.
 *
 * @param moduleName The module name.
 * @param entryFile The entry file of the module. Example: "./ets/xxx.ets".
 */
public static void loadModule(String moduleName, String entryFile);
```

**描述：**

加载指定的Hap模块，需要与ArkTS侧[ModuleLoader](../apis/js-apis-ModuleLoader.md)结合使用。

**参数：** 

| Name       | 类型   | 必填 | 描述                                                         |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| moduleName | String | 是   | Hap模块的模块名称。                                          |
| entryFile  | String | 是   | ArkTS侧Hap模块中自定义的[ModuleLoader](../apis/js-apis-ModuleLoader.md)实现类文件的相对路径（相对于Hap模块中ets目录）。<br>更详细的信息见[以Hap为主体的共享逻辑包开发指南-约束与限制](../../tutorial/how-to-decoupled-UI-and-Logic-on-android.md#约束与限制) |

**返回值：** 

无

**示例：** 

```java
// 接口使用范例。
// moduleName : "entry"					   : Hap模块的模块名称为entry
// entryFile  : "./ets/MyModuleLoader.ets" : Hap模块中"MyModuleLoader.ets"的路径
StageApplicationDelegate.loadModule("entry", "./ets/MyModuleLoader.ets");
```

### setLogger

```java

    /**
     * Set log interface and the min level.
     * 
     * @param logger the log interface.
     * @param level the min level, Logs below this level will not be logged.
     */
    public void setLogger(ILogger logger, int level);

```

**描述：**

注册截断日志并设置输出日志的最低打印级别。

**参数：** 

| Name         | 类型        | 必填 | 描述                                              |
| ------------ | ----------- | ---- | ------------------------------------------------- |
| logger  | ILogger | 是   | 应用实现的ILogger。                                |
| level | int     | 是   | 日志的最低打印级别，低于该等级的日志被拦截，不再输出。 |

**返回值：** 

无

**示例：** 

```java
public class MyApplication extends StageApplication {
    @Override
    public void onCreate() {
        LogInterface logInterface = new LogInterface(this);
        StageApplicationDelegate appDelegate = new StageApplicationDelegate();
        appDelegate.setLogger(logInterface,ILogger.LOG_DEBUG);
        super.onCreate();  
    }
}
```
