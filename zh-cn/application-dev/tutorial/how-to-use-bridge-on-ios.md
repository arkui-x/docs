# 平台桥接开发指南

​	 平台桥接用于ArkUI端和Platform平台端消息通信，包括数据传输、方法调用和事件调用，ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，iOS侧参考[BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md)。

## iOS平台与ArkUI交互

### ArkUI侧

#### 创建Bridge

createBridge(bridgeName: string): BridgeObject

创建BridgeObject类。

```javascript
import bridge from '@arkui-x.bridge';

const bridgeImpl = bridge.createBridge('Bridge');
```

#### 调用方法

callMethod(methodName: string, parameters?: Record\<string, Parameter\>): Promise\<ResultValue\>

callMethod(methodName: string, ...parameters: Array\<any\>): Promise\<ResultValue\>

调用平台侧的方法。

```javascript
bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    console.log('result: ' + res);
}).catch((err) => {
    console.error('error: ' + JSON.stringify(err));
});
```

#### 注册方法

registerMethod(method: MethodData, callback: AsyncCallback\<void\>): void

registerMethod(method: MethodData): Promise\<void\>

注册ArkUI侧方法，供Android或iOS侧调用。

```javascript
function getString() {
  return 'call js getString success';
}

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

#### 注销方法

unRegisterMethod(methodName: string, callback: AsyncCallback\<void\>): void

unRegisterMethod(methodName: string): Promise\<void\>

移除已注册的ArkUI侧方法。

```javascript
function getString() {
  return 'call js getString success';
}

bridgeImpl.unRegisterMethod('getString');
```

#### 发送数据

sendMessage(message: Message, callback: AsyncCallback\<Response\>): void

sendMessage(message: Message): Promise\<Response\>

向Android或iOS侧发送数据。

```javascript
bridgeImpl.sendMessage('text').then((res)=>{
    console.log('response: ' + res);
}).catch((err) => {
    console.log('error: ' + JSON.stringify(err));
});
```

#### 设置发送数据回调

setMessageListener(callback: (message: Message) => Response)

设置回调，用于接收Android或iOS侧发送的数据。

```javascript
bridgeImpl.setMessageListener((message) => {
    console.log('receive message: ' + message);
});
```

### iOS

#### 创建Bridge

(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName instanceId:(int32_t)instanceId;

创建BridgePlugin类。

```objective-c
BridgePlugin* plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId];
```

#### 调用方法

(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName instanceId:(int32_t)instanceId;

调用ArkUI侧方法。

```objective-c
MethodData* method = [[MethodData alloc] initMethodWithName:@"getString" parameter:nil];
[self.plugin callMethod:method];
```

#### 发送数据

(void)sendMessage:(id)data;

向ArkUI侧发送数据。

```objective-c
[self.plugin sendMessage:@[@"message", @"from", @"native"]];
```

#### 注册消息监听

@property(nonatomic, weak) id <IMessageListener> messageListener;

注册回调，监听消息发送的结果。

```objective-c
#pragma mark - listener
- (id)onMessage:(id)data {
    return @"oc onMessage success";
}

- (void)onMessageResponse:(id)data {}
```

#### 注册方法返回监听

@property(nonatomic, weak) id <IMethodResult> methodResult;

注册回调，监听方法返回的结果。

```objective-c
#pragma mark - listener
- (void)onSuccess:(NSString*)methodName resultValue:(id)resultValue {}

- (void)onError:(NSString*)methodName
    errorCode:(ErrorCode)errorCode
    errorMessage:(NSString*)errorMessage {}

- (void)onMethodCancel:(NSString*)methodName {}
```

## 场景示例

##### 前端UI用例

```javascript
// Index.ets

import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  private bridgeImpl = bridge.createBridge('Bridge');
  @State helloArkUI: string = '';
  @State nativeResponse: string = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: string) => {
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
