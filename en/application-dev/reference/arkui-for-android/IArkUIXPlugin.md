# IArkUIXPlugin

The **IArkUIXPlugin** module provides the plugin functions of the Android platform, including plugin lifecycle callbacks, obtaining the application context and BridgeManger, and interacting with ArkUI pages through the bridge. For details about the bridge, see [BridgePlugin](BridgePlugin.md).

> **NOTE**
>
> The initial APIs of this module are supported since API version 11. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## onRegistry

```java
public void onRegistry(PluginContext pluginContext);
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

```java
public void onUnRegistry(PluginContext pluginContext);
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

Provides the plugin context, including the application context and BridgeManager that may be required by the plugin.

> **NOTE**
>
> The initial APIs of this module are supported since API version 11. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## getContext

```java
public Context getContext(); 
```

**Description**

Obtains the context of this StageActivity.

**Parameters**

None

**Return value**

| Type   | Description                       |
| ------- | --------------------------- |
| Context | Context of the StageActivity. |

**Example**

```java
Context context = pluginContext.getContext();
```

## getBridgeManager

```java
public BridgeManager getBridgeManager();
```

**Description**

Obtains a **BridgeManager** instance.

**Parameters**

None

**Return value**

| Type         | Description               |
| ------------- | ------------------- |
| BridgeManager | **BridgeManager** instance.|

**Example**

```java
BridgeManager bridgeManager = pluginContext.getBridgeManager();
```

## getRawFilePath

```java
public String getRawFilePath(String filePath);
```

**Description**

Obtains the absolute path of the resource files in **rawfile**.

**Parameters**

| Name     | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | String | Yes  | Relative path of the resource files in the **rawfile** directory (**arkui-x/entry/resources/rawfile**).|

**Return value**

| Type  | Description                         |
| ------ | ----------------------------- |
| String | Absolute path of the resource files in **rawfile**.|

**Example**

```java
// Obtain arkui-x/entry/resources/rawfile/icon.png.
String filePath = pluginContext.getRawFilePath("icon.png");
```

## getRawFilePath

```java
String getRawFilePath(String name, String filePath);
```

**Description**

Obtains the absolute path of the resource files in **rawfile** of the specified module.

**Parameters**

| Name     | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| name     | String | Yes  | Name of the module to which **rawfile** belongs.                         |
| filePath | String | Yes  | Relative path of the resource files in the **rawfile** directory (**arkui-x/module/resources/rawfile**).|

**Return value**

| Type  | Description                         |
| ------ | ----------------------------- |
| String | Absolute path of the resource files in **rawfile**.|

**Example**

```java
// Obtain arkui-x/hsp/resources/rawfile/icon.png.
String filePath = pluginContext.getRawFilePath("hsp", "icon.png");
```



## Sample

[Sample](../../tutorial/how-to-use-arkui-x-plugin-on-android.md)
