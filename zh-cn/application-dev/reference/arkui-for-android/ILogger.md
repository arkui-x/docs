# ILogger

本模块是日志记录的通用接口，支持日志拦截能力，用户在注册LogInterface后，能够将框架日志以及TypeScript日志通过接口进行输出。

## isDebuggable

```java
public boolean isDebuggable();
```

**描述：**

确定是否允许输出调试日志。

**参数：** 

无

**返回值：** 

| 类型    | 描述                                            |
| ------- | ----------------------------------------------- |
| boolean | 如果可调试，则为真（true），否则为假（false）。 |

**示例：** 

无

## d

```java
 public void d(String tag, String msg);
```

**描述：**

用于打印调试日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## i

```java
 public void i(String tag, String msg);
```

**描述：**

用于打印信息日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## w

```java
 public void w(String tag, String msg);
```

**描述：**

用于打印警告日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## e

```java
 public void e(String tag, String msg);
```

**描述：**

用于打印错误日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## f

```java
 public void f(String tag, String msg);
```

**描述：**

用于打印致命日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | String | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

## jankLog

```java
 public void jankLog(int tag, String msg);
```

**描述：**

用于报告jank日志。

**参数：** 

| Name | 类型   | 必填 | 描述           |
| ---- | ------ | ---- | -------------- |
| tag  | int    | 是   | 消息标签。     |
| msg  | String | 是   | 要打印的消息。 |

**返回值：** 

无

**示例：** 

无

### Log日志等级定义

```java
public static final int LOG_DEBUG = 0;
public static final int LOG_INFO = 1;
public static final int LOG_WARN = 2;
public static final int LOG_ERROR = 3;
public static final int LOG_FATAL = 4;
```

**描述：**

用于设置拦截日志等级。

**参数：** 

| Name      | 类型 | 值   | 描述     | 引用方式          |
| --------- | ---- | ---- | -------- | ----------------- |
| LOG_DEBUG | int  | 0    | 调试日志 | ILogger.LOG_DEBUG |
| LOG_INFO  | int  | 1    | 信息日志 | ILogger.LOG_INFO  |
| LOG_WARN  | int  | 2    | 警告日志 | ILogger.LOG_WARN  |
| LOG_ERROR | int  | 3    | 错误日志 | ILogger.LOG_ERROR |
| LOG_FATAL | int  | 4    | 致命日志 | ILogger.LOG_FATAL |

**示例：** 

```java
this.appDelegate = new StageApplicationDelegate(); 
this.appDelegate.setLogLevel(ILogger.LOG_DEBUG);
```

## 完整示例

[完整示例](../../tutorial/how-to-use-arkui-x-loginterface-on-android.md)