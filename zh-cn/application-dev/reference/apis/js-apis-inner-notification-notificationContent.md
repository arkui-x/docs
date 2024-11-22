# NotificationContent

描述通知类型。

> **说明：**
>
> 本模块首批接口从API version 12开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## NotificationContent

**系统能力**：以下各项对应的系统能力均为SystemCapability.Notification.Notification

| 名称           | 类型                                                                        | 只读 | 可选 | 说明               |
| -----------   | --------------------------------------------------------------------------- | ---- | --- | ------------------ |
| notificationContentType    | [notificationManager.ContentType](js-apis-notificationManager.md#contenttype)                | 否  | 是  | 通知内容类型。       |
| normal         | [NotificationBasicContent](#notificationbasiccontent)                      | 否  | 是  | 基本类型通知内容。   |
| longText       | [NotificationLongTextContent](#notificationlongtextcontent)                | 否  | 是  | 长文本类型通知内容。 |
| multiLine      | [NotificationMultiLineContent](#notificationmultilinecontent)              | 否  | 是  | 多行类型通知内容。   |

## NotificationBasicContent

描述普通文本通知。

**系统能力**：以下各项对应的系统能力均为SystemCapability.Notification.Notification

| 名称           | 类型    | 只读 | 可选 | 说明                               |
| -------------- | ------ | ---- |-----| ---------------------------------- |
| title          | string |  否  |  否  | 通知标题（不可为空字符串，大小不超过200字节，超出部分会被截断）。         |
| text           | string |  否  |  否  | 通知内容（不可为空字符串，大小不超过200字节，超出部分会被截断）。         |

## NotificationLongTextContent

描述长文本通知。

> **说明：**
>
> 实际显示效果依赖于设备能力和通知中心UI样式。

**系统能力**：以下各项对应的系统能力均为SystemCapability.Notification.Notification

| 名称           | 类型    | 只读 | 可选 | 说明                             |
| -------------- | ------ | ---- | --- | -------------------------------- |
| title          | string |  否  | 否  | 通知标题（不可为空字符串，大小不超过200字节，超出部分会被截断）。                         |
| text           | string |  否  | 否  | 通知内容（不可为空字符串，大小不超过200字节，超出部分会被截断）。                         |
| longText       | string |  否  | 否  | 通知的长文本（不可为空字符串，大小不超过1024字节，超出部分会被截断），iOS不支持。                     |
| briefText      | string |  否  | 否  | 通知概要内容，是对通知内容的总结（不可为空字符串，大小不超过200字节，超出部分会被截断），iOS不支持。   |
| expandedTitle  | string |  否  | 否  | 通知展开时的标题（不可为空字符串，大小不超过200字节，超出部分会被截断），iOS不支持。                 |


## NotificationMultiLineContent

描述多行文本通知。

> **说明：**
>
> 实际显示效果依赖于设备能力和通知中心UI样式。

**系统能力**：以下各项对应的系统能力均为SystemCapability.Notification.Notification

| 名称           | 类型            | 只读 | 可选 | 说明                             |
| -------------- | --------------- | --- | --- | -------------------------------- |
| title          | string          | 否  | 否  | 通知标题（不可为空字符串，大小不超过200字节，超出部分会被截断）。       |
| text           | string          | 否  | 否  | 通知内容（不可为空字符串，大小不超过200字节，超出部分会被截断）。       |
| briefText      | string          | 否  | 否  | 通知概要内容，是对通知内容的总结（不可为空字符串，大小不超过200字节，超出部分会被截断），iOS不支持。 |
| longTitle      | string          | 否  | 否  | 通知展开时的标题（不可为空字符串，大小不超过200字节，超出部分会被截断），iOS不支持。|
| lines          | Array\<string\> | 否  | 否  | 通知的多行文本（大小不超过200字节，超出部分会被截断），iOS不支持。                  |
