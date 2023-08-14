# @ohos.net.socket (Socket Connection)

The **socket** module implements data transfer over TCPSocket, UDPSocket, WebSocket, and TLSSocket connections.

> **NOTE**
>
> The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```js
import socket from '@ohos.net.socket';
```

## socket.constructUDPSocketInstance<sup>7+</sup>

constructUDPSocketInstance(): UDPSocket

Creates a **UDPSocket** object.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                              | Description                   |
| :--------------------------------- | :---------------------- |
| [UDPSocket](#udpsocket) | **UDPSocket** object.|

**Example**

```js
let udp = socket.constructUDPSocketInstance();
```

## UDPSocket<sup>7+</sup>

Defines a **UDPSocket** connection. Before invoking UDPSocket APIs, you need to call [socket.constructUDPSocketInstance](#socketconstructudpsocketinstance) to create a **UDPSocket** object.

### bind<sup>7+</sup>

bind(address: NetAddress, callback: AsyncCallback\<void\>): void

Binds the IP address and port number. The port number can be specified or randomly allocated by the system. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                              | Mandatory| Description                                                  |
| -------- | ---------------------------------- | ---- | ------------------------------------------------------ |
| address  | [NetAddress](#netaddress) | Yes  | Destination address. For details, see [NetAddress](#netaddress).|
| callback | AsyncCallback\<void\>              | Yes  | Callback used to return the result.                                            |

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
udp.bind({address: '192.168.xx.xxx', port: xxxx, family: 1}, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
})
```

### bind<sup>7+</sup>

bind(address: NetAddress): Promise\<void\>

Binds the IP address and port number. The port number can be specified or randomly allocated by the system. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                              | Mandatory| Description                                                  |
| ------- | ---------------------------------- | ---- | ------------------------------------------------------ |
| address | [NetAddress](#netaddress) | Yes  | Destination address. For details, see [NetAddress](#netaddress).|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Return value**

| Type           | Description                                      |
| :-------------- | :----------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let promise = udp.bind({ address: '192.168.xx.xxx', port: 8080, family: 1 });
promise.then(() => {
  console.log('bind success');
}).catch(err => {
  console.log('bind fail');
});
```

### send<sup>7+</sup>

send(options: UDPSendOptions, callback: AsyncCallback\<void\>): void

Sends data over a UDPSocket connection. This API uses an asynchronous callback to return the result.

Before sending data, call [UDPSocket.bind()](#bind) to bind the IP address and port.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                    | Mandatory| Description                                                        |
| -------- | ---------------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [UDPSendOptions](#udpsendoptions) | Yes  | Parameters for sending data over the UDPSocket connection. For details, see [UDPSendOptions](#udpsendoptions).|
| callback | AsyncCallback\<void\>                    | Yes  | Callback used to return the result.                                                  |

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
udp.send({
  data: 'Hello, server!',
  address: {
    address: '192.168.xx.xxx',
    port: xxxx,
    family: 1
  }
}, err => {
  if (err) {
    console.log('send fail');
    return;
  }
  console.log('send success');
})
```

### send<sup>7+</sup>

send(options: UDPSendOptions): Promise\<void\>

Sends data over a UDPSocket connection. This API uses a promise to return the result.

Before sending data, call [UDPSocket.bind()](#bind) to bind the IP address and port.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                                    | Mandatory| Description                                                        |
| ------- | ---------------------------------------- | ---- | ------------------------------------------------------------ |
| options | [UDPSendOptions](#udpsendoptions) | Yes  | Parameters for sending data over the UDPSocket connection. For details, see [UDPSendOptions](#udpsendoptions).|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Return value**

| Type           | Description                                          |
| :-------------- | :--------------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let promise = udp.send({
  data: 'Hello, server!',
  address: {
    address: '192.168.xx.xxx',
    port: xxxx,
    family: 1
  }
});
promise.then(() => {
  console.log('send success');
}).catch(err => {
  console.log('send fail');
});
```

### close<sup>7+</sup>

close(callback: AsyncCallback\<void\>): void

Closes a UDPSocket connection. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                 | Mandatory| Description      |
| -------- | --------------------- | ---- | ---------- |
| callback | AsyncCallback\<void\> | Yes  | Callback used to return the result.|

**Example**

```js
let udp = socket.constructUDPSocketInstance();
udp.close(err => {
  if (err) {
    console.log('close fail');
    return;
  }
  console.log('close success');
})
```

### close<sup>7+</sup>

close(): Promise\<void\>

Closes a UDPSocket connection. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type           | Description                                      |
| :-------------- | :----------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let promise = udp.close();
promise.then(() => {
  console.log('close success');
}).catch(err => {
  console.log('close fail');
});
```

### getState<sup>7+</sup>

getState(callback: AsyncCallback\<SocketStateBase\>): void

Obtains the status of the UDPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **bind** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                  | Mandatory| Description      |
| -------- | ------------------------------------------------------ | ---- | ---------- |
| callback | AsyncCallback<[SocketStateBase](#socketstatebase)> | Yes  | Callback used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 201     | Permission denied.      |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
udp.bind({address: '192.168.xx.xxx', port: xxxx, family: 1}, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
  udp.getState((err, data) => {
    if (err) {
      console.log('getState fail');
      return;
    }
    console.log('getState success:' + JSON.stringify(data));
  })
})
```

### getState<sup>7+</sup>

getState(): Promise\<SocketStateBase\>

Obtains the status of the UDPSocket connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **bind** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                                            | Description                                      |
| :----------------------------------------------- | :----------------------------------------- |
| Promise\<[SocketStateBase](#socketstatebase)\> | Promise used to return the result.|

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let promise = udp.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promise.then(err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
  let promise = udp.getState();
  promise.then(data => {
    console.log('getState success:' + JSON.stringify(data));
  }).catch(err => {
    console.log('getState fail');
  });
});
```

### setExtraOptions<sup>7+</sup>

setExtraOptions(options: UDPExtraOptions, callback: AsyncCallback\<void\>): void

Sets other attributes of the UDPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **bind** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                    | Mandatory| Description                                                        |
| -------- | ---------------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [UDPExtraOptions](#udpextraoptions) | Yes  | Other properties of the UDPSocket connection. For details, see [UDPExtraOptions](#udpextraoptions).|
| callback | AsyncCallback\<void\>                    | Yes  | Callback used to return the result.                                                  |

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
udp.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
  udp.setExtraOptions({
    receiveBufferSize: 1000,
    sendBufferSize: 1000,
    reuseAddress: false,
    socketTimeout: 6000,
    broadcast: true
  }, err => {
    if (err) {
      console.log('setExtraOptions fail');
      return;
    }
    console.log('setExtraOptions success');
  })
})
```

### setExtraOptions<sup>7+</sup>

setExtraOptions(options: UDPExtraOptions): Promise\<void\>

Sets other attributes of the UDPSocket connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **bind** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                                    | Mandatory| Description                                                        |
| ------- | ---------------------------------------- | ---- | ------------------------------------------------------------ |
| options | [UDPExtraOptions](#udpextraoptions) | Yes  | Other properties of the UDPSocket connection. For details, see [UDPExtraOptions](#udpextraoptions).|

**Return value**

| Type           | Description                                                |
| :-------------- | :--------------------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let promise = udp.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promise.then(() => {
  console.log('bind success');
  let promise1 = udp.setExtraOptions({
    receiveBufferSize: 1000,
    sendBufferSize: 1000,
    reuseAddress: false,
    socketTimeout: 6000,
    broadcast: true
  });
  promise1.then(() => {
    console.log('setExtraOptions success');
  }).catch(err => {
    console.log('setExtraOptions fail');
  });
}).catch(err => {
  console.log('bind fail');
});
```

### on('message')<sup>7+</sup>

on(type: 'message', callback: Callback\<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}\>): void

Enables listening for message receiving events of the UDPSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback\<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo)}\> | Yes  | Callback used to return the result.                               |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let messageView = '';
udp.on('message', value => {
  for (var i = 0; i < value.message.length; i++) {
    let messages = value.message[i]
    let message = String.fromCharCode(messages);
    messageView += message;
  }
  console.log('on message message: ' + JSON.stringify(messageView));
  console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
});
```

### off('message')<sup>7+</sup>

off(type: 'message', callback?: Callback\<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}\>): void

Disables listening for message receiving events of the UDPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo)}> | No  | Callback used to return the result.                               |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let messageView = '';
let callback = value => {
  for (var i = 0; i < value.message.length; i++) {
    let messages = value.message[i]
    let message = String.fromCharCode(messages);
    messageView += message;
  }
  console.log('on message message: ' + JSON.stringify(messageView));
  console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
}
udp.on('message', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
udp.off('message', callback);
udp.off('message');
```

### on('listening' | 'close')<sup>7+</sup>

on(type: 'listening' | 'close', callback: Callback\<void\>): void

Enables listening for data packet message events or close events of the UDPSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                                                        |
| -------- | ---------------- | ---- | ------------------------------------------------------------ |
| type     | string           | Yes  | Type of the event to subscribe to.<br>- **listening**: data packet message event<br>- **close**: close event|
| callback | Callback\<void\> | Yes  | Callback used to return the result.                                                  |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
udp.on('listening', () => {
  console.log("on listening success");
});
udp.on('close', () => {
  console.log("on close success");
});
```

### off('listening' | 'close')<sup>7+</sup>

off(type: 'listening' | 'close', callback?: Callback\<void\>): void

Disables listening for data packet message events or close events of the UDPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                                                        |
| -------- | ---------------- | ---- | ------------------------------------------------------------ |
| type     | string           | Yes  | Type of the event to subscribe to.<br>- **listening**: data packet message event<br>- **close**: close event|
| callback | Callback\<void\> | No  | Callback used to return the result.                                                  |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let callback1 = () => {
  console.log("on listening, success");
}
udp.on('listening', callback1);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
udp.off('listening', callback1);
udp.off('listening');
let callback2 = () => {
  console.log("on close, success");
}
udp.on('close', callback2);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
udp.off('close', callback2);
udp.off('close');
```

### on('error')<sup>7+</sup>

on(type: 'error', callback: ErrorCallback): void

Enables listening for error events of the UDPSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | Yes  | Callback used to return the result.                          |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
udp.on('error', err => {
  console.log("on error, err:" + JSON.stringify(err))
});
```

### off('error')<sup>7+</sup>

off(type: 'error', callback?: ErrorCallback): void

Disables listening for error events of the UDPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | No  | Callback used to return the result.                          |

**Example**

```js
let udp = socket.constructUDPSocketInstance();
let callback = err => {
  console.log("on error, err:" + JSON.stringify(err));
}
udp.on('error', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
udp.off('error', callback);
udp.off('error');
```

## NetAddress<sup>7+</sup>

Defines the destination address.

**System capability**: SystemCapability.Communication.NetStack

| Name | Type  | Mandatory| Description                                                        |
| ------- | ------ | ---- | ------------------------------------------------------------ |
| address | string | Yes  | Bound IP address.                                          |
| port    | number | No  | Port number. The value ranges from **0** to **65535**. If this parameter is not specified, the system randomly allocates a port.          |
| family  | number | No  | Network protocol type.<br>- **1**: IPv4<br>- **2**: IPv6<br>The default value is **1**.|

## UDPSendOptions<sup>7+</sup>

Defines the parameters for sending data over the UDPSocket connection.

**System capability**: SystemCapability.Communication.NetStack

| Name | Type                              | Mandatory| Description          |
| ------- | ---------------------------------- | ---- | -------------- |
| data    | string \| ArrayBuffer<sup>7+</sup>                          | Yes  | Data to send.  |
| address | [NetAddress](#netaddress) | Yes  | Destination address.|

## UDPExtraOptions<sup>7+</sup>

Defines other properties of the UDPSocket connection.

**System capability**: SystemCapability.Communication.NetStack

| Name           | Type   | Mandatory| Description                            |
| ----------------- | ------- | ---- | -------------------------------- |
| broadcast         | boolean | No  | Whether to send broadcast messages. The default value is **false**. |
| receiveBufferSize | number  | No  | Size of the receive buffer, in bytes. The default value is **0**.  |
| sendBufferSize    | number  | No  | Size of the send buffer, in bytes. The default value is **0**.  |
| reuseAddress      | boolean | No  | Whether to reuse addresses. The default value is **false**.     |
| socketTimeout     | number  | No  | Timeout duration of the UDPSocket connection, in ms. The default value is **0**.|

## SocketStateBase<sup>7+</sup>

Defines the status of the socket connection.

**System capability**: SystemCapability.Communication.NetStack

| Name     | Type   | Mandatory| Description      |
| ----------- | ------- | ---- | ---------- |
| isBound     | boolean | Yes  | Whether the connection is in the bound state.|
| isClose     | boolean | Yes  | Whether the connection is in the closed state.|
| isConnected | boolean | Yes  | Whether the connection is in the connected state.|

## SocketRemoteInfo<sup>7+</sup>

Defines information about the socket connection.

**System capability**: SystemCapability.Communication.NetStack

| Name | Type  | Mandatory| Description                                                        |
| ------- | ------ | ---- | ------------------------------------------------------------ |
| address | string | Yes  | Bound IP address.                                          |
| family  | string | Yes  | Network protocol type.<br>- IPv4<br>- IPv6<br>The default value is **IPv4**.|
| port    | number | Yes  | Port number. The value ranges from **0** to **65535**.                                       |
| size    | number | Yes  | Length of the server response message, in bytes.                                  |

## Description of UDP Error Codes

The UDP error code mapping is in the format of 2301000 + Linux kernel error code.

For details about error codes, see [Socket Error Codes](../errorcodes/errorcode-net-socket.md).

## socket.constructTCPSocketInstance<sup>7+</sup>

constructTCPSocketInstance(): TCPSocket

Creates a **TCPSocket** object.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                              | Description                   |
  | :--------------------------------- | :---------------------- |
| [TCPSocket](#tcpsocket) | **TCPSocket** object.|

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
```

## TCPSocket<sup>7+</sup>

Defines a TCPSocket connection. Before invoking TCPSocket APIs, you need to call [socket.constructTCPSocketInstance](#socketconstructtcpsocketinstance) to create a **TCPSocket** object.

### bind<sup>7+</sup>

bind(address: NetAddress, callback: AsyncCallback\<void\>): void

Binds the IP address and port number. The port number can be specified or randomly allocated by the system. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                              | Mandatory| Description                                                  |
| -------- | ---------------------------------- | ---- | ------------------------------------------------------ |
| address  | [NetAddress](#netaddress) | Yes  | Destination address. For details, see [NetAddress](#netaddress).|
| callback | AsyncCallback\<void\>              | Yes  | Callback used to return the result.                                            |

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
tcp.bind({address: '192.168.xx.xxx', port: xxxx, family: 1}, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
})
```

### bind<sup>7+</sup>

bind(address: NetAddress): Promise\<void\>

Binds the IP address and port number. The port number can be specified or randomly allocated by the system. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                              | Mandatory| Description                                                  |
| ------- | ---------------------------------- | ---- | ------------------------------------------------------ |
| address | [NetAddress](#netaddress) | Yes  | Destination address. For details, see [NetAddress](#netaddress).|

**Return value**

| Type           | Description                                                    |
| :-------------- | :------------------------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise = tcp.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promise.then(() => {
  console.log('bind success');
}).catch(err => {
  console.log('bind fail');
});
```

### connect<sup>7+</sup>

connect(options: TCPConnectOptions, callback: AsyncCallback\<void\>): void

Sets up a connection to the specified IP address and port number. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **bind** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                    | Mandatory| Description                                                        |
| -------- | ---------------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [TCPConnectOptions](#tcpconnectoptions) | Yes  | TCPSocket connection parameters. For details, see [TCPConnectOptions](#tcpconnectoptions).|
| callback | AsyncCallback\<void\>                    | Yes  | Callback used to return the result.                                                  |

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 }, err => {
  if (err) {
    console.log('connect fail');
    return;
  }
  console.log('connect success');
})
```

### connect<sup>7+</sup>

connect(options: TCPConnectOptions): Promise\<void\>

Sets up a connection to the specified IP address and port number. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                                    | Mandatory| Description                                                        |
| ------- | ---------------------------------------- | ---- | ------------------------------------------------------------ |
| options | [TCPConnectOptions](#tcpconnectoptions) | Yes  | TCPSocket connection parameters. For details, see [TCPConnectOptions](#tcpconnectoptions).|

**Return value**

| Type           | Description                                                      |
| :-------------- | :--------------------------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise = tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 });
promise.then(() => {
  console.log('connect success')
}).catch(err => {
  console.log('connect fail');
});
```

### send<sup>7+</sup>

send(options: TCPSendOptions, callback: AsyncCallback\<void\>): void

Sends data over a TCPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                   | Mandatory| Description                                                        |
| -------- | --------------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [TCPSendOptions](#tcpsendoptions) | Yes  | Parameters for sending data over the TCPSocket connection. For details, see [TCPSendOptions](#tcpsendoptions).|
| callback | AsyncCallback\<void\>                   | Yes  | Callback used to return the result.                                                  |

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 }, () => {
  console.log('connect success');
  tcp.send({
    data: 'Hello, server!'
    // Encoding is omitted here. The UTF-8 encoding format is used by default.
  }, err => {
    if (err) {
      console.log('send fail');
      return;
    }
    console.log('send success');
  })
})
```

### send<sup>7+</sup>

send(options: TCPSendOptions): Promise\<void\>

Sends data over a TCPSocket connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                                   | Mandatory| Description                                                        |
| ------- | --------------------------------------- | ---- | ------------------------------------------------------------ |
| options | [TCPSendOptions](#tcpsendoptions) | Yes  | Parameters for sending data over the TCPSocket connection. For details, see [TCPSendOptions](#tcpsendoptions).|

**Return value**

| Type           | Description                                              |
| :-------------- | :------------------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise1 = tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 });
promise1.then(() => {
  console.log('connect success');
  let promise2 = tcp.send({
    data: 'Hello, server!'
  });
  promise2.then(() => {
    console.log('send success');
  }).catch(err => {
    console.log('send fail');
  });
}).catch(err => {
  console.log('connect fail');
});
```

### close<sup>7+</sup>

close(callback: AsyncCallback\<void\>): void

Closes a TCPSocket connection. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                 | Mandatory| Description      |
| -------- | --------------------- | ---- | ---------- |
| callback | AsyncCallback\<void\> | Yes  | Callback used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
tcp.close(err => {
  if (err) {
    console.log('close fail');
    return;
  }
  console.log('close success');
})
```

### close<sup>7+</sup>

close(): Promise\<void\>

Closes a TCPSocket connection. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type           | Description                                      |
| :-------------- | :----------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise = tcp.close();
promise.then(() => {
  console.log('close success');
}).catch(err => {
  console.log('close fail');
});
```

### getRemoteAddress<sup>7+</sup>

getRemoteAddress(callback: AsyncCallback\<NetAddress\>): void

Obtains the remote address of a socket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                             | Mandatory| Description      |
| -------- | ------------------------------------------------- | ---- | ---------- |
| callback | AsyncCallback<[NetAddress](#netaddress)> | Yes  | Callback used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 }, () => {
  console.log('connect success');
  tcp.getRemoteAddress((err, data) => {
    if (err) {
      console.log('getRemoteAddressfail');
      return;
    }
    console.log('getRemoteAddresssuccess:' + JSON.stringify(data));
  })
});
```

### getRemoteAddress<sup>7+</sup>

getRemoteAddress(): Promise\<NetAddress\>

Obtains the remote address of a socket connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                                       | Description                                       |
| :------------------------------------------ | :------------------------------------------ |
| Promise<[NetAddress](#netaddress)> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise1 = tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 });
promise1.then(() => {
  console.log('connect success');
  let promise2 = tcp.getRemoteAddress();
  promise2.then(() => {
    console.log('getRemoteAddress success');
  }).catch(err => {
    console.log('getRemoteAddressfail');
  });
}).catch(err => {
  console.log('connect fail');
});
```

### getState<sup>7+</sup>

getState(callback: AsyncCallback\<SocketStateBase\>): void

Obtains the status of the TCPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **bind** or **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                  | Mandatory| Description      |
| -------- | ------------------------------------------------------ | ---- | ---------- |
| callback | AsyncCallback<[SocketStateBase](#socketstatebase)> | Yes  | Callback used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise = tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 }, () => {
  console.log('connect success');
  tcp.getState((err, data) => {
    if (err) {
      console.log('getState fail');
      return;
    }
    console.log('getState success:' + JSON.stringify(data));
  });
});
```

### getState<sup>7+</sup>

getState(): Promise\<SocketStateBase\>

Obtains the status of the TCPSocket connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **bind** or **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                                            | Description                                      |
| :----------------------------------------------- | :----------------------------------------- |
| Promise<[SocketStateBase](#socketstatebase)> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise = tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 });
promise.then(() => {
  console.log('connect success');
  let promise1 = tcp.getState();
  promise1.then(() => {
    console.log('getState success');
  }).catch(err => {
    console.log('getState fail');
  });
}).catch(err => {
  console.log('connect fail');
});
```

### setExtraOptions<sup>7+</sup>

setExtraOptions(options: TCPExtraOptions, callback: AsyncCallback\<void\>): void

Sets other properties of the TCPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **bind** or **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                     | Mandatory| Description                                                        |
| -------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [TCPExtraOptions](#tcpextraoptions) | Yes  | Other properties of the TCPSocket connection. For details, see [TCPExtraOptions](#tcpextraoptions).|
| callback | AsyncCallback\<void\>                     | Yes  | Callback used to return the result.                                                  |

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise = tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 }, () => {
  console.log('connect success');
  tcp.setExtraOptions({
    keepAlive: true,
    OOBInline: true,
    TCPNoDelay: true,
    socketLinger: { on: true, linger: 10 },
    receiveBufferSize: 1000,
    sendBufferSize: 1000,
    reuseAddress: true,
    socketTimeout: 3000,
  }, err => {
    if (err) {
      console.log('setExtraOptions fail');
      return;
    }
    console.log('setExtraOptions success');
  });
});
```

### setExtraOptions<sup>7+</sup>

setExtraOptions(options: TCPExtraOptions): Promise\<void\>

Sets other properties of the TCPSocket connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **bind** or **connect** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                                     | Mandatory| Description                                                        |
| ------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| options | [TCPExtraOptions](#tcpextraoptions) | Yes  | Other properties of the TCPSocket connection. For details, see [TCPExtraOptions](#tcpextraoptions).|

**Return value**

| Type           | Description                                                |
| :-------------- | :--------------------------------------------------- |
| Promise\<void\> | Promise used to return the result.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let promise = tcp.connect({ address: { address: '192.168.xx.xxx', port: xxxx, family: 1 }, timeout: 6000 });
promise.then(() => {
  console.log('connect success');
  let promise1 = tcp.setExtraOptions({
    keepAlive: true,
    OOBInline: true,
    TCPNoDelay: true,
    socketLinger: { on: true, linger: 10 },
    receiveBufferSize: 1000,
    sendBufferSize: 1000,
    reuseAddress: true,
    socketTimeout: 3000,
  });
  promise1.then(() => {
    console.log('setExtraOptions success');
  }).catch(err => {
    console.log('setExtraOptions fail');
  });
}).catch(err => {
  console.log('connect fail');
});
```

### on('message')<sup>7+</sup>

on(type: 'message', callback: Callback<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}\>): void

Enables listening for message receiving events of the TCPSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo)}> | Yes  | Callback used to return the result.                               |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let messageView = '';
tcp.on('message', value => {
  for (var i = 0; i < value.message.length; i++) {
    let messages = value.message[i]
    let message = String.fromCharCode(messages);
    messageView += message;
  }
  console.log('on message message: ' + JSON.stringify(messageView));
  console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
});
```

### off('message')<sup>7+</sup>

off(type: 'message', callback?: Callback<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}\>): void

Disables listening for message receiving events of the TCPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo)}> | No  | Callback used to return the result.                               |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let messageView = '';
let callback = value => {
  for (var i = 0; i < value.message.length; i++) {
    let messages = value.message[i]
    let message = String.fromCharCode(messages);
    messageView += message;
  }
  console.log('on message message: ' + JSON.stringify(messageView));
  console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
}
tcp.on('message', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tcp.off('message', callback);
tcp.off('message');
```

### on('connect' | 'close')<sup>7+</sup>

on(type: 'connect' | 'close', callback: Callback\<void\>): void

Enables listening for connection or close events of the TCPSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                                                        |
| -------- | ---------------- | ---- | ------------------------------------------------------------ |
| type     | string           | Yes  | Type of the event to subscribe to.<br>- **connect**: connection event<br>- **close**: close event|
| callback | Callback\<void\> | Yes  | Callback used to return the result.                                                  |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
tcp.on('connect', () => {
  console.log("on connect success")
});
tcp.on('close', () => {
  console.log("on close success")
});
```

### off('connect' | 'close')<sup>7+</sup>

off(type: 'connect' | 'close', callback?: Callback\<void\>): void

Disables listening for connection or close events of the TCPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                                                        |
| -------- | ---------------- | ---- | ------------------------------------------------------------ |
| type     | string           | Yes  | Type of the event to subscribe to.<br>- **connect**: connection event<br>- **close**: close event|
| callback | Callback\<void\> | No  | Callback used to return the result.                                                  |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let callback1 = () => {
  console.log("on connect success");
}
tcp.on('connect', callback1);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tcp.off('connect', callback1);
tcp.off('connect');
let callback2 = () => {
  console.log("on close success");
}
tcp.on('close', callback2);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tcp.off('close', callback2);
tcp.off('close');
```

### on('error')<sup>7+</sup>

on(type: 'error', callback: ErrorCallback): void

Enables listening for error events of the TCPSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | Yes  | Callback used to return the result.                          |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
tcp.on('error', err => {
  console.log("on error, err:" + JSON.stringify(err))
});
```

### off('error')<sup>7+</sup>

off(type: 'error', callback?: ErrorCallback): void

Disables listening for error events of the TCPSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | No  | Callback used to return the result.                          |

**Example**

```js
let tcp = socket.constructTCPSocketInstance();
let callback = err => {
  console.log("on error, err:" + JSON.stringify(err));
}
tcp.on('error', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tcp.off('error', callback);
tcp.off('error');
```

## TCPConnectOptions<sup>7+</sup>

Defines TCPSocket connection parameters.

**System capability**: SystemCapability.Communication.NetStack

| Name | Type                              | Mandatory| Description                      |
| ------- | ---------------------------------- | ---- | -------------------------- |
| address | [NetAddress](#netaddress) | Yes  | Bound IP address and port number.      |
| timeout | number                             | No  | Timeout duration of the TCPSocket connection, in ms.|

## TCPSendOptions<sup>7+</sup>

Defines the parameters for sending data over the TCPSocket connection.

**System capability**: SystemCapability.Communication.NetStack

| Name  | Type  | Mandatory| Description                                                        |
| -------- | ------ | ---- | ------------------------------------------------------------ |
| data     | string\| ArrayBuffer<sup>7+</sup>  | Yes  | Data to send.                                                |
| encoding | string | No  | Character encoding format. The options are as follows: **UTF-8**, **UTF-16BE**, **UTF-16LE**, **UTF-16**, **US-AECII**, and **ISO-8859-1**. The default value is **UTF-8**.|

## TCPExtraOptions<sup>7+</sup>

Defines other properties of the TCPSocket connection.

**System capability**: SystemCapability.Communication.NetStack

| Name           | Type   | Mandatory| Description                                                        |
| ----------------- | ------- | ---- | ------------------------------------------------------------ |
| keepAlive         | boolean | No  | Whether to keep the connection alive. The default value is **false**.                                 |
| OOBInline         | boolean | No  | Whether to enable OOBInline. The default value is **false**.                                |
| TCPNoDelay        | boolean | No  | Whether to enable no-delay on the TCPSocket connection. The default value is **false**.                      |
| socketLinger      | Object  | Yes  | Socket linger.<br>- **on**: whether to enable socket linger. The value true means to enable socket linger and false means the opposite.<br>- **linger**: linger time, in ms. The value ranges from **0** to **65535**.<br>Specify this parameter only when **on** is set to **true**.|
| receiveBufferSize | number  | No  | Size of the receive buffer, in bytes. The default value is **0**.                              |
| sendBufferSize    | number  | No  | Size of the send buffer, in bytes. The default value is **0**.                              |
| reuseAddress      | boolean | No  | Whether to reuse addresses. The default value is **false**.                                 |
| socketTimeout     | number  | No  | Timeout duration of the UDPSocket connection, in ms. The default value is **0**.                            |

## socket.constructTCPSocketServerInstance<sup>10+</sup>

constructTCPSocketServerInstance(): TCPSocketServer

Creates a **TCPSocketServer** object.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                               | Description                         |
| :---------------------------------- | :---------------------------- |
| [TCPSocketServer](#tcpsocketserver10) | **TCPSocketServer** object.|

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
```

## TCPSocketServer<sup>10+</sup>

Defines a TCPSocketServer connection. Before invoking TCPSocketServer APIs, you need to call [socket.constructTCPSocketServerInstance](#socketconstructtcpsocketserverinstance10) to create a **TCPSocketServer** object.

### listen<sup>10+</sup>

listen(address: NetAddress, callback: AsyncCallback\<void\>): void

Binds the IP address and port number. The port number can be specified or randomly allocated by the system. The server listens to and accepts TCPSocket connections established over the socket. Multiple threads are used to process client data concurrently. This API uses an asynchronous callback to return the result.

> **NOTE**
> The server uses this API to perform the bind, listen, and accept operations.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                     | Mandatory| Description                                         |
| -------- | ------------------------- | ---- | --------------------------------------------- |
| address  | [NetAddress](#netaddress7) | Yes  | Destination address.|
| callback | AsyncCallback\<void\>     | Yes  | Callback used to return the result.                                   |

**Error codes**

| ID| Error Message                                   |
| -------- | ------------------------------------------- |
| 401      | Parameter error.                            |
| 201      | Permission denied.                          |
| 2300002  | System internal error.                      |
| 2303109  | Bad file number.                            |
| 2303111  | Resource temporarily unavailable try again. |
| 2303198  | Address already in use.                     |
| 2303199  | Cannot assign requested address.            |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.listen({ address: "192.168.xx.xxx", port: xxxx, family: 1 }, err => {
  if (err) {
    console.log("listen fail");
    return;
  }
  console.log("listen success");
})
```

### listen<sup>10+</sup>

listen(address: NetAddress): Promise\<void\>

Binds the IP address and port number. The port number can be specified or randomly allocated by the system. The server listens to and accepts TCPSocket connections established over the socket. Multiple threads are used to process client data concurrently. This API uses a promise to return the result.

> **NOTE**
> The server uses this API to perform the bind, listen, and accept operations.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                     | Mandatory| Description                                         |
| ------- | ------------------------- | ---- | --------------------------------------------- |
| address | [NetAddress](#netaddress7) | Yes  | Destination address.|

**Return value**

| Type           | Description                                                        |
| :-------------- | :----------------------------------------------------------- |
| Promise\<void\> | Promise used to return the result. If the operation is successful, no value is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                                   |
| -------- | ------------------------------------------- |
| 401      | Parameter error.                            |
| 201      | Permission denied.                          |
| 2300002  | System internal error.                      |
| 2303109  | Bad file number.                            |
| 2303111  | Resource temporarily unavailable try again. |
| 2303198  | Address already in use.                     |
| 2303199  | Cannot assign requested address.            |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
let promise = tcpServer.listen({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promise.then(() => {
  console.log('listen success');
}).catch(err => {
  console.log('listen fail');
});
```

### getState<sup>10+</sup>

getState(callback: AsyncCallback\<SocketStateBase\>): void

Obtains the status of the TCPSocketServer connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **listen** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                              | Mandatory| Description      |
| -------- | -------------------------------------------------- | ---- | ---------- |
| callback | AsyncCallback<[SocketStateBase](#socketstatebase7)> | Yes  | Callback used to return the result.|

**Error codes**

| ID| Error Message                       |
| -------- | ------------------------------- |
| 401      | Parameter error.                |
| 201      | Permission denied.              |
| 2300002  | System internal error.          |
| 2303188  | Socket operation on non-socket. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.listen({ address: "192.168.xx.xxx", port: xxxx, family: 1 }, err => {
  if (err) {
    console.log("listen fail");
    return;
  }
  console.log("listen success");
})
tcpServer.getState((err, data) => {
  if (err) {
    console.log('getState fail');
    return;
  }
  console.log('getState success:' + JSON.stringify(data));
})
```

### getState<sup>10+</sup>

getState(): Promise\<SocketStateBase\>

Obtains the status of the TCPSocketServer connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **listen** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                                        | Description                                      |
| :------------------------------------------- | :----------------------------------------- |
| Promise<[SocketStateBase](#socketstatebase7)> | Promise used to return the result.|

**Error codes**

| ID| Error Message                       |
| -------- | ------------------------------- |
| 201      | Permission denied.              |
| 2300002  | System internal error.          |
| 2303188  | Socket operation on non-socket. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
let promiseListen = tcpServer.listen({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promiseListen.then(() => {
  console.log('listen success');
}).catch(err => {
  console.log('listen fail');
});
let promise = tcpServer.getState();
promise.then(() => {
  console.log('getState success');
}).catch(err => {
  console.log('getState fail');
});
```

### setExtraOptions<sup>10+</sup>

setExtraOptions(options: TCPExtraOptions, callback: AsyncCallback\<void\>): void

Sets other properties of the TCPSocketServer connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after **listen** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                               | Mandatory| Description                                                        |
| -------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [TCPExtraOptions](#tcpextraoptions7) | Yes  | Other properties of the TCPSocketServer connection.|
| callback | AsyncCallback\<void\>               | Yes  | Callback used to return the result.                                                  |

**Error codes**

| ID| Error Message                       |
| -------- | ------------------------------- |
| 401      | Parameter error.                |
| 201      | Permission denied.              |
| 2300002  | System internal error.          |
| 2303188  | Socket operation on non-socket. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.listen({ address: "192.168.xx.xxx", port: xxxx, family: 1 }, err => {
  if (err) {
    console.log("listen fail");
    return;
  }
  console.log("listen success");
})
tcpServer.setExtraOptions({
  keepAlive: true,
  OOBInline: true,
  TCPNoDelay: true,
  socketLinger: { on: true, linger: 10 },
  receiveBufferSize: 1000,
  sendBufferSize: 1000,
  reuseAddress: true,
  socketTimeout: 3000,
}, err => {
  if (err) {
    console.log('setExtraOptions fail');
    return;
  }
  console.log('setExtraOptions success');
});
```

### setExtraOptions<sup>10+</sup>

setExtraOptions(options: TCPExtraOptions): Promise\<void\>

Sets other properties of the TCPSocketServer connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after **listen** is successfully called.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                               | Mandatory| Description                                                        |
| ------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| options | [TCPExtraOptions](#tcpextraoptions7) | Yes  | Other properties of the TCPSocketServer connection.|

**Return value**

| Type           | Description                                                      |
| :-------------- | :--------------------------------------------------------- |
| Promise\<void\> | Promise used to return the result. If the operation is successful, no value is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| -------- | ------------------------------- |
| 401      | Parameter error.                |
| 201      | Permission denied.              |
| 2300002  | System internal error.          |
| 2303188  | Socket operation on non-socket. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
let promiseListen = tcpServer.listen({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promiseListen.then(() => {
  console.log('listen success');
}).catch(err => {
  console.log('listen fail');
});
let promise = tcpServer.setExtraOptions({
  keepAlive: true,
  OOBInline: true,
  TCPNoDelay: true,
  socketLinger: { on: true, linger: 10 },
  receiveBufferSize: 1000,
  sendBufferSize: 1000,
  reuseAddress: true,
  socketTimeout: 3000,
});
promise.then(() => {
  console.log('setExtraOptions success');
}).catch(err => {
  console.log('setExtraOptions fail');
});
```

### on('connect')<sup>10+</sup>

on(type: 'connect', callback: Callback\<TCPSocketConnection\>): void

Enables listening for TCPSocketServer connection events. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                           | Mandatory| Description                                 |
| -------- | ------------------------------- | ---- | ------------------------------------- |
| type     | string                          | Yes  | Type of the event to subscribe to.<br/> **connect**: connection event|
| callback | Callback<[TCPSocketConnection](#tcpsocketconnection10)> | Yes  | Callback used to return the result.                           |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(data) {
  console.log(JSON.stringify(data))
});
```

### off('connect')<sup>10+</sup>

off(type: 'connect', callback?: Callback\<TCPSocketConnection\>): void

Disables listening for TCPSocketServer connection events. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                           | Mandatory| Description                                 |
| -------- | ------------------------------- | ---- | ------------------------------------- |
| type     | string                          | Yes  | Type of the event to subscribe to.<br/> **connect**: connection event|
| callback | Callback<[TCPSocketConnection](#tcpsocketconnection10)> | No  | Callback used to return the result.                           |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
let callback = data => {
  console.log('on connect message: ' + JSON.stringify(data));
}
tcpServer.on('connect', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tcpServer.off('connect', callback);
tcpServer.off('connect');
```

### on('error')<sup>10+</sup>

on(type: 'error', callback: ErrorCallback): void

Enables listening for error events of the TCPSocketServer connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | Yes  | Callback used to return the result.                          |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('error', err => {
  console.log("on error, err:" + JSON.stringify(err))
});
```

### off('error')<sup>10+</sup>

off(type: 'error', callback?: ErrorCallback): void

Disables listening for error events of the TCPSocketServer connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | No  | Callback used to return the result.                          |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
let callback = err => {
  console.log("on error, err:" + JSON.stringify(err));
}
tcpServer.on('error', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tcpServer.off('error', callback);
tcpServer.off('error');
```

## TCPSocketConnection<sup>10+</sup>

Defines a **TCPSocketConnection** object, that is, the connection between the TCPSocket client and the server. Before invoking TCPSocketConnection APIs, you need to obtain a **TCPSocketConnection** object.

**System capability**: SystemCapability.Communication.NetStack

### Attributes

| Name    | Type  | Mandatory| Description                                     |
| -------- | ------ | ---- | ----------------------------------------- |
| clientId | number | Yes  | ID of the connection between the client and TCPSocketServer.|

### send<sup>10+</sup>

send(options: TCPSendOptions, callback: AsyncCallback\<void\>): void

Sends data over a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after a connection with the client is set up.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                             | Mandatory| Description                                                        |
| -------- | --------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [TCPSendOptions](#tcpsendoptions7) | Yes  | Defines the parameters for sending data over the **TCPSocketConnection** object.|
| callback | AsyncCallback\<void\>             | Yes  | Callback used to return the result.                                                  |

**Error codes**

| ID| Error Message              |
| -------- | ---------------------- |
| 401      | Parameter error.       |
| 201      | Permission denied.     |
| 2300002  | System internal error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.send({data: 'Hello, client!'}, err => {
    if (err) {
        console.log('send fail');
        return;
    }
    console.log('send success');
  });
});
```

### send<sup>10+</sup>

send(options: TCPSendOptions): Promise\<void\>

Sends data over a **TCPSocketConnection** object. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after a connection with the client is set up.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                             | Mandatory| Description                                                        |
| ------- | --------------------------------- | ---- | ------------------------------------------------------------ |
| options | [TCPSendOptions](#tcpsendoptions7) | Yes  | Defines the parameters for sending data over the **TCPSocketConnection** object.|

**Return value**

| Type           | Description                                                        |
| :-------------- | :----------------------------------------------------------- |
| Promise\<void\> | Promise used to return the result. If the operation is successful, no value is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message              |
| -------- | ---------------------- |
| 401      | Parameter error.       |
| 201      | Permission denied.     |
| 2300002  | System internal error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  let promise = client.send({data: 'Hello, client!'});
  promise.then(() => {
    console.log('send success');
  }).catch(err => {
    console.log('send fail');
  });
});
```

### close<sup>10+</sup>

close(callback: AsyncCallback\<void\>): void

Closes a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                 | Mandatory| Description      |
| -------- | --------------------- | ---- | ---------- |
| callback | AsyncCallback\<void\> | Yes  | Callback used to return the result.|

**Error codes**

| ID| Error Message              |
| -------- | ---------------------- |
| 401      | Parameter error.       |
| 201      | Permission denied.     |
| 2300002  | System internal error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.close(err => {
    if (err) {
      console.log('close fail');
      return;
    }
    console.log('close success');
  });
});
```

### close<sup>10+</sup>

close(): Promise\<void\>

Closes a **TCPSocketConnection** object. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type           | Description                                        |
| :-------------- | :------------------------------------------- |
| Promise\<void\> | Promise used to return the result. If the operation is successful, no value is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message              |
| -------- | ---------------------- |
| 201      | Permission denied.     |
| 2300002  | System internal error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  let promise = client.close();
  promise.then(() => {
  	console.log('close success');
  }).catch(err => {
  	console.log('close fail');
  });
});
```

### getRemoteAddress<sup>10+</sup>

getRemoteAddress(callback: AsyncCallback\<NetAddress\>): void

Obtains the remote address of a socket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> This API can be called only after a connection with the client is set up.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                    | Mandatory| Description      |
| -------- | ---------------------------------------- | ---- | ---------- |
| callback | AsyncCallback<[NetAddress](#netaddress7)> | Yes  | Callback used to return the result.|

**Error codes**

| ID| Error Message                       |
| -------- | ------------------------------- |
| 401      | Parameter error.                |
| 201      | Permission denied.              |
| 2300002  | System internal error.          |
| 2303188  | Socket operation on non-socket. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.getRemoteAddress((err, data) => {
    if (err) {
      console.log('getRemoteAddress fail');
      return;
    }
    console.log('getRemoteAddress success:' + JSON.stringify(data));
  });
});
```

### getRemoteAddress<sup>10+</sup>

getRemoteAddress(): Promise\<NetAddress\>

Obtains the remote address of a socket connection. This API uses a promise to return the result.

> **NOTE**
> This API can be called only after a connection with the client is set up.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                              | Description                                       |
| :--------------------------------- | :------------------------------------------ |
| Promise<[NetAddress](#netaddress7)> | Promise used to return the result.|

**Error codes**

| ID| Error Message                       |
| -------- | ------------------------------- |
| 201      | Permission denied.              |
| 2300002  | System internal error.          |
| 2303188  | Socket operation on non-socket. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  let promise = client.getRemoteAddress();
  promise.then(() => {
    console.log('getRemoteAddress success');
  }).catch(err => {
    console.log('getRemoteAddress fail');
  });
});
```

### on('message')<sup>10+</sup>

on(type: 'message', callback: Callback<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}\>): void

Enables listening for **message** events of a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo7)}> | Yes  | Callback used to return the result.<br> **message**: received message.<br>**remoteInfo**: socket connection information.                               |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.on('message', value => {
    let messageView = '';
    for (var i = 0; i < value.message.length; i++) {
      let messages = value.message[i];
      let message = String.fromCharCode(messages);
      messageView += item;
    }
    console.log('on message message: ' + JSON.stringify(messageView));
    console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
  });
});
```

### off('message')<sup>10+</sup>

off(type: 'message', callback?: Callback<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}\>): void

Disables listening for **message** events of a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo7)}> | No  | Callback used to return the result. **message**: received message.<br>**remoteInfo**: socket connection information.                               |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let callback = value => {
  let messageView = '';
  for (var i = 0; i < value.message.length; i++) {
    let messages = value.message[i];
    let message = String.fromCharCode(messages);
    messageView += item;
  }
  console.log('on message message: ' + JSON.stringify(messageView));
  console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
}
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.on('message', callback);
  // You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
  client.off('message', callback);
  client.off('message');
});
```

### on('close')<sup>10+</sup>

on(type: 'close', callback: Callback\<void\>): void

Enables listening for **close** events of a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                               |
| -------- | ---------------- | ---- | ----------------------------------- |
| type     | string           | Yes  | Type of the event to subscribe to.<br/> **close**: close event|
| callback | Callback\<void\> | Yes  | Callback used to return the result.                         |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.on('close', () => {
    console.log("on close success")
  });
});
```

### off('close')<sup>10+</sup>

on(type: 'close', callback: Callback\<void\>): void

Disables listening for **close** events of a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                               |
| -------- | ---------------- | ---- | ----------------------------------- |
| type     | string           | Yes  | Type of the event to subscribe to.<br/> **close**: close event|
| callback | Callback\<void\> | No  | Callback used to return the result.                         |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let callback = () => {
  console.log("on close success");
}
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.on('close', callback);
  // You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
  client.off('close', callback);
  client.off('close');
});
```

### on('error')<sup>10+</sup>

on(type: 'error', callback: ErrorCallback): void

Enables listening for **error** events of a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | Yes  | Callback used to return the result.                          |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.on('error', err => {
    console.log("on error, err:" + JSON.stringify(err))
  });
});
```

### off('error')<sup>10+</sup>

off(type: 'error', callback?: ErrorCallback): void

Disables listening for **error** events of a **TCPSocketConnection** object. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | No  | Callback used to return the result.                          |

**Error codes**

| ID| Error Message        |
| -------- | ---------------- |
| 401      | Parameter error. |

**Example**

```js
let callback = err => {
  console.log("on error, err:" + JSON.stringify(err));
}
let tcpServer = socket.constructTCPSocketServerInstance();
tcpServer.on('connect', function(client) {
  client.on('error', callback);
  // You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
  client.off('error', callback);
  client.off('error');
});
```

## Description of TCP Error Codes

The TCP error code mapping is in the format of 2301000 + Linux kernel error code.

For details about error codes, see [Socket Error Codes](../errorcodes/errorcode-net-socket.md).

## socket.constructTLSSocketInstance<sup>9+</sup>

constructTLSSocketInstance(): TLSSocket

Creates a **TLSSocket** object.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                              | Description                   |
| :--------------------------------- | :---------------------- |
| [TLSSocket](#tlssocket9) | **TLSSocket** object.|

**Example**

```js
let tls = socket.constructTLSSocketInstance();
```

## TLSSocket<sup>9+</sup>

Defines a TLSSocket connection. Before invoking TLSSocket APIs, you need to call [socket.constructTLSSocketInstance](#socketconstructtlssocketinstance9) to create a **TLSSocket** object.

### bind<sup>9+</sup>

bind(address: NetAddress, callback: AsyncCallback\<void\>): void

Binds the IP address and port number. This API uses an asynchronous callback to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                              | Mandatory| Description                                                  |
| -------- | ---------------------------------- | ---- | ------------------------------------------------------ |
| address  | [NetAddress](#netaddress) | Yes  | Destination address. For details, see [NetAddress](#netaddress).|
| callback | AsyncCallback\<void\>              | Yes  | Callback used to return the result. If the operation is successful, the result of binding the local IP address and port number is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |
| 2303198 | Address already in use. |
| 2300002 | System internal error.  |

**Example**

```js
tls.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});
```

### bind<sup>9+</sup>

bind(address: NetAddress): Promise\<void\>

Binds the IP address and port number. This API uses a promise to return the result.

**Required permissions**: ohos.permission.INTERNET

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                              | Mandatory| Description                                                  |
| ------- | ---------------------------------- | ---- | ------------------------------------------------------ |
| address | [NetAddress](#netaddress)          | Yes  | Destination address. For details, see [NetAddress](#netaddress).|

**Return value**

| Type           | Description                                                    |
| :-------------- | :------------------------------------------------------- |
| Promise\<void\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                |
| ------- | ----------------------- |
| 401     | Parameter error.        |
| 201     | Permission denied.      |
| 2303198 | Address already in use. |
| 2300002 | System internal error.  |

**Example**

```js
let promise = tls.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promise.then(() => {
  console.log('bind success');
}).catch(err => {
  console.log('bind fail');
});
```

### getState<sup>9+</sup>

getState(callback: AsyncCallback\<SocketStateBase\>): void

Obtains the status of the TLSSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                  | Mandatory| Description      |
| -------- | ------------------------------------------------------ | ---- | ---------- |
| callback | AsyncCallback\<[SocketStateBase](#socketstatebase)> | Yes  | Callback used to return the result. If the operation is successful, the status of the TLSSocket connection is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303188 | Socket operation on non-socket.|
| 2300002 | System internal error.         |

**Example**

```js
let promise = tls.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});
tls.getState((err, data) => {
  if (err) {
    console.log('getState fail');
    return;
  }
  console.log('getState success:' + JSON.stringify(data));
});
```

### getState<sup>9+</sup>

getState(): Promise\<SocketStateBase\>

Obtains the status of the TLSSocket connection. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                                            | Description                                      |
| :----------------------------------------------- | :----------------------------------------- |
| Promise\<[SocketStateBase](#socketstatebase)> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303188 | Socket operation on non-socket.|
| 2300002 | System internal error.         |

**Example**

```js
let promiseBind = tls.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 });
promiseBind.then(() => {
  console.log('bind success');
}).catch((err) => {
  console.log('bind fail');
});
let promise = tls.getState();
promise.then(() => {
  console.log('getState success');
}).catch(err => {
  console.log('getState fail');
});
```

### setExtraOptions<sup>9+</sup>

setExtraOptions(options: TCPExtraOptions, callback: AsyncCallback\<void\>): void

Sets other properties of the TCPSocket connection after successful binding of the local IP address and port number of the connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                     | Mandatory| Description                                                        |
| -------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [TCPExtraOptions](#tcpextraoptions) | Yes  | Other properties of the TCPSocket connection. For details, see [TCPExtraOptions](#tcpextraoptions).|
| callback | AsyncCallback\<void\>                     | Yes  | Callback used to return the result. If the operation is successful, the result of setting other properties of the TCPSocket connection is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | -----------------------------  |
| 401     | Parameter error.               |
| 2303188 | Socket operation on non-socket.|
| 2300002 | System internal error.         |

**Example**

```js
tls.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});

tls.setExtraOptions({
  keepAlive: true,
  OOBInline: true,
  TCPNoDelay: true,
  socketLinger: { on: true, linger: 10 },
  receiveBufferSize: 1000,
  sendBufferSize: 1000,
  reuseAddress: true,
  socketTimeout: 3000,
}, err => {
  if (err) {
    console.log('setExtraOptions fail');
    return;
  }
  console.log('setExtraOptions success');
});
```

### setExtraOptions<sup>9+</sup>

setExtraOptions(options: TCPExtraOptions): Promise\<void\>

Sets other properties of the TCPSocket connection after successful binding of the local IP address and port number of the connection. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name | Type                                     | Mandatory| Description                                                        |
| ------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| options | [TCPExtraOptions](#tcpextraoptions) | Yes  | Other properties of the TCPSocket connection. For details, see [TCPExtraOptions](#tcpextraoptions).|

**Return value**

| Type           | Description                                                |
| :-------------- | :--------------------------------------------------- |
| Promise\<void\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 401     | Parameter error.               |
| 2303188 | Socket operation on non-socket.|
| 2300002 | System internal error.         |

**Example**

```js
tls.bind({ address: '192.168.xx.xxx', port: xxxx, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});
let promise = tls.setExtraOptions({
  keepAlive: true,
  OOBInline: true,
  TCPNoDelay: true,
  socketLinger: { on: true, linger: 10 },
  receiveBufferSize: 1000,
  sendBufferSize: 1000,
  reuseAddress: true,
  socketTimeout: 3000,
});
promise.then(() => {
  console.log('setExtraOptions success');
}).catch(err => {
  console.log('setExtraOptions fail');
});
```

### on('message')<sup>9+</sup>

on(type: 'message', callback: Callback<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}>): void;

Enables listening for **message** events of the TLSSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback\<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo)}\> | Yes  | Callback used to return the result.<br> **message**: received message.<br>**remoteInfo**: socket connection information.|

**Example**

```js
let tls = socket.constructTLSSocketInstance();
let messageView = '';
tls.on('message', value => {
  for (var i = 0; i < value.message.length; i++) {
    let messages = value.message[i]
    let message = String.fromCharCode(messages);
    messageView += message;
  }
  console.log('on message message: ' + JSON.stringify(messageView));
  console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
});
```

### off('message')<sup>9+</sup>

off(type: 'message', callback?: Callback\<{message: ArrayBuffer, remoteInfo: SocketRemoteInfo}\>): void

Disables listening for **message** events of the TLSSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                                        | Mandatory| Description                                     |
| -------- | ------------------------------------------------------------ | ---- | ----------------------------------------- |
| type     | string                                                       | Yes  | Type of the event to subscribe to.<br/> **message**: message receiving event|
| callback | Callback<{message: ArrayBuffer, remoteInfo: [SocketRemoteInfo](#socketremoteinfo)}> | No  | Callback used to return the result. **message**: received message.<br>**remoteInfo**: socket connection information.|

**Example**

```js
let tls = socket.constructTLSSocketInstance();
let messageView = '';
let callback = value => {
  for (var i = 0; i < value.message.length; i++) {
    let messages = value.message[i]
    let message = String.fromCharCode(messages);
    messageView += message;
  }
  console.log('on message message: ' + JSON.stringify(messageView));
  console.log('remoteInfo: ' + JSON.stringify(value.remoteInfo));
}
tls.on('message', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tls.off('message', callback);
```
### on('connect' | 'close')<sup>9+</sup>

on(type: 'connect' | 'close', callback: Callback\<void\>): void

Enables listening for connection or close events of the TLSSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                                                        |
| -------- | ---------------- | ---- | ------------------------------------------------------------ |
| type     | string           | Yes  | Type of the event to subscribe to.<br>- **connect**: connection event<br>- **close**: close event|
| callback | Callback\<void\> | Yes  | Callback used to return the result.                                                  |

**Example**

```js
let tls = socket.constructTLSSocketInstance();
tls.on('connect', () => {
  console.log("on connect success")
});
tls.on('close', () => {
  console.log("on close success")
});
```

### off('connect' | 'close')<sup>9+</sup>

off(type: 'connect' | 'close', callback?: Callback\<void\>): void

Disables listening for connection or close events of the TLSSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type            | Mandatory| Description                                                        |
| -------- | ---------------- | ---- | ------------------------------------------------------------ |
| type     | string           | Yes  | Type of the event to subscribe to.<br>- **connect**: connection event<br>- **close**: close event|
| callback | Callback\<void\> | No  | Callback used to return the result.                                                  |

**Example**

```js
let tls = socket.constructTLSSocketInstance();
let callback1 = () => {
  console.log("on connect success");
}
tls.on('connect', callback1);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tls.off('connect', callback1);
tls.off('connect');
let callback2 = () => {
  console.log("on close success");
}
tls.on('close', callback2);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tls.off('close', callback2);
```

### on('error')<sup>9+</sup>

on(type: 'error', callback: ErrorCallback): void

Enables listening for error events of the TLSSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | Yes  | Callback used to return the result.                          |

**Example**

```js
let tls = socket.constructTLSSocketInstance();
tls.on('error', err => {
  console.log("on error, err:" + JSON.stringify(err))
});
```

### off('error')<sup>9+</sup>

off(type: 'error', callback?: ErrorCallback): void

Disables listening for error events of the TLSSocket connection. This API uses an asynchronous callback to return the result.

> **NOTE**
> You can pass the callback of the **on** function if you want to cancel listening for a certain type of event. If you do not pass the callback, you will cancel listening for all events.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type         | Mandatory| Description                                |
| -------- | ------------- | ---- | ------------------------------------ |
| type     | string        | Yes  | Type of the event to subscribe to.<br/> **error**: error event|
| callback | ErrorCallback | No  | Callback used to return the result.                          |

**Example**

```js
let tls = socket.constructTLSSocketInstance();
let callback = err => {
  console.log("on error, err:" + JSON.stringify(err));
}
tls.on('error', callback);
// You can pass the callback of the on method to cancel listening for a certain type of callback. If you do not pass the callback, you will cancel listening for all callbacks.
tls.off('error', callback);
```

### connect<sup>9+</sup>

connect(options: TLSConnectOptions, callback: AsyncCallback\<void\>): void

Sets up a TLSSocket connection, and creates and initializes a TLS session after successful binding of the local IP address and port number of the TLSSocket connection. During this process, a TLS/SSL handshake is performed between the application and the server to implement data transmission. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                  | Mandatory| Description|
| -------- | ---------------------------------------| ----| --------------- |
| options  | [TLSConnectOptions](#tlsconnectoptions9) | Yes  | Parameters required for the TLSSocket connection.|
| callback | AsyncCallback\<void>                  | Yes  | Callback used to return the result. If the operation is successful, no value is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                                     |
| ------- | -------------------------------------------- |
| 401     | Parameter error.                             |
| 2303104 | Interrupted system call.                     |
| 2303109 | Bad file number.                             |
| 2303111 | Resource temporarily unavailable try again.  |
| 2303188 | Socket operation on non-socket.              |
| 2303191 | Protocol wrong type for socket.              |
| 2303198 | Address already in use.                      |
| 2303199 | Cannot assign requested address.             |
| 2303210 | Connection timed out.                        |
| 2303501 | SSL is null.                                 |
| 2303502 | Error in tls reading.                        |
| 2303503 | Error in tls writing                         |
| 2303505 | Error occurred in the tls system call.       |
| 2303506 | Error clearing tls connection.               |
| 2300002 | System internal error.                       |

**Example**

```js
let tlsTwoWay = socket.constructTLSSocketInstance(); // Two way authentication
tlsTwoWay.bind({ address: '192.168.xxx.xxx', port: 8080, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});
let options = {
  ALPNProtocols: ["spdy/1", "http/1.1"],
  address: {
    address: "192.168.xx.xxx",
    port: 8080,
    family: 1,
  },
  secureOptions: {
    key: "xxxx",
    cert: "xxxx",
    ca: ["xxxx"],
    password: "xxxx",
    protocols: [socket.Protocol.TLSv12],
    useRemoteCipherPrefer: true,
    signatureAlgorithms: "rsa_pss_rsae_sha256:ECDSA+SHA256",
    cipherSuite: "AES256-SHA256",
  },
};
tlsTwoWay.connect(options, (err, data) => {
  console.error("connect callback error" + err);
  console.log(JSON.stringify(data));
});

let tlsOneWay = socket.constructTLSSocketInstance(); // One way authentication
tlsOneWay.bind({ address: '192.168.xxx.xxx', port: 8080, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});
let oneWayOptions = {
  address: {
    address: "192.168.xxx.xxx",
    port: 8080,
    family: 1,
  },
  secureOptions: {
    ca: ["xxxx", "xxxx"],
    cipherSuite: "AES256-SHA256",
  },
};
tlsOneWay.connect(oneWayOptions, (err, data) => {
  console.error("connect callback error" + err);
  console.log(JSON.stringify(data));
});
```

### connect<sup>9+</sup>

connect(options: TLSConnectOptions): Promise\<void\>

Sets up a TLSSocket connection, and creates and initializes a TLS session after successful binding of the local IP address and port number of the TLSSocket connection. During this process, a TLS/SSL handshake is performed between the application and the server to implement data transmission. Both two-way and one-way authentication modes are supported. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                  | Mandatory| Description|
| -------- | --------------------------------------| ----| --------------- |
| options  | [TLSConnectOptions](#tlsconnectoptions9) | Yes  | Parameters required for the connection.|

**Return value**

| Type                                       | Description                         |
| ------------------------------------------- | ----------------------------- |
| Promise\<void\>                              | Promise used to return the result. If the operation is successful, no value is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                                     |
| ------- | -------------------------------------------- |
| 401     | Parameter error.                             |
| 2303104 | Interrupted system call.                     |
| 2303109 | Bad file number.                             |
| 2303111 | Resource temporarily unavailable try again.  |
| 2303188 | Socket operation on non-socket.              |
| 2303191 | Protocol wrong type for socket.              |
| 2303198 | Address already in use.                      |
| 2303199 | Cannot assign requested address.             |
| 2303210 | Connection timed out.                        |
| 2303501 | SSL is null.                                 |
| 2303502 | Error in tls reading.                        |
| 2303503 | Error in tls writing                         |
| 2303505 | Error occurred in the tls system call.       |
| 2303506 | Error clearing tls connection.               |
| 2300002 | System internal error.                       |

**Example**

```js
let tlsTwoWay = socket.constructTLSSocketInstance(); // Two way authentication
tlsTwoWay.bind({ address: '192.168.xxx.xxx', port: 8080, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});
let options = {
  ALPNProtocols: ["spdy/1", "http/1.1"],
  address: {
    address: "xxxx",
    port: 8080,
    family: 1,
  },
  secureOptions: {
    key: "xxxx",
    cert: "xxxx",
    ca: ["xxxx"],
    password: "xxxx",
    protocols: [socket.Protocol.TLSv12],
    useRemoteCipherPrefer: true,
    signatureAlgorithms: "rsa_pss_rsae_sha256:ECDSA+SHA256",
    cipherSuite: "AES256-SHA256",
  },
};
tlsTwoWay.connect(options).then(data => {
  console.log(JSON.stringify(data));
}).catch(err => {
  console.error(err);
});

let tlsOneWay = socket.constructTLSSocketInstance(); // One way authentication
tlsOneWay.bind({ address: '192.168.xxx.xxx', port: 8080, family: 1 }, err => {
  if (err) {
    console.log('bind fail');
    return;
  }
  console.log('bind success');
});
let oneWayOptions = {
  address: {
    address: "192.168.xxx.xxx",
    port: 8080,
    family: 1,
  },
  secureOptions: {
    ca: ["xxxx", "xxxx"],
    cipherSuite: "AES256-SHA256",
  },
};
tlsOneWay.connect(oneWayOptions).then(data => {
  console.log(JSON.stringify(data));
}).catch(err => {
  console.error(err);
});
```

### getRemoteAddress<sup>9+</sup>

getRemoteAddress(callback: AsyncCallback\<NetAddress\>): void

Obtains the remote address of a TLSSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                             | Mandatory| Description      |
| -------- | ------------------------------------------------- | ---- | ---------- |
| callback | AsyncCallback\<[NetAddress](#netaddress)\> | Yes  | Callback used to return the result. If the operation is successful, the remote address is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | -----------------------------  |
| 2303188 | Socket operation on non-socket.|
| 2300002 | System internal error.         |

**Example**

```js
tls.getRemoteAddress((err, data) => {
  if (err) {
    console.log('getRemoteAddress fail');
    return;
  }
  console.log('getRemoteAddress success:' + JSON.stringify(data));
});
```

### getRemoteAddress<sup>9+</sup>

getRemoteAddress(): Promise\<NetAddress\>

Obtains the remote address of a TLSSocket connection. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                                       | Description                                       |
| :------------------------------------------ | :------------------------------------------ |
| Promise\<[NetAddress](#netaddress)> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303188 | Socket operation on non-socket.|
| 2300002 | System internal error.         |

**Example**

```js
let promise = tls.getRemoteAddress();
promise.then(() => {
  console.log('getRemoteAddress success');
}).catch(err => {
  console.log('getRemoteAddress fail');
});
```

### getCertificate<sup>9+</sup>

getCertificate(callback: AsyncCallback\<[X509CertRawData](#x509certrawdata9)\>): void

Obtains the local digital certificate after a TLSSocket connection is established. This API is applicable to two-way authentication. It uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                  | Mandatory| Description|
| -------- | ----------------------------------------| ---- | ---------------|
| callback | AsyncCallback\<[X509CertRawData](#x509certrawdata9)\>    | Yes  | Callback used to return the result. If the operation is successful, the local certificate is returned. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2303504 | Error looking up x509.         |
| 2300002 | System internal error.         |

**Example**

```js
tls.getCertificate((err, data) => {
  if (err) {
    console.log("getCertificate callback error = " + err);
  } else {
    console.log("getCertificate callback = " + data);
  }
});
```

### getCertificate<sup>9+</sup>

getCertificate():Promise\<[X509CertRawData](#x509certrawdata9)\>

Obtains the local digital certificate after a TLSSocket connection is established. This API is applicable to two-way authentication. It uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type           | Description                 |
| -------------- | -------------------- |
| Promise\<[X509CertRawData](#x509certrawdata9)\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2303504 | Error looking up x509.         |
| 2300002 | System internal error.         |

**Example**

```js
tls.getCertificate().then(data => {
  console.log(data);
}).catch(err => {
  console.error(err);
});
```

### getRemoteCertificate<sup>9+</sup>

getRemoteCertificate(callback: AsyncCallback\<[X509CertRawData](#x509certrawdata9)\>): void

Obtains the digital certificate of the server after a TLSSocket connection is established. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name   | Type                                   | Mandatory | Description          |
| -------- | ----------------------------------------| ---- | ---------------|
| callback | AsyncCallback\<[X509CertRawData](#x509certrawdata9)\>  | Yes  | Callback used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2300002 | System internal error.         |

**Example**

```js
tls.getRemoteCertificate((err, data) => {
  if (err) {
    console.log("getRemoteCertificate callback error = " + err);
  } else {
    console.log("getRemoteCertificate callback = " + data);
  }
});
```

### getRemoteCertificate<sup>9+</sup>

getRemoteCertificate():Promise\<[X509CertRawData](#x509certrawdata9)\>

Obtains the digital certificate of the server after a TLSSocket connection is established. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type           | Description                 |
| -------------- | -------------------- |
| Promise\<[X509CertRawData](#x509certrawdata9)\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2300002 | System internal error.         |

**Example**

```js
tls.getRemoteCertificate().then(data => {
  console.log(data);
}).catch(err => {
  console.error(err);
});
```

### getProtocol<sup>9+</sup>

getProtocol(callback: AsyncCallback\<string\>): void

Obtains the communication protocol version after a TLSSocket connection is established. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                      | Mandatory| Description          |
| -------- | ----------------------------------------| ---- | ---------------|
| callback | AsyncCallback\<string\>                  | Yes  | Callback used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | -----------------------------  |
| 2303501 | SSL is null.                   |
| 2303505 | Error occurred in the tls system call. |
| 2300002 | System internal error.         |

**Example**

```js
tls.getProtocol((err, data) => {
  if (err) {
    console.log("getProtocol callback error = " + err);
  } else {
    console.log("getProtocol callback = " + data);
  }
});
```

### getProtocol<sup>9+</sup>

getProtocol():Promise\<string\>

Obtains the communication protocol version after a TLSSocket connection is established. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type           | Description                 |
| -------------- | -------------------- |
| Promise\<string\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2303505 | Error occurred in the tls system call. |
| 2300002 | System internal error.         |

**Example**

```js
tls.getProtocol().then(data => {
  console.log(data);
}).catch(err => {
  console.error(err);
});
```

### getCipherSuite<sup>9+</sup>

getCipherSuite(callback: AsyncCallback\<Array\<string\>\>): void

Obtains the cipher suite negotiated by both communication parties after a TLSSocket connection is established. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                    | Mandatory| Description|
| -------- | ----------------------------------------| ---- | ---------------|
| callback | AsyncCallback\<Array\<string\>\>          | Yes  | Callback used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2303502 | Error in tls reading.          |
| 2303505 | Error occurred in the tls system call. |
| 2300002 | System internal error.         |

**Example**

```js
tls.getCipherSuite((err, data) => {
  if (err) {
    console.log("getCipherSuite callback error = " + err);
  } else {
    console.log("getCipherSuite callback = " + data);
  }
});
```

### getCipherSuite<sup>9+</sup>

getCipherSuite(): Promise\<Array\<string\>\>

Obtains the cipher suite negotiated by both communication parties after a TLSSocket connection is established. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                   | Description                 |
| ---------------------- | --------------------- |
| Promise\<Array\<string\>\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2303502 | Error in tls reading.          |
| 2303505 | Error occurred in the tls system call. |
| 2300002 | System internal error.         |

**Example**

```js
tls.getCipherSuite().then(data => {
  console.log('getCipherSuite success:' + JSON.stringify(data));
}).catch(err => {
  console.error(err);
});
```

### getSignatureAlgorithms<sup>9+</sup>

getSignatureAlgorithms(callback: AsyncCallback\<Array\<string\>\>): void

Obtains the signing algorithm negotiated by both communication parties after a TLSSocket connection is established. This API is applicable to two-way authentication. It uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name  | Type                                  | Mandatory| Description           |
| -------- | -------------------------------------| ---- | ---------------|
| callback | AsyncCallback\<Array\<string\>\>         | Yes  | Callback used to return the result.  |

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2300002 | System internal error.         |

**Example**

```js
tls.getSignatureAlgorithms((err, data) => {
  if (err) {
    console.log("getSignatureAlgorithms callback error = " + err);
  } else {
    console.log("getSignatureAlgorithms callback = " + data);
  }
});
```

### getSignatureAlgorithms<sup>9+</sup>

getSignatureAlgorithms(): Promise\<Array\<string\>\>

Obtains the signing algorithm negotiated by both communication parties after a TLSSocket connection is established. This API is applicable to two-way authentication. It uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type                   | Description                 |
| ---------------------- | -------------------- |
| Promise\<Array\<string\>\> | Promise used to return the result.|

**Error codes**

| ID| Error Message                       |
| ------- | ------------------------------ |
| 2303501 | SSL is null.                   |
| 2300002 | System internal error.         |

**Example**

```js
tls.getSignatureAlgorithms().then(data => {
  console.log("getSignatureAlgorithms success" + data);
}).catch(err => {
  console.error(err);
});
```

### send<sup>9+</sup>

send(data: string, callback: AsyncCallback\<void\>): void

Sends a message to the server after a TLSSocket connection is established. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name   | Type                         | Mandatory| Description           |
| -------- | -----------------------------| ---- | ---------------|
|   data   | string                       | Yes  | Data content of the message to send.  |
| callback | AsyncCallback\<void\>         | Yes  | Callback used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                                     |
| ------- | -------------------------------------------- |
| 401     | Parameter error.                             |
| 2303501 | SSL is null.                                 |
| 2303503 | Error in tls writing.                         |
| 2303505 | Error occurred in the tls system call.       |
| 2303506 | Error clearing tls connection.               |
| 2300002 | System internal error.                       |

**Example**

```js
tls.send("xxxx", (err) => {
  if (err) {
    console.log("send callback error = " + err);
  } else {
    console.log("send success");
  }
});
```

### send<sup>9+</sup>

send(data: string): Promise\<void\>

Sends a message to the server after a TLSSocket connection is established. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name   | Type                         | Mandatory| Description           |
| -------- | -----------------------------| ---- | ---------------|
|   data   | string                       | Yes  | Data content of the message to send.  |

**Error codes**

| ID| Error Message                                     |
| ------- | -------------------------------------------- |
| 401     | Parameter error.                             |
| 2303501 | SSL is null.                                 |
| 2303503 | Error in tls writing.                         |
| 2303505 | Error occurred in the tls system call.       |
| 2303506 | Error clearing tls connection.               |
| 2300002 | System internal error.                       |

**Return value**

| Type          | Description                 |
| -------------- | -------------------- |
| Promise\<void\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Example**

```js
tls.send("xxxx").then(() => {
  console.log("send success");
}).catch(err => {
  console.error(err);
});
```

### close<sup>9+</sup>

close(callback: AsyncCallback\<void\>): void

Closes a TLSSocket connection. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Parameters**

| Name   | Type                         | Mandatory| Description           |
| -------- | -----------------------------| ---- | ---------------|
| callback | AsyncCallback\<void\>         | Yes  | Callback used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                                     |
| ------- | -------------------------------------------- |
| 401 | Parameter error.                                 |
| 2303501 | SSL is null.                                 |
| 2303505 | Error occurred in the tls system call.       |
| 2303506 | Error clearing tls connection.               |
| 2300002 | System internal error.                       |

**Example**

```js
tls.close((err) => {
  if (err) {
    console.log("close callback error = " + err);
  } else {
    console.log("close success");
  }
});
```

### close<sup>9+</sup>

close(): Promise\<void\>

Closes a TLSSocket connection. This API uses a promise to return the result.

**System capability**: SystemCapability.Communication.NetStack

**Return value**

| Type          | Description                 |
| -------------- | -------------------- |
| Promise\<void\> | Promise used to return the result. If the operation fails, an error message is returned.|

**Error codes**

| ID| Error Message                                     |
| ------- | -------------------------------------------- |
| 401 | Parameter error.                                 |
| 2303501 | SSL is null.                                 |
| 2303505 | Error occurred in the tls system call.       |
| 2303506 | Error clearing tls connection.               |
| 2300002 | System internal error.                       |

**Example**

```js
tls.close().then(() => {
  console.log("close success");
}).catch((err) => {
  console.error(err);
});
```

## TLSConnectOptions<sup>9+</sup>

Defines TLS connection options.

**System capability**: SystemCapability.Communication.NetStack

| Name         | Type                                    | Mandatory| Description           |
| -------------- | ------------------------------------- | ---  |-------------- |
| address        | [NetAddress](#netaddress)             | Yes |  Gateway address.      |
| secureOptions  | [TLSSecureOptions](#tlssecureoptions9) | Yes| TLS security options.|
| ALPNProtocols  | Array\<string\>                         | No| ALPN protocol. The value range is ["spdy/1", "http/1.1"]. The default value is **[]**.     |

## TLSSecureOptions<sup>9+</sup>

Defines TLS security options. The CA certificate is mandatory, and other parameters are optional. When **cert** (local certificate) and **key** (private key) are not empty, the two-way authentication mode is enabled. If **cert** or **key** is empty, one-way authentication is enabled.

**System capability**: SystemCapability.Communication.NetStack

| Name                | Type                                                   | Mandatory| Description                               |
| --------------------- | ------------------------------------------------------ | --- |----------------------------------- |
| ca                    | string \| Array\<string\>                               | Yes| CA certificate of the server, which is used to authenticate the digital certificate of the server.|
| cert                  | string                                                  | No| Digital certificate of the local client.                |
| key                   | string                                                  | No| Private key of the local digital certificate.                  |
| password                | string                                                  | No| Password for reading the private key.                     |
| protocols             | [Protocol](#protocol9) \|Array\<[Protocol](#protocol9)\> | No| TLS protocol version. The default value is **TLSv1.2**.                 |
| useRemoteCipherPrefer | boolean                                                 | No| Whether to use the remote cipher suite preferentially.       |
| signatureAlgorithms   | string                                                 | No| Signing algorithm used during communication. The default value is **""**.             |
| cipherSuite           | string                                                 | No| Cipher suite used during communication. The default value is **""**.             |

## Protocol<sup>9+</sup>

Enumerates TLS protocol versions.

**System capability**: SystemCapability.Communication.NetStack

| Name     |    Value   | Description               |
| --------- | --------- |------------------ |
| TLSv12    | "TLSv1.2" | TLSv1.2.|
| TLSv13    | "TLSv1.3" | TLSv1.3.|

## X509CertRawData<sup>9+</sup>

Defines the certificate raw data.

**System capability**: SystemCapability.Communication.NetStack