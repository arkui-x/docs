# ArkUI-X API 20+版本，Android bridge 里使用原生 UI 报错抛异常问题解决

**【问题现象】**

在bridge 的 IMethodResult 和 IMessageListener 监听结果回调的方法里直接使用Android UI，如 Toast，TextView等，抛出异常在日志中可以查阅到异常日志。

**【问题原因】**

在since 22 版本为了优化运行的性能，Bridge运行线程和主线程解耦，在独立的子线程运行。如果在Bridge的回调处理中，有涉及UI的，需要抛回到主线程中执行，否则会出现异常。

**【解决方法】**

将 Android UI 操作调度至主线程执行。

**修改前：**

<img src="../figures/Dev-faq-22-1.png" alt="image" style="zoom:60%;" />

**修改后：**

<img src="../figures/Dev-faq-22-2.png" alt="image" style="zoom:60%;" />

**代码示例：**

```java
package com.example.androidCopeObject;
import static android.widget.Toast.LENGTH_SHORT;

import android.content.Context;
import android.os.Handler;
import android.os.Looper;
import android.widget.Toast;

import ohos.ace.adapter.ALog;
import ohos.ace.adapter.capability.bridge.BridgePlugin;
import ohos.ace.adapter.capability.bridge.IMethodResult;

public class BridgeTest extends BridgePlugin implements IMethodResult {

    public static int STRESS_TEST = 0;
    public static String STRESS_TEST_MESSAGE = "";
    public Context context = null;

    public BridgeTest(String name, BridgeType bridgeType) {
        super(name, bridgeType);
        setMethodResultListener(this);
    }

    public void setContext(Context context) {
        this.context = context;
    }

    @Override
    public void onSuccess(Object o) {
        if (o == null) {
            ALog.i("bridge return data is null", " call Success");
            new Handler(Looper.getMainLooper()).post(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(context, "callMethod success, The method is of type void" , LENGTH_SHORT).show();
                }
            });
        } else {
            ALog.i("bridge onSuccess data:", o.toString());
            new Handler(Looper.getMainLooper()).post(new Runnable() {
                @Override
                public void run() {
                    Toast.makeText(context, o.toString() + "  ", Toast.LENGTH_SHORT).show();
                }
            });
        }
    }

    @Override
    public void onError(String s, int i, String s1) {
        ALog.e("BridgeTest:", "onError: " + s + " " + String.valueOf(i) + s1);
        new Handler(Looper.getMainLooper()).post(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(context,
                        "onError data:" + s + String.valueOf(i) + s1 + "  ", LENGTH_SHORT).show();
            }
        });
    }

    @Override
    public void onMethodCancel(String s) {
        ALog.i("BridgeTest:", "onMethodCancel");
        new Handler(Looper.getMainLooper()).post(new Runnable() {
            @Override
            public void run() {
                Toast.makeText(context, "onError data:" + s + "  ", LENGTH_SHORT).show();
            }
        });
    }
}
```

