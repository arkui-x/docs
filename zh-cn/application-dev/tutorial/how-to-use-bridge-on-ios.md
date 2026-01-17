# 平台桥接开发指南

平台桥接用于客户端（ArkUI）和平台（Android或iOS）之间传递消息，即用于ArkUI与平台双向数据传递、ArkUI侧调用平台的方法、平台调用ArkUI侧的方法。本文主要介绍iOS平台与ArkUI交互，ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，iOS侧参考[BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md)。<br>

## iOS平台与ArkUI交互

### 创建平台桥接

平台桥接要求ArkUI端和原生端(iOS)两端都创建Bridge对象后才可以进行通信流程。任意一端的Bridge对象未创建完成时，平台桥接的功能无法正常使用。<br>
在SDK版本6.0.2.118之后，对Bridge的优化主要包括解除Bridge与ViewController生命周期的绑定、移除instanceId依赖、优化构造方法、并将Bridge的执行移至子线程。<br>

#### SDK 版本小于6.0.2.118

##### 创建时机

- ArkUI端：Bridge 对象创建时机必须在UIAbility onCreate()函数内部，或在该函数执行完毕后。<br>

  ```tsx
  // EntryAbility.ets
  
  import bridge from '@arkui-x.bridge';
  
  export default class EntryAbility extends UIAbility {
    onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
  
      // 创建平台桥接实例(默认为JSON模式)
      const bridgeImplForJson: bridge.BridgeObject = bridge.createBridge('BridgeImplForJson');
      // 创建平台桥接实例(二进制模式)
      const bridgeImplForBinary: bridge.BridgeObject = bridge.createBridge('BridgeImplForBinary', bridge.BridgeType.BINARY_TYPE);
    }
  }
  ```
  
- 原生端(iOS)：Bridge 对象创建时机必须在ViewController viewDidLoad()函数内部，或在该函数执行完毕后。<br>

  ```objective-c
  // xxx.mm
  
  - (void)viewDidLoad {
      [super viewDidLoad];
  
      self.edgesForExtendedLayout = UIRectEdgeNone;
      self.extendedLayoutIncludesOpaqueBars = YES;
  
      // 构造方法1. 通过bridgeManager 构造
      self.pluginForJson = [[BridgeClass alloc] initBridgePlugin:@"BridgeImplForJson" bridgeManager:[self getBridgeManager]];
  
      // 构造方法2. 通过bridgeType、bridgeManager构造，bridgeType设置编解码类型
      self.pluginForBinary = [[BridgeClass alloc] initBridgePlugin:@"BridgeImplForBinary" bridgeManager:[self getBridgeManager] bridgeType:BINARY_TYPE];
  
      // 指定代理
      self.pluginForJson.messageListener = self;
      self.pluginForJson.methodResult = self;
      self.pluginForBinary.messageListener = self;
      self.pluginForBinary.methodResult = self;
  }
  ```

##### 接口调用时机

- ArkUI端：Bridge 接口调用时机必须在UIAbility onWindowStageCreate()函数执行完毕后。具体示例代码参考下文场景示例。<br>
- 原生端(iOS)：Bridge 接口调用时机必须在ArkUI端UIAbility onWindowStageCreate()函数执行完毕后。具体示例代码参考下文场景示例。<br>

#### SDK 版本大于6.0.2.118

##### 创建时机

- ArkUI端：Bridge 对象创建时机必须在UIAbility onCreate()函数内部，或在该函数执行完毕后。<br>

  ```tsx
  // EntryAbility.ets
  
  // 导入平台桥接模块
  import bridge from '@arkui-x.bridge';
  
  export default class EntryAbility extends UIAbility {
    onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
  
      // 创建平台桥接实例(默认为JSON模式)
      const bridgeImplForJson: bridge.BridgeObject = bridge.createBridge('BridgeImplForJson');
      // 创建平台桥接实例(二进制模式)
      const bridgeImplForBinary: bridge.BridgeObject = bridge.createBridge('BridgeImplForBinary', bridge.BridgeType.BINARY_TYPE);
    }
  }
  ```

- 原生端(iOS)：Bridge 对象创建时机必须在application: didFinishLaunchingWithOptions()函数内部，或在该函数执行完毕后。<br>

  ```objective-c
  // xxx.mm
  
  - (void)viewDidLoad {
      [super viewDidLoad];
  
      self.edgesForExtendedLayout = UIRectEdgeNone;
      self.extendedLayoutIncludesOpaqueBars = YES;
  
      // 构造方法
      self.pluginForJson = [[BridgeClass alloc] initBridgePlugin:@"BridgeImplForJson" bridgeType:JSON_TYPE];
      self.pluginForBinary = [[BridgeClass alloc] initBridgePlugin:@"BridgeImplForBinary" bridgeType:BINARY_TYPE];
  
      // 指定代理
      self.pluginForJson.messageListener = self;
      self.pluginForJson.methodResult = self;
      self.pluginForBinary.messageListener = self;
      self.pluginForBinary.methodResult = self;
  }
  ```
  

##### 接口调用时机

- ArkUI端：Bridge 接口调用时机必须在UIAbility onCreate()函数执行完毕后。具体示例代码参考下文场景示例。<br>
- 原生端(iOS)：Bridge 接口调用时机必须在application: didFinishLaunchingWithOptions()函数内部，或在该函数执行完毕后。具体示例代码参考下文场景示例。<br>



以下示例代码**基于SDK版本大于等于6.0.2.118**编写，原生端(iOS)使用的是使用该[Bridge构造方法](../reference/arkui-for-ios/BridgePlugin.md#initbridgeplugin22)。<br>


### 场景一：ArkUI侧向iOS侧传递数据

1、ArkUI侧向iOS侧传递数据。

```javascript
// xxx.ets

bridgeImpl.sendMessage('text').then((res)=>{
    console.log('response: ' + res);
}).catch((err: Error) => {
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

    // 收到消息后，向iOS侧发送回执
    return "ArkUI receive message success";
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
}).catch((err: Error) => {
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

### 场景六：ArkUI侧注册callBack且调用iOS侧的方法（无参）

1、在ArkUI侧注册callBack且调用iOS侧的方法。

```javascript
// xxx.ets
function testCallBackOfJs():bridge.ResultValue {
  return "bridge js testCallBack run";
}

this.bridgeCodec.callMethodWithCallBack("testCallBack", testCallBackOfJs).then((res)=>{
    console.log('result: ' + res);
}).catch((err:Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在iOS侧实现被调用的方法，调用ArkUI侧的方法。

```objective-c
// xxx.mm
@interface Bridge : BridgePlugin
- (NSString*)testCallBack;
@end

@implementation Bridge
- (NSString*)testCallBack {
    return @"call objective-c platformCallMethod success";
}
@end

MethodData* method = [[MethodData alloc] initMethodWithName:@"testCallBack" parameter:nil];
[self.plugin callMethod:method];
```

### 场景七：ArkUI侧注册callBack且调用iOS侧的方法（有参）

1、在ArkUI侧注册callBack且调用iOS侧的方法。

```javascript
// xxx.ets
function testCallBackOfJs(stringParam:string) {
  console.log("Js received a parameter of " + stringParam)
  return "js testCallBackReturn call success."
}

this.bridgeCodec.callMethodWithCallBack("testCallBack", testCallBackOfJs, "js sends parameter").then((res)=>{
    console.log('result: ' + res);
}).catch((err:Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在iOS侧实现被调用的方法，调用ArkUI侧的方法。

```objective-c
// xxx.mm
@interface Bridge : BridgePlugin
- (NSString*)testCallBack:(id)param;
@end

@implementation Bridge
- (NSString*)testCallBack:(id)param {
    return @"call objective-c platformCallMethod success";
}
@end

MethodData* method = [[MethodData alloc] initMethodWithName:@"testCallBack" parameter:@[@"ios sends parameter"]];
[self.plugin callMethod:method];
```
### 场景八：callMethod不同数据类型

1、在ArkUI侧注册callBack且调用iOS侧的方法。

```javascript
// xxx.ets
import bridge from '@arkui-x.bridge'

@Entry
@Component
struct Index {
  @State bridgeImpl: bridge.BridgeObject = bridge.createBridge("BridgeName");

  private funTest(p1: string, p2: number, p3: boolean) : bridge.ResultValue{
    console.info('IOS->Ts bridge funTest p1 is ' + p1);
    console.info('IOS->Ts bridge funTest p2 is ' + p2);
    console.info('IOS->Ts bridge funTest p3 is ' + p3);
    return "call success";
  }

  private funTestArray(p1: Array<string>, p2: Array<number>, p3: Array<boolean>): bridge.ResultValue {
      console.log('IOS->Ts bridge funTestArray p1 is ' + p1.toString());
      console.log('IOS->Ts bridge funTestArray p2 is ' + p2.toString());
      console.log('IOS->Ts bridge funTestArray p3 is ' + p3.toString());
      return "call success";
  }

  private funTestRecord(p1: Record<string, string>, p2: Record<string, number>, p3: Record<string, boolean>) : bridge.ResultValue {
    Object.keys(p1).forEach(key => {
      console.log(`IOS->Ts bridge funTestRecord p1 is Key: ${key}, Value: ${p1[key]}`);
    });
    Object.keys(p2).forEach(key => {
      console.log(`IOS->Ts bridge funTestRecord p2 is Key: ${key}, Value: ${p1[key]}`);
    });
    Object.keys(p3).forEach(key => {
      console.log(`IOS->Ts bridge funTestRecord p3 is Key: ${key}, Value: ${p1[key]}`);
    });
    return "call success";
  }

  onPageShow() {
    // Register JavaScript functions
    this.bridgeImpl.registerMethod({name: "funTest", method: this.funTest});
    this.bridgeImpl.registerMethod({name: "funTestArray", method: this.funTestArray});
    this.bridgeImpl.registerMethod({name: "funTestRecord", method: this.funTestRecord});
  }

  build() {
    Row() {
      Column() {
          
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

2、在iOS侧实现被调用的方法，调用ArkUI侧的方法。

```objective-c
// .h
#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>

NS_ASSUME_NONNULL_BEGIN

@interface EntryAbilityViewController : StageViewController
@end
  
// .mm
#import "EntryAbilityViewController.h"
#import <libarkui_ios/BridgePlugin.h>
#import "BridgeClass.h"

@interface EntryAbilityViewController ()<IMessageListener, IMethodResult>
@property (nonatomic, strong) BridgeClass* plugin;
@end

@implementation EntryAbilityViewController
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {
        //初始化plugin
        [self initBridgePlugin];
    }
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    for (UIView * view in self.view.subviews) {
        if ([NSStringFromClass([view class]) isEqualToString:@"WindowView"]) {
            [self.view insertSubview:view atIndex:0];
        }
    }
    
  //添加UI
    UIButton * buttonMethond1 = [[UIButton alloc] initWithFrame:CGRectMake(100, 100, 150, 40)];
    [buttonMethond1 setTitle:@"testCallMethod1" forState:UIControlStateNormal];
    [buttonMethond1 setTitleColor:UIColor.whiteColor forState:UIControlStateNormal];
    [buttonMethond1 setBackgroundColor:UIColor.blueColor];
    [buttonMethond1 addTarget:self action:@selector(testCallMethod1) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:buttonMethond1];
    
    UIButton * buttonMethond2 = [[UIButton alloc] initWithFrame:CGRectMake(100, CGRectGetMaxY(buttonMethond1.frame) + 20, 150, 40)];
    [buttonMethond2 setTitle:@"testCallMethod2" forState:UIControlStateNormal];
    [buttonMethond2 setTitleColor:UIColor.whiteColor forState:UIControlStateNormal];
    [buttonMethond2 setBackgroundColor:UIColor.blueColor];
    [buttonMethond2 addTarget:self action:@selector(testCallMethod2) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:buttonMethond2];
    
    UIButton * buttonMethond3 = [[UIButton alloc] initWithFrame:CGRectMake(100, CGRectGetMaxY(buttonMethond2.frame) + 20, 150, 40)];
    [buttonMethond3 setTitle:@"testCallMethod3" forState:UIControlStateNormal];
    [buttonMethond3 setTitleColor:UIColor.whiteColor forState:UIControlStateNormal];
    [buttonMethond3 setBackgroundColor:UIColor.blueColor];
    [buttonMethond3 addTarget:self action:@selector(testCallMethod3) forControlEvents:UIControlEventTouchUpInside];
    [self.view addSubview:buttonMethond3];
   
}

- (void)initBridgePlugin {

//  构造方法. 通过bridgeManager 构造
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"BridgeName" bridgeManager:[self getBridgeManager]];

//  添加代理
    self.plugin.messageListener = self;
    self.plugin.methodResult = self;
}

#pragma mark -- callMethod
//调用Arkui 测注册的方法，调用之前要实现methodResult代理，通过代理回调的到调用结果 注意method name 必须与Arkui测方法名保持一致

//调用JS测注册的funTest方法
- (void)testCallMethod1 {
    MethodData * method = [[MethodData alloc] initMethodWithName:@"funTest" parameter:@[@"Hello ArkUI-X!", @1, @true]];
    [self.plugin callMethod:method];
}

//调用JS测注册的funTestArray方法
- (void)testCallMethod2 {
    MethodData * method = [[MethodData alloc] initMethodWithName:@"funTestArray" parameter:@[@[@"Hello",@"ArkUI-X!"], @[@123, @456], @[@true, @false]]];
  //调用JS测注册的funTest方法
    [self.plugin callMethod:method];
}

//调用JS测注册的funTestRecord方法
- (void)testCallMethod3 {
    MethodData * method = [[MethodData alloc] initMethodWithName:@"funTestRecord" parameter:@[@{@"one": @"hello", @"two": @"world"}, @{@"one": @1, @"two": @2}, @{@"one": @true, @"two": @false}]];
    [self.plugin callMethod:method];
}

#pragma mark - IMessageListener ArkUI侧发送的消息代理实现
/**
  *  Arkui测调用sendMessage 发送消息到IOS测，将会触发次方法的回调
  *  @param data Arkui 传递过来的数据
  *   return 返回值传递传递给Arkui测
*/
- (NSString*)onMessage:(id)data {
//    NSLog(@"Arkui 传递消息给iOS message: %@", data);
    return @"ios onMessage respone message";
}

/**
 * iOS 通过sendMessage 方法发送消息到Arkui, Arkui成功接到消息后回调此方法传递结果
 * @param data Arkui 返回的信息
*/
- (void)onMessageResponse:(id)data {
    //NSLog(@"Arkui onMessageResponse: %@", data);
}

#pragma mark - IMethodResult 用于监听平台侧调用ArkUI侧注册的方法的执行情况
/**
  *  ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。
  *  @param methodName ArkUI侧方法的名称
  *  @param resultValue ResultValue 类 ，并将ArkUI侧方法的返回值传递给平台侧
*/
- (void)onSuccess:(nonnull NSString *)methodName resultValue:(nonnull id)resultValue {
    //NSLog(@"%@", [NSString stringWithFormat:@"methondName:%@, respone: %@",methodName, resultValue]);
}

/**
  *  平台侧调用ArkUI侧定义方法时，如果出错则触发该接口，并将出错信息返回平台侧。具体错误码查看ResultValue类中错误码或者接口文档
  *  @param methodName ArkUI侧方法的名称
  *  @param errorCode 错误码
  *  @param errorMessage 错误信息
*/
- (void)onError:(nonnull NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(nonnull NSString *)errorMessage {
    //NSLog(@"iOS call arkui method fail, methtod name: %@ errorCode: %d errorMessage: %@",methodName, errorCode, errorMessage);
}

/**
  *  ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。
  *  @param methodName ArkUI侧方法的名称
*/
- (void)onMethodCancel:(nonnull NSString *)methodName {
    //NSLog(@"iOS call arkui method cancel, methtod name: %@", methodName);
}

@end
```

### 场景九：ArkUI侧同步调用iOS侧的方法

1、在ArkUI侧调用iOS侧的方法。

```javascript
// xxx.ets

try {
  let data: bridge.ResultValue = bridgeImpl.callMethodSync('funcWithString', 'text')
  console.info(LOG_TAG + 'callMethodSync success; data ：： ' + data)
} catch (error) {
  console.error(LOG_TAG + `callMethodSync failed, error: ${error.toString()}`)
}
```

2、在iOS侧实现被调用的方法。

```objective-c
// xxx.mm

@interface Bridge : BridgePlugin
- (NSString *)funcWithString:(NSString *)param1;
@end

@implementation Bridge
- (NSString *)funcWithString:(NSString *)param1 {
    return @"call objective-c funcWithString success";
}
@end
```

### 场景十：iOS侧同步调用ArkUI侧的方法

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

id data = nil;
data = [self.plugin callMethodSync:@"getString" parameters: nil];
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
  @State helloArkUI: bridge.ResultValue = '';
  @State nativeResponse: bridge.Message = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    // 调用iOS侧方法
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: bridge.ResultValue) => {
      // 通过状态变量，将iOS侧方法的返回值显示在页面上
      this.helloArkUI = result;
    });
  }

  build() {
    Row() {
      Column() {
        Text(this.helloArkUI?.toString())
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

    // 建立与ArkUI侧同名的平台桥接，即可用于消息传递
	// 创建平台桥接实例(将在since 13废弃，推荐使用新构造方法)
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instanceId];
    // 创建平台桥接实例
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[mainView getBridgeManager]];

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
