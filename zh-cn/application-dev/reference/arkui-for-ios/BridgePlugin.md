# BridgePlugin (平台桥接)

本模块将详细阐述平台桥接**iOS端**的API方法及相关定义。ArkTS侧具体用法请参考[Bridge API](../apis/js-apis-bridge.md)。

**起始版本：**

10

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。



## BridgePlugin类

抽象类，开发者需要扩展此类并创建实例，用于iOS与ArkTS通信。

### 类型定义

#### **BridgeType<sup>11+</sup>**

**描述：** 

枚举类型，指定数据编解码格式。

| 枚举值      | 值   | **引用方式**             |
| ----------- | ---- | ------------------------ |
| JSON_TYPE   | 0    | JSON格式序列化编解码   |
| BINARY_TYPE | 1    | 二进制格式序列化编解码 |


### 属性



#### bridgeManager<sup>11+</sup>

```objective-c
@property(nonatomic, strong, readonly) BridgePluginManager* bridgeManager;
```

**描述：** 

获取当前StageViewController的绑定的bridgeManager。

**参数：** 

| 类型                 | 访问权限 |
| -------------------- | -------- |
| [BridgePluginManager*](#bridgepluginmanager类11) | 只读     |



#### instanceId<sup>废弃</sup>

```objective-c
@property(nonatomic, assign, readonly) int32_t instanceId;
```

**描述：** 

获取当前StageViewController的实例ID。

**废弃：**

从API version 11开始废弃。

**参数：** 

| 类型    | 访问权限 |
| ------- | -------- |
| int32_t | 只读     |



#### bridgeName

```objective-c
@property(nonatomic, strong, readonly) NSString* bridgeName;
```

**描述：** 

当前桥接对象实例的名称。

**参数：** 

| 类型      | 访问权限 |
| --------- | -------- |
| NSString* | 只读     |



#### messageListener

```objective-c
@property (nonatomic, assign) id<IMessageListener> messageListener;
```

**描述：** 

设置数据信息监听代理，用于捕获ArkTS侧发送的数据信息。

**参数：** 

| 类型                                          | 访问权限 |
| --------------------------------------------- | -------- |
| id\<[IMessageListener](#imessagelistener代理)> | 读写     |



#### methodResult

```objective-c
@property (nonatomic, assign) id<IMethodResult> methodResult;
```

**描述：** 

设置方法调用相关监听代理，用于接收iOS侧调用ArkTS侧方法的执行结果以及ArkTS侧方法移除的事件。

**参数：** 

| 类型                                    | 访问权限 |
| --------------------------------------- | -------- |
| id\<[IMethodResult](#imethodresult代理)> | 读写     |



#### taskOption<sup>11+</sup>

```objective-c
@property(nonatomic, strong, readonly) TaskOption* taskOption;
```

**描述：** 

获取当前平台桥接对象平台桥接线程并发模式类。

**参数：** 

| 类型                          | 访问权限 |
| ----------------------------- | -------- |
| [TaskOption](#taskoption类11) | 只读     |



#### type<sup>11+</sup>

```objective-c
@property (nonatomic, assign, readonly) BridgeType type;
```

**描述：** 

获取平台桥接对象当前使用的数据编解码格式。

**参数：** 

| 类型                        | 访问权限 |
| --------------------------- | -------- |
| [BridgeType](#bridgetype11) | 只读     |



### 成员函数

#### initBridgePlugin<sup>废弃</sup>

```javascript
(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName instanceId:(int32_t)instanceId;
```

**描述：**

使用instanceId构建平台桥接(BridgePlugin)对象实例，数据编解码格式为JSON_TYPE（默认）。

**废弃：**

从API version 11开始废弃，建议使用[最新的构造函数](#initbridgeplugin22)替代。

**参数：** 

| 参数名     | 类型       | 必填 | 说明           |
| ---------- | ---------- | ---- | -------------- |
| bridgeName | NSString * | 是   | 定义桥接名称 |
| instanceId | int32_t    | 是   | 实例ID       |

**返回值：** 

| 类型         | 描述               |
| ------------ | ------------------ |
| [BridgePlugin](#bridgeplugin类) | 平台桥接对象实例 |



#### initBridgePlugin<sup>11+</sup>

```objective-c
(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName
                    bridgeManager:(BridgePluginManager *)bridgeManager;
    
(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName
                    bridgeManager:(BridgePluginManager *)bridgeManager
                    bridgeType:(BridgeType)type;

(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName
                    bridgeManager:(BridgePluginManager *)bridgeManager
                    bridgeType:(BridgeType)type
                    taskOption:(TaskOption*)taskOption;
```

**描述：**

使用BridgePluginManager实例构建平台桥接(BridgePlugin)对象实例，可指定数据编解码格式（默认为JSON_TYPE）和处理数据的方式（默认为异步串行）。<br>
从API version 22开始，建议使用[最新的构造函数](#initbridgeplugin22)替代。

**参数：** 

| 参数名     | 类型   | 必填 | 说明           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | NSString* | 是   | 平台桥接名称 (必须与ArkTS侧一致) |
| bridgeManager | [BridgePluginManager](#bridgepluginmanager类11) | 是   | BridgeManager对象管理器，可通过StageViewController的getBridgeManager()方法获取 |
| bridgeType | [BridgeType](#bridgetype11) | 否 | 数据编解码格式，默认为JSON_TYPE |
| taskOption | [TaskOption](#taskoption类11) | 否 | 处理数据的方式，默认为异步串行 |

**返回值：** 

| 类型                            | 说明           |
| ------------------------------- | -------------- |
| [BridgePlugin](#bridgeplugin类) | 平台桥接对象实例 |

**示例：** 

```objective-c
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {
        [self initBridgePlugin];
    }
    return self;
}

// 提供了4种构造构造方法范例，开发者可根据实际情况自行选择其中一种构造方式。
- (void)InitBridgePlugin {
    int32_t instancedId = [self getInstanceId];
    // 构造方法1. 通过instanceId 构造 此方法在API 11 已经废弃
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instancedId];
    
    // 构造方法2. 通过bridgeManager 构造
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager]];
 
    // 构造方法3. 通过bridgeType、bridgeManager构造，bridgeType设置编解码类型
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE];

    // 构造方法4. 通过bridgeType、bridgeManager、taskOption构造
    // TaskOption设置队列任务类型 true 为异步串行，false 为异步并行
    TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE taskOption:taskOption];

    // 指定代理
    self.plugin.messageListener = self;
    self.plugin.methodResult = self;
}
```



#### initBridgePlugin<sup>22+</sup>

```objective-c
(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName
                    bridgeType:(BridgeType)type;
```

**描述：**

依据bridgeName和BridgeType构建平台桥接(BridgePlugin)对象实例，可指定数据编解码格式（默认为JSON_TYPE）。

**参数：** 

| 参数名     | 类型       | 必填 | 说明                               |
| ---------- | ---------- | ---- | ---------------------------------- |
| bridgeName | NSString*  | 是   | 平台桥接名称 (必须与ArkTS侧一致) |
| bridgeType | [BridgeType](#bridgetype11) | 是   | 数据编解码格式，默认为JSON_TYPE  |

**返回值：** 

| 类型                            | 说明           |
| ------------------------------- | -------------- |
| [BridgePlugin](#bridgeplugin类) | 平台桥接对象实例 |


**示例：** 

```objective-c
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {
        [self initBridgePlugin];
    }
    return self;
}

- (void)InitBridgePlugin {
    // 构造方法
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeType:JSON_TYPE];

    // 指定代理
    self.plugin.messageListener = self;
    self.plugin.methodResult = self;
}
```



#### callMethod

```objective-c
(void)callMethod:(MethodData *)method;
```

**描述：**

 调用ArkTS侧定义的方法，ArkTS侧方法的函数返回值通过 IMethodResult.onSucessess 接口接收。

**参数：** 

| Name       | 类型                        | 必填 | 描述                                    |
| ---------- | --------------------------- | ---- | --------------------------------------- |
| methodData | [MethodData](#methoddata类) | 是   | ArkTS侧方法描述，即方法名称、参数列表   |

**返回值：** 

无

**示例：**

接口具体使用方式详见[平台桥接开发指南-iOS](../../tutorial/how-to-use-bridge-on-ios.md#场景四ios侧调用arkui侧的方法)

```objective-c
- (void)bridgeCallMethod {
    // 调用ArkTS侧注册的方法，调用之前要实现methodResult代理，通过代理回调得到调用结果（注意method name必须与ArkTS侧方法名保持一致）
    MethodData * method = [[MethodData alloc] initMethodWithName:@"method" parameter:nil];
    [self.plugin callMethod:method];
}
```



#### callMethodSync<sup>22+</sup>

```objective-c
(id)callMethodSync:(NSString*)methodName parameters:(id)firstObj, ... NS_REQUIRES_NIL_TERMINATION;
```

**描述：**

 以同步方式调用ArkTS侧定义的方法，ArkTS侧方法的函数返回值同步返回，即函数本身的返回值。

**参数：** 

| Name       | 类型      | 必填 | 描述                  |
| ---------- | --------- | ---- | --------------------- |
| methodName | NSString* | 是   | ArkTS侧方法名称     |
| parameters | id...     | 否   | ArkTS侧方法参数列表，必须以nil结尾 |

**返回值：** 

ArkTS侧方法的函数返回值。

**示例：**

```objective-c
- (void)bridgeCallMethod {
    // 调用ArkTS侧注册的方法，调用同步接口返回调用结果（注意method name必须与ArkTS侧方法名保持一致,parameters参数必须以nil结尾）
    BridgeClass *bridge = [[BridgeClass alloc] initBridgePlugin:name bridgeType:bridgeType];
    id result = [bridge callMethodSync:@"method" parameters:@"firstObject", nil];
}
```



#### sendMessage

```objective-c
(void)sendMessage:(id)data;
```

**描述：** 

将数据(data)发送到ArkTS侧，ArkTS侧应答数据通过 IMessageListener.onMessageResponse 接口接收。

**参数：** 

| Name | 类型 | 必填 | 描述     |
| ---- | ---- | ---- | -------- |
| data | id   | 是   | 待发送数据 |

> **说明**
>
> [数据类型支持](../../quick-start/platform-bridge-introduction.md#数据类型支持)

**返回值：** 

无

**示例：**

```objective-c
- (void)bridgeSendMessage {
    // 发送数据到ArkTS，调用之前要实现messageListener代理，通过代理回调得到调用结果
    [self.plugin sendMessage:@[@"key",@"value"]];
}
```



#### isBridgeAvailable<sup>11+</sup>

```objective-c
(BOOL)isBridgeAvailable;
```

**描述：**

 检查当前平台桥接通道是否处于可通信状态。

**参数：** 

无

**返回值：** 

| 类型 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| BOOL | true表示当前平台桥接通道可以进行通信。<br>false表示不可通信，请检查ArkTS侧bridge对象实例是否创建 |



#### **unRegister**

```objective-c
(BOOL)unRegister:(NSString*)bridgeName;
```

**描述：**

注销指定bridgeName平台桥接对象的实例。

**参数：** 

| Name       | 类型      | 必填 | 描述           |
| ---------- | --------- | ---- | -------------- |
| bridgeName | NSString* | 是   | 平台桥接实例名称 |

**返回值：** 

| 类型 | 描述                                   |
| ---- | -------------------------------------- |
| BOOL | true表示注销成功。false表示注销失败 |



## BridgePluginManager类<sup>11+</sup>

构造BridgePlugin 传递的参数， 通过StageViewController 的getBridgeManager方法获取。

**开发者不需要关心BridgePluginManager类中的接口。**



## **IMessageListener代理**

用于接收ArkTS侧发送的数据信息。接口类，由开发者自行实现回调并通过messageListener设置到BridgePlugin实例中。此接口在**BridgePlugin.h**中声明。


### onMessage

```objective-c
(id)onMessage:(id)data;
```

**描述：**

接收ArkTS侧通过sendMessage方法发送的数据信息。

**参数：**

| Name | 类型 | 必填 | 描述                |
| ---- | ---- | ---- | ------------------- |
| data | id   | 是   | ArkTS侧发送的数据信息 |

**返回值：**

| 类型 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| id   | 应答数据信息，即iOS侧接收到ArkTS侧发送的数据信息后，回复ArkTS侧的数据信息 |



### onMessageResponse

```javascript
(void)onMessageResponse:(id)data;
```

**描述：**

用于接收iOS侧调用sendMessage发送数据信息后，ArkTS侧应答的数据信息，即接收ArkTS侧setMessageListener注册的方法的返回值。

**参数：**

| Name | 类型 | 必填 | 描述                                                         |
| ---- | ---- | ---- | ------------------------------------------------------------ |
| data | id   | 是   | iOS侧调用sendMessage发送数据信息后，ArkTS侧的应答数据信息 |

**返回值：**

无

**示例：**

IMessageListener代理实现。

```objective-c
@interface EntryEntryAbilityViewController ()<IMessageListener, IMethodResult>
@end
```

```objective-c
// pragma mark - IMessageListener ArkTS侧发送的数据信息代理实现 （注意 BridgePlugin 对象 messageListener 属性添加监听者）

- (NSString*)onMessage:(id)data {
    // NSLog(@"ArkTS 传递消息给iOS message: %@", data);
    return @"ios onMessage respone message";
}

- (void)onMessageResponse:(id)data {
    // NSLog(@"ArkTS onMessageResponse: %@", data);
}
```

## IMethodResult代理

用于监听iOS侧调用ArkTS侧方法的执行情况。接口类，由开发者自行实现回调并通过methodResult设置到BridgePlugin实例中。此接口在**BridgePlugin.h**中声明。


### onSuccess

```objective-c
(void)onSuccess:(NSString *)methodName resultValue:(id)resultValue;
```

**描述：**

iOS成功调用ArkTS定义的方法时触发该接口，并将ArkTS侧被调用方法的函数返回值传递给iOS侧。

**参数：**

| Name        | 类型       | 必填 | 描述                  |
| ----------- | ---------- | ---- | --------------------- |
| methodName  | NSString * | 是   | ArkTS侧方法的名称，即函数名   |
| resultValue | id         | 是   | ArkTS侧被调用方法的函数返回值 |

**返回值：**

无



### onError

```objective-c
(void)onError:(NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(NSString *)errorMessage;
```

**描述：**

iOS侧调用ArkTS侧定义方法时，如果出现异常错误则触发该接口，并将异常错误信息返回给iOS侧。

**参数：**

| Name         | 类型       | 必填 | 描述              |
| -------------| ---------- | ---- | ----------------- |
| methodName   | NSString * | 是   | ArkTS侧被调用方法的名称，即函数名 |
| errorCode    | [ErrorCode](#ErrorCode)  | 是   | 异常错误码          |
| errorMessage | NSString * | 是   | 异常错误信息描述    |

**返回值：**

无



### onMethodCancel

```objective-c
(void)onMethodCancel:(NSString *)methodName;
```

**描述：**

ArkTS侧调用unRegisterMethod方法移除已注册方法时将触发该接口，用于通知iOS侧相关方法被移除。

**参数：**

| Name       | 类型       | 必填 | 描述                      |
| ---------- | ---------- | ---- | ------------------------- |
| methodName | NSString * | 是   | ArkTS侧被注销的事件名称 |

**返回值：**

无

**示例：** 

IMethodResult代理实现。

```objective-c
// pragma mark - IMethodResult 用于监听平台侧调用ArkTS侧注册的方法的执行情况

- (void)onSuccess:(nonnull NSString *)methodName resultValue:(nonnull id)resultValue {
    // NSLog(@"iOS call ArkTS method success, methtod name: %@ result: %@",methodName, resultValue);
}

- (void)onError:(nonnull NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(nonnull NSString *)errorMessage {
    // NSLog(@"iOS call ArkTS method fail, methtod name: %@ errorCode: %d errorMessage: %@",methodName, errorCode, errorMessage);
}

- (void)onMethodCancel:(nonnull NSString *)methodName {
    // NSLog(@"iOS call ArkTS method cancel, methtod name: %@", methodName);
}
```



## MethodData类

存储ArkTS侧的方法名称和参数列表，供iOS侧callMethod方法使用。

### 成员函数

#### **initMethodWithName**

```objective-c
(instancetype)initMethodWithName:(NSString* _Nonnull)methodName
                        parameter:(NSArray* _Nullable)parameter;
```

**描述：**

 构造方法 MethodData 对象。

**参数：** 

| Name       | 类型      | 必填 | 描述              |
| ---------- | --------- | ---- | ----------------- |
| methodName | NSString* | 是   | ArkTS的方法名称 |
| parameter  | NSArray*  | 是   | 方法的参数数组  |

**示例：**

接口具体使用方式详见[平台桥接开发指南-iOS](../../tutorial/how-to-use-bridge-on-ios.md#场景四ios侧调用arkui侧的方法)



## BridgeArray类<sup>11+</sup>

采用二进制格式序列化编解码时，iOS想要传递指定类型的数组时，需要将指定类型的数组转化为BridgeArray类，然后通过BridgePlugin的方法传递给ArkTS。

### 类型定义

#### BridgeArrayType<sup>11+</sup>

**描述：** 

枚举类型，采用二进制格式序列化编解码时，传递指定如下指定类型数组时。

| 枚举值                | 值   | **引用方式**           |
| --------------------- | ---- | ---------------------- |
| BridgeArrayTypeBooL   | 1    | 传递bool类型数组       |
| BridgeArrayTypeInt32  | 2    | 传递Int32类型数组      |
| BridgeArrayTypeInt64  | 3    | 传递Int64类型数组      |
| BridgeArrayTypeDouble | 4    | 传递Double类型数组     |
| BridgeArrayTypeString | 5    | 传递NSString类型数组   |


### 属性

#### arrayType<sup>11+</sup>

```objective-c
@property (readonly, nonatomic, assign) BridgeArrayType arrayType;
```

**描述：** 

获取当前传递的数组类型。

**参数：** 

| 类型            | 访问权限 |
| --------------- | -------- |
| BridgeArrayType | 只读     |



#### array<sup>11+</sup>

```objective-c
@property (readonly, nonatomic) NSArray* array;
```

**描述：** 

获取传递原始数组。

**参数：** 

| 类型    | 访问权限 |
| ------- | -------- |
| NSArray | 只读     |



### 成员函数

#### **bridgeArray<sup>11+</sup>**

```objective-c
(instancetype)bridgeArray:(NSArray*)array type:(BridgeArrayType)type;
```

**描述：**

 构造 BridgeArray 对象。

**参数：** 

| Name  | 类型            | 必填 | 描述                                         |
| ----- | --------------- | ---- | -------------------------------------------- |
| array | NSArray*        | 是   | 需要传递的数组（注意必须和枚举类型保持一致） |
| type  | BridgeArrayType | 是   | 传递的数组类型                               |

**返回值：** 

| 类型        | 描述                |
| ----------- | ------------------- |
| BridgeArray | 返回BridgeArray对象 |



**示例：**

当构造BridgePlugin类，是采用二进制格式序列化编解码传输时（详情请参考完整实例）。

```objective-c
// 使用二进制码传递数据
- (void)bridgeUnit_8 {
    // 1. BridgeArrayTypeBooL
    BridgeArray* valueListBool = [BridgeArray bridgeArray:@[[NSNumber numberWithBool:false],[NSNumber numberWithBool:true],[NSNumber numberWithBool:false]] type:BridgeArrayTypeBooL];
    [_plugin sendMessage:valueListBool];
}
```



## TaskOption类<sup>11+</sup>



### 成员函数

#### initTaskOption<sup>11+</sup>

```objective-c
(instancetype)initTaskOption:(BOOL)isSerial;
```

**描述：** 

构造TaskOption对象，设置BridgePlugin 消息通道多线程能力。

**参数：** 

| Name     | 类型 | 必填 | 描述                            |
| -------- | ---- | ---- | ------------------------------- |
| isSerial | BOOL | 是   | true 异步串行，false 异步并行。 |

**返回值：** 

| 类型       | 描述               |
| ---------- | ------------------ |
| TaskOption | 返回TaskOption对象 |

**示例：**

当构造BridgePlugin类，采用队列的构造方法。

```objective-c
// TaskOption设置队列任务类型 true 为异步串行，false 为异步并行
TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];
```

## ErrorCode

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


## 完整示例

[完整示例](../../tutorial/how-to-use-bridge-on-ios.md#场景示例)

