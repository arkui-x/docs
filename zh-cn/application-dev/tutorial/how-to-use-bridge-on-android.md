# 平台桥接开发指南

平台桥接用于客户端（ArkUI）和平台（Android或iOS）之间传递消息，即用于ArkUI与平台双向数据传递、ArkUI侧调用平台的方法、平台调用ArkUI侧的方法。本文主要介绍Android平台与ArkUI交互，ArkUI侧具体用法请参考[Bridge API](../reference/apis/js-apis-bridge.md)，Android侧参考[BridgePlugin](../reference/arkui-for-android/BridgePlugin.md)。

## Android平台与ArkUI交互

### 创建平台桥接

1、在ArkUI侧创建平台桥接。指定名称，该名称应与Android侧平台桥接的名称一致。通过创建的该对象即可调用平台桥接的方法。

```typescript
// xxx.ets

// 导入平台桥接模块
import bridge from '@arkui-x.bridge';

// 创建平台桥接实例(默认为JSON模式)
const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
// 创建平台桥接实例(二进制模式)
const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName', bridge.BridgeType.BINARY_TYPE);
```

2、在Android侧创建BridgePlugin类。指定名称，该名称应与ArkUI侧平台桥接的名称一致。通过创建的该对象即可调用平台桥接的方法。

```java
// xxx.java

// 创建平台桥接实例(JSON模式)
BridgeImpl bridgeImpl = new BridgeImpl("BridgeName", BridgePlugin.BridgeType.JSON_TYPE);
// 创建平台桥接实例(二进制模式)
BridgeImpl bridgeImpl = new BridgeImpl("BridgeName", BridgePlugin.BridgeType.BINARY_TYPE);
```

```java
// BridgeImpl.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class BridgeImpl extends BridgePlugin {

    public BridgeImpl(String name, BridgeType bridgeType) {
        super(name, bridgeType);
    }

}
```



### 场景一：ArkUI侧向Android侧传递数据

1、ArkUI侧向Android侧传递数据。

```typescript
// xxx.ets

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
bridgeImpl.sendMessage("data").then((res)=>{
    // 监听Android侧的回执
    console.log('response: ' + res);
}).catch((err: Error) => {
    console.log('error: ' + JSON.stringify(err));
});
```

2、Android侧接收来自ArkUI侧的数据。

```java
// xxx.java

BridgeImpl bridgeImpl = new BridgeImpl("BridgeName", BridgePlugin.BridgeType.JSON_TYPE);
// 设置消息监听，用于捕获ArkUI侧发送的消息(onMessage)。
bridgeImpl.setMessageListener(new BridgeMessageListener());
```

```java
// BridgeMessageListener.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.IMessageListener;

public class BridgeMessageListener implements IMessageListener {
    @Override
    public Object onMessage(Object data) {
        // 返回回执给ArkUI侧
        return "Android onMessage success";
    }
    
    @Override
    public void onMessageResponse(Object o) {

    }
}
```



### 场景二：Android侧向ArkUI侧传递数据

1、Android侧向ArkUI侧发送数据。

```java
// xxx.java

BridgeImpl bridgeImpl = new BridgeImpl("BridgeName", BridgePlugin.BridgeType.JSON_TYPE);
bridgeImpl.sendMessage("message from android");
```

2、ArkUI侧设置回调，用于接收Android侧发送的数据。

```typescript
// xxx.ets

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
bridgeImpl.setMessageListener((data: bridge.Message) => {
    console.log("bridge setMessageListener data : " + JSON.stringify(data));
    return "ArkTS Received";
});
```

3、Android侧注册回调，监听ArkUI侧收到数据后的回执。

```java
// xxx.java

// 设置消息监听，用于捕获ArkUI侧应答的消息(onMessageResponse)。
bridgeImpl.setMessageListener(new BridgeMessageListener());
```

```java
// BridgeMessageListener.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.IMessageListener;

public class BridgeMessageListener implements IMessageListener {
    @Override
    public Object onMessage(Object data) {
        // 返回回执给ArkUI侧
        return "Android onMessage success";
    }

    @Override
    public void onMessageResponse(Object o) {
        System.out.println("ArkTS received data is " + o.toString());
    }
}
```

### 场景三：ArkUI侧调用Android侧的方法

1、在ArkUI侧调用Android侧的方法。

```typescript
// xxx.ets

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
bridgeImpl.callMethod('platformCallMethod').then((res)=>{
    // Android侧方法返回结果
    console.log('result: ' + res);
}).catch((err: Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在Android侧实现被调用的方法。

```java
// BridgeImpl.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class BridgeImpl extends BridgePlugin {

    public BridgeImpl(String name, BridgeType bridgeType) {
        super(name, bridgeType);
    }

    // Android侧方法，供ArkUI侧调用
    public String platformCallMethod() {
        return "call java platformCallMethod success";
    }
}
```

> 复杂多参数方法调用参考[场景八](#场景八callmethod不同数据类型)

### 场景四：Android侧调用ArkUI侧的方法

1、注册ArkUI侧方法，供Android侧调用。

```typescript
// xxx.ets

private getArkTSData(param: number) : bridge.ResultValue {
    console.log("The getArkTSData param is " + JSON.stringify(param))
    return "call getArkTSData success";
}

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
bridgeImpl.registerMethod({name: "getArkTSData", method: this.getArkTSData});
```

2、Android侧调用ArkUI侧的方法。

```java
// xxx.java

// 方式一: 构造ArkUI侧方法描述对象实例调用
Object[] paramObject = { 100 };
MethodData methodData = new MethodData("getArkTSData", paramObject);
bridge.callMethod(methodData);
    
// 方式二: ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
bridgeImpl.callMethod("getArkTSData", 100);

```

> 获取ArkUI侧的方法执行结果参考[场景五](#场景五：Android侧监听ArkUI侧的方法执行结果)
>
> 复杂多参数方法调用参考[场景八](#场景八callmethod不同数据类型)

### 场景五：Android侧监听ArkUI侧的方法执行结果

1、Android侧设置ArkUI侧方法的执行结果监听

```java
// xxx.java

// 设置ArkUI侧方法的执行结果监听
bridgeImpl.setMethodResultListener(new BridgeMethodResult());
```

2、在Android侧注册回调，监听方法注册、注销类。

```java
// BridgeMethodResult.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.IMethodResult;

public class BridgeMethodResult implements IMethodResult {
    @Override
    public void onSuccess(Object object) {
        if (object == null) {
            System.out.println("ArkTS method call success, return data is null.");
        } else {
            System.out.println("ArkTS method call success, return data is " + object.toString());
        }
    }

    @Override
    public void onError(String methodName, int errorCode, String errorMessage) {
        System.out.println("ArkTS method call error, methodName : " + methodName +
                                                    "errorCode: " + errorCode +
                                                    "errorMessage: " + errorMessage);
    }
    @Override
    public void onMethodCancel(String methodName) {
        System.out.println("ArkTS cancel method is " + methodName);
    }
}
public void onMethodCancel(String s) {}
```

### 场景六：ArkUI侧注册callBack且调用Android侧的方法（无参）

1、在ArkUI侧注册callBack且调用Android侧的方法。

```typescript
// xxx.ets
private testCallBackOfArkTS():bridge.ResultValue {
    return "bridge js testCallBackOfJs run";
}

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
bridgeImpl.callMethodWithCallback("testCallBack", this.testCallBackOfArkTS).then((res)=>{
    console.log('result: ' + res);
}).catch((err:Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在Android侧实现被调用的方法，调用ArkUI侧的方法。

```java
// BridgeImpl.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class BridgeImpl extends BridgePlugin {

    public BridgeImpl(String name, BridgeType bridgeType) {
        super(name, bridgeType);
    }

    // Android侧实现被调用的方法
    public String testCallBack() {
      return "call android testCallBack success";
    }
}
```

```java
// xxx.java

// 调用ArkUI侧的方法
bridgeImpl.callMethod("testCallBack");
```

### 场景七：ArkUI侧注册callBack且调用Android侧的方法（有参）

1、在ArkUI侧注册callBack且调用Android侧的方法。

```typescript
// xxx.ets

private testCallBackOfArkTSParam(stringParam:string) {
    console.log("ArkTS received a parameter of " + stringParam)
    return "ArkTS testCallBackReturn call success."
}

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
bridgeImpl.callMethodWithCallback("testCallBack", this.testCallBackOfArkTSParam, "ArkTS sends parameter").then((res) => {
    console.log('result: ' + res);
}).catch((err: Error) => {
    console.error('error: ' + JSON.stringify(err));
});
```

2、在Android侧实现被调用的方法，调用ArkUI侧的方法。

```java
// BridgeImpl.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class BridgeImpl extends BridgePlugin {

    public BridgeImpl(String name, BridgeType bridgeType) {
        super(name, bridgeType);
    }

    // Android侧实现被调用的方法
    public String testCallBack(String sParam) {
        Log.i("Android received a parameter of ", sParam);
        return "call android testCallBack success";
    }
}
```

```java
// xxx.java

// 调用ArkUI侧的方法
bridgeImpl.callMethod("testCallBack", "android sends parameter");
```

### 场景八：callMethod不同数据类型

1、ArkUI侧

```typescript
// xxx.ets

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

registerMethod() {
    const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
    bridgeImpl.registerMethod({name: "funTest", method: this.funTest});
    bridgeImpl.registerMethod({name: "funTestArray", method: this.funTestArray});
    bridgeImpl.registerMethod({name: "funTestRecord", method: this.funTestRecord});
}
```

2、Android侧

```java
// xxx.java

// 定义对象数组，存放ArkUI侧方法形参对应的实参
Object[] paramObject = { "param1", 1, true};
// 方式一: 构造ArkUI侧方法描述对象实例调用
MethodData methodData = new MethodData("funTest", paramObject);
bridgeImpl.callMethod(methodData);
// 方式二: ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
bridgeImpl.callMethod("funTest", "param1", 1, true);

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
```

### 场景九：ArkUI侧同步调用Android侧的方法

1、在ArkUI侧同步调用Android侧的方法。

```typescript
// xxx.ets

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');

try {
	let result : bridge.ResultValue | undefined = bridgeImpl.callMethodSync("platformCallMethod");
	console.log('platform Method result: ' + JSON.stringify(result));
} catch (exception) {
    console.error(`Cause code: ${exception.code}, message: ${exception.message}`);
}
```

2、在Android侧实现被调用的方法。

```java
// BridgeImpl.java

package XXX.XXX.XXX;

import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class BridgeImpl extends BridgePlugin {

    public BridgeImpl(String name, BridgeType bridgeType) {
        super(name, bridgeType);
    }

    // Android侧方法，供ArkUI侧调用
    public String platformCallMethod() {
        return "call java platformCallMethod success";
    }
}
```

### 场景十：Android侧同步调用ArkUI侧的方法

1、注册ArkUI侧方法，供Android侧调用。

```typescript
// xxx.ets

private getArkTSData(param: number) : bridge.ResultValue {
    console.log("The getArkTSData param is " + JSON.stringify(param))
    return "call getArkTSData success";
}

const bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');
bridgeImpl.registerMethod({name: "getArkTSData", method: this.getArkTSData});
```

2、Android侧调用ArkUI侧的方法。

```java
// xxx.java

try {
	// ArkUI侧函数名加ArkUI侧方法形参对应的实参调用
	Object result = bridgeImpl.callMethodSync("getArkTsData", 100);
} catch (Exception e) {
    textView1.setText(e.toString());
}
```



## 场景示例

本场景展示了当ArkUI页面启动时，调用Android侧方法，并将Android侧方法的返回值显示在页面上。点击按钮，ArkUI侧发送数据到Android侧，Android侧收到数据后，返回回执数据，并将回执数据显示在页面上。

#### ArkUI侧

编写ArkUI页面Index.ets。

```typescript
import bridge from '@arkui-x.bridge';

@Entry
@Component
struct Index {
  @State message: string = 'Hello World';
  @State bridgeImpl: bridge.BridgeObject = bridge.createBridge('BridgeName');

  private testSendMessage() {
    this.bridgeImpl.sendMessage("data").then((res)=>{
      // 监听Android侧的回执
      this.message = JSON.stringify(res);
      console.log('response: ' + res);
    }).catch((err: Error) => {
      console.log('error: ' + JSON.stringify(err));
    });
  }

  private setMessageListener() {
    this.bridgeImpl.setMessageListener((data: bridge.Message) => {
      console.log("bridge setMessageListener data : " + JSON.stringify(data));
      return "ArkTS Received";
    });
  }

  private testCallMethod() {
    this.bridgeImpl.callMethod('platformCallMethod').then((res)=>{
      console.log('result: ' + res);
      this.message = JSON.stringify(res);
    }).catch((err: Error) => {
      console.error('error: ' + JSON.stringify(err));
    });
  }

  private testCallBackOfArkTS():bridge.ResultValue {
    return "bridge testCallBackOfArkTS run";
  }

  private testCallMethodWithCallback() {
    this.bridgeImpl.callMethodWithCallback("testCallBack", this.testCallBackOfArkTS).then((res)=>{
      console.log('result: ' + res);
      this.message = JSON.stringify(res);
    }).catch((err:Error) => {
      console.error('error: ' + JSON.stringify(err));
    });
  }

  private testCallBackOfArkTSParam(stringParam:string) {
    console.log("ArkTS received a parameter of " + stringParam)
    return "ArkTS testCallBackReturn call success."
  }

  private testCallMethodWithCallback2() {
    this.bridgeImpl.callMethodWithCallback("testCallBack", this.testCallBackOfArkTSParam, "ArkTS sends parameter").then((res) => {
      this.message = JSON.stringify(res);
      console.log('result: ' + res);
    }).catch((err: Error) => {
      console.error('error: ' + JSON.stringify(err));
    });
  }

  private getArkTSData(param: number) : bridge.ResultValue {
    console.log("The getArkTSData param is " + JSON.stringify(param))
    return "call getArkTSData success";
  }

  private testRegisterMethod() {
    this.bridgeImpl.registerMethod({name: "getArkTSData", method: this.getArkTSData});
  }

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

  private registerMethod() {
    this.bridgeImpl.registerMethod({name: "funTest", method: this.funTest});
    this.bridgeImpl.registerMethod({name: "funTestArray", method: this.funTestArray});
    this.bridgeImpl.registerMethod({name: "funTestRecord", method: this.funTestRecord});
  }

  private testCallMethodSync() {
    try {
      let result : bridge.ResultValue | undefined = this.bridgeImpl.callMethodSync("platformCallMethod");
      console.log('platform Method result: ' + JSON.stringify(result));
    } catch (exception) {
      console.error(`Cause code: ${exception.code}, message: ${exception.message}`);
    }
  }


  build() {
    Row() {
      Column() {
        Text(this.message)
          .fontSize($r('app.float.page_text_font_size'))
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.message = 'Welcome';
          })

        Button("Register & Listener")
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.setMessageListener();
            this.testRegisterMethod();
            this.registerMethod();
          })

        Button("testSendMessage")
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.testSendMessage();
          })

        Button("testCallMethod")
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.testCallMethod();
          })

        Button("testCallMethodWithCallback")
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.testCallMethodWithCallback();
            this.testCallMethodWithCallback2();
          })

        Button("testCallMethodSync")
          .fontWeight(FontWeight.Bold)
          .onClick(() => {
            this.testCallMethodSync();
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

#### Android侧

**EntryEntryAbilityActivity.java**

```java
package com.example.bridgeimpl;

import android.os.Bundle;

import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.stage.ability.adapter.StageActivity;

public class EntryEntryAbilityActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        setInstanceName("com.example.bridgeimpl:entry:EntryAbility:");

        BridgeImpl bridgeImpl = new BridgeImpl("BridgeName", BridgePlugin.BridgeType.JSON_TYPE);
        bridgeImpl.setMessageListener(new BridgeMessageListener());
        bridgeImpl.setMethodResultListener(new BridgeMethodResult());

        super.onCreate(savedInstanceState);
    }
}

```

**BridgeImpl.java**

```java
package com.example.bridgeimpl;

import ohos.ace.adapter.capability.bridge.BridgePlugin;

public class BridgeImpl extends BridgePlugin {

    public BridgeImpl(String name, BridgeType bridgeType) {
        super(name, bridgeType);
    }

    public String platformCallMethod() {
        return "call java platformCallMethod success";
    }

    public String testCallBack(String sParam) {
        return "call android testCallBack success";
    }
}
```

**BridgeMessageListener.java**

```java
package com.example.bridgeimpl;

import ohos.ace.adapter.capability.bridge.IMessageListener;

public class BridgeMessageListener implements IMessageListener {
    @Override
    public Object onMessage(Object data) {
        // 返回回执给ArkUI侧
        return "Android onMessage success";
    }

    @Override
    public void onMessageResponse(Object o) {
        System.out.println("ArkTs received data is " + o.toString());
    }
}

```

**BridgeMethodResult.java**

```java
package com.example.bridgeimpl;

import ohos.ace.adapter.capability.bridge.IMethodResult;

public class BridgeMethodResult implements IMethodResult {
    @Override
    public void onSuccess(Object object) {
        if (object == null) {
            System.out.println("ArkTs method call success, return data is null.");
        } else {
            System.out.println("ArkTs method call success, return data is " + object.toString());
        }
    }

    @Override
    public void onError(String methodName, int errorCode, String errorMessage) {
        System.out.println("ArkTs method call error, methodName : " + methodName +
                                                    "errorCode: " + errorCode +
                                                    "errorMessage: " + errorMessage);
    }
    @Override
    public void onMethodCancel(String methodName) {
        System.out.println("ArkTs cancel method is " + methodName);
    }
}

```
