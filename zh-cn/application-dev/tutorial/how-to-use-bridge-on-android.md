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
const bridgeImpl = bridge.createBridge('Bridge', bridge.BridgeType.BINARY_TYPE);
```

2、在Android侧创建BridgePlugin类。指定名称，该名称应与ArkUI侧平台桥接的名称一致。通过创建的该对象即可调用平台桥接的方法。

```java
// xxx.java

// 创建平台桥接实例(将在since 13废弃，推荐使用新构造方法)
Bridge bridge = new Bridge(this, "Bridge", getInstanceId());
Bridge bridge = new Bridge(this, "Bridge", getInstanceId(),  BridgePlugin.BridgeType.BINARY_TYPE);

// 创建平台桥接实例(新)
Bridge bridge = new Bridge(this, "Bridge", getBridgeManager());
Bridge bridge = new Bridge(this, "Bridge", getBridgeManager(), BridgePlugin.BridgeType.BINARY_TYPE);
```

### 场景一：ArkUI侧向Android侧传递数据

1、ArkUI侧向Android侧传递数据。

```javascript
// xxx.ets

private bridgeImpl = bridge.createBridge('Bridge');

this.bridgeImpl.sendMessage('text').then((res)=>{
    // 监听Android侧的回执
    console.log('response: ' + res);
}).catch((err: Error) => {
    console.log('error: ' + JSON.stringify(err));
});
```

2、Android侧接收来自ArkUI侧的数据。

```java
// xxx.java

// 创建平台桥接实例(将在since 13废弃，推荐使用新构造方法)
public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

// 创建平台桥接实例(新)
public Bridge(Context context, String name, BridgeManager bridgeManager) {
    super(context, name, bridgeManager);
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

private bridgeImpl = bridge.createBridge('Bridge');

this.bridgeImpl.setMessageListener((message) => {
    console.log('receive message: ' + message);

    // 收到消息后，向Android侧发送回执
    return "ArkUI receive message success";
});
```

3、Android侧注册回调，监听ArkUI侧收到数据后的回执。
```java
// xxx.java

// 创建平台桥接实例(将在since 13废弃，推荐使用新构造方法)
public Bridge(Context context, String name, int id) {
    super(context, name, id);
    setMessageListener(this);
}

// 创建平台桥接实例(新)
public Bridge(Context context, String name, BridgeManager bridgeManager) {
    super(context, name, bridgeManager);
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

private bridgeImpl = bridge.createBridge('Bridge');

this.bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    console.log('result: ' + res);
}).catch((err: Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在Android侧实现被调用的方法。

```java
// xxx.java

public String platformCallMethod() {
  return "call java platformCallMethod success";
}
```

复杂多参数方法调用参考[场景八](#场景八callmethod不同数据类型)

### 场景四：Android侧调用ArkUI侧的方法

1、注册ArkUI侧方法，供Android侧调用。

```javascript
// xxx.ets

private bridgeImpl = bridge.createBridge('Bridge');
private getString() : bridge.ResultValue {
    return 'call js getString success';
}

this.bridgeImpl.registerMethod({ name: 'getString', method: this.getString });
```

2、Android侧调用ArkUI侧的方法。

```java
// 方式一: 构造ArkUI侧方法描述对象实例调用
Object[] paramObject = {};
MethodData methodData = new MethodData("getString", paramObject);
bridge.callMethod(methodData);
// 方式二: ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
bridge.callMethod("getString");
```

复杂多参数方法调用参考[场景八](#场景八callmethod不同数据类型)

### 场景五：ArkUI侧监听Android侧的方法

1、注册ArkUI侧方法，供Android侧调用。

```javascript
// xxx.ets

private bridgeImpl = bridge.createBridge('Bridge');
private getString() : bridge.ResultValue {
    return 'call js getString success';
}

this.bridgeImpl.registerMethod({ name: 'getString', method: this.getString });
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
}

public Bridge(Context context, String name, BridgeManager bridgeManager) {
    super(context, name, bridgeManager);
    setMethodResultListener(this);
}

@Override
public void onSuccess(Object o) {}

@Override
public void onError(String s, int i, String s1) {}

@Override
public void onMethodCancel(String s) {}
```

### 场景六：ArkUI侧注册callBack且调用Android侧的方法（无参）

1、在ArkUI侧注册callBack且调用Android侧的方法。

```javascript
// xxx.ets
function testCallBackOfJs():bridge.ResultValue {
  return "bridge js testCallBackOfJs run";
}

this.bridgeCodec.callMethodWithCallback("testCallBack", testCallBackOfJs).then((res)=>{
    console.log('result: ' + res);
}).catch((err:Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在Android侧实现被调用的方法，调用ArkUI侧的方法。

```java
// xxx.java

public String testCallBack() {
  return "call android testCallBack success";
}

Object[] paramObject = {};
MethodData methodData = new MethodData("testCallBack", paramObject);
bridge.callMethod(methodData);
```

### 场景七：ArkUI侧注册callBack且调用Android侧的方法（有参）

1、在ArkUI侧注册callBack且调用Android侧的方法。

```javascript
// xxx.ets
function testCallBackOfJs(stringParam:string) {
  console.log("Js received a parameter of " + stringParam)
  return "js testCallBackReturn call success."
}

this.bridgeCodec.callMethodWithCallback("testCallBack", testCallBackOfJs, "js sends parameter").then((res)=>{
    console.log('result: ' + res);
}).catch((err:Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在Android侧实现被调用的方法，调用ArkUI侧的方法。

```java
// xxx.java

public String testCallBack(String sParam) {
	Log.i("Android received a parameter of ", sParam);
    return "call android testCallBack success";
}

Object[] paramObject = {"android sends parameter"};
MethodData methodData = new MethodData("testCallBack", paramObject);
bridge.callMethod(methodData);
```

### 场景八：callMethod不同数据类型

```javascript
import bridge from '@arkui-x.bridge'

@Entry
@Component
struct Index {
  @State bridgeImpl: bridge.BridgeObject = bridge.createBridge("BridgeName");

  private funTest(p1: string, p2: number, p3: boolean) : bridge.ResultValue {
    console.info('Java->Ts bridge funTest p1 is ' + p1);
    console.info('Java->Ts bridge funTest p2 is ' + p2);
    console.info('Java->Ts bridge funTest p3 is ' + p3);
    return "call success";
  }

  private funTestArray(p1: Array<string>, p2: Array<number>, p3: Array<boolean>) : bridge.ResultValue {
    console.log('Java->Ts bridge funTestArray p1 is ' + p1.toString());
    console.log('Java->Ts bridge funTestArray p2 is ' + p2.toString());
    console.log('Java->Ts bridge funTestArray p3 is ' + p3.toString());
    return "call success";
  }

  private funTestRecord(p1: Record<string, string>, p2: Record<string, number>, p3: Record<string, boolean>) : bridge.ResultValue {
    console.log('Java->Ts bridge funTestRecord p1 is ' + p1.toString());
    console.log('Java->Ts bridge funTestRecord p2 is ' + p2.toString());
    console.log('Java->Ts bridge funTestRecord p3 is ' + p3.toString());
    return "call success";
  }

  onPageShow() {
    // Register ArkUI侧 functions
    this.bridgeImpl.registerMethod({name: "funTest", method: this.funTest});
    this.bridgeImpl.registerMethod({name: "funTestArray", method: this.funTestArray});
    this.bridgeImpl.registerMethod({name: "funTestRecord", method: this.funTestRecord});
  }

  build() {
    Row() {
      Column() {
          
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

```java
// EntryEntryAbilityActivity.java
package com.example.androidTestDemo;

import android.os.Bundle;
import android.view.View;
import android.widget.Button;

import java.util.HashMap;
import java.util.Map;

import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.ace.adapter.capability.bridge.MethodData;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {
    private BridgeImpl bridgeImpl = null;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        bridgeImpl = new BridgeImpl(this, "BridgeName", getBridgeManager());
        setInstanceName("com.example.basebridge:entry:EntryAbility:");
        super.onCreate(savedInstanceState);
        // 显示应用程序界面布局(在项目的 res/layout 目录下，添加main_activity.xml文件)
        setContentView(R.layout.main_activity);
        // 注册按钮
        testCallMethod1();
        testCallMethod2();
        testCallMethod3();
    }
    
    public void testCallMethod1() {
        // 使用button按钮点击，发送信息。
        Button button = (Button) findViewById(R.id.TestCallMethod1);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 定义对象数组，存放ArkUI侧方法形参对应的实参
                Object[] paramObject = { "param1", 1, true};
                // 方式一: 构造ArkUI侧方法描述对象实例调用
                MethodData methodData = new MethodData("funTest", paramObject);
                bridgeImpl.callMethod(methodData);
                // 方式二: ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
                bridgeImpl.callMethod("funTest", "param1", 1, true);
            }
        });
    }
    public void testCallMethod2() {
        // 使用button按钮点击，发送信息。
        Button button = (Button) findViewById(R.id.TestCallMethod2);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 定义对象数组，存放ArkUI侧方法形参对应的实参
                String[] sArray = {"hello", "world"};
                int[] iArray = {123, 456};
                boolean[] bArray = {true, false};
                Object[] paramObject = {sArray, iArray, bArray};
                // 方式一: 构造ArkUI侧方法描述对象实例调用
                MethodData methodData = new MethodData("funTestArray", paramObject);
                bridgeImpl.callMethod(methodData);
                // 方式二: ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
                bridgeImpl.callMethod("funTestArray", sArray, iArray, bArray);
            }
        });
    }
    public void testCallMethod3() {
        // 使用button按钮点击，发送信息。
        Button button = (Button) findViewById(R.id.TestCallMethod3);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // 定义对象数组，存放ArkUI侧方法形参对应的实参
                Map<String, String> map1 = new HashMap<>();
                map1.put("one", "hello");
                map1.put("two", "world");
                Map<String, Integer> map2 = new HashMap<>();
                map2.put("one", 1);
                map2.put("two", 2);
                Map<String, Boolean> map3 = new HashMap<>();
                map3.put("one", true);
                map3.put("two", false);

                Object[] paramObject = {map1, map2, map3};
                // 方式一: 构造ArkUI侧方法描述对象实例调用
                MethodData methodData = new MethodData("funTestRecord", paramObject);
                bridgeImpl.callMethod(methodData);
                // 方式二: ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
                bridgeImpl.callMethod("funTestRecord", map1, map2, map3);
            }
        });
    }
}

```

```xml
// main_activity.xml

<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:rotation="360"
    android:rotationX="360"
    android:rotationY="360"
    android:visibility="visible"
    tools:context=".EntryEntryAbilityActivity"
    android:orientation="vertical">

    <LinearLayout
        android:layout_width="410dp"
        android:layout_height="650dp"
        android:orientation="vertical"
        tools:ignore="MissingConstraints">
        <Button
            android:id="@+id/TestCallMethod1"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="funTest1" />
        <Button
            android:id="@+id/TestCallMethod2"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="funTest1" />
        <Button
            android:id="@+id/TestCallMethod3"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:text="funTest1" />
    </LinearLayout>

</androidx.constraintlayout.widget.ConstraintLayout>
```

### 

## 场景示例

本场景展示了当ArkUI页面启动时，调用Android侧方法，并将Android侧方法的返回值显示在页面上。点击按钮，ArkUI侧发送数据到Android侧，Android侧收到数据后，返回回执数据，并将回执数据显示在页面上。

##### ArkUI侧

编写ArkUI页面Index.ets。

```javascript
import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  // 创建平台桥接对象
  private bridgeImpl = bridge.createBridge('Bridge');
  @State helloArkUI: bridge.ResultValue = '';
  @State nativeResponse: bridge.Message = '';

  aboutToAppear() {
    this.getHelloArkUI();
  }

  getHelloArkUI() {
    // 调用Android侧方法
    this.bridgeImpl.callMethod('getHelloArkUI').then((result: bridge.ResultValue) => {
      // 通过状态变量，将Android侧方法的返回值显示在页面上
      this.helloArkUI = result;
    });
  }

  build() {
    Row() {
      Column() {
        Text(this.helloArkUI?.toString())
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
import ohos.ace.adapter.capability.bridge.BridgeManager;
import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.ace.adapter.capability.bridge.IMessageListener;

public class Bridge extends BridgePlugin implements IMessageListener {
	// 创建平台桥接实例(将在since 13废弃，推荐使用新构造方法)
    public Bridge(Context context, String name, int id) {
    	super(context, name, id);
        setMessageListener(this);
    }
    
    // 创建平台桥接实例
    public Bridge(Context context, String name, BridgeManager bridgeManager) {
    	super(context, name, bridgeManager);
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
