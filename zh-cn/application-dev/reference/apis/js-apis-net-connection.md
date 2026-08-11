# @ohos.net.connection (网络连接管理)

网络连接管理提供管理网络一些基础能力，包括获取默认激活的数据网络、获取所有激活数据网络列表、开启关闭飞行模式、获取网络能力信息等功能。

> **说明：**
> 本模块首批接口从API version 8开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import connection from '@ohos.net.connection'
```

## connection.createNetConnection<sup>8+</sup>

createNetConnection(netSpecifier?: NetSpecifier, timeout?: number): NetConnection

返回一个NetConnection对象，netSpecifier指定关注的网络的各项特征；timeout是超时时间(单位是毫秒)；netSpecifier是timeout的必要条件，两者都没有则表示关注默认网络。

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

```ts
import connection from '@ohos.net.connection'

// 关注默认网络, 不需要传参
let netConnection = connection.createNetConnection()

// 关注蜂窝网络，需要传入相关网络特征，timeout参数未传入说明未使用超时时间，此时timeout为0
let netConnectionCellular = connection.createNetConnection({
  netCapabilities: {
    bearerTypes: [connection.NetBearType.BEARER_CELLULAR]
  }
})
```

## connection.hasDefaultNet<sup>8+</sup>

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

```ts
import connection from '@ohos.net.connection'
connection.hasDefaultNet().then((data: boolean) => {
  console.log('data: ' + data)
})
```

## NetConnection

网络连接的句柄。

> **说明：**
> 设备从无网络到有网络会触发netAvailable事件、netCapabilitiesChange事件和netConnectionPropertiesChange事件；
> 设备从有网络到无网络状态会触发netLost事件；
> 设备从WiFi到蜂窝会触发netLost事件（WiFi丢失）之后触发 netAvaliable事件（蜂窝可用）；

### register<sup>8+</sup>

register(callback: AsyncCallback\<void>): void

订阅指定网络状态变化的通知。

**需要权限**：ohos.permission.GET_NETWORK_INFO

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<void> | 是   | 回调函数。当订阅指定网络状态变化的通知成功，error为undefined，否则为错误对象。 |

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

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"
let netCon: connection.NetConnection = connection.createNetConnection();
netCon.register((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

### unregister<sup>8+</sup>

unregister(callback: AsyncCallback\<void>): void

取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<void> | 是   | 回调函数。当取消订阅指定网络状态变化的通知成功，error为undefined，否则为错误对象。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201 | Permission denied.|
| 401 | Parameter error.         |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |
| 2101007 | The callback is not exists.      |

**示例：**

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"
let netCon: connection.NetConnection = connection.createNetConnection();
netCon.unregister((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

### on('netAvailable')<sup>8+</sup>

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

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

// 创建NetConnection对象
let netCon: connection.NetConnection = connection.createNetConnection();

// 先使用register接口注册订阅事件
netCon.register((error: BusinessError) => {
  console.log(JSON.stringify(error))
})

// 订阅网络可用事件。调用register后，才能接收到此事件通知
netCon.on('netAvailable', (data: connection.NetHandle) => {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

### on('netBlockStatusChange')<sup>8+</sup>

on(type: 'netBlockStatusChange', callback: Callback&lt;{ netHandle: NetHandle, blocked: boolean }&gt;): void

订阅网络阻塞状态事件，使用callback方式作为异步方法。

**模型约束**：此接口调用之前需要先调用register接口，使用unregister取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 订阅事件，固定为'netBlockStatusChange'。<br/>netBlockStatusChange：网络阻塞状态事件。 |
| callback | Callback&lt;{&nbsp;netHandle:&nbsp;[NetHandle](#nethandle),&nbsp;blocked:&nbsp;boolean&nbsp;}&gt; | 是   | 回调函数，返回数据网络句柄(netHandle),及网络堵塞状态(blocked)。|

**示例：**

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

// 创建NetConnection对象
let netCon: connection.NetConnection = connection.createNetConnection();

// 先使用register接口注册订阅事件
netCon.register((error: BusinessError) => {
  console.log(JSON.stringify(error))
})

// 订阅网络可用事件。调用register后，才能接收到此事件通知
class Value {
    netHandle: NetHandle = connection.NetHandle
    blocked: boolean = false
}
netCon.on('netBlockStatusChange', (data: Value) => {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

### on('netCapabilitiesChange')<sup>8+</sup>

on(type: 'netCapabilitiesChange', callback: Callback\<NetCapabilityInfo>): void

订阅网络能力变化事件。

**模型约束**：此接口调用之前需要先调用register接口，使用unregister取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 订阅事件，固定为'netCapabilitiesChange'。<br/>netCapabilitiesChange：网络能力变化事件。 |
| callback | Callback<[NetCapabilityInfo](#netcapabilityinfo)> | 是   | 回调函数，返回数据网络句柄(netHandle)和网络的能力信息(netCap)。|

**示例：**

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

// 创建NetConnection对象
let netCon: connection.NetConnection = connection.createNetConnection();

// 先使用register接口注册订阅事件
netCon.register((error: BusinessError) => {
  console.log(JSON.stringify(error))
})

// 订阅网络可用事件。调用register后，才能接收到此事件通知
netCon.on('netAvailable', (data: connection.NetHandle) => {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

### on('netConnectionPropertiesChange')<sup>8+</sup>

on(type: 'netConnectionPropertiesChange', callback: Callback<{ netHandle: NetHandle, connectionProperties:
ConnectionProperties }>): void

订阅网络连接信息变化事件。

**模型约束**：此接口调用之前需要先调用register接口，使用unregister取消订阅默认网络状态变化的通知。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 订阅事件，固定为'netConnectionPropertiesChange'。<br/>netConnectionPropertiesChange：网络连接信息变化事件。 |
| callback | Callback<{ netHandle: [NetHandle](#nethandle), connectionProperties: [ConnectionProperties](#connectionproperties) }> | 是   | 回调函数，返回数据网络句柄(netHandle)和网络的连接信息(connectionProperties)|

**示例：**

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

// 创建NetConnection对象
let netCon: connection.NetConnection = connection.createNetConnection();

// 先使用register接口注册订阅事件
netCon.register((error: BusinessError) => {
  console.log(JSON.stringify(error))
})

class Value {
    netHandle: NetHandle = connection.NetHandle
    connectionProperties: ConnectionProperties = connection.ConnectionProperties
}

// 订阅网络可用事件。调用register后，才能接收到此事件通知
netCon.on('netConnectionPropertiesChange', (data: Value) => {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

### on('netLost')<sup>8+</sup>

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

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

// 创建NetConnection对象
let netCon: connection.NetConnection = connection.createNetConnection();

// 先使用register接口注册订阅事件
netCon.register((error: BusinessError) => {
  console.log(JSON.stringify(error))
})

// 订阅网络可用事件。调用register后，才能接收到此事件通知
netCon.on('netLost', (data: connection.NetHandle) => {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

### on('netUnavailable')<sup>8+</sup>

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

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

// 创建NetConnection对象
let netCon: connection.NetConnection = connection.createNetConnection();

// 先使用register接口注册订阅事件
netCon.register((error: BusinessError) => {
  console.log(JSON.stringify(error))
})

// 订阅网络可用事件。调用register后，才能接收到此事件通知
netCon.on('netAvailable', (data: connection.NetHandle) => {
  console.log(JSON.stringify(data))
})

// 使用unregister接口取消订阅
netCon.unregister((error: BusinessError) => {
  console.log(JSON.stringify(error))
})
```

## NetHandle<sup>8+</sup>

数据网络的句柄。

在调用NetHandle的方法之前，需要先获取NetHandle对象。

**系统能力**：SystemCapability.Communication.NetManager.Core

### 属性

| 名称    | 类型   | 必填 | 说明                      |
| ------ | ------ | --- |------------------------- |
| netId  | number | 是  |  网络ID，取值为0代表没有默认网络，其余取值必须大于等于100。 |

### bindSocket<sup>9+</sup>

bindSocket(socketParam: TCPSocket \| UDPSocket, callback: AsyncCallback\<void>): void

将TCPSocket或UDPSocket绑定到当前网络，使用callback方式作为异步方法。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名      | 类型                     | 必填 | 说明                            |
| ----------- | ------------------------ | ---- | -------------------------------|
| socketParam | [TCPSocket](js-apis-socket.md#tcpsocket) \| [UDPSocket](js-apis-socket.md#udpsocket) | 是 | 待绑定的TCPSocket或UDPSocket对象。|
| callback    | AsyncCallback\<void>      | 是   | 回调函数。当TCPSocket或UDPSocket成功绑定到当前网络，error为undefined，否则为错误对象。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 401     | Parameter error.               |
| 2100001 | Invalid parameter value.                |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```ts
import socket from "@ohos.net.socket";
import connection from '@ohos.net.connection';
import { BusinessError } from '@ohos.base';

interface Data {
  message: ArrayBuffer,
  remoteInfo: socket.SocketRemoteInfo
}

connection.getDefaultNet().then((netHandle: connection.NetHandle) => {
  let tcp = socket.constructTCPSocketInstance();
  let udp = socket.constructUDPSocketInstance();
  let socketType = "TCPSocket";
  if (socketType == "TCPSocket") {
    tcp.bind({address:"192.168.xxx.xxx",
              port:8080,
              family:1} as socket.NetAddress, (error: Error) => {
      if (error) {
        console.log('bind fail');
        return;
      }
      netHandle.bindSocket(tcp, (error: BusinessError, data: void) => {
        if (error) {
          console.log(JSON.stringify(error));
        } else {
          console.log(JSON.stringify(data));
        }
      })
    })
  } else {
    let callback: (value: Data) => void = (value: Data) => {
      console.log("on message, message:" + value.message + ", remoteInfo:" + value.remoteInfo);
    }
    udp.bind({address:"192.168.xxx.xxx",
              port:8080,
              family:1} as socket.NetAddress, (error: BusinessError) => {
      if (error) {
        console.log('bind fail');
        return;
      }
      udp.on('message', (data: Data) => {
        console.log(JSON.stringify(data))
      });
      netHandle.bindSocket(udp, (error: BusinessError, data: void) => {
        if (error) {
          console.log(JSON.stringify(error));
        } else {
          console.log(JSON.stringify(data));
        }
      })
    })
  }
})
```

### bindSocket<sup>9+</sup>

bindSocket(socketParam: TCPSocket \| UDPSocket): Promise\<void>;

将TCPSocket或UDPSockett绑定到当前网络，使用Promise方式作为异步方法。

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名          | 类型                  | 必填  | 说明                           |
| --------------- | --------------------- | ---- | ------------------------------ |
| socketParam     | [TCPSocket](js-apis-socket.md#tcpsocket) \| [UDPSocket](js-apis-socket.md#udpsocket) | 是   | 待绑定的TCPSocket或UDPSocket对象。|

**返回值：**

| 类型           | 说明                   |
| -------------- | ---------------------- |
| Promise\<void> | 无返回值的Promise对象。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 401     | Parameter error.               |
| 2100001 | Invalid parameter value.                |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```ts
import socket from "@ohos.net.socket";
import connection from '@ohos.net.connection';
import { BusinessError } from '@ohos.base';
interface Data {
  message: ArrayBuffer,
  remoteInfo: socket.SocketRemoteInfo
}

connection.getDefaultNet().then((netHandle: connection.NetHandle) => {
  let tcp = socket.constructTCPSocketInstance();
  let udp = socket.constructUDPSocketInstance();
  let socketType = "TCPSocket";
  if (socketType == "TCPSocket") {
    tcp.bind({address:"192.168.xxx.xxx",
              port:8080,
              family:1} as socket.NetAddress, (error: Error) => {
      if (error) {
        console.log('bind fail');
        return;
      }
      netHandle.bindSocket(tcp, (error: BusinessError, data: void) => {
        if (error) {
          console.log(JSON.stringify(error));
        } else {
          console.log(JSON.stringify(data));
        }
      })
    })
  } else {
    let callback: (value: Data) => void = (value: Data) => {
      console.log("on message, message:" + value.message + ", remoteInfo:" + value.remoteInfo);
    }
    udp.bind({address:"192.168.xxx.xxx",
              port:8080,
              family:1} as socket.NetAddress, (error: BusinessError) => {
    if (error) {
      console.log('bind fail');
      return;
    }
    udp.on('message', (data: Data) => {
      console.log(JSON.stringify(data))
    });
    netHandle.bindSocket(udp, (error: BusinessError, data: void) => {
      if (error) {
        console.log(JSON.stringify(error));
      } else {
        console.log(JSON.stringify(data));
      }
    })
  })
}
})
```

### getAddressesByName<sup>8+</sup>

getAddressesByName(host: string, callback: AsyncCallback\<Array\<NetAddress>>): void

使用对应网络解析主机名以获取所有IP地址，使用callback方式作为异步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| host     | string                                                       | 是   | 需要解析的主机名。                                           |
| callback | AsyncCallback\<Array\<[NetAddress<sup>24+</sup>](#netaddress24)>> | 是   | 回调函数。当使用对应网络解析主机名成功获取所有IP地址，error为undefined，data为获取到的所有IP地址；否则为错误对象。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100001 | Invalid parameter value.                |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

connection.getDefaultNet().then((netHandle: connection.NetHandle) => {
  let host = "xxxx";
  netHandle.getAddressesByName(host, (error: BusinessError, data: connection.NetAddress[]) => {
    console.log(JSON.stringify(error))
    console.log(JSON.stringify(data))
  })
})
```

### getAddressesByName<sup>8+</sup>

getAddressesByName(host: string): Promise\<Array\<NetAddress>>

使用对应网络解析主机名以获取所有IP地址，使用Promise方式作为异步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| host   | string | 是   | 需要解析的主机名。 |

**返回值：**

| 类型                                                        | 说明                          |
| ----------------------------------------------------------- | ----------------------------- |
| Promise\<Array\<[NetAddress<sup>24+</sup>](#netaddress24)>> | 以Promise形式返回所有IP地址。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100001 | Invalid parameter value.                |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```ts
import connection from '@ohos.net.connection'

connection.getDefaultNet().then((netHandle: connection.NetHandle) => {
  let host = "xxxx";
  netHandle.getAddressesByName(host).then((data: connection.NetAddress[]) => {
    console.log(JSON.stringify(data))
  })
})
```

### getAddressByName<sup>8+</sup>

getAddressByName(host: string, callback: AsyncCallback\<NetAddress>): void

使用对应网络解析主机名以获取第一个IP地址，使用callback方式作为异步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名   | 类型                                                      | 必填 | 说明                                                         |
| -------- | --------------------------------------------------------- | ---- | ------------------------------------------------------------ |
| host     | string                                                    | 是   | 需要解析的主机名。                                           |
| callback | AsyncCallback\<[NetAddress<sup>24+</sup>](#netaddress24)> | 是   | 回调函数。当使用对应网络解析主机名获取第一个IP地址成功，error为undefined，data为获取的第一个IP地址；否则为错误对象。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100001 | Invalid parameter value.                |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```ts
import connection from '@ohos.net.connection'
import { BusinessError } from "@ohos.base"

connection.getDefaultNet().then((netHandle: connection.NetHandle) => {
  let host = "xxxx";
  netHandle.getAddressByName(host, (error: BusinessError, data: connection.NetAddress) => {
    console.log(JSON.stringify(error))
    console.log(JSON.stringify(data))
  })
}) 
```

### getAddressByName<sup>8+</sup>

getAddressByName(host: string): Promise\<NetAddress>

使用对应网络解析主机名以获取第一个IP地址，使用Promise方式作为异步方法。

**需要权限**：ohos.permission.INTERNET

**系统能力**：SystemCapability.Communication.NetManager.Core

**参数：**

| 参数名 | 类型   | 必填 | 说明               |
| ------ | ------ | ---- | ------------------ |
| host   | string | 是   | 需要解析的主机名。 |

**返回值：**

| 类型                                                | 说明                            |
| --------------------------------------------------- | ------------------------------- |
| Promise\<[NetAddress<sup>24+</sup>](#netaddress24)> | 以Promise形式返回第一个IP地址。 |

**错误码：**

| 错误码ID | 错误信息                        |
| ------- | -----------------------------  |
| 201     | Permission denied.             |
| 401     | Parameter error.               |
| 2100001 | Invalid parameter value.                |
| 2100002 | Operation failed. Cannot connect to service.|
| 2100003 | System internal error.         |

**示例：**

```ts
import connection from '@ohos.net.connection'

connection.getDefaultNet().then((netHandle: connection.NetHandle) => {
  let host = "xxxx";
  netHandle.getAddressByName(host).then((data: connection.NetAddress) => {
    console.log(JSON.stringify(data))
  })
})
```

## NetBearType<sup>8+</sup>

网络类型。

**系统能力**：SystemCapability.Communication.NetManager.Core

| 名称         | 值   | 说明        |
| --------------- | ---- | ----------- |
| BEARER_CELLULAR | 0    | 蜂窝网络。  |
| BEARER_WIFI     | 1    | Wi-Fi网络。 |
| BEARER_ETHERNET | 3 | 以太网网络。 |

## HttpProxy<sup>10+</sup>

网络代理配置信息

**系统能力**：SystemCapability.Communication.NetManager.Core

| 名称          | 类型            | 只读 | 可选 | 说明                                                         |
| ------------- | --------------- | ---- | ---- | ------------------------------------------------------------ |
| host          | string          | 否   | 否   | 代理服务器主机名。<br>                                       |
| port          | number          | 否   | 否   | 主机端口。取值范围[0,65535]。<br>                            |
| exclusionList | Array\<string\> | 否   | 否   | 不使用代理的主机名列表，主机名支持域名、IP地址以及通配符形式，详细匹配规则如下：<br/>1、域名匹配规则：<br/>（1）完全匹配：代理服务器主机名只要与列表中的任意一个主机名完全相同，就可以匹配。<br/>（2）包含匹配：代理服务器主机名只要包含列表中的任意一个主机名，就可以匹配。<br/>例如，如果在主机名列表中设置了 “ample.com”，则  “ample.com”、“www.ample.com”、“ample.com:80”都会被匹配，而 “www.example.com”、“ample.com.org”则不会被匹配。<br/>2、IP地址匹配规则：代理服务器主机名只要与列表中的任意一个IP地址完全相同，就可以匹配。<br/>3、域名跟IP地址可以同时添加到列表中进行匹配。<br/>4、单个“\*”是唯一有效的通配符，当列表中只有通配符时，将与所有代理服务器主机名匹配，表示禁用代理。通配符只能单独添加，不可以与其他域名、IP地址一起添加到列表中，否则通配符将不生效。<br/>5、匹配规则不区分主机名大小写。<br/>6、匹配主机名时，不考虑http和https等协议前缀。<br> |

## NetCapabilityInfo<sup>10+</sup>

提供承载数据网络能力的实例。

**系统能力**：SystemCapability.Communication.NetManager.Core

| 名称                     | 类型                                | 必填  | 说明                                                         |
| ----------------------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| netHandle         | [NetHandle](#nethandle) |  是  | 数据网络句柄。                                |
| netCap |  [NetCapabilities](#netcapabilities)       |  否  |  存储数据网络的传输能力和承载类型。 |

## NetCapabilities<sup>8+</sup>

网络的能力集。

**系统能力**：SystemCapability.Communication.NetManager.Core

| 名称                  | 类型                                | 必填 | 说明                     |
| --------------------- | ---------------------------------- | --- | ------------------------ |
| linkUpBandwidthKbps   | number                             |  否 |  上行（设备到网络）带宽，0表示无法评估当前网络带宽。  |
| linkDownBandwidthKbps | number                             |  否 |  下行（网络到设备）带宽，0表示无法评估当前网络带宽。   |
| networkCap            | Array\<[NetCap](#netcap)>           |  否 |  网络具体能力。           |
| bearerTypes           | Array\<[NetBearType](#netbeartype)> |  是 |  网络类型。               |

## NetAddress<sup>24+</sup>

网络地址。

**系统能力**：SystemCapability.Communication.NetManager.Core

**支持平台**： Android 、iOS

| 名称 | 类型 | 必填 | 说明 |
| ------- | ------ | -- |------------------------------ |
| address<sup>24+</sup> | string | 是 |地址。 |
| family<sup>24+</sup> | number | 否 |IPv4 = 1，IPv6 = 2，默认IPv4。 |
| port<sup>24+</sup> | number | 否 |端口，取值范围\[0, 65535]。 |
