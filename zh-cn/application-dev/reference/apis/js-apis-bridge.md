# @arkui-x.bridge.d.ts (平台桥接)

本模块提供ArkUI端和Android或iOS平台端消息通信的功能，包括数据传输、方法调用和事件调用。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```javaScript
import bridge from '@arkui-x.bridge';
```
## createBridge

createBridge(bridgeName: string): BridgeObject

定义BridgeObject类。

**系统能力：**   SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型   | 必填 | 说明           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | string | 是   | 定义桥接名称。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| [BridgeObject](#bridgeobject) | 桥接的接口类。 |

**示例：** 

  ```javaScript
  const bridgeObj: BridgeObject = bridge.createBridge('Bridge');
  ```

## BridgeObject

桥接的接口类。

### callMethod

callMethod(methodName: string, parameters?: Record\<string, Parameter\>): Promise\<ResultValue\>;

callMethod(methodName: string, ...parameters: Array\<any\>): Promise\<ResultValue\>;

调用平台方法。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型                      | 必填 | 说明           |
| ---------- | ------------------------- | ---- | -------------- |
| methodName | string                    | 是   | 方法名称。     |
| parameters | Record\<string, Parameter\> | 否   | 方法参数列表。 |
| parameters | Array\<any\>                | 否   | 方法参数列表。 |

**返回值：** 

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | 平台方法执行结果。 |

**错误码：**

| 错误码ID   | 错误信息 |
| --------- | ------- |
| 1 | 管道不可用。 |
| 4 | 方法名称错误。 |
| 5 | 方法正确运行，不能重复运行。 |
| 6 | 方法未实现。 |


**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.callMethod('nativeMethod').then((data)=>{
    console.log('data = ' + data);
}).catch((err) => {
    console.error('error = ' + JSON.stringify(err));
});
```

### registerMethod

registerMethod(method: MethodData, callback: AsyncCallback\<void\>): void

registerMethod(method: MethodData): Promise\<void\>

注册ArkUI端方法，供Android或iOS平台端调用。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名   | 类型                | 必填 | 说明                       |
| -------- | ------------------- | ---- | -------------------------- |
| method   | MethodData          | 是   | 方法数据。                 |
| callback | AsyncCallback\<void\> | 否   | callback方式的回调函数。 |

**错误码：**

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 1        | 管道不可用。                     |
| 8        | 方法已经被注册，不允许重复注册。 |

**示例：**

```javaScript
function jsMethod() {
  return 'ts return：jsMethod';
}

const bridgeObj = bridge.createBridge('Bridge');
bridgeObj.registerMethod({ name: 'jsMethod', method: jsMethod });
```

### unRegisterMethod

unRegisterMethod(methodName: string, callback: AsyncCallback\<void\>): void

unRegisterMethod(methodName: string): Promise\<void\>

移除已注册的ArkUI端的方法。

**参数：** 

| 参数名   | 类型                | 必填 | 说明                       |
| -------- | ------------------- | ---- | -------------------------- |
| method   | string              | 是   | 方法名称。                 |
| callback | AsyncCallback\<void\> | 否   | callback方式的回调函数。 |

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.unRegisterMethod('jsMethod');
```

### sendMessage

sendMessage(message: Message, callback: AsyncCallback\<Response\>): void

sendMessage(message: Message): Promise\<Response\>

向Platform平台侧发送数据。

**参数：** 

| 参数名   | 类型                | 必填 | 说明                       |
| -------- | ------------------- | ---- | -------------------------- |
| message  | [Message](#message) | 是   | 数据。                     |
| callback | AsyncCallback\<void\> | 否   | callback方式的回调函数。 |

**返回值：** 

| 类型                  | 说明                     |
| --------------------- | ------------------------ |
| [Response](#response) | Platform平台侧应答数据。 |

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.sendMessage('jsMessage').then((data)=>{
    console.log('data =' + data);
}).catch((err) => {
    console.error('error =' + JSON.stringify(err));
});
```

### setMessageListener

setMessageListener(callback: (message: Message) => Response)

设置用于接收Platform平台侧发送数据的回调。

**参数：** 

| 参数名   | 类型                         | 必填 | 说明                               |
| -------- | ---------------------------- | ---- | ---------------------------------- |
| callback | (message: Message)=>Response | 是   | 回调函数，接收Platform平台侧数据。 |
| message  | [Message](#message)          | 是   | Platform平台侧数据。               |

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.setMessageListener((data) => {
    console.log('receive data =' + data);
});
```



## S 

type S = number | boolean | string | null

**说明：** 定义桥接使用的基础数据类型。

## T

type T  = S | Array\<number\> | Array\<boolean\> | Array\<string\>

**说明：** 定义桥接使用的基础数据类型的数组类型。

## Message

type Message = T | Record\<string, T\>

**说明：** 定义桥接使用结构数据类型。

## Parameter

type Parameter = Message

**说明：** 定义方法参数类型。

## Response

type Response = Message

**说明：** 定义应答的数据类型。

## ResultValue

type ResultValue = T | Map\<string, T\>

**说明：** 定义方法返回值的类型。
