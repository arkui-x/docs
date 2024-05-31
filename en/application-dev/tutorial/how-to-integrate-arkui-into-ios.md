# How to Build ArkUI Apps on iOS

## Overview

This tutorial describes how to use the ArkUI-X SDK to develop iOS apps, thereby implementing the display of the ArkTS-based declarative development paradigm on iOS. It covers the following topics:

* Integrating the ArkUI-X SDK into an iOS project
* Integrating an ArkUI JS bundle instance into an iOS project
* Using ACE Tools and DevEco Studio to integrate the ArkUI-X SDK for iOS app development

### Creating an iOS App Project

* Use Xcode to create an iOS app project, and set the language to Objective-C.
* In versions later than Xcode 11, when an iOS project of the SceneDelegate type is created, you must delete the SceneDelegate-related content.
    1. Delete the **Application Scene Manifest** option from **Info.plist**.
    2. Delete the **SceneDelegate** code file or comment out all code in the file.
    3. Delete or comment out the **Scene** method in **AppDelegate**.

### Adding the ArkUI-X SDK
The integration complies with the integration framework rules of iOS app projects. Specifically, you need to copy and import the packages in the SDK, for example, **libarkui_ios.xcframework**, to the project directory.

### Creating the ViewController Class

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
### Modifying the AppDelegate Implementation

**AppDelegate**

Instantiate **EntryMainViewController** in **AppDelegate.m**, and load the ArkUI page.

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

The **instanceName** parameter specifies the directory that stores the JS bundle compiled using the ArkUI paradigm code in the app project.

### Copying the ArkUI JS Bundle Instance

After the ArkUI JS bundle is generated, copy it to the **arkui-x** directory of the iOS app project. In the iOS app project, **arkui-x** is a fixed directory name that cannot be changed, **entry** indicates the module instance, whose name can be customized as needed, **entryTest** indicates the test module, which can be copied on demand, and **systemres** holds the system resources, which are mandatory for the JS bundle and need to be copied from the ArkUI-X SDK.

```
iOS app project
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
Now you can build an ArkUI iOS app according to the iOS app building process.

###  Debugging and Verification

**Debugging Using a Real Device**

1. Configure **Signing** in **Target signing & capabilities**.
2. In **Target General**, set **Frameworks, Libraries, and Embedded Content** to **Embed &Sign**.
3. In **Target Building Settings**, set **Build Options Enable Bitcode** to **No**.

**Debugging Using an Emulator**

1. Add the ArkUI cross-platform framework corresponding to the emulator version.
2. In **Target General**, set **Frameworks, Libraries, and Embedded Content** to **Do Not Embed**.


### References

[1] [ArkUI-X App Samples](https://gitee.com/arkui-x/samples)
