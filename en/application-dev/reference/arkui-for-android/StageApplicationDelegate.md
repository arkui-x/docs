## StageApplicationDelegate

```
java.lang.Object
    └── StageApplicationDelegate
```

```
public class StageApplicationDelegate
```

**StageApplicationDelegate** is the proxy class of ArkUI-X cross-platform applications of the stage model. It is the entry for starting these applications. For a cross-platform application, you must create **StageApplicationDelegate** in its **onCreate** callback and call the initialization method to create JS runtime and parse JsBundle resources.

### Method Summary

| Type| Method                                        | Description            |
| ---- | -------------------------------------------- | ---------------- |
| void | init(Application application)                | Initializes the system environment.          |
| void | onConfigurationChanged(Configuration newCfg) | Called when the system environment changes.|

### Method Description

- init

  ```
  /**
  * Initializes the JS environment. It should be called in onCreate().
  * 
  * @param application Target application.
  */
  public void init(Application application);
  ```

- onConfigurationChanged

  ```
  /**
  * Called by the system when the device configuration changes while your component is running.
  * 
  * @param newConfig New environment configuration.
  */
  public void onConfigurationChanged(Configuration newConfig);
  ```