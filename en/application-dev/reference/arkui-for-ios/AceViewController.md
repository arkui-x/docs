# AceViewController

```
UIResponder
    └── UIViewController
        └── AceViewController
```

```
@interface AceViewController : UIViewController
```

**AceViewController** is a child class of **UIViewController** and is the entrance of lifecycle management for iOS applications. When building an ArkUI application on iOS, you must instantiate **AceViewController** and complete the ArkUI development paradigm configuration and JS bundle loading.

## Method Summary

| Type   | Method            | Description|
| ----------- | ----------------------------------- | ---- |
| instancetype | initWithVersion | Sets the name of an ArkUI JS bundle instance and the type of the ArkUI development paradigm.|

## Methods in Action

- initWithVersion

```
/**
 * Initializes this AceViewController with the specified instance name.
 *
 *  This is used for pure ace application. It will combine the js/`instanceName` as the
 *  bundleDirectory.
 *
 * @param version  ACE version, can be one of this:
 *                ACE_VERSION_JS / ACE_VERSION_ETS,
 * @param instanceName instance name.
 */
- (instancetype)initWithVersion:(ACE_VERSION)version
                  instanceName:(nonnull NSString*)instanceName;
```
