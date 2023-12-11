# 平台桥接开发指南

​平台桥接用于客户端（ArkUI）和平台（Android或iOS）之间传递消息，即用于ArkUI与平台双向数据传递、ArkUI侧调用平台的方法、平台调用ArkUI侧的方法。本文主要介绍iOS平台与ArkUI交互，ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，iOS侧参考[BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md)。

## iOS平台与ArkUI交互

### 创建平台桥接

1、在ArkUI侧创建平台桥接。指定名称，该名称应与iOS侧平台桥接的名称一致。通过创建的该对象即可调用平台桥接的方法。

```javascript
// xxx.ets

// 导入平台桥接模块
import bridge from '@arkui-x.bridge';

// 创建平台桥接实例
const bridgeImpl = bridge.createBridge('Bridge');
```

2、在iOS侧创建BridgePlugin类。指定名称，该名称应与ArkUI侧平台桥接的名称一致。通过创建的该对象即可调用平台桥接的方法。

```objective-c
// xxx.mm

BridgePlugin* plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId];
```

### 场景一：ArkUI侧向iOS侧传递数据

1、ArkUI侧向iOS侧传递数据。

```javascript
// xxx.ets

bridgeImpl.sendMessage('text').then((res)=>{
    console.log('response: ' + res);
}).catch((err) => {
    console.log('error: ' + JSON.stringify(err));
});
```

2、iOS侧接收来自ArkUI侧的数据。

```objective-c
// xxx.mm

#pragma mark - listener
// 注册回调，监听ArkUI侧的数据传递
- (id)onMessage:(id)data {
    // 返回回执给ArkUI侧
    return @"objective-c onMessage success";
}
```

### 场景二：iOS侧向ArkUI侧传递数据

1、iOS侧向ArkUI侧发送数据。

```objective-c
// xxx.mm

[self.plugin sendMessage:@[@"message", @"from", @"ios"]];
```

2、ArkUI侧设置回调，用于接收iOS侧发送的数据。

```javascript
// xxx.ets

bridgeImpl.setMessageListener((message) => {
    console.log('receive message: ' + message);
});
```

3、iOS侧注册回调，监听ArkUI侧收到数据后的回执。

```objective-c
// xxx.mm

#pragma mark - listener
// 注册回调，监听ArkUI侧的回执
- (void)onMessageResponse:(id)data {}
```

### 场景三：ArkUI侧调用iOS侧的方法

1、在ArkUI侧调用iOS侧的方法。

```javascript
// xxx.ets

bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    console.log('result: ' + res);
}).catch((err) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在iOS侧实现被调用的方法。

```objective-c
// xxx.mm

@interface Bridge : BridgePlugin
- (NSString*)platformCallMethod;
@end

@implementation Bridge
- (NSString*)platformCallMethod {
    return @"call objective-c platformCallMethod success";
}
@end
```


### 场景四：iOS侧调用ArkUI侧的方法

1、注册ArkUI侧方法，供iOS侧调用。

```javascript
// xxx.ets

function getString() {
  return 'call js getString success';
}

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2、iOS侧调用ArkUI侧的方法。

```objective-c
// xxx.mm

MethodData* method = [[MethodData alloc] initMethodWithName:@"getString" parameter:nil];
[self.plugin callMethod:method];
```


### 场景五：ArkUI侧监听iOS侧的方法

1、注册ArkUI侧方法，供iOS侧调用。

```javascript
// xxx.ets

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2、移除已注册的ArkUI侧方法。

```javascript
// xxx.ets

bridgeImpl.unRegisterMethod('getString');
```

3、在iOS侧注册回调，监听方法注册、注销。

```objective-c
// xxx.mm

#pragma mark - listener
- (void)onSuccess:(NSString*)methodName resultValue:(id)resultValue {}

- (void)onError:(NSString*)methodName
    errorCode:(ErrorCode)errorCode
    errorMessage:(NSString*)errorMessage {}

- (void)onMethodCancel:(NSString*)methodName {}
```

## 场景示例

##### ArkUI用例

本场景展示了当ArkUI页面启动时，调用iOS侧方法，并将iOS侧方法的返回值显示在页面上。点击按钮，ArkUI侧发送数据到iOS侧，iOS侧收到数据后，返回响应数据，并将响应数据显示在页面上。

```javascript
// Index.ets

// 导入平台桥接模块
import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  // 创建平台桥接对象
  private bridgeImpl = bridge.createBridge('Bridge');
  @State helloArkUI: string = '';
  @State nativeResponse: string = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    // 调用iOS侧方法
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: string) => {
      // 通过状态变量，将iOS侧方法的返回值显示在页面上
      this.helloArkUI = result;
    });
  }

  build() {
    Row() {
      Column() {
        Text(this.helloArkUI)
          .fontSize(15)
          .margin(10)
        Button('sendMessage')
          .fontSize(15)
          .margin(10)
          .onClick(async () => {
            // 发送数据到iOS侧，并通过状态变量，将iOS侧的响应数据显示在页面上
            this.nativeResponse = await this.bridgeImpl.sendMessage('Hello ArkUI-X!');
          })
        Text('Response from Native: ' + this.nativeResponse)
          .fontSize(15)
          .margin(10)
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

##### iOS用例

```objective-c
// BridgeClass.h

// 引用平台桥接模块
#import <libarkui_ios/BridgePlugin.h>

NS_ASSUME_NONNULL_BEGIN

@interface BridgeClass : BridgePlugin
- (NSString*)getHelloArkUI;
@end

NS_ASSUME_NONNULL_END
```

```objective-c
// BridgeClass.m

#import "BridgeClass.h"

// iOS侧方法，供ArkUI侧调用
@implementation BridgeClass
- (NSString*)getHelloArkUI {
    return @"Hello ArkUI!";
}
@end
```

```objective-c
// AppDelegate.h

#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>
@property (nonatomic, strong) UIWindow* window;
@end
```

```objective-c
// AppDelegate.m

#import "AppDelegate.h"
#import <libarkui_ios/StageApplication.h>
#import <libarkui_ios/BridgePlugin.h>
#import "EntryMainViewController.h"
#import "BridgeClass.h"

#define BUNDLE_DIRECTORY @"arkui-x"
#define BUNDLE_NAME @"com.example.bridgestage"

@interface AppDelegate ()
<IMessageListener> {}
@property (nonatomic, strong) BridgeClass* plugin;
@end

@implementation AppDelegate
- (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions {
    [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
    [StageApplication launchApplication];

    NSString* instanceName = [NSString stringWithFormat:@"%@:%@:%@",BUNDLE_NAME, @"entry", @"MainAbility"];
    EntryMainViewController *mainView = [[EntryMainViewController alloc] initWithInstanceName:instanceName];
    [self setNavRootVC:mainView];

    int32_t instanceId = [mainView getInstanceId];

    // 建立与ArkUI侧同名的平台桥接，即可用于消息传递
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instanceId];
    self.plugin.messageListener = self;
    return YES;
}

- (void)setNavRootVC:(id)viewController {
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    self.window.rootViewController = viewController;
}

// 监听ArkUI侧发来的消息
#pragma mark - listener
- (NSString*)onMessage:(id)data {
    return @"oc onMessage success";
}

- (void)onMessageResponse:(id)data {}
@end
```

```objective-c
// EntryMainViewController.h

#ifndef EntryMainViewController_h
#define EntryMainViewController_h

#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>

@interface EntryMainViewController : StageViewController
@end

#endif
```

```objective-c
// EntryMainViewController.m

#import "EntryMainViewController.h"

@interface EntryMainViewController ()
@end

@implementation EntryMainViewController
- (instancetype)initWithInstanceName:(NSString*)instanceName {
    self = [super initWithInstanceName:instanceName];
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
}
@end
```

```objective-c
// main.m

#import <UIKit/UIKit.h>
#import "AppDelegate.h"

int main(int argc, char * argv[]) {
    NSString * appDelegateClassName;
    @autoreleasepool {
        appDelegateClassName = NSStringFromClass([AppDelegate class]);
    }
    return UIApplicationMain(argc, argv, nil, appDelegateClassName);
}
```
