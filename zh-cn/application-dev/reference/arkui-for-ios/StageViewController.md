## StageViewController

```
UIResponder
    └── UIViewController
        └── StageViewController
```

```
@interface StageViewController : UIViewController
```

StageViewController是UIViewController的子类，是iOS应用的生命周期入口。ArkUI-X iOS平台应用开发时，必须实例化StageViewController，并完成JSBundle加载。

## 方法概要

| 类型         | 方法                 | 描述   |
| ------------ | -------------------- | ------ |
| instancetype | initWithInstanceName | 初始化 |
| BOOL | processBackPress | 是否由框架处理返回键 |
| BOOL | supportWindowPrivacyMode | 是否支持隐私模式 |

## 方法说明

- initWithInstanceName

```
/**
* Initializes this StageViewController with the specified instance name.
 *
 *  instanceName(bundleName:moduleName:abilityName)
 *  This is used for pure stage application. It will combine the instanceName as the
 *  abilityDirectory.
 *
 * @param instanceName instance name.
 */- (instancetype)initWithInstanceName:(NSString *_Nonnull)instanceName;
```

- processBackPress
具体用法请参考[IOS中实现跨平台页面左滑返回指南](../../quick-start/ios-slip-left-back.md)。

```
/**
 * processBackPress.
 * @return if ArkUI handle return true ,otherwise return false.
 * @since 11
 */
 - (BOOL)processBackPress;
```

- supportWindowPrivacyMode
```
/**
 * config privacy mode, if your ability need support privacy mode, please return YES, default is NO.
 * @since 20
 */
- (BOOL)supportWindowPrivacyMode;
```

### StageViewController跟Ability映射命名规则
![](../../../figures/StageIos.png)