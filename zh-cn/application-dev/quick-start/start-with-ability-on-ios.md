# Stage模型iOS端开发文档

## iOS端APP开发指南

### 关键依赖集成

* hap包, 导入到程序bundle路径内
* arkui-X构建的libarkui_ios.xcframework库
  **注** Xcode链接库需设置Embed&Sign

### Xcode配置

* info文件内的URL Types、Queried URL Schemes需正确配置对应应用的bundleName，用于应用路由模式页面跳转
* Build Setting -> Enable Bitcode -> NO
* Minmum Deployments -> iOS10及以上

### 关键类

#### StageViewController

StageViewController是stage模型iOS端视图控制器基类，若要实现跨平台基础能力及触发对应ability生命周期，所有iOS端视图控制器均要继承于StageViewController。

##### 公共属性：

* instanceName：StageViewController唯一标识，拼接规则为bundleName:moduleName:abilityName

```objc
@property (nonatomic, readonly) NSString *instanceName;
```

* params：StageViewController外部属性，用于startAbility时传递的want参数。

```objc
@property (nonatomic, strong) NSString *params;
```

##### 初始化方法：

```objc
- (instancetype)initWithInstanceName:(NSString *_Nonnull)instanceName;
```


#### StageApplication

StageApplication本质上是一个调度类，其作用主要用于触发内部相关类实现路径解析与配置、注册相关configuration信息、触发ability部分生命周期事件等。

##### 公共方法

* 配置本地hap包路径

```objc
+ (void)configModuleWithBundleDirectory:(NSString *_Nonnull)bundleDirectory;
```

* 启动ability、配置进程id、本地化信息、configuration等

```objc
+ (void)launchApplication;
```

* 触发ability进入前台生命周期事件

```objc
+ (void)callCurrentAbilityOnForeground;
```

* 触发ability进入后台生命周期事件

```objc
+ (void)callCurrentAbilityOnBackground;
```

* 处理单/多实例ability

```objc
+ (BOOL)handleSingleton:(NSString *)bundleName moduleName:(NSString *)moduleName abilityName:(NSString *)abilityName;
```

* 释放导航视图栈内的所有viewcontroller，触发OnDestory事件

```objc
+ (void)releaseViewControllers;
```

* 获取导航视图栈最顶层viewcontroller

```objc
+ (StageViewController *)getApplicationTopViewController;
```


### AppDelegate内关键实现参考

#### 程序启动及初始化：

```objc
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions {

    // 配置hap包路径
    [StageApplication configModuleWithBundleDirectory:@"arkui-x"];
    // 启动ability
    [StageApplication launchApplication];
    
    // APP自启动,初始化StageViewController子类VC，并设置为APP根视图控制器
    if (!launchOptions.count) { 
        NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",@"com.example.iosabilitystage", @"entry", @"MainAbility"];
        EntryMainViewController *mainView = [[EntryMainViewController alloc] initWithInstanceName:instanceName];
        [self setNavRootVC:mainView];
    }
    return YES;
}
```

#### 通过路由模式（openURL:）实现的页面跳转回调：

```objc
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString *,id> *)options {

    // 根据规则截取URL相应参数
    NSString *bundleName = url.scheme;
    NSString *moduleName = url.host;
    NSString *abilityName, *params;

    NSURLComponents *urlComponents = [NSURLComponents componentsWithString:url.absoluteString];
    NSArray <NSURLQueryItem *> *array = urlComponents.queryItems;
    for (NSURLQueryItem * item in array) {
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

- (BOOL)handleOpenUrlWithBundleName:(NSString *)bundleName
                         moduleName:(NSString *)moduleName
                        abilityName:(NSString *)abilityName
                             params:(NSString *)params, ...NS_REQUIRES_NIL_TERMINATION {
    
    UIViewController *rootVC = [[UIApplication sharedApplication].delegate window].rootViewController;
    
    BOOL hasRoot = NO;
    if ([rootVC isKindOfClass:[UINavigationController class]]) {
        hasRoot = YES;
    }
    
    id subStageVC = nil;
    NSString *instanceName = [NSString stringWithFormat:@"%@:%@:%@",bundleName, moduleName, abilityName];
    
    // 跳转ability对应的viewtroller
    if ([bundleName isEqualToString:BUNDLE_NAME] &&
               [abilityName isEqualToString:@"MainAbility"]) {
        EntryMainAbilityViewController *entryMainVC = [[EntryMainAbilityViewController alloc] initWithInstanceName:instanceName];
        entryMainVC.params = params;
        subStageVC = (EntryMainAbilityViewController *)entryMainVC;
    }else if ([bundleName isEqualToString:BUNDLE_NAME] && [abilityName isEqualToString:@"Other"]) {
        EntryOtherViewController *entryOtherVC = [[EntryOtherViewController alloc] initWithInstanceName:instanceName];
        entryOtherVC.params = params;
        subStageVC = (EntryOtherViewController *)entryOtherVC;
    }

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
```

#### 其它程序级生命周期回调相应处理：

* 程序进入后台，触发对应生命周期事件

```objc
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [StageApplication callCurrentAbilityOnBackground];
}
```

* 程序进入前台，触发对应生命周期事件

```objc
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [StageApplication callCurrentAbilityOnForeground];
}
```

* 终止程序进程

```objc
- (void)applicationWillTerminate:(UIApplication *)application {
    [StageApplication releaseViewControllers];
}
```

#### 其它私有方法

* 设置根视图

```objc
- (void)setNavRootVC:(id)viewController {
    self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
    self.window.backgroundColor = [UIColor whiteColor];
    [self.window makeKeyAndVisible];
    UINavigationController *navi = [[UINavigationController alloc]initWithRootViewController:viewController];
    [self setNaviAppearance:navi];
    self.window.rootViewController = navi;
}
```

* 调整导航栏样式

```objc
- (void)setNaviAppearance:(UINavigationController *)navi {
    UINavigationBarAppearance *appearance = [UINavigationBarAppearance new];
    [appearance configureWithOpaqueBackground];
    appearance.backgroundColor = UIColor.whiteColor;
    navi.navigationBar.standardAppearance = appearance;
    navi.navigationBar.scrollEdgeAppearance = navi.navigationBar.standardAppearance;
}
```