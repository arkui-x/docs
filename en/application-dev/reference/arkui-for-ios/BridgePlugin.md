# BridgePlugin (Platform Bridge)

The **BridgePlugin** module provides APIs for communication between ArkUI and the iOS platform, including data transfer, method invoking, and event invoking. The APIs must be used together with the ArkUI APIs. For details about usage on ArkUI, see [Bridge API](../apis/js-apis-bridge.md).

> **NOTE**
>
> The initial APIs of this module are supported since API version 10. Newly added APIs will be marked with a superscript to indicate their earliest API version.

```objective-c
#import "BridgePlugin.h"
```
## initBridgePlugin

(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName instanceId:(int32_t)instanceId;

@since 10

@deprecated since 11

Creates a **BridgePlugin** instance.

**Parameters**

| Name    | Type  | Mandatory| Description          |
| ---------- | ------ | ---- | -------------- |
| bridgeName | string | Yes  | Bridge name.|
| instanceId | int    | Yes  | Instance ID.      |

**Return value**

| Type        | Description          |
| ------------ | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId];
  ```

(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName	instanceId:(int32_t)instanceId   bridgeType:(BridgeType)type;

@since 10

@deprecated since 11

Creates a **BridgePlugin** instance in codec mode.

**BridgeType**

| Name     | Description                  |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON type.  |
| BINARY_TYPE | Binary type.|

**Parameters**

| Name    | Type      | Mandatory| Description                                |
| ---------- | ---------- | ---- | ------------------------------------ |
| bridgeName | string     | Yes  | Bridge name.                      |
| instanceId | int        | Yes  | Instance ID.                            |
| bridgeType | BridgeType | No  | Codec type. This parameter is optional. The default value is JSON.|

**Return value**

| Type        | Description          |
| ------------ | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" instanceId:self.plugin.instanceId bridgeType: BINARY_TYPE];
  ```

(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName bridgeManager:(BridgePluginManager *)bridgeManager

@since 11

Creates a **BridgePlugin** instance.

**Parameters**

| Name    | Type  | Mandatory| Description          |
| ---------- | ------ | ---- | -------------- |
| bridgeName | string | Yes  | Bridge name.|
| bridgeManager | BridgePluginManager | Yes  | **BridgePluginManager** instance.|

**Return value**

| Type                             | Description          |
| --------------------------------- | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" bridgeManager:[controller getBridgeManager]];
  ```

(instancetype)initBridgePlugin:(NSString* _Nonnull)bridgeName bridgeType:(BridgeType)type bridgeManager:(BridgePluginManager *)bridgeManager;

@since 11

Creates a **BridgePlugin** instance in codec mode.

**BridgeType:**

| Name     | Description                  |
| ----------- | ---------------------- |
| JSON_TYPE   | JSON type.  |
| BINARY_TYPE | Binary type.|

**Parameters**

| Name       | Type               | Mandatory| Description                                |
| ------------- | ------------------- | ---- | ------------------------------------ |
| bridgeName    | string              | Yes  | Bridge name.                      |
| bridgeManager | BridgePluginManager | Yes  | **BridgePluginManager** instance.           |
| bridgeType    | BridgeType          | No  | Codec type. This parameter is optional. The default value is JSON.|

**Return value**

| Type        | Description          |
| ------------ | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```objective-c
StageViewController * controller = [[StageViewController alloc] init];
BridgePlugin * plugin_ = [[BridgePlugin alloc] initBridgePlugin:@"Bridge" bridgeType: BINARY_TYPE bridgeManager:[controller getBridgeManager]];
  ```

## callMethod

(void)callMethod:(MethodData *)method;

Calls the specified ArkUI method.

**Parameters**

| Name    | Type      | Mandatory| Description        |
| ---------- | ---------- | ---- | ------------ |
| methodData | MethodData | Yes  | Method data structure.|

**MethodData**

| Name      | Type    | Description    |
| ---------- | -------- | -------- |
| methodName | NSString | Method name.  |
| Parameters | NSArray  | Method parameters.|

**Return value**

None

**Example**

```objective-c
MethodData * method = [[MethodData alloc] initMethodWithName:@"method" parameter:nil];
[self.plugin callMethod:method];
```

## sendMessage

(void)sendMessage:(id)data;

Sends data to the ArkUI side.

**Parameters**

| Name| Type| Mandatory| Description  |
| ------ | ---- | ---- | ------ |
| data   | id   | Yes  | Data to send.|

**Return value**

None

**Example**

```objective-c
[self.plugin sendMessage:@[@"key",@"value"]];
```

## messageListener

@property(nonatomic, weak) id \<IMessageListener\> messageListener;

Sets a message listener.

| Name          | Type| Mandatory| Description  |
| ---------------- | ---- | ---- | ------ |
| IMessageListener | id   | Yes  | Data to send.|



### **IMessageListener**

@protocol IMessageListener \<NSObject\>

(id)onMessage:(id)data;

(void)onMessageResponse:(id)data;

| IMessageListener | Parameter | Description| Return Value| Description            |
| ------------------ | --------- | -------- | ---------- | -------------------- |
| onMessage          | data : id | Data information.| ID.       | Invoked when the ArkUI side sends a message.    |
| onMessageResponse  | data : id | Data information.| None        | Invoked when the ArkUI side sends a response.|

**Example**

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

Sets a method result listener.

**Parameters**

| Name        | Type| Mandatory| Description              |
| ------------- | ---- | ---- | ------------------ |
| IMethodResult | id   | Yes  | Method result listener.|

### IMethodResult

@protocol IMethodResult \<NSObject\>

(void)onSuccess:(NSString *)methodName resultValue:(id)resultValue;

(void)onError:(NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(NSString *)errorMessage;

(void)onMethodCancel:(NSString *)methodName;

| IMethodResult  | Parameter                                                    | Description                        | Return Value| Description        |
| -------------- | ------------------------------------------------------------ | -------------------------------- | ------ | ---------------- |
| onSuccess      | resultValue: Object                                         | Return value information.                      | None    | Invoked when the method is called successfully.|
| onError        | methodName : NSString<br>errorCode : int<br>errorMessage : NSString | Method name.<br>Error type.<br>Error message.| None    | Invoked when the method fails to be called.|
| onMethodCancel | methodName : NSString                                        | Method name.                          | None    | Invoked when the method call is canceled.|

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


## Sample Code

[Sample Code](../../tutorial/how-to-use-bridge-on-ios.md#example-scenario)
