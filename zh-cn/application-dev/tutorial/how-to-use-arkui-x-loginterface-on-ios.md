# ArkUI-X框架LogInterface使用指南

ArkUI-X框架支持日志拦截能力，iOS侧提供原生接口，用于注入LogInterface接口，框架日志及ts日志通过该接口输出，本文的核心内容是介绍如何在iOS平台上有效利用ArkUI-X框架的LogInterface，拦截日志。

## iOS平台创建ArkUI-X框架LogInterface

在iOS平台创建ArkUI-X框架LogInterface需要实现[ILogger](../reference/arkui-for-ios/ILogger.md)接口，实现声明接口完整示例如下：

```objective-c
//AppDelegate.m

#import <libarkui_ios/StageApplication.h>
#import <libarkui_ios/ILogger.h>
  
@interface AppDelegate ()<ILogger>

@end

@implementation AppDelegate
- (void)d:(nonnull NSString *)tag msg:(nonnull NSString *)msg {
    //对日志信息处理，落盘或输出
}

- (void)e:(nonnull NSString *)tag msg:(nonnull NSString *)msg {
    //对日志信息处理，落盘或输出
}

- (void)f:(nonnull NSString *)tag msg:(nonnull NSString *)msg {
    //对日志信息处理，落盘或输出
}

- (void)i:(nonnull NSString *)tag msg:(nonnull NSString *)msg {
    //对日志信息处理，落盘或输出
}

- (void)w:(nonnull NSString *)tag msg:(nonnull NSString *)msg {
    //对日志信息处理，落盘或输出
}
```

## 设置ArkUI-X框架LogInterface以及日志拦截等级

​		在需要控制ArkUI-X框架日志及TypeScript日志的输出时，可以利用[StageApplication](../reference/arkui-for-ios/StageApplication.md)类中[setLogInterface](../reference/arkui-for-ios/StageApplication.md)方法来注入LogInterface，注入成功，框架和TypeScript的ERROR和FATAL日志通过提供的这个实例的方法输出，注入失败，执行日志输出原逻辑。

​		设置日志拦截等级需使用[StageApplication](../reference/arkui-for-ios/StageApplication.md)类中[setLogLevel](../reference/arkui-for-ios/StageApplication.md)方法，设置日志拦截等级成功，日志等级优先级低于该日志拦截等级时，日志不被输出。

设置ArkUI-X框架LogInterface以及日志拦截等级，完整示例如下：

```objective-c
//AppDelegate.m

#import <libarkui_ios/StageApplication.h>
#import <libarkui_ios/ILogger.h>
  
@interface AppDelegate ()<ILogger>

@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [StageApplication setLogInterface:self];//设置LogInterface
    [StageApplication setLogLevel:LOG_INFO];//设置日志拦截等级

   	return YES;
}
```







