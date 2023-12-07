# BridgePlugin (平台桥接)

本模块提供ArkUI端和Android平台端消息通信的功能，包括数据传输、方法调用和事件调用。需配套ArkUI端API使用，ArkUI侧具体用法请参考[Bridge API](../apis/js-apis-bridge.md)。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

```java
import ohos.ace.adapter.capability.bridge.BridgePlugin;
```
## BridgePlugin

**BridgePlugin(Context context, String bridgeName, int instanceId);** 

@since 10

@deprecated since 11

创建BridgePlugin类。

**参数：** 

| 参数名     | 类型    | 必填 | 说明               |
| ---------- | ------- | ---- | ------------------ |
| context    | Context | 是   | 应用程序的上下文。 |
| bridgeName | String  | 是   | 定义桥接名称。     |
| instanceId | int     | 是   | 实例ID。           |

**返回值：** 

| 类型         | 说明           |
| ------------ | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getInstanceId());
  ```

**BridgePlugin(Context context, String bridgeName, int instanceId, BridgeType bridgeType);** 

@since 10

@deprecated since 11

创建BridgePlugin类(编解码模式)。

BridgeType:

| 参数名      | 说明                   |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON格式序列化编解码   |
| BINARY_TYPE | 二进制格式序列化编解码 |

**参数：** 

| 参数名     | 类型       | 必填 | 说明                                 |
| ---------- | ---------- | ---- | ------------------------------------ |
| context    | Context    | 是   | 应用程序的上下文。                   |
| bridgeName | string     | 是   | 定义桥接名称。                       |
| instanceId | int        | 是   | 实例ID。                             |
| bridgeType | BridgeType | 否   | 编解码类型（可不填，默认为json格式） |

**返回值：** 

| 类型         | 说明           |
| ------------ | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getInstanceId(), BridgePlugin.BridgeType.BINARY_TYPE);
  ```

**BridgePlugin(Context context, String bridgeName, BridgeManager bridgeManager);**

@since 11

创建BridgePlugin类。

**参数：** 

| 参数名     | 类型    | 必填 | 说明               |
| ---------- | ------- | ---- | ------------------ |
| context    | Context | 是   | 应用程序的上下文。 |
| bridgeName | string  | 是   | 定义桥接名称。     |
| bridgeManager | BridgeManager | 是   | BridgeManager实例。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getBridgeManager());
  ```

**BridgePlugin(Context context, String bridgeName, BridgeManager bridgeManager, BridgeType bridgeType);**

@since 11

创建BridgePlugin类(编解码模式)。

BridgeType:

| 参数名      | 说明                   |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON格式序列化编解码   |
| BINARY_TYPE | 二进制格式序列化编解码 |

**参数：** 

| 参数名        | 类型          | 必填 | 说明                                 |
| ------------- | ------------- | ---- | ------------------------------------ |
| context       | Context       | 是   | 应用程序的上下文。                   |
| bridgeName    | string        | 是   | 定义桥接名称。                       |
| bridgeManager | BridgeManager | 是   | BridgeManager实例。                  |
| bridgeType    | BridgeType    | 否   | 编解码类型（可不填，默认为json格式） |

**返回值：** 

| 类型         | 说明           |
| ------------ | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```java
public class BridgeImpl extends BridgePlugin {
    ...
}

BridgeImpl bridgeImpl = new BridgeImpl(this, "Bridge", getBridgeManager(), BridgePlugin.BridgeType.BINARY_TYPE);
  ```

## callMethod

public void callMethod(MethodData methodData);

调用ArkUI端的方法。

**参数：** 

| 参数名     | 类型       | 必填 | 说明         |
| ---------- | ---------- | ---- | ------------ |
| methodData | MethodData | 是   | 方法数据结构。 |

**MethodData结构**

| 名称       | 类型     | 说明     |
| ---------- | -------- | -------- |
| methodName | String   | 方法名。   |
| Parameters | Object[] | 方法参数。 |

**返回值：** 

无

**示例：**

```java
Object[] paramObject = { "param1", "param2" };
MethodData methodData = new MethodData("jsMethod", paramObject);
bridgeImpl.callMethod(methodData);
```

## sendMessage

public void sendMessage(Object data);

向ArkUI端发送数据。

**参数：** 

| 参数名 | 类型   | 必填 | 说明   |
| ------ | ------ | ---- | ------ |
| data   | Object | 是   | 数据。 |

**返回值：** 无

**示例：**

```java
String[] data = { "message1", "message2" };
bridgeImpl.sendMessage(data);
```

## setMessageListener

public void setMessageListener(IMessageListener messageListener);

注册消息监听。

**参数：** 

| 参数名          | 类型             | 必填 | 说明           |
| --------------- | ---------------- | ---- | -------------- |
| messageListener | IMessageListener | 是   | 信息监听接口类。 |

### **IMessageListener**

| IMessageListener | 参数     | 参数描述 | 返回值 | 说明            |
| ------------------  |------------- | -------- | ---------- | -------------------- |
| onMessage          | data: Object | 数据信息。 | Object     | 等待ArkUI端发送信息。     |
| onMessageResponse  | data: Object | 数据信息。 | 无         | 等待ArkUI端发送信息应答。 |

**示例：**

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

注册方法返回监听

**参数：** 

| 参数名               | 类型          | 必填 | 说明               |
| -------------------- | ------------- | ---- | ------------------ |
| methodResultListener | IMethodResult | 是   | 方法返回监听接口类。 |

### IMethodResult

| IMethodResult  | 参数                                                     | 参数描述                         | 返回值 | 说明         |
| -------------- | ------------------------------------------------------------ | -------------------------------- | ------ | ---------------- |
| onSuccess      | resultValue：Object                                          | 返回值信息。                       | 无     | 调用方法返回成功。 |
| onError        | methodName : String<br/>errorCode : int<br/>errorMessage : string | 方法名。<br/>错误类型。<br/>错误信息。 | 无     | 调用方法返回失败。 |
| onMethodCancel | methodName : string                                          | 方法名。                           | 无     | 监听取消方法注册。 |

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


## 完整示例

[完整示例](../../tutorial/how-to-use-bridge-on-android.md#场景示例)