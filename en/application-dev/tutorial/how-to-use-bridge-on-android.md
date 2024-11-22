# How to Use the Platform Bridge on Android

Platform bridge is used for interaction between the client (ArkUI) and the platform (Android or iOS). It allows you to exchange messages between ArkUI and the platform, call platform APIs on ArkUI, and call ArkUI APIs on the platform. This tutorial describes the interaction between Android and ArkUI. For details about the API usage on ArkUI, see [Bridge API](../reference/apis/js-apis-bridge.md). For details about the API usage on Android, see [BridgePlugin](../reference/arkui-for-android/BridgePlugin.md).

## Interaction Between Android and ArkUI

### Creating a Platform Bridge

1. Create a platform bridge on ArkUI. The specified name must be the same as that on Android. You can then use the created object to call platform bridge APIs.

```javascript
// xxx.ets

// Import the platform bridge module.
import bridge from '@arkui-x.bridge';

// Create a platform bridge object.
const bridgeImpl = bridge.createBridge('Bridge');
```

2. Create the **BridgePlugin** class on Android. The specified name must be the same as that on ArkUI. You can then use the created object to call platform bridge APIs.

```java
// xxx.java

Bridge bridge = new Bridge(this, "Bridge", getInstanceId());
```

### Scenario 1: Sending Data from ArkUI to Android

1. On ArkUI, send data to Android.

```javascript
// xxx.ets

bridgeImpl.sendMessage('text').then((res)=>{
    // Listen for the response from Android.
    console.log('response: ' + res);
}).catch((err) => {
    console.log('error: ' + JSON.stringify(err));
});
```

2. On Android, receive the data sent from ArkUI.

```java
// xxx.java

public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

// Register a callback to listen for data sending events of ArkUI.
@Override
public Object onMessage(Object data) {
    // Return a response to ArkUI.
    return "java onMessage success";
}
```

### Scenario 2: Sending Data from Android to ArkUI

1. On Android, send data to ArkUI.

```java
// xxx.java

String[] data = { "message", "from", "android" };
bridge.sendMessage(data);
```

2. Register a callback on ArkUI to listen for the data sent from Android.

```javascript
// xxx.ets

bridgeImpl.setMessageListener((message) => {
    console.log('receive message: ' + message);

    // Send a response to Android.
    return "ArkUI reveice message success";
});
```

3. Register a callback on Android to listen for responses from ArkUI.
```java
// xxx.java

public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

// Register a callback to listen for responses from ArkUI.
@Override
public void onMessageResponse(Object data) {}
```

### Scenario 3: Calling Android APIs on ArkUI

1. Call Android APIs on ArkUI.

```javascript
// xxx.ets

bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    console.log('result: ' + res);
}).catch((err) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2. Implement the called APIs on Android.

```java
// xxx.java

public platformCallMethod() {
  return "call java platformCallMethod success";
}
```


### Scenario 4: Calling ArkUI APIs on Android

1. Register ArkUI APIs for Android to call.

```javascript
// xxx.ets

function getString() {
  return 'call js getString success';
}

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2. Call ArkUI APIs on Android.

```java
Object[] paramObject = {};
MethodData methodData = new MethodData("getString", paramObject);
bridge.callMethod(methodData);
```

### Scenario 5: Listening for Android APIs on ArkUI

1. Register ArkUI APIs for Android to call.

```javascript
// xxx.ets

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2. Remove the registered ArkUI APIs.

```javascript
// xxx.ets

bridgeImpl.unRegisterMethod('getString');
```

3. Register a callback on Android to listen for API registration and removal.

```java
// xxx.java

public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMethodResultListener(this);
}

@Override
public void onSuccess(Object o) {}

@Override
public void onError(String s, int i, String s1) {}

@Override
public void onMethodCancel(String s) {}
```

## Example Scenario

This example shows how to call an Android API and display its return value on the ArkUI page. Specifically, when you click the button to send a message from ArkUI to Android, Android returns a response and displays the received data on the ArkUI page.

##### ArkUI

Write the ArkUI page **Index.ets**.

```javascript
// Index.ets

// Import the platform bridge module.
import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  // Create a platform bridge object.
  private bridgeImpl = bridge.createBridge('Bridge');
  @State helloArkUI: string = '';
  @State nativeResponse: string = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    // Call Android APIs.
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: string) => {
      // Display the return value of the Android API through a status variable.
      this.helloArkUI = result;
    });
  }

  build() {
    Row() {
      Column() {
        Text(this.helloArkUI)
          .fontSize(15)
          .margin(10)
        Button('sendMessage')
          .fontSize(15)
          .margin(10)
          .onClick(async () => {
            // Send data to Android and display the response from Android through a status variable.
            this.nativeResponse = await this.bridgeImpl.sendMessage('Hello ArkUI-X!');
          })
        Text('Response from Native: ' + this.nativeResponse)
          .fontSize(15)
          .margin(10)
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

##### Android side

```java
// Bridge.java

package com.example.bridgestage;

import android.content.Context;

// Reference the platform bridge module.
import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.ace.adapter.capability.bridge.IMessageListener;

public class Bridge extends BridgePlugin implements IMessageListener {
    public Bridge(Context context, String name, int id) {
        super(context, name, id);

        setMessageListener(this);
    }

    // Define the Android API for ArkUI to call.
    public String getHelloArkUI() {
        return "Hello ArkUI!";
    }

    // Register a callback to receive data from ArkUI.
    @Override
    public Object onMessage(Object object) {
        return "java onMessage success";
    }

    @Override
    public void onMessageResponse(Object object) {}
}
```

```java
//	EntryMainActivity.java

package com.example.bridgestage;

import android.os.Bundle;
import ohos.stage.ability.adapter.StageActivity;

public class EntryMainActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        // Create a platform bridge with the same name as that of ArkUI.
        new Bridge(this, "Bridge", getInstanceId());

        super.setInstanceName("com.example.bridgestage:entry:MainAbility:");
        super.onCreate(savedInstanceState);
    }
}
```

```java
// MyApplication.java

package com.example.bridgestage;

import ohos.stage.ability.adapter.StageApplication;

public class MyApplication extends StageApplication {
    @Override
    public void onCreate() {
        super.onCreate();
    }
}
```
