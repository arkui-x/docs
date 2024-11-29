# @ohos.bluetooth.a2dp (蓝牙a2dp模块)

a2dp模块提供了访问蓝牙音频接口的方法。

> **说明：**
>
> 本模块首批接口从API version 13开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 依赖权限

#### iOS平台

**说明：**Xcode项目里存在蓝牙模块任意一个库（libbluetooth_ble、libbluetooth_a2dp、libbluetooth_access、libbluetooth_connection、libbluetooth_common）时，需要配置蓝牙权限。**如果没有配置蓝牙权限会导致运行异常**。

**配置方法：**在Xcode中右击项目中的info.plist，选择Open As > Source Code，在plist标签中加入NSBluetoothAlwaysUsageDescription。

示例如下：

```
<plist version="1.0">
<dict>
    <key>NSBluetoothAlwaysUsageDescription</key>
    <string>获取蓝牙权限描述文案</string>
</dict>
</plist>

```


## 导入模块

```js
import a2dp from '@ohos.bluetooth.a2dp';
```

## a2dp.createA2dpSrcProfile

createA2dpSrcProfile(): A2dpSourceProfile

创建a2dp profile实例。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

**支持平台：** Android、iOS

**返回值：**

| 类型                            | 说明         |
| ----------------------------- | ---------- |
| A2dpSourceProfile | 返回该profile的实例。 |

**错误码**：

以下错误码的详细介绍请参见[蓝牙服务错误码](../errorcodes/errorcode-bluetooth.md)。

| 错误码ID | 错误信息 |
| -------- | ---------------------------- |
|401 | Invalid parameter. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3. Parameter verification failed.                         |
|801 | Capability not supported.                |


**示例：**

```js
import a2dp from '@ohos.bluetooth.a2dp';
import { BusinessError } from '@ohos.base';

try {
    let a2dpProfile = a2dp.createA2dpSrcProfile();
    console.info('a2dp success');
} catch (err) {
    console.error('errCode: ' + (err as BusinessError).code + ', errMessage: ' + (err as 			BusinessError).message);
}
```


## A2dpSourceProfile

使用A2dpSourceProfile方法之前需要创建该类的实例进行操作，通过createA2dpSrcProfile()方法构造此实例。

**支持平台：** Android、iOS