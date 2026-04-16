# iOS 原生页面与跨平台页面跳转开发指南

## 概述

本文档介绍如何在现有 iOS 原生工程中集成 ArkUI-X，并实现以下两类页面跳转：

- iOS 原生页面跳转到跨平台页面
- 跨平台页面跳转到 iOS 原生页面

## 整体架构

iOS 与 ArkUI-X 混合开发采用双框架共存模式。宿主应用仍使用 iOS 原生 `UIApplication` 和 `UIViewController` 体系，ArkUI-X 则通过适配层完成运行时初始化和页面承载。

核心组件如下：

- `StageApplication`：ArkUI-X 应用级适配器，负责初始化 ArkUI-X 运行时环境
- `StageViewController`：跨平台页面承载类，负责映射 iOS 页面生命周期并加载跨平台页面
- `frameworks/`：iOS 侧 ArkUI-X 运行时框架目录，通常包含 `libarkui_ios.xcframework` 等产物
- `arkui-x/`：跨平台页面产物与系统资源目录

页面跳转链路如下：

1. iOS 系统启动应用并加载宿主 `AppDelegate`
2. `AppDelegate` 调用 `StageApplication` 初始化 ArkUI-X 运行时
3. 原生页面通过导航或展示控制器跳转到跨平台页面承载页
4. 承载页通过 `initWithInstanceName:` 绑定目标 Ability 并加载跨平台页面
5. 满足 URL Scheme 和映射规则时，跨平台页面也可通过 `startAbility()` 拉起原生页面
6. 宿主应用在 `application:openURL:options:` 中解析参数并完成原生页面跳转

## 前置条件

在开始接入前，请先在 DevEco Studio 中完成一次 ArkUI-X 工程构建，确保生成以下 iOS 集成产物：

- `arkui-x` 目录，包含模块配置、字节码文件和系统资源
- `frameworks` 目录，包含 `libarkui_ios.xcframework` 等动态链接库

建议在接入前先检查以下约束：

- iOS 示例中使用的 `bundleName` 与实际工程配置、URL Scheme 保持一致
- 已确认宿主工程具备接入 `arkui-x` 目录和 `frameworks` 目录的空间与权限

## 接入步骤

### 步骤 1：拷贝 ArkUI-X 页面产物

将 ArkUI-X 工程构建生成的整个 `arkui-x` 目录拷贝到 iOS 工程根目录，建议与 `.xcodeproj` 保持同级。

拷贝完成后，将 `arkui-x` 目录整体加入 Xcode 工程，并确认该目录已进入 `Build Phases -> Copy Bundle Resources`。

`arkui-x` 是跨平台页面运行时资源根目录，`StageApplication` 和 `StageViewController` 会从该目录查找页面字节码、模块配置和系统资源。如果 `arkui-x` 未完整打包，常见现象包括黑屏、资源加载失败或运行时异常。

常见目录如下：

- `arkui-x/entry/`：业务模块产物，通常包含 `ark_module.json`、`module.json`、`ets/modules.abc`、`resources/`
- `arkui-x/systemres/`：ArkUI-X 运行时系统资源

### 步骤 2：拷贝并引入 ArkUI-X 框架

将 ArkUI-X 工程构建生成的 `frameworks` 目录拷贝到 iOS 工程根目录。

目录示例如下：

```shell
frameworks/
├── libarkui_ios.xcframework
└── ...
```

在 Xcode 中打开现有 iOS 工程，将 `frameworks` 目录下的相关 `.xcframework` 添加到 App Target。

添加完成后，在 `General -> Frameworks, Libraries, and Embedded Content` 中确认相关框架均已加入，并将嵌入方式设置为 `Embed & Sign`。

![Frameworks 配置示意图](./figures/how-to-use-arkuix-on-nativePageLaunch_001.png)

### 步骤 3：修改 `AppDelegate`

在现有 iOS 工程的 `AppDelegate` 中引入 `StageApplication`，并在应用启动时完成 ArkUI-X 初始化。

参考实现：

```objc
#import "AppDelegate.h"
#import <libarkui_ios/StageApplication.h>

#define BUNDLE_DIRECTORY @"arkui-x"

- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
    [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
    [StageApplication launchApplication];
    return YES;
}
```

### 步骤 4：接入跨平台页面承载页

在现有 iOS 工程中新增一个承载 ArkUI 页面的控制器，例如 `EntryEntryAbilityViewController`，并让该控制器继承 `StageViewController`。

参考实现：

```objc
#import <libarkui_ios/StageViewController.h>

@interface EntryEntryAbilityViewController : StageViewController
@end

@implementation EntryEntryAbilityViewController

- (instancetype)initWithInstanceName:(NSString *)instanceName
{
    self = [super initWithInstanceName:instanceName];
    if (self) {
        self.view.backgroundColor = UIColor.whiteColor;
    }
    return self;
}

- (void)viewDidLoad
{
    [super viewDidLoad];
    self.edgesForExtendedLayout = UIRectEdgeNone;
    self.extendedLayoutIncludesOpaqueBars = YES;
}

@end
```

`initWithInstanceName:` 的值需要与 ArkUI-X 工程实际生成的模块信息保持一致，否则页面可能无法正确加载。

### 步骤 5：配置跨平台页面拉起原生页面的回调入口（可选）

如果当前只需要实现 iOS 原生页面跳转到跨平台页面，可跳过本步骤。

只有在需要支持“跨平台页面拉起 iOS 原生页面”时，才需要同时完成以下两项配置：

- 在 `Info.plist` 中配置 URL Scheme，用于接收 ArkUI 侧跳转请求
- 在 `AppDelegate` 中实现 `application:openURL:options:`，用于解析跳转参数并映射到目标原生页面

#### 5.1 配置 URL Scheme

`Info.plist` 配置示例如下：

```xml
<key>CFBundleURLTypes</key>
<array>
    <dict>
        <key>CFBundleURLSchemes</key>
        <array>
            <string>com.example.helloworld</string>
        </array>
    </dict>
</array>
```

其中，`CFBundleURLSchemes` 的值需要与 ArkUI 侧 `want.bundleName` 保持一致。

#### 5.2 实现 `openURL` 回调入口

在 `AppDelegate` 中实现 `application:openURL:options:`，用于接收 ArkUI 侧 `startAbility()` 发起的跳转请求，并解析 `bundleName`、`moduleName`、`abilityName` 和 `params`。

完整回调实现和页面映射示例可参考下文“跨平台页面跳转到 iOS 原生页面”章节。

## 页面跳转开发

### iOS 原生页面跳转到跨平台页面

在原生 `UIViewController` 中，通常通过创建 `StageViewController` 子类实例并完成页面跳转，例如 `EntryEntryAbilityViewController`。

`instanceName` 建议根据 `arkui-x/entry/ark_module.json` 中的模块信息拼接，格式如下：

```text
BundleName:ModuleName:AbilityName
```

#### 场景 1：无参跳转

如果只需要完成页面切换、不涉及参数传递，可直接使用以下方式。

```objc
#define BUNDLE_NAME @"com.example.helloworld"

- (void)pushArkUIPage
{
    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",
                              BUNDLE_NAME, @"entry", @"EntryAbility"];
    EntryEntryAbilityViewController *vc =
        [[EntryEntryAbilityViewController alloc] initWithInstanceName:instanceName];
    [self.navigationController pushViewController:vc animated:YES];
}
```

如果跳转时还需要向跨平台页面传递参数，可继续使用同一跳转方式，并通过承载控制器的 `params` 字段传参。

#### 场景 2：带参跳转

##### 方式 1：手动拼接 `params`

手动传参时，`params` 的值为 JSON 字符串。

参数格式和支持的参数类型可参考：

- [参数格式](../quick-start/start-with-ability-on-ios.md#1-使用手动方式)
- [支持的参数类型](../quick-start/start-with-ability-on-ios.md#支持的参数类型列表)
- [注意事项](../quick-start/start-with-ability-on-ios.md#注意事项)

Objective-C 示例：

```objc
#define BUNDLE_NAME @"com.example.helloworld"

- (void)pushArkUIPageWithParams
{
    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",
                              BUNDLE_NAME, @"entry", @"EntryAbility"];
    NSString *params =
        @"{\"params\":[{\"key\":\"keyfirst\",\"type\":1,\"value\":\"true\"},"
        "{\"key\":\"keysecond\",\"type\":9,\"value\":\"2.3\"},"
        "{\"key\":\"keythird\",\"type\":5,\"value\":\"2\"},"
        "{\"key\":\"keyfourth\",\"type\":10,\"value\":\"test\"}]}";

    EntryEntryAbilityViewController *vc =
        [[EntryEntryAbilityViewController alloc] initWithInstanceName:instanceName];
    vc.params = params;
    [self.navigationController pushViewController:vc animated:YES];
}
```

ArkTS 接收示例：

```ts
export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    console.log("value = " + want.parameters?.keyfirst)
    console.log("value = " + want.parameters?.keysecond)
    console.log("value = " + want.parameters?.keythird)
    console.log("value = " + want.parameters?.keyfourth)
  }
}
```

##### 方式 2：使用 `WantParams`（推荐）

推荐优先使用 `WantParams` 传递参数。该方式可读性更好，也更适合后续扩展复杂参数。

参数格式、支持类型和注意事项可参考：

- [参数格式](../quick-start/start-with-ability-on-ios.md#2-wantparams工具类)
- [支持的参数类型](../quick-start/start-with-ability-on-ios.md#支持的参数类型)
- [注意事项](../quick-start/start-with-ability-on-ios.md#注意事项)

Objective-C 示例：

```objc
#define BUNDLE_NAME @"com.example.helloworld"

- (void)pushArkUIPageWithWantParams
{
    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",
                              BUNDLE_NAME, @"entry", @"EntryAbility"];
    EntryEntryAbilityViewController *vc =
        [[EntryEntryAbilityViewController alloc] initWithInstanceName:instanceName];

    WantParams *childParams = [[WantParams alloc] init];
    [childParams addValue:@"stringKey2" value:@"It's me."];

    WantParams *wantParams = [[WantParams alloc] init];
    [wantParams addValue:@"stringKey" value:@"normal"];
    [wantParams addValue:@"intKey" value:@(-2147483648)];
    [wantParams addValue:@"doubleKey" value:@(-6.9)];
    [wantParams addValue:@"boolKey" value:@(YES)];
    [wantParams addValue:@"arrayKey" value:@[@(NO), @(YES)]];
    [wantParams addValue:@"wantParamsKey" value:childParams];

    vc.params = [wantParams toWantParamsString];
    [self.navigationController pushViewController:vc animated:YES];
}
```

ArkTS 接收示例：

```ts
export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    console.log("value = " + want.parameters?.stringKey)
    console.log("value = " + want.parameters?.intKey)
    console.log("value = " + want.parameters?.doubleKey)
    console.log("value = " + want.parameters?.boolKey)
    console.log("value = " + JSON.stringify(want.parameters?.arrayKey))
    console.log("value = " + JSON.stringify(want.parameters?.wantParamsKey))
  }
}
```

无参跳转适合验证集成链路。手动方式更适合简单基础类型，复杂类型和嵌套参数优先推荐使用 `WantParams`。

### 跨平台页面跳转到 iOS 原生页面

跨平台页面可以通过 `startAbility()` 拉起 iOS 原生页面。iOS 宿主侧通过 URL Scheme 触发 `AppDelegate` 的 `application:openURL:options:` 回调，并在回调中解析 `bundleName`、`moduleName`、`abilityName` 和 `params`，再根据映射关系跳转到对应的原生 `UIViewController`。

映射关系建议遵循以下规则：

- `bundleName` 需要与 iOS 工程中配置的 URL Scheme 保持一致
- `instanceName` 按 `bundleName:moduleName:abilityName` 规则拼接
- 宿主工程中的原生承接页需要根据 `moduleName` 和 `abilityName` 显式维护映射逻辑
- 若原生承接页也需要复用 ArkUI-X Stage 能力，建议按 `StageViewController` 子类方式实现，并通过 `initWithInstanceName:` 和 `params` 接收上下文

#### ArkUI 页面发起跳转

ArkTS 示例：

```ts
let want: Want = {
  bundleName: 'com.example.helloworld', //与iOS工程配置的URL Scheme相同
  moduleName: 'entry', //小写
  abilityName: 'Jump', //首字母大写
  parameters: { id: 1, name: 'ArkUI-X' } //可选参数
};

let context = getContext(this) as common.UIAbilityContext;
context.startAbility(want, (err, data) => {
});
```

#### iOS 宿主接收跳转回调

当 ArkUI 页面发起跳转后，iOS 宿主会触发 `application:openURL:options:` 回调。参考实现如下：

```objc
- (BOOL)application:(UIApplication *)app
            openURL:(NSURL *)url
            options:(NSDictionary<NSString *, id> *)options
{
    NSString *bundleName = url.scheme;
    NSString *moduleName = url.host;
    NSString *abilityName = nil;
    NSString *params = nil;

    NSURLComponents *urlComponents = [NSURLComponents componentsWithString:url.absoluteString];
    NSArray<NSURLQueryItem *> *array = urlComponents.queryItems;
    for (NSURLQueryItem *item in array) {
        if ([item.name isEqualToString:@"abilityName"]) {
            abilityName = item.value;
        } else if ([item.name isEqualToString:@"params"]) {
            params = item.value;
        }
    }

    if ([StageApplication handleSingleton:bundleName moduleName:moduleName abilityName:abilityName] == YES) {
        return YES;
    }

    [self handleOpenUrlWithBundleName:bundleName
                           moduleName:moduleName
                          abilityName:abilityName
                               params:params, nil];
    return YES;
}
```

`handleSingleton` 用于处理单实例 Ability 场景。常规页面跳转场景中，重点仍然是确保 `bundleName`、`moduleName`、`abilityName` 和 `params` 被正确解析。

#### 根据参数映射原生页面

在 `AppDelegate.m` 中，可通过 `handleOpenUrlWithBundleName:moduleName:abilityName:params:` 方法，将解析得到的参数映射到对应原生页面。

参考实现如下：

```objc
- (BOOL)handleOpenUrlWithBundleName:(NSString *)bundleName
                         moduleName:(NSString *)moduleName
                        abilityName:(NSString *)abilityName
                             params:(NSString *)params, ...NS_REQUIRES_NIL_TERMINATION
{
    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",
                              bundleName, moduleName, abilityName];

    if ([moduleName isEqualToString:@"entry"] && [abilityName isEqualToString:@"Jump"]) {
        EntryJumpViewController *jumpVC =
            [[EntryJumpViewController alloc] initWithInstanceName:instanceName];
        jumpVC.params = params;
        UINavigationController *nav = (UINavigationController *)self.window.rootViewController;
        [nav pushViewController:jumpVC animated:YES];
    }

    return YES;
}
```

这里需要注意：

- `bundleName` 需要与 `Info.plist` 中配置的 URL Scheme 一致
- `moduleName` 和 `abilityName` 需要与 ArkUI 侧传入值保持一致
- 原生页面映射逻辑需要在宿主工程中显式维护，`EntryJumpViewController` 这类目标页也应按当前映射规则实现
- ArkUI 侧传入的 `params` 可继续传递给目标 `UIViewController` 进行处理

## 快速检查清单

如果页面无法正常跳转，建议优先检查以下内容：

- `StageApplication` 是否已在 `AppDelegate` 中完成初始化
- `arkui-x` 目录是否已完整加入工程并打包进应用
- `frameworks` 中相关 `.xcframework` 是否已设置为 `Embed & Sign`
- `EntryEntryAbilityViewController` 是否继承自 `StageViewController`
- `instanceName` 是否与实际模块信息一致
- `URL Scheme` 是否与 ArkUI 侧 `bundleName` 一致
- `application:openURL:options:` 是否已正确实现
- 原生与 ArkUI 两侧的参数名是否一致

## 常见问题

### Q1：应用启动时崩溃，提示找不到 ArkUI-X 相关类或动态库

可能原因：

- 相关 framework 未添加到正确 Target
- `frameworks` 中的 `.xcframework` 未设置为 `Embed & Sign`
- `StageApplication` 未在 `AppDelegate` 中正确初始化

处理建议：

- 检查 `General -> Frameworks, Libraries, and Embedded Content` 中的配置
- 检查相关 framework 是否已加入正确 Target
- 检查 `didFinishLaunchingWithOptions` 中是否已调用 `configModuleWithBundleDirectory` 和 `launchApplication`

### Q2：跳转到跨平台页面后黑屏或崩溃

可能原因：

- `arkui-x` 未完整打包进应用
- `arkui-x` 加入工程时目录层级被破坏
- `instanceName` 与 ArkUI-X 构建产物中的实际模块信息不一致

处理建议：

- 检查 `arkui-x` 是否以完整目录形式打包进应用
- 检查 `arkui-x/entry/ets/modules.abc`、`ark_module.json`、`module.json` 是否存在
- 对照构建产物检查 `instanceName` 是否正确

### Q3：跨平台页面无法拉起 iOS 原生页面

可能原因：

- `Info.plist` 中未正确配置 URL Scheme
- ArkUI 侧 `want.bundleName` 与 iOS 配置不一致
- 宿主未正确实现 `application:openURL:options:`
- 原生页面映射逻辑未覆盖目标页面

处理建议：

- 检查 `CFBundleURLSchemes` 配置
- 检查 ArkUI 侧 `bundleName` 是否与 iOS 配置一致
- 检查 `AppDelegate` 是否实现了对应回调方法
- 检查目标原生页面是否已在映射逻辑中处理

### Q4：参数传递失败或原生页面映射失败

可能原因：

- `params` 内容格式不正确
- 手动方式传入了不支持的复杂类型
- `moduleName`、`abilityName` 解析错误
- ArkUI 侧读取的字段名与原生侧传入字段名不一致

处理建议：

- 检查 `params` 的内容是否符合约定格式
- 复杂类型优先使用 `WantParams`
- 检查 `moduleName`、`abilityName` 是否与 ArkUI 侧保持一致
- 检查两侧参数键名是否完全一致

## 项目结构示例

```shell
your-ios-app/
├── YourApp.xcodeproj/
├── YourApp/
│   ├── AppDelegate.m
│   ├── HomeViewController.m
│   ├── EntryEntryAbilityViewController.h
│   ├── EntryEntryAbilityViewController.m
│   ├── EntryJumpViewController.h
│   └── EntryJumpViewController.m
├── arkui-x/
│   ├── entry/
│   │   ├── ark_module.json
│   │   ├── module.json
│   │   ├── ets/modules.abc
│   │   └── resources/
│   └── systemres/
└── frameworks/
    ├── libarkui_ios.xcframework
    └── ...
```

## 参考资料

- [ArkUI-X iOS集成开发指南](./how-to-integrate-arkui-into-ios.md)
- [通过Stage模型开发iOS端应用指南](../quick-start/start-with-ability-on-ios.md)
- [Xcode官方文档](https://developer.apple.com/documentation/xcode)
