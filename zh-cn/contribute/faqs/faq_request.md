# FAQ：不同安卓机型使用ohos.request的部分接口，可能存在不同表现<a name="ZH-CN_TOPIC_0000001051065413"></a>

使用 [request.downloadFile](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md#requestdownloadfile9) 
或 [request.agent.create](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md#requestagentcreate10) 接口时，在部分安卓机型上表现可能不同。

## 复杂类问题<a name="section13861135611514"></a>

**现象描述**

-   使用的系统软件为 Android 12，硬件版本为部分安卓机型。
-   跨平台项目中使用 [request.downloadFile](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md#requestdownloadfile9) 接口时，收到 [on('fail')](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md#onfail7) 失败回调，失败错误码为8（ERROR_UNKNOWN）。
-   跨平台项目中使用 [request.agent.create](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md#requestagentcreate10) 接口时，收到 [on('failed')](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md#onfailed10) 失败回调，
查询 [TaskInfo](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-request.md#taskinfo10)，faults为 0x20，reason为“Request error”。

**可能原因**

部分安卓机型底层对于分块下载或断点续传功能的支持度不同。
遇到206 Partial Content响应码时处理逻辑存在区别。

**处理步骤**

-   检查失败场景，查看是否存在请求头里设置了"range"相关参数，服务端是否返回了206 Partial Content响应码。
-   若存在上述情况，删除请求头中的"range"相关参数，尝试请求完整数据。
-   重新发起请求，查看是否请求成功。
