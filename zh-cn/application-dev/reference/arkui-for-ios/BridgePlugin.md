# BridgePlugin (平台桥接)

本模块提供ArkUI端和iOS平台端消息通信的功能，包括数据传输、方法调用和事件调用。需配套ArkUI端API使用，ArkUI侧具体用法请参考[Bridge API](../apis/js-apis-bridge.md)。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

```objective-c
#import "BridgePlugin.h"
```
## initBridgePlugin

(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName instanceId:(int32_t)instanceId;

@since 10

@deprecated since 11

创建BridgePlugin类。

**参数：** 

| 参数名     | 类型   | 必填 | 说明           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | string | 是   | 定义桥接名称。 |
| instanceId | int    | 是   | 实例ID。       |

**返回值：** 

| 类型         | 说明           |
| ------------ | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId];
  ```

(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName	instanceId:(int32_t)instanceId   bridgeType:(BridgeType)type;

@since 10

@deprecated since 11

创建BridgePlugin类(编解码模式)。

**BridgeType:**

| 参数名      | 说明                   |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON格式序列化编解码   |
| BINARY_TYPE | 二进制格式序列化编解码 |

**参数：** 

| 参数名     | 类型       | 必填 | 说明                                 |
| ---------- | ---------- | ---- | ------------------------------------ |
| bridgeName | string     | 是   | 定义桥接名称。                       |
| instanceId | int        | 是   | 实例ID。                             |
| bridgeType | BridgeType | 否   | 编解码类型（可不填，默认为json格式） |

**返回值：** 

| 类型         | 说明           |
| ------------ | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId bridgeType：BINARY_TYPE];
  ```

(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName bridgeManager:(BridgePluginManager *)bridgeManager

@since 11

创建BridgePlugin类。

**参数：** 

| 参数名     | 类型   | 必填 | 说明           |
| ---------- | ------ | ---- | -------------- |
| bridgeName | string | 是   | 定义桥接名称。 |
| bridgeManager | BridgePluginManager | 是   | 实例BridgePluginManager。 |

**返回值：** 

| 类型                              | 说明           |
| --------------------------------- | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" bridgeManager:[controller getBridgeManager]];
  ```

(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName bridgeType:(BridgeType)type bridgeManager:(BridgePluginManager *)bridgeManager;

@since 11

创建BridgePlugin类(编解码模式)。

**BridgeType:**

| 参数名      | 说明                   |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON格式序列化编解码   |
| BINARY_TYPE | 二进制格式序列化编解码 |

**参数：** 

| 参数名        | 类型                | 必填 | 说明                                 |
| ------------- | ------------------- | ---- | ------------------------------------ |
| bridgeName    | string              | 是   | 定义桥接名称。                       |
| bridgeManager | BridgePluginManager | 是   | 实例BridgePluginManager。            |
| bridgeType    | BridgeType          | 否   | 编解码类型（可不填，默认为json格式） |

**返回值：** 

| 类型         | 说明           |
| ------------ | -------------- |
| BridgePlugin | 桥接结果接口。 |

**示例：** 

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" bridgeType：BINARY_TYPE bridgeManager:[controller getBridgeManager]];
  ```

## callMethod

(void)callMethod:(MethodData *)method;

调用ArkUI端的方法。

**参数：** 

| 参数名     | 类型       | 必填 | 说明         |
| ---------- | ---------- | ---- | ------------ |
| methodData | MethodData | 是   | 方法数据结构。 |

**MethodData结构**

| 名称       | 类型     | 说明     |
| ---------- | -------- | -------- |
| methodName | NSString | 方法名。   |
| Parameters | NSArray  | 方法参数。 |

**返回值：** 

无

**示例：**

```objective-c
MethodData * method = [[MethodData alloc] initMethodWithName:@"method" parameter:nil];
[self.plugin callMethod:method];
```

## sendMessage

(void)sendMessage:(id)data;

向ArkUI端发送数据。

**参数：** 

| 参数名 | 类型 | 必填 | 说明   |
| ------ | ---- | ---- | ------ |
| data   | id   | 是   | 数据。 |

**返回值：** 无

**示例：**

```objective-c
[self.plugin sendMessage:@[@"key",@"value"]];
```

## messageListener

@property(nonatomic, weak) id \<IMessageListener\> messageListener;

注册消息监听。

| 参数名           | 类型 | 必填 | 说明   |
| ---------------- | ---- | ---- | ------ |
| IMessageListener | id   | 是   | 数据。 |



### **IMessageListener**

@protocol IMessageListener \<NSObject\>

(id)onMessage:(id)data;

(void)onMessageResponse:(id)data;

| IMessageListener | 参数  | 参数描述 | 返回值 | 说明             |
| ------------------ | --------- | -------- | ---------- | -------------------- |
| onMessage          | data : id | 数据信息。 | id         | 等待ArkUI端发送信息。     |
| onMessageResponse  | data : id | 数据信息。 | 无         | 等待ArkUI端发送信息应答。 |

**示例：**

```objective-c
<IMethodResult,IMessageListener> {
    UIView * _boardView;
}

#pragma mark - listener
- (id)onMessage:(id)data {
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onMessage : \n %@",data]];
    return @{@"response first":@"response second"};
}

- (void)onMessageResponse:(id)data {
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onMessageResponse : \n %@",data]];
}
```

## methodResult

@property(nonatomic, weak) id \<IMethodResult\> methodResult;

注册方法返回监听

**参数：** 

| 参数名         | 类型 | 必填 | 说明               |
| ------------- | ---- | ---- | ------------------ |
| IMethodResult | id   | 是   | 方法返回监听接口类。 |

### IMethodResult

@protocol IMethodResult \<NSObject\>

(void)onSuccess:(NSString *)methodName resultValue:(id)resultValue;

(void)onError:(NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(NSString *)errorMessage;

(void)onMethodCancel:(NSString *)methodName;

| IMethodResult  | 参数                                                     | 参数描述                         | 返回值 | 说明         |
| -------------- | ------------------------------------------------------------ | -------------------------------- | ------ | ---------------- |
| onSuccess      | resultValue：Object                                          | 返回值信息。                       | 无     | 调用方法返回成功。 |
| onError        | methodName : NSString<br/>errorCode : int<br/>errorMessage : NSString | 方法名。<br/>错误类型。<br/>错误信息。 | 无     | 调用方法返回失败。 |
| onMethodCancel | methodName : NSString                                        | 方法名。                           | 无     | 监听取消方法注册。 |

```objective-c
- (void)onSuccess:(NSString *)methodName
      resultValue:(id)resultValue {
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onSuccess:resultValue : \n %@",resultValue]];
}

- (void)onError:(NSString *)methodName
      errorCode:(ErrorCode)errorCode
   errorMessage:(NSString *)errorMessage {

    NSLog(@"%s methodName %@ ,errorCode %d, errorMessage %@",__func__,methodName,errorCode,errorMessage);
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onError : %@ \n errorCode %d \n errorMessage%@",methodName,errorCode,errorMessage]];
}

- (void)onMethodCancel:(NSString *)methodName {
    NSLog(@"%s methodName %@ ",__func__,methodName);
    [TestAlert showAlertMessage:[NSString stringWithFormat:@"onMethodCancel"]];
}
```


## 完整示例

[完整示例](../../tutorial/how-to-use-bridge-on-ios.md#场景示例)