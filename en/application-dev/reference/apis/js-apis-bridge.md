# @arkui-x.bridge.d.ts (Platform Bridge)

The **@arkui-x.bridge** module provides APIs for communication between ArkUI and the Android or iOS platform, including data transfer, method invoking, and event invoking. The APIs must be used together with the platform APIs. For details about how to use them on Android devices, see [BridgePlugin](../arkui-for-android/BridgePlugin.md). For details about how to use them on iOS devices, see [BridgePlugin](../arkui-for-ios/BridgePlugin.md).

> **NOTE**
>
> The initial APIs of this module are supported since API version 10. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import

```javaScript
import bridge from '@arkui-x.bridge';
```
## createBridge

createBridge(bridgeName: string): BridgeObject

Creates a **BridgeObject** instance.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type  | Mandatory| Description          |
| ---------- | ------ | ---- | -------------- |
| bridgeName | string | Yes  | Bridge name.|

**Return value**

| Type                             | Description          |
| --------------------------------- | -------------- |
| [BridgeObject](#bridgeobject) | **BridgeObject** instance.|

**Example**

  ```javaScript
  const bridgeObj: BridgeObject = bridge.createBridge('Bridge');
  ```

 createBridge(bridgeName: string, type: BridgeType): BridgeObject;

Creates a **BridgeObject** instance (codec mode).

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type      | Mandatory| Description                                |
| ---------- | ---------- | ---- | ------------------------------------ |
| bridgeName | string     | Yes  | Bridge name.                      |
| bridgeType | BridgeType | No  | Codec type. This parameter is optional. The default value is JSON.|

**Return value**

| Type                         | Description          |
| ----------------------------- | -------------- |
| [BridgeObject](#bridgeobject) | **BridgeObject** instance.|

**Example**

  ```javaScript
  const bridgeObj: BridgeObject = bridge.createBridge('Bridge', BridgeType.BINARY_TYPE);
  ```

## BridgeObject

Implements the **BridgeObject** class.

### callMethod

callMethod(methodName: string, parameters?: Record\<string, Parameter\>): Promise\<ResultValue\>;

callMethod(methodName: string, ...parameters: Array\<any\>): Promise\<ResultValue\>;

Calls a platform method.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                     | Mandatory| Description          |
| ---------- | ------------------------- | ---- | -------------- |
| methodName | string                    | Yes  | Method name.    |
| parameters | Record\<string, Parameter\> | No  | Parameters of the method.|
| parameters | Array\<any\>                | No  | Parameters of the method.|

**Return value**

| Type                       | Description              |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | Execution result of the method.|

**Error codes**

| ID  | Error Message|
| --------- | ------- |
| 1 | Pipe unavailable.|
| 4 | Incorrect method name.|
| 5 | The method runs correctly and cannot run repeatedly.|
| 6 | Method unimplemented.|

**Example**

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

Registers an ArkUI method for the Android or iOS platform to call.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name  | Type               | Mandatory| Description                      |
| -------- | ------------------- | ---- | -------------------------- |
| method   | MethodData          | Yes  | Method data.                |
| callback | AsyncCallback\<void\> | No  | Callback used to return the result.|

**Error codes**

| ID| Error Message                      |
| -------- | ------------------------------ |
| 1        | Pipe unavailable.                    |
| 8        | The method has already been registered.|

**Example**

```javaScript
function jsMethod() {
  return 'ts return: jsMethod';
}

const bridgeObj = bridge.createBridge('Bridge');
bridgeObj.registerMethod({ name: 'jsMethod', method: jsMethod });
```

### unRegisterMethod

unRegisterMethod(methodName: string, callback: AsyncCallback\<void\>): void

unRegisterMethod(methodName: string): Promise\<void\>

Unregisteres an ArkUI method.

**Parameters**

| Name  | Type               | Mandatory| Description                      |
| -------- | ------------------- | ---- | -------------------------- |
| method   | string              | Yes  | Method name.                |
| callback | AsyncCallback\<void\> | No  | Callback used to return the result.|

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Example**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.unRegisterMethod('jsMethod');
```

### sendMessage

sendMessage(message: Message, callback: AsyncCallback\<Response\>): void

sendMessage(message: Message): Promise\<Response\>

Sends data to the platform.

**Parameters**

| Name  | Type               | Mandatory| Description                      |
| -------- | ------------------- | ---- | -------------------------- |
| message  | [Message](#message) | Yes  | Data to send.                    |
| callback | AsyncCallback\<void\> | No  | Callback used to return the result.|

**Return value**

| Type                 | Description                    |
| --------------------- | ------------------------ |
| [Response](#response) | Response data from the platform.|

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Example**

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

Sets a listener for receiving data from the platform.

**Parameters**

| Name  | Type                        | Mandatory| Description                              |
| -------- | ---------------------------- | ---- | ---------------------------------- |
| callback | (message: Message)=>Response | Yes  | Callback for receiving data from the platform.|
| message  | [Message](#message)          | Yes  | Data from the platform.              |

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Example**

```javaScript
const bridgeObj = bridge.createBridge('Bridge');

bridgeObj.setMessageListener((data) => {
    console.log('receive data =' + data);
    
    // Send a response to the platform
    return "ArkUI receive message success";
});
```

### callMethodWithCallback

callMethodWithCallback(methodName: string, method: (parameters?: Record<string , Parameter>) => ResultValue, parameters?: Record<string, Parameter>): Promise<ResultValue>;

callMethodWithCallback(methodName: string, method: (parameters?: Record<string , Parameter>) => ResultValue, parameters?: Array<any>): Promise<ResultValue>;

Registers a callback for the platform to call functions on the platform.

**System capability**: SystemCapability.ArkUI.ArkUI.Full

**Parameters**

| Name    | Type                       | Mandatory| Description          |
| ---------- | --------------------------- | ---- | -------------- |
| methodName | string                      | Yes  | Method name.    |
| method     | function                    | Yes  | Method on the JS Side      |
| parameters | Record\<string, Parameter\> | No  | Parameters of the method.|
| parameters | Array\<any\>                | No  | Parameters of the method.|

**Return value**

| Type                       | Description              |
| --------------------------- | ------------------ |
| [ResultValue](#resultvalue) | Execution result of the method.|

**Example**

```javaScript
function jsMethod() {
  return 'ts return: jsMethod';
}

const bridgeObj = bridge.createBridge('Bridge');
bridgeObj.callMethodWithCallBack('jsMethod', jsMethod, PlatformParams).then((data)=>{
    console.log('data = ' + data);
}).catch((err) => {
    console.error('error = ' + JSON.stringify(err));
});
```

## **BridgeType**

| Name     | Description                  |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON type.  |
| BINARY_TYPE | Binary type.|

## S 

type S = number | boolean | string | null | ArrayBuffer;

Defines the basic data types used for the bridge.

## T

type T  = S | Array\<number\> | Array\<boolean\> | Array\<string\>

Defines the array types used for the bridge.

## Message

type Message = T | Record\<string, T\>

Defines the structured data type used for the bridge.

## Parameter

type Parameter = Message

Defines the method parameter type.

## Response

type Response = Message

Defines the response data type.

## ResultValue

type ResultValue = T | Map\<string, T\>

Defines the type of the method return value.


## Sample Code

[Sample Code (Android)] (../../tutorial/how-to-use-bridge-on-android.md#example-scenario)<br>
[Sample Code (iOS)](../../tutorial/how-to-use-bridge-on-ios.md#example-scenario)
