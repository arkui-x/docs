# 平台桥接开发指南

​	 平台桥接用于ArkUI端和Platform平台端消息通信，包括数据传输、方法调用和事件调用，ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，Android侧参考[BridgePlugin](../reference/arkui-for-android/BridgePlugin.md)。

## Android平台与ArkUI交互

### ArkUI

#### 创建Bridge

createBridge(bridgeName: string): BridgeObject

创建BridgeObject类。

```javascript
import bridge from '@arkui-x.bridge';

const bridgeImpl = bridge.createBridge('Bridge');
```

#### 调用方法

callMethod(methodName: string, parameters?: Record\<string, Parameter\>): Promise\<ResultValue\>

callMethod(methodName: string, ...parameters: Array\<any\>): Promise\<ResultValue\>

调用平台侧的方法。

```javascript
bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    console.log('result: ' + res);
}).catch((err) => {
    console.error('error: ' + JSON.stringify(err));
});
```

#### 注册方法

registerMethod(method: MethodData, callback: AsyncCallback\<void\>): void

registerMethod(method: MethodData): Promise\<void\>

注册ArkUI侧方法，供Android或iOS侧调用。

```javascript
function getString() {
  return 'call js getString success';
}

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

#### 注销方法

unRegisterMethod(methodName: string, callback: AsyncCallback\<void\>): void

unRegisterMethod(methodName: string): Promise\<void\>

移除已注册的ArkUI侧方法。

```javascript
function getString() {
  return 'call js getString success';
}

bridgeImpl.unRegisterMethod('getString');
```

#### 发送数据

sendMessage(message: Message, callback: AsyncCallback\<Response\>): void

sendMessage(message: Message): Promise\<Response\>

向Android或iOS侧发送数据。

```javascript
bridgeImpl.sendMessage('text').then((res)=>{
    console.log('response: ' + res);
}).catch((err) => {
    console.log('error: ' + JSON.stringify(err));
});
```

#### 设置发送数据回调

setMessageListener(callback: (message: Message) => Response)

设置回调，用于接收Android或iOS侧发送的数据。

```javascript
bridgeImpl.setMessageListener((message) => {
    console.log('receive message: ' + message);
});
```

### Android

#### 创建Bridge

BridgePlugin(Context context, String bridgeName, int instanceId)

创建BridgePlugin类。

```java
Bridge bridge = new Bridge(this, "Bridge", getInstanceId());
```

#### 调用方法

public void callMethod(MethodData methodData)

调用ArkUI侧方法。

```java
Object[] paramObject = {};
MethodData methodData = new MethodData("getString", paramObject);
bridge.callMethod(methodData);
```

#### 发送数据

public void sendMessage(Object data)

向ArkUI侧发送数据。

```java
String[] data = { "message", "from", "native" };
bridge.sendMessage(data);
```

#### 注册消息监听

public void setMessageListener(IMessageListener messageListener)

注册回调，监听消息发送的结果。

```java
public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

@Override
public Object onMessage(Object data) {
    return "java onMessage success";
}
@Override
public void onMessageResponse(Object data) {}
```

#### 注册方法返回监听

public void setMethodResultListener(IMethodResult methodResultListener)

注册回调，监听方法返回的结果。

```java
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

## 场景示例

##### 前端UI用例

```javascript
// Index.ets

import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  private bridgeImpl = bridge.createBridge('Bridge');
  @State helloArkUI: string = '';
  @State nativeResponse: string = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: string) => {
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

##### Android用例

```java
// Bridge.java

package com.example.bridgestage;

import android.content.Context;

import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.ace.adapter.capability.bridge.IMessageListener;

public class Bridge extends BridgePlugin implements IMessageListener {
    public Bridge(Context context, String name, int id) {
        super(context, name, id);

        setMessageListener(this);
    }

    public String getHelloArkUI() {
        return "Hello ArkUI!";
    }

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
