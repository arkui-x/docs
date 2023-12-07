## StageApplication

```
NSObject
    └── StageApplication
```

StageApplication负责iOS应用初始化流程。

## 方法概要

| 类型         | 方法                 | 描述   |
| ------------ | -------------------- | ------ |
| void | configModuleWithBundleDirectory | 配置工程内Hap包路径 |
| void | launchApplication | 调度application初始化流程 |

## 方法说明


+ configModuleWithBundleDirectory:;

```
/**
* Configure the Hap package path in the iOS project.
 *
 *  Configure the Hap package path : bundleDirectory
 *  This path is within the iOS project.
 *
 * @param bundleDirectory hap path.
 */+ (void)configModuleWithBundleDirectory:(NSString *_Nonnull)bundleDirectory;
```

+ launchApplication;

```
/**
* Schedule the application initialization process.
 *
 *  When the stage ability launching.
 *  The iOS project requires scheduling the application initialization process.
 *
 */+ (void)launchApplication;
```