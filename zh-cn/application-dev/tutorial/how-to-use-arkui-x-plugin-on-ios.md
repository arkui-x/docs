# Plugin生命周期开发指南

ArkUI-X插件用于拓展ArkUI应用的能力，提供管理插件生命周期的能力。本文主要介绍Android平台的ArkUI-X插件生命周期的使用。

### iOS平台创建ArkUI-X插件生命周期

在iOS平台创建ArkUI-X插件生命周期需要实现` IArkUIXPlugin `接口。

```objective-c
// PluginTest.h
#import <Foundation/Foundation.h>
#import <libarkui_ios/IArkUIXPlugin.h>

@interface PluginTest : NSObject <IArkUIXPlugin>
    // 插件注册生命周期回调声明
- (void)onRegistry:(PluginContext *)pluginContext;
	// 插件注销生命周期回调声明
- (void)onUnRegistry:(PluginContext *)pluginContext;
@end
```

```objective-c
// PluginTest.m
#import "PluginTest.h"

@interface PluginTest () {
    MyBridge * myBridge;
}
@implementation PluginTest
    // 插件注册生命周期回调实现
- (void)onRegistry:(PluginContext *)pluginContext {
    myBridge = [[MyBridge alloc] initBridgePlugin:@"Bridge" bridgeType:BINARY_TYPE 
    bridgeManager:[pluginContext getBridgePluginManager]];
    NSLog(@"PluginTest onRegistry");
}
   // 插件注销生命周期回调实现
- (void)onUnRegistry:(PluginContext *)pluginContext {
	if (myBridge) {
        myBridge = nil;
    }
    NSLog(@"PluginTest onUnRegistry");
}
@end
```

### 添加ArkUI-X插件

在StageViewController中，新增addPlugin<sup>11+</sup>方法，并以字符串形式提供IArkUIXPlugin的实现类的完整类名，用于将开发者实现IArkUIXPlugin接口的对象添加到StageViewController中。addPlugin方法声明如下：

```objective-c
// EntryEntryAbilityViewController.h
#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>

@interface EntryEntryAbilityViewController : StageViewController
@end
```

在StageViewController的viewDidLoad()中触发onRegistry()方法，通知开发者创建插件；在StageViewController的dealloc()中触发onUnRegistry()方法，通知开发者销毁插件。

**注意：开发者调用addPlugin()方法，必须位于调用超类的viewDidLoad方法之前，如下：**

```objective-c
// EntryEntryAbilityViewController.m
#import "EntryEntryAbilityViewController.h"

@interface EntryEntryAbilityViewController()
@end

@implementation EntryEntryAbilityViewController
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {}
    return self;
}

- (void)viewDidLoad {
    // 必须在[super viewDidLoad]之前添加，因为在超类的viewDidLoad中要调用addPlugin()添加的对象
    [self addPlugin:@"PluginTest"];

    [super viewDidLoad];
    self.edgesForExtendedLayout = UIRectEdgeNone;
    self.extendedLayoutIncludesOpaqueBars = YES;
}
@end
```

### 示例

Bridge 相关具体参考 [Bridge](how-to-use-bridge-on-ios.md)。

```objective-c
// PluginTest.h
#import <Foundation/Foundation.h>
#import <libarkui_ios/IArkUIXPlugin.h>

@interface PluginTest : NSObject <IArkUIXPlugin>
- (void)onRegistry:(PluginContext *)pluginContext;

- (void)onUnRegistry:(PluginContext *)pluginContext;
@end
```

```objective-c
// PluginTest.m
#import "PluginTest.h"
#import "MyBridge.h"

@interface PluginTest () {
    MyBridge * myBridge;
}
@end
@implementation PluginTest
    // 在EntryEntryAbilityViewController中调用viewDidLoad函数时被触发
- (void)onRegistry:(PluginContext *)pluginContext {
    // 创建插件以及插件初始化
    myBridge = [[MyBridge alloc] initBridgePlugin:@"Bridge" bridgeType:BINARY_TYPE 
    bridgeManager:[pluginContext getBridgePluginManager]];
    NSLog(@"PluginTest onRegistry");
}
    // 在EntryEntryAbilityViewController中调用dealloc函数时被触发
- (void)onUnRegistry:(PluginContext *)pluginContext {
    // 释放插件，回收资源
    if (myBridge) {
        myBridge = nil;
    }
    NSLog(@"PluginTest onUnRegistry");
}
@end
```

```objective-c
// MyBridge.h
#import <libarkui_ios/BridgePlugin.h>

@interface MyBridge : BridgePlugin
- (NSString *) getHelloArkUI;
@end
```

```objective-c
// MyBridge.m
#import <libarkui_ios/BridgeArray.h>
#import "MyBridge.h"
#import <UIKit/UIKit.h>

@implementation MyBridge
- (NSString *) getHelloArkUI {
    return @"Hello ArkUI!";
}
@end
```

注册插件示例如下：

```objective-c
// EntryEntryAbilityViewController.h
#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>

@interface EntryEntryAbilityViewController : StageViewController
@end
```

```objective-c
// EntryEntryAbilityViewController.m
#import "EntryEntryAbilityViewController.h"
@interface EntryEntryAbilityViewController()
@end

@implementation EntryEntryAbilityViewController
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {}
    return self;
}

- (void)viewDidLoad {
    [self addPlugin:@"PluginTest"];
    [super viewDidLoad];
    self.edgesForExtendedLayout = UIRectEdgeNone;
    self.extendedLayoutIncludesOpaqueBars = YES;
}
@end
```

