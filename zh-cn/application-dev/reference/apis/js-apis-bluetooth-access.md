# @ohos.bluetooth.access (蓝牙access模块)

access模块提供了打开和关闭蓝牙、获取蓝牙状态的方法。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```js
import access from '@ohos.bluetooth.access';
```


## access.enableBluetooth

enableBluetooth(): void

开启蓝牙。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台：** Android（AndroidAPI33以下支持）

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息            |
| -------- | ------------------ |
| 201 | Permission denied. |
|801 | Capability not supported.          |
|2900001   | Service stopped.   |
|2900099   | Operation failed.  |

**示例：**

```js
import access from '@ohos.bluetooth.access';
import { BusinessError } from '@ohos.base';

try {
    access.enableBluetooth();
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## access.disableBluetooth

disableBluetooth(): void

关闭蓝牙。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台：** Android（AndroidAPI33以下支持）

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

|错误码ID   | 错误信息           |
| -------- | ------------------ |
|201 | Permission denied. |
|801 | Capability not supported.          |
|2900001   | Service stopped.   |
|2900099   | Operation failed.  |

**示例：**

```js
import access from '@ohos.bluetooth.access';
import { BusinessError } from '@ohos.base';

try {
    access.disableBluetooth();
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## access.getState

getState(): BluetoothState

获取蓝牙开关状态。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台：** Android、iOS

**返回值：**

| 类型                              | 说明              |
| --------------------------------- | ---------------- |
| [BluetoothState](#bluetoothstate) | 表示蓝牙开关状态。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

|错误码ID   | 错误信息           |
| -------- | ------------------ |
|201 | Permission denied. |
|801 | Capability not supported.          |
|2900001   | Service stopped.   |
|2900099   | Operation failed.  |

**示例：**

```js
import access from '@ohos.bluetooth.access';
try {
    let state = access.getState();
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```

## access.on('stateChange')<a name="stateChange"></a>

on(type: "stateChange", callback: Callback&lt;BluetoothState&gt;): void

订阅蓝牙设备开关状态事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                               | 必填  | 说明                                                       |
| -------- | ------------------------------------------------- | ----- | ---------------------------------------------------------- |
| type     | string                                            | 是    | 填写"stateChange"字符串，表示蓝牙状态改变事件。               |
| callback | Callback&lt;[BluetoothState](#bluetoothstate)&gt; | 是    | 表示回调函数的入参，蓝牙状态。回调函数由用户创建通过该接口注册。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

|错误码ID   | 错误信息           |
| -------- | ------------------ |
|201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900099   | Operation failed.  |

**示例：**

```js
import access from '@ohos.bluetooth.access';
import { BusinessError } from '@ohos.base';

function onReceiveEvent(data: access.BluetoothState) {
    console.info('bluetooth state = '+ JSON.stringify(data));
}
try {
    access.on('stateChange', onReceiveEvent);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## access.off('stateChange')

off(type: "stateChange", callback?: Callback&lt;BluetoothState&gt;): void

取消订阅蓝牙设备开关状态事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台：** Android、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"stateChange"字符串，表示蓝牙状态改变事件。           |
| callback | Callback&lt;[BluetoothState](#bluetoothstate)&gt; | 否    | 表示取消订阅蓝牙状态改变事件上报。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900099 | Operation failed.                        |

**示例：**

```js
import access from '@ohos.bluetooth.access';
import { BusinessError } from '@ohos.base';

function onReceiveEvent(data: access.BluetoothState) {
    console.info('bluetooth state = '+ JSON.stringify(data));
}
try {
    access.on('stateChange', onReceiveEvent);
    access.off('stateChange', onReceiveEvent);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## BluetoothState

枚举，定义蓝牙开关状态。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                    | 值  | 说明                 | Android平台        | iOS平台   |
| --------------------- | ---- | ------------------ | ------------------ | ------------------ |
| STATE_OFF             | 0    | 表示蓝牙已关闭。          | 支持        | 支持        |
| STATE_TURNING_ON      | 1    | 表示蓝牙正在打开。          | 支持        | 不支持       |
| STATE_ON              | 2    | 表示蓝牙已打开。           | 支持         | 支持         |
| STATE_TURNING_OFF     | 3    | 表示蓝牙正在关闭。          | 支持        | 不支持       |

