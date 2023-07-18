# iOS平台构建ArkUI framework与使用

## 总览
本教程主要讲述如何利用ArkUI-X SDK完成iOS framework开发与使用，实现基于ArkTS的声明式开发范式在iOS平台显示。包括：

* iOS framework工程集成ArkUI-X SDK
* iOS framework工程集成ArkUI JSBundle实例
* 使用ACE Tools和DevEco Studio集成ArkUI-X SDK进行framework 开发
* framework在iOS应用工程的使用
### iOS framwork工程集成ArkUI跨平台SDK
iOS framework工程集成ArkUI跨平台SDK与iOS应用工程集成Framework规则一致，即将SDK中libarkui_ios.xcframework等包拷贝到工程目录下，并引入到工程目录。

**ViewController部分**
* EntryMainViewController.h
```objective-c
#ifndef EntryMainViewController_h
#define EntryMainViewController_h
#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>
@interface EntryMainViewController : StageViewController


@end

#endif /* EntryMainViewController_h */
```
* EntryMainViewController.m
```objective-c
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
    self.edgesForExtendedLayout = UIRectEdgeNone;
    self.extendedLayoutIncludesOpaqueBars = YES;
}
@end

```
**AppDelegate部分**
* ArkUIAppDelegate.h
```objective-c
#import <UIKit/UIKit.h>

@interface AppDelegate : UIResponder <UIApplicationDelegate>

@property (nonatomic, strong) UIWindow *window;

@end
```
* ArkUIAppDelegate.m
```objective-c
#import "ArkUIAppDelegate.h"
#import "EntryMainViewController.h"
#import <libarkui_ios/StageApplication.h>

#define BUNDLE_DIRECTORY @"Frameworks/myframework.framework/arkui-x"
#define BUNDLE_NAME @"com.example.myframework"

@interface ArkUIAppDelegate ()

@end

@implementation ArkUIAppDelegate

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
    [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
    [StageApplication launchApplication];
    
    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",BUNDLE_NAME, @"entry", @"MainAbility"];
    EntryMainViewController *mainView = [[EntryMainViewController alloc] initWithInstanceName:instanceName];
    [self setNavRootVC:mainView];
    return YES;
}

- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options {
    NSLog(@"appdelegate openUrl callback, url : %@", url.absoluteString); // eg: (com.entry.arkui://entry?OtherAbility)
    
    NSString *bundleName = url.scheme;
    NSString *moduleName = url.host;
    NSString *abilityName, *params;

    NSURLComponents * urlComponents = [NSURLComponents componentsWithString:url.absoluteString];
    NSArray <NSURLQueryItem *> *array = urlComponents.queryItems;
    for (NSURLQueryItem * item in array) {
        if ([item.name isEqualToString:@"abilityName"]) {
            abilityName = item.value;
        } else if ([item.name isEqualToString:@"params"]) {
            params = item.value;
        }
    }

    [self handleOpenUrlWithBundleName:bundleName
                           moduleName:moduleName
                          abilityName:abilityName
                               params:params, nil];
    
    return YES;
}

- (BOOL)handleOpenUrlWithBundleName:(NSString *)bundleName
                         moduleName:(NSString *)moduleName
                        abilityName:(NSString *)abilityName
                             params:(NSString *)params, ...NS_REQUIRES_NIL_TERMINATION {
    
    id rootVC = [[UIApplication sharedApplication].delegate window].rootViewController;
    BOOL hasRoot = NO;
    if ([rootVC isKindOfClass:[UINavigationController class]]) {
        hasRoot = YES;
    }
    
    id subStageVC = nil;
    
    if ([moduleName isEqualToString:@"entry"] && [abilityName isEqualToString:@"MainAbility"]) {
        NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",bundleName, moduleName, abilityName];
        EntryMainViewController *entryOtherVC = [[EntryMainViewController alloc] initWithInstanceName:instanceName];
        entryOtherVC.params = params;
        subStageVC = (EntryMainViewController *)entryOtherVC;
    } // other ViewController
    
    if (!subStageVC) {
        return NO;
    }
    
    if (!hasRoot) {
        [self setNavRootVC:subStageVC];
    } else {
        UINavigationController *rootNav = (UINavigationController *)self.window.rootViewController;
        [rootNav pushViewController:subStageVC animated:YES];
    }
    return YES;
}

- (void)setNavRootVC:(id)viewController {
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    UINavigationController *navi = [[UINavigationController alloc]initWithRootViewController:viewController];
    [self setNaviAppearance:navi];
    self.window.rootViewController = navi;
}

- (void)setNaviAppearance:(UINavigationController *)navi {
    UINavigationBarAppearance *appearance = [UINavigationBarAppearance new];
    [appearance configureWithOpaqueBackground];
    appearance.backgroundColor = UIColor.whiteColor;
    navi.navigationBar.standardAppearance = appearance;
    navi.navigationBar.scrollEdgeAppearance = navi.navigationBar.standardAppearance;
}

@end
```
### iOS framework程集成ArkUI JSBundle实例
ArkUI JSBundle生成后，拷贝到iOS应用工程arkui-x目录下。这里“arkui-x”目录名称是固定的，不能更改；entry为模块实例名称，可以自定义名称；entryTest为测试模块，可按照实际需求拷贝；systemres是JSBundle必须的系统资源，需从ArkUI-X SDK中拷贝。

```
iOS应用工程
├── myframework.xcodeproj
├── myframework
|   ├── EntryMainViewController.h
|   ├── EntryMainViewController.m
│   ├── ArkUIAppDelegate.h
│   ├── ArkUIAppDelegate.mm
│   └── myframework.h
├── arkui-x
│   ├── entry
│   │   └── ets
│   │       ├── modules.abc
│   │       └── sourceMaps.map
│   ├── entryTest
|   └── systemres
└── frameworks
    └── libarkui_ios.xcframework
```

### 使用ACE Tools和DevEco Studio集成ArkUI-X SDK进行iOS framework开发
上述步骤均可通过ACE Tools或DevEco Studio完成
* ACE Tools
1. ace create 命令创建一个跨平台应用工程
2. 进入工程后，通过ace create framework命令创建一个framework工程
3. 执行ace build framework命令，就会执行上述步骤。对jsbundle的生成与拷贝，以及其所依赖的动态库文件和jar包的拷贝，并且最后构建出iOS framework包。
* DevEco Studio
1. 创建跨平台library工程
2. 通过执行build app选项，即可执行上述步骤，并且构建出iOS framework
### framework在应用工程的使用

通过xcode创建一个应用工程，删除其他xcode自动生成的的代码头文件和源文件(参考[ios工程集成 ArkUI sdk]())。将上述myframework.framework与libarkui_ios.framework引入到工程中。
**AppDelegate部分**
* AppDelegate.h
```objective-c
#import <UIKit/UIKit.h>

 #import <myframework/ArkUIAppDelegate.h>
 @interface AppDelegate : ArkUIAppDelegate


 @end
```
* AppDelegate.m
```objective-c
 #import "AppDelegate.h"

 @interface AppDelegate ()

 @end

 @implementation AppDelegate

 @end
```

完成上述步骤后即可按照iOS应用构建流程，构建ArkUI iOS应用。