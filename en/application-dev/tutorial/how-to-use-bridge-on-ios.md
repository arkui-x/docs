# How to Use the Platform Bridge on iOS

​Platform bridge is used for interaction between the client (ArkUI) and the platform (Android or iOS). It allows you to exchange messages between ArkUI and the platform, call platform APIs on ArkUI, and call ArkUI APIs on the platform. This tutorial describes the interaction between iOS and ArkUI. For details about the API usage on ArkUI, see [Bridge API](../reference/apis/js-apis-bridge.md). For details about the API usage on iOS, see [BridgePlugin](../reference/arkui-for-ios/BridgePlugin.md).

## Interaction Between iOS and ArkUI

### Creating a Platform Bridge

1. Create a platform bridge on ArkUI. The specified name must be the same as that on iOS. You can then use the created object to call platform bridge APIs.

```javascript
// xxx.ets

// Import the platform bridge module.
import bridge from '@arkui-x.bridge';

// Create a platform bridge object.
const bridgeImpl = bridge.createBridge('Bridge');
```

2. Create the **BridgePlugin** class on iOS. The specified name must be the same as that on ArkUI. You can then use the created object to call platform bridge APIs.

```objective-c
// xxx.mm

BridgePlugin* plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId];
```

### Scenario 1: Sending Data from ArkUI to iOS

1. Send data from ArkUI to Android.

```javascript
// xxx.ets

bridgeImpl.sendMessage('text').then((res)=>{
    console.log('response: ' + res);
}).catch((err) => {
    console.log('error: ' + JSON.stringify(err));
});
```

2. Receive the data sent from ArkUI to iOS.

```objective-c
// xxx.mm

#pragma mark - listener
// Register a callback to listen for data sending events of ArkUI.
- (id)onMessage:(id)data {
    // Return a response to ArkUI.
    return @"objective-c onMessage success";
}
```

### Scenario 2: Sending Data from iOS to ArkUI

1. On iOS, send data to ArkUI.

```objective-c
// xxx.mm

[self.plugin sendMessage:@[@"message", @"from", @"ios"]];
```

2. Register a callback on ArkUI to listen for the data sent from iOS.

```javascript
// xxx.ets

bridgeImpl.setMessageListener((message) => {
    console.log('receive message: ' + message);

    // Send a response to iOS.
    return "ArkUI receive message success";
});
```

3. Register a callback on iOS to listen for responses from ArkUI.

```objective-c
// xxx.mm

#pragma mark - listener
// Register a callback to listen for responses from ArkUI.
- (void)onMessageResponse:(id)data {}
```

### Scenario 3: Calling iOS APIs on ArkUI

1. Call iOS APIs on ArkUI.

```javascript
// xxx.ets

bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    console.log('result: ' + res);
}).catch((err) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2. Implement the called APIs on iOS.

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


### Scenario 4: Calling ArkUI APIs on iOS

1. Register ArkUI APIs for iOS to call.

```javascript
// xxx.ets

function getString() {
  return 'call js getString success';
}

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2. Call ArkUI APIs on iOS.

```objective-c
// xxx.mm

MethodData* method = [[MethodData alloc] initMethodWithName:@"getString" parameter:nil];
[self.plugin callMethod:method];
```


### Scenario 5: Listening for iOS APIs on ArkUI

1. Register ArkUI APIs for iOS to call.

```javascript
// xxx.ets

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2. Remove the registered ArkUI APIs.

```javascript
// xxx.ets

bridgeImpl.unRegisterMethod('getString');
```

3. Register a callback on iOS to listen for API registration and removal.

```objective-c
// xxx.mm

#pragma mark - listener
- (void)onSuccess:(NSString*)methodName resultValue:(id)resultValue {}

- (void)onError:(NSString*)methodName
    errorCode:(ErrorCode)errorCode
    errorMessage:(NSString*)errorMessage {}

- (void)onMethodCancel:(NSString*)methodName {}
```

## Example Scenario

##### ArkUI

This example shows how to call an iOS API and display its return value on the ArkUI page. Specifically, when you click the button to send a message from ArkUI to iOS, iOS returns a response and displays the received data on the ArkUI page.

```javascript
// Index.ets

// Import the platform bridge module.
import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  // Create a platform bridge object.
  private bridgeImpl = bridge.createBridge('Bridge');
  @State helloArkUI: string = '';
  @State nativeResponse: string = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    // Call the iOS API.
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: string) => {
      // Display the return value of the iOS API through a status variable.
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
            // Send data to iOS and display the response from iOS through a status variable.
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

##### iOS

```objective-c
// BridgeClass.h

// Reference the platform bridge module.
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

// Define the iOS API for ArkUI to call.
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

    // Create a platform bridge with the same name as that of ArkUI.
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

// Listen for messages sent from ArkUI.
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
