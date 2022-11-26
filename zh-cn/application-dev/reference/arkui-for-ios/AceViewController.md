# AceViewController

```
UIResponder
    └── UIViewController
        └── AceViewController
```

```
@interface AceViewController : UIViewController
```

AceViewController是UIViewController的子类，是iOS应用的生命周期入口。ArkUI-X iOS平台应用开发时，必须实例化AceViewController，并完成ArkUI开发范式设置和JSBundle加载。

## 方法概要

| 类型    | 方法             | 描述 |
| ----------- | ----------------------------------- | ---- |
| instancetype | initWithVersion | 设置ArkUI JSBundle实例名和开发范式类型|

## 方法说明

- initWithVersion

```
/**
 * Initializes this AceViewController with the specified instance name.
 *
 *  This is used for pure ace application. It will combine the js/`instanceName` as the
 *  bundleDirectory.
 *
 * @param version  ACE version, can be one of this:
 *                ACE_VERSION_JS / ACE_VERSION_ETS,
 * @param instanceName instance name.
 */
- (instancetype)initWithVersion:(ACE_VERSION)version
                  instanceName:(nonnull NSString*)instanceName;
```
