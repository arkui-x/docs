# BridgePlugin (平台桥接)

本模块将详细阐述平台桥接**Android端**的API方法及相关定义。ArkTS侧具体用法请参考[Bridge API](../apis/js-apis-bridge.md)。

**起始版本：**

10

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## BridgePlugin类

抽象类，开发者需要扩展此类并创建实例，用于Android与ArkTS通信。

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

**废弃：**

从API version 11开始废弃，建议使用[最新的构造函数](#bridgeplugin22)替代。

**参数：**

| Name       | 类型    | 必填 | 描述                                                    |
| ---------- | ------- | ---- | ------------------------------------------------------- |
| context    | Context | 是   | 当前Activity（Android原生）的上下文                   |
| bridgeName | String  | 是   | 平台桥接名称 (必须与ArkTS侧一致)                      |
| instanceId | int     | 是   | 实例ID，可通过StageActivity的getInstanceId() 方法获取 |


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

使用BridgeManager实例构建平台桥接(BridgePlugin)对象实例，可指定数据编解码格式（默认为JSON_TYPE）。<br>
从API version 22开始，建议使用[最新的构造函数](#bridgeplugin22)替代。

**参数：**

| Name          | 类型                              | 必填 | 描述                                                         |
| ------------- | --------------------------------- | ---- | ------------------------------------------------------------ |
| context       | Context                           | 是   | 当前Activity（Android原生）的上下文                        |
| bridgeName    | String                            | 是   | 平台桥接名称 (必须与ArkTS侧一致)                           |
| bridgeManager | [BridgeManager](#bridgemanager类) | 是   | BridgePlugin对象管理器，可通过StageActivity的getBridgeManager() 方法获取 |
| bridgeType    | [BridgeType](#bridgetype11)       | 否   | 数据编解码格式，默认为JSON_TYPE                            |
| taskOption    | [TaskOption](#taskoption类11)     | 是   | BridgePlugin线程并发模式，默认为连续并发模式               |

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
    public BridgeImpl(Context context, String name, BridgeManager bridgeManager, BridgeType codecType, TaskOption taskOption) {
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
        bridgeImplByManagerAndType = new BridgeImpl(this, "BridgeImplByManagerAndType", getBridgeManager(), BridgePlugin.BridgeType.BINARY_TYPE);
        
        // 以线程连续并发模式, 使用bridgeManager对象实例构建BridgeImpl实例
        bridgeImplByManagerAndTask = new BridgeImpl(this, "BridgeImplByManagerAndTask", getBridgeManager(), BridgePlugin.BridgeType.BINARY_TYPE, new TaskOption());
        
        // bundleName需根据实际情况调整。
        setInstanceName("bundleName:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
    }
}
```



#### BridgePlugin<sup>22+</sup>

```java
public BridgePlugin(String bridgeName, BridgeType bridgeType);
```

**描述：**

依据bridgeName和BridgeType构建平台桥接(BridgePlugin)对象实例，可指定数据编解码格式（默认为JSON_TYPE）。

**参数：**

| Name       | 类型       | 必填 | 描述                               |
| ---------- | ---------- | ---- | ---------------------------------- |
| bridgeName | String     | 是   | 平台桥接名称 (必须与ArkTS侧一致) |
| bridgeType | [BridgeType](#bridgetype11) | 是   | 数据编解码格式，默认为JSON_TYPE  |

**示例：** 

  ```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
    // 指定名称和数据编解码格式构建BridgePlugin对象
    public BridgeImpl(String name, BridgeType bridgeType) {
        super(name, bridgeType);
}
  ```

```java
// EntryEntryAbilityActivity.java

public class EntryEntryAbilityActivity extends StageActivity {
    private BridgeImpl bridgeImpl = null;

    protected void onCreate(Bundle savedInstanceState) {
        // 以二进制数据编解码格式, 构建名称为"BridgeName"的BridgeImpl实例对象。
        bridgeImplByManager = new BridgeImpl("BridgeName", BridgePlugin.BridgeType.BINARY_TYPE);
        
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

将数据(data)发送到ArkTS侧，ArkTS侧应答数据通过 IMessageListener.onMessageResponse 接口接收。

**参数：** 

| Name | 类型   | 必填 | 描述   |
| ---- | ------ | ---- | ------ |
| data | Object | 是   | 待发送数据 |

> **说明**
>
> [数据类型支持](../../quick-start/platform-bridge-introduction.md#数据类型支持)

**返回值：** 

无

**示例：**

接口具体使用方式详见[平台桥接开发指南-Android](../../tutorial/how-to-use-bridge-on-android.md#场景二android侧向arkui侧传递数据)

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
        		// 向ArkTS侧发送数据
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

 调用ArkTS侧定义的方法，ArkTS侧方法的函数返回值通过 IMethodResult.onSucessess 接口接收。

**参数：** 

| Name       | 类型                        | 必填 | 描述                                    |
| ---------- | --------------------------- | ---- | --------------------------------------- |
| methodData | [MethodData](#methoddata类) | 是   | ArkTS侧方法描述，即方法名称、参数列表 |

**返回值：** 

无

**示例：**

接口具体使用方式详见[平台桥接开发指南-Android](../../tutorial/how-to-use-bridge-on-android.md#场景四android侧调用arkui侧的方法)

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
                // 定义对象数组，存放ArkTS侧方法形参对应的实参
                Object[] paramObject = { "param1", "param2" };
                // 构造ArkTS侧方法描述对象实例, jsMethodName需根据实际情况调整。
                MethodData methodData = new MethodData("jsMethodName", paramObject);
                // 调用ArkTS侧的方法（jsMethodName）
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

 调用ArkTS侧定义的方法，ArkTS侧方法的函数返回值通过 IMethodResult.onSucessess 接口接收。

**参数：** 

| Name       | 类型      | 必填 | 描述                  |
| ---------- | --------- | ---- | --------------------- |
| methodName | String    | 是   | ArkTS侧方法名称     |
| parameters | Object... | 否   | ArkTS侧方法参数列表 |

**返回值：** 

无

**示例：**

接口具体使用方式详见[平台桥接开发指南-Android](../../tutorial/how-to-use-bridge-on-android.md#场景四android侧调用arkui侧的方法)

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
                // ArkTS侧函数名加ArkTS侧方法形参对应的实参调用
                bridgeImpl.callMethod("jsMethodName", "param1", "param2");
            }
        });
    }
}
```



#### callMethodSync<sup>22+</sup>

```java
public Object callMethodSync(String methodName, Object... parameters)
```

**描述：**

 以同步方式调用ArkTS侧定义的方法，ArkTS侧方法的函数返回值同步返回，即函数本身的返回值。

**参数：** 

| Name       | 类型      | 必填 | 描述                  |
| ---------- | --------- | ---- | --------------------- |
| methodName | String    | 是   | ArkTS侧方法名称     |
| parameters | Object... | 否   | ArkTS侧方法参数列表 |

**返回值：** 

ArkTS侧方法的函数返回值。

**示例：**

```java
// BridgeImpl.java

public class BridgeImpl extends BridgePlugin {
    public BridgeImpl(String bridgeName, BridgeType bridgeType) {
        super(bridgeName, bridgeType);
    }
}
```

```java
// EntryEntryAbilityActivity.java

public class EntryEntryAbilityActivity extends StageActivity {
    public BridgeImpl bridgeImpl = null;
    
    protected void onCreate(Bundle savedInstanceState) {
        // 创建平台桥接对象实例
        bridgeImpl = new BridgeImpl("BridgeName", BridgePlugin.BridgeType.BINARY_TYPE());
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
                // ArkTS侧函数名加ArkTS侧方法形参对应的实参调用，同步返回结果
                Object result = bridgeImpl.callMethodSync("jsMethodName", "param1", "param2");
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

设置数据信息监听回调，用于捕获ArkTS侧发送的数据信息。

**参数：** 

| Name            | 类型             | 必填 | 描述               |
| --------------- | ---------------- | ---- | ------------------ |
| messageListener | [IMessageListener](#imessagelistener接口) | 是   | 数据信息监听接口实例 |

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
      	// 注册数据信息监听。
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

设置方法调用相关监听回调，用于接收Android侧调用ArkTS侧方法的执行结果以及ArkTS侧方法移除的事件。

**参数：** 

| Name                 | 类型          | 必填 | 描述                 |
| -------------------- | ------------- | ---- | -------------------- |
| methodResultListener | [IMethodResult](#imethodresult接口) | 是   | 方法返回监听接口类 |

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
| [BridgeType](#bridgetype11) | 返回平台桥接对象的数据编解码格式。 |



#### isBridgeAvailable

```java
public boolean isBridgeAvailable();
```

**描述：** 

检查当前平台桥接通道是否处于可通信状态。

**参数：** 

无

**返回值：** 

| **类型** | 说明                                               |
| -------- | -------------------------------------------------- |
| boolean  | true表示当前平台桥接通道可以进行通信。<br>false表示不可通信，请检查ArkTS侧bridge对象实例是否创建 |



#### unRegister

```java
public boolean unRegister(String bridgeName);
```

**描述：** 

注销指定bridgeName平台桥接对象的实例。

**参数：** 

| Name       | 类型   | 必填 | 描述           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | String | 是   | 平台桥接实例名称 |

**返回值：** 

| **类型** | 说明                                     |
| -------- | ---------------------------------------- |
| boolean  | true表示注销成功。false表示注销失败 |


## BridgeManager类

用于管理BridgePlugin实例，由StageActiivty负责创建和销毁。开发者通过StageActivity的getBridgeManager() 获取BridgeManager实例，用于构建BridgePlugin实例。

**开发者不需要关心BridgeManager类中的接口。**


## **IMessageListener接口**

用于接收ArkTS侧发送的数据信息。接口类，由开发者自行实现回调并通过setMessageListener方法设置到BridgePlugin实例中。

### onMessage

```java
Object onMessage(Object data);
```

**描述：**

接收ArkTS侧通过sendMessage方法发送的数据信息。

**参数：**

| Name | 类型   | 必填 | 描述                |
| ---- | ------ | ---- | ------------------- |
| data | Object | 是   | ArkTS侧发送的数据信息 |

**返回值：**

| 类型   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| Object | 应答数据信息，即Android侧接收到ArkTS侧发送的数据信息后，回复ArkTS侧的数据信息 |



### onMessageResponse

```java
void onMessageResponse(Object data);
```

**描述：**

用于接收Android侧调用sendMessage发送数据信息后，ArkTS侧应答的数据信息，即接收ArkTS侧setMessageListener注册的方法的返回值。


**参数：**

| Name | 类型   | 必填 | 描述                                                 |
| ---- | ------ | ---- | ---------------------------------------------------- |
| data | Object | 是   | Android侧调用sendMessage发送数据信息后，ArkTS侧的应答数据信息 |

**返回值：**

无



## IMethodResult接口

用于监听Android侧调用ArkTS侧方法的执行情况。接口类，由开发者自行实现回调并通过setMethodResultListener方法设置到BridgePlugin实例中。

### onSuccess

```java
void onSuccess(Object resultValue);
```

**描述：**

Android侧成功调用ArkTS侧定义的方法时触发该接口，并将ArkTS侧被调用方法的函数返回值传递给Android侧。

**参数：**

| Name        | 类型   | 必填 | 描述                  |
| ----------- | ------ | ---- | --------------------- |
| resultValue | Object | 是   | ArkTS侧被调用方法的函数返回值 |

**返回值：**

无



### onError

```java
void onError(String methodName, int errorCode, String errorMessage);
```

**描述：**

Android侧调用ArkTS侧定义方法时，如果出现异常错误则触发该接口，并将异常错误信息返回给Android侧。

**参数：**

| Name                      | 类型   | 必填 | 描述              |
| ------------------------- | ------ | ---- | ----------------- |
| methodName                | String | 是   | ArkTS侧被调用方法的名称，即函数名 |
| [errorCode](#errorcode类) | int    | 是   | 异常错误码          |
| errorMessage              | String | 是   | 异常错误信息描述    |

**返回值：**

无



### onMethodCancel

```java
void onMethodCancel(String methodName);
```

**描述：**

ArkTS侧调用unRegisterMethod方法移除已注册方法时将触发该接口，用于通知Android侧相关方法被移除。

**参数：**

| Name       | 类型   | 必填 | 描述                      |
| ---------- | ------ | ---- | ------------------------- |
| methodName | String | 是   | ArkTS侧被移除的方法名称，即函数名 |

**返回值：**

无


## MethodData类

存储ArkTS侧的方法名称和参数列表，供Android侧callMethod方法使用。

### 构造函数

```java
public MethodData(String methodName, Object[] parameter);
```

**描述：**

使用ArkTS侧的方法名称和参数列表构造MethodData。

**参数：**

| Name       | 类型     | 必填 | 描述                      |
| ---------- | -------- | ---- | ------------------------- |
| methodName | String   | 是   | 调用ArkTS侧方法名       |
| parameter  | Object[] | 是   | 调用ArkTS侧方法参数列表 |



## **TaskOption类<sup>11+</sup>**

指定平台桥接线程并发模式类，供bridgeplugin构造函数使用。

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
| isSerial | boolean | 否   | 控制线程连续并发模式 |



## ErrorCode类

平台桥接的状态返回码。

| errorCode        | errorMessage                  | 描述                               |
| ---------------- | ----------------------------- | ---------------------------------- |
| 0                | "Correct!"                    | 成功                               |
| 1                | "Bridge name error!"          | Bridge名称错误                     |
| 2                | "Bridge creation failure!"    | 创建Bridge失败                     |
| 3                | "Bridge unavailable!"         | Bridge不可用                       |
| 4                | "Method name error!"          | 没有提供方法名称                    |
| 5                | "Method is running..."        | 方法正在运行...                    |
| 6                | "Method not implemented!"     | 方法未实现。                       |
| 7                | "Method parameter error!"     | 方法参数错误，如实参与形参不对应     |
| 8                | "Method already exists!"      | 方法已经被注册了                    |
| 9                | "Data error"                  | 数据错误                           |
| 10<sup>11+</sup> | "Bottom Communication error!" | 底层通信错误                       |
| 11<sup>11+</sup> | "Bridge codec type mismatch"  | Bridge编解码器类型不匹配            |
| 12<sup>11+</sup> | "Bridge codec is invalid"     | Bridge编解码器无效                 |

####

## 完整示例

[完整示例](../../tutorial/how-to-use-bridge-on-android.md#场景示例)