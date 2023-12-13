# ProcessInformation

ProcessInformation模块提供对进程运行信息进行查询的能力。

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 使用说明

通过ApplicationContext的[getRunningProcessInformation](js-apis-inner-application-applicationContext.md#getRunningProcessInformation)来获取。

```ts
applicationContext.getRunningProcessInformation((err, data) => {
    if (err) {
        console.error('getRunningProcessInformation faile, err: ${JSON.stringify(err)}');
    } else {
        console.log('The process running information is: ${JSON.stringify(data)}');
    }
})
```

## 属性

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityRuntime.Core

| 名称 | 类型 | 可读 | 可写 | 说明 |
| -------- | -------- | -------- | -------- | -------- |
| pid | number | 是 | 否 | 进程ID。 |
| processName | string | 是 | 否 | 进程名称。 |
| bundleNames | Array&lt;string&gt; | 是 | 否 | 进程中所有运行的Bundle名称。 |
