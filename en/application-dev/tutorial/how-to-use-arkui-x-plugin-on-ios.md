# How to Use ArkUI-X Plugins on iOS

ArkUI-X provides APIs for managing the lifecycle of ArkUI-X plugins, which are used to expand the capabilities of ArkUI applications. This topic describes how to use the ArkUI-X plugins on the iOS platform.

### Creating an ArkUI-X Plugin

To create an ArkUI-X plugin that can run on iOS devices, implement the **IArkUIXPlugin** API.

```objective-c
// pluginTest.h
#import <Foundation/Foundation.h>
#import <libarkui_ios/IArkUIXPlugin.h>

@interface PluginTest : NSObject <IArkUIXPlugin>
    // Declare the callback invoked when a plugin is registered.
- (void)onRegistry:(PluginContext *)pluginContext;
	// Declare the callback invoked when a plugin is unregistered.
- (void)onUnRegistry:(PluginContext *)pluginContext;
@end
```

```objective-c
// pluginTest.m
#import "PluginTest.h"

@interface PluginTest () {
    MyBridge * myBridge;
}
@implementation PluginTest
    // Implement the callback invoked when a plugin is registered.
- (void)onRegistry:(PluginContext *)pluginContext {
    myBridge = [[MyBridge alloc] initBridgePlugin:@"Bridge" bridgeType:BINARY_TYPE 
    bridgeManager:[pluginContext getBridgePluginManager]];
    NSLog(@"PluginTest onRegistry");
}
   // Implement the callback invoked when a plugin is unregistered.
- (void)onUnRegistry:(PluginContext *)pluginContext {
	if (myBridge) {
        myBridge = nil;
    }
    NSLog(@"PluginTest onUnRegistry");
}
@end
```

### Adding an ArkUI-X Plugin

Call **addPlugin<sup>11+</sup>** in **StageViewController** to add the object of the **IArkUIXPlugin** implementation class (whose complete class name is in the form of a string) to the StageViewController. Declare **addPlugin** as follows:

```objective-c
// EntryEntryAbilityViewController.h
#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>

@interface EntryEntryAbilityViewController : StageViewController
@end
```

Use **onRegistry()** in the **viewDidLoad()** method of the StageViewController to instruct the application to create a plugin. Use **onUnRegistry()** in the **dealloc()** method of the StageViewController to instruct the application to destroy the plugin.

**NOTE: Call addPlugin() in prior to viewDidLoad() of the superclass.**

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
    // The plugin must be added by using addPlugin() before super viewDidLoad() because the plugin will be called in viewDidLoad() of the superclass.
    [self addPlugin:@"PluginTest"];

    [super viewDidLoad];
    self.edgesForExtendedLayout = UIRectEdgeNone;
    self.extendedLayoutIncludesOpaqueBars = YES;
}
@end
```

### Example

For details about bridge, see [Bridge](how-to-use-bridge-on-ios.md).

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
    // Called in viewDidLoad() of EntryEntryAbilityViewController.
- (void)onRegistry:(PluginContext *)pluginContext {
    // Create a plugin and initialize it.
    myBridge = [[MyBridge alloc] initBridgePlugin:@"Bridge" bridgeType:BINARY_TYPE 
    bridgeManager:[pluginContext getBridgePluginManager]];
    NSLog(@"PluginTest onRegistry");
}
    // Called in dealloc() of EntryEntryAbilityViewController.
- (void)onUnRegistry:(PluginContext *)pluginContext {
    // Release the plugin.
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

Sample code for plugin registration:

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

