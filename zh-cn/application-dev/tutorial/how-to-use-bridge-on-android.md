# 平台桥接开发指南

平台桥接用于客户端（ArkUI）和平台（Android或iOS）之间传递消息，即用于ArkUI与平台双向数据传递、ArkUI侧调用平台的方法、平台调用ArkUI侧的方法。本文主要介绍Android平台与ArkUI交互，ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，Android侧参考[BridgePlugin](../reference/arkui-for-android/BridgePlugin.md)。

## Android平台与ArkUI交互

### 创建平台桥接

1、在ArkUI侧创建平台桥接。指定名称，该名称应与Android侧平台桥接的名称一致。通过创建的该对象即可调用平台桥接的方法。

```javascript
// xxx.ets

// 导入平台桥接模块
import bridge from '@arkui-x.bridge';

// 创建平台桥接实例
const bridgeImpl = bridge.createBridge('Bridge');
// 创建平台桥接实例(二进制格式)
const bridgeImpl = bridge.createBridge('Bridge', BridgeType.BINARY_TYPE);
```

2、在Android侧创建BridgePlugin类。指定名称，该名称应与ArkUI侧平台桥接的名称一致。通过创建的该对象即可调用平台桥接的方法。

```java
// xxx.java

Bridge bridge = new Bridge(this, "Bridge", getInstanceId());
// 创建平台桥接实例(二进制格式)
Bridge bridge = new Bridge(this, "Bridge", getInstanceId()， BridgePlugin.BridgeType.BINARY_TYPE);
```

### 场景一：ArkUI侧向Android侧传递数据

1、ArkUI侧向Android侧传递数据。

```javascript
// xxx.ets

bridgeImpl.sendMessage('text').then((res)=>{
    // 监听Android侧的回执
    console.log('response: ' + res);
}).catch((err) => {
    console.log('error: ' + JSON.stringify(err));
});
```

2、Android侧接收来自ArkUI侧的数据。

```java
// xxx.java

public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

// 注册回调，监听ArkUI侧的数据传递
@Override
public Object onMessage(Object data) {
    // 返回回执给ArkUI侧
    return "java onMessage success";
}
```

### 场景二：Android侧向ArkUI侧传递数据

1、Android侧向ArkUI侧发送数据。

```java
// xxx.java

String[] data = { "message", "from", "android" };
bridge.sendMessage(data);
```

2、ArkUI侧设置回调，用于接收Android侧发送的数据。

```javascript
// xxx.ets

bridgeImpl.setMessageListener((message) => {
    console.log('receive message: ' + message);

    // 收到消息后，向Android侧发送回执
    return "ArkUI reveice message success";
});
```

3、Android侧注册回调，监听ArkUI侧收到数据后的回执。
```java
// xxx.java

public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

// 注册回调，监听ArkUI侧的回执
@Override
public void onMessageResponse(Object data) {}
```

### 场景三：ArkUI侧调用Android侧的方法

1、在ArkUI侧调用Android侧的方法。

```javascript
// xxx.ets

bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    console.log('result: ' + res);
}).catch((err) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在Android侧实现被调用的方法。

```java
// xxx.java

public platformCallMethod() {
  return "call java platformCallMethod success";
}
```


### 场景四：Android侧调用ArkUI侧的方法

1、注册ArkUI侧方法，供Android侧调用。

```javascript
// xxx.ets

function getString() {
  return 'call js getString success';
}

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2、Android侧调用ArkUI侧的方法。

```java
Object[] paramObject = {};
MethodData methodData = new MethodData("getString", paramObject);
bridge.callMethod(methodData);
```

### 场景五：ArkUI侧监听Android侧的方法

1、注册ArkUI侧方法，供Android侧调用。

```javascript
// xxx.ets

bridgeImpl.registerMethod({ name: 'getString', method: getString });
```

2、移除已注册的ArkUI侧方法。

```javascript
// xxx.ets

bridgeImpl.unRegisterMethod('getString');
```

3、在Android侧注册回调，监听方法注册、注销。

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

## 场景示例

本场景展示了当ArkUI页面启动时，调用Android侧方法，并将Android侧方法的返回值显示在页面上。点击按钮，ArkUI侧发送数据到Android侧，Android侧收到数据后，返回回执数据，并将回执数据显示在页面上。

##### ArkUI侧

编写ArkUI页面Index.ets。

```javascript
// Index.ets

// 导入平台桥接模块
import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  // 创建平台桥接对象
  private bridgeImpl = bridge.createBridge('Bridge');
  @State helloArkUI: string = '';
  @State nativeResponse: string = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    // 调用Android侧方法
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: string) => {
      // 通过状态变量，将Android侧方法的返回值显示在页面上
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
            // 发送数据到Android侧，并通过状态变量，将Android侧的响应数据显示在页面上
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

##### Android侧

```java
// Bridge.java

package com.example.bridgestage;

import android.content.Context;

// 引用平台桥接模块
import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.ace.adapter.capability.bridge.IMessageListener;

public class Bridge extends BridgePlugin implements IMessageListener {
    public Bridge(Context context, String name, int id) {
        super(context, name, id);

        setMessageListener(this);
    }

    // Android侧方法，供ArkUI侧调用
    public String getHelloArkUI() {
        return "Hello ArkUI!";
    }

    // 注册回调，接收ArkUI侧发来的数据
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
        // 建立与ArkUI侧同名的平台桥接，即可用于消息传递
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
