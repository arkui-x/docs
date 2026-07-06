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
| void | launchApplicationWithoutUI | 调度application初始化流程，控制不加载ArkUI，应用启动占用内存较小 |
| void | loadModule:(NSString *)moduleName entryFile:(NSString *)entryFile | 原生侧加载指定的Hap模块，需要与ArkTS侧[ModuleLoader](../apis/js-apis-ModuleLoader.md)结合使用 |

## 方法说明

### configModuleWithBundleDirectory:;

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

### launchApplication;

```
/**
* Schedule the application initialization process.
 *
 *  When the stage ability launching.
 *  The iOS project requires scheduling the application initialization process.
 *
 */+ (void)launchApplication;
```

### setLogInterface

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

### setLogLevel

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

### launchApplicationWithoutUI

```objective-c
+ (void)launchApplicationWithoutUI;
```

**描述：**

应用初始化ArkUI-X框架，控制不加载ArkUI，应用启动占用内存较小。

**参数：** 

无

**返回值：** 

无

**示例：** 

```objective-c
[StageApplication launchApplicationWithoutUI];
```

### loadModule

```objective-c
+ (void)loadModule:(NSString *)moduleName entryFile:(NSString *)entryFile;
```

**描述：**

原生侧加载指定的Hap模块，需要与ArkTS侧[ModuleLoader](../apis/js-apis-ModuleLoader.md)结合使用。

**参数：** 

| 参数名     | 类型   | 必填 | 说明                                                         |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| moduleName | String | 是   | Hap模块的模块名称。                                          |
| entryFile  | String | 是   | ArkTS侧Hap模块中自定义的[ModuleLoader](../apis/js-apis-ModuleLoader.md)实现类文件的相对路径（相对于Hap模块中ets目录）。<br>更详细的信息见[以Hap为主体的共享逻辑包开发指南-约束与限制](../../tutorial/how-to-decoupled-UI-and-Logic-on-ios.md#约束与限制)。 |

**返回值：** 

无

**示例：** 

```objective-c
// 接口使用范例
// moduleName : "entry"					   : Hap模块的模块名称为entry
// entryFile  : "./ets/MyModuleLoader.ets" : Hap模块中"MyModuleLoader.ets"的路径
[StageApplication loadModule:@"entry" entryFile:@"./ets/MyModuleLoader.ets"];
```

### destroyVm

\+ (BOOL)destroyVm;

主动销毁ArkUI引擎的运行时及关联的Worker运行时资源。

**参数：** 无

**返回值：** 

| 类型 | 说明                                            |
| ---- | ----------------------------------------------- |
| BOOL | YES 表示运行时销毁成功；NO 表示运行时销毁失败。 |

**返回值 NO 的常见原因：**

| 原因              | 说明                                         |
| ----------------- | -------------------------------------------- |
| Runtime 已销毁    | 运行时已被销毁，无需重复操作。               |
| 存在活跃的 Worker | 当前仍有Worker线程在运行，运行时无法被销毁。 |

**示例：** 

  ```objective-c
// XXX.m iOS原生业务逻辑文件
#import "XXX.h"
#import <libarkui_ios/StageApplication.h>

- (void)destroy{
    // 从ArkUI页面返回时，主动销毁运行时，释放资源。
    BOOL result = [StageApplication destroyVm];
    if (!result) {
        // 销毁失败，可记录日志或稍后重试。
        NSLog(@"destroyVm failed, ArkTS runtime may still have active workers.");
    }
}
  ```

