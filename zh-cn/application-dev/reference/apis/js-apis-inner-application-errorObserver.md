# ErrorObserver

定义异常监听，可以作为[ErrorManager.on](js-apis-app-ability-errorManager.md#errormanageronerror)的入参监听当前应用发生的异常。

> **说明：**
> 
> 本模块首批接口从API version 19开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。 

## 导入模块

```ts
import { errorManager } from '@kit.AbilityKit';
```

## ErrorObserver.onUnhandledException

onUnhandledException(errMsg: string): void;

将在js运行时引发用户未捕获的异常时调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| errMsg | string | 是 | 有关异常的消息和错误堆栈跟踪。 |

**示例：**

```ts
import { errorManager } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let observer: errorManager.ErrorObserver = {
  onUnhandledException(errorMsg) {
    console.error('onUnhandledException, errorMsg: ', errorMsg);
  }
};

try {
  errorManager.on('error', observer);
} catch (error) {
  console.error(`registerErrorObserver failed, error.code: ${(error as BusinessError).code}, error.message: ${(error as BusinessError).message}`);
}
```

## ErrorObserver.onException

onException?(errObject: Error): void;

将在js运行时引发用户未捕获的异常时调用。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| errObject | Error | 是 | 有关异常事件名字、消息和错误堆栈信息的对象。 |

**示例：**

```ts
import { errorManager } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let observer: errorManager.ErrorObserver = {
  onUnhandledException(errorMsg) {
    console.error('onUnhandledException, errorMsg: ', errorMsg);
  },
  onException(errorObj) {
    console.log('onException, name: ', errorObj.name);
    console.log('onException, message: ', errorObj.message);
    if (typeof (errorObj.stack) === 'string') {
      console.log('onException, stack: ', errorObj.stack);
    }
  }
};

try {
  errorManager.on('error', observer);
} catch (error) {
  console.error(`registerErrorObserver failed, error.code: ${(error as BusinessError).code}, error.message: ${(error as BusinessError).message}`);
}
```
