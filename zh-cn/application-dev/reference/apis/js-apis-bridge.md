# @arkui-x.bridge.d.ts (平台桥接)

本模块将详细阐述平台桥接**ArkTS端**的API方法及相关定义。<br>Android侧请参考[BridgePlugin](../arkui-for-android/BridgePlugin.md)，iOS侧参考[BridgePlugin](../arkui-for-ios/BridgePlugin.md)。<br>
关于数据类型的说明详见[平台桥接数据类型](../../quick-start/platform-bridge-introduction.md#数据类型支持)。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```javaScript
import bridge from '@arkui-x.bridge';
```
## createBridge

createBridge(bridgeName: string): BridgeObject

创建BridgeObject类的实例对象，数据编解码类型采用JSON格式。

**系统能力：**   SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型   | 必填 | 说明           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | string | 是   | 定义平台桥接对象名称，需要与平台侧定义的名称保持一致 |

**返回值：** 

| 类型                          | 说明           |
| ----------------------------- | -------------- |
| [BridgeObject](#bridgeobject) | 桥接的接口类实例 |

**示例：** 

  ```javaScript
  const bridgeObj: bridge.BridgeObject = bridge.createBridge('Bridge');
  ```

createBridge(bridgeName: string, type: BridgeType): BridgeObject;

创建BridgeObject类的实例对象，自行设定编解码类型。<br>要求bridgeName与原生平台侧创建的平台桥接对象的bridgeName保持一致<br>要求type与原生平台侧创建的平台桥接对象的type保持一致

**系统能力：**   SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型       | 必填 | 说明                                 |
| ---------- | ---------- | ---- | ------------------------------------ |
| bridgeName | string     | 是   | 定义平台桥接对象名称，需要与原生平台侧设置的名称保持一致                       |
| bridgeType | [BridgeType](#bridgetype) | 是   | 定义平台桥接对象编解码类型，需要与原生平台侧设置的类型保持一致 |

**返回值：** 

| 类型                          | 说明           |
| ----------------------------- | -------------- |
| [BridgeObject](#bridgeobject) | 桥接的接口类实例 |

**示例：** 

  ```javaScript
  const bridgeObj: bridge.BridgeObject = bridge.createBridge('Bridge', bridge.BridgeType.BINARY_TYPE);
  ```

## BridgeObject

平台桥接的接口类。

### callMethod

callMethod(methodName: string, ...parameters: Array\<Parameter\>): Promise\<ResultValue\>;

调用原生平台侧已定义的方法。<br>接口具体使用方式详见[平台桥接开发指南-Android](../../tutorial/how-to-use-bridge-on-android.md#场景三arkui侧调用android侧的方法)或[平台桥接开发指南-iOS](../../tutorial/how-to-use-bridge-on-ios.md#场景三arkui侧调用ios侧的方法)。<br>
关于数据类型的说明详见[平台桥接数据类型](../../quick-start/platform-bridge-introduction.md#数据类型支持)。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型               | 必填 | 说明                 |
| ---------- | ------------------ | ---- | -------------------- |
| methodName | string             | 是   | 被调用的原生平台的方法名称,即函数名 |
| parameters | Array\<Parameter\> | 否   | 被调用的原生平台的方法所需要的参数，即函数形参列表所对应的实参。要求实际传递的实参与函数的形参列表保持类型和个数匹配。<br>不设置该参数，则视为被调用的原生平台的方法的形参列表为空 |

**返回值：** 

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | 被调用的原生平台的方法的返回值 |

**错误码：**

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 1        | Bridge不可用                 |
| 4        | 方法名称错误               |
| 5        | 方法运行中，不能重复运行 |
| 6        | 方法未实现                 |

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.callMethod('nativeMethod').then((data)=>{
    console.log('data = ' + data);
}).catch((err : Error) => {
    console.error('error = ' + JSON.stringify(err));
});
```

### registerMethod

registerMethod(method: MethodData, callback: AsyncCallback\<void\>): void

registerMethod(method: MethodData): Promise\<void\>

注册ArkTS端方法，供Android或iOS平台端调用。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名   | 类型                  | 必填 | 说明                     |
| -------- | --------------------- | ---- | ------------------------ |
| method   | [MethodData](#methoddata)            | 是   | ArkTS侧方法数据信息           |
| callback | AsyncCallback\<void\> | 是   | callback方式的回调函数 |

**错误码：**

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 1        | Bridge不可用                     |
| 8        | 方法已经被注册，不允许重复注册 |

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

移除已注册的ArkTS端的方法。

**参数：** 

| 参数名   | 类型                  | 必填 | 说明                     |
| -------- | --------------------- | ---- | ------------------------ |
| method   | string                | 是   | 已注册的ArkTS侧方法签名           |
| callback | AsyncCallback\<void\> | 否   | callback方式的回调函数 |

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.unRegisterMethod('jsMethod');
```

### sendMessage

sendMessage(message: Message, callback: AsyncCallback\<Response\>): void

sendMessage(message: Message): Promise\<Response\>

向原生平台侧发送数据。<br>接口具体使用方式详见[平台桥接开发指南-Android](../../tutorial/how-to-use-bridge-on-android.md#场景一arkui侧向android侧传递数据)或[平台桥接开发指南-iOS](../../tutorial/how-to-use-bridge-on-ios.md#场景一arkui侧向ios侧传递数据)。<br>

**参数：** 

| 参数名   | 类型                  | 必填 | 说明                     |
| -------- | --------------------- | ---- | ------------------------ |
| message  | [Message](#message)   | 是   | 待发送数据                   |
| callback | AsyncCallback\<void\> | 否   | callback方式的回调函数 |

**返回值：** 

| 类型                  | 说明                     |
| --------------------- | ------------------------ |
| [Response](#response) | 原生平台侧应答数据 |

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.sendMessage('jsMessage').then((data)=>{
    console.log('data =' + data);
}).catch((err: Error) => {
    console.error('error =' + JSON.stringify(err));
});
```

### setMessageListener

setMessageListener(callback: (message: Message) => Response)

设置用于接收原生平台侧发送数据的回调。

**参数：** 

| 参数名   | 类型                         | 必填 | 说明                               |
| -------- | ---------------------------- | ---- | ---------------------------------- |
| callback | (message: Message) => Response | 是   | 回调函数，用于接收原生平台侧数据 |
| message  | [Message](#message)          | 是   | 原生平台侧发送的数据               |

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.setMessageListener((data) => {
    // 收到消息后，解析原生侧发送的数据
    console.log('receive data =' + data);

    // 收到消息后，向原生侧发送的应答数据
    return "ArkUI receive message success";
});
```

### callMethodWithCallback

callMethodWithCallback(methodName: string, method: (...parameters: Array\<Parameter\>) => ResultValue, ...parameters: Array\<Parameter\>): Promise\<ResultValue\>;

在ArkTS侧定义callback函数，并调用原生平台侧已定义的方法。在原生平台侧方法执行过程中，可以获取callback并执行。<br>
接口具体使用方式详见[平台桥接开发指南-Android](../../tutorial/how-to-use-bridge-on-android.md#场景六arkui侧注册callback且调用android侧的方法无参)或[平台桥接开发指南-iOS](../../tutorial/how-to-use-bridge-on-ios.md#场景六arkui侧注册callback且调用ios侧的方法无参)<br>
关于数据类型的说明详见[平台桥接数据类型](../../quick-start/platform-bridge-introduction.md#数据类型支持)。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型                              | 必填 | 说明                 |
| ---------- | --------------------------------- | ---- | -------------------- |
| methodName | string                            | 是   | 被调用的原生平台的方法名称,即函数名。     |
| method     | ...parameters: Array\<Parameter\> | 是   | ArkTS侧定义的callback函数。       |
| parameters | Array\<Parameter\>                | 否   | 被调用的原生平台的方法所需要的参数，即函数形参列表所对应的实参。要求实际传递的实参与函数的形参列表保持类型和个数匹配。<br>不设置该参数，则视为被调用的原生平台的方法的形参列表为空 |

**返回值：** 

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | 被调用的原生平台的方法的返回值 |

**示例：**

```javaScript
function testCallBackOfJs(stringParam : string) {
  console.log("Js received a parameter of " + stringParam)
  return "js testCallBackReturn call success."
}

this.bridgeCodec.callMethodWithCallback("testCallBack", testCallBackOfJs, "js sends parameter").then((res)=>{
    console.log('result: ' + res);
}).catch((err: Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

### callMethodSync<sup>22+</sup>

callMethodSync(methodName: string, ...parameters: Array<Parameter>): ResultValue;

使用同步方式调用原生平台侧已定义的方法，同步返回结果。
使用该接口需用try-catch捕获异常错误。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型               | 必填 | 说明                 |
| ---------- | ------------------ | ---- | -------------------- |
| methodName | string             | 是   | 被调用的原生平台的方法名称,即函数名     |
| parameters | Array\<Parameter\> | 否   | 被调用的原生平台的方法所需要的参数，即函数形参列表所对应的实参。要求实际传递的实参与函数的形参列表保持类型和个数匹配。<br>不设置该参数，则视为被调用的原生平台的方法的形参列表为空 |

**返回值：** 

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | 被调用的原生平台的方法的返回值 |

**示例：**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');
try {
    let result : bridge.ResultValue = bridgeObj.callMethodSync('nativeMethod')
    console.log('callMethodSync result is ' + JSON.stringify(result));
} catch (exception) {
    console.error(`callMethodSync error, Cause code: ${exception.code}, message: ${exception.message}`);
}
```



## **BridgeType**

| 参数名      | 说明                   |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON格式序列化编解码   |
| BINARY_TYPE | 二进制格式序列化编解码 |

## 数据类型

### **MethodData**

**说明：** ArkTS侧待注册方法的数据结构。

| 参数名 | 类型                              | 必填 | 说明           |
| ------ | --------------------------------- | ---- | -------------- |
| name   | string                            | 是   | ArkTS侧待注册方法的方法签名   |
| method | ...parameters: Array\<Parameter\> | 否   | ArkTS侧待注册方法的参数列表 |



### S 

type S = number | boolean | string | null | ArrayBuffer;

**说明：** 定义平台桥接使用的基础数据类型。



### T

type T  = S | Array\<number\> | Array\<boolean\> | Array\<string\>

**说明：** 定义平台桥接使用的基础数据类型的数组类型。



### Message

type Message = T | Record\<string, T\>

**说明：** 定义平台桥接使用的结构数据类型。

**示例**：

注：Record为ArkTS侧的自定义类型，原生平台侧对应的数据类型详见[平台桥接数据类型](../../quick-start/platform-bridge-introduction.md#数据类型支持)。


### Parameter

type Parameter = Message

**说明：** 定义方法参数类型。



### Response

type Response = Message

**说明：** 定义应答的数据类型。



### ResultValue

type ResultValue = T | Map\<string, T\>

**说明：** 定义方法返回值的类型。



## 完整示例

[完整示例（Android）](../../tutorial/how-to-use-bridge-on-android.md#场景示例)<br />
[完整示例（iOS）](../../tutorial/how-to-use-bridge-on-ios.md#场景示例)