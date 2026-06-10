# CommonEventPublishData

包含公共事件内容和属性。

> **说明：**
>
> 本模块首批接口从API version 7开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 属性

**系统能力：** 以下各项对应的系统能力均为SystemCapability.Notification.CommonEvent

| 名称                  | 类型                 | 可读 | 可写 | 说明                         |
| --------------------- | -------------------- | ---- | ---- | ---------------------------- |
| data                  | string               | 是  | 否  | 表示发布方传递的公共事件数据（string类型）。数据大小不超过64KB。 |
| bundleName<sup>26+</sup> | string | 是 | 否 | 表示订阅者包名称，只有包名为bundleName的订阅者才能收到该公共事件。 |
| code<sup>26+</sup> | number | 是 | 否 | 表示发布方传递的公共事件数据（number类型）。默认值为0。 |
| parameters<sup>26+</sup> | {[key: string]: any} | 是 | 否 | 表示发布方传递的公共事件的附加信息。 |