# @ohos.bundle.bundleManager (bundleManager模块)

本模块提供应用信息查询能力，支持BundleInfo、ApplicationInfo、Ability、ExtensionAbility等信息的查询

> **说明：**
> 本模块首批接口从API version 9 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import bundleManager from '@ohos.bundle.bundleManager';
```

## 枚举

### LaunchType

指示组件的启动方式。

| 名称 | 值 | 说明 |
|:----------------:|:---:|:---:|
| SINGLETON        | 0   | ability的启动模式，表示单实例。 |
| MULTITON         | 1   | ability的启动模式，表示普通多实例。 |

### DisplayOrientation

标识该Ability的显示模式。该标签仅适用于page类型的Ability。

| 名称                               |值 |说明 |
|:----------------------------------|---|---|
| UNSPECIFIED                        |0 |表示未定义方向模式，由系统判定。 |
| LANDSCAPE                          |1 |表示横屏显示模式。 |
| PORTRAIT                           |2 |表示竖屏显示模式。 |
| FOLLOW_RECENT                      |3 |表示跟随上一个显示模式。 |
| LANDSCAPE_INVERTED                 |4 |表示反向横屏显示模式。 |
| PORTRAIT_INVERTED                  |5 |表示反向竖屏显示模式。 |
| AUTO_ROTATION                      |6 |表示传感器自动旋转模式。 |
| AUTO_ROTATION_LANDSCAPE            |7 |表示传感器自动横向旋转模式。 |
| AUTO_ROTATION_PORTRAIT             |8 |表示传感器自动竖向旋转模式。 |
| AUTO_ROTATION_RESTRICTED           |9 |表示受开关控制的自动旋转模式。 |
| AUTO_ROTATION_LANDSCAPE_RESTRICTED |10|表述受开关控制的自动横向旋转模式。|
| AUTO_ROTATION_PORTRAIT_RESTRICTED  |11|表示受开关控制的自动竖向旋转模式。|
| LOCKED                             |12|表示锁定模式。|

### ModuleType

标识模块类型。

| 名称    | 值   | 说明                 |
| ------- | ---- | -------------------- |
| ENTRY   | 1    | 应用的主模块。   |
| FEATURE | 2    | 应用的动态特性模块。 |
| SHARED  | 3    | 应用的动态共享库模块。  |

### BundleType

标识应用的类型。

| 名称           | 值   | 说明            |
| -------------- | ---- | --------------- |
| APP            | 0    | 该Bundle是普通应用程序。    |
| ATOMIC_SERVICE | 1    | 该Bundle是原子化服务。 |