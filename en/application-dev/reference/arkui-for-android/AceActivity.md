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

**AceActivity** is a child class of **Activity** and is the entrance of lifecycle management for Android applications. When building an ArkUI application on Android, you must extend **AceActivity** and in this child class complete the ArkUI development paradigm configuration and JS bundle loading.

## Method Summary

| Type| Method            | Description|
| ----------- | ----------------------------------- | ---- |
| void | setInstanceName(String name) | Sets the name of an ArkUI JS bundle instance.|
| void | setVersion(int version)      | Sets the type of the ArkUI development paradigm.  |

## Methods in Action

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
