# ArkUI-X框架LogInterface使用指南

ArkUI-X框架支持日志拦截能力，Android侧提供原生接口，用于注入LogInterface接口，框架日志及ts日志通过该接口输出，本文的核心内容是介绍如何在Android平台上有效利用ArkUI-X框架的LogInterface拦截日志。

## Android平台创建ArkUI-X框架LogInterface

在Android平台创建ArkUI-X框架LogInterface需要实现[ILogger](../reference/arkui-for-android/ILogger.md)接口，实现声明接口完整示例如下：

```java
//LogInterface.java
import ohos.ace.adapter.ILogger;
public class LogInterface implements ILogger {
    @Override
    public boolean isDebuggable() {
        return false;
    }

    @Override
    public void d(String tag, String msg) {
        //对日志信息处理，落盘或输出
    }

    @Override
    public void i(String tag, String msg) {
        //对日志信息处理，落盘或输出
    }

    @Override
    public void w(String tag, String msg) {
        //对日志信息处理，落盘或输出
    }

    @Override
    public void e(String tag, String msg) {
        //对日志信息处理，落盘或输出
    }

    @Override
    public void f(String tag, String msg) {
        //对日志信息处理，落盘或输出
    }

    @Override
    public void jankLog(int tag, String msg) {
    }
}
```

## 设置ArkUI-X框架LogInterface以及日志拦截等级

​		在需要控制ArkUI-X框架日志及TypeScript日志的输出时，可以利用[StageApplicationDelegate](../reference/arkui-for-android/StageApplicationDelegate.md)类中[setLogInterface](../reference/arkui-for-android/StageApplicationDelegate.md)方法来注入LogInterface，注入成功，框架和TypeScript的ERROR和FATAL日志通过提供的这个实例的方法输出，注入失败，执行日志输出原逻辑。

​		设置日志拦截等级需使用[StageApplicationDelegate](../reference/arkui-for-android/StageApplicationDelegate.md)类中[setLogLevel](../reference/arkui-for-android/StageApplicationDelegate.md)方法，设置日志拦截等级成功，日志等级优先级低于该日志拦截等级时，日志不被输出。

​		通过[setLogInterface](../reference/arkui-for-android/StageApplicationDelegate.md)注入LogInterface时，**默认仅拦截并处理ERROR和FATAL等级日志**；通过[setLogLevel](../reference/arkui-for-android/StageApplicationDelegate.md)可降低日志拦截等级以输出更详细日志，但需特别注意：**若将日志等级开放至ERROR以下（如 WARN/INFO/DEBUG），存在应用崩溃的风险**。

**注意：开发者使用时注册，必须位于调用MyApplication超类的onCreate()方法之后**

设置ArkUI-X框架LogInterface以及日志拦截等级，完整示例如下：

```java
// MyApplication.java
import android.util.Log;
import ohos.ace.adapter.ILogger;
import ohos.stage.ability.adapter.StageApplication;
import ohos.stage.ability.adapter.StageApplicationDelegate;

public class MyApplication extends StageApplication {
    private StageApplicationDelegate appDelegate = null;
    @Override
    public void onCreate() {
        super.onCreate();//在此onCreate后注册
        LogInterface logInterface = new LogInterface(); //创建实例
        this.appDelegate = new StageApplicationDelegate(); //创建appDelegate
        this.appDelegate.setLogInterface(logInterface); //设置LogInterface
        this.appDelegate.setLogLevel(ILogger.LOG_DEBUG);//设置日志拦截等级
    }
}
```







