# BridgePlugin (平台桥接)

本模块提供ArkUI侧和iOS平台消息通信的能力，包括数据传输、方法调用和事件调用。需配套ArkUI侧API使用，ArkUI侧具体用法请参考[Bridge API](../apis/js-apis-bridge.md)。

**起始版本：**

10

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。



## **IMessageListener代理**

用于监听ArkUI侧发送的消息。接口类，由开发者实现并通过messageListener方法设置到BridgePlugin实例中。此接口在**BridgePlugin.h**中声明。



### onMessage

```objective-c
(id)onMessage:(id)data;
```

**描述：**

用于监听ArkUI侧发送信息，即接收ArkUI侧通过sendMessage方法发送的消息。

**参数：**

| Name | 类型 | 必填 | 描述                |
| ---- | ---- | ---- | ------------------- |
| data | id   | 是   | ArkUI侧发送的数据。 |

**返回值：**

| 类型 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| id   | 应答消息数据，即平台侧接收到ArkUI侧的消息数据后，回复ArkUI侧的消息数据。 |



### onMessageResponse

```javascript
(void)onMessageResponse:(id)data;
```

**描述：**

用于监听平台侧调用sendMessage发送消息数据后，ArkUI侧应答的消息数据，即接收ArkUI侧setMessageListener注册的方法的返回值。

**参数：**

| Name | 类型 | 必填 | 描述                                                         |
| ---- | ---- | ---- | ------------------------------------------------------------ |
| data | id   | 是   | 平台侧调用sendMessage发送消息数据后，ArkUI侧的应答消息数据。 |

**返回值：**

无

**示例：**

IMessageListener代理实现。

```objective-c
@interface EntryEntryAbilityViewController ()<IMessageListener, IMethodResult>
@end
```

  ```objective-c

#pragma mark - IMessageListener ArkUI侧发送的消息代理实现 （注意 BridgePlugin 对象 messageListener 属性添加监听者）
  
/** 
    Arkui测调用sendMessage 发送消息到IOS测，将会触发次方法的回调
    @param data Arkui 传递过来的数据
     return 返回值传递传递给Arkui测
*/
- (NSString*)onMessage:(id)data {
//    NSLog(@"Arkui 传递消息给iOS message: %@", data);
    return @"ios onMessage respone message";
}

/**
 * iOS 通过sendMessage 方法发送消息到Arkui, Arkui成功接到消息后回调此方法传递结果
 * @param data Arkui 返回的信息
*/
- (void)onMessageResponse:(id)data {
    //    NSLog(@"Arkui onMessageResponse: %@", data);
}
  ```

## IMethodResult代理

用于监听平台侧调用ArkUI侧方法的执行情况。接口类，由开发者实现并通methodResult方法设置到BridgePlugin实例中。此接口在**BridgePlugin.h**中声明。



### onSuccess

```objective-c
(void)onSuccess:(NSString *)methodName resultValue:(id)resultValue;
```

**描述：**

平台成功调用ArkUI定义的方法时触发该接口，并将ArkUI方法的返回值传递给平台侧。

**参数：**

| Name        | 类型       | 必填 | 描述                  |
| ----------- | ---------- | ---- | --------------------- |
| methodName  | NSString * | 是   | ArkUI侧方法的名称。   |
| resultValue | id         | 是   | ArkUI侧方法的返回值。 |

**返回值：**

无



### onError

```objective-c
(void)onError:(NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(NSString *)errorMessage;
```

**描述：**

平台侧调用ArkUI侧定义方法时，如果出错则触发该接口，并将出错信息返回平台侧。

**参数：**

| Name                        | 类型       | 必填 | 描述              |
| --------------------------- | ---------- | ---- | ----------------- |
| methodName                  | NSString * | 是   | ArkUI侧方法名称。 |
| errorCode    | ErrorCode  | 是   | 错误码。          |
| errorMessage | NSString * | 是   | 错误信息描述。    |

**返回值：**

无



### onMethodCancel

```objective-c
(void)onMethodCancel:(NSString *)methodName;
```

**描述：**

ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。

**参数：**

| Name       | 类型       | 必填 | 描述                      |
| ---------- | ---------- | ---- | ------------------------- |
| methodName | NSString * | 是   | ArkUI侧被注销的事件名称。 |

**返回值：**

无

**示例：** 

IMethodResult代理实现。

  ```objective-c
#pragma mark - IMethodResult 用于监听平台侧调用ArkUI侧注册的方法的执行情况
/**
  *  ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。
  *  @param methodName ArkUI侧方法的名称
  *  @param resultValue ResultValue 类 ，并将ArkUI侧方法的返回值传递给平台侧
*/
- (void)onSuccess:(nonnull NSString *)methodName resultValue:(nonnull id)resultValue {
    //NSLog(@"iOS call arkui method success, methtod name: %@ result: %@",methodName, resultValue);
}

/**
  *  平台侧调用ArkUI侧定义方法时，如果出错则触发该接口，并将出错信息返回平台侧。具体错误码查看ResultValue类中错误码或者接口文档
  *  @param methodName ArkUI侧方法的名称
  *  @param errorCode 错误码
  *  @param errorMessage 错误信息
*/
- (void)onError:(nonnull NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(nonnull NSString *)errorMessage {
    //NSLog(@"iOS call arkui method fail, methtod name: %@ errorCode: %d errorMessage: %@",methodName, errorCode, errorMessage);
}

/**
  *  ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。
  *  @param methodName ArkUI侧方法的名称
*/
- (void)onMethodCancel:(nonnull NSString *)methodName {
    //NSLog(@"iOS call arkui method cancel, methtod name: %@", methodName);
}
  ```

## BridgePlugin类

提供平台间通信能力，开发者需要扩展此类并创建实例，用于ArkUI与原生平台通信。

### 类型定义

#### **BridgeType<sup>11+</sup>**

**描述：** 

枚举类型，指定数据编解码格式。

| 枚举值      | 值   | **引用方式**             |
| ----------- | ---- | ------------------------ |
| JSON_TYPE   | 0    | JSON格式序列化编解码。   |
| BINARY_TYPE | 1    | 二进制格式序列化编解码。 |


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
| BridgePluginManager* | 只读     |



#### bridgeName

```objective-c
@property(nonatomic, strong, readonly) NSString* bridgeName;
```

**描述：** 

获取当前桥接对象实例的名称。

**参数：** 

| 类型      | 访问权限 |
| --------- | -------- |
| NSString* | 只读     |



#### instanceId<sup>(deprecated)</sup>

```objective-c
@property(nonatomic, assign, readonly) int32_t instanceId;
```

**描述：** 

获取当前StageViewController的实例ID。

**参数：** 

| 类型    | 访问权限 |
| ------- | -------- |
| int32_t | 只读     |

**废弃：**

从API version 11开始废弃。



#### messageListener

```objective-c
@property (nonatomic, assign) id<IMessageListener> messageListener;
```

**描述：** 

设置消息代理，用于捕获ArkUI侧发送的消息。

**参数：** 

| 类型                                          | 访问权限 |
| --------------------------------------------- | -------- |
| id\<IMessageListener> | 读写     |



#### methodResult

```objective-c
@property (nonatomic, assign) id<IMethodResult> methodResult;
```

**描述：** 

设置代理，用于捕获平台侧调用ArkUI侧方法的执行结果以及ArkUI侧注销的事件。

**参数：** 

| 类型                                    | 访问权限 |
| --------------------------------------- | -------- |
| id\<IMethodResult> | 读写     |



#### taskOption<sup>11+</sup>

```objective-c
@property(nonatomic, strong, readonly) TaskOption* taskOption;
```

**描述：** 

获取当前桥接对象处理数据的方式。

**参数：** 

| 类型                                      | 访问权限 |
| ----------------------------------------- | -------- |
| TaskOption | 只读     |



#### type<sup>11+</sup>

```objective-c
@property (nonatomic, assign, readonly) BridgeType type;
```

**描述：** 

获取当前桥接对象使用的数据编解码格式。

**参数：** 

| 类型                                    | 访问权限 |
| --------------------------------------- | -------- |
| BridgeType | 只读     |



### 成员函数

#### initBridgePlugin<sup>(deprecated)</sup>

```javascript
(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName instanceId:(int32_t)instanceId;
```

**描述：**

使用instanceId构建平台桥接(BridgePlugin)对象实例，数据编解码格式为JSON_TYPE（默认）。

**参数：** 

| 参数名     | 类型       | 必填 | 说明           |
| ---------- | ---------- | ---- | -------------- |
| bridgeName | NSString * | 是   | 定义桥接名称。 |
| instanceId | int32_t    | 是   | 实例ID。       |

**返回值：** 

| 类型         | 描述               |
| ------------ | ------------------ |
| BridgePlugin | 平台桥接对象实例。 |

**废弃：**

从API version 11开始废弃，建议使用initBridgePlugin<sup>11+</sup>替代。



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

使用BridgePluginManager实例构建平台桥接(BridgePlugin)对象实例，可指定数据编解码格式（默认 JSON_TYPE）和处理数据的方式（默认为异步串行）。

**参数：** 

| 参数名     | 类型   | 必填 | 说明           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | NSString* | 是   | 平台桥接名称 (必须与ArkUI侧一致)。 |
| bridgeManager | BridgePluginManager | 是   | BridgeManager对象管理器，可通过StageViewController的getBridgeManager()方法获取。 |
| bridgeType | BridgeType | 否 | 数据编解码格式，默认为JSON_TYPE。 |
| taskOption | TaskOption | 否 | 处理数据的方式，默认为异步串行。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| BridgePlugin | 平台桥接对象实例。 |

**示例：** 

  ```objective-c
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {
        [self initBridgePlugin];
    }
    return self;
}

//提供了4中构造构造方法开发者根据实际情况选择其中一种构造方式
- (void)InitBridgePlugin {
    int32_t instancedId = [self getInstanceId];
    //构造方法1. 通过instanceId 构造 此方法在API 11 已经废弃
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instancedId];
    
    //构造方法2. 通过bridgeManager 构造
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager]];
 
    //构造方法3. 通过bridgeType、bridgeManager构造，bridgeType设置编解码类型
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE];

    //构造方法4. 通过bridgeType、bridgeManager、taskOption构造
    //TaskOption设置队列任务类型 true 为异步串行，false 为异步并行
    TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE taskOption:taskOption];

    //指定代理
    self.plugin.messageListener = self;
    self.plugin.methodResult = self;
}
  ```



#### callMethod

```objective-c
(void)callMethod:(MethodData *)method;
```

**描述：**

 调用ArkUI侧定义的方法，ArkUI侧方法的返回结果通过 IMethodResult.onSucessess 接口获取。

**参数：** 

| Name       | 类型                        | 必填 | 描述                                    |
| ---------- | --------------------------- | ---- | --------------------------------------- |
| methodData | MethodData | 是   | ArkUI侧方法描述，即方法名称、参数列表。 |

**返回值：** 

无

**示例：**

```objective-c
- (void)bridgeCallMethod {
    //调用Arkui 测注册的方法，调用之前要实现methodResult代理，通过代理回调的到调用结果（注意method name必须与Arkui测方法名保持一致）
    MethodData * method = [[MethodData alloc] initMethodWithName:@"method" parameter:nil];
    [self.plugin callMethod:method];
}
```



#### sendMessage

```objective-c
(void)sendMessage:(id)data;
```

**描述：** 

将数据(data)发送到ArkUI侧，ArkUI侧应答信息通过 IMessageListener.onMessageResponse 接口获取。

**参数：** 

| Name | 类型 | 必填 | 描述     |
| ---- | ---- | ---- | -------- |
| data | id   | 是   | 消息内容 |

> **说明**
>
> [数据类型支持](../../quick-start/platform-bridge-introduction.md#数据类型支持)

**返回值：** 

无

**示例：**

```objective-c
- (void)bridgeSendMessage {
    //发送消息到Arkui，调用之前要实现messageListener代理，通过代理回调的到调用结果
    [self.plugin sendMessage:@[@"key",@"value"]];
}
```



#### isBridgeAvailable<sup>11+</sup>

```objective-c
(BOOL)isBridgeAvailable;
```

**描述：**

 判断当前BridgePluagin是否有效 。

**参数：** 

无

**返回值：** 

| 类型 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| BOOL | 返回true BridgePluagin有效，返回false BridgePluagin失效、不可用。 |



#### **unRegister**

```objective-c
(BOOL)unRegister:(NSString*)bridgeName;
```

**描述：**根据bridgeName取消注册BridgePluagin 。

**参数：** 

| Name       | 类型      | 必填 | 描述           |
| ---------- | --------- | ---- | -------------- |
| bridgeName | NSString* | 是   | 定义桥接名称。 |

**返回值：** 

| 类型 | 描述                                   |
| ---- | -------------------------------------- |
| BOOL | true 取消注册成功，false取消注册失败。 |



## BridgePluginManager类<sup>11+</sup>

构造BridgePlugin 传递的参数， 通过StageViewController 的getBridgeManager方法获取。



## MethodData类

iOS侧调用ArkUI侧方法的参数数据结构。

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
| methodName | NSString* | 是   | ArkUI的方法名称。 |
| parameter  | NSArray*  | 是   | 方法的参数数组。  |

**示例：**

参考callMethodcallmethod使用示例。



## BridgeArray类<sup>11+</sup>

采用二进制格式序列化编解码时，iOS想要传递指定类型的数组时，需要将指定类型的数组转化为BridgeArray类，然后通过BridgePlugin的方法传递给ArkUI。

### 类型定义

#### BridgeArrayType<sup>11+</sup>

**描述：** 

枚举类型，采用二进制格式序列化编解码时，传递指定如下指定类型数组时。

| 枚举值               | 值   | **引用方式**           |
| -------------------- | ---- | ---------------------- |
| BridgeArrayTypeBooL  | 1    | 传递bool类型数组。 |
| BridgeArrayTypeInt32 | 2   | 传递Int32类型数组。 |
| BridgeArrayTypeInt64 | 3   | 传递Int64类型数组。 |
| BridgeArrayTypeDouble | 4   | 传递Double类型数组。 |
| BridgeArrayTypeString | 5  | 传递NSString类型数组。 |


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
//使用二进制码传递数据
- (void)bridgeUnit_8 {
    //1. BridgeArrayTypeBooL
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

当构造BridgePlugin类，采用队列的构造方法（参考initBridgePlugin<sup>11+</sup>示例的构造方法4）。

```objective-c
//TaskOption设置队列任务类型 true 为异步串行，false 为异步并行
TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];

```

## 错误代码

平台桥接的状态返回码。

| errorCode        | errorMessage                  | 描述                               |
| ---------------- | ----------------------------- | ---------------------------------- |
| 0                | "Correct!"                    | 成功。                             |
| 1                | "Bridge name error!"          | Bridge名称错误。                   |
| 2                | "Bridge creation failure!"    | 创建Bridge失败。                   |
| 3                | "Bridge unavailable!"         | Bridge不可用。                     |
| 4                | "Method name error!"          | 没有提供方法名称。                 |
| 5                | "Method is running..."        | 方法正在运行...                    |
| 6                | "Method not implemented!"     | 方法未实现。                       |
| 7                | "Method parameter error!"     | 方法参数错误，如实参与形参不对应。 |
| 8                | "Method already exists!"      | 方法已经被注册了。                 |
| 9                | "Data error"                  | 数据错误。                         |
| 10<sup>11+</sup> | "Bottom Communication error!" | 底层通信错误。                     |
| 11<sup>11+</sup> | "Bridge codec type mismatch"  | Bridge编解码器类型不匹配。         |
| 12<sup>11+</sup> | "Bridge codec is invalid"     | Bridge编解码器无效。               |


```objective-c

/// BridgeClass
#import <UIKit/UIKit.h>
#import <libarkui_ios/BridgePlugin.h>

@interface BridgeClass : BridgePlugin
@end

@implementation BridgeClass
@end


/// EntryEntryAbilityViewController
#import <libarkui_ios/BridgePlugin.h>
#import <libarkui_ios/BridgeArray.h>
#import <libarkui_ios/TaskOption.h>
#import "BridgeClass.h"

@interface EntryEntryAbilityViewController ()<IMessageListener, IMethodResult>
@property (nonatomic, strong) BridgeClass* plugin;

@end

@implementation EntryEntryAbilityViewController
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {
        //初始化plugin
        [self initBridgePlugin];
        //调用Arkui 注册方法
        [self bridgeCallMethod];
        //发送消息到Arkui
        [self bridgeSendMessage];
    }
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
}

- (void)initBridgePlugin {
    int32_t instancedId = [self getInstanceId];
//    构造方法1. 通过instanceId 构造 此方法在API 11 已经废弃
//    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instancedId];
    
//    构造方法2. 通过bridgeManager 构造
      self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager]];
//
//    构造方法3. 通过bridgeType、bridgeManager构造，bridgeType设置编解码类型
//    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:BINARY_TYPE ];
//
//    构造方法4. 通过bridgeType、bridgeManager、taskOption构造
//    TaskOption设置队列任务类型 true 为异步串行，false 为异步并行
//    TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];
//    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE taskOption:taskOption];

//  添加代理
    self.plugin.messageListener = self;
    self.plugin.methodResult = self;
}

- (void)bridgeCallMethod {
    //调用Arkui 测注册的方法，调用之前要实现methodResult代理，通过代理回调的到调用结果 注意method name 必须与Arkui测方法名保持一致
    MethodData * method = [[MethodData alloc] initMethodWithName:@"method" parameter:nil];
    [self.plugin callMethod:method];
}

- (void)bridgeSendMessage {
    //发送消息到Arkui，调用之前要实现messageListener代理，通过代理回调的到调用结果
    [self.plugin sendMessage:@[@"key",@"value"]];
}

//使用二进制码传递数据
- (void)bridgeUnit_8 {
    //1. BridgeArrayTypeBooL
    BridgeArray* valueListBool = [BridgeArray bridgeArray:@[[NSNumber numberWithBool:false],[NSNumber numberWithBool:true],[NSNumber numberWithBool:false]] type:BridgeArrayTypeBooL];
    [_plugin sendMessage:valueListBool];
    
//    //2. BridgeArrayTypeInt32
//    NSArray * listInt32Array = @[@10,@20,@30];
//    BridgeArray* valueListInt32 = [BridgeArray bridgeArray:listInt32Array type:BridgeArrayTypeInt32];
//    [_plugin sendMessage:valueListInt32];
//
//    //3. BridgeArrayTypeInt64
//    NSArray * ListInt64VectorValue = @[@999999999999999999,@888888888888888888,@777777777777777777];
//    BridgeArray* valueListInt64 = [BridgeArray bridgeArray:ListInt64VectorValue type:BridgeArrayTypeInt64];
//    [_plugin sendMessage:valueListInt64];
//
//    //4. BridgeArrayTypeDouble
//    NSArray * ListDoublevectorValue = @[@0.123,@0.456f,@0.789f];
//    BridgeArray* valuedouble = [BridgeArray bridgeArray:ListDoublevectorValue type:BridgeArrayTypeDouble];
//    [_plugin sendMessage:valuedouble];
//
//    //5. BridgeArrayTypeString
//    BridgeArray* valueListString = [BridgeArray bridgeArray:@[@"ios",@"codec",@"code"] type:BridgeArrayTypeString];
//    [_plugin sendMessage:valueListString];
}

#pragma mark - IMessageListener ArkUI侧发送的消息代理实现 （注意 BridgePlugin 对象 messageListener 属性添加监听者）
/**
  *  Arkui测调用sendMessage 发送消息到IOS测，将会触发次方法的回调
  *  @param data Arkui 传递过来的数据
  *   return 返回值传递传递给Arkui测
*/
- (NSString*)onMessage:(id)data {
//    NSLog(@"Arkui 传递消息给iOS message: %@", data);
    return @"ios onMessage respone message";
}

/**
 * iOS 通过sendMessage 方法发送消息到Arkui, Arkui成功接到消息后回调此方法传递结果
 * @param data Arkui 返回的信息
*/
- (void)onMessageResponse:(id)data {
    //NSLog(@"Arkui onMessageResponse: %@", data);
}

#pragma mark - IMethodResult 用于监听平台侧调用ArkUI侧注册的方法的执行情况
/**
  *  ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。
  *  @param methodName ArkUI侧方法的名称
  *  @param resultValue ResultValue 类 ，并将ArkUI侧方法的返回值传递给平台侧
*/
- (void)onSuccess:(nonnull NSString *)methodName resultValue:(nonnull id)resultValue {
    //NSLog(@"iOS call arkui method success, methtod name: %@ result: %@",methodName, resultValue);
}

/**
  *  平台侧调用ArkUI侧定义方法时，如果出错则触发该接口，并将出错信息返回平台侧。具体错误码查看ResultValue类中错误码或者接口文档
  *  @param methodName ArkUI侧方法的名称
  *  @param errorCode 错误码
  *  @param errorMessage 错误信息
*/
- (void)onError:(nonnull NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(nonnull NSString *)errorMessage {
    //NSLog(@"iOS call arkui method fail, methtod name: %@ errorCode: %d errorMessage: %@",methodName, errorCode, errorMessage);
}

/**
  *  ArkUI侧调用unRegisterMethod方法时将触发该接口，用于通知平台侧事件被注销了。
  *  @param methodName ArkUI侧方法的名称
*/
- (void)onMethodCancel:(nonnull NSString *)methodName {
    //NSLog(@"iOS call arkui method cancel, methtod name: %@", methodName);
}

@end

```

## 完整示例

[完整示例](../../tutorial/how-to-use-bridge-on-ios.md#场景示例)

