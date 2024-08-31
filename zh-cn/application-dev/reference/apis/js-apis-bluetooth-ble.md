# @ohos.bluetooth.ble (蓝牙ble模块)

ble模块提供了对蓝牙操作和管理的方法。

> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。



## 导入模块

```js
import ble from '@ohos.bluetooth.ble'
```


## ble.createGattServer<a name="createGattServer"></a>

createGattServer(): GattServer

创建GattServer实例。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**返回值：**

| 类型                            | 说明         |
| ----------------------------- | ---------- |
| GattServer | 返回一个Gatt服务的实例。 |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let gattServer: ble.GattServer = ble.createGattServer();
```


## ble.createGattClientDevice<a name="createGattClientDevice"></a>

createGattClientDevice(deviceId: string): GattClientDevice

创建一个可使用的GattClientDevice实例。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型     | 必填   | 说明                                   |
| -------- | ------ | ---- | ------------------------------------ |
| deviceId | string | 是    | 对端设备地址，&nbsp;例如："XX:XX:XX:XX:XX:XX"。 |

**返回值：**

| 类型                                  | 说明                     |
| ------------------------------------- | ------------------------ |
| [GattClientDevice](#gattclientdevice) | 返回一个client端的实例。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX')
```


## ble.getConnectedBLEDevices<a name="getConnectedBLEDevices"></a>

getConnectedBLEDevices(): Array&lt;string&gt;

获取和当前设备连接的BLE设备。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS (当设备作为Server端时，Client端Notify订阅或读写后为连接状态)

**返回值：**

| 类型                | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| Array&lt;string&gt; | 返回当前设备作为Server端时连接BLE设备地址集合。基于信息安全考虑，此处获取的设备地址为随机MAC地址。配对成功后，该地址不会变更；已配对设备取消配对后重新扫描或蓝牙服务下电时，该随机地址会变更。<br /> |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let result: Array<string> = ble.getConnectedBLEDevices()
```


## ble.startBLEScan<a name="startBLEScan"></a>

startBLEScan(filters: Array&lt;ScanFilter&gt;, options?: ScanOptions): void

发起BLE扫描流程。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名     | 类型                                     | 必填   | 说明                                  |
| ------- | -------------------------------------- | ---- | ----------------------------------- |
| filters | Array&lt;[ScanFilter](#scanfilter)&gt; | 是    | 表示扫描结果过滤策略集合，如果不使用过滤的方式，该参数设置为null。 |
| options | [ScanOptions](#scanoptions)            | 否    | 表示扫描的参数配置，可选参数。                     |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
    let scanFilter: ble.ScanFilter = {
      //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
        deviceId:"XX:XX:XX:XX:XX:XX",
        name:"test",
        serviceUuid:"00001888-0000-1000-8000-00805f9b34fb"
    };
    let scanOptions: ble.ScanOptions = {
    	interval: 500,
    	dutyMode: ble.ScanDuty.SCAN_MODE_LOW_POWER,
    }
    ble.startBLEScan([scanFilter],scanOptions);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## ble.stopBLEScan<a name="stopBLEScan"></a>

stopBLEScan(): void

停止BLE扫描流程。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
   
ble.stopBLEScan();
```


## ble.startAdvertising<a name="startAdvertising"></a>

startAdvertising(setting: AdvertiseSetting, advData: AdvertiseData, advResponse?: AdvertiseData): void

开始发送BLE广播。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名         | 类型                                    | 必填   | 说明             |
| ----------- | ------------------------------------- | ---- | -------------- |
| setting     | [AdvertiseSetting](#advertisesetting) | 是    | BLE广播的相关参数。    |
| advData     | [AdvertiseData](#advertisedata)       | 是    | BLE广播包内容。      |
| advResponse | [AdvertiseData](#advertisedata)       | 否    | BLE回复扫描请求回复响应。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
    let setting: ble.AdvertiseSetting = {
        connectable: true,
    }
    let data: ArrayBuffer = ConvertStringToArrayBuffer('MD')
    let serviceDatas: ble.ServiceData[] = []
    let manufactureDatas: ble.ManufactureData[] = []
    serviceDatas.push({
        serviceUuid: "0000FFF1-0000-1000-8000-00805f9b34fb",
        serviceValue: data
    })
    manufactureDatas.push({
        manufactureId: 0xF1,
        manufactureValue: data
    })
    let advData: ble.AdvertiseData = {
        serviceUuids: ['0000FFF1-0000-1000-8000-00805F9B34FB'],
        manufactureData: manufactureDatas,
        serviceData: serviceDatas,
        includeDeviceName: true,
    }
    ble.startAdvertising(setting, advData)
} catch (err) {
    ('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## ble.stopAdvertising<a name="stopAdvertising"></a>

stopAdvertising(): void

停止发送BLE广播。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

ble.stopAdvertising();
```


## ble.startAdvertising<sup>11+</sup><a name="startAdvertising"></a>

startAdvertising(advertisingParams: AdvertisingParams, callback: AsyncCallback&lt;number&gt;): void

开始发送BLE广播。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名               | 类型                                    | 必填  | 说明                             |
| ------------------- | --------------------------------------- | ----- | ------------------------------- |
| advertisingParams   | [AdvertisingParams](#advertisingparams11) | 是    | 启动BLE广播的相关参数。           |
| callback            | AsyncCallback&lt;number&gt;             | 是    | 广播ID标识，通过注册回调函数获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | -------------------------------------- |
|401     | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                       |
|801     | Capability not supported.                |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let manufactureValueBuffer = new Uint8Array(4);
manufactureValueBuffer[0] = 1;
manufactureValueBuffer[1] = 2;
manufactureValueBuffer[2] = 3;
manufactureValueBuffer[3] = 4;

let serviceValueBuffer = new Uint8Array(4);
serviceValueBuffer[0] = 4;
serviceValueBuffer[1] = 6;
serviceValueBuffer[2] = 7;
serviceValueBuffer[3] = 8;
console.info('manufactureValueBuffer = '+ JSON.stringify(manufactureValueBuffer));
console.info('serviceValueBuffer = '+ JSON.stringify(serviceValueBuffer));
try {
  let setting: ble.AdvertiseSetting = {
    connectable:true,
  };
  let manufactureDataUnit: ble.ManufactureData = {
    manufactureId:4567,
    manufactureValue:manufactureValueBuffer.buffer
  };
  let serviceDataUnit: ble.ServiceData = {
    serviceUuid:"00001888-0000-1000-8000-00805f9b34fb",
    serviceValue:serviceValueBuffer.buffer
  };
  let advData: ble.AdvertiseData = {
    serviceUuids:["00001888-0000-1000-8000-00805f9b34fb"],
    manufactureData:[manufactureDataUnit],
    serviceData:[serviceDataUnit]
  };
  let advResponse: ble.AdvertiseData = {
    serviceUuids:["00001888-0000-1000-8000-00805f9b34fb"],
    manufactureData:[manufactureDataUnit],
    serviceData:[serviceDataUnit]
  };
  let advertisingParams: ble.AdvertisingParams = {
    advertisingSettings: setting,
    advertisingData: advData,
    advertisingResponse: advResponse,
  }
  let advHandle = 0xFF;
  ble.startAdvertising(advertisingParams, (err, outAdvHandle) => {
    if (err) {
      return;
    } else {
      advHandle = outAdvHandle;
      console.info("advHandle: " + advHandle);
    }
  });
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## ble.startAdvertising<sup>11+</sup><a name="startAdvertising"></a>

startAdvertising(advertisingParams: AdvertisingParams): Promise&lt;number&gt;

开始发送BLE广播。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名               | 类型                                   | 必填  | 说明                    |
| ------------------- | -------------------------------------- | ----- | ----------------------- |
| advertisingParams   | [AdvertisingParams](#advertisingparams11) | 是    | 启动BLE广播的相关参数。  |

**返回值：**

| 类型                       | 说明                            |
| -------------------------- | ------------------------------- |
| Promise&lt;number&gt;      | 广播ID标识，通过promise形式获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | -------------------------------------- |
|401     | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                       |
|801     | Capability not supported.                |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let manufactureValueBuffer = new Uint8Array(4);
manufactureValueBuffer[0] = 1;
manufactureValueBuffer[1] = 2;
manufactureValueBuffer[2] = 3;
manufactureValueBuffer[3] = 4;

let serviceValueBuffer = new Uint8Array(4);
serviceValueBuffer[0] = 4;
serviceValueBuffer[1] = 6;
serviceValueBuffer[2] = 7;
serviceValueBuffer[3] = 8;
console.info('manufactureValueBuffer = '+ JSON.stringify(manufactureValueBuffer));
console.info('serviceValueBuffer = '+ JSON.stringify(serviceValueBuffer));
try {
  let setting: ble.AdvertiseSetting = {
    connectable:true
  };
  let manufactureDataUnit: ble.ManufactureData = {
    manufactureId:4567,
    manufactureValue:manufactureValueBuffer.buffer
  };
  let serviceDataUnit: ble.ServiceData = {
    serviceUuid:"00001888-0000-1000-8000-00805f9b34fb",
    serviceValue:serviceValueBuffer.buffer
  };
  let advData: ble.AdvertiseData = {
    serviceUuids:["00001888-0000-1000-8000-00805f9b34fb"],
    manufactureData:[manufactureDataUnit],
    serviceData:[serviceDataUnit]
  };
  let advResponse: ble.AdvertiseData = {
    serviceUuids:["00001888-0000-1000-8000-00805f9b34fb"],
    manufactureData:[manufactureDataUnit],
    serviceData:[serviceDataUnit]
  };
  let advertisingParams: ble.AdvertisingParams = {
    advertisingSettings: setting,
    advertisingData: advData,
    advertisingResponse: advResponse,
  }
  let advHandle = 0xFF;
  ble.startAdvertising(advertisingParams)
    .then(outAdvHandle => {
      advHandle = outAdvHandle;
    });
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```

## ble.stopAdvertising<sup>11+</sup><a name="stopAdvertising"></a>

stopAdvertising(advertisingId: number, callback: AsyncCallback&lt;void&gt;): void

停止发送BLE广播。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名                    | 类型                          | 必填  | 说明                         |
| ------------------------- | ---------------------------- | ----- | --------------------------- |
| advertisingId             | number                       | 是    | 需要停止的广播ID标识。        |
| callback                  | AsyncCallback&lt;void&gt;    | 是    | 回调函数。                   |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401     | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.                  |
|801     | Capability not supported.                |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let advHandle = 0xFF;
ble.stopAdvertising(advHandle, (err) => {
  if (err) {
    return;
  }
});
```


## ble.stopAdvertising<sup>11+</sup><a name="stopAdvertising"></a>

stopAdvertising(advertisingId: number): Promise&lt;void&gt;

停止发送BLE广播。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名                    | 类型                          | 必填  | 说明                         |
| ------------------------- | ---------------------------- | ----- | --------------------------- |
| advertisingId             | number                       | 是    | 需要停止的广播ID标识。        |

**返回值：**

| 类型                       | 说明          |
| -------------------------- | ------------ |
| Promise&lt;void&gt;        | 回调函数。    |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401     | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types.                 |
|801     | Capability not supported.                |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let advHandle = 0xFF;
ble.stopAdvertising(advHandle)
  .then(() => {
    console.info("enable success");
  });
```


## ble.on('advertisingStateChange')<sup>11+</sup>

on(type: 'advertisingStateChange', callback: Callback&lt;AdvertisingStateChangeInfo&gt;): void

订阅BLE广播状态。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                                                    | 必填   | 说明                                                      |
| -------- | ------------------------------------------------------------------------- | ----- | ---------------------------------------------------------- |
| type     | string                                                                    | 是    | 填写"advertisingStateChange"字符串，表示广播状态事件。        |
| callback | Callback&lt;[AdvertisingStateChangeInfo](#advertisingstatechangeinfo11)&gt; | 是    | 表示回调函数的入参，广播状态。回调函数由用户创建通过该接口注册。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401     | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                       |
|801     | Capability not supported.                |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

function onReceiveEvent(data: ble.AdvertisingStateChangeInfo) {
  console.info('bluetooth advertising state = ' + JSON.stringify(data));
}
try {
  ble.on('advertisingStateChange', onReceiveEvent);
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## ble.off('advertisingStateChange')<sup>11+</sup>

off(type: 'advertisingStateChange', callback?: Callback&lt;AdvertisingStateChangeInfo&gt;): void

取消订阅BLE广播状态。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                                                    | 必填   | 说明                                                      |
| -------- | ------------------------------------------------------------------------- | ----- | ---------------------------------------------------------- |
| type     | string                                                                    | 是    | 填写"advertisingStateChange"字符串，表示广播状态事件。        |
| callback | Callback&lt;[AdvertisingStateChangeInfo](#advertisingstatechangeinfo11)&gt; | 否    | 表示取消订阅广播状态上报。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401     | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                       |
|801     | Capability not supported.                |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

function onReceiveEvent(data: ble.AdvertisingStateChangeInfo) {
  console.info('bluetooth advertising state = ' + JSON.stringify(data));
}
try {
  ble.on('advertisingStateChange', onReceiveEvent);
  ble.off('advertisingStateChange', onReceiveEvent);
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## ble.on('BLEDeviceFind')

on(type: 'BLEDeviceFind', callback: Callback&lt;Array&lt;ScanResult&gt;&gt;): void

订阅BLE设备发现上报事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                  |
| -------- | ---------------------------------------- | ---- | ----------------------------------- |
| type     | string                                   | 是    | 填写"BLEDeviceFind"字符串，表示BLE设备发现事件。   |
| callback | Callback&lt;Array&lt;[ScanResult](#scanresult)&gt;&gt; | 是    | 表示回调函数的入参，发现的设备集合。回调函数由用户创建通过该接口注册。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

function onReceiveEvent(data: Array<ble.ScanResult>) {
  console.info('bluetooth device find = '+ JSON.stringify(data));
}
try {
  ble.on('BLEDeviceFind', onReceiveEvent);
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## ble.off('BLEDeviceFind')

off(type: 'BLEDeviceFind', callback?: Callback&lt;Array&lt;ScanResult&gt;&gt;): void

取消订阅BLE设备发现上报事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"BLEDeviceFind"字符串，表示BLE设备发现事件。        |
| callback | Callback&lt;Array&lt;[ScanResult](#scanresult)&gt;&gt; | 否    | 表示取消订阅BLE设备发现事件上报。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。


| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

function onReceiveEvent(data: Array<ble.ScanResult>) {
  console.info('bluetooth device find = '+ JSON.stringify(data));
}
try {
  ble.on('BLEDeviceFind', onReceiveEvent);
  ble.off('BLEDeviceFind', onReceiveEvent);
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


## GattServer

server端类，使用server端方法之前需要创建该类的实例进行操作，通过createGattServer()方法构造此实例。


### addService

addService(service: GattService): void

server端添加服务。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名  | 类型                        | 必填 | 说明                |
| ------- | --------------------------- | ---- | ------------------- |
| service | [GattService](#gattservice) | 是   | 服务端的service数据 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

// 创建descriptors
let descriptors: Array<ble.BLEDescriptor> = [];
let arrayBuffer = new ArrayBuffer(8);
let descV = new Uint8Array(arrayBuffer);
descV[0] = 11;
let descriptor: ble.BLEDescriptor = {
  serviceUuid: '00001810-0000-1000-8000-00805F9B34FB',
  characteristicUuid: '00001820-0000-1000-8000-00805F9B34FB',
  descriptorUuid: '00002902-0000-1000-8000-00805F9B34FB',
  descriptorValue: arrayBuffer
};
descriptors[0] = descriptor;

// 创建characteristics
let characteristics: Array<ble.BLECharacteristic> = [];
let arrayBufferC = new ArrayBuffer(8);
let cccV = new Uint8Array(arrayBufferC);
cccV[0] = 1;
let characteristic: ble.BLECharacteristic = {
  serviceUuid: '00001810-0000-1000-8000-00805F9B34FB',
  characteristicUuid: '00001820-0000-1000-8000-00805F9B34FB',
  characteristicValue: arrayBufferC,
  descriptors: descriptors
};
let characteristicN: ble.BLECharacteristic = {
  serviceUuid: '00001810-0000-1000-8000-00805F9B34FB',
  characteristicUuid: '00001821-0000-1000-8000-00805F9B34FB',
  characteristicValue: arrayBufferC,
  descriptors: descriptors
};
characteristics[0] = characteristic;

// 创建gattService
let gattService: ble.GattService = {
  serviceUuid: '00001810-0000-1000-8000-00805F9B34FB',
  isPrimary: true,
  characteristics: characteristics,
};

try {
  let gattServer: ble.GattServer = ble.createGattServer();
  gattServer.addService(gattService);
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### removeService

removeService(serviceUuid: string): void

删除已添加的服务。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名         | 类型     | 必填   | 说明                                       |
| ----------- | ------ | ---- | ---------------------------------------- |
| serviceUuid | string | 是    | service的UUID，例如“00001810-0000-1000-8000-00805F9B34FB”。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900004 | Profile not supported.                |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let server: ble.GattServer = ble.createGattServer();
try {
  // 调用removeService接口前需要完成server端和client端的配对及连接。
  server.removeService('00001810-0000-1000-8000-00805F9B34FB');
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### close

close(): void

关闭服务端，调用该接口后[GattServer](#gattserver)实例将不能再使用。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|801 | Capability not supported.          |
|2900001 | Service stopped.                         |
|2900003 | Bluetooth disabled.                 |
|2900099 | Operation failed.                        |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let server: ble.GattServer = ble.createGattServer();
server.close();
```

### notifyCharacteristicChanged

notifyCharacteristicChanged(deviceId: string, notifyCharacteristic: NotifyCharacteristic, callback: AsyncCallback<void>): void

server端特征值发生变化时，主动通知已连接的client设备。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**：iOS (通知已连接的client设备的特征值value为发送广播添加服务时service中的对应特征值value，不是notifyCharacteristic中设置的value。)

**参数：**

| 参数名               | 类型                                                         | 必填 | 说明                                                   |
| :------------------- | :----------------------------------------------------------- | :--- | :----------------------------------------------------- |
| deviceId             | string                                                       | 是   | 接收通知的client端设备地址，例如“XX:XX:XX:XX:XX:XX”。  |
| notifyCharacteristic | [NotifyCharacteristic](#notifycharacteristic) | 是   | 通知的特征值数据。                                     |
| callback             | AsyncCallback<void>                                          | 是   | 回调函数。当通知成功，err为undefined，否则为错误对象。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                 |
| :------- | :----------------------- |
| 201      | Permission denied.       |
| 2900001  | Service stopped.         |
| 2900003  | Bluetooth switch is off. |
| 2900099  | Operation failed.        |

**示例：**

```js
import { BusinessError } from '@ohos.base';
let arrayBufferC = new ArrayBuffer(8);
let notifyCharacter: ble.NotifyCharacteristic = {
    serviceUuid: '00001810-0000-1000-8000-00805F9B34FB',
    characteristicUuid: '00001820-0000-1000-8000-00805F9B34FB',
    characteristicValue: arrayBufferC,
    confirm: true,
};
try {
    let gattServer: ble.GattServer = ble.createGattServer();
    gattServer.notifyCharacteristicChanged('XX:XX:XX:XX:XX:XX', notifyCharacter, (err: BusinessError) => {
        if (err) {
            console.info('notifyCharacteristicChanged callback failed');
        } else {
            console.info('notifyCharacteristicChanged callback successful');
        }
    });
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```

### notifyCharacteristicChanged

notifyCharacteristicChanged(deviceId: string, notifyCharacteristic: NotifyCharacteristic): Promise<void>

server端特征值发生变化时，主动通知已连接的client设备。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**：iOS （通知已连接的client设备的特征值value为发送广播添加服务时service中的对应特征值value，不是notifyCharacteristic中设置的value。）

**参数：**

| 参数名               | 类型                                                         | 必填 | 说明                                                  |
| :------------------- | :----------------------------------------------------------- | :--- | :---------------------------------------------------- |
| deviceId             | string                                                       | 是   | 接收通知的client端设备地址，例如“XX:XX:XX:XX:XX:XX”。 |
| notifyCharacteristic | [NotifyCharacteristic](#notifycharacteristic) | 是   | 通知的特征值数据。                                    |

**返回值：**

| 类型          | 说明              |
| :------------ | :---------------- |
| Promise<void> | 返回promise对象。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                 |
| :------- | :----------------------- |
| 201      | Permission denied.       |
| 2900001  | Service stopped.         |
| 2900003  | Bluetooth switch is off. |
| 2900099  | Operation failed.        |

**示例：**

```js
import { BusinessError } from '@ohos.base';
let arrayBufferC = new ArrayBuffer(8);
let notifyCharacter: ble.NotifyCharacteristic = {
    serviceUuid: '00001810-0000-1000-8000-00805F9B34FB',
    characteristicUuid: '00001820-0000-1000-8000-00805F9B34FB',
    characteristicValue: arrayBufferC,
    confirm: true,
};
try {
    let gattServer: ble.GattServer = ble.createGattServer();
    gattServer.notifyCharacteristicChanged('XX:XX:XX:XX:XX:XX', notifyCharacter).then(() => {
        console.info('notifyCharacteristicChanged promise successful');
    });
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
js
```

### sendResponse

sendResponse(serverResponse: ServerResponse): void

server端回复client端的读写请求。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**：iOS

**参数：**

| 参数名         | 类型                                                         | 必填 | 说明                     |
| :------------- | :----------------------------------------------------------- | :--- | :----------------------- |
| serverResponse | [ServerResponse](#serverresponse) | 是   | server端回复的响应数据。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                 |
| :------- | :----------------------- |
| 201      | Permission denied.       |
| 2900001  | Service stopped.         |
| 2900003  | Bluetooth switch is off. |
| 2900099  | Operation failed.        |

**示例：**

```js
import { BusinessError } from '@ohos.base';
/* send response */
let arrayBufferCCC = new ArrayBuffer(8);
let cccValue = new Uint8Array(arrayBufferCCC);
cccValue[0] = 1123;
let serverResponse: ble.ServerResponse = {
    deviceId: 'XX:XX:XX:XX:XX:XX',
    transId: 0,
    status: 0,
    offset: 0,
    value: arrayBufferCCC,
};
try {
    let gattServer: ble.GattServer = ble.createGattServer();
    gattServer.sendResponse(serverResponse);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```



### on('characteristicRead')

on(type: 'characteristicRead', callback: Callback&lt;CharacteristicReadRequest&gt;): void

server端订阅特征值读请求事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                    |
| -------- | ---------------------------------------- | ---- | ------------------------------------- |
| type     | string                                   | 是    | 填写"characteristicRead"字符串，表示特征值读请求事件。 |
| callback | Callback&lt;[CharacteristicReadRequest](#characteristicreadrequest)&gt; | 是    | 表示回调函数的入参，client端发送的读请求数据。            |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let arrayBufferCCC = new ArrayBuffer(8);
let cccValue = new Uint8Array(arrayBufferCCC);
cccValue[0] = 1123;
let gattServer: ble.GattServer = ble.createGattServer();
function ReadCharacteristicReq(characteristicReadRequest: ble.CharacteristicReadRequest) {
  let deviceId: string = characteristicReadRequest.deviceId;
  let transId: number = characteristicReadRequest.transId;
  let characteristicUuid: string = characteristicReadRequest.characteristicUuid;
}
gattServer.on('characteristicRead', ReadCharacteristicReq);
```


### off('characteristicRead')

off(type: 'characteristicRead', callback?: Callback&lt;CharacteristicReadRequest&gt;): void

server端取消订阅特征值读请求事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"characteristicRead"字符串，表示特征值读请求事件。    |
| callback | Callback&lt;[CharacteristicReadRequest](#characteristicreadrequest)&gt; | 否    | 表示取消订阅特征值读请求事件上报。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let gattServer: ble.GattServer = ble.createGattServer();
gattServer.off('characteristicRead');
```


### on('characteristicWrite')

on(type: 'characteristicWrite', callback: Callback&lt;CharacteristicWriteRequest&gt;): void

server端订阅特征值写请求事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                     |
| -------- | ---------------------------------------- | ---- | -------------------------------------- |
| type     | string                                   | 是    | 填写"characteristicWrite"字符串，表示特征值写请求事件。 |
| callback | Callback&lt;[CharacteristicWriteRequest](#characteristicwriterequest)&gt; | 是    | 表示回调函数的入参，client端发送的写请求数据。             |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let arrayBufferCCC = new ArrayBuffer(8);
let cccValue = new Uint8Array(arrayBufferCCC);
let gattServer: ble.GattServer = ble.createGattServer();
function WriteCharacteristicReq(characteristicWriteRequest: ble.CharacteristicWriteRequest) {
  let deviceId: string = characteristicWriteRequest.deviceId;
  let transId: number = characteristicWriteRequest.transId;
  let value: Uint8Array =  new Uint8Array(characteristicWriteRequest.value);
  let characteristicUuid: string = characteristicWriteRequest.characteristicUuid;
}
gattServer.on('characteristicWrite', WriteCharacteristicReq);
```


### off('characteristicWrite')

off(type: 'characteristicWrite', callback?: Callback&lt;CharacteristicWriteRequest&gt;): void

server端取消订阅特征值写请求事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"characteristicWrite"字符串，表示特征值写请求事件。   |
| callback | Callback&lt;[CharacteristicWriteRequest](#characteristicwriterequest)&gt; | 否    | 表示取消订阅特征值写请求事件上报。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let gattServer: ble.GattServer = ble.createGattServer();
gattServer.off('characteristicWrite');
```


### on('descriptorRead')

on(type: 'descriptorRead', callback: Callback&lt;DescriptorReadRequest&gt;): void

server端订阅描述符读请求事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                |
| -------- | ---------------------------------------- | ---- | --------------------------------- |
| type     | string                                   | 是    | 填写"descriptorRead"字符串，表示描述符读请求事件。 |
| callback | Callback&lt;[DescriptorReadRequest](#descriptorreadrequest)&gt; | 是    | 表示回调函数的入参，client端发送的读请求数据。        |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let arrayBufferDesc = new ArrayBuffer(8);
let descValue = new Uint8Array(arrayBufferDesc);
descValue[0] = 1101;
let gattServer: ble.GattServer = ble.createGattServer();
function ReadDescriptorReq(descriptorReadRequest: ble.DescriptorReadRequest) {
  let deviceId: string = descriptorReadRequest.deviceId;
  let transId: number = descriptorReadRequest.transId;
  let descriptorUuid: string = descriptorReadRequest.descriptorUuid;

}
gattServer.on('descriptorRead', ReadDescriptorReq);
```


### off('descriptorRead')

off(type: 'descriptorRead', callback?: Callback&lt;DescriptorReadRequest&gt;): void

server端取消订阅描述符读请求事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"descriptorRead"字符串，表示描述符读请求事件。        |
| callback | Callback&lt;[DescriptorReadRequest](#descriptorreadrequest)&gt; | 否    | 表示取消订阅描述符读请求事件上报。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let gattServer: ble.GattServer = ble.createGattServer();
gattServer.off('descriptorRead');
```


### on('descriptorWrite')

on(type: 'descriptorWrite', callback: Callback&lt;DescriptorWriteRequest&gt;): void

server端订阅描述符写请求事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                 |
| -------- | ---------------------------------------- | ---- | ---------------------------------- |
| type     | string                                   | 是    | 填写"descriptorWrite"字符串，表示描述符写请求事件。 |
| callback | Callback&lt;[DescriptorWriteRequest](#descriptorwriterequest)&gt; | 是    | 表示回调函数的入参，client端发送的写请求数据。         |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let arrayBufferDesc = new ArrayBuffer(8);
let descValue = new Uint8Array(arrayBufferDesc);
let gattServer: ble.GattServer = ble.createGattServer();
function WriteDescriptorReq(descriptorWriteRequest: ble.DescriptorWriteRequest) {
  let deviceId: string = descriptorWriteRequest.deviceId;
  let transId: number = descriptorWriteRequest.transId;
  let value: Uint8Array = new Uint8Array(descriptorWriteRequest.value);
  let descriptorUuid: string = descriptorWriteRequest.descriptorUuid;

  descValue[0] = value[0];

}
gattServer.on('descriptorWrite', WriteDescriptorReq);
```


### off('descriptorWrite')

off(type: 'descriptorWrite', callback?: Callback&lt;DescriptorWriteRequest&gt;): void

server端取消订阅描述符写请求事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"descriptorWrite"字符串，表示描述符写请求事件。       |
| callback | Callback&lt;[DescriptorWriteRequest](#descriptorwriterequest)&gt; | 否    | 表示取消订阅描述符写请求事件上报。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let gattServer: ble.GattServer = ble.createGattServer();
gattServer.off('descriptorWrite');
```


### on('connectionStateChange')

on(type: 'connectionStateChange', callback: Callback&lt;BLEConnectionChangeState&gt;): void

server端订阅BLE连接状态变化事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS (当设备作为Server端时，Client端Notify订阅或读写后为连接状态，取消订阅时为断开连接状态。)

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"connectionStateChange"字符串，表示BLE连接状态变化事件。 |
| callback | Callback&lt;[BLEConnectionChangeState](#bleconnectionchangestate)&gt; | 是    | 表示回调函数的入参，连接状态。                          |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'
import constant from '@ohos.bluetooth.constant'

let Connected = (bleConnectionChangeState: ble.BLEConnectionChangeState) => {
  let deviceId: string = bleConnectionChangeState.deviceId;
  let status: constant.ProfileConnectionState = bleConnectionChangeState.state;
}
try {
  let gattServer: ble.GattServer = ble.createGattServer();
  gattServer.on('connectionStateChange', Connected);
} catch (err) {
  console.error("errCode:" + (err as BusinessError).code + ",errMessage:" + (err as BusinessError).message);
}
```


### off('connectionStateChange')

off(type: 'connectionStateChange', callback?: Callback&lt;BLEConnectionChangeState&gt;): void

server端取消订阅BLE连接状态变化事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 填写"connectionStateChange"字符串，表示BLE连接状态变化事件。 |
| callback | Callback&lt;[BLEConnectionChangeState](#bleconnectionchangestate)&gt; | 否    | 表示取消订阅BLE连接状态变化事件。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let gattServer: ble.GattServer = ble.createGattServer();
gattServer.off('connectionStateChange');
```


### on('BLEMtuChange')

on(type: 'BLEMtuChange', callback: Callback&lt;number&gt;): void

server端订阅MTU状态变化事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 必须填写"BLEMtuChange"字符串，表示MTU状态变化事件。填写不正确将导致回调无法注册。 |
| callback | Callback&lt;number&gt; | 是    | 返回MTU字节数的值，通过注册回调函数获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  let gattServer: ble.GattServer = ble.createGattServer();
  gattServer.on('BLEMtuChange', (mtu: number) => {
    console.info('BLEMtuChange, mtu: ' + mtu);
  });
} catch (err) {
  console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### off('BLEMtuChange')

off(type: 'BLEMtuChange', callback?: Callback&lt;number&gt;): void

server端取消订阅MTU状态变化事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名      | 类型                                       | 必填   | 说明                                       |
| -------- | ---------------------------------------- | ---- | ---------------------------------------- |
| type     | string                                   | 是    | 必须填写"BLEMtuChange"字符串，表示MTU状态变化事件。填写不正确将导致回调无法注册。 |
| callback | Callback&lt;number&gt; | 否    | 返回MTU字节数的值，通过注册回调函数获取。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
| 201 | Permission denied. |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                 |
|801 | Capability not supported.          |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'

let gattServer: ble.GattServer = ble.createGattServer();
gattServer.off('BLEMtuChange');
```

## GattClientDevice

client端类，使用client端方法之前需要创建该类的实例进行操作，通过createGattClientDevice(deviceId: string)方法构造此实例。

### connect

connect(): void

client端发起连接远端蓝牙低功耗设备。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 201      | Permission denied.        |
| 801      | Capability not supported. |
| 2900001  | Service stopped.          |
| 2900003  | Bluetooth disabled.       |
| 2900099  | Operation failed.         |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  	//android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.connect();
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```

### disconnect

disconnect(): void

client端断开与远端蓝牙低功耗设备的连接。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 201      | Permission denied.        |
| 801      | Capability not supported. |
| 2900001  | Service stopped.          |
| 2900003  | Bluetooth disabled.       |
| 2900099  | Operation failed.         |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  	//android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.disconnect();
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### close

close(): void

关闭客户端功能，注销client在协议栈的注册，调用该接口后[GattClientDevice](#gattclientdevice)实例将不能再使用。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 201      | Permission denied.        |
| 801      | Capability not supported. |
| 2900001  | Service stopped.          |
| 2900003  | Bluetooth disabled.       |
| 2900099  | Operation failed.         |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.close();
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### getDeviceName

getDeviceName(callback: AsyncCallback&lt;string&gt;): void

client获取远端蓝牙低功耗设备名。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                               |
| -------- | --------------------------- | ---- | -------------------------------------------------- |
| callback | AsyncCallback&lt;string&gt; | 是   | client获取对端server设备名，通过注册回调函数获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let gattClient: ble.GattClientDevice = ble.createGattClientDevice("XX:XX:XX:XX:XX:XX");
    gattClient.connect();
    gattClient.getDeviceName((err: BusinessError, data: string)=> {
        console.info('device name err ' + JSON.stringify(err));
        console.info('device name' + JSON.stringify(data));
    })
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### getDeviceName

getDeviceName(): Promise&lt;string&gt;

client获取远端蓝牙低功耗设备名。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**返回值：**

| 类型                  | 说明                                              |
| --------------------- | ------------------------------------------------- |
| Promise&lt;string&gt; | client获取对端server设备名，通过promise形式获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let gattClient: ble.GattClientDevice = ble.createGattClientDevice("XX:XX:XX:XX:XX:XX");
    gattClient.connect();
    gattClient.getDeviceName().then((data: string) => {
        console.info('device name' + JSON.stringify(data));
    })
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### getServices

getServices(callback: AsyncCallback&lt;Array&lt;GattService&gt;&gt;): void

client端获取蓝牙低功耗设备的所有服务，即服务发现。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                       |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------ |
| callback | AsyncCallback&lt;Array&lt;[GattService](#gattservice)&gt;&gt; | 是   | client进行服务发现，通过注册回调函数获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.connect();
    g_device.getServices((err: BusinessError, result: Array<ble.GattService>) => {
        if (err != null) {
            console.info('GattClinet getDeviceName callback name ' + JSON.stringify(err));
        }
        console.info('getServices callback successfully:' + JSON.stringify(result));
    });
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### getServices

getServices(): Promise&lt;Array&lt;GattService&gt;&gt;

client端获取蓝牙低功耗设备的所有服务，即服务发现。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**返回值：**

| 类型                                                    | 说明                                      |
| ------------------------------------------------------- | ----------------------------------------- |
| Promise&lt;Array&lt;[GattService](#gattservice)&gt;&gt; | client进行服务发现，通过promise形式获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.connect();
    device.getServices().then((result: Array<ble.GattService>) => {
        console.info('getServices successfully:' + JSON.stringify(result));
    });
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### readCharacteristicValue

readCharacteristicValue(characteristic: BLECharacteristic, callback: AsyncCallback&lt;BLECharacteristic&gt;): void

client端读取蓝牙低功耗设备特定服务的特征值。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名         | 类型                                                         | 必填 | 说明                                     |
| -------------- | ------------------------------------------------------------ | ---- | ---------------------------------------- |
| characteristic | [BLECharacteristic](#blecharacteristic)                      | 是   | 待读取的特征值。                         |
| callback       | AsyncCallback&lt;[BLECharacteristic](#blecharacteristic)&gt; | 是   | client读取特征值，通过注册回调函数获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901000  | Read forbidden.                                              |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let descriptors: Array<ble.BLEDescriptor> = [];
  let bufferDesc = new ArrayBuffer(8);
  let descV = new Uint8Array(bufferDesc);
  descV[0] = 11;
  let descriptor: ble.BLEDescriptor = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX', descriptorValue: bufferDesc
  };
  descriptors[0] = descriptor;

  let bufferCCC = new ArrayBuffer(8);
  let cccV = new Uint8Array(bufferCCC);
  let characteristic: ble.BLECharacteristic = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicValue: bufferCCC, descriptors:descriptors
  };

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.readCharacteristicValue(characteristic, (err, data) => {
        if (err) {
            console.error('readCharacteristicValue errCode: ' + 
            	(err as BusinessError).code + ', errMessage: ' 	+ (err as BusinessError).message);
        }
        console.info('readCharacteristicValue successfully:' + JSON.stringify(data));
    });
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### readCharacteristicValue

readCharacteristicValue(characteristic: BLECharacteristic): Promise&lt;BLECharacteristic&gt;

client端读取蓝牙低功耗设备特定服务的特征值。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名         | 类型                                    | 必填 | 说明             |
| -------------- | --------------------------------------- | ---- | ---------------- |
| characteristic | [BLECharacteristic](#blecharacteristic) | 是   | 待读取的特征值。 |

**返回值：**

| 类型                                                   | 说明                                    |
| ------------------------------------------------------ | --------------------------------------- |
| Promise&lt;[BLECharacteristic](#blecharacteristic)&gt; | client读取特征值，通过promise形式获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901000  | Read forbidden.                                              |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let descriptors: Array<ble.BLEDescriptor> = [];
  let bufferDesc = new ArrayBuffer(8);
  let descV = new Uint8Array(bufferDesc);
  descV[0] = 11;
  let descriptor: ble.BLEDescriptor = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX', descriptorValue: bufferDesc
  };
  descriptors[0] = descriptor;

  let bufferCCC = new ArrayBuffer(8);
  let cccV = new Uint8Array(bufferCCC);
  let characteristic: ble.BLECharacteristic = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicValue: bufferCCC, descriptors:descriptors
  };

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.readCharacteristicValue(characteristic);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### readDescriptorValue

readDescriptorValue(descriptor: BLEDescriptor, callback: AsyncCallback&lt;BLEDescriptor&gt;): void

client端读取蓝牙低功耗设备特定的特征包含的描述符。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名     | 类型                                                 | 必填 | 说明                                     |
| ---------- | ---------------------------------------------------- | ---- | ---------------------------------------- |
| descriptor | [BLEDescriptor](#bledescriptor)                      | 是   | 待读取的描述符。                         |
| callback   | AsyncCallback&lt;[BLEDescriptor](#bledescriptor)&gt; | 是   | client读取描述符，通过注册回调函数获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901000  | Read forbidden.                                              |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let bufferDesc = new ArrayBuffer(8);
let descV = new Uint8Array(bufferDesc);
descV[0] = 11;
let descriptor: ble.BLEDescriptor = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorValue: bufferDesc
};
//android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
device.readDescriptorValue(descriptor, (err, data) => {
    if (err) {
        console.error('readDescriptorValue errCode: ' +
        	(err as BusinessError).code + ', errMessage: ' 	+ (err as BusinessError).message);
    }
    console.info('readDescriptorValue successfully:' + JSON.stringify(data));
});
```


### readDescriptorValue

readDescriptorValue(descriptor: BLEDescriptor): Promise&lt;BLEDescriptor&gt;

client端读取蓝牙低功耗设备特定的特征包含的描述符。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名     | 类型                            | 必填 | 说明             |
| ---------- | ------------------------------- | ---- | ---------------- |
| descriptor | [BLEDescriptor](#bledescriptor) | 是   | 待读取的描述符。 |

**返回值：**

| 类型                                           | 说明                                    |
| ---------------------------------------------- | --------------------------------------- |
| Promise&lt;[BLEDescriptor](#bledescriptor)&gt; | client读取描述符，通过promise形式获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901000  | Read forbidden.                                              |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let bufferDesc = new ArrayBuffer(8);
let descV = new Uint8Array(bufferDesc);
descV[0] = 11;
let descriptor: ble.BLEDescriptor = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorValue: bufferDesc
};
try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.readDescriptorValue(descriptor);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### writeCharacteristicValue

writeCharacteristicValue(characteristic: BLECharacteristic, writeType: GattWriteType, callback: AsyncCallback&lt;void&gt;): void

client端向低功耗蓝牙设备写入特定的特征值。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名         | 类型                                    | 必填 | 说明                                                   |
| -------------- | --------------------------------------- | ---- | ------------------------------------------------------ |
| characteristic | [BLECharacteristic](#blecharacteristic) | 是   | 蓝牙设备特征对应的二进制值及其它参数。                 |
| writeType      | GattWriteType                           | 是   | 蓝牙设备特征的写入类型。                               |
| callback       | AsyncCallback&lt;void&gt;               | 是   | 回调函数。当写入成功，err为undefined，否则为错误对象。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901001  | Write forbidden.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let descriptors: Array<ble.BLEDescriptor> = [];
let bufferDesc = new ArrayBuffer(8);
let descV = new Uint8Array(bufferDesc);
descV[0] = 11;
let descriptor: ble.BLEDescriptor = {
  serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX', descriptorValue: bufferDesc
};
descriptors[0] = descriptor;

let bufferCCC = new ArrayBuffer(8);
let cccV = new Uint8Array(bufferCCC);
let characteristic: ble.BLECharacteristic = {
  serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  characteristicValue: bufferCCC, descriptors:descriptors
};

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
  let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
  device.writeCharacteristicValue(characteristic, ble.GattWriteType.WRITE, (err, data) => {
    if (err) {
      console.error('writeCharacteristicValue errCode: ' +
        (err as BusinessError).code + ', errMessage: ' 	+ (err as BusinessError).message);
    }
    console.info('writeCharacteristicValue successfully:' + JSON.stringify(data));
  });
} catch (err) {
  console.error('ETS writeCharacteristicValue errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### writeCharacteristicValue

writeCharacteristicValue(characteristic: BLECharacteristic, writeType: GattWriteType): Promise&lt;void&gt;

client端向低功耗蓝牙设备写入特定的特征值。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名         | 类型                                    | 必填 | 说明                                   |
| -------------- | --------------------------------------- | ---- | -------------------------------------- |
| characteristic | [BLECharacteristic](#blecharacteristic) | 是   | 蓝牙设备特征对应的二进制值及其它参数。 |
| writeType      | GattWriteType                           | 是   | 蓝牙设备特征的写入类型。               |

**返回值：**

| 类型                | 说明                                    |
| ------------------- | --------------------------------------- |
| Promise&lt;void&gt; | client读取描述符，通过promise形式获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901001  | Write forbidden.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let descriptors: Array<ble.BLEDescriptor> = [];
let bufferDesc = new ArrayBuffer(8);
let descV = new Uint8Array(bufferDesc);
descV[0] = 11;
let descriptor: ble.BLEDescriptor = {
  serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX', descriptorValue: bufferDesc
};
descriptors[0] = descriptor;

let bufferCCC = new ArrayBuffer(8);
let cccV = new Uint8Array(bufferCCC);
let characteristic: ble.BLECharacteristic = {
  serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
  characteristicValue: bufferCCC, descriptors:descriptors
};

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
  let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
  device.writeCharacteristicValue(characteristic, ble.GattWriteType.WRITE).then((data) => {
    console.log("writeCharacteristicValue data is " + JSON.stringify(data));
  }).catch((error: BusinessError) => {
    console.log("writeCharacteristicValue failed, "+ JSON.stringify(error));
  })
} catch (err) {
  console.error('writeCharacteristicValue errCode: ' + 
    (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### writeDescriptorValue

writeDescriptorValue(descriptor: BLEDescriptor, callback: AsyncCallback&lt;void&gt;): void

client端向低功耗蓝牙设备特定的描述符写入二进制数据。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名     | 类型                            | 必填 | 说明                                                   |
| ---------- | ------------------------------- | ---- | ------------------------------------------------------ |
| descriptor | [BLEDescriptor](#bledescriptor) | 是   | 蓝牙设备描述符的二进制值及其它参数。                   |
| callback   | AsyncCallback&lt;void&gt;       | 是   | 回调函数。当写入成功，err为undefined，否则为错误对象。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901001  | Write forbidden.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let bufferDesc = new ArrayBuffer(8);
let descV = new Uint8Array(bufferDesc);
descV[0] = 11;
let descriptor: ble.BLEDescriptor = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorValue: bufferDesc
};

let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
device.writeDescriptorValue(descriptor, (err, data) => {
    if (err) {
        console.error('writeDescriptorValue errCode: ' +
        	(err as BusinessError).code + ', errMessage: ' 	+ (err as BusinessError).message);
    }
    console.info('writeDescriptorValue successfully:' + JSON.stringify(data));
});
```


### writeDescriptorValue

writeDescriptorValue(descriptor: BLEDescriptor): Promise&lt;void&gt;

client端向低功耗蓝牙设备特定的描述符写入二进制数据。使用Promise异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名     | 类型                            | 必填 | 说明                                 |
| ---------- | ------------------------------- | ---- | ------------------------------------ |
| descriptor | [BLEDescriptor](#bledescriptor) | 是   | 蓝牙设备描述符的二进制值及其它参数。 |

**返回值：**

| 类型                | 说明                                    |
| ------------------- | --------------------------------------- |
| Promise&lt;void&gt; | client读取描述符，通过promise形式获取。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2901001  | Write forbidden.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

let bufferDesc = new ArrayBuffer(8);
let descV = new Uint8Array(bufferDesc);
descV[0] = 11;
let descriptor: ble.BLEDescriptor = {
    serviceUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    characteristicUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorUuid: 'XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXXXX',
    descriptorValue: bufferDesc
};

try {
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.writeDescriptorValue(descriptor).then(() => {
        console.info('writeDescriptorValue promise success');
    });
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```

### setBLEMtuSize

setBLEMtuSize(mtu: number): void

设置蓝牙低功耗（BLE）设备的MTU（Maximum Transmission Unit）大小，在调用[connect](#connect)接口成功连接远端设备后，方可使用此功能。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| mtu    | number | 是   | 设置范围为22-512字节。（Android设置范围为23-517，当Android 14及以上版本时，设置的mtu需要比上一次设置的mtu值大，才能请求成功） |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 2900001  | Service stopped.                                             |
| 2900099  | Operation failed.                                            |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.setBLEMtuSize(128);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### on('BLEConnectionStateChange')

on(type: 'BLEConnectionStateChange', callback: Callback&lt;BLEConnectionChangeState&gt;): void

client端订阅蓝牙低功耗设备的连接状态变化事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 填写"BLEConnectionStateChange"字符串，表示连接状态变化事件。 |
| callback | Callback&lt;[BLEConnectionChangeState](#bleconnectionchangestate)&gt; | 是   | 表示连接状态，已连接或断开。                                 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

function ConnectStateChanged(state: ble.BLEConnectionChangeState) {
    console.info('bluetooth connect state changed');
    let connectState: ble.ProfileConnectionState = state.state;
}

try {
  
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.on('BLEConnectionStateChange', ConnectStateChanged);
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### off('BLEConnectionStateChange')

off(type: 'BLEConnectionStateChange', callback?: Callback&lt;BLEConnectionChangeState&gt;): void

取消订阅蓝牙低功耗设备的连接状态变化事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 填写"BLEConnectionStateChange"字符串，表示连接状态变化事件。 |
| callback | Callback&lt;[BLEConnectionChangeState](#bleconnectionchangestate)&gt; | 否   | 表示取消订阅蓝牙低功耗设备的连接状态变化事件。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
  //android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX"
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.off('BLEConnectionStateChange');
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### on('BLEMtuChange')

on(type: 'BLEMtuChange', callback: Callback&lt;number&gt;): void

client端订阅MTU状态变化事件。使用Callback异步回调。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| -------- | ---------------------- | ---- | ------------------------------------------------------------ |
| type     | string                 | 是   | 必须填写"BLEMtuChange"字符串，表示MTU状态变化事件。填写不正确将导致回调无法注册。 |
| callback | Callback&lt;number&gt; | 是   | 返回MTU字节数的值，通过注册回调函数获取。                    |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
    let gattClient: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    gattClient.on('BLEMtuChange', (mtu: number) => {
      console.info('BLEMtuChange, mtu: ' + mtu);
    });
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```


### off('BLEMtuChange')

off(type: 'BLEMtuChange', callback?: Callback&lt;number&gt;): void

client端取消订阅MTU状态变化事件。

**需要权限**：ohos.permission.ACCESS_BLUETOOTH

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台**： Android 

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| -------- | ---------------------- | ---- | ------------------------------------------------------------ |
| type     | string                 | 是   | 必须填写"BLEMtuChange"字符串，表示MTU状态变化事件。填写不正确将导致回调无法注册。 |
| callback | Callback&lt;number&gt; | 否   | 返回MTU字节数的值，通过注册回调函数获取。不填该参数则取消订阅该type对应的所有回调。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed. |
| 801      | Capability not supported.                                    |

**示例：**

```js
import ble from '@ohos.bluetooth.ble'
import { BusinessError } from '@ohos.base'

try {
    let device: ble.GattClientDevice = ble.createGattClientDevice('XX:XX:XX:XX:XX:XX');
    device.off('BLEMtuChange');
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as BusinessError).message);
}
```

## GattService

描述service的接口参数定义。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称            | 类型                                                 | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| --------------- | ---------------------------------------------------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| serviceUuid     | string                                               | 是   | 是   | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |
| isPrimary       | boolean                                              | 是   | 是   | 如果是主服务设置为true，否则设置为false。                    | 支持        | 支持    |
| characteristics | Array&lt;[BLECharacteristic](#blecharacteristic)&gt; | 是   | 是   | 当前服务包含的特征列表。                                     | 支持        | 支持    |

## NotifyCharacteristic

描述server端特征值变化时发送的特征通知参数定义。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                | 类型        | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| :------------------ | :---------- | :--- | :--- | :----------------------------------------------------------- | ----------- | ------- |
| serviceUuid         | string      | 是   | 是   | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 不支持      | 支持    |
| characteristicUuid  | string      | 是   | 是   | 特定特征（characteristic）的UUID，例如：00002a11-0000-1000-8000-00805f9b34fb。 | 不支持      | 支持    |
| characteristicValue | ArrayBuffer | 是   | 是   | 特征对应的二进制值。                                         | 不支持      | 支持    |

## ServerResponse

描述server端回复client端读/写请求的响应参数结构。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称     | 类型        | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| :------- | :---------- | :--- | :--- | :----------------------------------------------------------- | ----------- | ------- |
| deviceId | string      | 是   | 否   | 表示远端设备地址，例如：“XX:XX:XX:XX:XX:XX”。                | 不支持      | 支持    |
| transId  | number      | 是   | 否   | 表示请求的传输ID，与订阅的读/写请求事件携带的ID保持一致。    | 不支持      | 支持    |
| status   | number      | 是   | 否   | 表示响应的状态，设置为0即可，表示正常。                      | 不支持      | 支持    |
| value    | ArrayBuffer | 是   | 否   | 表示回复响应的二进制数据。                                   | 不支持      | 支持    |

## BLECharacteristic

描述characteristic的接口参数定义 。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                  | 类型                                     | 可读   | 可写   | 说明                                 | Android平台                        | iOS平台                            |
| ------------------- | ---------------------------------------- | ---- | ---- | ---------------------------------------- | ---------------------------------------- | ---------------------------------------- |
| serviceUuid         | string                                   | 是    | 是    | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持 | 支持 |
| characteristicUuid  | string                  | 是    | 是    | 特定特征（characteristic）的UUID，例如：00002a11-0000-1000-8000-00805f9b34fb。 | 支持 | 支持 |
| characteristicValue | ArrayBuffer                              | 是    | 是    | 特征对应的二进制值。                      | 支持                    | 支持 |
| descriptors         | Array&lt;[BLEDescriptor](#bledescriptor)&gt; | 是    | 是    | 特定特征的描述符列表。                | 支持              | 支持 |
| properties  | [GattProperties](#gattproperties) |   是   | 是     | 特定特征的属性描述。     | 支持   | 支持 |


## BLEDescriptor

描述descriptor的接口参数定义。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称               | 类型        | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------------------ | ----------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| serviceUuid        | string      | 是   | 是   | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |
| characteristicUuid | string      | 是   | 是   | 特定特征（characteristic）的UUID，例如：00002a11-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |
| descriptorUuid     | string      | 是   | 是   | 描述符（descriptor）的UUID，例如：00002902-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |
| descriptorValue    | ArrayBuffer | 是   | 是   | 描述符对应的二进制值。                                       | 支持        | 支持    |


## CharacteristicReadRequest

描述server端订阅后收到的特征值读请求事件参数结构。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称               | 类型   | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------------------ | ------ | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| deviceId           | string | 是   | 否   | 表示发送特征值读请求的远端设备地址，例如："XX:XX:XX:XX:XX:XX"。android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" | 支持        | 支持    |
| transId            | number | 是   | 否   | 表示读请求的传输ID，server端回复响应时需填写相同的传输ID。   | 支持        | 支持    |
| characteristicUuid | string | 是   | 否   | 特定特征（characteristic）的UUID，例如：00002a11-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |
| serviceUuid        | string | 是   | 否   | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |


## CharacteristicWriteRequest

描述server端订阅后收到的特征值写请求事件参数结构。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                 | 类型   | 可读   | 可写   | 说明                                       | Android平台                              | iOS平台                                  |
| ------------------ | ------ | ---- | ---- | ---------------------------------------- | ------------------ | ------------------ |
| deviceId           | string | 是    | 否    | 表示发送特征值写请求的远端设备地址，例如："XX:XX:XX:XX:XX:XX"。android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" | 支持 | 支持 |
| transId            | number | 是    | 否    | 表示写请求的传输ID，server端回复响应时需填写相同的传输ID。       | 支持     | 支持 |
| value | ArrayBuffer | 是 | 否 | 表示写入的描述符二进制数据。 | 支持 | 支持 |
| characteristicUuid | string | 是    | 否    | 特定特征（characteristic）的UUID，例如：00002a11-0000-1000-8000-00805f9b34fb。 | 支持 | 支持 |
| serviceUuid        | string | 是    | 否    | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持 | 支持 |


## DescriptorReadRequest

描述server端订阅后收到的描述符读请求事件参数结构。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称               | 类型   | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------------------ | ------ | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| deviceId           | string | 是   | 否   | 表示发送描述符读请求的远端设备地址，例如："XX:XX:XX:XX:XX:XX"。android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" | 支持        | 支持    |
| transId            | number | 是   | 否   | 表示读请求的传输ID，server端回复响应时需填写相同的传输ID。   | 支持        | 支持    |
| descriptorUuid     | string | 是   | 否   | 表示描述符（descriptor）的UUID，例如：00002902-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |
| characteristicUuid | string | 是   | 否   | 特定特征（characteristic）的UUID，例如：00002a11-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |
| serviceUuid        | string | 是   | 否   | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持        | 支持    |

## DescriptorWriteRequest

描述server端订阅后收到的描述符写请求事件参数结构。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称               | 类型        | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------------------ | ----------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| deviceId           | string      | 是   | 否   | 表示发送描述符写请求的远端设备地址，例如："XX:XX:XX:XX:XX:XX"。 | 支持        | 不支持  |
| transId            | number      | 是   | 否   | 表示写请求的传输ID，server端回复响应时需填写相同的传输ID。   | 支持        | 不支持  |
| value              | ArrayBuffer | 是   | 否   | 表示写入的描述符二进制数据。                                 | 支持        | 不支持  |
| descriptorUuid     | string      | 是   | 否   | 表示描述符（descriptor）的UUID，例如：00002902-0000-1000-8000-00805f9b34fb。 | 支持        | 不支持  |
| characteristicUuid | string      | 是   | 否   | 特定特征（characteristic）的UUID，例如：00002a11-0000-1000-8000-00805f9b34fb。 | 支持        | 不支持  |
| serviceUuid        | string      | 是   | 否   | 特定服务（service）的UUID，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持        | 不支持  |


## BLEConnectionChangeState

描述Gatt profile连接状态。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称     | 类型                                                         | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| -------- | ------------------------------------------------------------ | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| deviceId | string                                                       | 是   | 否   | 表示远端设备地址，例如："XX:XX:XX:XX:XX:XX"。<br />android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" | 支持        | 支持    |
| state    | [ProfileConnectionState](js-apis-bluetooth-constant.md#profileconnectionstate) | 是   | 是   | 表示BLE连接状态的枚举。                                      | 支持        | 支持    |


## ScanResult

扫描结果上报数据。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称       | 类型        | 可读   | 可写   | 说明                                 | Android平台                        | iOS平台                            |
| -------- | ----------- | ---- | ---- | ---------------------------------- | ---------------------------------- | ---------------------------------- |
| deviceId | string      | 是    | 否    | 表示扫描到的设备地址，例如："XX:XX:XX:XX:XX:XX"。基于信息安全考虑，此处获取的设备地址为随机MAC地址。配对成功后，该地址不会变更；已配对设备取消配对后重新扫描或蓝牙服务下电时，该随机地址会变更。android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" | 支持 | 支持 |
| rssi     | number      | 是    | 否    | 表示扫描到的设备的rssi值。                    | 支持                  | 支持 |
| data     | ArrayBuffer | 是    | 否    | 表示扫描到的设备发送的广播包。                    | 支持                  | 支持 |
| deviceName | string | 是    | 否    | 表示扫描到的设备名称。                    | 支持                  | 支持 |
| connectable  | boolean | 是    | 否    | 表示扫描到的设备是否可连接。true表示可连接，false表示不可连接。                    | 支持                  | 支持 |


## AdvertiseSetting

描述蓝牙低功耗设备发送广播的参数。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**说明**：iOS设置此属性后在广播数据包中生成key为kCBAdvDataIsConnectable值，当前广播依然可以连接。

| 名称        | 类型    | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ----------- | ------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| connectable | boolean | 是   | 是   | 表示是否是可连接广播，默认值设置为true，表示可连接，false表示不可连接。 | 支持        | 支持    |

## AdvertiseData

描述BLE广播数据包的内容，广播包数据长度为31个字节。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称              | 类型                                     | 可读   | 可写   | 说明                          | Android平台                 | iOS平台                     |
| --------------- | ---------------------------------------- | ---- | ---- | --------------------------- | --------------------------- | --------------------------- |
| serviceUuids    | Array&lt;string&gt;                      | 是    | 是    | 表示要广播的服务&nbsp;UUID&nbsp;列表。 | 支持 | 支持 |
| manufactureData | Array&lt;[ManufactureData](#manufacturedata)&gt; | 是    | 是    | 表示要广播的广播的制造商信息列表。           | 支持         | 不支持 |
| serviceData     | Array&lt;[ServiceData](#servicedata)&gt; | 是    | 是    | 表示要广播的服务数据列表。               | 支持             | 支持 |
| includeDeviceName | boolean     | 是    | 是    | 表示是否携带设备名，可选参数。true表示携带，false或未设置此参数表示不携带。注意带上设备名时广播包长度不能超出31个字节。        | 支持      | 支持 |

## AdvertisingParams<sup>11+</sup>

描述启动广播设置的参数，适用于初次启动或重新配置场景。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                | 类型                             | 可读  | 可写  | 说明                      | Android平台             | iOS平台                 |
| ------------------- | ------------------------------- | ----- | ----- | ------------------------ | ------------------- | ------------------- |
| advertisingSettings<sup>11+</sup> | AdvertiseSetting                | 是    | 是    | 表示发送广播的相关参数。    | 支持  | 支持 |
| advertisingData<sup>11+</sup>    | [AdvertiseData](#advertisedata) | 是    | 是    | 表示广播的数据包内容。      | 支持    | 支持 |
| advertisingResponse<sup>11+</sup> | [AdvertiseData](#advertisedata) | 是    | 是    | 表示回复扫描请求的响应内容。 | 支持 | 不支持 |

## AdvertisingStateChangeInfo<sup>11+</sup>

描述广播启动、停止等状态信息。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                | 类型                                     | 可读  | 可写  | 说明                      | Android平台             | iOS平台                 |
| ------------------- | --------------------------------------- | ----- | ----- | ------------------------ | ------------------------ | ------------------------ |
| advertisingId<sup>11+</sup>       | number                                  | 是    | 是    | 表示广播ID标识。           | 支持         | 支持 |
| state<sup>11+</sup>               | [AdvertisingState](#advertisingstate11)   | 是    | 是    | 表示广播状态。             | 支持           | 支持 |

## ManufactureData

描述BLE广播数据包的内容。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称               | 类型                | 可读   | 可写   | 说明                 | Android平台        | iOS平台            |
| ---------------- | ------------------- | ---- | ---- | ------------------ | ------------------ | ------------------ |
| manufactureId    | number  | 是    | 是    | 表示制造商的ID，由蓝牙SIG分配。 | 支持 | 不支持 |
| manufactureValue | ArrayBuffer         | 是    | 是    | 表示制造商发送的制造商数据。     | 支持   | 不支持 |


## ServiceData

描述广播包中服务数据内容。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称         | 类型        | 可读 | 可写 | 说明             | Android平台 | iOS平台 |
| ------------ | ----------- | ---- | ---- | ---------------- | ----------- | ------- |
| serviceUuid  | string      | 是   | 是   | 表示服务的UUID。 | 支持        | 支持    |
| serviceValue | ArrayBuffer | 是   | 是   | 表示服务数据。   | 支持        | 支持    |


## ScanFilter

扫描过滤参数。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                                     | 类型    | 必填  | 说明                                                         | Android平台                                                | iOS平台                                                    |
| ------------------------------------------ | -------- | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| deviceId                                 | string      | 否    | 表示过滤的BLE设备地址，例如："XX:XX:XX:XX:XX:XX"。<br />android deviceId:"XX:XX:XX:XX:XX:XX", ios deviceId:"XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX" | 支持         | 支持 |
| name                                     | string      | 否    | 表示过滤的BLE设备名。                                        | 支持                                      | 支持 |
| serviceUuid                              | string      | 否    | 表示过滤包含该UUID服务的设备，例如：00001888-0000-1000-8000-00805f9b34fb。 | 支持 | 支持 |
| serviceUuidMask             | string      | 否     | 表示过滤包含该UUID服务掩码的设备，例如：FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF。 | 支持 | 不支持 |
| serviceSolicitationUuid     | string      | 否     | 表示过滤包含该UUID服务请求的设备，例如：00001888-0000-1000-8000-00805F9B34FB。 | 支持 | 支持 |
| serviceSolicitationUuidMask | string      | 否     | 表示过滤包含该UUID服务请求掩码的设备，例如：FFFFFFFF-FFFF-FFFF-FFFF-FFFFFFFFFFFF。<br>设置该参数必须同时设置serviceSolicitationUuid。 | 支持 | 不支持 |
| serviceData                 | ArrayBuffer | 否     | 表示过滤包含该服务相关数据的设备，例如：[0x90,0x00,0xF1,0xF2]。<br/>设置该参数必须同时设置serviceUuid。 | 支持 | 不支持 |
| serviceDataMask             | ArrayBuffer | 否     | 表示过滤包含该服务相关数据掩码的设备，例如：[0xFF,0xFF,0xFF,0xFF]。<br/>设置该参数必须同时设置serviceUuid。 | 支持 | 不支持 |
| manufactureId               | number      | 否     | 表示过滤包含该制造商ID的设备，例如：0x0006。                 | 支持               | 不支持 |
| manufactureData             | ArrayBuffer | 否     | 表示过滤包含该制造商相关数据的设备，例如：[0x1F,0x2F,0x3F]。<br/>设置该参数必须同时设置manufactureId。 | 支持 | 不支持 |
| manufactureDataMask         | ArrayBuffer | 否     | 表示过滤包含该制造商相关数据掩码的设备，例如：[0xFF,0xFF,0xFF]。<br/>设置该参数必须同时设置manufactureId。 | 支持 | 不支持 |


## ScanOptions

扫描的配置参数。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称        | 类型                    | 可读   | 可写   | 说明                                     | Android平台                            | iOS平台                                |
| --------- | ----------------------- | ---- | ---- | -------------------------------------- | --------- | --------- |
| interval  | number                  | 是    | 是    | 表示扫描结果上报延迟时间，默认值为0。                    | 支持                  | 不支持 |
| dutyMode  | [ScanDuty](#scanduty)   | 是    | 是    | 表示扫描模式，默认值为SCAN_MODE_LOW_POWER。        | 支持      | 不支持 |
| phyType<sup>12+</sup> | [PhyType](#phytype12) | 是    | 是    | 表示扫描中使用的PHY类型。 | 支持 | 不支持 |


## GattProperties<a name="GattProperties"></a>

描述gatt characteristic的属性。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称       | 类型  | 必填   | 说明          | Android平台 | iOS平台     |
| -------- | ------ |---- | ----------- | -------- | -------- |
| write    | boolean | 否  | 表示该特征支持写操作，需要对端设备的回复。 | 支持 | 支持 |
| writeNoResponse | boolean | 否    | 表示该特征支持写操作，无需对端设备回复。 | 支持 | 支持 |
| read | boolean   |  否    | 表示该特征支持读操作。 | 支持 | 支持 |
| notify | boolean   | 否    | 表示该特征可通知对端设备。 | 支持 | 支持 |


## GattWriteType<a name="GattWriteType"></a>

枚举，定义表示gatt写入类型。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                                   | 值    | 说明   | Android平台     | iOS平台 |
| ------------------------------------| ------ | --------------- | --------------- | --------------- |
| WRITE               | 1 | 表示写入特征值，需要对端设备的回复。 | 支持 | 支持 |
| WRITE_NO_RESPONSE   | 2 | 表示写入特征值，不需要对端设备的回复。 | 支持 | 支持 |


## ScanDuty

枚举，定义扫描模式。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                    | 值  | 说明           | Android平台  | iOS平台      |
| --------------------- | ---- | ------------ | ------------ | ------------ |
| SCAN_MODE_LOW_POWER   | 0    | 表示低功耗模式，默认值。 | 支持 | 不支持 |
| SCAN_MODE_BALANCED    | 1    | 表示均衡模式。      | 支持    | 不支持 |
| SCAN_MODE_LOW_LATENCY | 2    | 表示低延迟模式。     | 支持   | 不支持 |

## AdvertisingState<sup>11+</sup>

枚举，定义广播状态。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称      | 值    | 说明                           | Android平台                  | iOS平台                      |
| --------  | ---- | ------------------------------ | ------------------------------ | ------------------------------ |
| STARTED<sup>11+</sup>   | 1    | 表示启动广播后的状态。       | 支持     | 支持 |
| STOPPED<sup>11+</sup>    | 4    | 表示广播完全停止后的状态。       | 支持     | 支持 |

## PhyType<sup>12+</sup>

枚举，定义扫描中使用的PHY类型。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称      | 值    | 说明                           | Android平台                  | iOS平台                      |
| --------  | ---- | ------------------------------ | ------------------------------ | ------------------------------ |
| PHY_LE_1M<sup>12+</sup>   | 1    | 表示扫描中使用1M PHY。       | 支持     | 不支持 |
| PHY_LE_ALL_SUPPORTED<sup>12+</sup>   | 255    | 表示扫描中使用蓝牙协议支持的PHY模式。    | 支持  | 不支持 |