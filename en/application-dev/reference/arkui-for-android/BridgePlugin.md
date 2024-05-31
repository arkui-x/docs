# BridgePlugin (Platform Bridge)

The **BridgePlugin** module provides APIs for communication between ArkUI and the Android platform, including data transfer, method invoking, and event invoking. The APIs must be used together with the ArkUI APIs. For details about usage on ArkUI, see [Bridge API](../apis/js-apis-bridge.md).

> **NOTE**
>
> The initial APIs of this module are supported since API version 10. Newly added APIs will be marked with a superscript to indicate their earliest API version.

```java
import ohos.ace.adapter.capability.bridge.BridgePlugin;
```
## BridgePlugin

**BridgePlugin(Context context, String bridgeName, int instanceId);** 

@since 10

@deprecated since 11

Creates a **BridgePlugin** instance.

**Parameters**

| Name    | Type   | Mandatory| Description              |
| ---------- | ------- | ---- | ------------------ |
| context    | Context | Yes  | Application context.|
| bridgeName | String  | Yes  | Bridge name.    |
| instanceId | int     | Yes  | Instance ID.          |

**Return value**

| Type        | Description          |
| ------------ | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getInstanceId());
  ```

**BridgePlugin(Context context, String bridgeName, int instanceId, BridgeType bridgeType);** 

@since 10

@deprecated since 11

Creates a **BridgePlugin** instance in codec mode.

BridgeType:

| Name     | Description                  |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON type.  |
| BINARY_TYPE | Binary type.|

**Parameters**

| Name    | Type      | Mandatory| Description                                |
| ---------- | ---------- | ---- | ------------------------------------ |
| context    | Context    | Yes  | Application context.                  |
| bridgeName | string     | Yes  | Bridge name.                      |
| instanceId | int        | Yes  | Instance ID.                           |
| bridgeType | BridgeType | No  | Codec type. This parameter is optional. The default value is JSON.|

**Return value**

| Type        | Description          |
| ------------ | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getInstanceId(), BridgePlugin.BridgeType.BINARY_TYPE);
  ```

**BridgePlugin(Context context, String bridgeName, BridgeManager bridgeManager);**

@since 11

Creates a **BridgePlugin** instance.

**Parameters**

| Name    | Type   | Mandatory| Description              |
| ---------- | ------- | ---- | ------------------ |
| context    | Context | Yes  | Application context.|
| bridgeName | string  | Yes  | Bridge name.    |
| bridgeManager | BridgeManager | Yes  | **BridgeManager** instance.|

**Return value**

| Type                             | Description          |
| --------------------------------- | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getBridgeManager());
  ```

**BridgePlugin(Context context, String bridgeName, BridgeManager bridgeManager, BridgeType bridgeType);**

@since 11

Creates a **BridgePlugin** instance in codec mode.

**BridgeType**

| Name     | Description                  |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON type.  |
| BINARY_TYPE | Binary type.|

**Parameters**

| Name       | Type         | Mandatory| Description                                |
| ------------- | ------------- | ---- | ------------------------------------ |
| context       | Context       | Yes  | Application context.                  |
| bridgeName    | string        | Yes  | Bridge name.                      |
| bridgeManager | BridgeManager | Yes  | **BridgeManager** instance.                 |
| bridgeType    | BridgeType    | No  | Codec type. This parameter is optional. The default value is JSON.|

**Return value**

| Type        | Description          |
| ------------ | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getBridgeManager(), BridgePlugin.BridgeType.BINARY_TYPE);
  ```

## callMethod

public void callMethod(MethodData methodData);

Calls the specified ArkUI method.

**Parameters**

| Name    | Type      | Mandatory| Description        |
| ---------- | ---------- | ---- | ------------ |
| methodData | MethodData | Yes  | Method data structure.|

**MethodData**

| Name      | Type    | Description    |
| ---------- | -------- | -------- |
| methodName | String   | Method name.  |
| Parameters | Object[] | Method parameters.|

**Return value**

None

**Example**

```java
Object[] paramObject = { "param1", "param2" };
MethodData methodData = new MethodData("jsMethod", paramObject);
bridgeImpl.callMethod(methodData);
```

## sendMessage

public void sendMessage(Object data);

Sends data to the ArkUI side.

**Parameters**

| Name| Type  | Mandatory| Description  |
| ------ | ------ | ---- | ------ |
| data   | Object | Yes  | Data to send.|

**Return value**

None

**Example**

```java
String[] data = { "message1", "message2" };
bridgeImpl.sendMessage(data);
```

## setMessageListener

public void setMessageListener(IMessageListener messageListener);

Sets a message listener.

**Parameters**

| Name         | Type            | Mandatory| Description          |
| --------------- | ---------------- | ---- | -------------- |
| messageListener | IMessageListener | Yes  | Message listener.|

### **IMessageListener**

| IMessageListener | Parameter    | Description| Return Value| Description           |
| ------------------  |------------- | -------- | ---------- | -------------------- |
| onMessage          | data: Object | Data information.| Object     | Invoked when the ArkUI side sends a message.    |
| onMessageResponse  | data: Object | Data information.| None        | Invoked when the ArkUI side sends a response.|

**Example**

```java
public BridgeImpl(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

public BridgeImpl(Context context, String name, BridgeManager bridgeManager) {
    super(context, name, bridgeManager);
    this.name = name;
    setMessageListener(this);
}

@Override
public Object onMessage(Object data) {
    ALog.i("onMessage data: ", data.toString());
    return jsonObject;
}

@Override
public void onMessageResponse(Object data) {
    ALog.i("onMessageResponse data: ", data.toString());
}
```

## setMethodResultListener

public void setMethodResultListener(IMethodResult methodResultListener);

Sets a method result listener.

**Parameters**

| Name              | Type         | Mandatory| Description              |
| -------------------- | ------------- | ---- | ------------------ |
| methodResultListener | IMethodResult | Yes  | Method result listener.|

### IMethodResult

| IMethodResult  | Parameter                                                    | Description                        | Return Value| Description        |
| -------------- | ------------------------------------------------------------ | -------------------------------- | ------ | ---------------- |
| onSuccess      | resultValue: Object                                         | Return value information.                      | None    | Invoked when the method is called successfully.|
| onError        | methodName : String<br>errorCode : int<br>errorMessage : string | Method name.<br>Error type.<br>Error message.| None    | Invoked when the method fails to be called.|
| onMethodCancel | methodName : string                                          | Method name.                          | None    | Invoked when the method call is canceled.|

```java
public BridgeImpl(Context context, String name, int id) {
    super(context, name, id);
    this.name = name;
    setMethodResultListener(this);
}

public BridgeImpl(Context context, String name, BridgeManager bridgeManager) {
    super(context, name, bridgeManager);
    this.name = name;
    setMethodResultListener(this);
}

@Override
public void onSuccess(Object res) {
    ALog.i("onJsSendMethodResult result: ", res.toString());
}

@Override
public void onError(String name, int code, String message) {
    ALog.i("onError: ", message);
}

@Override
public void onMethodCancel(String name) {
    ALog.i("onCancel: ", name);
}
```


## Sample Code

[Sample Code](../../tutorial/how-to-use-bridge-on-android.md#example-scenario)
