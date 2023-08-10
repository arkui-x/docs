# iOS平台构建ArkUI应用

## 简介

本教程主要讲述如何利用ArkUI-X SDK完成iOS应用开发，实现基于ArkTS的声明式开发范式在iOS平台显示。包括：

* iOS应用工程集成ArkUI-X SDK
* iOS应用工程集成ArkUI-X应用编译产物
* 使用ACE Tools和DevEco Studio集成ArkUI-X SDK进行iOS应用开发

### iOS 工程创建
通过ACE Tools或DevEco Studio创建一个ArkUI-X应用工程（示例工程名为HelloWorld），其工程目录下的.arkui-x/ios目录代表对应的iOS工程。iOS应用的入口AppDelegate和ViewController类，其中ViewController需要继承自ArkUI提供的基类StageViewController，详情参见[使用说明](https://gitee.com/arkui-x/docs/tree/master/zh-cn/application-dev/reference/arkui-for-ios)：
* ViewController类\
该类名通过通过module名和ability名拼接而得，一个ability对应一个iOS工程侧的ViewController类。详情参见[Ability使用说明](../quick-start/start-with-ability-on-ios.md):\
EntryEntryAbilityViewController.h 
    ``` objective-c
    #ifndef EntryEntryAbilityViewController_h
    #define EntryEntryAbilityViewController_h
    #import <UIKit/UIKit.h>
    #import <libarkui_ios/StageViewController.h>
    @interface EntryEntryAbilityViewController : StageViewController


    @end

    #endif /* EntryEntryAbilityViewController_h */
    ```
    EntryEntryAbilityViewController.m
    ``` objective-c
    #import "EntryEntryAbilityViewController.h"

    @interface EntryEntryAbilityViewController ()

    @end

    @implementation EntryEntryAbilityViewController
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
* AppDelegate类

    AppDelegate.m中实例化EntryEntryAbilityViewController，并加载ArkUI页面。

    ```objective-c
    #import "AppDelegate.h"
    #import "EntryEntryAbilityViewController.h"
    #import <libarkui_ios/StageApplication.h>

    #define BUNDLE_DIRECTORY @"arkui-x"
    #define BUNDLE_NAME @"com.example.helloworld"

    @interface AppDelegate ()

    @end

    @implementation AppDelegate

    - (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {
        [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
        [StageApplication launchApplication];
        
        NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",BUNDLE_NAME, @"entry", @"EntryAbility"];
        EntryEntryAbilityViewController *mainView = [[EntryEntryAbilityViewController alloc] initWithInstanceName:instanceName];//instanceName为ArkUI-X应用编译产物在应用工程中存放的目录
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
    

### iOS 工程编译

对iOS工程编译时，ACE Tools或DevEco Studio会完成两个步骤：
* 集成ArkUI-X SDK\
iOS工程集成ArkUI跨平台SDK遵循iOS应用工程集成Framework规则，SDK中Framework(libarkui_ios.xcframework\libhilog.xcframework\libresourcemanager.xcframework)会自动拷贝到工程目frameworks录下，并引入到工程目录。
* 集成ArkUI-X应用编译产物\
ArkUI-X编译产物生成后，自动拷贝到iOS应用工程arkui-x目录下。这里“arkui-x”目录名称是固定的，不能更改；详情参见[ArkUI-X应用工程结构说明](../quick-start/package-structure-guide.md)

```
    arkui-x
        ├── entry
        |   ├── ets
        |   |   ├── modules.abc
        |   |   └── sourceMaps.map
        |   ├── resouces.index
        |   ├── resouces
        |   └── module.json
        └── systemres
```
完成上述步骤后即可按照iOS应用构建流程，构建ArkUI iOS应用，并且可以安装至iOS手机后运行。


### 参考

【1】[ArkUI-X Samples仓](https://gitee.com/arkui-x/samples)

