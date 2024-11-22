# IArkUIXPlugin

The **IArkUIXPlugin** module provides the plugin functions of the iOS platform, including plugin lifecycle callbacks, obtaining the application registration information and BridgePluginManger, and interacting with ArkUI pages through the bridge. For details about the bridge, see [BridgePlugin](BridgePlugin.md).

> **NOTE**
>
> The initial APIs of this module are supported since API version 11. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## onRegistry

```objective-c
- (void)onRegistry:(PluginContext *)pluginContext;
```

**Description**

Called when a plugin is registered.

**Parameters**

| Name          | Type         | Mandatory| Description              |
| ------------- | ------------- | ---- | ------------------ |
| PluginContext | pluginContext | Yes  | Context required by the plugin.|

For details, see [PluginContext](#plugincontext).

**Return value**

None

**Example**

None

## onUnRegistry

```objective-c
- (void)onUnRegistry:(PluginContext *)pluginContext;
```

**Description**

Called when a plugin is unregistered.

**Parameters**

| Name          | Type         | Mandatory| Description              |
| ------------- | ------------- | ---- | ------------------ |
| PluginContext | pluginContext | Yes  | Context required by the plugin.|

For details, see [PluginContext](#plugincontext).

**Return value**

None

**Example**

None

# PluginContext

Provides the plugin context, including the BridgePluginManager that may be required by the plugin.

> **NOTE**
>
> The initial APIs of this module are supported since API version 11. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## getBridgePluginManager

```
- (BridgePluginManager *)getBridgePluginManager;
```

**Description**

Obtains a **BridgePluginManager** instance.

**Parameters**

None

**Return value**

| Type               | Description                   |
| ------------------- | ----------------------- |
| BridgePluginManager | **BridgePluginManager** instance.|

**Example**

```objective-c
 myBridge = [[MyBridge alloc] initBridgePlugin:@"Bridge" bridgeType:BINARY_TYPE 
 bridgeManager:[pluginContext getBridgePluginManager]];
```

## getRawFilePath

```
(NSString *)getRawFilePath:(NSString *)filePath;
```

**Description**

Obtains the absolute path of the resource files in **rawfile**.

**Parameters**

| Name     | Type    | Mandatory| Description                                                        |
| -------- | -------- | ---- | ------------------------------------------------------------ |
| filePath | NSString | Yes  | Relative path of the resource files in the **rawfile** directory (**arkui-x/entry/resources/rawfile**).|

**Return value**

| Type    | Description                         |
| -------- | ----------------------------- |
| NSString | Absolute path of the resource files in **rawfile**.|

**Example**

```objective-c
// Obtain arkui-x/entry/resources/rawfile/icon.png.
NSString *filePath = [pluginContext getRawFilePath:@"icon.png"];
```

## getRawFilePath

```
(NSString *)getRawFilePath:(NSString *)name filePath:(NSString *)filePath;
```

**Description**

Obtains the absolute path of the resource files in **rawfile** of the specified module.

**Parameters**

| Name     | Type    | Mandatory| Description                                                        |
| -------- | -------- | ---- | ------------------------------------------------------------ |
| name     | NSString | Yes  | Name of the module to which **rawfile** belongs.                         |
| filePath | NSString | Yes  | Relative path of the resource files in the **rawfile** directory (**arkui-x/module/resources/rawfile**).|

**Return value**

| Type    | Description                         |
| -------- | ----------------------------- |
| NSString | Absolute path of the resource files in **rawfile**.|


**Example**

```objective-c
// Obtain arkui-x/hsp/resources/rawfile/icon.png.
NSString *filePath = [pluginContext getRawFilePath:@"hsp"filePath:@"icon.png"];
```



## Sample

[Sample](../../tutorial/how-to-use-arkui-x-plugin-on-ios.md)
