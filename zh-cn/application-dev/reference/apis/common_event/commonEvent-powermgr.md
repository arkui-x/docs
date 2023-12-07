# 电源管理子系统公共事件定义
电源管理子系统面向应用发布如下系统公共事件，应用如需订阅系统公共事件，请参考公共事件[接口文档](../js-apis-commonEventManager.md)。

## COMMON_EVENT_BATTERY_LOW
表示电池电量低的普通事件的动作。

- 值： usual.event.BATTERY_LOW
- 订阅者所需权限： 无

当电池电量低于设备设置的低电量百分比值时，将会触发事件通知服务发布该系统公共事件。

## COMMON_EVENT_BATTERY_OKAY
表示电池退出低电量状态的公共事件的动作。

- 值： usual.event.BATTERY_OKAY
- 订阅者所需权限： 无

当电池电量从低电量等级变化到电池电量高于低电量等级时，将会触发事件通知服务发布该系统公共事件。

## COMMON_EVENT_SCREEN_ON
表示设备屏幕打开且设备处于交互状态的公共事件的操作。

- 值： usual.event.SCREEN_ON
- 订阅者所需权限： 无

当设备屏幕打开且设备处于交互状态时，将会触发事件通知服务发布该系统公共事件。

## COMMON_EVENT_POWER_SAVE_MODE_CHANGED
表示系统节能模式更改的公共事件的动作。

- 值： usual.event.POWER_SAVE_MODE_CHANGED
- 订阅者所需权限： 无

当系统节能模式更改时，将会触发事件通知服务发布该系统公共事件。
