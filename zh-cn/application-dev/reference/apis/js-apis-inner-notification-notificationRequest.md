# NotificationRequest

描述通知的请求。

> **说明：**
>
> 本模块首批接口从API version 12开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## NotificationRequest

**系统能力**：以下各项对应的系统能力均为SystemCapability.Notification.Notification

| 名称                            | 类型                                                    |  只读 | 可选 | 说明                                                                    |
|-------------------------------| -------------------------------------------------------- | ----- | --- |-----------------------------------------------------------------------|
| content                       | [NotificationContent](js-apis-inner-notification-notificationContent.md#notificationcontent)   |   否  | 否  | 通知内容。                                                                 |
| id                            | number                                                   |   否  | 是  | 通知ID，默认为0。                                                                 |
| isOngoing                     | boolean                                                  |   否  | 是  | 是否进行时通知，iOS不支持。                                                               |
| deliveryTime                  | number                                                   |   否  | 是  | 通知发送时间，iOS不支持。                                                               |
| tapDismissed                  | boolean                                                  |   否  | 是  | 通知是否自动清除，iOS不支持。                                                             |
| autoDeletedTime               | number                                                   |   否  | 是  | 自动清除的时间，iOS不支持。                                                              |
| isAlertOnce                   | boolean                                                  |   否  | 是  | 设置是否仅有一次此通知提醒，iOS不支持。                                                        |
| showDeliveryTime              | boolean                                                  |   否  | 是  | 是否显示分发时间，iOS不支持。                                                             |
| groupName                     | string                                                   |   否  | 是  | 组通知名称。                                                                |
| badgeNumber                   | number                                                   |   否  | 是  | 应用程序图标上显示的通知数。                                                        |
