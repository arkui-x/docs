# @ohos.net.connection (网络连接管理)

网络连接管理提供管理网络一些基础能力，包括获取默认激活的数据网络、监听网络变化等功能。

> **说明：**
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import connection from '@ohos.net.connection'
```

## connection.createNetConnection<sup>10+</sup>

createNetConnection(netSpecifier?: NetSpecifier, timeout?: number): NetConnection

返回一个NetConnection对象，目前只支持netSpecifier为默认网络和timeout为0。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名       | 类型                          | 必填 | 说明                                                         |
| ------------ | ----------------------------- | ---- | ------------------------------------------------------------ |
| netSpecifier | [NetSpecifier](#netspecifier) | 否   | 指定网络的各项特征，不指定或为undefined时关注默认网络。                   |
| timeout      | number                        | 否   | 获取netSpecifier指定的网络时的超时时间，仅netSpecifier存在时生效，undefined时默认值为0。 |

**返回值：**

| 类型                            | 说明                 |
| ------------------------------- | -------------------- |
| [NetConnection](#netconnection) | 所关注的网络的句柄。 |

**示例：**

```js
// 关注默认网络, 不需要传参
let netConnection = connection.createNetConnection()

// 关注蜂窝网络，需要传入相关网络特征，timeout参数未传入说明未使用超时时间，此时timeout为0
let netConnectionCellular = connection.createNetConnection({
  netCapabilities: {
    bearerTypes: [connection.NetBearType.BEARER_CELLULAR]
  }
})
```

## connection.hasDefaultNet<sup>10+</sup>

hasDefaultNet(callback: AsyncCallback\<boolean>): void

检查默认数据网络是否被激活，使用callback方式作为异步方法。如果有默认数据网路，可以使用[getDefaultNet](#connectiongetdefaultnet)去获取。

**需要权限**：ohos.permission.GET_NETWORK_INFO

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                   |
| -------- | ----------------------- | ---- | -------------------------------------- |
| callback | AsyncCallback\<boolean> | 是   | 回调函数。默认数据网络被激活返回true。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```js
connection.hasDefaultNet(function (error, data) {
  console.log(JSON.stringify(error))
  console.log('data: ' + data)
})
```

## connection.hasDefaultNet<sup>10+</sup>

hasDefaultNet(): Promise\<boolean>

检查默认数据网络是否被激活，使用Promise方式作为异步方法。如果有默认数据网路，可以使用[getDefaultNet](#connectiongetdefaultnet)去获取。

**需要权限**：ohos.permission.GET_NETWORK_INFO

**系统能力**：SystemCapability.Communication.NetManager.Core

**返回值：**

| 类型              | 说明                                            |
| ----------------- | ----------------------------------------------- |
| Promise\<boolean> | 以Promise形式返回，默认数据网络被激活返回true。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```js
connection.hasDefaultNet().then(function (data) {
  console.log('data: ' + data)
})
```

## NetConnection

网络连接的句柄。

> **说明：**
> 设备从无网络到有网络会触发netAvailable事件、netCapabilitiesChange事件和netConnectionPropertiesChange事件；
> 设备从有网络到无网络状态会触发netLost事件；
> 设备从WiFi到蜂窝会触发netLost事件（WiFi丢失）之后触发 netAvaliable事件（蜂窝可用）；

### register<sup>10+</sup>

register(callback: AsyncCallback\<void>): void

订阅指定网络状态变化的通知。

**需要权限**：ohos.permission.GET_NETWORK_INFO

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                 | 必填 | 说明       |
| -------- | -------------------- | ---- | ---------- |
| callback | AsyncCallback\<void> | 是   | 回调函数。当订阅指定网络状态变化的通知成功，err为undefined，否则为错误对象。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.             |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |
| 2101008 | The same callback exists.     |
| 2101022 | The number of requests exceeded the maximum. |

**示例：**

```js
netConnection.register(function (error) {
  console.log(JSON.stringify(error))
})
```

### unregister<sup>10+</sup>

unregister(callback: AsyncCallback\<void>): void

取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                 | 必填 | 说明       |
| -------- | -------------------- | ---- | ---------- |
| callback | AsyncCallback\<void> | 是   | 回调函数。当取消订阅指定网络状态变化的通知成功，err为undefined，否则为错误对象。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201 | Permission denied.|
| 401 | Parameter error.         |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |
| 2101007 | The callback is not exists.      |

**示例：**

```js
netConnection.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netAvailable')<sup>10+</sup>

on(type: 'netAvailable', callback: Callback\<NetHandle>): void

订阅网络可用事件。

**模型约束**：此接口调用之前需要先调用register接口，使用unregister取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                               | 必填 | 说明                                                         |
| -------- | ---------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                             | 是   | 订阅事件，固定为'netAvailable'。<br>netAvailable：数据网络可用事件。 |
| callback | Callback\<[NetHandle](#nethandle)> | 是   | 回调函数，返回数据网络句柄。|

**示例：**

```js
// 创建NetConnection对象
let netCon = connection.createNetConnection()

// 先使用register接口注册订阅事件
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// 订阅网络可用事件。调用register后，才能接收到此事件通知
netCon.on('netAvailable', function (data) {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netCapabilitiesChange')<sup>10+</sup>

on(type: 'netCapabilitiesChange', callback: Callback<{ netHandle: NetHandle, netCap: NetCapabilities }>): void

订阅网络能力变化事件。

**模型约束**：此接口调用之前需要先调用register接口，使用unregister取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 订阅事件，固定为'netCapabilitiesChange'。<br/>netCapabilitiesChange：网络能力变化事件。 |
| callback | Callback<{ netHandle: [NetHandle](#nethandle), netCap: [NetCapabilities](#netcapabilities) }> | 是   | 回调函数，返回数据网络句柄(netHandle)和网络的能力信息(netCap)。|

**示例：**

```js
// 创建NetConnection对象
let netCon = connection.createNetConnection()

// 先使用register接口注册订阅事件
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// 订阅网络能力变化事件。调用register后，才能接收到此事件通知
netCon.on('netCapabilitiesChange', function (data) {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netLost')<sup>10+</sup>

on(type: 'netLost', callback: Callback\<NetHandle>): void

订阅网络丢失事件。

**模型约束**：此接口调用之前需要先调用register接口，使用unregister取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                               | 必填 | 说明                                                         |
| -------- | ---------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                             | 是   | 订阅事件，固定为'netLost'。<br/>netLost：网络严重中断或正常断开事件。 |
| callback | Callback\<[NetHandle](#nethandle)> | 是   | 回调函数，数据网络句柄(netHandle)|

**示例：**

```js
// 创建NetConnection对象
let netCon = connection.createNetConnection()

// 先使用register接口注册订阅事件
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// 订阅网络丢失事件。调用register后，才能接收到此事件通知
netCon.on('netLost', function (data) {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

### on('netUnavailable')<sup>10+</sup>

on(type: 'netUnavailable', callback: Callback\<void>): void

订阅网络不可用事件。

**模型约束**：此接口调用之前需要先调用register接口，使用unregister取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型            | 必填 | 说明                                                         |
| -------- | --------------- | ---- | ------------------------------------------------------------ |
| type     | string          | 是   | 订阅事件，固定为'netUnavailable'。<br/>netUnavailable：网络不可用事件。 |
| callback | Callback\<void> | 是   | 回调函数，无返回结果。|

**示例：**

```js
// 创建NetConnection对象
let netCon = connection.createNetConnection()

// 先使用register接口注册订阅事件
netCon.register(function (error) {
  console.log(JSON.stringify(error))
})

// 订阅网络不可用事件。调用register后，才能接收到此事件通知
netCon.on('netUnavailable', function (data) {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister(function (error) {
  console.log(JSON.stringify(error))
})
```

## NetHandle<sup>10+</sup>

数据网络的句柄。

在调用NetHandle的方法之前，需要先获取NetHandle对象。

**系统能力**：SystemCapability.Communication.NetManager.Core

### 属性

| 名称    | 类型   | 必填 | 说明                      |
| ------ | ------ | --- |------------------------- |
| netId  | number | 是  |  网络ID，取值为0代表没有默认网络，其余取值必须大于等于100。 |

## NetBearType<sup>10+</sup>

网络类型。

**系统能力**：SystemCapability.Communication.NetManager.Core

| 名称         | 值   | 说明        |
| --------------- | ---- | ----------- |
| BEARER_CELLULAR | 0    | 蜂窝网络。  |
| BEARER_WIFI     | 1    | Wi-Fi网络。 |

## NetCapabilities<sup>10+</sup>

网络的能力集。

**系统能力**：SystemCapability.Communication.NetManager.Core

| 名称                  | 类型                                | 必填 | 说明                     |
| --------------------- | ---------------------------------- | --- | ------------------------ |
| bearerTypes           | Array\<[NetBearType](#netbeartype)> |  是 |  网络类型。               |
