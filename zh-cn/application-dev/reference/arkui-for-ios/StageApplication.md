# StageApplication

```
NSObject
    └── StageApplication
```

StageApplication负责iOS应用初始化流程。

## 方法概要

| 类型         | 方法                 | 描述   |
| ------------ | -------------------- | ------ |
| void | configModuleWithBundleDirectory | 配置工程内Hap包路径 |
| void | launchApplication | 调度application初始化流程 |
| void | setLogInterface(ILogger log) | 注册LogInterface来截断日志 |
| void | setLogLevel(int logLevel) | 设置输出日志的输出等级，低于该等级的日志被拦截，不再输出 |

## 方法说明


+ configModuleWithBundleDirectory:;

```
/**
* Configure the Hap package path in the iOS project.
 *
 *  Configure the Hap package path : bundleDirectory
 *  This path is within the iOS project.
 *
 * @param bundleDirectory hap path.
 */+ (void)configModuleWithBundleDirectory:(NSString *_Nonnull)bundleDirectory;
```

+ launchApplication;

```
/**
* Schedule the application initialization process.
 *
 *  When the stage ability launching.
 *  The iOS project requires scheduling the application initialization process.
 *
 */+ (void)launchApplication;
```

- setLogInterface

```objective-c
/**
* Set log interface.
*
* @param logger the log interface.
*/

+ (void)setLogInterface:(id)log;
```

**描述：**

设置日志接口。

**参数：** 

| Name | 类型 | 必填 | 描述                    |
| ---- | ---- | ---- | ----------------------- |
| log  | id   | 是   | 实现ILogger接口的对象。 |

**返回值：** 

无

**示例：** 

```objective-c
[StageApplication setLogInterface:self];
```

- setLogLevel

```objective-c
/**
* Set log level.
*
* @param logLevel the log level.
*/
+ (void)setLogLevel:(int)logLevel;
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

```objective-c
[StageApplication setLogLevel:LOG_INFO];
```

