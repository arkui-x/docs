# @ohos.app.ability.UIAbility (UIAbility)

UIAbility is an application component that has the UI. The **UIAbility** module provides lifecycle callback such as component creation, destruction, and foreground/background switching. It also provides the following capabilities related to component collaboration:

- [Caller](#caller): an object returned by **startAbilityByCall**. The CallerAbility (caller) uses this object to communicate with the CalleeAbility (callee).
- [Callee](#callee): an internal object of UIAbility. The CalleeAbility (callee) uses this object to communicate with the CallerAbility (caller).

> **NOTE**
> 
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version. 
> The APIs of this module can be used only in the stage model.

## Modules to Import

```ts
import UIAbility from '@ohos.app.ability.UIAbility';
```

## Attributes

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| context | UIAbilityContext | Yes| No| Context of the UIAbility.|
| launchWant | Want | Yes| No| Parameters for starting the UIAbility.|
| lastRequestWant | Want | Yes| No| Parameters used when the UIAbility was started last time.|
| callee | [Callee](#callee) | Yes| No| Object that invokes the stub service.|

## UIAbility.onCreate

onCreate(want: Want, param: AbilityConstant.LaunchParam): void;

Called to initialize the service logic when a UIAbility is created.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| want | [Want](js-apis-app-ability-want.md) | Yes| Information related to this UIAbility, including the ability name and bundle name.|
| param | AbilityConstant.LaunchParam] | Yes| Parameters for starting the UIAbility, and the reason for the last abnormal exit.|

**Example**

  ```ts
  class MyUIAbility extends UIAbility {
      onCreate(want, param) {
          console.log('onCreate, want: ${want.abilityName}');
      }
  }
  ```


## UIAbility.onWindowStageCreate

onWindowStageCreate(windowStage: window.WindowStage): void

Called when a **WindowStage** is created for this UIAbility.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| windowStage | window.WindowStage | Yes| **WindowStage** information.|

**Example**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onWindowStageCreate(windowStage) {
          console.log('onWindowStageCreate');
      }
  }
  ```


## UIAbility.onWindowStageDestroy

onWindowStageDestroy(): void

Called when the **WindowStage** is destroyed for this UIAbility.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Example**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onWindowStageDestroy() {
          console.log('onWindowStageDestroy');
      }
  }
  ```


## UIAbility.onWindowStageRestore

onWindowStageRestore(windowStage: window.WindowStage): void

Called when the **WindowStage** is restored during the migration of this UIAbility, which is a multi-instance ability.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| windowStage | window.WindowStage | Yes| **WindowStage** information.|

**Example**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onWindowStageRestore(windowStage) {
          console.log('onWindowStageRestore');
      }
  }
  ```


## UIAbility.onDestroy

onDestroy(): void | Promise&lt;void&gt;;

Called when this UIAbility is destroyed to clear resources.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Example**
    

  ```ts
  class MyUIAbility extends UIAbility {
      onDestroy() {
          console.log('onDestroy');
      }
  }
  ```

After the **onDestroy** lifecycle callback is executed, the application may exit. As a result, the asynchronous function in **onDestroy** may fail to be executed correctly, for example, asynchronously writing data to the database. The asynchronous lifecycle can be used to ensure that the subsequent lifecycle continues after the asynchronous **onDestroy** is complete.

  ```ts
class MyUIAbility extends UIAbility {
    async onDestroy() {
        console.log('onDestroy');
        // Call the asynchronous function.
    }
}
  ```

## UIAbility.onForeground

onForeground(): void;

Called when this UIAbility is switched from the background to the foreground.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Example**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onForeground() {
          console.log('onForeground');
      }
  }
  ```


## UIAbility.onBackground

onBackground(): void;

Called when this UIAbility is switched from the foreground to the background.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Example**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onBackground() {
          console.log('onBackground');
      }
  }
  ```


## UIAbility.onContinue

onContinue(wantParam: { [key: string]: Object }): AbilityConstant.OnContinueResult;

Called to save data during the ability migration preparation process.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| wantParam | {[key:&nbsp;string]:&nbsp;any} | Yes| **want** parameter.|

**Return value**

| Type| Description|
| -------- | -------- |
| AbilityConstant.OnContinueResult | Continuation result.|

**Example**
    
  ```ts
  import AbilityConstant from '@ohos.app.ability.AbilityConstant';
  class MyUIAbility extends UIAbility {
      onContinue(wantParams) {
          console.log('onContinue');
          wantParams['myData'] = 'my1234567';
          return AbilityConstant.OnContinueResult.AGREE;
      }
  }
  ```


## UIAbility.onNewWant

onNewWant(want: Want, launchParams: AbilityConstant.LaunchParam): void;

Called when a new Want is passed in and this UIAbility is started again.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| want | Want | Yes| Want information, such as the ability name and bundle name.|
| launchParams | AbilityConstant.LaunchParam | Yes| Reason for the UIAbility startup and the last abnormal exit.|

**Example**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onNewWant(want, launchParams) {
          console.log('onNewWant, want: ${want.abilityName}');
          console.log('onNewWant, launchParams: ${JSON.stringify(launchParams)}');
      }
  }
  ```

## UIAbility.onDump

onDump(params: Array\<string>): Array\<string>;

Dumps client information.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| params | Array\<string> | Yes| Parameters in the form of a command.|

**Example**
    
  ```ts
  class MyUIAbility extends UIAbility {
      onDump(params) {
          console.log('dump, params: ${JSON.stringify(params)}');
          return ['params'];
      }
  }
  ```


## UIAbility.onSaveState

onSaveState(reason: AbilityConstant.StateType, wantParam : {[key: string]: Object}): AbilityConstant.OnSaveResult;

Called when the framework automatically saves the UIAbility state in the case of an application fault. This API is used together with **appRecovery**. If automatic state saving is enabled, **onSaveState** is called to save the state of this UIAbility.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| reason | AbilityConstant.StateType | Yes| Reason for triggering the callback to save the UIAbility state.|
| wantParam | {[key:&nbsp;string]:&nbsp;any} | Yes| **want** parameter.|

**Return value**

| Type| Description|
| -------- | -------- |
| AbilityConstant.OnSaveResult | Whether the UIAbility state is saved.|

**Example**

  ```ts
import AbilityConstant from '@ohos.app.ability.AbilityConstant';

class MyUIAbility extends UIAbility {
    onSaveState(reason, wantParam) {
        console.log('onSaveState');
        wantParam['myData'] = 'my1234567';
        return AbilityConstant.OnSaveResult.RECOVERY_AGREE;
    }
}
  ```

## UIAbility.onShare<sup>10+</sup>

onShare(wantParam:{ [key: string]: Object }): void;

Called when this UIAbility sets data to share. **ohos.extra.param.key.contentTitle** indicates the title of the content to share in the sharing box, **ohos.extra.param.key.shareAbstract** provides an abstract description of the content, and **ohos.extra.param.key.shareUrl** indicates the online address of the service. You need to set these three items as objects, with the key set to **title**, **abstract**, and **url**, respectively.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| wantParam | {[key:&nbsp;string]:&nbsp;Object} | Yes| **want** parameter.|

**Example**
    
  ```ts
import AbilityConstant from '@ohos.app.ability.AbilityConstant';
class MyUIAbility extends UIAbility {
    onShare(wantParams) {
        console.log('onShare');
        wantParams['ohos.extra.param.key.contentTitle'] = {title: "OA"};
        wantParams['ohos.extra.param.key.shareAbstract'] = {abstract: "communication for company employee"};
        wantParams['ohos.extra.param.key.shareUrl'] = {url: "oa.example.com"};
    }
}
  ```

## UIAbility.onPrepareToTerminate<sup>10+</sup>

onPrepareToTerminate(): boolean;

Triggered when this UIAbility is about to terminate in case that the system parameter **persist.sys.prepare_terminate** is set to **true**. You can define an operation in this callback to determine whether to continue terminating the UIAbility. If a confirmation from the user is required, you can define a pre-termination operation in the callback and use it together with **terminateSelf**, for example, displaying a dialog box to ask the user whether to terminate the UIAbility. The UIAbility termination process is canceled when **persist.sys.prepare_terminate** is set to **true**.

**Required permissions**: ohos.permission.PREPARE_APP_TERMINATE

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Return value**

| Type| Description|
| -- | -- |
| boolean | Whether to terminate the UIAbility. The value **true** means that the termination process is canceled and the UIAbility is not terminated. The value **false** means to continue terminating the UIAbility.|

**Example**

  ```ts
  export default class EntryAbility extends UIAbility {
    onPrepareToTerminate() {
      // Define a pre-termination operation,
      // for example, starting another UIAbility and performing asynchronous termination based on the startup result.
      let want:Want = {
        bundleName: "com.example.myapplication",
        moduleName: "entry",
        abilityName: "SecondAbility"
      }
      this.context.startAbilityForResult(want)
        .then((result)=>{
          // Obtain the startup result and terminate the current UIAbility when resultCode in the return value is 0.
          console.log('startAbilityForResult success, resultCode is ' + result.resultCode);
          if (result.resultCode === 0) {
            this.context.terminateSelf();
          }
        }).catch((err)=>{
          // Exception handling.
          console.log('startAbilityForResult failed, err:' + JSON.stringify(err));
          this.context.terminateSelf();
        })

      return true; // The pre-termination operation is defined. The value true means that the UIAbility termination process is canceled.
    }
  }
  ```

## Caller

Implements sending of sequenceable data to the target ability when the CallerAbility invokes the target ability (CalleeAbility).

## Caller.call

call(method: string, data: rpc.Parcelable): Promise&lt;void&gt;;

Sends sequenceable data to the target ability.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| method | string | Yes| Notification message string negotiated between the two abilities. The message is used to instruct the callee to register a function to receive the sequenceable data.|
| data | rpc.Parcelable | Yes| Parcelable data. You need to customize the data.|

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;void&gt; | Promise used to return a response.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16200001 | Caller released. The caller has been released. |
| 16200002 | Callee invalid. The callee does not exist. |
| 16000050 | Internal error. |

**Example**    

  ```ts
  class MyMessageAble{ // Custom parcelable data structure.
    name:''
    str:''
    num: 1
    constructor(name, str) {
      this.name = name;
      this.str = str;
    }
    marshalling(messageSequence) {
      messageSequence.writeInt(this.num);
      messageSequence.writeString(this.str);
      console.log('MyMessageAble marshalling num[${this.num}] str[${this.str}]');
      return true;
    }
    unmarshalling(messageSequence) {
      this.num = messageSequence.readInt();
      this.str = messageSequence.readString();
      console.log('MyMessageAble unmarshalling num[${this.num}] str[${this.str}]');
      return true;
    }
  };
  let method = 'call_Function'; // Notification message string negotiated by the two abilities.
  let caller;
  export default class MainUIAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
      this.context.startAbilityByCall({
        bundleName: 'com.example.myservice',
        abilityName: 'MainUIAbility',
        deviceId: ''
      }).then((obj) => {
        caller = obj;
        let msg = new MyMessageAble('msg', 'world'); // See the definition of Parcelable.
        caller.call(method, msg)
          .then(() => {
            console.log('Caller call() called');
          })
          .catch((callErr) => {
            console.log('Caller.call catch error, error.code: ${callErr.code}, error.message: ${callErr.message}');
          });
      }).catch((err) => {
        console.log('Caller GetCaller error, error.code: ${err.code}, error.message: ${err.message}');
      });
    }
  }
  ```


## Caller.callWithResult

callWithResult(method: string, data: rpc.Parcelable): Promise&lt;rpc.MessageSequence&gt;;

Sends sequenceable data to the target ability and obtains the sequenceable data returned by the target ability.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| method | string | Yes| Notification message string negotiated between the two abilities. The message is used to instruct the callee to register a function to receive the sequenceable data.|
| data | rpc.Parcelable | Yes| Parcelable data. You need to customize the data.|

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;rpc.MessageSequence&gt; | Promise used to return the sequenceable data from the target ability.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16200001 | Caller released. The caller has been released. |
| 16200002 | Callee invalid. The callee does not exist. |
| 16000050 | Internal error. |

**Example**

  ```ts
  class MyMessageAble{
    name:''
    str:''
    num: 1
    constructor(name, str) {
      this.name = name;
      this.str = str;
    }
    marshalling(messageSequence) {
      messageSequence.writeInt(this.num);
      messageSequence.writeString(this.str);
      console.log('MyMessageAble marshalling num[${this.num}] str[${this.str}]');
      return true;
    }
    unmarshalling(messageSequence) {
      this.num = messageSequence.readInt();
      this.str = messageSequence.readString();
      console.log('MyMessageAble unmarshalling num[${this.num] str[${this.str}]');
      return true;
    }
  };
  let method = 'call_Function';
  let caller;
  export default class MainUIAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
      this.context.startAbilityByCall({
        bundleName: 'com.example.myservice',
        abilityName: 'MainUIAbility',
        deviceId: ''
      }).then((obj) => {
        caller = obj;
        let msg = new MyMessageAble(1, 'world');
        caller.callWithResult(method, msg)
          .then((data) => {
            console.log('Caller callWithResult() called');
            let retmsg = new MyMessageAble(0, '');
            data.readParcelable(retmsg);
          })
          .catch((callErr) => {
            console.log('Caller.callWithResult catch error, error.code: ${callErr.code}, error.message: ${callErr.message}');
          });
      }).catch((err) => {
        console.log('Caller GetCaller error, error.code: ${err.code}, error.message: ${err.message}');
      });
    }
  }
  ```


## Caller.release

release(): void;

Releases the caller interface of the target ability.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16200001 | Caller released. The caller has been released. |
| 16200002 | Callee invalid. The callee does not exist. |

**Example**
    
  ```ts
  let caller;
  export default class MainUIAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
      this.context.startAbilityByCall({
        bundleName: 'com.example.myservice',
        abilityName: 'MainUIAbility',
        deviceId: ''
      }).then((obj) => {
        caller = obj;
        try {
          caller.release();
        } catch (releaseErr) {
          console.log('Caller.release catch error, error.code: ${releaseErr.code}, error.message: ${releaseErr.message}');
        }
      }).catch((err) => {
        console.log('Caller GetCaller error, error.code: ${err.code}, error.message: ${err.message}');
      });
    }
  }
  ```

## Caller.onRelease

 onRelease(callback: OnReleaseCallback): void;

Registers a callback that is invoked when the stub on the target ability is disconnected.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16200001 | Caller released. The caller has been released. |

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | [OnReleaseCallback](#onreleasecallback) | Yes| Callback used to return the result.|

**Example**
    
  ```ts
  let caller;
  export default class MainUIAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
      this.context.startAbilityByCall({
        bundleName: 'com.example.myservice',
        abilityName: 'MainUIAbility',
        deviceId: ''
      }).then((obj) => {
          caller = obj;
          try {
            caller.onRelease((str) => {
                console.log(' Caller OnRelease CallBack is called ${str}');
            });
          } catch (error) {
            console.log('Caller.onRelease catch error, error.code: $error.code}, error.message: ${error.message}');
          }
      }).catch((err) => {
        console.log('Caller GetCaller error, error.code: ${err.code}, error.message: ${err.message}');
      });
    }
  }
  ```

## Caller.onRemoteStateChange<sup>10+</sup>

onRemoteStateChange(callback: OnRemoteStateChangeCallback): void;

Registers a callback that is invoked when the remote ability state changes in the collaboration scenario.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| callback | [OnRemoteStateChangeCallback](#onremotestatechangecallback) | Yes| Callback used to return the result.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16200001 | Caller released. The caller has been released. |

**Example**
    
  ```ts
  import UIAbility from '@ohos.app.ability.UIAbility';
  import window from '@ohos.window';

  let caller;
  let dstDeviceId: string;
  export default class MainAbility extends UIAbility {
      onWindowStageCreate(windowStage: window.WindowStage) {
          this.context.startAbilityByCall({
              bundleName: 'com.example.myservice',
              abilityName: 'MainUIAbility',
              deviceId: dstDeviceId
          }).then((obj) => {
              caller = obj;
              try {
                  caller.onRemoteStateChange((str) => {
                      console.log('Remote state changed ' + str);
                  });
              } catch (error) {
                  console.log('Caller.onRemoteStateChange catch error, error.code: ${JSON.stringify(error.code)}, error.message: ${JSON.stringify(error.message)}');
              }
          }).catch((err) => {
              console.log('Caller GetCaller error, error.code: ${JSON.stringify(err.code)}, error.message: ${JSON.stringify(err.message)}');
          })
      }
  }
  ```

## Caller.on

on(type: 'release', callback: OnReleaseCallback): void;

Registers a callback that is invoked when the stub on the target ability is disconnected.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Event type. The value is fixed at **release**.|
| callback | [OnReleaseCallback](#onreleasecallback) | Yes| Callback used to return the result.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 401 | If the input parameter is not valid parameter. |
| 16200001 | Caller released. The caller has been released. |

**Example**
    
  ```ts
  let caller;
  export default class MainUIAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
      this.context.startAbilityByCall({
        bundleName: 'com.example.myservice',
        abilityName: 'MainUIAbility',
        deviceId: ''
      }).then((obj) => {
          caller = obj;
          try {
            caller.on('release', (str) => {
                console.log(' Caller OnRelease CallBack is called ${str}');
            });
          } catch (error) {
            console.log('Caller.on catch error, error.code: ${error.code}, error.message: ${error.message}');
          }
      }).catch((err) => {
        console.log('Caller GetCaller error, error.code: ${err.code}, error.message: ${err.message}');
      });
    }
  }
  ```

## Caller.off

off(type: 'release', callback: OnReleaseCallback): void;

Deregisters a callback that is invoked when the stub on the target ability is disconnected. This capability is reserved.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Event type. The value is fixed at **release**.|
| callback | [OnReleaseCallback](#onreleasecallback) | Yes| Callback used to return the result.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 401 | If the input parameter is not valid parameter. |

**Example**
    
  ```ts
  let caller;
  export default class MainUIAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
      this.context.startAbilityByCall({
        bundleName: 'com.example.myservice',
        abilityName: 'MainUIAbility',
        deviceId: ''
      }).then((obj) => {
          caller = obj;
          try {
            let onReleaseCallBack = (str) => {
                console.log(' Caller OnRelease CallBack is called ${str}');
            };
            caller.on('release', onReleaseCallBack);
            caller.off('release', onReleaseCallBack);
          } catch (error) {
            console.log('Caller.on or Caller.off catch error, error.code: ${error.code}, error.message: ${error.message}');
          }
      }).catch((err) => {
        console.log('Caller GetCaller error, error.code: ${err.code}, error.message: ${err.message}');
      });
    }
  }
  ```

## Caller.off

off(type: 'release'): void;

Deregisters a callback that is invoked when the stub on the target ability is disconnected. This capability is reserved.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | string | Yes| Event type. The value is fixed at **release**.|

**Example**
    
  ```ts
  let caller;
  export default class MainUIAbility extends UIAbility {
    onWindowStageCreate(windowStage) {
      this.context.startAbilityByCall({
        bundleName: 'com.example.myservice',
        abilityName: 'MainUIAbility',
        deviceId: ''
      }).then((obj) => {
          caller = obj;
          try {
            let onReleaseCallBack = (str) => {
                console.log(' Caller OnRelease CallBack is called ${str}');
            };
            caller.on('release', onReleaseCallBack);
            caller.off('release');
          } catch (error) {  
            console.error('Caller.on or Caller.off catch error, error.code: ${error.code}, error.message: ${error.message}');
          }
      }).catch((err) => {
        console.error('Caller GetCaller error, error.code: ${err.code}, error.message: ${err.message}');
      });
    }
  }
  ```

## Callee

Implements callbacks for caller notification registration and deregistration.

## Callee.on

on(method: string, callback: CalleeCallback): void;

Registers a caller notification callback, which is invoked when the target ability registers a function.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| method | string | Yes| Notification message string negotiated between the two abilities.|
| callback | [CalleeCallback](#calleecallback) | Yes| JS notification synchronization callback of the **rpc.MessageSequence** type. The callback must return at least one empty **rpc.Parcelable** object. Otherwise, the function execution fails. |

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16200004 | Method registered. The method has registered. |
| 16000050 | Internal error. |

**Example**

  ```ts
  class MyMessageAble{
      name:''
      str:''
      num: 1
      constructor(name, str) {
        this.name = name;
        this.str = str;
      }
      marshalling(messageSequence) {
          messageSequence.writeInt(this.num);
          messageSequence.writeString(this.str);
          console.log('MyMessageAble marshalling num[${this.num}] str[${this.str}]');
          return true;
      }
      unmarshalling(messageSequence) {
          this.num = messageSequence.readInt();
          this.str = messageSequence.readString();
          console.log('MyMessageAble unmarshalling num[${this.num}] str[${this.str}]');
          return true;
      }
  };
  let method = 'call_Function';
  function funcCallBack(pdata) {
      console.log('Callee funcCallBack is called ${pdata}');
      let msg = new MyMessageAble('test', '');
      pdata.readParcelable(msg);
      return new MyMessageAble('test1', 'Callee test');
  }
  export default class MainUIAbility extends UIAbility {
    onCreate(want, launchParam) {
      console.log('Callee onCreate is called');
      try {
        this.callee.on(method, funcCallBack);
      } catch (error) {
        console.log('Callee.on catch error, error.code: ${error.code}, error.message: ${error.message}');
      }
    }
  }
  ```

## Callee.off

off(method: string): void;

Deregisters a caller notification callback, which is invoked when the target ability registers a function.

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| method | string | Yes| Registered notification message string.|

**Error codes**

| ID| Error Message|
| ------- | -------------------------------- |
| 16200005 | Method not registered. The method has not registered. |
| 16000050 | Internal error. |

**Example**
    

  ```ts
  let method = 'call_Function';
  export default class MainUIAbility extends UIAbility {
    onCreate(want, launchParam) {
      console.log('Callee onCreate is called');
      try {
        this.callee.off(method);
      } catch (error) {
        console.log('Callee.off catch error, error.code: ${error.code}, error.message: ${error.message}');
      }
    }
  }
  ```

## OnReleaseCallback

(msg: string): void;

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

| Name| Readable| Writable| Type| Description|
| -------- | -------- | -------- | -------- | -------- |
| (msg: string) | Yes| No| function | Prototype of the listener function registered by the caller.|

## OnRemoteStateChangeCallback<sup>10+</sup>

(msg: string): void;

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

| Name| Readable| Writable| Type| Description|
| -------- | -------- | -------- | -------- | -------- |
| (msg: string) | Yes| No| function | Prototype of the ability state change listener function registered by the caller in the collaboration scenario.|

## CalleeCallback

(indata: rpc.MessageSequence): rpc.Parcelable;

**System capability**: SystemCapability.Ability.AbilityRuntime.AbilityCore

| Name| Readable| Writable| Type| Description|
| -------- | -------- | -------- | -------- | -------- |
| (indata: rpc.MessageSequence | Yes| No| rpc.Parcelable | Prototype of the listener function registered by the callee.|
