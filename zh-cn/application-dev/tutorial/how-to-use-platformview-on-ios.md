# iOS平台视图开发指南

## 概述

平台视图（PlatformView）是ArkUI-X框架提供的核心能力之一，允许开发者在ArkUI界面中嵌入iOS原生UI组件。<br>通过此能力，您可以充分利用iOS平台特有的UI组件（如MKMapView、WKWebView等），实现ArkUI组件与原生视图的无缝混合渲染。<br>
ArkUI侧具体API请参考[PlatformView API](../reference/apis/js-apis-PlatformView.md)，iOS侧参考[PlatformView](../reference/arkui-for-ios/platformview-interface-ios.md)。<br>

## 示例

### 自定义IPlatformView接口的实现类

```objective-c
// MyPlatformView.h
// 自定义平台视图类的头文件，用于声明 MyPlatformView 类接口
// 该类遵循 IPlatformView 协议，提供地图视图的封装

// 导入基础框架
#import <Foundation/Foundation.h>
#import <UIKit/UIKit.h>
// 导入自定义协议（假设 IPlatformView 是项目特定协议）
#import <libarkui_ios/IPlatformView.h>

NS_ASSUME_NONNULL_BEGIN

// MyPlatformView 类接口定义
// 继承自 NSObject，并遵循 IPlatformView 协议
@interface MyPlatformView : NSObject <IPlatformView>

// 返回关联的 UIView 对象（地图视图）
- (UIView *)view;

// 清理资源的方法（当前为空实现，可根据需要扩展）
- (void)onDispose;

// 返回平台视图的唯一标识符
- (NSString *)getPlatformViewID;

// 初始化方法
// 使用指定的 CGRect 框架初始化视图（原代码无参数，现添加 frame 参数以明确用途）
- (instancetype)initWithFrame:(CGRect)frame;

@end

NS_ASSUME_NONNULL_END
```

```objective-c
// MyPlatformView.m
// 自定义平台视图类的实现文件
// 实现 MyPlatformView 类，封装 MKMapView 以提供地图功能

#import "MyPlatformView.h"
// 导入 MapKit 框架以使用 MKMapView
#import <MapKit/MapKit.h>

@implementation MyPlatformView {
    // 私有实例变量：地图视图
    MKMapView *_mapView;
    // 私有实例变量：视图标识符
    NSString *_viewTag;
}

// 初始化方法
// 参数 frame: 视图的框架（位置和大小）
// 返回: 初始化后的实例
- (instancetype)initWithFrame:(CGRect)frame {
    self = [super init];
    if (self) {
        // 设置视图标识符
        _viewTag = @"PlatformViewTest1";
        
        // 初始化地图视图，并设置其框架
        _mapView = [[MKMapView alloc] initWithFrame:frame];
        
        // 配置地图属性
        _mapView.showsCompass = YES;        // 显示指南针
        _mapView.showsScale = YES;          // 显示比例尺
        [_mapView setMapType:MKMapTypeStandard]; // 设置地图类型为标准
        _mapView.showsUserLocation = YES;   // 显示用户位置
    }
    return self;
}

// 返回关联的 UIView 对象（即地图视图）
// 用于在界面上显示地图
- (UIView *)view {
    return _mapView;
}

// 清理资源的方法
// 当前为空实现，可用于释放地图视图或其它资源
- (void)onDispose {
    // 示例：如果需要清理，可在此添加代码
  	webView = nil;
    return;
}

// 返回平台视图的唯一标识符
// 用于识别视图实例
- (NSString *)getPlatformViewID {
    return _viewTag;
}

@end
```

### 自定义PlatformViewFactory接口的实现类

```objective-c
// MyPlatformViewFactory.h
// 自定义平台视图工厂类的头文件
// 实现 PlatformViewFactory 协议，负责创建和管理平台视图实例

#import <Foundation/Foundation.h>
#import <libarkui_ios/PlatformViewFactory.h>
#import <libarkui_ios/IPlatformView.h>

NS_ASSUME_NONNULL_BEGIN

/**
 自定义平台视图工厂类
 遵循 PlatformViewFactory 协议，用于创建特定类型的平台视图
 */
@interface MyPlatformViewFactory : NSObject<PlatformViewFactory>

/**
 初始化工厂方法
 @return 初始化后的工厂实例
 */
- (instancetype)initWithFrame;

/**
 根据平台视图类型标识符获取对应的平台视图
 
 @param platformTypeId 平台视图类型标识符，用于区分不同类型的视图
 @return 符合 IPlatformView 协议的视图对象，如果类型标识符不匹配则返回 nil
 */
- (nullable NSObject<IPlatformView> *)getPlatformView:(NSString *)platformTypeId;

@end

NS_ASSUME_NONNULL_END
```

```objective-c
// MyPlatformViewFactory.mm
// 自定义平台视图工厂类的实现文件
// 负责根据类型标识符创建对应的平台视图实例

#import "MyPlatformViewFactory.h"
#import "MyPlatformView.h"

@implementation MyPlatformViewFactory {
}

#pragma mark - 初始化方法

/**
 初始化工厂实例
 @return 初始化后的工厂实例
 */
- (instancetype)initWithFrame:{
    return self;
}

#pragma mark - PlatformViewFactory 协议方法实现

/**
 根据平台视图类型标识符创建对应的平台视图
 
 @param platformTypeId 平台视图类型标识符
 @return 对应的平台视图实例，如果不支持该类型则返回 nil
 
 实现逻辑：
 1. 检查传入的类型标识符是否匹配已支持的视图类型
 2. 根据匹配结果创建对应的平台视图实例
 3. 返回创建的视图对象
 */
- (nullable NSObject<IPlatformView> *)getPlatformView:(NSString *)platformTypeId {
    // 检查类型标识符是否匹配支持的视图类型
    if ([platformTypeId isEqualToString:@"PlatformViewTest1"]) {
        // 创建 MyPlatformView 实例
        NSObject<IPlatformView> *platformView = [[MyPlatformView alloc] initWithFrame];
        return platformView;
    }
    
    // 如果不支持指定的类型标识符，记录日志并返回 nil
    NSLog(@"MyPlatformViewFactory: 不支持的平台视图类型标识符: %@", platformTypeId);
    return nil;
}

@end
```

### 自定义ViewController，把PlatformViewFactory实现类注册到ViewController中

```objective-c
// EntryEntryAbilityViewController.h
// 自定义ViewController，用于将PlatformViewFactory实现类注册到ViewController中
// 继承自StageViewController，是ArkUI-X在iOS平台上的入口视图控制器

// 导入UIKit框架
#import <UIKit/UIKit.h>
// 导入ArkUI-X iOS框架的StageViewController
#import <libarkui_ios/StageViewController.h>

// 接口定义
@interface EntryEntryAbilityViewController : StageViewController

@end
```

```objective-c
// EntryEntryAbilityViewController.mm
// EntryEntryAbilityViewController类的实现文件
// 负责创建PlatformViewFactory并将其注册到ViewController中

// 导入当前类的头文件
#import "EntryEntryAbilityViewController.h"
// 导入自定义平台视图工厂类的头文件
#import "MyPlatformViewFactory.h"

// 类扩展，可用于声明私有属性和方法
@interface EntryEntryAbilityViewController ()

@end

// 类实现
@implementation EntryEntryAbilityViewController {
    // 私有实例变量：平台视图工厂对象
    // 遵循PlatformViewFactory协议，用于创建和管理平台视图
    NSObject<PlatformViewFactory> *factory;
}

// 初始化方法
// 参数 instanceName：视图控制器的实例名称
// 返回值：初始化后的视图控制器实例
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    // 调用父类的初始化方法
    self = [super initWithInstanceName:instanceName];
    // 返回初始化后的实例
    return self;
}

// 视图加载完成时调用的方法
// 在此方法中创建PlatformViewFactory并注册到父类中
- (void)viewDidLoad {
    [super viewDidLoad];
    
    // 创建PlatformViewFactory对象
    factory = [MyPlatformViewFactory alloc];
    
    // 将PlatformViewFactory注册到父类中
    [super registerPlatformViewFactory:factory];
}

@end
```

```objective-c
// AppDelegate.m
// 应用程序委托类的实现文件
// 负责应用程序的初始化和生命周期管理

// 导入AppDelegate类的头文件
#import "AppDelegate.h"
// 导入自定义入口视图控制器的头文件
#import "EntryEntryAbilityViewController.h"
// 导入ArkUI-X iOS框架的StageApplication
#import <libarkui_ios/StageApplication.h>

// 定义宏常量：资源包目录
// 指定ArkUI-X资源文件所在的目录名称
#define BUNDLE_DIRECTORY @"arkui-x"

// 定义宏常量：资源包名称
// 指定应用程序的资源包标识符
#define BUNDLE_NAME @"com.example.platformviewtest"

// 类扩展，可用于声明私有属性和方法
@interface AppDelegate ()

@end

// 类实现
@implementation AppDelegate

// 应用程序启动完成时调用的方法
// 参数 application：UIApplication实例
// 参数 launchOptions：启动选项字典
// 返回值：BOOL类型，表示应用程序是否启动成功
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    // 配置ArkUI-X模块，设置资源包目录
    [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
    
    // 启动ArkUI-X应用程序
    [StageApplication launchApplication];
    
    // 创建实例名称
    // 格式：资源包名称 + "entry" + "EntryAbility"
    NSString *instanceName = [NSString stringWithFormat:@"%@%@", BUNDLE_NAME, @"entry", @"EntryAbility"];
    
    // 创建EntryEntryAbilityViewController实例
    EntryEntryAbilityViewController *mainView = [[EntryEntryAbilityViewController alloc] initWithInstanceName:instanceName];
    
    // 将主视图控制器设置为导航根视图控制器
    [self setNavRootVC:mainView];
    
    // 返回YES表示应用程序启动成功
    return YES;
}

// 设置导航根视图控制器的方法
// 该方法用于创建应用主窗口、配置导航控制器，并将其设置为窗口的根视图控制器
// 参数 viewController: 要作为导航栈根视图控制器的视图控制器对象
- (void)setNavRootVC:(id)viewController {
    // 创建应用主窗口，使用屏幕边界作为窗口的框架
    // 这样窗口会占据整个屏幕空间
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    
    // 设置窗口的背景颜色为红色
    // 在开发阶段，使用红色背景有助于观察窗口是否被正确创建和显示
    self.window.backgroundColor = [UIColor redColor];
    
    // 将窗口设置为关键窗口（接收键盘和触摸事件的窗口）并使其可见
    [self.window makeKeyAndVisible];
    
    // 创建一个导航控制器，将传入的viewController作为其根视图控制器
    // 导航控制器用于管理多个视图控制器的导航堆栈
    UINavigationController *navi = [[UINavigationController alloc]initWithRootViewController:viewController];
    
    // 调用自定义方法，配置导航控制器的外观样式
    // 这可能包括设置导航栏颜色、标题样式、按钮样式等
    [self setNaviAppearance:navi];
    
    // 将配置好的导航控制器设置为窗口的根视图控制器
    // 这样导航控制器及其管理的视图层次结构将成为窗口的主要内容
    self.window.rootViewController = navi;
}

@end
```
