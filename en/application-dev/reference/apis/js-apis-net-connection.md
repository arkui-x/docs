# @ohos.net.connection (Network Connection Management)

The network connection management module provides basic network management capabilities. You can obtain the default active data network or the list of all active data networks, enable or disable the airplane mode, and obtain network capability information.

> **NOTE**
> The initial APIs of this module are supported since API version 8. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```js
import connection from '@ohos.net.connection'
```

## connection.createNetConnection<sup>8+</sup>

createNetConnection(netSpecifier?: NetSpecifier, timeout?: number): NetConnection

Creates a **NetConnection** object. Currently, this API accepts only netSpecifier and timeout at their default values.

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name      | Type                         | Mandatory| Description                                                        |
| ------------ | ----------------------------- | ---- | ------------------------------------------------------------ |
| netSpecifier | [NetSpecifier](#netspecifier) | No  | Network specifier, which specifies the characteristics of a network. If this parameter is not set or is set to undefined, the default network is used. Currently, only the default value is supported.                  |
| timeout      | number                        | No  | Timeout duration for obtaining the network specified by netSpecifier. This parameter is valid only when netSpecifier is specified. The default value 0 is used if netSpecifier is undefined. Currently, only the default value is supported.|

**Return value**

| Type                           | Description                |
| ------------------------------- | -------------------- |
| [NetConnection](#netconnection) | Handle of the network specified by **netSpecifier**.|

**Example**

```js
// For the default network, you do not need to pass in parameters.
let netConnection = connection.createNetConnection()
```

## connection.hasDefaultNet<sup>8+</sup>

hasDefaultNet(callback: AsyncCallback\<boolean>): void

Checks whether the default data network is activated. This API uses an asynchronous callback to return the result. 

**Required permission**: ohos.permission.GET_NETWORK_INFO

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name  | Type                   | Mandatory| Description                                  |
| -------- | ----------------------- | ---- | -------------------------------------- |
| callback | AsyncCallback\<boolean> | Yes  | Callback used to return the result. The value **true** indicates the default data network is activated.|

**Error codes**

| ID| Error Message                       |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**Example**

```js
connection.hasDefaultNet(function (error, data) {
  console.log(JSON.stringify(error))
  console.log('data: ' + data)
})
```

## connection.hasDefaultNet<sup>8+</sup>

hasDefaultNet(): Promise\<boolean>

Checks whether the default data network is activated. This API uses a promise to return the result.

**Required permission**: ohos.permission.GET_NETWORK_INFO

**System capability**: SystemCapability.Communication.NetManager.Core

**Return value**

| Type             | Description                                           |
| ----------------- | ----------------------------------------------- |
| Promise\<boolean> | Promise used to return the result. The value **true** indicates that the default data network is activated.|

**Error codes**

| ID| Error Message                       |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**Example**

```js
connection.hasDefaultNet().then(function (data) {
  console.log('data: ' + data)
})
```

## NetConnection

Represents the network connection handle.

> **NOTE**
> When a device changes to the network connected state, the **netAvailable**, **netCapabilitiesChange**, and **netConnectionPropertiesChange** events will be triggered.
> When a device changes to the network disconnected state, the **netLost** event will be triggered.
> When a device switches from a Wi-Fi network to a cellular network, the **netLost** event will be first triggered to indicate that the Wi-Fi network is lost and then the **netAvaliable** event will be triggered to indicate that the cellular network is available.

### register<sup>8+</sup>

register(callback: AsyncCallback\<void>): void

Registers a listener for network status changes.

**Required permission**: ohos.permission.GET_NETWORK_INFO

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name  | Type                | Mandatory| Description                                                        |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<void> | Yes  | Callback used to return the result. If a listener for network status changes is registered successfully, **error** is **undefined**. Otherwise, **error** is an error object.|

**Error codes**

| ID| Error Message                       |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.             |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |
| 2101008 | The same callback exists.     |
| 2101022 | The number of requests exceeded the maximum. |

**Example**

```js
netConnection.register(function (error) {
  console.log(JSON.stringify(error))
})
```

### unregister<sup>8+</sup>

unregister(callback: AsyncCallback\<void>): void

Unregisters the listener for network status changes.

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name  | Type                | Mandatory| Description                                                        |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<void> | Yes  | Callback used to return the result. If a listener for network status changes is unregistered successfully, **error** is **undefined**. Otherwise, **error** is an error object.|

**Error codes**

| ID| Error Message                       |
| ------- | -----------------------------  |
| 201 | Permission denied.|
| 401 | Parameter error.         |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |
| 2101007 | The callback is not exists.      |

**Example**

```js
netConnection.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netAvailable')<sup>8+</sup>

on(type: 'netAvailable', callback: Callback\<NetHandle>): void

Registers a listener for **netAvailable** events.

**Model restriction**: Before you call this API, make sure tat you have called **register** to add a listener and called **unregister** API to unsubscribe from status changes of the default network.

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name  | Type                              | Mandatory| Description                                                        |
| -------- | ---------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                             | Yes  | Event type. The value is fixed to **netAvailable**.<br>**netAvailable**: event indicating that the data network is available.|
| callback | Callback\<[NetHandle](#nethandle)> | Yes  | Callback used to return the network handle.|

**Example**

```js
// Create a NetConnection object.
let netCon = connection.createNetConnection()

// Call register to register a listener.
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// Subscribe to netAvailable events. Event notifications can be received only after register is called.
netCon.on('netAvailable', function (data) {
  console.log(JSON.stringify(data))
})

// Call unregister to unregister the listener.
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netCapabilitiesChange')<sup>8+</sup>

on(type: 'netCapabilitiesChange', callback: Callback<{ netHandle: NetHandle, netCap: NetCapabilities }>): void

Registers a listener for **netCapabilitiesChange** events.

**Model restriction**: Before you call this API, make sure tat you have called **register** to add a listener and called **unregister** API to unsubscribe from status changes of the default network.

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                                        |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | Yes  | Event type. The value is fixed to **netCapabilitiesChange**.<br>**netCapabilitiesChange**: event indicating that the network capabilities have changed.|
| callback | Callback<{ netHandle: [NetHandle](#nethandle), netCap: [NetCapabilities](#netcapabilities) }> | Yes  | Callback used to return the network handle (**netHandle**) and capability information (**netCap**).|

**Example**

```js
// Create a NetConnection object.
let netCon = connection.createNetConnection()

// Call register to register a listener.
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// Subscribe to netCapabilitiesChange events. Event notifications can be received only after register is called.
netCon.on('netCapabilitiesChange', function (data) {
  console.log(JSON.stringify(data))
})

// Call unregister to unregister the listener.
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netLost')<sup>8+</sup>

on(type: 'netLost', callback: Callback\<NetHandle>): void

Registers a listener for **netLost** events.

**Model restriction**: Before you call this API, make sure tat you have called **register** to add a listener and called **unregister** API to unsubscribe from status changes of the default network.

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name  | Type                              | Mandatory| Description                                                        |
| -------- | ---------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                             | Yes  | Event type. The value is fixed to **netLost**.<br>netLost: event indicating that the network is interrupted or normally disconnected.|
| callback | Callback\<[NetHandle](#nethandle)> | Yes  | Callback used to return the network handle (**netHandle**).|

**Example**

```js
// Create a NetConnection object.
let netCon = connection.createNetConnection()

// Call register to register a listener.
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// Subscribe to netLost events. Event notifications can be received only after register is called.
netCon.on('netLost', function (data) {
  console.log(JSON.stringify(data))
})

// Call unregister to unregister the listener.
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netUnavailable')<sup>8+</sup>

on(type: 'netUnavailable', callback: Callback\<void>): void

Registers a listener for **netUnavailable** events.

**Model restriction**: Before you call this API, make sure tat you have called **register** to add a listener and called **unregister** API to unsubscribe from status changes of the default network.

**System capability**: SystemCapability.Communication.NetManager.Core

**Parameters**

| Name  | Type           | Mandatory| Description                                                        |
| -------- | --------------- | ---- | ------------------------------------------------------------ |
| type     | string          | Yes  | Event type. The value is fixed to **netUnavailable**.<br>**netUnavailable**: event indicating that the network is unavailable.|
| callback | Callback\<void> | Yes  | Callback used to return the result, which is empty.|

**Example**

```js
// Create a NetConnection object.
let netCon = connection.createNetConnection()

// Call register to register a listener.
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// Subscribe to netUnavailable events. Event notifications can be received only after register is called.
netCon.on('netUnavailable', function (data) {
  console.log(JSON.stringify(data))
})

// Call unregister to unregister the listener.
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

## NetHandle<sup>8+</sup>

Defines the handle of the data network.

Before invoking **NetHandle** APIs, call **getNetHandle** to obtain a **NetHandle** object.

**System capability**: SystemCapability.Communication.NetManager.Core

### Attributes

| Name   | Type  | Mandatory| Description                     |
| ------ | ------ | --- |------------------------- |
| netId  | number | Yes |  Network ID. The value **0** indicates no default network. Any other value must be greater than or equal to 100.|

## NetBearType<sup>8+</sup>

Enumerates network types.

**System capability**: SystemCapability.Communication.NetManager.Core

| Name        | Value  | Description       |
| --------------- | ---- | ----------- |
| BEARER_CELLULAR | 0    | Cellular network. |
| BEARER_WIFI     | 1    | Wi-Fi network.|

## NetSpecifier<sup>8+</sup>

Provides an instance that bears data network capabilities.

**System capability**: SystemCapability.Communication.NetManager.Core

| Name                    | Type                               | Mandatory | Description                                                        |
| ----------------------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| netCapabilities         | [NetCapabilities](#netcapabilities) |  Yes | Network transmission capabilities and bearer types of the data network.                               |
| bearerPrivateIdentifier | string                              |  No |  Network identifier. The identifier of a Wi-Fi network is **wifi**, and that of a cellular network is **slot0** (corresponding to SIM card 1).|

## NetCapabilities<sup>8+</sup>

Defines the network capability set.

**System capability**: SystemCapability.Communication.NetManager.Core

| Name                 | Type                               | Mandatory| Description                    |
| --------------------- | ---------------------------------- | --- | ------------------------ |
| bearerTypes           | Array\<[NetBearType](#netbeartype)> |  Yes|  Network type.              |
