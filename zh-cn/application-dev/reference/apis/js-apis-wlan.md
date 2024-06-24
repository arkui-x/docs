# @ohos.wifiManager (WLAN)

该模块主要提供WLAN基础功能的相应服务，让应用可以通过WLAN和其他设备互联互通。

> **说明：**
>
>本模块首批接口从API version 6开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>

## 导入模块

```ts
import wifiManager from '@ohos.wifiManager';
```


## wifiManager.isWifiActive<sup>11+</sup>

isWifiActive(): boolean

查询WLAN是否已使能。

iOS不支持

**系统能力**：SystemCapability.Communication.WiFi.STA

**返回值：**

| 类型        | 说明                                                         |
| :---------- | :----------------------------------------------------------- |
| boolean | true:已使能， false:未使能。 |

**示例：**

```ts
    import wifiManager from '@ohos.wifiManager';

	try {
		let isWifiActive = wifiManager.isWifiActive();
		console.info("isWifiActive:" + isWifiActive);
	}catch(error){
		console.error("failed:" + JSON.stringify(error));
	}
```

## wifiManager.getLinkedInfo<sup>11+</sup>

getLinkedInfo(): Promise\<WifiLinkedInfo\>

获取WLAN连接信息，使用Promise异步回调。

**需要权限**：ohos.permission.GET_WIFI_INFO

**系统能力**：SystemCapability.Communication.WiFi.STA

**返回值：**

| 类型             | 说明                              |
| :------------------------------------------ | :--------------------------------- |
| Promise<`WifiLinkedInfo`> | Promise对象。表示WLAN连接信息。 | 


## wifiManager.getLinkedInfo<sup>11+</sup>

getLinkedInfo(callback: AsyncCallback\<WifiLinkedInfo\>): void

获取WLAN连接信息，使用callback异步回调。

**需要权限**：ohos.permission.GET_WIFI_INFO

**系统能力**：SystemCapability.Communication.WiFi.STA

**参数：**

| 参数名       | 类型             | 必填        | 说明                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| callback | AsyncCallback<`WifiLinkedInfo`> |    是    | 回调函数。当获取成功时，err为0，data表示WLAN连接信息。如果error为非0，表示处理出现错误。 |

**示例：**

```ts
import wifiManager from '@ohos.wifiManager';

wifiManager.getLinkedInfo((err, data) => {
    if (err) {
        console.error("get linked info error");
        return;
    }
    console.info("get wifi linked info: " + JSON.stringify(data));
});

wifiManager.getLinkedInfo().then(data => {
    console.info("get wifi linked info: " + JSON.stringify(data));
}).catch(error => {
    console.info("get linked info error");
});
```


## WifiLinkedInfo<sup>11+</sup>

提供WLAN连接的相关信息。

**系统能力**：SystemCapability.Communication.WiFi.STA


| 名称       | 类型             | 可读        | 可写        | Android       | iOS      | 说明                              |
| :---------- | :---------------- | :---------- | :---------- | :----------| :----------| :--------------------------------- |
| ssid | string|    是    |    否    |    是    |    是    | 热点的SSID，编码格式为UTF-8。 |
| bssid | string|    是    |    否    |    是    |    是    | 热点的BSSID。 |
| networkId | number|    是    |    否    |    是    |    否    | 网络配置ID。系统接口： 此接口为系统接口。 仅Android支持|
| rssi| number|    是    |    否    |    是    |    否    | 热点的信号强度(dBm)。 iOS不支持|
| band | number|    是    |    否    |    否    |    否    | WLAN接入点的频段。 不支持跨平台|
| linkSpeed | number|    是    |    否    |    是    |    否    | WLAN接入点的速度。 iOS不支持|
| frequency | number|    是    |    否    |    是    |    否    | WLAN接入点的频率。 iOS不支持|
| isHidden | boolean|    是    |    否    |    是    |    否    | WLAN接入点是否是隐藏网络。iOS不支持|
| isRestricted | boolean|    是    |    否    |    否    |    否    | WLAN接入点是否限制数据量。 不支持跨平台|
| macType | number|    是    |    否    |    否    |    否    | MAC地址类型。 不支持跨平台|
| macAddress | string|    是    |    否    |    否    |    否    | 设备的MAC地址。 不支持跨平台|
| ipAddress | number|    是    |    否    |    否    |    否    | WLAN连接的IP地址。 不支持跨平台|
| connState | `ConnState`|    是    |    否    |    否    |    否    | WLAN连接状态。 不支持跨平台|


## ConnState<sup>11+</sup>

表示WLAN连接状态的枚举。

**系统能力**：SystemCapability.Communication.WiFi.STA


| 名称       | 值             | 说明                              |
| :---------- | :---------------- | :--------------------------------- |
| SCANNING | 0 | 设备正在搜索可用的AP。 |
| CONNECTING | 1 | 正在建立WLAN连接。 |
| AUTHENTICATING | 2 | WLAN连接正在认证中。 |
| OBTAINING_IPADDR | 3 | 正在获取WLAN连接的IP地址。 |
| CONNECTED | 4 | WLAN连接已建立。 |
| DISCONNECTING | 5 | WLAN连接正在断开。 |
| DISCONNECTED | 6 | WLAN连接已断开。 |
| UNKNOWN | 7 | WLAN连接建立失败。 |


## wifiManager.isConnected<sup>11+</sup>

isConnected(): boolean

查询WLAN是否已连接。

**需要权限**：ohos.permission.GET_WIFI_INFO

**系统能力**：SystemCapability.Communication.WiFi.STA

**返回值：**

| 类型             | 说明                              |
| :------------------------------------------ | :--------------------------------- |
| boolean| true:已连接， false:未连接。 | 

**示例：**

```ts
	import wifiManager from '@ohos.wifiManager';

	try {
		let ret = wifiManager.isConnected();
		console.info("isConnected:" + ret);
	}catch(error){
		console.error("failed:" + JSON.stringify(error));
	}
```


## wifiManager.on('wifiStateChange')<sup>11+</sup>

on(type: "wifiStateChange", callback: Callback<number>): void

注册WLAN状态改变事件。

iOS不支持

**需要权限**：ohos.permission.GET_WIFI_INFO

**系统能力**：SystemCapability.Communication.WiFi.STA

**参数：**

| 参数名       | 类型             | 必填        | 说明                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    是    | 固定填"wifiStateChange"字符串。 |
| callback | Callback<`number`> |    是    | 状态改变回调函数。 |

**状态改变事件的枚举：**

| 枚举值      | 说明                              |
| :---------- | :--------------------------------- |
| 0 | 未激活。 |
| 1 | 已激活。 |
| 2 | 激活中。iOS不支持 |
| 3 | 去激活中。iOS不支持 |


## wifiManager.off('wifiStateChange')<sup>11+</sup>

off(type: "wifiStateChange", callback?: Callback<number>): void

取消注册WLAN状态改变事件。

iOS不支持

**需要权限**：ohos.permission.GET_WIFI_INFO

**系统能力**：SystemCapability.Communication.WiFi.STA

**参数：**

| 参数名       | 类型             | 必填        | 说明                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    是    | 固定填"wifiStateChange"字符串。 |
| callback | Callback<`number`> |    否    | 状态改变回调函数。如果callback不填，将去注册该事件关联的所有回调函数。 |

**示例：**

```ts
import wifiManager from '@ohos.wifiManager';

var recvPowerNotifyFunc = result => {
    console.info("Receive power state change event: " + result);
}

// Register event
wifiManager.on("wifiStateChange", recvPowerNotifyFunc);

// Unregister event
wifiManager.off("wifiStateChange", recvPowerNotifyFunc);
```


## wifiManager.on('wifiConnectionChange')<sup>11+</sup>

on(type: "wifiConnectionChange", callback: Callback<number>): void

注册WLAN连接状态改变事件。

**需要权限**：ohos.permission.GET_WIFI_INFO

**系统能力**：SystemCapability.Communication.WiFi.STA

**参数：**

| 参数名       | 类型             | 必填        | 说明                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    是    | 固定填"wifiConnectionChange"字符串。|
| callback | Callback<`number`> |    是    | 状态改变回调函数。 |

**连接状态改变事件的枚举：**

| 枚举值      | 说明                              |
| :---------- | :--------------------------------- |
| 0 | 已断开。 |
| 1 | 已连接。 |


## wifiManager.off('wifiConnectionChange')<sup>11+</sup>

off(type: "wifiConnectionChange", callback?: Callback<number>): void

取消注册WLAN连接状态改变事件。

**需要权限**：ohos.permission.GET_WIFI_INFO

**系统能力**：SystemCapability.Communication.WiFi.STA

**参数：**

| 参数名       | 类型             | 必填        | 说明                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    是    | 固定填"wifiConnectionChange"字符串。 |
| callback | Callback<`number`> |    否    | 状态改变回调函数。如果callback不填，将去注册该事件关联的所有回调函数。 |

**示例：**

```ts
  import wifiManager from '@ohos.wifiManager';
  
  let recvWifiConnectionChangeFunc = (result:number) => {
      console.info("Receive wifi connection change event: " + result);
  }
  
  // Register event
  wifiManager.on("wifiConnectionChange", recvWifiConnectionChangeFunc);
  
  // Unregister event
  wifiManager.off("wifiConnectionChange", recvWifiConnectionChangeFunc);
```