# BridgePlugin (Platform Bridge)

The **BridgePlugin** module provides APIs for communication between ArkUI and the iOS platform, including data transfer, method invoking, and event invoking. The APIs must be used together with the ArkUI APIs. For details about usage on ArkUI, see [Bridge API](../apis/js-apis-bridge.md).

**Since**

10

> **NOTE**
>
> The initial APIs of this module are supported since API version 10. Newly added APIs will be marked with a superscript to indicate their earliest API version.



## **IMessageListener**

Defines an **IMessageListener** object for listening for messages sent by ArkUI. You need to implement this class and set the object in a **BridgePlugin** instance using the **messageListener** method. This API is declared in the **BridgePlugin.h** file.



### onMessage

```objective-c
(id)onMessage:(id)data;
```

**Description**

Listens for messages sent from the ArkUI side through the **sendMessage** method.

**Parameters**

| Name | Type| Mandatory| Description               |
| ---- | ---- | ---- | ------------------- |
| data | id   | Yes  | Message sent from the ArkUI side.|

**Return value**

| Type| Description                                                        |
| ---- | ------------------------------------------------------------ |
| id   | Response message from the platform to the ArkUI side.|



### onMessageResponse

```javascript
(void)onMessageResponse:(id)data;
```

**Description**

Listens for the response message from the ArkUI side after the platform calls the **sendMessage** API to send a message. In other words, this API is used to receive the return value of the **setMessageListener** method registered on the ArkUI side.

**Parameters**

| Name | Type| Mandatory| Description                                                        |
| ---- | ---- | ---- | ------------------------------------------------------------ |
| data | id   | Yes  | Response message from the ArkUI side after the platform calls the **sendMessage** API.|

**Return value**

None

**Example**

This example is an implementation of **IMessageListener**.

```objective-c
@interface EntryEntryAbilityViewController ()<imessagelistener, IMethodResult>
@end
```

  ```objective-c

#pragma mark - IMessageListener Message listener for messages sent from the ArkUI side (Note that the listener is added to the messageListener attribute of the BridgePlugin object.)
  
/** 
    This callback is triggered if sendMessage is called on ArkUI to send a message to iOS.
    @param data Data sent from ArkUI.
     return Return value passed to ArkUI.
*/
- (NSString*)onMessage:(id)data {
//    NSLog(@"Message sent from ArkUI to iOS: %@", data);
    return @"ios onMessage respone message";
}

/**
 * Call sendMessage on iOS to send a message to ArkUI. After receiving the message, ArkUI returns the result through a callback.
 * @param data Message returned by ArkUI.
*/
- (void)onMessageResponse:(id)data {
    //    NSLog(@"Arkui onMessageResponse: %@", data);
}
  ```

## IMethodResult

Defines an **IMethodResult** object to listen for the execution of an ArkUI method called on the platform. You need to implement this class and set the object in a **BridgePlugin** instance using the **methodResult** method. This API is declared in the **BridgePlugin.h** file.



### onSuccess

```objective-c
(void)onSuccess:(NSString *)methodName resultValue:(id)resultValue;
```

**Description**

Invoked when the specified ArkUI method is called successfully on the platform. This API returns the return value of the ArkUI method to the platform.

**Parameters**

| Name        | Type      | Mandatory| Description                 |
| ----------- | ---------- | ---- | --------------------- |
| methodName  | NSString * | Yes  | Name of an ArkUI method.  |
| resultValue | id         | Yes  | Return value of the ArkUI method.|

**Return value**

None



### onError

```objective-c
(void)onError:(NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(NSString *)errorMessage;
```

**Description**

Invoked when an error occurs in calling the specified ArkUI method on the platform. This API returns the error information to the platform.

**Parameters**

| Name                        | Type      | Mandatory| Description             |
| --------------------------- | ---------- | ---- | ----------------- |
| methodName                  | NSString * | Yes  | Name of an ArkUI method.|
| errorCode    | ErrorCode  | Yes  | Error code.         |
| errorMessage | NSString * | Yes  | Error message.   |

**Return value**

None



### onMethodCancel

```objective-c
(void)onMethodCancel:(NSString *)methodName;
```

**Description**

Invoked when **unRegisterMethod** is called on ArkUI. It is used to notify the platform that an event is deregistered.

**Parameters**

| Name       | Type      | Mandatory| Description                     |
| ---------- | ---------- | ---- | ------------------------- |
| methodName | NSString * | Yes  | Name of the deregistered event on ArkUI.|

**Return value**

None

**Example**

This example is an implementation of **IMethodResult**.

  ```objective-c
#pragma mark - IMethodResult Result of calling an ArkUI API on the platform.
/**
  *  This callback is triggered when unRegisterMethod is called on ArkUI. It is used to notify iOS that an event is deregistered.
  *  @param methodName Name of the ArkUI API.
  * @param resultValue Return value for sending the result of calling the method of the ArkUI API to iOS.
*/
- (void)onSuccess:(nonnull NSString *)methodName resultValue:(nonnull id)resultValue {
    //NSLog(@"iOS call arkui method success, methtod name: %@ result: %@",methodName, resultValue);
}

/**
  * If an error occurs when iOS calls a method registered on ArkUI, this API is triggered to return an error message to iOS. For details about the error code, see the error codes defined in the ResultValue class or the API reference.
  *  @param methodName Name of the ArkUI API.
  *  @param errorCode Error code.
  *  @param errorMessage Error message.
*/
- (void)onError:(nonnull NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(nonnull NSString *)errorMessage {
    //NSLog(@"iOS call arkui method fail, methtod name: %@ errorCode: %d errorMessage: %@",methodName, errorCode, errorMessage);
}

/**
  *  This callback is triggered when unRegisterMethod is called on ArkUI. It is used to notify iOS that an event is deregistered.
  *  @param methodName Name of the ArkUI API.
*/
- (void)onMethodCancel:(nonnull NSString *)methodName {
    //NSLog(@"iOS call arkui method cancel, methtod name: %@", methodName);
}
  ```

## BridgePlugin

Implements inter-platform communication. You need to extend this class and create instances for ArkUI to communicate with the native platform.

### Types

#### **BridgeType<sup>11+</sup>**

**Description**

Enumerates the data encoding and decoding formats.

| Name     | Value  | Description            |
| ----------- | ---- | ------------------------ |
| JSON_TYPE   | 0    | Serialization, encoding, and decoding in JSON format.  |
| BINARY_TYPE | 1    | Serialization, encoding, and decoding in binary format.|


### Attributes



#### bridgeManager<sup>11+</sup>

```objective-c
@property(nonatomic, strong, readonly) BridgePluginManager* bridgeManager;
```

**Description**

Obtains the **bridgeManager** object bound to this **StageViewController**.

**Parameters**

| Type                | Access Permission|
| -------------------- | -------- |
| BridgePluginManager* | Read-only.    |



#### bridgeName

```objective-c
@property(nonatomic, strong, readonly) NSString* bridgeName;
```

**Description**

Obtains the name of this bridge.

**Parameters**

| Type     | Access Permission|
| --------- | -------- |
| NSString* | Read-only.    |



#### instanceId<sup>(deprecated)</sup>

```objective-c
@property(nonatomic, assign, readonly) int32_t instanceId;
```

**Description**

Obtains the instance ID of this **StageViewController**.

**Parameters**

| Type   | Access Permission|
| ------- | -------- |
| int32_t | Read-only.    |

**Deprecated since**

API version 11



#### messageListener

```objective-c
@property (nonatomic, assign) id<IMessageListener> messageListener;
```

**Description**

Sets a message listener to capture messages from the ArkUI side.

**Parameters**

| Type                                         | Access Permission|
| --------------------------------------------- | -------- |
| id<IMessageListener> | Read and write.    |



#### methodResult

```objective-c
@property (nonatomic, assign) id<IMethodResult> methodResult;
```

**Description**

Sets a method result listener to capture the execution result of an ArkUI method on the platform and the deregistered event on the ArkUI side.

**Parameters**

| Type                                   | Access Permission|
| --------------------------------------- | -------- |
| id<IMethodResult> | Read and write.    |



#### taskOption<sup>11+</sup>

```objective-c
@property(nonatomic, strong, readonly) TaskOption* taskOption;
```

**Description**

Obtains the data processing mode of this bridge.

**Parameters**

| Type                                     | Access Permission|
| ----------------------------------------- | -------- |
| TaskOption | Read-only.    |



#### type<sup>11+</sup>

```objective-c
@property (nonatomic, assign, readonly) BridgeType type;
```

**Description**

Obtains the data encoding and decoding format used by this bridge.

**Parameters**

| Type                                   | Access Permission|
| --------------------------------------- | -------- |
| BridgeType | Read-only.    |



### Functions

#### initBridgePlugin<sup>(deprecated)</sup>

```javascript
(instancetype)initBridgePlugin:(NSString *_Nonnull)bridgeName instanceId:(int32_t)instanceId;
```

**Description**

Constructs a **BridgePlugin** instance based on the specified instance ID, with the data encoding and decoding format as **JSON_TYPE** (default).

**Parameters**

| Name    | Type      | Mandatory| Description          |
| ---------- | ---------- | ---- | -------------- |
| bridgeName | NSString * | Yes  | Bridge name.|
| instanceId | int32_t    | Yes  | Instance ID.      |

**Return value**

| Type        | Description              |
| ------------ | ------------------ |
| BridgePlugin | **BridgePlugin** instance.|

**Deprecated since**

API version 11. You are advised to use **initBridgePlugin<sup>11+</sup>** instead.



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

**Description**

Constructs a **BridgePlugin** instance based on a **BridgePluginManager** instance. You can specify the data encoding and decoding format (**JSON_TYPE** by default) and data processing mode (asynchronous serial by default).

**Parameters**

| Name    | Type  | Mandatory| Description          |
| ---------- | ------ | ---- | -------------- |
| bridgeName | NSString* | Yes  | Bridge name, which must be the same as that on ArkUI.|
| bridgeManager | BridgePluginManager | Yes  | **BridgeManager** object, which can be obtained using the **getBridgeManager()** method of **StageViewController**.|
| bridgeType | BridgeType | No| Data encoding and decoding format. The default value is **JSON_TYPE**.|
| taskOption | TaskOption | No| Data processing mode. The default mode is asynchronous serial.|

**Return value**

| Type                             | Description          |
| --------------------------------- | -------------- |
| BridgePlugin | **BridgePlugin** instance.|

**Example**

  ```objective-c
- (instancetype)initWithInstanceName:(NSString *)instanceName {
    self = [super initWithInstanceName:instanceName];
    if (self) {
        [self initBridgePlugin];
    }
    return self;
}

// Four construction methods are provided. Choose one that best suits your need.
- (void)InitBridgePlugin {
    int32_t instancedId = [self getInstanceId];
    // Method 1: Use instanceId for construction. This method has been deprecated since API version 11.
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instancedId];
    
    // Method 2: Use bridgeManager for construction.
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager]];
 
    // Method 3: Use bridgeType and bridgeManager for construction. You can set bridgeType as required.
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE];

    // Method 4: Use bridgeType, bridgeManager, and taskOption for construction.
    // TaskOption sets the data processing mode. true means asynchronous serial, and false means asynchronous parallel.
    TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];
    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE taskOption:taskOption];

    // Specify a messageListener and methodResult.
    self.plugin.messageListener = self;
    self.plugin.methodResult = self;
}
  ```



#### callMethod

```objective-c
(void)callMethod:(MethodData *)method;
```

**Description**

Calls an ArkUI method. The return result of the method is obtained through the **IMethodResult.onSucessess** API.

**Parameters**

| Name       | Type                       | Mandatory| Description                                   |
| ---------- | --------------------------- | ---- | --------------------------------------- |
| methodData | MethodData | Yes  | Information about the ArkUI method, including the method name and parameter list.|

**Return value**

None

**Example**

```objective-c
- (void)bridgeCallMethod {
    // Call the registered ArkUI method. Before this, implement the methodResult API to return the call result through a callback. Note that the method name must be the same as that of the ArkUI method.
    MethodData * method = [[MethodData alloc] initMethodWithName:@"method" parameter:nil];
    [self.plugin callMethod:method];
}
```



#### sendMessage

```objective-c
(void)sendMessage:(id)data;
```

**Description**

Sends a message to the ArkUI side. The response from the ArkUI side is obtained through the **IMessageListener.onMessageResponse** API.

**Parameters**

| Name | Type| Mandatory| Description    |
| ---- | ---- | ---- | -------- |
| data | id   | Yes  | Message content.|

**Return value**

None

**Example**

```objective-c
- (void)bridgeSendMessage {
    // Send a message to the ArkUI side. Before this, implement the messageListener API to obtain the response through a callback.
    [self.plugin sendMessage:@[@"key",@"value"]];
}
```



#### isBridgeAvailable<sup>11+</sup>

```objective-c
(BOOL)isBridgeAvailable;
```

**Description**

Checks whether this **BridgePlugin** instance is available.

**Parameters**

None

**Return value**

| Type| Description                                                        |
| ---- | ------------------------------------------------------------ |
| BOOL | Returns **true** if the **BridgePlugin** instance is available; returns **false** otherwise.|



#### **unRegister**

```objective-c
(BOOL)unRegister:(NSString*)bridgeName;
```

**Description**

Deregisters a **BridgePlugin** instance based on **bridgeName**.

**Parameters**

| Name       | Type     | Mandatory| Description          |
| ---------- | --------- | ---- | -------------- |
| bridgeName | NSString* | Yes  | Bridge name.|

**Return value**

| Type| Description                                  |
| ---- | -------------------------------------- |
| BOOL | Returns **true** if the deregistration is successful; returns **false** otherwise.|



## BridgePluginManager<sup>11+</sup>

Implements a bridge plugin manager, for which parameters can be obtained through the **getBridgeManager** API of **StageViewController**.



## MethodData

Defines the data structure for invoking an ArkUI API on iOS.

### Functions

#### **initMethodWithName**

```objective-c
(instancetype)initMethodWithName:(NSString* _Nonnull)methodName
                        parameter:(NSArray* _Nullable)parameter;
```

**Description**

 Constructs a **MethodData** object.

**Parameters**

| Name       | Type     | Mandatory| Description             |
| ---------- | --------- | ---- | ----------------- |
| methodName | NSString* | Yes  | Name of an ArkUI method.|
| parameter  | NSArray*  | Yes  | Parameter array of the method. |

**Example**

For details, see the example for **callMethodcallmethod**.



## BridgeArray Class<sup>11+</sup>

When the binary format is used for serialization, encoding, and decoding, an array to be transferred from iOS to ArkUI must be converted into the BridgeArray class type before being transferred through the **BridgePlugin** API.

### Types

#### BridgeArrayType<sup>11+</sup>

**Description**

Enumerates the types of arrays that can be transferred when the binary format is used for serialization, encoding, and decoding.

| Name              | Value  | Description          |
| -------------------- | ---- | ---------------------- |
| BridgeArrayTypeBooL  | 1    | Array of the Boolean type.|
| BridgeArrayTypeInt32 | 2   | Array of the Int32 type.|
| BridgeArrayTypeInt64 | 3   | Array of the Int64 type.|
| BridgeArrayTypeDouble | 4   | Array of the Double type.|
| BridgeArrayTypeString | 5  | Array of the NSString type.|


### Attributes

#### arrayType<sup>11+</sup>

```objective-c
@property (readonly, nonatomic, assign) BridgeArrayType arrayType;
```

**Description**

Obtains the type of the transferred array.

**Parameters**

| Type           | Access Permission|
| --------------- | -------- |
| BridgeArrayType | Read-only.    |



#### array<sup>11+</sup>

```objective-c
@property (readonly, nonatomic) NSArray* array;
```

**Description**

Obtains the original array transferred.

**Parameters**

| Type   | Access Permission|
| ------- | -------- |
| NSArray | Read-only.    |



### Functions

#### **bridgeArray<sup>11+</sup>**

```objective-c
(instancetype)bridgeArray:(NSArray*)array type:(BridgeArrayType)type;
```

**Description**

 Constructs a **BridgeArray** object.

**Parameters**

| Name  | Type           | Mandatory| Description                                        |
| ----- | --------------- | ---- | -------------------------------------------- |
| array | NSArray*        | Yes  | Array to be transferred (Note that its type must be the same as the specified type.)|
| type  | BridgeArrayType | Yes  | Type of the transferred array.                              |

**Return value**

| Type       | Description               |
| ----------- | ------------------- |
| BridgeArray | **BridgeArray** object.|



**Example**

In this example, a **BridgePlugin** instance is constructed, with the binary format used for serialization, encoding, and decoding. For the complete code, see **Sample**.

```objective-c
// Transfer data in binary format.
- (void)bridgeUnit_8 {
    // 1. BridgeArrayTypeBooL
    BridgeArray* valueListBool = [BridgeArray bridgeArray:@[[NSNumber numberWithBool:false],[NSNumber numberWithBool:true],[NSNumber numberWithBool:false]] type:BridgeArrayTypeBooL];
    [_plugin sendMessage:valueListBool];
}
```



## TaskOption Class <sup>11+</sup>



### Functions

#### initTaskOption<sup>11+</sup>

```objective-c
(instancetype)initTaskOption:(BOOL)isSerial;
```

**Description**

Defines a **TaskOption** object to set the multi-thread capability for **BridgePlugin**.

**Parameters**

| Name     | Type| Mandatory| Description                           |
| -------- | ---- | ---- | ------------------------------- |
| isSerial | BOOL | Yes  | Whether data processing is in asynchronous serial mode. The value **true** means asynchronous serial, and **false** asynchronous parallel.|

**Return value**

| Type      | Description              |
| ---------- | ------------------ |
| TaskOption | **TaskOption** object.|

**Example**

In this example, a **BridgePlugin** instance is constructed with **taskOption**. For details, see construction method 4 in the example for **initBridgePlugin<sup>11+</sup>**.

```objective-c
// TaskOption sets the data processing mode. true means asynchronous serial, and false means asynchronous parallel.
TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];

```

## Error Codes

The table below lists the status codes of the platform bridge.

| Error Code       | Error Message                 | Description                                                  |
| ---------------- | ----------------------------- | ------------------------------------------------------------ |
| 0                | "Correct!"                    | Success.                                                     |
| 1                | "Bridge name error!"          | The bridge name is incorrect.                                |
| 2                | "Bridge creation failure!"    | Failed to create the bridge.                                 |
| 3                | "Bridge unavailable!"         | The bridge is unavailable.                                   |
| 4                | "Method name error!"          | No method name is provided.                                  |
| 5                | "Method is running..."        | The method is running...                                     |
| 6                | "Method not implemented!"     | Method not yet implemented.                                  |
| 7                | "Method parameter error!"     | Parameter error. For example, the argument and parameter do not match. |
| 8                | "Method already exists!"      | The method has already been registered.                      |
| 9                | "Data error"                  | Data error.                                                  |
| 10<sup>11+</sup> | "Bottom Communication error!" | Bottom-layer communication error.                            |
| 11<sup>11+</sup> | "Bridge codec type mismatch"  | Bridge codec type mismatch.                                  |
| 12<sup>11+</sup> | "Bridge codec is invalid"     | Invalid bridge codec.                                        |


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
        // Initialize the plug-in.
        [self initBridgePlugin];
        // Call the registered ArkUI method.
        [self bridgeCallMethod];
        // Send a message to ArkUI.
        [self bridgeSendMessage];
    }
    return self;
}

- (void)viewDidLoad {
    [super viewDidLoad];
}

- (void)initBridgePlugin {
    int32_t instancedId = [self getInstanceId];
// Method 1: Use instanceId for construction. This method has been deprecated since API version 11.
//    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" instanceId:instancedId];
    
// Method 2: Use bridgeManager for construction.
      self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager]];
//
// Method 3: Use bridgeType and bridgeManager for construction. You can set bridgeType as required.
//    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:BINARY_TYPE ];
//
// Method 4: Use bridgeType, bridgeManager, and taskOption for construction.
//    TaskOption sets the data processing mode. true means asynchronous serial, and false means asynchronous parallel.
//    TaskOption * taskOption = [[TaskOption alloc] initTaskOption:true];
//    self.plugin = [[BridgeClass alloc] initBridgePlugin:@"Bridge" bridgeManager:[self getBridgeManager] bridgeType:JSON_TYPE taskOption:taskOption];

// Add a delegate.
    self.plugin.messageListener = self;
    self.plugin.methodResult = self;
}

- (void)bridgeCallMethod {
    // Call the registered ArkUI method. Before this, implement the methodResult API to return the call result through a callback. Note that the method name must be the same as that of the ArkUI method.
    MethodData * method = [[MethodData alloc] initMethodWithName:@"method" parameter:nil];
    [self.plugin callMethod:method];
}

- (void)bridgeSendMessage {
    // Send a message to the ArkUI side. Before this, implement the messageListener API to obtain the response through a callback.
    [self.plugin sendMessage:@[@"key",@"value"]];
}

// Transfer data in binary format.
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

#pragma mark - IMessageListener Message listener for messages sent from the ArkUI side (Note that the listener is added to the messageListener attribute of the BridgePlugin object.)
/**
  * This callback is triggered if sendMessage is called on ArkUI to send a message to iOS.
  *  @param data Data sent from ArkUI.
  *   return Return value passed to ArkUI.
*/
- (NSString*)onMessage:(id)data {
//    NSLog(@"Message sent from ArkUI to iOS: %@", data);
    return @"ios onMessage respone message";
}

/**
 * Call sendMessage on iOS to send a message to ArkUI. After receiving the message, ArkUI returns the result through a callback.
 * @param data Message returned by ArkUI.
*/
- (void)onMessageResponse:(id)data {
    //NSLog(@"Arkui onMessageResponse: %@", data);
}

#pragma mark - IMethodResult Result of calling the registered ArkUI method on iOS.
/**
  *  This callback is triggered when unRegisterMethod is called on ArkUI. It is used to notify iOS that an event is deregistered.
  *  @param methodName Name of the ArkUI method.
  *  @param resultValue Return value for sending the result of calling the ArkUI method to iOS.
*/
- (void)onSuccess:(nonnull NSString *)methodName resultValue:(nonnull id)resultValue {
    //NSLog(@"iOS call arkui method success, methtod name: %@ result: %@",methodName, resultValue);
}

/**
  * If an error occurs when iOS calls a method registered on ArkUI, this API is triggered to return an error message to iOS. For details about the error code, see the error codes defined in the ResultValue class or the API reference.
  *  @param methodName Name of the ArkUI method.
  *  @param errorCode Error code.
  *  @param errorMessage Error message.
*/
- (void)onError:(nonnull NSString *)methodName errorCode:(ErrorCode)errorCode errorMessage:(nonnull NSString *)errorMessage {
    //NSLog(@"iOS call arkui method fail, methtod name: %@ errorCode: %d errorMessage: %@",methodName, errorCode, errorMessage);
}

/**
  *  This callback is triggered when unRegisterMethod is called on ArkUI. It is used to notify iOS that an event is deregistered.
  *  @param methodName Name of the ArkUI method.
*/
- (void)onMethodCancel:(nonnull NSString *)methodName {
    //NSLog(@"iOS call arkui method cancel, methtod name: %@", methodName);
}

@end

```

## Sample

[Example Scenario](../../tutorial/how-to-use-bridge-on-ios.md#example-scenario)
