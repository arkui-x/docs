# @arkui-x.bridge.d.ts (平台桥接)

本模块提供ArkUI端和Android或iOS平台端消息通信的功能，包括数据传输、方法调用和事件调用。需配套平台端API使用，Android侧请参考[BridgePlugin](../arkui-for-android/BridgePlugin.md)，iOS侧参考[BridgePlugin](../arkui-for-ios/BridgePlugin.md)。

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

| 类型                          | 说明           |
| ----------------------------- | -------------- |
| [BridgeObject](#bridgeobject) | 桥接的接口类。 |

**示例：** 

  ```javaScript
  const bridgeObj: bridge.BridgeObject = bridge.createBridge('Bridge');
  ```

 createBridge(bridgeName: string, type: BridgeType): BridgeObject;

定义BridgeObject类(编解码模式)。

**系统能力：**   SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型       | 必填 | 说明                                 |
| ---------- | ---------- | ---- | ------------------------------------ |
| bridgeName | string     | 是   | 定义桥接名称。                       |
| bridgeType | BridgeType | 否   | 编解码类型（可不填，默认为json格式） |

**返回值：** 

| 类型                          | 说明           |
| ----------------------------- | -------------- |
| [BridgeObject](#bridgeobject) | 桥接的接口类。 |

**示例：** 

  ```javaScript
  const bridgeObj: bridge.BridgeObject = bridge.createBridge('Bridge', bridge.BridgeType.BINARY_TYPE);
  ```

## BridgeObject

桥接的接口类。

### callMethod

callMethod(methodName: string, ...parameters: Array\<Parameter\>): Promise\<ResultValue\>;

调用平台方法。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型               | 必填 | 说明                 |
| ---------- | ------------------ | ---- | -------------------- |
| methodName | string             | 是   | 平台侧方法名称。     |
| parameters | Array\<Parameter\> | 否   | 平台侧方法参数列表。 |

注：iOS侧设置Parameter类型为number时，整数取值范围为-2<sup>31</sup>~2<sup>31</sup>-1；
如果不在该范围的整数值，可以转换为string类型或将bridgeType设置成BINARY_TYPE的方式进行传递。

**返回值：** 

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | 平台方法执行结果。 |

**错误码：**

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 1        | 管道不可用。                 |
| 4        | 方法名称错误。               |
| 5        | 方法正确运行，不能重复运行。 |
| 6        | 方法未实现。                 |

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

注册ArkUI端方法，供Android或iOS平台端调用。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名   | 类型                  | 必填 | 说明                     |
| -------- | --------------------- | ---- | ------------------------ |
| method   | MethodData            | 是   | JS侧方法数据。           |
| callback | AsyncCallback\<void\> | 否   | callback方式的回调函数。 |

**错误码：**

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
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

| 参数名   | 类型                  | 必填 | 说明                     |
| -------- | --------------------- | ---- | ------------------------ |
| method   | string                | 是   | JS侧方法名称。           |
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

| 参数名   | 类型                  | 必填 | 说明                     |
| -------- | --------------------- | ---- | ------------------------ |
| message  | [Message](#message)   | 是   | 数据。                   |
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
}).catch((err: Error) => {
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

    // 收到消息后，向原生侧发送回执
    return "ArkUI receive message success";
});
```

### callMethodWithCallback

callMethodWithCallback(methodName: string, method: (...parameters: Array\<Parameter\>) => ResultValue, ...parameters: Array\<Parameter\>): Promise\<ResultValue\>;

注册callback, 供平台侧调用，调用平台侧函数。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型                              | 必填 | 说明                 |
| ---------- | --------------------------------- | ---- | -------------------- |
| methodName | string                            | 是   | 平台侧方法名称。     |
| method     | ...parameters: Array\<Parameter\> | 是   | JS侧参数列表。       |
| parameters | Array\<Parameter\>                | 否   | 平台侧方法参数列表。 |

**返回值：** 

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | 平台方法执行结果。 |

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

调用平台方法，同步返回结果。

**系统能力：**  SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名     | 类型               | 必填 | 说明                 |
| ---------- | ------------------ | ---- | -------------------- |
| methodName | string             | 是   | 平台侧方法名称。     |
| parameters | Array\<Parameter\> | 否   | 平台侧方法参数列表。 |

**返回值：** 

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | 平台方法执行结果。 |

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

**说明：** js侧注册方法的数据类型。

| 参数名 | 类型                              | 必填 | 说明           |
| ------ | --------------------------------- | ---- | -------------- |
| name   | string                            | 是   | JS侧方法名。   |
| method | ...parameters: Array\<Parameter\> | 否   | JS侧参数列表。 |



### S 

type S = number | boolean | string | null | ArrayBuffer;

**说明：** 定义桥接使用的基础数据类型。



### T

type T  = S | Array\<number\> | Array\<boolean\> | Array\<string\>

**说明：** 定义桥接使用的基础数据类型的数组类型。



### Message

type Message = T | Record\<string, T\>

**说明：** 定义桥接使用结构数据类型。

**示例**：

注：Record为ArkUI侧的自定义类型，Android侧需要用object类型对应。

```javascript
// ArkUI侧

let params : Record<string, number> = {"abc" : 123};
this.bridgeImpl.callMethod("recordFun", params);
```

```java
// android侧

public String recordFun(HashMap<String, Integer> params) {
    Map<String, Integer> map = (Map)params;
    Log.i("BridgeTest recordFun params is ", map.toString());
    return map.toString();
}
```



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