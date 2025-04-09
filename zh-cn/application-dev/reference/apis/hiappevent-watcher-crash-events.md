# 崩溃事件介绍

HiAppEvent提供接口用于订阅系统崩溃事件。

- [订阅崩溃事件（ArkTS）](hiappevent-watcher-crash-events-arkts.md)

崩溃事件信息中params属性的详细描述如下：

**params属性：**

| 名称    | 类型   | 说明                       |
| ------- | ------ | ------------------------- |
| time     | number | 事件触发时间，单位为毫秒。 |
| crash_type | string | 崩溃类型。支持JsError崩溃类型。 |
| foreground | boolean | 应用是否处于前台状态。 |
| bundle_version | string | 应用版本。 |
| bundle_name | string | 应用名称。 |
| pid | number | 应用的进程id。|
| uid | number | 应用的用户id。 |
| exception | object | 异常信息，详见exception属性。 |

**exception属性：**

| 名称    | 类型   | 说明                       |
| ------- | ------ | ------------------------- |
| name | string | 异常类型。 |
| message | string | 异常原因。 |
| stack | string | 异常调用栈。 |
