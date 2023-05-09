

# Bridge

​	 Bridge用于ArkUI端和Platform平台端消息通信，包括数据传输、方法调用和事件调用，ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，Android侧参考[BridgePlugin](../reference/arkui-for-android/BridgePlugin.md)，iOS侧参考[BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md)。

## 开发指导

### ArkUI侧

#### 创建Bridge

createBridge(bridgeName: string): BridgeObject

在main/ets/MainAbility/pages目录下的ets文件中定义BridgeObject类。

```javascript
// xxx.ets

import bridge from '@arkui-x.bridge';

const bridgeImpl = bridge.createBridge("Bridge");
```

#### 调用方法

callMethod(methodName: string, parameters?: Record\<string, Parameter\>): Promise\<ResultValue\>

callMethod(methodName: string, ...parameters: Array\<any\>): Promise\<ResultValue\>

在main/ets/MainAbility/pages目录下的ets文件中调用平台方法。

```javascript
// xxx.ets

bridgeImpl.callMethod("platformCallMethod").then((data)=>{
    console.log("data = " + data);
}).catch((err) => {
    console.error("error = " + JSON.stringify(err));
});
```

#### 注册方法

registerMethod(method: MethodData, callback: AsyncCallback\<void\>): void

registerMethod(method: MethodData): Promise\<void\>

在main/ets/MainAbility/pages目录下的ets文件中注册JS侧方法，供Android或iOS侧调用。

```javascript
// xxx.ets

function getString() 
  return "call js getString success";
}

bridgeImpl.registerMethod({name: "getString", method: getString});
```

#### 注销方法

unRegisterMethod(methodName: string, callback: AsyncCallback\<void\>): void

unRegisterMethod(methodName: string): Promise\<void\>

在main/ets/MainAbility/pages目录下的ets文件中移除已注册的JS方法。

```javascript
// xxx.ets

function getString() 
  return "call js getString success";
}

bridgeImpl.unRegisterMethod("getString");
```

#### 发送数据

sendMessage(message: Message, callback: AsyncCallback\<Response\>): void

sendMessage(message: Message): Promise\<Response\>

在main/ets/MainAbility/pages目录下的ets文件中向Android或iOS侧发送数据。

```javascript
// xxx.ets

bridgeImpl.sendMessage('text').then((data)=>{
    console.log("data =" + data);
}).catch((err) => {
    console.log("error =" + JSON.stringify(err));
});
```

#### 设置发送数据回调

setMessageListener(callback: (message: Message) => Response)

在main/ets/MainAbility/pages目录下的ets文件中设置用于接收Android或iOS侧发送数据的回调。

```javascript
// xxx.ets

bridgeImpl.setMessageListener((data) => {
    console.log("receive data =" + data);
});
```

### Android

#### 创建通道

BridgePlugin(Context context, String bridgeName, int instanceId)

创建BridgePlugin类。

```java
//	EntryMainActivity.java

Bridge bridge = new Bridge(this, "Bridge", getInstanceId());
```

#### 调用方法

public void callMethod(MethodData methodData)

在main/ets/MainAbility/pages目录下的MainActivity.java文件中创建BridgePlugin类。

```java
//	EntryMainActivity.java

Object[] paramObject = {"I", "just", "made", "it"};
MethodData methodData = new MethodData("javaCallMethod", paramObject);
bridge.callMethod(methodData);
```

#### 发送数据

public void sendMessage(Object data)

在main/ets/MainAbility/pages目录下的MainActivity.java文件中向js侧发送数据。

```java
//	EntryMainActivity.java

String[] data = {"I", "just", "made", "it"};
bridge.sendMessage(data);
```

#### 注册消息监听

public void setMessageListener(IMessageListener messageListener)

在main/ets/MainAbility/pages目录下的MainActivity.java文件中注册消息监听。

```java
// Bridge.java

public Bridge(Context context, String name, int id) {
    super(context, name, id);
    this.name = name;
    setMessageListener(this);
}

@Override
public Object onMessage(Object data) {
    ALog.i("onMessage data:", data.toString());
    return jsonObject;
}
@Override
public void onMessageResponse(Object data) {
    ALog.i("onMessageResponse data:", data.toString());
}
```

#### 注册方法返回监听

public void setMethodResultListener(IMethodResult methodResultListener)

在main/ets/MainAbility/pages目录下的MainActivity.java文件中注册方法返回监听

```java
// Bridge.java

public Bridge(Context context, String name, int id) {
    super(context, name, id);
    this.name = name;
    setMethodResultListener(this);
}

@Override
public void onSuccess(Object o) {
    ALog.i("onJsSendMethodResult result", o.toString());
}

@Override
public void onError(String s, int i, String s1) {
    ALog.i("onError", s);
}

@Override
public void onMethodCancel(String s) {
    ALog.i("onError", s);
}
```

### ios

#### 创建Bridge

(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName	instanceId:(int32_t)instanceId;

创建BridgePlugin类。

```objective-c
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId];
```

#### 调用方法

(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName	instanceId:(int32_t)instanceId;

创建BridgePlugin类。

```objective-c
MethodData * method = [[MethodData alloc] initMethodWithName:@"getString0Paras" parameter:nil];
[self.plugin callMethod:method];
```

#### 发送数据

(void)sendMessage:(id)data;

向js侧发送数据。

```objective-c
[title isEqualToString:@"PlatformsendMessage"]) {
[self.plugin sendMessage:@[@"send first message",@"send second message"]];
```

#### 注册消息监听

@property(nonatomic, weak) id <IMessageListener> messageListener;

注册消息监听。

```objective-c
<IMethodResult,IMessageListener> {
    UIView * _boardView;
}

#pragma mark - listener
- (id)onMessage:(id)data {
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onMessage : \n %@",data]];
    return @{@"response: first":@"response: second"};
}

- (void)onMessageResponse:(id)data {
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onMessageResponse : \n %@",data]];
}
```

#### 注册方法返回监听

@property(nonatomic, weak) id <IMethodResult> methodResult;

注册方法返回监听

```objective-c
- (void)onSuccess:(NSString *)methodName
      resultValue:(id)resultValue {
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onSuccess:resultValue : \n %@",resultValue]];
}

- (void)onError:(NSString *)methodName
      errorCode:(ErrorCode)errorCode
   errorMessage:(NSString *)errorMessage {

    NSLog(@"%s methodName %@ ,errorCode %d, errorMessage %@",__func__,methodName,errorCode,errorMessage);
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onError : %@ \n errorCode %d \n errorMessage%@",methodName,errorCode,errorMessage]];
}

- (void)onMethodCancel:(NSString *)methodName {
    NSLog(@"%s methodName %@ ",__func__,methodName);
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onMethodCancel"]];
}
```

## 场景示例

##### 前端UI用例

```javascript
// xxx.ets

import bridge from '@arkui-x.bridge';

const bridgeImpl = bridge.createBridge("Bridge");
const message = {
  "data" : "message from ArkUI",
};

@Entry
@Component
struct Index {
  build() {
    Row() {
      Column() {
        Button("callMethod")
          .backgroundColor("#AFEEEE")
          .width(280).height(30).fontColor(Color.Black)
          .fontSize(15).margin(10)
          .onClick(() => {
            bridgeImpl.callMethod("getHelloWorld").then((data)=>{
              console.log("js data = " + data);
            });
          })

        Button("sendMessage")
          .backgroundColor("#AFEEEE")
          .width(280).height(30).fontColor(Color.Black)
          .fontSize(15).margin(10)
          .onClick(() => {
            bridgeImpl.sendMessage(message).then((data)=>{
              console.log("js data = " + data);
            });
          });
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

##### Android用例

```java
// Bridge.java

package com.example.bridgestage;

import android.content.Context;
import ohos.ace.adapter.ALog;
import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.ace.adapter.capability.bridge.IMessageListener;
import ohos.ace.adapter.capability.bridge.IMethodResult;

public class Bridge extends BridgePlugin implements IMessageListener, IMethodResult {
    public String name;

    public Bridge(Context context, String name, int id) {
        super(context, name, id);
        this.name = name;
        setMethodResultListener(this);
        setMessageListener(this);
    }

    public String getHelloWorld() {
        return "java: Hello World";
    }

    @Override
    public Object onMessage(Object object) {
        return "java onMessage success";
    }

    @Override
    public void onMessageResponse(Object object) {
        ALog.i("onMessageResponse data: ", object.toString());
    }

    @Override
    public void onSuccess(Object object) {
        ALog.i("onJsSendMethodResult result: ", object.toString());
    }

    @Override
    public void onError(String s, int i, String s1) {
        ALog.i("onError: ", s);
    }

    @Override
    public void onMethodCancel(String s) {
        ALog.i("onError: ", s);
    }
}
```

```java
//	EntryMainActivity.java

package com.example.bridgestage;

import android.os.Bundle;
import ohos.stage.ability.adapter.StageActivity;
import ohos.ace.adapter.capability.bridge.MethodData;

public class EntryMainActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        int id = getInstanceId();
        Bridge bridge = new Bridge(this, "Bridge", id);

        super.setInstanceName("com.example.bridgestage:entry:MainAbility:");
        super.onCreate(savedInstanceState);

        Object[] paramObject = {"I", "just", "made", "it"};
        MethodData methodData = new MethodData("javaCallMethod", paramObject);
        bridge.callMethod(methodData);

		bridge.sendMessage("java send message");
    }
}
```

```java
// MyApplication.java

package com.example.bridgestage;

import ohos.stage.ability.adapter.StageApplication;

public class MyApplication extends StageApplication {
    @Override
    public void onCreate() {
        super.onCreate();
    }
}
```

##### ios用例

```objective-c
//  BridgeClass.h

#import <libarkui_ios/BridgePlugin.h>

NS_ASSUME_NONNULL_BEGIN

@interface BridgeClass : BridgePlugin

- (NSString *)getHelloWorld;

@end

NS_ASSUME_NONNULL_END
```

```objective-c
//  BridgeClass.m

#import "BridgeClass.h"
#import <UIKit/UIKit.h>

@implementation BridgeClass

- (NSString *)getHelloWorld {
    return @"oc: Hello World";
}
@end
```

```objective-c
//	AppDelegate.h

#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>
@property (nonatomic, strong) UIWindow *window;
@end
```

```objective-c
//	AppDelegate.m

#import "AppDelegate.h"
#import <libarkui_ios/StageApplication.h>
#import <libarkui_ios/BridgePlugin.h>
#import "EntryMainViewController.h"
#import "BridgeClass.h"

#define BUNDLE_DIRECTORY @"arkui-x"
#define BUNDLE_NAME @"com.example.bridgestage"

@interface AppDelegate ()

@property (nonatomic, strong) BridgeClass * plugin;
@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
    [StageApplication launchApplication];

    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",BUNDLE_NAME, @"entry", @"MainAbility"];
    EntryMainViewController *mainView = [[EntryMainViewController alloc] initWithInstanceName:instanceName];
    [self setNavRootVC:mainView];

    int32_t instanceId = [mainView getInstanceId];
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instanceId];
    return YES;
}

- (void)setNavRootVC:(id)viewController {
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    self.window.rootViewController = viewController;
}

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
#endif /* EntryMainViewController_h */
```

```objective-c
// EntryMainViewController.m

#import "EntryMainViewController.h"

@interface EntryMainViewController ()

@end

@implementation EntryMainViewController
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {

    }
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    
}
@end
```
