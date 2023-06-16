## iOS平台构建ArkUI应用

#### 总览

本教程主要讲述如何利用ArkUI-X SDK完成iOS应用开发，实现基于ArkTS的声明式开发范式在iOS平台显示。包括：

* iOS应用工程集成ArkUI-X SDK
* iOS应用工程集成ArkUI JSBundle实例
* 使用ace tool和DevEco Studio IDE集成ArkUI-X SDK进行iOS应用开发

##### iOS工程集成ArkUI跨平台SDK

iOS工程集成ArkUI跨平台SDK遵循iOS应用工程集成Framework规则，即将SDK中libarkui_ios.xcframework等包拷贝到工程目录下，并引入到工程目录。


**ViewController部分**
* EntryMainViewController.h
```
#ifndef EntryMainViewController_h
#define EntryMainViewController_h
#import <UIKit/UIKit.h>
#import <libarkui_ios/StageViewController.h>
@interface EntryMainViewController : StageViewController


@end

#endif /* EntryMainViewController_h */
```
* EntryMainViewController.m
```
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
* AppDelegate.m中实例化EntryMainViewController，并加载ArkUI页面

```
#import "AppDelegate.h"
#import "EntryMainViewController.h"
#import <libarkui_ios/StageApplication.h>

#define BUNDLE_DIRECTORY @"arkui-x"
#define BUNDLE_NAME @"com.example.newdemo"

@interface AppDelegate ()

@end

@implementation AppDelegate

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

其中参数`instanceName`为ArkUI范式代码编译为JSBundle后在应用工程中存放的目录名。

##### iOS工程集成ArkUI JSBundle实例

ArkUI JSBundle生成后，拷贝到iOS应用工程arkui-x目录下。这里“arkui-x”目录名称是固定的，不能更改；entry为模块实例名称，可以自定义名称；entryTest为测试模块，可按照实际需求拷贝；systemres是JSBundle必须的系统资源，需从ArkUI-X SDK中拷贝。

```
iOS应用工程
├── myapp.xcodeproj
├── myapp
│   ├── Assets.xcassets
│   ├── Base.Iproj
|   ├── EntryMainViewController.h
|   ├── EntryMainViewController.m
│   ├── AppDelegate.h
│   ├── AppDelegate.mm
│   ├── Info.plist
│   └── main.m
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


##### 注意事项
**工程修改**
XCode11之后，新建iOS工程为SceneDelegate类型工程，需删除SceneDelegate相关内容：
1. 删除Info.plist中的Application Scene Manifest选项。
2. SceneDelegate代码文件删除，或注释掉文件中的所有代码。
3. 删除或注释掉AppDelegate中关于Scene的方法。

**真机调试**
1. Target signing & capabilities选项中Signing配置签名信息。
2. Target General选项中Frameworks，Libraries，and Embedded Content配置Embed类型为Embed & Sign。
3. Target Building Settings选项中Build Options Enable Bitcode设置为No。

**模拟器调试**
1. 添加模拟器版本的ArkUI跨平台Framework。
2. Target General选项中Frameworks，Libraries，and Embedded Content配置Embed类型为Do Not Embed。

按照注意事项配置后，即可按照iOS应用构建流程，构建ArkUI iOS应用。

##### 使用ace tool和DevEco Studio IDE集成ArkUI-X SDK进行iOS应用开发
上述步骤我们可以很方便地通过ace tool或DevEco Studio IDE完成
* ace tool
1. ace create 命令创建一个跨平台应用工程
2. 进入工程目录，执行ace build app命令，就会执行上述步骤。对jsbundle的生成与拷贝，以及其所依赖的动态库文件和jar包的拷贝，并且最后构建出一个iOS应用包。
* DevEco Studio IDE
1. 创建跨平台工程
2. 通过执行build app选项，即可执行上述步骤，并且构建出iOS应用
##### 参考

【1】[ArkUI跨平台应用示例](https://gitee.com/arkui-x/samples)

