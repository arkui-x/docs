# BridgePlugin (Platform Bridge)

The **BridgePlugin** module provides APIs for communication between ArkUI and the Android platform, including data transfer, method invoking, and event invoking. The APIs must be used together with the ArkUI APIs. For details about usage on ArkUI, see [Bridge API](../apis/js-apis-bridge.md).

**Since**

10

> **NOTE**
>
> The initial APIs of this module are supported since API version 10. Newly added APIs will be marked with a superscript to indicate their earliest API version.



## **IMessageListener**

Defines an **IMessageListener** object for listening for messages sent by ArkUI. You need to implement this class and set the object in a **BridgePlugin** instance using the **setMessageListener** method.

### onMessage

```java
Object onMessage(Object data);
```

**Description**

Listens for messages sent from the ArkUI side through the **sendMessage** method.

**Parameters**

| Name | Type  | Mandatory| Description               |
| ---- | ------ | ---- | ------------------- |
| data | Object | Yes  | Message sent from the ArkUI side.|

**Return value**

| Type  | Description                                                        |
| ------ | ------------------------------------------------------------ |
| Object | Response message from the platform to the ArkUI side.|



### onMessageResponse

```java
void onMessageResponse(Object data);
```

**Description**

Listens for the response message from the ArkUI side after the platform calls the **sendMessage** API to send a message. In other words, this API is used to receive the return value of the **setMessageListener** method registered on the ArkUI side.

**Parameters**

| Name | Type  | Mandatory| Description                                                |
| ---- | ------ | ---- | ---------------------------------------------------- |
| data | Object | Yes  | Response message from the ArkUI side after the platform calls the **sendMessage** API.|

**Return value**

None



## IMethodResult

Defines an **IMethodResult** object to listen for the execution of an ArkUI method called on the platform. You need to implement this class and set the object in a **BridgePlugin** instance using the **setMethodResultListener** method.

### onSuccess

```java
void onSuccess(Object resultValue);
```

**Description**

Invoked when the specified ArkUI method is called successfully on the platform. This API returns the return value of the ArkUI method to the platform.

**Parameters**

| Name        | Type  | Mandatory| Description                 |
| ----------- | ------ | ---- | --------------------- |
| resultValue | Object | Yes  | Return value of the ArkUI method.|

**Return value**

None



### onError

```java
void onError(String methodName, int errorCode, String errorMessage);
```

**Description**

Invoked when an error occurs in calling the specified ArkUI method on the platform. This API returns the error information to the platform.

**Parameters**

| Name                      | Type  | Mandatory| Description             |
| ------------------------- | ------ | ---- | ----------------- |
| methodName                | String | Yes  | Name of an ArkUI method.|
| [errorCode](#errorcode) | int    | Yes  | Error code.         |
| errorMessage              | String | Yes  | Error message.   |

**Return value**

None



### onMethodCancel

```java
void onMethodCancel(String methodName);
```

**Description**

Invoked when **unRegisterMethod** is called on ArkUI. It is used to notify the platform that an event is deregistered.

**Parameters**

| Name       | Type  | Mandatory| Description                     |
| ---------- | ------ | ---- | ------------------------- |
| methodName | String | Yes  | Name of the deregistered event on ArkUI.|

**Return value**

None



## BridgePlugin

Implements inter-platform communication. You need to extend this class and create instances for ArkUI to communicate with the native platform.

### Types

#### **BridgeType<sup>11+</sup>**

**Description**

Enumerates the data encoding and decoding formats.

| Name     | Description                  | Reference Mode                       |
| ----------- | ---------------------- | ----------------------------------- |
| JSON_TYPE   | JSON type.  | BridgePlugin.BridgeType.JSON_TYPE   |
| BINARY_TYPE | Binary type.| BridgePlugin.BridgeType.BINARY_TYPE |



### Constructor

#### BridgePlugin<sup>(deprecated)</sup>

```java
public BridgePlugin(Context context, String bridgeName, int instanceId);
```

**Description**

Constructs a **BridgePlugin** instance based on the specified instance ID, with the data encoding and decoding format as **JSON_TYPE** (default).

**Parameters**

| Name       | Type   | Mandatory| Description                                                   |
| ---------- | ------- | ---- | ------------------------------------------------------- |
| context    | Context | Yes  | Context of the current (Android native) activity.                  |
| bridgeName | String  | Yes  | Bridge name, which must be the same as that on ArkUI.                     |
| instanceId | int     | Yes  | Instance ID, which can be obtained using the **getInstanceId()** method of **StageActivity**.|

**Deprecated since**

API version 11. You are advised to use **BridgePlugin<sup>11+</sup>** instead.

**Example**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
    // Construct a BridgePlugin instance based on the specified instance ID, with the data encoding and decoding format as JSON_TYPE (default).
    public BridgeImpl(Context context, String name, int instanceId) {
        super(context, name, instanceId);
    }
}
```

```java
// EntryEntryAbilityActivity.java

public class EntryEntryAbilityActivity extends StageActivity {
    private BridgeImpl bridgeImplByID = null;

    protected void onCreate(Bundle savedInstanceState) {
        // Construct a BridgeImpl instance based on the specified instance ID
        bridgeImplByID = new BridgeImpl(this, "BridgeImplByID", getInstanceId());
        super.onCreate(savedInstanceState);
    }
}
```



#### BridgePlugin<sup>11+</sup>

```java
public BridgePlugin(Context context, String bridgeName, BridgeManager bridgeManager);

public BridgePlugin(Context context, String bridgeName, BridgeManager bridgeManager, BridgeType bridgeType);

public BridgePlugin(Context context, String bridgeName, BridgeManager bridgeManager, BridgeType bridgeType, TaskOption taskOption);
```

**Description**

Constructs a **BridgePlugin** instance based on a **BridgeManager** instance. You can specify the data encoding and decoding format (**JSON_TYPE** by default).

**Parameters**

| Name          | Type                             | Mandatory| Description                                                        |
| ------------- | --------------------------------- | ---- | ------------------------------------------------------------ |
| context       | Context                           | Yes  | Context of the current (Android native) activity.                       |
| bridgeName    | String                            | Yes  | Bridge name, which must be the same as that on ArkUI.                          |
| bridgeManager | [BridgeManager](#bridgemanager) | Yes  | **BridgeManager** object, which can be obtained using the **getBridgeManager()** method of **StageActivity**.|
| bridgeType    | BridgeType                        | No  | Data encoding and decoding format. The default value is **JSON_TYPE**.                           |
| taskOption    | TaskOption                        | Yes  | **BridgePlugin** thread concurrency mode. By default, serial mode is used.              |

**Example**

  ```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
	// Construct a BridgePlugin instance based on a BridgeManager instance. You can specify the data encoding and decoding format (JSON_TYPE by default).
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager) {
        super(context, name, bridgeManager);
    }

    // Construct a BridgePlugin instance based on a BridgeManager instance, specifying the data encoding and decoding format.
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager, BridgeType bridgeType) {
        super(context, name, bridgeManager);
    }
    
    // Construct a BridgePlugin instance based on a BridgeManager instance, specifying the thread concurrency mode.
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager, BridgeType codecType, TaskOption 			taskOption) {
        super(context, name, bridgeManager, codecType, taskOption);
    }
}
  ```

```java
// EntryEntryAbilityActivity.java

public class EntryEntryAbilityActivity extends StageActivity {
    private BridgeImpl bridgeImplByManager = null;
    private BridgeImpl bridgeImplByManagerAndType = null;

    protected void onCreate(Bundle savedInstanceState) {
        // Construct a BridgeImpl instance based on a bridgeManager instance.
        bridgeImplByManager = new BridgeImpl(this, "BridgeImplByManager", getBridgeManager());
        
        // Construct a BridgeImpl instance based on a bridgeManager instance, with data encoding and decoding in binary format.
        bridgeImplByManagerAndType = new BridgeImpl(this, "BridgeImplByManagerAndType", getBridgeManager(), 					BridgePlugin.BridgeType.BINARY_TYPE);
        
        // Construct a BridgeImpl instance based on a bridgeManager instance, with continuous concurrency as the thread concurrency mode.
        bridgeImplByManagerAndTask = new BridgeImpl(this, "BridgeImplByManagerAndTask", getBridgeManager(), 					BridgePlugin.BridgeType.BINARY_TYPE, new TaskOption());
        
        // Set bundleName to the actual bundle name.
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```



### Functions

#### sendMessage

```java
public void sendMessage(Object data);
```

**Description**

Sends a message to the ArkUI side. The response from the ArkUI side is obtained through the **IMessageListener.onMessageResponse** API.

**Parameters**

| Name | Type  | Mandatory| Description  |
| ---- | ------ | ---- | ------ |
| data | Object | Yes  | Data to send.|

**Return value**

None

**Example**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
	// Construct a BridgePlugin instance based on a BridgeManager instance. You can specify the data encoding and decoding format (JSON_TYPE by default).
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager) {
        super(context, name, bridgeManager);
    }
}
```

```java
// EntryEntryAbilityActivity.java

public class EntryEntryAbilityActivity extends StageActivity {
    public BridgeImpl bridgeImpl = null;
    
    protected void onCreate(Bundle savedInstanceState) {
        // Create a platform bridge object.
        bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
        // Set bundleName to the actual bundle name.
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        // Register the TestSendMessage button.
        testSendMessage();
    }

    public void testSendMessage() {
        // Touch the button to send a message.
        Button button = (Button) findViewById(R.id.TestSendMessage);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String androidData = "Android side data";
        		// Send data to the ArkUI side.
                bridgeImpl.sendMessage(androidData);
            }
        });
    }
}
```



#### callMethod

```java
public void callMethod(MethodData methodData);
```

**Description**

 Calls an ArkUI method. The return result of the method is obtained through the **IMethodResult.onSucessess** API.

**Parameters**

| Name       | Type                       | Mandatory| Description                                   |
| ---------- | --------------------------- | ---- | --------------------------------------- |
| methodData | [MethodData](#methoddata) | Yes  | Information about the ArkUI method, including the method name and parameter list.|

**Return value**

None

**Example**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
	// Construct a BridgePlugin instance based on a BridgeManager instance. You can specify the data encoding and decoding format (JSON_TYPE by default).
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager) {
        super(context, name, bridgeManager);
    }
}
```

```java
// EntryEntryAbilityActivity.java

public class EntryEntryAbilityActivity extends StageActivity {
    public BridgeImpl bridgeImpl = null;
    
    protected void onCreate(Bundle savedInstanceState) {
        // Create a platform bridge object.
        bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
        // Set bundleName to the actual bundle name.
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        // Register the TestCallMethod button.
        testCallMethod();
    }

	public void testCallMethod() {
        // Touch the button to send a message.
        Button button = (Button) findViewById(R.id.TestCallMethod);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Define an object array to store the actual parameters corresponding to the arguments of the JS method.
                Object[] paramObject = { "param1", "param2" };
                // Construct a JS MethodData instance, passing in the method name in jsMethodName.
                MethodData methodData = new MethodData("jsMethodName", paramObject);
                // Invoke the JS method (indicated by jsMethodName).
                bridgeImpl.callMethod(methodData);
            }
        });
    }
}
```



#### setMessageListener

```java
public void setMessageListener(IMessageListener messageListener);
```

**Description**

Sets a message listener to capture messages from the ArkUI side.

**Parameters**

| Name            | Type            | Mandatory| Description              |
| --------------- | ---------------- | ---- | ------------------ |
| messageListener | IMessageListener | Yes  | Instance of the message listener.|

**Return value**

None

**Example**

```java
// MessageListener.java

public class MessageListener implements IMessageListener {
    @Override
    public Object onMessage(Object object) {
        Log.i("MessageListener", "Android bridge onMessage success");
        return "Android success onMessage";
    }
    
    @Override
    public void onMessageResponse(Object object) {
        Log.i("MessageListener", "Android bridge onMessageResponse success");
    }
}
```

```java
// EntryEntryAbilityActivity.java

public class EntryEntryAbilityActivity extends StageActivity {
    protected void onCreate(Bundle savedInstanceState) {
      	// Create a platform bridge object.
        BridgeImpl bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
      	// Register a message listener.
        bridgeImpl.setMessageListener(new MessageListener());
        // Set bundleName to the actual bundle name.
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```



#### setMethodResultListener

```java
public void setMethodResultListener(IMethodResult methodResultListener);
```

**Description** 

Sets a method result listener to capture the execution result of an ArkUI method on the platform and the deregistered event on the ArkUI side.

**Parameters**

| Name                 | Type         | Mandatory| Description                |
| -------------------- | ------------- | ---- | -------------------- |
| methodResultListener | IMethodResult | Yes  | Method result listener.|

**Return value**

None

**Example**

```java
// MethodListener.java

public class MethodListener implements IMethodResult {
    @Override
    public void onSuccess(Object object) {
        Log.i("MethodListener", "Android bridge onSuccess success");
    }
    
    @Override
    public void onError(String methodName, int errorCode, String errorMessage) {
        Log.i("MethodListener", "Android bridge onError success");
    }
    
    @Override
    public void onMethodCancel(String s) {
        Log.i("MethodListener", "Android bridge onMethodCancel success");
    }
}
```

```java
// EntryEntryAbilityActivity.java
public class EntryEntryAbilityActivity extends StageActivity {
    protected void onCreate(Bundle savedInstanceState) {
      	// Create a platform bridge object.
        BridgeImpl bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
      	// Register a method result listener.
        bridgeImpl.setMethodResultListener(new MethodListener());
        // Set bundleName to the actual bundle name.
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```



#### getBridgeType

```java
public BridgeType getBridgeType();
```

**Description**

Obtains the data encoding and decoding format of this bridge.

**Parameters**

None

**Return value**

| Type  | Description                              |
| ---------- | ---------------------------------- |
| BridgeType | Data encoding and decoding format.|



#### isBridgeAvailable

```java
public boolean isBridgeAvailable();
```

**Description**

Checks whether this bridge is available.

**Parameters**

None

**Return value**

| Type| Description                                              |
| -------- | -------------------------------------------------- |
| boolean  | Returns **true** if the bridge instance is available; returns **false** otherwise.|



#### unRegister

```java
public boolean unRegister(String bridgeName);
```

**Description**

Deregisters a bridge based on **bridgeName**.

**Parameters**

| Name       | Type  | Mandatory| Description          |
| ---------- | ------ | ---- | -------------- |
| bridgeName | String | Yes  | Bridge name.|

**Return value**

| Type| Description                                    |
| -------- | ---------------------------------------- |
| boolean  | Returns **true** if the deregistration is successful; returns **false** otherwise.|



## BridgeManager

Manages **BridgePlugin** instances, which are created and destroyed by **StageActiivty**. You obtain a **BridgeManager** instance through the **getBridgeManager()** method of **StageActivity**.

You do not need to handle the APIs in the **BridgeManager** class.



## MethodData

Stores method names and parameters on the ArkUI side for the **callMethod** API on the platform to use.

### Constructor

```java
public MethodData(String methodName, Object[] parameter);
```

**Description**

Constructs a **MethodData** object with a method name and parameter list on the ArkUI side.

**Parameters**

| Name       | Type    | Mandatory| Description                     |
| ---------- | -------- | ---- | ------------------------- |
| methodName | String   | Yes  | Name of the ArkUI method.      |
| parameter  | Object[] | Yes  | Parameter list of the ArkUI method.|



## TaskOption<sup>11+</sup>

Defines the platform bridge thread concurrency mode, to be used by the **BridgePlugin** constructor on the platform.

### Constructor

```java
public TaskOption();

public TaskOption(boolean isSerial);
```

**Description**

Specifies whether to use serial execution for threads of the platform bridge. The value **true** (default) means to use serial execution, and **false** means the opposite.

**Parameters**

| Name     | Type   | Mandatory| Description                  |
| -------- | ------- | ---- | ---------------------- |
| isSerial | boolean | No  | Whether to use serial execution for threads.|



## ErrorCode

The table below lists the status codes of the platform bridge.

| Error Code | Error Message              | Description                                                  |
| ---------- | -------------------------- | ------------------------------------------------------------ |
| 0          | "Correct!"                 | Success.                                                     |
| 1          | "Bridge name error!"       | The bridge name is incorrect.                                |
| 2          | "Bridge creation failure!" | Failed to create the bridge.                                 |
| 3          | "Bridge unavailable!"      | The bridge is unavailable.                                   |
| 4          | "Method name error!"       | No method name is provided.                                  |
| 5          | "Method is running..."     | The method is running...                                     |
| 6          | "Method not implemented!"  | Method not yet implemented.                                  |
| 7          | "Method parameter error!"  | Parameter error. For example, the argument and parameter do not match. |
| 8          | "Method already exists!"   | The method has already been registered.                      |
| 9          | "Data error"               | Data error.                                                  |



## Example

  ```java
// BridgeImpl.java
package com.example.bridgeDemo;

import android.content.Context;
import ohos.ace.adapter.capability.bridge.BridgeManager;
import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class BridgeImpl extends BridgePlugin {
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager) {
        super(context, name, bridgeManager);
    }
}
  ```

```java
// MessageListener.java
package com.example.bridgeDemo;

import android.util.Log;

import ohos.ace.adapter.capability.bridge.IMessageListener;

public class MessageListener implements IMessageListener {
    @Override
    public Object onMessage(Object object) {
        Log.i("MessageListener", "onMessage success");
        return "android success onMessage";
    }
    @Override
    public void onMessageResponse(Object object) {
        Log.i("MessageListener", "onMessageResponse success");
    }
}
```

```java
// EntryEntryAbilityActivity.java
package com.example.bridgeDemo;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {
    private BridgeImpl bridgeImpl = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager(), BridgePlugin.BridgeType.BINARY_TYPE);
        bridgeImpl.setMessageListener(new MessageListener());
        // Set bundleName to the actual bundle name.
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main_activity);
        testSendMessage();
    }

    public void testSendMessage() {
        // Touch the button to send a message.
        Button button = (Button) findViewById(R.id.TestSendMessage);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String androidData = "Android side data";
                bridgeImpl.sendMessage(androidData);
            }
        });
    }
}
```

####

## Sample

[Sample Code](../../tutorial/how-to-use-bridge-on-android.md#example-scenario)
