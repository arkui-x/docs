# iOS平台构建ArkUI应用

## 简介

本教程主要讲述如何利用ArkUI-X SDK完成iOS应用开发，实现基于ArkTS的声明式开发范式在iOS平台显示。包括：

* iOS应用工程集成ArkUI-X SDK
* iOS应用工程集成ArkUI JSBundle实例
* 使用ACE Tools和DevEco Studio集成ArkUI-X SDK进行iOS应用开发

### iOS应用工程创建
* 使用Xcode创建一个iOS应用工程，语言选择Objective-C，
* 工程修改
XCode11之后，新建iOS工程为SceneDelegate类型工程，需删除SceneDelegate相关内容：
    1. 删除Info.plist中的Application Scene Manifest选项。
    2. SceneDelegate代码文件删除，或注释掉文件中的所有代码。
    3. 删除或注释掉AppDelegate中关于Scene的方法。
### 添加ArkUI-X SDK
iOS工程集成ArkUI跨平台SDK遵循iOS应用工程集成Framework规则，即将SDK中libarkui_ios.xcframework等包拷贝到工程目录下，并引入到工程目录。

### 创建ViewController类
* EntryEntryAbilityViewController.h
    ``` objective-c
    #ifndef EntryMainViewController_h
    #define EntryMainViewController_h
    #import <UIKit/UIKit.h>
    #import <libarkui_ios/StageViewController.h>
    @interface EntryMainViewController : StageViewController


    @end

    #endif /* EntryEntryAbilityViewController_h */
    ```
* EntryEntryAbilityViewController.m
    ``` objective-c
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
### 修改AppDelegate实现
**AppDelegate部分**
AppDelegate.m中实例化EntryMainViewController，并加载ArkUI页面。

```objective-c
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

### 拷贝ArkUI JSBundle实例

ArkUI JSBundle生成后，拷贝到iOS应用工程arkui-x目录下。这里“arkui-x”目录名称是固定的，不能更改；entry为模块实例名称，可以自定义名称；entryTest为测试模块，可按照实际需求拷贝；systemres是JSBundle必须的系统资源，需从ArkUI-X SDK中拷贝。

```
iOS应用工程
├── app.xcodeproj
├── app
│   ├── Assets.xcassets
│   ├── Base.Iproj
|   ├── EntryEntryAbilityViewController.h
|   ├── EntryEntryAbilityViewController.m
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
按照注意事项配置后，即可按照iOS应用构建流程，构建ArkUI iOS应用。

###  运行
**真机调试**
1. Target signing & capabilities选项中Signing配置签名信息。
2. Target General选项中Frameworks，Libraries，and Embedded Content配置Embed类型为Embed & Sign。
3. Target Building Settings选项中Build Options Enable Bitcode设置为No。

**模拟器调试**
1. 添加模拟器版本的ArkUI跨平台Framework。
2. Target General选项中Frameworks，Libraries，and Embedded Content配置Embed类型为Do Not Embed。


### 参考

【1】[ArkUI跨平台应用示例](https://gitee.com/arkui-x/samples)

