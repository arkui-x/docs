# IArkUIXPlugin

本模块提供ios平台端的插件的功能，包括插件的生命周期回调，获取应用的注册相关信息和BridgePluginManger，并通过Bridge与ArkUI页面进行交互，Bridge相关参考[BridgePlugin](BridgePlugin.md)。

> 说明：
>
> 本模块首批接口从API version 11 开始支持，后续版本的新增接口，采用上角标单独标记接口的起始版本。

## onRegistry

```objective-c
- (void)onRegistry:(PluginContext *)pluginContext;
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

```objective-c
- (void)onUnRegistry:(PluginContext *)pluginContext;
```

**描述：**

插件注销生命周期回调

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

本模块提供插件上下文，包括插件的可能需要的BridgePluginManager等。

> 说明：
>
> 本模块首批接口从API version 11 开始支持，后续版本的新增接口，采用上角标单独标记接口的起始版本。

## getBridgePluginManager

```
- (BridgePluginManager *)getBridgePluginManager;
```

**描述：**

获取BridgePluginManager实例。

**参数：** 

无

**返回值：** 

| 类型                | 描述                    |
| ------------------- | ----------------------- |
| BridgePluginManager | BridgePluginManager实例 |

**示例：**

```objective-c
 myBridge = [[MyBridge alloc] initBridgePlugin:@"Bridge" bridgeType:BINARY_TYPE 
 bridgeManager:[pluginContext getBridgePluginManager]];
```

## getRawFilePath

```
(NSString *)getRawFilePath:(NSString *)filePath;
```

**描述：**

获取rawfile中资源文件的绝对路径。

**参数：** 

| Name     | 类型     | 必填 | 描述                                                         |
| -------- | -------- | ---- | ------------------------------------------------------------ |
| filePath | NSString | 是   | rawfile资源目录下文件的（在arkui-x/entry/resources/rawfile目录下）相对路径。 |

**返回值：** 

| 类型     | 描述                          |
| -------- | ----------------------------- |
| NSString | rawfile中资源文件的绝对路径。 |

**示例：** 

```objective-c
// 获取 arkui-x/entry/resources/rawfile/icon.png
NSString *filePath = [pluginContext getRawFilePath:@"icon.png"];
```

## getRawFilePath

```
(NSString *)getRawFilePath:(NSString *)name filePath:(NSString *)filePath;
```

**描述：**

获取特定module的rawfile中资源文件的绝对路径。

**参数：** 

| Name     | 类型     | 必填 | 描述                                                         |
| -------- | -------- | ---- | ------------------------------------------------------------ |
| name     | NSString | 是   | 要访问的rawfile资源所属的module名。                          |
| filePath | NSString | 是   | rawfile资源目录下文件的（在arkui-x/module/resources/rawfile目录下）相对路径。 |

**返回值：**

| 类型     | 描述                          |
| -------- | ----------------------------- |
| NSString | rawfile中资源文件的绝对路径。 |


**示例：** 

```objective-c
// 获取 arkui-x/hsp/resources/rawfile/icon.png
NSString *filePath = [pluginContext getRawFilePath:@"hsp"filePath:@"icon.png"];
```



## 完整示例

[完整示例](../../tutorial/how-to-use-arkui-x-plugin-on-ios.md)