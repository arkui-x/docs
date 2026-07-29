# CommonEventData

公共事件的数据。

> **说明：**
>
> 本模块首批接口从API version 11开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 属性

**系统能力：** 以下各项对应的系统能力均为SystemCapability.Notification.CommonEvent

| 名称          | 类型                 | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------------- | -------------------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| event         | string               | 是   | 否   | 表示当前接收的公共事件名称。                                 | 支持        | 不支持  |
| data          | string               | 是   | 否   | 表示订阅者接收到的公共事件数据（string类型）。该字段取值与发布者使用[commonEventManager.publish](./js-apis-commonEventManager.md#commoneventmanagerpublish-1)发布公共事件时，通过[CommonEventPublishData](./js-apis-inner-commonEvent-commonEventPublishData.md)中的`data`字段传递的数据一致。 | 支持        | 不支持  |
| bundleName26+ | string               | 是   | 否   | 表示包名称，默认为空字符串。                                 | 支持        | 不支持  |
| code26+       | number               | 是   | 否   | 表示订阅者接收到的公共事件数据（number类型）。该字段取值与发布者使用[commonEventManager.publish](./js-apis-commonEventManager.md#commoneventmanagerpublish-1)发布公共事件时，通过[CommonEventPublishData](./js-apis-inner-commonEvent-commonEventPublishData.md)中的`code`字段传递的数据一致。默认值为0。 | 支持        | 不支持  |
| parameters26+ | {[key: string]: any} | 是   | 否   | 表示订阅者接收到的公共事件的附加信息。该字段取值与发布者使用[commonEventManager.publish](./js-apis-commonEventManager.md#commoneventmanagerpublish-1)发布公共事件时，通过[CommonEventPublishData](./js-apis-inner-commonEvent-commonEventPublishData.md)中的`parameters`字段传递的数据一致。 | 支持        | 不支持  |