## StageApplication

```
NSObject
    └── StageApplication
```

**StageApplication** is responsible for initialization of iOS applications.

## Method Summary

| Type        | Method                | Description  |
| ------------ | -------------------- | ------ |
| void | configModuleWithBundleDirectory | Configures the HAP file path in the project.|
| void | launchApplication | Schedules the application initialization process.|

## Method Description


+ configModuleWithBundleDirectory:;

  ```
  /**
  * Configures the HAP file path in the iOS project.
   *
   *  Configures the HAP file path: bundleDirectory,
   *  which is within the iOS project.
   *
   * @param bundleDirectory HAP file path.
   */+ (void)configModuleWithBundleDirectory:(NSString *_Nonnull)bundleDirectory;
  ```

+ launchApplication;

  ```
  /**
  * Schedules the application initialization process.
   *
   *  When the stage-model ability is launching,
   *  the iOS project schedules the application initialization process.
   *
   */+ (void)launchApplication;
  ```