# @ohos.wifiManager (WLAN)

The WLAN module provides basic wireless local area network (WLAN) functions, peer-to-peer (P2P) functions, and WLAN message notification services. It allows applications to communicate with devices over WLAN.

> **NOTE：**
>
>The initial APIs of this module are supported since API version 6. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>

## Modules to Import

```ts
import wifiManager from '@ohos.wifiManager';
```


## wifiManager.isWifiActive<sup>11+</sup>

isWifiActive(): boolean

Checks whether WLAN is enabled.

iOS: This API is not supported.

**System capability**：SystemCapability.Communication.WiFi.STA

**Return value：**

| Type        | Description                                                         |
| :---------- | :----------------------------------------------------------- |
| boolean | Returns true if WLAN is enabled; returns false otherwise.|

**Example：**

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

Obtains WLAN connection information. This API uses a promise to return the result.

**Required permissions**：ohos.permission.GET_WIFI_INFO

**System capability**：SystemCapability.Communication.WiFi.STA

**Return value：**

| Type             | Description                              |
| :------------------------------------------ | :--------------------------------- |
| Promise<`WifiLinkedInfo`> | Promise used to return the WLAN connection information. | 


## wifiManager.getLinkedInfo<sup>11+</sup>

getLinkedInfo(callback: AsyncCallback\<WifiLinkedInfo\>): void

Obtains WLAN connection information. This API uses an asynchronous callback to return the result.

**Required permissions**：ohos.permission.GET_WIFI_INFO

**System capability**：SystemCapability.Communication.WiFi.STA

**Parameters：**

| Name       | Type             | Mandatory        | Description                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| callback | AsyncCallback<`WifiLinkedInfo`> |    YES    | Callback invoked to return the result. If the operation is successful, err is 0 and data is the WLAN connection information obtained. If the operation fails, err is not 0.|

**Example：**

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

Represents the WLAN connection information.

**System capability**：SystemCapability.Communication.WiFi.STA


| Name       | Type             | Readable        | Writable        | Android       | iOS      | Description                              |
| :---------- | :---------------- | :---------- | :---------- | :----------| :----------| :--------------------------------- |
| ssid | string|    Yes    |    No    |    Yes    |    Yes    | SSID of the hotspot, in UTF-8 format. |
| bssid | string|    Yes    |    No    |    Yes    |    Yes    | BSSID of the hotspot. |
| networkId | number|    Yes    |    No    |    Yes    |    No    | Network configuration ID. System API: This is a system API. iOS not support|
| rssi| number|    Yes    |    No    |    Yes    |    No    | Hotspot RSSI, in dBm. iOS not support|
| band | number|    Yes    |    No    |    No    |    No    | Band of the WLAN AP.  Android and ios all not support|
| linkSpeed | number|    Yes    |    No    |    Yes    |    No    | link speed of the WLAN AP. iOS not support|
| frequency | number|    Yes    |    No    |    Yes    |    No    | Frequency of the WLAN AP. iOS not support|
| isHidden | boolean|    Yes    |    No    |    Yes    |    No    | Whether to hide the WLAN AP. iOS not support|
| isRestricted | boolean|    Yes    |    No    |    No    |    No    | Whether to restrict data volume at the WLAN AP. Android and ios all not support|
| macType | number|    Yes    |    No    |    No    |    No    | MAC address type. Android and ios all not support|
| macAddress | string|    Yes    |    No    |    No    |    No    | MAC address of the device. Android and ios all not support|
| ipAddress | number|    Yes    |    No    |    No    |    No    | IP address of the device that sets up the WLAN connection. Android and ios all not support|
| connState | `ConnState`|    Yes    |    No    |    No    |    No    | WLAN connection state. Android and ios all not support|


## ConnState<sup>11+</sup>

Enumerates the WLAN connection states.

**System capability**：SystemCapability.Communication.WiFi.STA


| Name       | Value             | Description                              |
| :---------- | :---------------- | :--------------------------------- |
| SCANNING | 0 | The device is scanning for available APs. |
| CONNECTING | 1 | A WLAN connection is being established. |
| AUTHENTICATING | 2 | An authentication is being performed for a WLAN connection. |
| OBTAINING_IPADDR | 3 | The IP address of the WLAN connection is being acquired. |
| CONNECTED | 4 | A WLAN connection is established. |
| DISCONNECTING | 5 | The WLAN connection is being disconnected. |
| DISCONNECTED | 6 | The WLAN connection is disconnected. |
| UNKNOWN | 7 | Failed to set up the WLAN connection. |


## wifiManager.isConnected<sup>11+</sup>

isConnected(): boolean

Checks whether WLAN is connected.

**Required permissions**：ohos.permission.GET_WIFI_INFO

**System capability**：SystemCapability.Communication.WiFi.STA

**Return value：**

| Type             | Description                              |
| :------------------------------------------ | :--------------------------------- |
| boolean| Returns true if the WLAN is connected; returns false otherwise. | 

**Example：**

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

Subscribes to WLAN state changes.

iOS: This API is not supported.

**Required permissions**：ohos.permission.GET_WIFI_INFO

**System capability**：SystemCapability.Communication.WiFi.STA

**Parameters：**

| Name       | Type             | Mandatory        | Description                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    Yes    | Event type, which has a fixed value of wifiStateChange. |
| callback | Callback<`number`> |    Yes    | Callback invoked to return the WLAN state. |

**WLAN states：**

| Value      | Description                              |
| :---------- | :--------------------------------- |
| 0 | Deactivated |
| 1 | Activated |
| 2 | Activating. iOS not support|
| 3 | Deactivating. iOS not support|


## wifiManager.off('wifiStateChange')<sup>11+</sup>

off(type: "wifiStateChange", callback?: Callback<number>): void

Unsubscribes from WLAN state changes.

iOS: This API is not supported.

**Required permissions**：ohos.permission.GET_WIFI_INFO

**System capability**：SystemCapability.Communication.WiFi.STA

**Parameters：**

| Name       | Type             | Mandatory        | Description                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    Yes    | Event type, which has a fixed value of wifiStateChange. |
| callback | Callback<`number`> |    No    | Callback to unregister. If this parameter is not specified, all callbacks registered for the specified event will be unregistered. |

**Example：**

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

Subscribes to WLAN connection state changes.

**Required permissions**：ohos.permission.GET_WIFI_INFO

**System capability**：SystemCapability.Communication.WiFi.STA

**Parameters：**

| Name       | Type             | Mandatory        | Description                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    Yes    | Event type, which has a fixed value of wifiConnectionChange.|
| callback | Callback<`number`> |    Yes    | Callback invoked to return the WLAN connection state. |

**WLAN connection states：**

| Value      | Description                              |
| :---------- | :--------------------------------- |
| 0 | Disconnected. |
| 1 | Connected. |


## wifiManager.off('wifiConnectionChange')<sup>11+</sup>

off(type: "wifiConnectionChange", callback?: Callback<number>): void

Unsubscribes from WLAN connection state changes.

**Required permissions**：ohos.permission.GET_WIFI_INFO

**System capability**：SystemCapability.Communication.WiFi.STA

**Parameters**

| Name       | Type             | Mandatory        | Description                              |
| :---------- | :------------------------------------------ | :---------- | :--------------------------------- |
| type | string |    Yes    | Event type, which has a fixed value of wifiConnectionChange. |
| callback | Callback<`number`> |    No    | Callback to unregister. If this parameter is not specified, all callbacks registered for the specified event will be unregistered. |

**Example**

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