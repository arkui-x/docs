# 平台视图开发指南

平台视图，提供了ArkUI-X与原生view进行混合显示。本文主要介绍iOS平台进行平台视图开发的步骤说明，iOS接口参考[PlatformView](../reference/arkui-for-ios/platformview-interface-ios.md)。

开发者需要做如下3个步骤。

1、【iOS】 自定义PlatformView, PlatformViewFactory

    在 iOS 端新建一个实现 IPlatformView 接口的类，并实现 getView 、onDispose()接口；
    在 iOS 端新建一个实现 PlatformViewFactory 接口的类，并实现 getPlatformView()接口；

2、【iOS】 定义ViewController，注册上述的 PlatformViewFactory类

    再实现一个继承 ViewController的类，在viewDidLoad函数中创建(1)的 PlatformViewFactory 对象；
    把 PlatformViewFactory 类注册到 ViewController中。

3、【Arkui-X ets】使用PlatformView

    导入模块
    import PlatformView, { PlatformViewAttribute } from '@arkui-x.platformview'
    在 iOS 端实现好了之后，就能在Arkui端用ets来显示原生组件的视图了，如显示MapView。

## iOS平台

### 场景：使用原生地图组件

1、自定义IPlatformView接口的实现类。

```java
// MyPlatformView.h
#import <Foundation/Foundation.h>
#import <UIKit/UIKit.h>
#import <libarkui_ios/IPlatformView.h>

NS_ASSUME_NONNULL_BEGIN

@interface MyPlatformView : NSObject<IPlatformView>
- (UIView *) view;
- (void) onDispose;
- (NSString *) getPlatformViewID;
- initWithFrame;
@end

NS_ASSUME_NONNULL_END

```

```java
// MyPlatformView.m
@implementation MyPlatformView {
    MKMapView *mapView;
    NSString* viewTag;
}

- (instancetype)initWithFrame
{
    viewTag = @"PlatformViewTest1";

    mapView = [[IMapView alloc] init];
    mapView.showsCompass = YES;
    mapView.showsScale = YES;

    [mapView setMapType:MKMapTypeStandard];
    mapView.showsUserLocation = YES;

    return self;
}

- (UIView*) view {
    return mapView;	
}
- (void)onDispose {
    return;
}
- (NSString*) getPlatformViewID {
    return viewTag;
}

@end

```


2、自定义PlatformViewFactory接口的实现类。

```java
// MyPlatformViewFactory.h
#import <libarkui_ios/PlatformViewFactory.h>
#import <libarkui_ios/IPlatformView.h>

NS_ASSUME_NONNULL_BEGIN
@interface MyPlatformViewFactory : NSObject<PlatformViewFactory>

- (NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId;
- initWithFrame;
@end
NS_ASSUME_NONNULL_END

```

```java
// MyPlatformViewFactory.mm
#import "MyPlatformViewFactory.h"
#import "MyPlatformView.h"

@implementation MyPlatformViewFactory {

}

- (instancetype)initWithFrame{
    return self;
}

- (NSObject<IPlatformView>*) getPlatformView:(NSString*) platformViewId {
    //create PlatfformView
    if ([platformViewId isEqualToString:@"PlatformViewTest1"]) {
        NSObject<IPlatformView> * view = [[MyPlatformView alloc] initWithFrame];
        return  view;
    }
    return  nil;
}
@end

```


3、自定义ViewController，把PlatformViewFactory实现类注册到ViewController中。

```java
// EntryEntryAbilityViewController.h
#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>
@interface EntryEntryAbilityViewController : StageViewController

@end

// EntryEntryAbilityViewController.mm
#import "EntryEntryAbilityViewController.h"
#import "MyPlatformViewFactory.h"

@interface EntryEntryAbilityViewController ()

@end

@implementation EntryEntryAbilityViewController{
    NSObject<PlatformViewFactory> *factory;
}

- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
    //create PlatformViewFactory
    factory = [MyPlatformViewFactory alloc];
    // register PlatformViewFactory
    [super registerPlatformViewFactory:factory];
}
@end
```

```java
// AppDelegate.h
#import <UIKit/UIKit.h>
@interface AppDelegate : UIResponder <UIApplicationDelegate>
@property (nonatomic, strong) UIWindow *window;
@end

// AppDelegate.m
#import "AppDelegate.h"
#import "EntryEntryAbilityViewController.h"
#import <libarkui_ios/StageApplication.h>

#define BUNDLE_DIRECTORY @"arkui-x"
#define BUNDLE_NAME @"com.example.platformviewtest"

@interface AppDelegate ()

@end

@implementation AppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
    [StageApplication launchApplication];
    
    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",BUNDLE_NAME, @"entry", @"EntryAbility"];
    EntryEntryAbilityViewController *mainView = [[EntryEntryAbilityViewController alloc] initWithInstanceName:instanceName];
    [self setNavRootVC:mainView];
    return YES;
}
@end

```

4、在ArkTS中，使用PlatformView。
   编写ArkUI页面platformview.ets。

```java
// platformview.ets
import PlatformView, { PlatformViewAttribute } from '@arkui-x.platformview';
@Entry
@Component
struct PlatformViewSample {
  @State changeValue: string = ''
  @State submitValue: string = ''
  controller: SearchController = new SearchController()
  build() {
    Column() {
      PlatformView('PlatformViewTest1')
        .width('100%')
        .height('80%')
        .backgroundColor(Color.Gray)

      Search({ value: this.changeValue, placeholder: 'Type to search...', controller: this.controller })
        .searchButton('SEARCH')
        .width('95%')
        .height(40)
        .backgroundColor('#F5F5F5')
        .placeholderColor(Color.Grey)
        .placeholderFont({ size: 14, weight: 400 })
        .textFont({ size: 14, weight: 400 })
        .onSubmit((value: string) => {
          this.submitValue = value
        })
        .onChange((value: string) => {
          this.changeValue = value
        })
        .margin(20)

    }.height('100%')
  }
}

```
