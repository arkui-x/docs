# AceActivity

```
java.lang.Object
    └── android.content.Context
        └── android.content.ContextWrapper
            └── android.view.ContextThemeWrapper
                └── android.app.Activity
                    └── ohos.ace.adapter.AceActivity
```

```
public class AceActivity
    extends Activity
```

AceActivity是Activity的子类，是Android应用的生命周期入口。ArkUI-X Android平台应用开发时，必须继承AceActivity，并在子类中完成ArkUI开发范式设置和JSBundle加载。

## 方法概要

| 类型 | 方法             | 描述 |
| ----------- | ----------------------------------- | ---- |
| void | setInstanceName(String name) | 设置ArkUI JSBundle实例名|
| void | setVersion(int version)      | 设置ArkUI开发范式类型   |

## 方法说明

- setInstanceName

```
/**
* set the instance name, should called before super.onCreate()
* 
* @param name the instance name to set
*/
public void setInstanceName(String name);
```

- setVersion

```
/**
* set ACE version
* 
* @param version ACE version, can be one of this:
*                VERSION_JS / VERSION_ETS, should called before super.onCreate()
*/
public void setVersion(int version);
```
