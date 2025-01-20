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
| void | setLogInterface(ILogger log)                 | 注册LogInterface截断日志                                 |
| void | setLogLevel(int logLevel)                    | 设置输出日志的输出等级，低于该等级的日志被拦截，不再输出 |

## 方法说明

- init

```
/**
* init js environment, should called in application onCreate()
* 
* @param application the target application
*/
public void init(Application application);
```

- onConfigurationChanged

```
/**
* Called by the system when the device configuration changes while your component is running.
* 
* @param newConfig current configuration of environment.
*/
public void onConfigurationChanged(Configuration newConfig);
```

- setLogInterface

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

- setLogLevel

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

