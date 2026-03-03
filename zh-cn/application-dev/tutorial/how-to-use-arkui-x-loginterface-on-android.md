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
   
   可以通过以下两种方式注入LogInterface并设置日志等级。注入成功，框架和ArkTS的日志通过提供的这个实例的方法输出，注入失败，执行日志输出原逻辑。注意：**若将日志等级开放至ERROR以下（如 WARN/INFO/DEBUG），存在应用崩溃的风险**。

### setLogger

​		[StageApplicationDelegate](../reference/arkui-for-android/StageApplicationDelegate.md)类中[setLogger](../reference/arkui-for-android/StageApplicationDelegate.md)方法用来注入LogInterface并设置日志等级。

​		区别于setLogInterface方法，setLogger能拦截到启动时的完整日志，推荐使用setLogger方法。

**注意：setLogger方法调用必须位于MyApplication超类的onCreate()方法之前**

使用setLogger设置ArkUI-X框架LogInterface以及日志拦截等级，完整示例如下：

```java
// MyApplication.java
import ohos.ace.adapter.ILogger;
import ohos.stage.ability.adapter.StageApplication;
import ohos.stage.ability.adapter.StageApplicationDelegate;

public class MyApplication extends StageApplication {
    private StageApplicationDelegate appDelegate = null;
    @Override
    public void onCreate() {
        LogInterface logInterface = new LogInterface(this); //创建实例
        appDelegate = new StageApplicationDelegate(); //创建appDelegate
        appDelegate.setLogger(logInterface,ILogger.LOG_DEBUG); //设置拦截的实例及日志等级
        super.onCreate();
    }
}
```

### setLogInterface

​		[StageApplicationDelegate](../reference/arkui-for-android/StageApplicationDelegate.md)类中[setLogInterface](../reference/arkui-for-android/StageApplicationDelegate.md)方法用来注入LogInterface。[setLogLevel](../reference/arkui-for-android/StageApplicationDelegate.md)方法用来设置日志拦截等级，日志等级于该日志拦截等级时，日志不被输出。
​		通过[setLogInterface](../reference/arkui-for-android/StageApplicationDelegate.md)注入LogInterface时，**默认仅拦截并处理ERROR和FATAL等级日志**。

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







