# LoopObserver

定义异常监听，可以作为[ErrorManager.on](./js-apis-app-ability-errorManager.md#errormanageronloopobserver)的入参监听当前应用主线程事件处理事件。

> **说明：**
> 
> 本模块首批接口从API version 19开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。 

## 导入模块

```ts
import { errorManager } from '@kit.AbilityKit';
```

## LoopObserver.onLoopTimeOut

onLoopTimeOut?(timeout: number): void

将在js运行时应用主线程处理事件超时的回调。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| timeout | number | 是 | 返回应用主线程消息实际执行时间。 |

**示例：**

```ts
import { errorManager } from '@kit.AbilityKit';

let observer: errorManager.LoopObserver = {
  onLoopTimeOut(timeout: number) {
    console.log('Duration timeout: ' + timeout);
  }
};

errorManager.on("loopObserver", 1, observer);
```
