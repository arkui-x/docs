# @ohos.bluetooth.constant (蓝牙constant模块)

constant模块提供了蓝牙中常量的定义。

> **说明：**
>
> 本模块首批接口从API version 13开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。



## 导入模块

```js
import constant from '@ohos.bluetooth.constant';
```

## ProfileId

枚举，定义蓝牙profile类型。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                               | 值    | 说明              | Android平台     | iOS平台         |
| -------------------------------- | ------ | --------------- | --------------- | --------------- |
| PROFILE_A2DP_SOURCE              | 1 | 表示A2DP profile。 | 支持 | 支持 |
| PROFILE_HANDSFREE_AUDIO_GATEWAY | 4 | 表示HFP profile。  | 支持 | 支持 |


## ProfileConnectionState

枚举，定义蓝牙设备的profile连接状态。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                  | 值  | 说明             | Android平台    | **iOS平台**    |
| ------------------- | ---- | -------------- | -------------- | -------------- |
| STATE_DISCONNECTED  | 0    | 表示profile已断连。  | 支持 | 支持 |
| STATE_CONNECTING    | 1    | 表示profile正在连接。 | 支持 | 支持 |
| STATE_CONNECTED     | 2    | 表示profile已连接。  | 支持 | 支持 |
| STATE_DISCONNECTING | 3    | 表示profile正在断连。 | 支持 | 支持 |


## MajorClass

枚举，定义蓝牙设备主要类别。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                  | 值    | 说明         | Android平台 | **iOS平台** |
| ------------------- | ------ | ---------- | ---------- | ---------- |
| MAJOR_AUDIO_VIDEO   | 0x0400 | 表示音频和视频设备。 | 支持 | 支持 |
| MAJOR_HEALTH        | 0x0900 | 表示健康设备。    | 支持  | 支持  |


## MajorMinorClass

枚举，定义主要次要蓝牙设备类别。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                                       | 值    | 说明              | Android平台     | **iOS平台**     |
| ---------------------------------------- | ------ | --------------- | --------------- | --------------- |
| COMPUTER_UNCATEGORIZED                   | 0x0100 | 表示未分类计算机设备。     | 支持   | 支持   |
| COMPUTER_DESKTOP                         | 0x0104 | 表示台式计算机设备。      | 支持    | 支持    |
| COMPUTER_SERVER                          | 0x0108 | 表示服务器设备。        | 支持      | 支持      |
| COMPUTER_LAPTOP                          | 0x010C | 表示便携式计算机设备。     | 支持   | 支持   |
| COMPUTER_HANDHELD_PC_PDA                 | 0x0110 | 表示手持式计算机设备。     | 支持   | 支持   |
| COMPUTER_PALM_SIZE_PC_PDA                | 0x0114 | 表示掌上电脑设备。       | 支持     | 支持     |
| COMPUTER_WEARABLE                        | 0x0118 | 表示可穿戴计算机设备。     | 支持   | 支持   |
| PHONE_UNCATEGORIZED                      | 0x0200 | 表示未分类手机设备。      | 支持    | 支持    |
| PHONE_CELLULAR                           | 0x0204 | 表示便携式手机设备。      | 支持    | 支持    |
| PHONE_CORDLESS                           | 0x0208 | 表示无线电话设备。       | 支持     | 支持     |
| PHONE_SMART                              | 0x020C | 表示智能手机设备。       | 支持     | 支持     |
| PHONE_MODEM_OR_GATEWAY                   | 0x0210 | 表示调制解调器或网关手机设备。 | 支持 | 支持 |
| PHONE_ISDN                               | 0x0214 | 表示ISDN手机设备。     | 支持   | 支持   |
| AUDIO_VIDEO_WEARABLE_HEADSET             | 0x0404 | 表示可穿戴式音频视频设备。   | 支持 | 支持 |
| AUDIO_VIDEO_HANDSFREE                    | 0x0408 | 表示免提音频视频设备。     | 支持   | 支持   |
| AUDIO_VIDEO_MICROPHONE                   | 0x0410 | 表示麦克风音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_LOUDSPEAKER                  | 0x0414 | 表示扬声器音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_HEADPHONES                   | 0x0418 | 表示头戴式音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_PORTABLE_AUDIO               | 0x041C | 表示便携式音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_CAR_AUDIO                    | 0x0420 | 表示汽车音频视频设备。     | 支持   | 支持   |
| AUDIO_VIDEO_SET_TOP_BOX                  | 0x0424 | 表示机顶盒音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_HIFI_AUDIO                   | 0x0428 | 表示高保真音响设备。      | 支持    | 支持    |
| AUDIO_VIDEO_VCR                          | 0x042C | 表示录像机音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_VIDEO_CAMERA                 | 0x0430 | 表示照相机音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_CAMCORDER                    | 0x0434 | 表示摄像机音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_VIDEO_MONITOR                | 0x0438 | 表示监视器音频视频设备。    | 支持  | 支持  |
| AUDIO_VIDEO_VIDEO_DISPLAY_AND_LOUDSPEAKER | 0x043C | 表示视频显示器和扬声器设备。  | 支持 | 支持 |
| AUDIO_VIDEO_VIDEO_CONFERENCING           | 0x0440 | 表示音频视频会议设备。     | 支持   | 支持   |
| AUDIO_VIDEO_VIDEO_GAMING_TOY             | 0x0448 | 表示游戏玩具音频视频设备。   | 支持 | 支持 |
| PERIPHERAL_NON_KEYBOARD_NON_POINTING     | 0x0500 | 表示非键盘非指向外围设备。   | 支持 | 支持 |
| PERIPHERAL_KEYBOARD                      | 0x0540 | 表示外设键盘设备。       | 支持     | 支持     |
| PERIPHERAL_POINTING_DEVICE               | 0x0580 | 表示定点装置外围设备。     | 支持   | 支持   |
| PERIPHERAL_KEYBOARD_POINTING             | 0x05C0 | 表示键盘指向外围设备。     | 支持   | 支持   |
| WEARABLE_UNCATEGORIZED                   | 0x0700 | 表示未分类的可穿戴设备。    | 支持  | 支持  |
| WEARABLE_WRIST_WATCH                     | 0x0704 | 表示可穿戴腕表设备。      | 支持    | 支持    |
| WEARABLE_PAGER                           | 0x0708 | 表示可穿戴寻呼机设备。     | 支持   | 支持   |
| WEARABLE_JACKET                          | 0x070C | 表示夹克可穿戴设备。      | 支持    | 支持    |
| WEARABLE_HELMET                          | 0x0710 | 表示可穿戴头盔设备。      | 支持    | 支持    |
| WEARABLE_GLASSES                         | 0x0714 | 表示可穿戴眼镜设备。      | 支持    | 支持    |
| TOY_UNCATEGORIZED                        | 0x0800 | 表示未分类的玩具设备。     | 支持   | 支持   |
| TOY_ROBOT                                | 0x0804 | 表示玩具机器人设备。      | 支持    | 支持    |
| TOY_VEHICLE                              | 0x0808 | 表示玩具车设备。        | 支持      | 支持      |
| TOY_DOLL_ACTION_FIGURE                   | 0x080C | 表示人形娃娃玩具设备。     | 支持   | 支持   |
| TOY_CONTROLLER                           | 0x0810 | 表示玩具控制器设备。      | 支持    | 支持    |
| TOY_GAME                                 | 0x0814 | 表示玩具游戏设备。       | 支持     | 支持     |
| HEALTH_BLOOD_PRESSURE                    | 0x0904 | 表示血压健康设备。       | 支持     | 支持     |
| HEALTH_THERMOMETER                       | 0x0908 | 表示温度计健康设备。      | 支持    | 支持    |
| HEALTH_WEIGHING                          | 0x090C | 表示体重健康设备。       | 支持     | 支持     |
| HEALTH_GLUCOSE                           | 0x0910 | 表示葡萄糖健康设备。      | 支持    | 支持    |
| HEALTH_PULSE_OXIMETER                    | 0x0914 | 表示脉搏血氧仪健康设备。    | 支持  | 支持  |
| HEALTH_PULSE_RATE                        | 0x0918 | 表示脉搏率健康设备。      | 支持    | 支持    |
| HEALTH_DATA_DISPLAY                      | 0x091C | 表示数据显示健康设备。     | 支持   | 支持   |

## ProfileUuids

支持枚举，表示Profile的UUID。

**系统能力**：SystemCapability.Communication.Bluetooth.Core。

| 名称                                   | 值    | 说明              | Android平台     | **iOS平台**     |
| ------------------------------------| ------ | --------------- | --------------- | --------------- |
| PROFILE_UUID_HFP_AG      | '0000111F-0000-1000-8000-00805F9B34FB' | 代表HFPAG Profile的UUID。 | 支持 | 支持 |
| PROFILE_UUID_HFP_HF      | '0000111E-0000-1000-8000-00805F9B34FB' | 代表HFPHF Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_HSP_AG      | '00001112-0000-1000-8000-00805F9B34FB' | 代表HSPAG Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_HSP_HS      | '00001108-0000-1000-8000-00805F9B34FB' | 代表HSPHS Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_A2DP_SRC    | '0000110A-0000-1000-8000-00805F9B34FB' | 代表A2DPSRC Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_A2DP_SINK   | '0000110B-0000-1000-8000-00805F9B34FB' | 代表A2DPSINK Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_AVRCP_CT    | '0000110E-0000-1000-8000-00805F9B34FB' | 代表AVRCPCT Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_AVRCP_TG    | '0000110C-0000-1000-8000-00805F9B34FB' | 代表AVRCPTG Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_HID         | '00001124-0000-1000-8000-00805F9B34FB' | 代表HID Profile的UUID。  | 支持 | 支持 |
| PROFILE_UUID_HOGP        | '00001812-0000-1000-8000-00805F9B34FB' | 代表HOGP Profile的UUID。  | 支持 | 支持 |

