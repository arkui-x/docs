# BridgePlugin (平台桥接)

本模块提供ArkUI侧和Android平台侧消息通信的功能，包括数据传输、方法调用和事件调用。需配套ArkUI侧API使用，ArkUI侧具体用法请参考[Bridge API](../apis/js-apis-bridge.md)。

**起始版本：**

10

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。



## **IMessageListener接口**

用于监听ArkUI侧发送的消息。接口类，由开发者实现并通过setMessageListener方法设置到BridgePlugin实例中。

### onMessage

```java
Object onMessage(Object data);
```

**描述：**用于监听ArkUI侧发送信息，即接收ArkUI侧通过sendMessage方法发送的消息。

**参数：**

| Name | 类型   | 必填 | 描述                |
| ---- | ------ | ---- | ------------------- |
| data | Object | 是   | ArkUI侧发送的消息。 |

**返回值：**

| 类型   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| Object | 应答消息，即平台侧接收到ArkUI侧的消息后，回复ArkUI侧的消息。 |



### onMessageResponse

```java
void onMessageResponse(Object data);
```

**描述：**用于监听平台侧调用sendMessage发送消息后，ArkUI侧应答的消息，即接收ArkUI侧setMessageListener注册的方法的返回值。

**参数：**

| Name | 类型   | 必填 | 描述                                                 |
| ---- | ------ | ---- | ---------------------------------------------------- |
| data | Object | 是   | 平台侧调用sendMessage发送消息后，ArkUI侧的应答消息。 |

**返回值：**

无



## IMethodResult接口

用于监听平台侧调用ArkUI侧方法的执行情况。接口类，由开发者实现并通过setMethodResultListener方法设置到BridgePlugin实例中。

### onSuccess

```java
void onSuccess(Object resultValue);
```

**描述：**

平台侧成功调用ArkUI侧定义的方法时触发该接口，并将ArkUI侧方法的返回值传递给平台侧。

**参数：**

| Name        | 类型   | 必填 | 描述                  |
| ----------- | ------ | ---- | --------------------- |
| resultValue | Object | 是   | ArkUI侧方法的返回值。 |

**返回值：**

无



### onError

```java
void onError(String methodName, int errorCode, String errorMessage);
```

**描述：**

平台侧调用ArkUI侧定义方法时，如果出错则触发该接口，并将出错信息返回平台侧。

**参数：**

| Name                      | 类型   | 必填 | 描述              |
| ------------------------- | ------ | ---- | ----------------- |
| methodName                | String | 是   | ArkUI侧方法名称。 |
| [errorCode](#errorcode类) | int    | 是   | 错误码。          |
| errorMessage              | String | 是   | 错误信息描述。    |

**返回值：**

无



### onMethodCancel

```java
void onMethodCancel(String methodName);
```

**描述：**

ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。

**参数：**

| Name       | 类型   | 必填 | 描述                      |
| ---------- | ------ | ---- | ------------------------- |
| methodName | String | 是   | ArkUI侧被注销的事件名称。 |

**返回值：**

无



## BridgePlugin类

抽象类，提供平台间通信能力，开发者需要扩展此类并创建实例，用于ArkUI与原生平台通信。

### 类型定义

#### **BridgeType<sup>11+</sup>**

**描述：** 

枚举类型，指定数据编解码格式。

| 枚举值      | 描述                   | **引用方式**                        |
| ----------- | ---------------------- | ----------------------------------- |
| JSON_TYPE   | JSON格式序列化编解码   | BridgePlugin.BridgeType.JSON_TYPE   |
| BINARY_TYPE | 二进制格式序列化编解码 | BridgePlugin.BridgeType.BINARY_TYPE |



### 构造函数

#### BridgePlugin<sup>(废弃)</sup>

```java
public BridgePlugin(Context context, String bridgeName, int instanceId);
```

**描述：**

使用instanceId构建平台桥接(BridgePlugin)对象实例，数据编解码格式为JSON_TYPE（默认）。

**参数：**

| Name       | 类型    | 必填 | 描述                                                    |
| ---------- | ------- | ---- | ------------------------------------------------------- |
| context    | Context | 是   | 当前Activity（Android原生）的上下文。                   |
| bridgeName | String  | 是   | 平台桥接名称 (必须与ArkUI侧一致)。                      |
| instanceId | int     | 是   | 实例ID，可通过StageActivity的getInstanceId() 方法获取。 |

**废弃：**

从API version 11开始废弃，建议使用BridgePlugin<sup>11+</sup>替代。

**示例：**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
    // 使用instanceId构建BridgePlugin对象，数据编解码格式默认为 JSON_TYPE
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
        // 使用instanceId构建BridgeImpl实例
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

**描述：**

使用BridgeManager实例构建平台桥接(BridgePlugin)对象实例，可指定数据编解码格式（默认 JSON_TYPE）。

**参数：**

| Name          | 类型                              | 必填 | 描述                                                         |
| ------------- | --------------------------------- | ---- | ------------------------------------------------------------ |
| context       | Context                           | 是   | 当前Activity（Android原生）的上下文。                        |
| bridgeName    | String                            | 是   | 平台桥接名称 (必须与ArkUI侧一致)。                           |
| bridgeManager | [BridgeManager](#bridgemanager类) | 是   | BridgePlugin对象管理器，可通过StageActivity的getBridgeManager() 方法获取。 |
| bridgeType    | BridgeType                        | 否   | 数据编解码格式，默认为JSON_TYPE。                            |
| taskOption    | TaskOption                        | 是   | BridgePlugin线程并发模式，默认为连续并发模式。               |

**示例：** 

  ```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
	// 使用BridgeManager对象构造BridgePlugin对象，数据编解码格式为JSON_TYPE(默认)
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager) {
        super(context, name, bridgeManager);
    }

    // 使用BridgeManager对象且指定数据编解码格式构建BridgePlugin对象
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager, BridgeType bridgeType) {
        super(context, name, bridgeManager);
    }
    
    // 使用BridgeManager对象且指定线程并发模式构建BridgePlugin对象
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
        // 使用bridgeManager对象实例构建BridgeImpl实例
        bridgeImplByManager = new BridgeImpl(this, "BridgeImplByManager", getBridgeManager());
        
        // 以二进制数据编解码格式, 使用bridgeManager对象实例构建BridgeImpl实例
        bridgeImplByManagerAndType = new BridgeImpl(this, "BridgeImplByManagerAndType", getBridgeManager(), 					BridgePlugin.BridgeType.BINARY_TYPE);
        
        // 以线程连续并发模式, 使用bridgeManager对象实例构建BridgeImpl实例
        bridgeImplByManagerAndTask = new BridgeImpl(this, "BridgeImplByManagerAndTask", getBridgeManager(), 					BridgePlugin.BridgeType.BINARY_TYPE, new TaskOption());
        
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```



### 成员函数

#### sendMessage

```java
public void sendMessage(Object data);
```

**描述：** 

将数据(data)发送到ArkUI侧，ArkUI侧应答信息通过 IMessageListener.onMessageResponse 接口获取。

**参数：** 

| Name | 类型   | 必填 | 描述   |
| ---- | ------ | ---- | ------ |
| data | Object | 是   | 数据。 |

> **说明**
>
> [数据类型支持](../../quick-start/platform-bridge-introduction.md#数据类型支持)

**返回值：** 

无

**示例：**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
	// 使用BridgeManager对象构造BridgePlugin对象，数据编解码格式为JSON_TYPE(默认)
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
        // 创建平台桥接对象实例
        bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        // 注册TestSendMessage按钮
        testSendMessage();
    }

    public void testSendMessage() {
        // 使用button按钮点击，发送信息。
        Button button = (Button) findViewById(R.id.TestSendMessage);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                String androidData = "Android side data";
        		// 向ArkUI侧发送数据
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

**描述：**

 调用ArkUI侧定义的方法，ArkUI侧方法的返回结果通过 IMethodResult.onSucessess 接口获取。

**参数：** 

| Name       | 类型                        | 必填 | 描述                                    |
| ---------- | --------------------------- | ---- | --------------------------------------- |
| methodData | [MethodData](#methoddata类) | 是   | ArkUI侧方法描述，即方法名称、参数列表。 |

**返回值：** 

无

**示例：**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
	// 使用BridgeManager对象构造BridgePlugin对象，数据编解码格式为JSON_TYPE(默认)
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
        // 创建平台桥接对象实例
        bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        // 注册TestCallMethod按钮
        testCallMethod();
    }

	public void testCallMethod() {
        // 使用button按钮点击，发送信息。
        Button button = (Button) findViewById(R.id.TestCallMethod);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 定义对象数组，存放ArkUI侧方法形参对应的实参
                Object[] paramObject = { "param1", "param2" };
                // 构造ArkUI侧方法描述对象实例, jsMethodName需根据实际情况调整。
                MethodData methodData = new MethodData("jsMethodName", paramObject);
                // 调用ArkUI侧的方法（jsMethodName）
                bridgeImpl.callMethod(methodData);
            }
        });
    }
}
```



#### callMethod

```java
public void callMethod(String methodName, Object... parameters);
```

**描述：**

 调用ArkUI侧定义的方法，ArkUI侧方法的返回结果通过 IMethodResult.onSucessess 接口获取。

**参数：** 

| Name       | 类型      | 必填 | 描述                  |
| ---------- | --------- | ---- | --------------------- |
| methodName | String    | 是   | ArkUI侧方法名称。     |
| parameters | Object... | 否   | ArkUI侧方法参数列表。 |

**返回值：** 

无

**示例：**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
    // 使用BridgeManager对象构造BridgePlugin对象，数据编解码格式为JSON_TYPE(默认)
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
        // 创建平台桥接对象实例
        bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        // 注册TestCallMethod按钮
        testCallMethod();
    }

	public void testCallMethod() {
        // 使用button按钮点击，发送信息。
        Button button = (Button) findViewById(R.id.TestCallMethod);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
                bridgeImpl.callMethod("jsMethodName", "param1", "param2");
            }
        });
    }
}
```

#### setMessageListener

```java
public void setMessageListener(IMessageListener messageListener);
```

**描述：** 

设置消息监听，用于捕获ArkUI侧发送的消息。

**参数：** 

| Name            | 类型             | 必填 | 描述               |
| --------------- | ---------------- | ---- | ------------------ |
| messageListener | IMessageListener | 是   | 消息监听接口实例。 |

**返回值：** 

无

**示例：**

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
      	// 创建平台桥接对象。
        BridgeImpl bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
      	// 注册消息监听。
        bridgeImpl.setMessageListener(new MessageListener());
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```



#### setMethodResultListener

```java
public void setMethodResultListener(IMethodResult methodResultListener);
```

**描述：**  

设置监听器，用于捕获平台侧调用ArkUI侧方法的执行结果以及ArkUI侧注销的事件。

**参数：** 

| Name                 | 类型          | 必填 | 描述                 |
| -------------------- | ------------- | ---- | -------------------- |
| methodResultListener | IMethodResult | 是   | 方法返回监听接口类。 |

**返回值：** 

无

**示例：**

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
      	// 创建平台桥接对象。
        BridgeImpl bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
      	// 注册方法返回监听。
        bridgeImpl.setMethodResultListener(new MethodListener());
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```



#### getBridgeType

```java
public BridgeType getBridgeType();
```

**描述：** 

获取平台桥接对象当前使用的数据编解码格式。

**参数：** 

无

**返回值：** 

| **类型**   | 说明                               |
| ---------- | ---------------------------------- |
| BridgeType | 返回BridgePlugin的数据编解码格式。 |



#### isBridgeAvailable

```java
public boolean isBridgeAvailable();
```

**描述：** 

检查当前平台桥接对象是否可用。

**参数：** 

无

**返回值：** 

| **类型** | 说明                                               |
| -------- | -------------------------------------------------- |
| boolean  | 返回值为true表示当前平台桥接对象可用，否则不可用。 |



#### unRegister

```java
public boolean unRegister(String bridgeName);
```

**描述：** 

注销指定bridgeName平台桥接对象的实例。

**参数：** 

| Name       | 类型   | 必填 | 描述           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | String | 是   | 平台桥接名称。 |

**返回值：** 

| **类型** | 说明                                     |
| -------- | ---------------------------------------- |
| boolean  | 返回值为true表示注销成功，否则注销失败。 |



## BridgeManager类

用于管理BridgePlugin实例，由StageActiivty负责创建和销毁。开发者通过StageActivity的getBridgeManager() 获取BridgeManager实例，用于构建BridgePlugin实例。

**开发者不需要关心BridgeManager类中的接口。**



## MethodData类

存储ArkUI侧的方法名称和参数列表，供平台侧callMethod方法使用。

### 构造函数

```java
public MethodData(String methodName, Object[] parameter);
```

**描述：**

使用ArkUI侧的方法名称和参数列表构造MethodData。

**参数：**

| Name       | 类型     | 必填 | 描述                      |
| ---------- | -------- | ---- | ------------------------- |
| methodName | String   | 是   | 调用ArkUI侧方法名。       |
| parameter  | Object[] | 是   | 调用ArkUI侧方法参数列表。 |



## **TaskOption类<sup>11+</sup>**

指定平台桥接线程并发模式类，供平台侧bridgeplugin构造函数使用

### 构造函数

```java
public TaskOption();

public TaskOption(boolean isSerial);
```

**描述：** 

指定平台桥接线程并发模式(默认为线程连续并发模式)，使用true构造指定线程连续并发模式，使用false构造指定线程无序并发模式。

**参数：**

| Name     | 类型    | 必填 | 描述                   |
| -------- | ------- | ---- | ---------------------- |
| isSerial | boolean | 否   | 控制线程连续并发模式。 |



## ErrorCode类

平台桥接的状态返回码。

| errorCode | errorMessage               | 描述                               |
| --------- | -------------------------- | ---------------------------------- |
| 0         | "Correct!"                 | 成功。                             |
| 1         | "Bridge name error!"       | Bridge名称错误。                   |
| 2         | "Bridge creation failure!" | 创建Bridge失败。                   |
| 3         | "Bridge unavailable!"      | Bridge不可用。                     |
| 4         | "Method name error!"       | 没有提供方法名称。                 |
| 5         | "Method is running..."     | 方法正在运行...                    |
| 6         | "Method not implemented!"  | 方法未实现。                       |
| 7         | "Method parameter error!"  | 方法参数错误，如实参与形参不对应。 |
| 8         | "Method already exists!"   | 方法已经被注册了。                 |
| 9         | "Data error"               | 数据错误。                         |



## 示例

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
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        setContentView(R.layout.main_activity);
        testSendMessage();
    }

    public void testSendMessage() {
        // 使用button按钮点击，发送信息。
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

## 完整示例

[完整示例](../../tutorial/how-to-use-bridge-on-android.md#场景示例)
