# ProcessInformation

ProcessInformation模块提供对进程运行信息进行查询的能力。

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 属性

| 名称 | 类型 | 可读 | 可写 | 说明 |
| -------- | -------- | -------- | -------- | -------- |
| pid | number | 是 | 否 | 进程ID。 |
| processName | string | 是 | 否 | 进程名称。 |
| bundleNames | Array&lt;string&gt; | 是 | 否 | 进程中所有运行的Bundle名称。 |
