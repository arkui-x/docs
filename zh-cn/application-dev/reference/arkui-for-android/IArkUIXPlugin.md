# IArkUIXPlugin

本模块提供Android平台端的插件的功能，包括插件的生命周期回调，获取应用的context和BridgeManger，并通过Bridge与ArkUI页面进行交互，Bridge相关参考[BridgePlugin](BridgePlugin.md)。

> 说明：
>
> 本模块首批接口从API version 11 开始支持，后续版本的新增接口，采用上角标单独标记接口的起始版本。

## onRegistry

```java
public void onRegistry(PluginContext pluginContext);
```

**描述：**

插件注册生命周期回调。

**参数：** 

| Name          | 类型          | 必填 | 描述               |
| ------------- | ------------- | ---- | ------------------ |
| PluginContext | pluginContext | 是   | 插件所需的上下文。 |

关于参数的详细说明参考[PluginContext](#plugincontext)。

**返回值：** 

无

**示例：** 

无

## onUnRegistry

```java
public void onUnRegistry(PluginContext pluginContext);
```

**描述：**

插件注销生命周期回调。

**参数：** 

| Name          | 类型          | 必填 | 描述               |
| ------------- | ------------- | ---- | ------------------ |
| PluginContext | pluginContext | 是   | 插件所需的上下文。 |

关于参数的详细说明参考[PluginContext](#plugincontext)。

**返回值：** 

无

**示例：** 

无

# PluginContext

本模块提供插件上下文，包括插件的可能需要的应用context和BridgeManager等。

> 说明：
>
> 本模块首批接口从API version 11 开始支持，后续版本的新增接口，采用上角标单独标记接口的起始版本。

## getContext

```java
public Context getContext(); 
```

**描述：**

返回当前StageActvity的context。

**参数：** 

无

**返回值：** 

| 类型    | 描述                        |
| ------- | --------------------------- |
| Context | 当前StageActivity的上下文。 |

**示例：** 

```java
Context context = pluginContext.getContext();
```

## getBridgeManager

```java
public BridgeManager getBridgeManager();
```

**描述：**

返回BridgeManager实例。

**参数：** 

无

**返回值：** 

| 类型          | 描述                |
| ------------- | ------------------- |
| BridgeManager | BridgeManager实例。 |

**示例：** 

```java
BridgeManager bridgeManager = pluginContext.getBridgeManager();
```

## getRawFilePath

```java
public String getRawFilePath(String filePath);
```

**描述：**

获取rawfile中资源文件的绝对路径。

**参数：** 

| Name     | 类型   | 必填 | 描述                                                         |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| filePath | String | 是   | rawfile资源目录下文件的（在arkui-x/entry/resources/rawfile目录下）相对路径。 |

**返回值：** 

| 类型   | 描述                          |
| ------ | ----------------------------- |
| String | rawfile中资源文件的绝对路径。 |

**示例：** 

```java
// 获取 arkui-x/entry/resources/rawfile/icon.png
String filePath = pluginContext.getRawFilePath("icon.png");
```

## getRawFilePath

```java
String getRawFilePath(String name, String filePath);
```

**描述：**

获取特定module的rawfile中资源文件的绝对路径。

**参数：** 

| Name     | 类型   | 必填 | 描述                                                         |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| name     | String | 是   | 要访问的rawfile资源所属的module名。                          |
| filePath | String | 是   | rawfile资源目录下文件的（在arkui-x/module/resources/rawfile目录下）相对路径。 |

**返回值：** 

| 类型   | 描述                          |
| ------ | ----------------------------- |
| String | rawfile中资源文件的绝对路径。 |

**示例：** 

```java
// 获取 arkui-x/hsp/resources/rawfile/icon.png
String filePath = pluginContext.getRawFilePath("hsp", "icon.png");
```



## 完整示例

[完整示例](../../tutorial/how-to-use-arkui-x-plugin-on-android.md)