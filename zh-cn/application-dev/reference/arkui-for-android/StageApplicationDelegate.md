## StageApplicationDelegate

```
java.lang.Object
    └── StageApplicationDelegate
```

```
public class StageApplicationDelegate
```

StageApplicationDelegate是ArkUI-X跨平台上Stage模型Application的代理类，是启动跨平台应用的入口。跨平台的应用必须在其Application的onCreate里创建StageApplicationDelegate，并调用其初始化方法，完成js runtime创建、JsBundle资源解析等工作。

### 方法概要

| 类型 | 方法                                         | 描述             |
| ---- | -------------------------------------------- | ---------------- |
| void | init(Application application)                | 初始化           |
| void | onConfigurationChanged(Configuration newCfg) | 系统环境变化通知 |

### 方法说明

- init

```
/**
* init js environment, should called in application onCreate()
* 
* @param application the target application
*/
public void init(Application application);
```

- onConfigurationChanged

```
/**
* Called by the system when the device configuration changes while your component is running.
* 
* @param newConfig current configuration of environment.
*/
public void onConfigurationChanged(Configuration newConfig);
```