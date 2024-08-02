# ohos.multimedia.audio (音频管理)

音频管理提供管理音频的一些基础能力，包括对音频音量、音频设备的管理，以及对音频数据的采集和渲染等。

该模块提供以下音频相关的常用功能：

- [AudioManager](#audiomanager)：音频管理。
- [AudioRenderer](#audiorenderer)：音频渲染，用于播放PCM（Pulse Code Modulation）音频数据。
- [AudioCapturer](#audiocapturer)：音频采集，用于录制PCM音频数据。

> **说明：**
> 本模块首批接口从API version 12开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import audio from '@ohos.multimedia.audio';
```

## 常量

| 名称                    | 类型   | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ----------------------- | ------ | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| DEFAULT_VOLUME_GROUP_ID | number | 是   | 否   | 默认音量组id。<br> **系统能力：** SystemCapability.Multimedia.Audio.Volume | 支持        | 支持    |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

const defaultVolumeGroupId = audio.DEFAULT_VOLUME_GROUP_ID;
```

## audio.getAudioManager

getAudioManager(): AudioManager

获取音频管理器。

**系统能力：** SystemCapability.Multimedia.Audio.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                          | 说明           |
| ----------------------------- | -------------- |
| [AudioManager](#audiomanager) | 音频管理对象。 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let audioManager = audio.getAudioManager();
```

## audio.createAudioRenderer

createAudioRenderer(options: AudioRendererOptions, callback: AsyncCallback\<AudioRenderer>): void

获取音频渲染器。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                           | 必填 | 说明                                                         |
| -------- | ---------------------------------------------- | ---- | ------------------------------------------------------------ |
| options  | [AudioRendererOptions](#audiorendereroptions)  | 是   | 配置渲染器。                                                 |
| callback | AsyncCallback<[AudioRenderer](#audiorenderer)> | 是   | 回调函数。当获取音频渲染器成功，err为undefined，data为获取到的音频渲染器对象；否则为错误对象。 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let audioStreamInfo: audio.AudioStreamInfo = {
  samplingRate: audio.AudioSamplingRate.SAMPLE_RATE_44100,
  channels: audio.AudioChannel.CHANNEL_1,
  sampleFormat: audio.AudioSampleFormat.SAMPLE_FORMAT_S16LE,
  encodingType: audio.AudioEncodingType.ENCODING_TYPE_RAW
}

let audioRendererInfo: audio.AudioRendererInfo = {
  usage: audio.StreamUsage.STREAM_USAGE_VOICE_COMMUNICATION,
  rendererFlags: 0
}

let audioRendererOptions: audio.AudioRendererOptions = {
  streamInfo: audioStreamInfo,
  rendererInfo: audioRendererInfo
}

audio.createAudioRenderer(audioRendererOptions,(err, data) => {
  if (err) {
    console.error(`AudioRenderer Created: Error: ${err}`);
  } else {
    console.info('AudioRenderer Created: Success: SUCCESS');
    let audioRenderer = data;
  }
});
```

## audio.createAudioRenderer

createAudioRenderer(options: AudioRendererOptions): Promise<AudioRenderer\>

获取音频渲染器。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                          | 必填 | 说明         |
| :------ | :-------------------------------------------- | :--- | :----------- |
| options | [AudioRendererOptions](#audiorendereroptions) | 是   | 配置渲染器。 |

**返回值：**

| 类型                                     | 说明                              |
| ---------------------------------------- | --------------------------------- |
| Promise<[AudioRenderer](#audiorenderer)> | Promise对象，返回音频渲染器对象。 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let audioStreamInfo: audio.AudioStreamInfo = {
  samplingRate: audio.AudioSamplingRate.SAMPLE_RATE_44100,
  channels: audio.AudioChannel.CHANNEL_1,
  sampleFormat: audio.AudioSampleFormat.SAMPLE_FORMAT_S16LE,
  encodingType: audio.AudioEncodingType.ENCODING_TYPE_RAW
}

let audioRendererInfo: audio.AudioRendererInfo = {
  usage: audio.StreamUsage.STREAM_USAGE_VOICE_COMMUNICATION,
  rendererFlags: 0
}

let audioRendererOptions: audio.AudioRendererOptions = {
  streamInfo: audioStreamInfo,
  rendererInfo: audioRendererInfo
}

let audioRenderer: audio.AudioRenderer;
audio.createAudioRenderer(audioRendererOptions).then((data) => {
  audioRenderer = data;
  console.info('AudioFrameworkRenderLog: AudioRenderer Created : Success : Stream Type: SUCCESS');
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRenderLog: AudioRenderer Created : ERROR : ${err}`);
});
```

## audio.createAudioCapturer

createAudioCapturer(options: AudioCapturerOptions, callback: AsyncCallback<AudioCapturer\>): void

获取音频采集器。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**需要权限：** ohos.permission.MICROPHONE

仅设置Mic音频源（即[SourceType](#sourcetype)为SOURCE_TYPE_MIC）时需要该权限。

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                           | 必填 | 说明                                                         |
| :------- | :--------------------------------------------- | :--- | :----------------------------------------------------------- |
| options  | [AudioCapturerOptions](#audiocaptureroptions)  | 是   | 配置音频采集器。                                             |
| callback | AsyncCallback<[AudioCapturer](#audiocapturer)> | 是   | 回调函数。当获取音频采集器成功，err为undefined，data为获取到的音频采集器对象；否则为错误对象。 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let audioStreamInfo: audio.AudioStreamInfo = {
  samplingRate: audio.AudioSamplingRate.SAMPLE_RATE_44100,
  channels: audio.AudioChannel.CHANNEL_2,
  sampleFormat: audio.AudioSampleFormat.SAMPLE_FORMAT_S16LE,
  encodingType: audio.AudioEncodingType.ENCODING_TYPE_RAW
}

let audioCapturerInfo: audio.AudioCapturerInfo = {
  source: audio.SourceType.SOURCE_TYPE_MIC,
  capturerFlags: 0
}

let audioCapturerOptions: audio.AudioCapturerOptions = {
  streamInfo: audioStreamInfo,
  capturerInfo: audioCapturerInfo
}

audio.createAudioCapturer(audioCapturerOptions, (err, data) => {
  if (err) {
    console.error(`AudioCapturer Created : Error: ${err}`);
  } else {
    console.info('AudioCapturer Created : Success : SUCCESS');
    let audioCapturer = data;
  }
});
```

## audio.createAudioCapturer

createAudioCapturer(options: AudioCapturerOptions): Promise<AudioCapturer\>

获取音频采集器。使用Promise 方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**需要权限：** ohos.permission.MICROPHONE

仅设置Mic音频源（即[SourceType](#sourcetype)为SOURCE_TYPE_MIC）时需要该权限。

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                          | 必填 | 说明             |
| :------ | :-------------------------------------------- | :--- | :--------------- |
| options | [AudioCapturerOptions](#audiocaptureroptions) | 是   | 配置音频采集器。 |

**返回值：**

| 类型                                     | 说明                              |
| ---------------------------------------- | --------------------------------- |
| Promise<[AudioCapturer](#audiocapturer)> | Promise对象，返回音频采集器对象。 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let audioStreamInfo: audio.AudioStreamInfo = {
  samplingRate: audio.AudioSamplingRate.SAMPLE_RATE_44100,
  channels: audio.AudioChannel.CHANNEL_2,
  sampleFormat: audio.AudioSampleFormat.SAMPLE_FORMAT_S16LE,
  encodingType: audio.AudioEncodingType.ENCODING_TYPE_RAW
}

let audioCapturerInfo: audio.AudioCapturerInfo = {
  source: audio.SourceType.SOURCE_TYPE_MIC,
  capturerFlags: 0
}

let audioCapturerOptions:audio.AudioCapturerOptions = {
  streamInfo: audioStreamInfo,
  capturerInfo: audioCapturerInfo
}

let audioCapturer: audio.AudioCapturer;
audio.createAudioCapturer(audioCapturerOptions).then((data) => {
  audioCapturer = data;
  console.info('AudioCapturer Created : Success : Stream Type: SUCCESS');
}).catch((err: BusinessError) => {
  console.error(`AudioCapturer Created : ERROR : ${err}`);
});
```

## AudioVolumeType

枚举值，定义音频流类型。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

| 名称          | 值   | 说明       | Android平台 | iOS平台 |
| ------------- | ---- | ---------- | ----------- | ------- |
| VOICE_CALL    | 0    | 语音电话。 | 支持        | 不支持  |
| RINGTONE      | 2    | 铃声。     | 支持        | 不支持  |
| MEDIA         | 3    | 媒体。     | 支持        | 不支持  |
| ALARM         | 4    | 闹钟。     | 支持        | 不支持  |
| ACCESSIBILITY | 5    | 无障碍。   | 支持        | 不支持  |

## InterruptMode

枚举值，定义焦点模型。

**系统能力：** SystemCapability.Multimedia.Audio.Interrupt

| 名称             | 值   | 说明           | Android平台 | iOS平台 |
| ---------------- | ---- | -------------- | ----------- | ------- |
| SHARE_MODE       | 0    | 共享焦点模式。 | 不支持      | 支持    |
| INDEPENDENT_MODE | 1    | 独立焦点模式。 | 不支持      | 支持    |

## DeviceFlag

枚举值，定义可获取的设备种类。

**系统能力：** SystemCapability.Multimedia.Audio.Device

| 名称                | 值   | 说明       | Android平台 | iOS平台 |
| ------------------- | ---- | ---------- | ----------- | ------- |
| OUTPUT_DEVICES_FLAG | 1    | 输出设备。 | 支持        | 不支持  |
| INPUT_DEVICES_FLAG  | 2    | 输入设备。 | 支持        | 支持    |
| ALL_DEVICES_FLAG    | 3    | 所有设备。 | 支持        | 不支持  |

## DeviceRole

枚举值，定义设备角色。

**系统能力：** SystemCapability.Multimedia.Audio.Device

| 名称          | 值   | 说明           | Android平台 | iOS平台 |
| ------------- | ---- | -------------- | ----------- | ------- |
| INPUT_DEVICE  | 1    | 输入设备角色。 | 支持        | 支持    |
| OUTPUT_DEVICE | 2    | 输出设备角色。 | 支持        | 支持    |

## DeviceType

枚举值，定义设备类型。

**系统能力：** SystemCapability.Multimedia.Audio.Device

| 名称             | 值   | 说明                                                      | Android平台 | iOS平台 |
| ---------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| INVALID          | 0    | 无效设备。                                                | 支持        | 不支持  |
| EARPIECE         | 1    | 听筒。                                                    | 支持        | 支持    |
| SPEAKER          | 2    | 扬声器。                                                  | 支持        | 支持    |
| WIRED_HEADSET    | 3    | 有线耳机，带麦克风。                                      | 支持        | 支持    |
| WIRED_HEADPHONES | 4    | 有线耳机，无麦克风。                                      | 支持        | 支持    |
| BLUETOOTH_SCO    | 7    | 蓝牙设备SCO（Synchronous Connection Oriented）连接。      | 支持        | 支持    |
| BLUETOOTH_A2DP   | 8    | 蓝牙设备A2DP（Advanced Audio Distribution Profile）连接。 | 支持        | 支持    |
| MIC              | 15   | 麦克风。                                                  | 支持        | 支持    |
| USB_HEADSET      | 22   | USB耳机，带麦克风。                                       | 支持        | 支持    |
| DEFAULT          | 1000 | 默认设备类型。                                            | 支持        | 支持    |

## CommunicationDeviceType

枚举值，定义用于通信的可用设备类型。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

| 名称    | 值   | 说明     | Android平台 | iOS平台 |
| ------- | ---- | -------- | ----------- | ------- |
| SPEAKER | 2    | 扬声器。 | 支持        | 支持    |

## AudioRingMode

枚举值，定义铃声模式。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

| 名称                | 值   | 说明       | Android平台 | iOS平台 |
| ------------------- | ---- | ---------- | ----------- | ------- |
| RINGER_MODE_SILENT  | 0    | 静音模式。 | 支持        | 不支持  |
| RINGER_MODE_VIBRATE | 1    | 震动模式。 | 支持        | 不支持  |
| RINGER_MODE_NORMAL  | 2    | 响铃模式。 | 支持        | 不支持  |

## AudioSampleFormat

枚举值，定义音频采样格式。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称                  | 值   | 说明                                                         | Android平台 | iOS平台 |
| --------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| SAMPLE_FORMAT_INVALID | -1   | 无效格式。                                                   | 支持        | 支持    |
| SAMPLE_FORMAT_U8      | 0    | 无符号8位整数。                                              | 支持        | 支持    |
| SAMPLE_FORMAT_S16LE   | 1    | 带符号的16位整数，小尾数。                                   | 支持        | 支持    |
| SAMPLE_FORMAT_S24LE   | 2    | 带符号的24位整数，小尾数。 <br>由于系统限制，该采样格式仅部分设备支持，请根据实际情况使用。 | 支持        | 支持    |
| SAMPLE_FORMAT_S32LE   | 3    | 带符号的32位整数，小尾数。 <br>由于系统限制，该采样格式仅部分设备支持，请根据实际情况使用。 | 支持        | 支持    |
| SAMPLE_FORMAT_F32LE   | 4    | 带符号的32位浮点数，小尾数。 <br>由于系统限制，该采样格式仅部分设备支持，请根据实际情况使用。 | 支持        | 支持    |

## AudioErrors

枚举值，定义音频错误码。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称                | 值      | 说明             | Android平台 | iOS平台 |
| ------------------- | ------- | ---------------- | ----------- | ------- |
| ERROR_INVALID_PARAM | 6800101 | 无效入参。       | 支持        | 支持    |
| ERROR_NO_MEMORY     | 6800102 | 分配内存失败。   | 支持        | 支持    |
| ERROR_ILLEGAL_STATE | 6800103 | 状态不支持。     | 支持        | 支持    |
| ERROR_UNSUPPORTED   | 6800104 | 参数选项不支持。 | 支持        | 支持    |
| ERROR_SYSTEM        | 6800301 | 系统处理异常。   | 支持        | 支持    |

## AudioChannel

枚举值， 定义音频声道。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称       | 值       | 说明       | Android平台 | iOS平台 |
| ---------- | -------- | ---------- | ----------- | ------- |
| CHANNEL_1  | 0x1 << 0 | 单声道。   | 支持        | 支持    |
| CHANNEL_2  | 0x1 << 1 | 双声道。   | 支持        | 支持    |
| CHANNEL_3  | 3        | 三声道。   | 支持        | 支持    |
| CHANNEL_4  | 4        | 四声道。   | 支持        | 支持    |
| CHANNEL_5  | 5        | 五声道。   | 支持        | 支持    |
| CHANNEL_6  | 6        | 六声道。   | 支持        | 支持    |
| CHANNEL_7  | 7        | 七声道。   | 支持        | 支持    |
| CHANNEL_8  | 8        | 八声道。   | 支持        | 支持    |
| CHANNEL_9  | 9        | 九声道。   | 不支持      | 支持    |
| CHANNEL_10 | 10       | 十声道。   | 支持        | 支持    |
| CHANNEL_12 | 12       | 十二声道。 | 支持        | 支持    |
| CHANNEL_14 | 14       | 十四声道。 | 支持        | 支持    |
| CHANNEL_16 | 16       | 十六声道。 | 支持        | 支持    |

## AudioSamplingRate

枚举值，定义音频采样率，具体设备支持的采样率规格会存在差异。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称              | 值    | 说明            | Android平台 | iOS平台 |
| ----------------- | ----- | --------------- | ----------- | ------- |
| SAMPLE_RATE_8000  | 8000  | 采样率为8000。  | 支持        | 支持    |
| SAMPLE_RATE_11025 | 11025 | 采样率为11025。 | 支持        | 支持    |
| SAMPLE_RATE_12000 | 12000 | 采样率为12000。 | 支持        | 支持    |
| SAMPLE_RATE_16000 | 16000 | 采样率为16000。 | 支持        | 支持    |
| SAMPLE_RATE_22050 | 22050 | 采样率为22050。 | 支持        | 支持    |
| SAMPLE_RATE_24000 | 24000 | 采样率为24000。 | 支持        | 支持    |
| SAMPLE_RATE_32000 | 32000 | 采样率为32000。 | 支持        | 支持    |
| SAMPLE_RATE_44100 | 44100 | 采样率为44100。 | 支持        | 支持    |
| SAMPLE_RATE_48000 | 48000 | 采样率为48000。 | 支持        | 支持    |
| SAMPLE_RATE_64000 | 64000 | 采样率为64000。 | 支持        | 支持    |
| SAMPLE_RATE_96000 | 96000 | 采样率为96000。 | 支持        | 支持    |

## AudioEncodingType

枚举值，定义音频编码类型。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称                  | 值   | 说明      | Android平台 | iOS平台 |
| --------------------- | ---- | --------- | ----------- | ------- |
| ENCODING_TYPE_INVALID | -1   | 无效。    | 支持        | 支持    |
| ENCODING_TYPE_RAW     | 0    | PCM编码。 | 支持        | 支持    |

## AudioChannelLayout

枚举值，定义音频文件声道布局类型。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称                          | 值             | 说明                                             | Android平台 | iOS平台 |
| ----------------------------- | -------------- | ------------------------------------------------ | ----------- | ------- |
| CH_LAYOUT_UNKNOWN             | 0x0            | 未知声道布局。                                   | 支持        | 支持    |
| CH_LAYOUT_MONO                | 0x4            | 声道布局为MONO。                                 | 支持        | 支持    |
| CH_LAYOUT_STEREO              | 0x3            | 声道布局为STEREO。                               | 支持        | 支持    |
| CH_LAYOUT_2POINT1             | 0xB            | 声道布局为2.1。                                  | 不支持      | 支持    |
| CH_LAYOUT_3POINT0             | 0x103          | 声道布局为3.0。                                  | 不支持      | 支持    |
| CH_LAYOUT_SURROUND            | 0x7            | 声道布局为SURROUND。                             | 支持        | 支持    |
| CH_LAYOUT_3POINT1             | 0xF            | 声道布局为3.1。                                  | 不支持      | 支持    |
| CH_LAYOUT_4POINT0             | 0x107          | 声道布局为4.0。                                  | 不支持      | 支持    |
| CH_LAYOUT_QUAD                | 0x33           | 声道布局为QUAD。                                 | 支持        | 支持    |
| CH_LAYOUT_2POINT0POINT2       | 0x3000000003   | 声道布局为2.0.2。                                | 不支持      | 支持    |
| CH_LAYOUT_4POINT1             | 0x10F          | 声道布局为4.1                                    | 不支持      | 支持    |
| CH_LAYOUT_5POINT0             | 0x607          | 声道布局为5.0。                                  | 不支持      | 支持    |
| CH_LAYOUT_5POINT1             | 0x60F          | 声道布局为5.1。                                  | 支持        | 支持    |
| CH_LAYOUT_6POINT0             | 0x707          | 声道布局为6.0。                                  | 不支持      | 支持    |
| CH_LAYOUT_HEXAGONAL           | 0x137          | 声道布局为HEXAGONAL。                            | 不支持      | 支持    |
| CH_LAYOUT_6POINT1             | 0x70F          | 声道布局为6.1。                                  | 支持        | 支持    |
| CH_LAYOUT_7POINT0             | 0x637          | 声道布局为7.0。                                  | 不支持      | 支持    |
| CH_LAYOUT_7POINT0_FRONT       | 0x6C7          | 声道布局为7.0-FRONT。                            | 不支持      | 支持    |
| CH_LAYOUT_7POINT1             | 0x63F          | 声道布局为7.1。                                  | 支持        | 支持    |
| CH_LAYOUT_OCTAGONAL           | 0x737          | 声道布局为OCTAGONAL。                            | 不支持      | 支持    |
| CH_LAYOUT_5POINT1POINT2       | 0x300000060F   | 声道布局为5.1.2。                                | 支持        | 支持    |
| CH_LAYOUT_5POINT1POINT4       | 0x2D60F        | 声道布局为5.1.4。                                | 支持        | 支持    |
| CH_LAYOUT_7POINT1POINT2       | 0x300000063F   | 声道布局为7.1.2。                                | 支持        | 支持    |
| CH_LAYOUT_7POINT1POINT4       | 0x2D63F        | 声道布局为7.1.4。                                | 支持        | 支持    |
| CH_LAYOUT_10POINT2            | 0x180005737    | 声道布局为10.2。                                 | 支持        | 支持    |
| CH_LAYOUT_9POINT1POINT4       | 0x18002D63F    | 声道布局为9.1.4。                                | 支持        | 不支持  |
| CH_LAYOUT_9POINT1POINT6       | 0x318002D63F   | 声道布局为9.1.6。                                | 支持        | 支持    |
| CH_LAYOUT_HEXADECAGONAL       | 0x18003F737    | 声道布局为HEXADECAGONAL。                        | 不支持      | 支持    |
| CH_LAYOUT_AMB_ORDER3_ACN_N3D  | 0x100000000003 | 声道排序为ACN_N3D（根据ITU标准）的三阶HOA文件。  | 不支持      | 支持    |
| CH_LAYOUT_AMB_ORDER3_ACN_SN3D | 0x100000001003 | 声道排序为ACN_SN3D（根据ITU标准）的三阶HOA文件。 | 不支持      | 支持    |

## StreamUsage

枚举值，定义音频流使用类型。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称                             | 值   | 说明         | Android平台 | iOS平台 |
| -------------------------------- | ---- | ------------ | ----------- | ------- |
| STREAM_USAGE_UNKNOWN             | 0    | 未知类型。   | 支持        | 不支持  |
| STREAM_USAGE_MUSIC               | 1    | 音乐。       | 支持        | 不支持  |
| STREAM_USAGE_VOICE_COMMUNICATION | 2    | 语音通信。   | 支持        | 不支持  |
| STREAM_USAGE_VOICE_ASSISTANT     | 3    | 语音播报。   | 支持        | 不支持  |
| STREAM_USAGE_ALARM               | 4    | 闹钟。       | 支持        | 不支持  |
| STREAM_USAGE_RINGTONE            | 6    | 铃声。       | 支持        | 不支持  |
| STREAM_USAGE_NOTIFICATION        | 7    | 通知。       | 支持        | 不支持  |
| STREAM_USAGE_ACCESSIBILITY       | 8    | 无障碍。     | 支持        | 不支持  |
| STREAM_USAGE_MOVIE               | 10   | 电影或视频。 | 支持        | 不支持  |
| STREAM_USAGE_GAME                | 11   | 游戏音效。   | 支持        | 不支持  |
| STREAM_USAGE_AUDIOBOOK           | 12   | 有声读物。   | 支持        | 不支持  |
| STREAM_USAGE_NAVIGATION          | 13   | 导航。       | 支持        | 不支持  |

## AudioState

枚举值，定义音频状态。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称           | 值   | 说明             | Android平台 | iOS平台 |
| -------------- | ---- | ---------------- | ----------- | ------- |
| STATE_INVALID  | -1   | 无效状态。       | 支持        | 支持    |
| STATE_NEW      | 0    | 创建新实例状态。 | 支持        | 支持    |
| STATE_PREPARED | 1    | 准备状态。       | 支持        | 支持    |
| STATE_RUNNING  | 2    | 运行状态。       | 支持        | 支持    |
| STATE_STOPPED  | 3    | 停止状态。       | 支持        | 支持    |
| STATE_RELEASED | 4    | 释放状态。       | 支持        | 支持    |
| STATE_PAUSED   | 5    | 暂停状态。       | 支持        | 支持    |

## InterruptType

枚举值，定义中断类型。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

| 名称                 | 值   | 说明                   | Android平台 | iOS平台 |
| -------------------- | ---- | ---------------------- | ----------- | ------- |
| INTERRUPT_TYPE_BEGIN | 1    | 音频播放中断事件开始。 | 不支持      | 支持    |
| INTERRUPT_TYPE_END   | 2    | 音频播放中断事件结束。 | 不支持      | 支持    |

## InterruptForceType

枚举值，定义强制打断类型。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

| 名称            | 值   | 说明                               | Android平台 | iOS平台 |
| --------------- | ---- | ---------------------------------- | ----------- | ------- |
| INTERRUPT_FORCE | 0    | 由系统进行操作，强制打断音频播放。 | 不支持      | 支持    |

## InterruptHint

枚举值，定义中断提示。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

| 名称                  | 值   | 说明           | Android平台 | iOS平台 |
| --------------------- | ---- | -------------- | ----------- | ------- |
| INTERRUPT_HINT_RESUME | 1    | 提示音频恢复。 | 不支持      | 支持    |

## AudioStreamInfo

音频流信息。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称          | 类型                                      | 必填 | 说明               | Android平台 | iOS平台 |
| ------------- | ----------------------------------------- | ---- | ------------------ | ----------- | ------- |
| samplingRate  | [AudioSamplingRate](#audiosamplingrate)   | 是   | 音频文件的采样率。 | 支持        | 支持    |
| channels      | [AudioChannel](#audiochannel)             | 是   | 音频文件的通道数。 | 支持        | 支持    |
| sampleFormat  | [AudioSampleFormat](#audiosampleformat)   | 是   | 音频采样格式。     | 支持        | 支持    |
| encodingType  | [AudioEncodingType](#audioencodingtype)   | 是   | 音频编码格式。     | 支持        | 支持    |
| channelLayout | [AudioChannelLayout](#audiochannellayout) | 否   | 音频声道布局。     | 支持        | 支持    |

## AudioRendererInfo

音频渲染器信息。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称          | 类型                        | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------- | --------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| usage         | [StreamUsage](#streamusage) | 是   | 音频流使用类型。                                             | 支持        | 不支持  |
| rendererFlags | number                      | 是   | 音频渲染器标志。<br>0代表普通音频渲染器，1代表低时延音频渲染器。ArkTS接口暂不支持低时延音频渲染器。 | 支持        | 支持    |

## AudioRendererOptions

音频渲染器选项信息。

| 名称         | 类型                                    | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------ | --------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| streamInfo   | [AudioStreamInfo](#audiostreaminfo)     | 是   | 表示音频流信息。<br/>**系统能力：** SystemCapability.Multimedia.Audio.Renderer | 支持        | 支持    |
| rendererInfo | [AudioRendererInfo](#audiorendererinfo) | 是   | 表示渲染器信息。<br/>**系统能力：** SystemCapability.Multimedia.Audio.Renderer | 支持        | 不支持  |
| privacyType  | [AudioPrivacyType](#audioprivacytype)   | 否   | 表示音频流是否可以被其他应用录制，默认值为0。<br/>**系统能力：** SystemCapability.Multimedia.Audio.PlaybackCapture | 支持        | 不支持  |

## AudioPrivacyType

枚举值，定义用于标识对应播放音频流是否支持被其他应用录制。

**系统能力：** SystemCapability.Multimedia.Audio.PlaybackCapture

| 名称                 | 值   | 说明                             | Android平台 | iOS平台 |
| -------------------- | ---- | -------------------------------- | ----------- | ------- |
| PRIVACY_TYPE_PUBLIC  | 0    | 表示音频流可以被其他应用录制。   | 支持        | 不支持  |
| PRIVACY_TYPE_PRIVATE | 1    | 表示音频流不可以被其他应用录制。 | 支持        | 不支持  |

## InterruptEvent

播放中断时，应用接收的中断事件。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

| 名称      | 类型                                      | 必填 | 说明                                 | Android平台 | iOS平台 |
| --------- | ----------------------------------------- | ---- | ------------------------------------ | ----------- | ------- |
| eventType | [InterruptType](#interrupttype)           | 是   | 中断事件类型，开始或是结束。         | 不支持      | 支持    |
| forceType | [InterruptForceType](#interruptforcetype) | 是   | 操作是由系统执行或是由应用程序执行。 | 不支持      | 支持    |
| hintType  | [InterruptHint](#interrupthint)           | 是   | 中断提示。                           | 不支持      | 支持    |

## VolumeEvent

音量改变时，应用接收的事件。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

| 名称   | 类型   | 必填 | 说明                                                     | Android平台 | iOS平台 |
| ------ | ------ | ---- | -------------------------------------------------------- | ----------- | ------- |
| volume | number | 是   | 音量等级，可设置范围通过getMinVolume和getMaxVolume获取。 | 不支持      | 支持    |

## DeviceChangeAction

描述设备连接状态变化和设备信息。

**系统能力：** SystemCapability.Multimedia.Audio.Device

| 名称              | 类型                                              | 必填 | 说明               | Android平台 | iOS平台 |
| :---------------- | :------------------------------------------------ | :--- | :----------------- | ----------- | ------- |
| type              | [DeviceChangeType](#devicechangetype)             | 是   | 设备连接状态变化。 | 支持        | 不支持  |
| deviceDescriptors | [AudioDeviceDescriptors](#audiodevicedescriptors) | 是   | 设备信息。         | 支持        | 不支持  |

## ChannelBlendMode

枚举值，定义声道混合模式类型。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称           | 值   | 说明                           | Android平台 | iOS平台 |
| :------------- | :--- | :----------------------------- | ----------- | ------- |
| MODE_DEFAULT   | 0    | 无声道混合。                   | 支持        | 不支持  |
| MODE_BLEND_LR  | 1    | 混合左右声道。                 | 支持        | 不支持  |
| MODE_ALL_LEFT  | 2    | 从左声道拷贝覆盖到右声道混合。 | 支持        | 不支持  |
| MODE_ALL_RIGHT | 3    | 从右声道拷贝覆盖到左声道混合。 | 支持        | 不支持  |

## AudioStreamDeviceChangeReason

枚举值，定义流设备变更原因。

**系统能力：** SystemCapability.Multimedia.Audio.Device

| 名称                          | 值   | 说明                                                       | Android平台 | iOS平台 |
| :---------------------------- | :--- | :--------------------------------------------------------- | ----------- | ------- |
| REASON_UNKNOWN                | 0    | 未知原因。                                                 | 不支持      | 支持    |
| REASON_NEW_DEVICE_AVAILABLE   | 1    | 新设备可用。                                               | 不支持      | 支持    |
| REASON_OLD_DEVICE_UNAVAILABLE | 2    | 旧设备不可用。当报告此原因时，应用程序应考虑暂停音频播放。 | 不支持      | 支持    |
| REASON_OVERRODE               | 3    | 强选。                                                     | 不支持      | 支持    |

## AudioStreamDeviceChangeInfo

流设备变更时，应用接收的事件。

**系统能力：** SystemCapability.Multimedia.Audio.Device

| 名称         | 类型                                                         | 必填 | 说明             | Android平台 | iOS平台 |
| :----------- | :----------------------------------------------------------- | :--- | :--------------- | ----------- | ------- |
| devices      | [AudioDeviceDescriptors](#audiodevicedescriptors)            | 是   | 设备信息。       | 不支持      | 支持    |
| changeReason | [AudioStreamDeviceChangeReason](#audiostreamdevicechangereason) | 是   | 流设备变更原因。 | 不支持      | 支持    |

## DeviceChangeType

枚举值，定义设备连接状态变化。

**系统能力：** SystemCapability.Multimedia.Audio.Device

| 名称       | 值   | 说明           | Android平台 | iOS平台 |
| :--------- | :--- | :------------- | ----------- | ------- |
| CONNECT    | 0    | 设备连接。     | 支持        | 不支持  |
| DISCONNECT | 1    | 断开设备连接。 | 支持        | 不支持  |

## AudioCapturerOptions

音频采集器选项信息。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

| 名称         | 类型                                    | 必填 | 说明             | Android平台 | iOS平台 |
| ------------ | --------------------------------------- | ---- | ---------------- | ----------- | ------- |
| streamInfo   | [AudioStreamInfo](#audiostreaminfo)     | 是   | 表示音频流信息。 | 支持        | 支持    |
| capturerInfo | [AudioCapturerInfo](#audiocapturerinfo) | 是   | 表示采集器信息。 | 支持        | 不支持  |

## AudioCapturerInfo

描述音频采集器信息。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称          | 类型                      | 必填 | 说明                                                         | Android平台 | iOS平台 |
| :------------ | :------------------------ | :--- | :----------------------------------------------------------- | ----------- | ------- |
| source        | [SourceType](#sourcetype) | 是   | 音源类型。                                                   | 支持        | 不支持  |
| capturerFlags | number                    | 是   | 音频采集器标志。<br>0代表普通音频采集器，1代表低时延音频采集器。ArkTS接口暂不支持低时延音频采集器。 | 支持        | 不支持  |

## SourceType

枚举值，定义音源类型。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称                            | 值   | 说明                   | Android平台 | iOS平台 |
| :------------------------------ | :--- | :--------------------- | ----------- | ------- |
| SOURCE_TYPE_MIC                 | 0    | Mic音频源。            | 支持        | 不支持  |
| SOURCE_TYPE_VOICE_RECOGNITION   | 1    | 语音识别源。           | 支持        | 不支持  |
| SOURCE_TYPE_VOICE_COMMUNICATION | 7    | 语音通话场景的音频源。 | 支持        | 不支持  |

## AudioScene

枚举值，定义音频场景。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

| 名称                   | 值   | 说明           | Android平台 | iOS平台 |
| :--------------------- | :--- | :------------- | ----------- | ------- |
| AUDIO_SCENE_DEFAULT    | 0    | 默认音频场景。 | 支持        | 支持    |
| AUDIO_SCENE_VOICE_CHAT | 3    | 语音聊天模式。 | 支持        | 支持    |

## AudioManager

管理音频音量和音频设备。在调用AudioManager的接口前，需要先通过[getAudioManager](#audiogetaudiomanager)创建实例。

### getAudioScene

getAudioScene\(callback: AsyncCallback<AudioScene\>\): void

获取音频场景模式，使用callback方式返回异步结果。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                     | 必填 | 说明                                                         |
| :------- | :--------------------------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<[AudioScene](#audioscene)> | 是   | 回调函数。当获取音频场景模式成功，err为undefined，data为获取到的音频场景模式；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioManager.getAudioScene((err: BusinessError, value: audio.AudioScene) => {
  if (err) {
    console.error(`Failed to obtain the audio scene mode. ${err}`);
    return;
  }
  console.info(`Callback invoked to indicate that the audio scene mode is obtained ${value}.`);
});
```

### getAudioScene

getAudioScene\(\): Promise<AudioScene\>

获取音频场景模式，使用Promise方式返回异步结果。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**返回值：**

| 类型                               | 说明                            |
| :--------------------------------- | :------------------------------ |
| Promise<[AudioScene](#audioscene)> | Promise对象，返回音频场景模式。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioManager.getAudioScene().then((value: audio.AudioScene) => {
  console.info(`Promise returned to indicate that the audio scene mode is obtained ${value}.`);
}).catch ((err: BusinessError) => {
  console.error(`Failed to obtain the audio scene mode ${err}`);
});
```

### getAudioSceneSync

getAudioSceneSync\(\): AudioScene

获取音频场景模式，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**返回值：**

| 类型                      | 说明           |
| :------------------------ | :------------- |
| [AudioScene](#audioscene) | 音频场景模式。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: audio.AudioScene = audioManager.getAudioSceneSync();
  console.info(`indicate that the audio scene mode is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the audio scene mode ${error}`);
}
```

### getVolumeManager

getVolumeManager(): AudioVolumeManager

获取音频音量管理器。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**返回值：**

| 类型                                      | 说明                   |
| ----------------------------------------- | ---------------------- |
| [AudioVolumeManager](#audiovolumemanager) | AudioVolumeManager实例 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let audioVolumeManager: audio.AudioVolumeManager = audioManager.getVolumeManager();
```

### getStreamManager

getStreamManager(): AudioStreamManager

获取音频流管理器。

**系统能力：** SystemCapability.Multimedia.Audio.Core

**支持平台：** Android、iOS

**返回值：**

| 类型                                      | 说明                   |
| ----------------------------------------- | ---------------------- |
| [AudioStreamManager](#audiostreammanager) | AudioStreamManager实例 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let audioStreamManager: audio.AudioStreamManager = audioManager.getStreamManager();
```

### getRoutingManager

getRoutingManager(): AudioRoutingManager

获取音频路由设备管理器。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**返回值：**

| 类型                                        | 说明                    |
| ------------------------------------------- | ----------------------- |
| [AudioRoutingManager](#audioroutingmanager) | AudioRoutingManager实例 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let audioRoutingManager: audio.AudioRoutingManager = audioManager.getRoutingManager();
```

## AudioVolumeManager

音量管理。在使用AudioVolumeManager的接口前，需要使用[getVolumeManager](#getvolumemanager)获取AudioVolumeManager实例。

### getVolumeGroupManager

getVolumeGroupManager(groupId: number, callback: AsyncCallback<AudioVolumeGroupManager\>\): void

获取音频组管理器，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| groupId  | number                                                       | 是   | 音量组id，默认使用LOCAL_VOLUME_GROUP_ID。                    |
| callback | AsyncCallback&lt;[AudioVolumeGroupManager](#audiovolumegroupmanager)&gt; | 是   | 回调函数。当获取音频组管理器成功，err为undefined，data为获取到的音频组管理器对象；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let groupId: number = audio.DEFAULT_VOLUME_GROUP_ID;
audioVolumeManager.getVolumeGroupManager(groupId, (err: BusinessError, value: audio.AudioVolumeGroupManager) => {
  if (err) {
    console.error(`Failed to obtain the volume group infos list. ${err}`);
    return;
  }
  console.info('Callback invoked to indicate that the volume group infos list is obtained.');
});

```

### getVolumeGroupManager

getVolumeGroupManager(groupId: number\): Promise<AudioVolumeGroupManager\>

获取音频组管理器，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型   | 必填 | 说明                                      |
| ------- | ------ | ---- | ----------------------------------------- |
| groupId | number | 是   | 音量组id，默认使用LOCAL_VOLUME_GROUP_ID。 |

**返回值：**

| 类型                                                         | 说明                          |
| ------------------------------------------------------------ | ----------------------------- |
| Promise&lt; [AudioVolumeGroupManager](#audiovolumegroupmanager) &gt; | Promise对象，返回音量组实例。 |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let groupId: number = audio.DEFAULT_VOLUME_GROUP_ID;
let audioVolumeGroupManager: audio.AudioVolumeGroupManager | undefined = undefined;
async function getVolumeGroupManager(){
  audioVolumeGroupManager = await audioVolumeManager.getVolumeGroupManager(groupId);
  console.info('Promise returned to indicate that the volume group infos list is obtained.');
}
```

### getVolumeGroupManagerSync

getVolumeGroupManagerSync(groupId: number\): AudioVolumeGroupManager

获取音频组管理器，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型   | 必填 | 说明                                      |
| ------- | ------ | ---- | ----------------------------------------- |
| groupId | number | 是   | 音量组id，默认使用LOCAL_VOLUME_GROUP_ID。 |

**返回值：**

| 类型                                                | 说明         |
| --------------------------------------------------- | ------------ |
| [AudioVolumeGroupManager](#audiovolumegroupmanager) | 音量组实例。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let audioVolumeGroupManager: audio.AudioVolumeGroupManager = audioVolumeManager.getVolumeGroupManagerSync(audio.DEFAULT_VOLUME_GROUP_ID);
  console.info(`Get audioVolumeGroupManager success.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to get audioVolumeGroupManager, error: ${error}`);
}
```

### on('volumeChange')

on(type: 'volumeChange', callback: Callback\<VolumeEvent>): void

监听系统音量变化事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** iOS

**参数：**

| 参数名   | 类型                                  | 必填 | 说明                                         |
| -------- | ------------------------------------- | ---- | -------------------------------------------- |
| type     | string                                | 是   | 事件回调类型，支持的事件为：'volumeChange'。 |
| callback | Callback<[VolumeEvent](#volumeevent)> | 是   | 回调函数，返回变化后的音量信息。             |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioVolumeManager.on('volumeChange', (volumeEvent: audio.VolumeEvent) => {
  console.info(`Volume level: ${volumeEvent.volume} `);
});
```

## AudioVolumeGroupManager

管理音频组音量。在调用AudioVolumeGroupManager的接口前，需要先通过 [getVolumeGroupManager](#getvolumegroupmanager) 创建实例。

### getVolume

getVolume(volumeType: AudioVolumeType, callback: AsyncCallback&lt;number&gt;): void

获取指定流的音量，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明                                                         |
| ---------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。                                                 |
| callback   | AsyncCallback&lt;number&gt;         | 是   | 回调函数。当获取指定流的音量成功，err为undefined，data为获取到的指定流的音量；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.getVolume(audio.AudioVolumeType.MEDIA, (err: BusinessError, value: number) => {
  if (err) {
    console.error(`Failed to obtain the volume. ${err}`);
    return;
  }
  console.info('Callback invoked to indicate that the volume is obtained.');
});
```

### getVolume

getVolume(volumeType: AudioVolumeType): Promise&lt;number&gt;

获取指定流的音量，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型                  | 说明                        |
| --------------------- | --------------------------- |
| Promise&lt;number&gt; | Promise对象，返回音量大小。 |

**示例：**

```ts
audioVolumeGroupManager.getVolume(audio.AudioVolumeType.MEDIA).then((value: number) => {
  console.info(`Promise returned to indicate that the volume is obtained ${value}.`);
});
```

### getVolumeSync

getVolumeSync(volumeType: AudioVolumeType): number;

获取指定流的音量，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型   | 说明           |
| ------ | -------------- |
| number | 返回音量大小。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: number = audioVolumeGroupManager.getVolumeSync(audio.AudioVolumeType.MEDIA);
  console.info(`Indicate that the volume is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the volume, error ${error}.`);
}
```

### getMinVolume

getMinVolume(volumeType: AudioVolumeType, callback: AsyncCallback&lt;number&gt;): void

获取指定流的最小音量，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明                                                         |
| ---------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。                                                 |
| callback   | AsyncCallback&lt;number&gt;         | 是   | 回调函数。当获取指定流的最小音量成功，err为undefined，data为获取到的指定流的最小音量；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.getMinVolume(audio.AudioVolumeType.MEDIA, (err: BusinessError, value: number) => {
  if (err) {
    console.error(`Failed to obtain the minimum volume. ${err}`);
    return;
  }
  console.info(`Callback invoked to indicate that the minimum volume is obtained. ${value}`);
});
```

### getMinVolume

getMinVolume(volumeType: AudioVolumeType): Promise&lt;number&gt;

获取指定流的最小音量，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型                  | 说明                        |
| --------------------- | --------------------------- |
| Promise&lt;number&gt; | Promise对象，返回最小音量。 |

**示例：**

```ts
audioVolumeGroupManager.getMinVolume(audio.AudioVolumeType.MEDIA).then((value: number) => {
  console.info(`Promised returned to indicate that the minimum volume is obtained ${value}.`);
});
```

### getMinVolumeSync

getMinVolumeSync(volumeType: AudioVolumeType): number;

获取指定流的最小音量，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型   | 说明           |
| ------ | -------------- |
| number | 返回最小音量。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: number = audioVolumeGroupManager.getMinVolumeSync(audio.AudioVolumeType.MEDIA);
  console.info(`Indicate that the minimum volume is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the minimum volume, error ${error}.`);
}
```

### getMaxVolume

getMaxVolume(volumeType: AudioVolumeType, callback: AsyncCallback&lt;number&gt;): void

获取指定流的最大音量，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明                                                         |
| ---------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。                                                 |
| callback   | AsyncCallback&lt;number&gt;         | 是   | 回调函数。当获取指定流的最大音量成功，err为undefined，data为获取到的指定流的最大音量；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.getMaxVolume(audio.AudioVolumeType.MEDIA, (err: BusinessError, value: number) => {
  if (err) {
    console.error(`Failed to obtain the maximum volume. ${err}`);
    return;
  }
  console.info(`Callback invoked to indicate that the maximum volume is obtained. ${value}`);
});
```

### getMaxVolume

getMaxVolume(volumeType: AudioVolumeType): Promise&lt;number&gt;

获取指定流的最大音量，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型                  | 说明                            |
| --------------------- | ------------------------------- |
| Promise&lt;number&gt; | Promise对象，返回最大音量大小。 |

**示例：**

```ts
audioVolumeGroupManager.getMaxVolume(audio.AudioVolumeType.MEDIA).then((data: number) => {
  console.info('Promised returned to indicate that the maximum volume is obtained.');
});
```

### getMaxVolumeSync

getMaxVolumeSync(volumeType: AudioVolumeType): number;

获取指定流的最大音量，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型   | 说明               |
| ------ | ------------------ |
| number | 返回最大音量大小。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: number = audioVolumeGroupManager.getMaxVolumeSync(audio.AudioVolumeType.MEDIA);
  console.info(`Indicate that the maximum volume is obtained. ${value}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the maximum volume, error ${error}.`);
}
```

### isMute

isMute(volumeType: AudioVolumeType, callback: AsyncCallback&lt;boolean&gt;): void

获取指定音量流是否被静音，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名     | 类型                                | 必填 | 说明                                                         |
| ---------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。                                                 |
| callback   | AsyncCallback&lt;boolean&gt;        | 是   | 回调函数。当获取指定音量流是否被静音成功，err为undefined，data为true为静音，false为非静音；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.isMute(audio.AudioVolumeType.MEDIA, (err: BusinessError, value: boolean) => {
  if (err) {
    console.error(`Failed to obtain the mute status. ${err}`);
    return;
  }
  console.info(`Callback invoked to indicate that the mute status of the stream is obtained ${value}.`);
});
```

### isMute

isMute(volumeType: AudioVolumeType): Promise&lt;boolean&gt;

获取指定音量流是否被静音，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型                   | 说明                                                     |
| ---------------------- | -------------------------------------------------------- |
| Promise&lt;boolean&gt; | Promise对象，返回流静音状态，true为静音，false为非静音。 |

**示例：**

```ts
audioVolumeGroupManager.isMute(audio.AudioVolumeType.MEDIA).then((value: boolean) => {
  console.info(`Promise returned to indicate that the mute status of the stream is obtained ${value}.`);
});
```

### isMuteSync

isMuteSync(volumeType: AudioVolumeType): boolean

获取指定音量流是否被静音，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |

**返回值：**

| 类型    | 说明                                        |
| ------- | ------------------------------------------- |
| boolean | 返回流静音状态，true为静音，false为非静音。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: boolean = audioVolumeGroupManager.isMuteSync(audio.AudioVolumeType.MEDIA);
  console.info(`Indicate that the mute status of the stream is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the mute status of the stream, error ${error}.`);
}
```

### getRingerMode

getRingerMode(callback: AsyncCallback&lt;AudioRingMode&gt;): void

获取铃声模式，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名   | 类型                                                 | 必填 | 说明                                                         |
| -------- | ---------------------------------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback&lt;[AudioRingMode](#audioringmode)&gt; | 是   | 回调函数。当获取铃声模式成功，err为undefined，data为获取到的铃声模式；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.getRingerMode((err: BusinessError, value: audio.AudioRingMode) => {
  if (err) {
    console.error(`Failed to obtain the ringer mode. ${err}`);
    return;
  }
  console.info(`Callback invoked to indicate that the ringer mode is obtained ${value}.`);
});
```

### getRingerMode

getRingerMode(): Promise&lt;AudioRingMode&gt;

获取铃声模式，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**返回值：**

| 类型                                           | 说明                              |
| ---------------------------------------------- | --------------------------------- |
| Promise&lt;[AudioRingMode](#audioringmode)&gt; | Promise对象，返回系统的铃声模式。 |

**示例：**

```ts
audioVolumeGroupManager.getRingerMode().then((value: audio.AudioRingMode) => {
  console.info(`Promise returned to indicate that the ringer mode is obtained ${value}.`);
});
```

### getRingerModeSync

getRingerModeSync(): AudioRingMode

获取铃声模式，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**返回值：**

| 类型                            | 说明                 |
| ------------------------------- | -------------------- |
| [AudioRingMode](#audioringmode) | 返回系统的铃声模式。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: audio.AudioRingMode = audioVolumeGroupManager.getRingerModeSync();
  console.info(`Indicate that the ringer mode is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the ringer mode, error ${error}.`);
}
```

### isMicrophoneMute

isMicrophoneMute(callback: AsyncCallback&lt;boolean&gt;): void

获取麦克风静音状态，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名   | 类型                         | 必填 | 说明                                                         |
| -------- | ---------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback&lt;boolean&gt; | 是   | 回调函数。当获取麦克风静音状态成功，err为undefined，data为true为静音，false为非静音；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.isMicrophoneMute((err: BusinessError, value: boolean) => {
  if (err) {
    console.error(`Failed to obtain the mute status of the microphone. ${err}`);
    return;
  }
  console.info(`Callback invoked to indicate that the mute status of the microphone is obtained ${value}.`);
});
```

### isMicrophoneMute

isMicrophoneMute(): Promise&lt;boolean&gt;

获取麦克风静音状态，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**返回值：**

| 类型                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Promise&lt;boolean&gt; | Promise对象，返回系统麦克风静音状态，true为静音，false为非静音。 |

**示例：**

```ts
audioVolumeGroupManager.isMicrophoneMute().then((value: boolean) => {
  console.info(`Promise returned to indicate that the mute status of the microphone is obtained ${value}.`);
});
```

### isMicrophoneMuteSync

isMicrophoneMuteSync(): boolean

获取麦克风静音状态，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android、iOS

**返回值：**

| 类型    | 说明                                                |
| ------- | --------------------------------------------------- |
| boolean | 返回系统麦克风静音状态，true为静音，false为非静音。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: boolean = audioVolumeGroupManager.isMicrophoneMuteSync();
  console.info(`Indicate that the mute status of the microphone is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the mute status of the microphone, error ${error}.`);
}
```

### isVolumeUnadjustable

isVolumeUnadjustable(): boolean

获取固定音量模式开关状态，打开时进入固定音量模式，此时音量固定无法被调节，使用同步方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 同步接口，返回固定音量模式开关状态，true为固定音量模式，false为非固定音量模式。 |

**示例：**

```ts
let volumeAdjustSwitch: boolean = audioVolumeGroupManager.isVolumeUnadjustable();
console.info(`Whether it is volume unadjustable: ${volumeAdjustSwitch}.`);
```

### getSystemVolumeInDb

getSystemVolumeInDb(volumeType: AudioVolumeType, volumeLevel: number, device: DeviceType, callback: AsyncCallback&lt;number&gt;): void

获取音量增益dB值，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名      | 类型                                | 必填 | 说明                                                         |
| ----------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| volumeType  | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。                                                 |
| volumeLevel | number                              | 是   | 音量等级。                                                   |
| device      | [DeviceType](#devicetype)           | 是   | 设备类型。                                                   |
| callback    | AsyncCallback&lt;number&gt;         | 是   | 回调函数。当获取音量增益dB值成功，err为undefined，data为获取到的音量增益dB值；否则为错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                     |
| -------- | -------------------------------------------- |
| 6800101  | Invalid parameter error. Return by callback. |
| 6800301  | System error. Return by callback.            |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.getSystemVolumeInDb(audio.AudioVolumeType.MEDIA, 3, audio.DeviceType.SPEAKER, (err: BusinessError, dB: number) => {
  if (err) {
    console.error(`Failed to get the volume DB. ${err}`);
  } else {
    console.info(`Success to get the volume DB. ${dB}`);
  }
});
```

### getSystemVolumeInDb

getSystemVolumeInDb(volumeType: AudioVolumeType, volumeLevel: number, device: DeviceType): Promise&lt;number&gt;

获取音量增益dB值，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名      | 类型                                | 必填 | 说明         |
| ----------- | ----------------------------------- | ---- | ------------ |
| volumeType  | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |
| volumeLevel | number                              | 是   | 音量等级。   |
| device      | [DeviceType](#devicetype)           | 是   | 设备类型。   |

**返回值：**

| 类型                  | 说明                                  |
| --------------------- | ------------------------------------- |
| Promise&lt;number&gt; | Promise对象，返回对应的音量增益dB值。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                    |
| -------- | ------------------------------------------- |
| 6800101  | Invalid parameter error. Return by promise. |
| 6800301  | System error. Return by promise.            |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioVolumeGroupManager.getSystemVolumeInDb(audio.AudioVolumeType.MEDIA, 3, audio.DeviceType.SPEAKER).then((value: number) => {
  console.info(`Success to get the volume DB. ${value}`);
}).catch((error: BusinessError) => {
  console.error(`Fail to adjust the system volume by step. ${error}`);
});
```

### getSystemVolumeInDbSync

getSystemVolumeInDbSync(volumeType: AudioVolumeType, volumeLevel: number, device: DeviceType): number

获取音量增益dB值，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Volume

**支持平台：** Android

**参数：**

| 参数名      | 类型                                | 必填 | 说明         |
| ----------- | ----------------------------------- | ---- | ------------ |
| volumeType  | [AudioVolumeType](#audiovolumetype) | 是   | 音量流类型。 |
| volumeLevel | number                              | 是   | 音量等级。   |
| device      | [DeviceType](#devicetype)           | 是   | 设备类型。   |

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| number | 返回对应的音量增益dB值。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: number = audioVolumeGroupManager.getSystemVolumeInDbSync(audio.AudioVolumeType.MEDIA, 3, audio.DeviceType.SPEAKER);
  console.info(`Success to get the volume DB. ${value}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Fail to adjust the system volume by step. ${error}`);
}
```

## AudioStreamManager

管理音频流。在使用AudioStreamManager的API前，需要使用[getStreamManager](#getstreammanager)获取AudioStreamManager实例。

### getCurrentAudioRendererInfoArray

getCurrentAudioRendererInfoArray(callback: AsyncCallback&lt;AudioRendererChangeInfoArray&gt;): void

获取当前音频渲染器的信息。使用callback异步回调。

**系统能力**: SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback<[AudioRendererChangeInfoArray](#audiorendererchangeinfoarray)> | 是   | 回调函数。当获取当前音频渲染器的信息成功，err为undefined，data为获取到的当前音频渲染器的信息；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioStreamManager.getCurrentAudioRendererInfoArray(async (err: BusinessError, AudioRendererChangeInfoArray: audio.AudioRendererChangeInfoArray) => {
  console.info('getCurrentAudioRendererInfoArray **** Get Callback Called ****');
  if (err) {
    console.error(`getCurrentAudioRendererInfoArray :ERROR: ${err}`);
  } else {
    if (AudioRendererChangeInfoArray != null) {
      for (let i = 0; i < AudioRendererChangeInfoArray.length; i++) {
        let AudioRendererChangeInfo: audio.AudioRendererChangeInfo = AudioRendererChangeInfoArray[i];
        console.info(`StreamId for ${i} is: ${AudioRendererChangeInfo.streamId}`);
        console.info(`Stream ${i} is: ${AudioRendererChangeInfo.rendererInfo.usage}`);
        console.info(`Flag ${i} is: ${AudioRendererChangeInfo.rendererInfo.rendererFlags}`);
        for (let j = 0;j < AudioRendererChangeInfo.deviceDescriptors.length; j++) {
          console.info(`Id: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].id}`);
          console.info(`Type: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceType}`);
          console.info(`Role: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceRole}`);
          console.info(`Name: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].name}`);
          console.info(`Address: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].address}`);
          console.info(`SampleRate: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].sampleRates[0]}`);
          console.info(`ChannelCount: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelCounts[0]}`);
          console.info(`ChannelMask: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelMasks[0]}`);
        }
      }
    }
  }
});
```

### getCurrentAudioRendererInfoArray

getCurrentAudioRendererInfoArray(): Promise&lt;AudioRendererChangeInfoArray&gt;

获取当前音频渲染器的信息。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                                                         | 说明                                  |
| ------------------------------------------------------------ | ------------------------------------- |
| Promise<[AudioRendererChangeInfoArray](#audiorendererchangeinfoarray)> | Promise对象，返回当前音频渲染器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

async function getCurrentAudioRendererInfoArray(){
  await audioStreamManager.getCurrentAudioRendererInfoArray().then((AudioRendererChangeInfoArray: audio.AudioRendererChangeInfoArray) => {
    console.info(`getCurrentAudioRendererInfoArray ######### Get Promise is called ##########`);
    if (AudioRendererChangeInfoArray != null) {
      for (let i = 0; i < AudioRendererChangeInfoArray.length; i++) {
        let AudioRendererChangeInfo: audio.AudioRendererChangeInfo = AudioRendererChangeInfoArray[i];
        console.info(`StreamId for ${i} is: ${AudioRendererChangeInfo.streamId}`);
        console.info(`Stream ${i} is: ${AudioRendererChangeInfo.rendererInfo.usage}`);
        console.info(`Flag ${i} is: ${AudioRendererChangeInfo.rendererInfo.rendererFlags}`);
        for (let j = 0;j < AudioRendererChangeInfo.deviceDescriptors.length; j++) {
          console.info(`Id: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].id}`);
          console.info(`Type: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceType}`);
          console.info(`Role: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceRole}`);
          console.info(`Name: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].name}`);
          console.info(`Address: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].address}`);
          console.info(`SampleRate: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].sampleRates[0]}`);
          console.info(`ChannelCount: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelCounts[0]}`);
          console.info(`ChannelMask: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelMasks[0]}`);
        }
      }
    }
  }).catch((err: BusinessError) => {
    console.error(`getCurrentAudioRendererInfoArray :ERROR: ${err}`);
  });
}
```

### getCurrentAudioRendererInfoArraySync

getCurrentAudioRendererInfoArraySync(): AudioRendererChangeInfoArray

获取当前音频渲染器的信息，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                                                         | 说明                     |
| ------------------------------------------------------------ | ------------------------ |
| [AudioRendererChangeInfoArray](#audiorendererchangeinfoarray) | 返回当前音频渲染器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let audioRendererChangeInfoArray: audio.AudioRendererChangeInfoArray = audioStreamManager.getCurrentAudioRendererInfoArraySync();
  console.info(`getCurrentAudioRendererInfoArraySync success.`);
  if (audioRendererChangeInfoArray != null) {
    for (let i = 0; i < audioRendererChangeInfoArray.length; i++) {
      let AudioRendererChangeInfo: audio.AudioRendererChangeInfo = audioRendererChangeInfoArray[i];
      console.info(`StreamId for ${i} is: ${AudioRendererChangeInfo.streamId}`);
      console.info(`Stream ${i} is: ${AudioRendererChangeInfo.rendererInfo.usage}`);
      console.info(`Flag ${i} is: ${AudioRendererChangeInfo.rendererInfo.rendererFlags}`);
      for (let j = 0;j < AudioRendererChangeInfo.deviceDescriptors.length; j++) {
        console.info(`Id: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].id}`);
        console.info(`Type: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceType}`);
        console.info(`Role: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceRole}`);
        console.info(`Name: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].name}`);
        console.info(`Address: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].address}`);
        console.info(`SampleRate: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].sampleRates[0]}`);
        console.info(`ChannelCount: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelCounts[0]}`);
        console.info(`ChannelMask: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelMasks[0]}`);
      }
    }
  }
} catch (err) {
  let error = err as BusinessError;
  console.error(`getCurrentAudioRendererInfoArraySync :ERROR: ${error}`);
}
```

### getCurrentAudioCapturerInfoArray

getCurrentAudioCapturerInfoArray(callback: AsyncCallback&lt;AudioCapturerChangeInfoArray&gt;): void

获取当前音频采集器的信息。使用callback异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback<[AudioCapturerChangeInfoArray](#audiocapturerchangeinfoarray)> | 是   | 回调函数。当获取当前音频采集器的信息成功，err为undefined，data为获取到的当前音频采集器的信息；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioStreamManager.getCurrentAudioCapturerInfoArray(async (err: BusinessError, AudioCapturerChangeInfoArray: audio.AudioCapturerChangeInfoArray) => {
  console.info('getCurrentAudioCapturerInfoArray **** Get Callback Called ****');
  if (err) {
    console.error(`getCurrentAudioCapturerInfoArray :ERROR: ${err}`);
  } else {
    if (AudioCapturerChangeInfoArray != null) {
      for (let i = 0; i < AudioCapturerChangeInfoArray.length; i++) {
        console.info(`StreamId for ${i} is: ${AudioCapturerChangeInfoArray[i].streamId}`);
        console.info(`Source for ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.source}`);
        console.info(`Flag  ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.capturerFlags}`);
        for (let j = 0; j < AudioCapturerChangeInfoArray[i].deviceDescriptors.length; j++) {
          console.info(`Id: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].id}`);
          console.info(`Type: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceType}`);
          console.info(`Role: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceRole}`);
          console.info(`Name: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].name}`);
          console.info(`Address: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].address}`);
          console.info(`SampleRate: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].sampleRates[0]}`);
          console.info(`ChannelCount: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelCounts[0]}`);
          console.info(`ChannelMask: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelMasks[0]}`);
        }
      }
    }
  }
});
```

### getCurrentAudioCapturerInfoArray

getCurrentAudioCapturerInfoArray(): Promise&lt;AudioCapturerChangeInfoArray&gt;

获取当前音频采集器的信息。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                                                         | 说明                                  |
| ------------------------------------------------------------ | ------------------------------------- |
| Promise<[AudioCapturerChangeInfoArray](#audiocapturerchangeinfoarray)> | Promise对象，返回当前音频采集器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

async function getCurrentAudioCapturerInfoArray(){
  await audioStreamManager.getCurrentAudioCapturerInfoArray().then((AudioCapturerChangeInfoArray: audio.AudioCapturerChangeInfoArray) => {
    console.info('getCurrentAudioCapturerInfoArray **** Get Promise Called ****');
    if (AudioCapturerChangeInfoArray != null) {
      for (let i = 0; i < AudioCapturerChangeInfoArray.length; i++) {
        console.info(`StreamId for ${i} is: ${AudioCapturerChangeInfoArray[i].streamId}`);
        console.info(`Source for ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.source}`);
        console.info(`Flag  ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.capturerFlags}`);
        for (let j = 0; j < AudioCapturerChangeInfoArray[i].deviceDescriptors.length; j++) {
          console.info(`Id: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].id}`);
          console.info(`Type: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceType}`);
          console.info(`Role: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceRole}`);
          console.info(`Name: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].name}`);
          console.info(`Address: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].address}`);
          console.info(`SampleRate: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].sampleRates[0]}`);
          console.info(`ChannelCount: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelCounts[0]}`);
          console.info(`ChannelMask: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelMasks[0]}`);
        }
      }
    }
  }).catch((err: BusinessError) => {
    console.error(`getCurrentAudioCapturerInfoArray :ERROR: ${err}`);
  });
}
```

### getCurrentAudioCapturerInfoArraySync

getCurrentAudioCapturerInfoArraySync(): AudioCapturerChangeInfoArray

获取当前音频采集器的信息，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型                                                         | 说明                     |
| ------------------------------------------------------------ | ------------------------ |
| [AudioCapturerChangeInfoArray](#audiocapturerchangeinfoarray) | 返回当前音频采集器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let audioCapturerChangeInfoArray: audio.AudioCapturerChangeInfoArray = audioStreamManager.getCurrentAudioCapturerInfoArraySync();
  console.info('getCurrentAudioCapturerInfoArraySync success.');
  if (audioCapturerChangeInfoArray != null) {
    for (let i = 0; i < audioCapturerChangeInfoArray.length; i++) {
      console.info(`StreamId for ${i} is: ${audioCapturerChangeInfoArray[i].streamId}`);
      console.info(`Source for ${i} is: ${audioCapturerChangeInfoArray[i].capturerInfo.source}`);
      console.info(`Flag  ${i} is: ${audioCapturerChangeInfoArray[i].capturerInfo.capturerFlags}`);
      for (let j = 0; j < audioCapturerChangeInfoArray[i].deviceDescriptors.length; j++) {
        console.info(`Id: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].id}`);
        console.info(`Type: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceType}`);
        console.info(`Role: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceRole}`);
        console.info(`Name: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].name}`);
        console.info(`Address: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].address}`);
        console.info(`SampleRate: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].sampleRates[0]}`);
        console.info(`ChannelCount: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].channelCounts[0]}`);
        console.info(`ChannelMask: ${i} : ${audioCapturerChangeInfoArray[i].deviceDescriptors[j].channelMasks[0]}`);
      }
    }
  }
} catch (err) {
  let error = err as BusinessError;
  console.error(`getCurrentAudioCapturerInfoArraySync ERROR: ${error}`);
}
```

### on('audioRendererChange')

on(type: 'audioRendererChange', callback: Callback&lt;AudioRendererChangeInfoArray&gt;): void

监听音频渲染器更改事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 事件类型，支持的事件`'audioRendererChange'`：当音频渲染器发生更改时触发。 |
| callback | Callback<[AudioRendererChangeInfoArray](#audiorendererchangeinfoarray)> | 是   | 回调函数，返回当前音频渲染器信息。                           |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioStreamManager.on('audioRendererChange',  (AudioRendererChangeInfoArray: audio.AudioRendererChangeInfoArray) => {
  for (let i = 0; i < AudioRendererChangeInfoArray.length; i++) {
    let AudioRendererChangeInfo: audio.AudioRendererChangeInfo = AudioRendererChangeInfoArray[i];
    console.info(`## RendererChange on is called for ${i} ##`);
    console.info(`StreamId for ${i} is: ${AudioRendererChangeInfo.streamId}`);
    console.info(`Stream ${i} is: ${AudioRendererChangeInfo.rendererInfo.usage}`);
    console.info(`Flag ${i} is: ${AudioRendererChangeInfo.rendererInfo.rendererFlags}`);
    for (let j = 0;j < AudioRendererChangeInfo.deviceDescriptors.length; j++) {
      console.info(`Id: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].id}`);
      console.info(`Type: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceType}`);
      console.info(`Role: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].deviceRole}`);
      console.info(`Name: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].name}`);
      console.info(`Address: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].address}`);
      console.info(`SampleRate: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].sampleRates[0]}`);
      console.info(`ChannelCount: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelCounts[0]}`);
      console.info(`ChannelMask: ${i} : ${AudioRendererChangeInfo.deviceDescriptors[j].channelMasks[0]}`);
    }
  }
});
```

### off('audioRendererChange')

off(type: 'audioRendererChange'): void

取消监听音频渲染器更改事件。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| type   | string | 是   | 事件类型，支持的事件`'audioRendererChange'`：音频渲染器更改事件。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioStreamManager.off('audioRendererChange');
console.info('######### RendererChange Off is called #########');
```

### on('audioCapturerChange')

on(type: 'audioCapturerChange', callback: Callback&lt;AudioCapturerChangeInfoArray&gt;): void

监听音频采集器更改事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 事件类型，支持的事件`'audioCapturerChange'`：当音频采集器发生更改时触发。 |
| callback | Callback<[AudioCapturerChangeInfoArray](#audiocapturerchangeinfoarray)> | 是   | 回调函数，返回当前音频采集器信息。                           |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioStreamManager.on('audioCapturerChange', (AudioCapturerChangeInfoArray: audio.AudioCapturerChangeInfoArray) =>  {
  for (let i = 0; i < AudioCapturerChangeInfoArray.length; i++) {
    console.info(`## CapChange on is called for element ${i} ##`);
    console.info(`StreamId for ${i} is: ${AudioCapturerChangeInfoArray[i].streamId}`);
    console.info(`Source for ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.source}`);
    console.info(`Flag  ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.capturerFlags}`);
    for (let j = 0; j < AudioCapturerChangeInfoArray[i].deviceDescriptors.length; j++) {
      console.info(`Id: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].id}`);
      console.info(`Type: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceType}`);
      console.info(`Role: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceRole}`);
      console.info(`Name: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].name}`);
      console.info(`Address: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].address}`);
      console.info(`SampleRate: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].sampleRates[0]}`);
      console.info(`ChannelCount: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelCounts[0]}`);
      console.info(`ChannelMask: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelMasks[0]}`);
    }
  }
});
```

### off('audioCapturerChange')

off(type: 'audioCapturerChange'): void

取消监听音频采集器更改事件。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| type   | string | 是   | 事件类型，支持的事件`'audioCapturerChange'`：音频采集器更改事件。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioStreamManager.off('audioCapturerChange');
console.info('######### CapturerChange Off is called #########');

```

### isActive

isActive(volumeType: AudioVolumeType, callback: AsyncCallback&lt;boolean&gt;): void

获取指定音频流是否为活跃状态，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明                                                         |
| ---------- | ----------------------------------- | ---- | ------------------------------------------------------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音频流类型。                                                 |
| callback   | AsyncCallback&lt;boolean&gt;        | 是   | 回调函数。当获取指定音频流是否为活跃状态成功，err为undefined，data为true为活跃，false为不活跃；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioStreamManager.isActive(audio.AudioVolumeType.MEDIA, (err: BusinessError, value: boolean) => {
if (err) {
  console.error(`Failed to obtain the active status of the stream. ${err}`);
  return;
}
  console.info(`Callback invoked to indicate that the active status of the stream is obtained ${value}.`);
});
```

### isActive

isActive(volumeType: AudioVolumeType): Promise&lt;boolean&gt;

获取指定音频流是否为活跃状态，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音频流类型。 |

**返回值：**

| 类型                   | 说明                                                       |
| ---------------------- | ---------------------------------------------------------- |
| Promise&lt;boolean&gt; | Promise对象，返回流的活跃状态，true为活跃，false为不活跃。 |

**示例：**

```ts
audioStreamManager.isActive(audio.AudioVolumeType.MEDIA).then((value: boolean) => {
  console.info(`Promise returned to indicate that the active status of the stream is obtained ${value}.`);
});
```

### isActiveSync

isActiveSync(volumeType: AudioVolumeType): boolean

获取指定音频流是否为活跃状态，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                | 必填 | 说明         |
| ---------- | ----------------------------------- | ---- | ------------ |
| volumeType | [AudioVolumeType](#audiovolumetype) | 是   | 音频流类型。 |

**返回值：**

| 类型    | 说明                                          |
| ------- | --------------------------------------------- |
| boolean | 返回流的活跃状态，true为活跃，false为不活跃。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: boolean = audioStreamManager.isActiveSync(audio.AudioVolumeType.MEDIA);
  console.info(`Indicate that the active status of the stream is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the active status of the stream ${error}.`);
}
```

## AudioRoutingManager

音频路由管理。在使用AudioRoutingManager的接口前，需要使用[getRoutingManager](#getroutingmanager)获取AudioRoutingManager实例。

### getDevices

getDevices(deviceFlag: DeviceFlag, callback: AsyncCallback&lt;AudioDeviceDescriptors&gt;): void

获取音频设备列表，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                                         | 必填 | 说明                                                         |
| ---------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| deviceFlag | [DeviceFlag](#deviceflag)                                    | 是   | 设备类型的flag。                                             |
| callback   | AsyncCallback&lt;[AudioDeviceDescriptors](#audiodevicedescriptors)&gt; | 是   | 回调函数。当获取音频设备列表成功，err为undefined，data为获取到的音频设备列表；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRoutingManager.getDevices(audio.DeviceFlag.OUTPUT_DEVICES_FLAG, (err: BusinessError, value: audio.AudioDeviceDescriptors) => {
  if (err) {
    console.error(`Failed to obtain the device list. ${err}`);
    return;
  }
  console.info('Callback invoked to indicate that the device list is obtained.');
});
```

### getDevices

getDevices(deviceFlag: DeviceFlag): Promise&lt;AudioDeviceDescriptors&gt;

获取音频设备列表，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                      | 必填 | 说明             |
| ---------- | ------------------------- | ---- | ---------------- |
| deviceFlag | [DeviceFlag](#deviceflag) | 是   | 设备类型的flag。 |

**返回值：**

| 类型                                                         | 说明                        |
| ------------------------------------------------------------ | --------------------------- |
| Promise&lt;[AudioDeviceDescriptors](#audiodevicedescriptors)&gt; | Promise对象，返回设备列表。 |

**示例：**

```ts
audioRoutingManager.getDevices(audio.DeviceFlag.OUTPUT_DEVICES_FLAG).then((data: audio.AudioDeviceDescriptors) => {
  console.info('Promise returned to indicate that the device list is obtained.');
});
```

### getDevicesSync

getDevicesSync(deviceFlag: DeviceFlag): AudioDeviceDescriptors

获取音频设备列表，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                      | 必填 | 说明             |
| ---------- | ------------------------- | ---- | ---------------- |
| deviceFlag | [DeviceFlag](#deviceflag) | 是   | 设备类型的flag。 |

**返回值：**

| 类型                                              | 说明           |
| ------------------------------------------------- | -------------- |
| [AudioDeviceDescriptors](#audiodevicedescriptors) | 返回设备列表。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let data: audio.AudioDeviceDescriptors = audioRoutingManager.getDevicesSync(audio.DeviceFlag.OUTPUT_DEVICES_FLAG);
  console.info(`Indicate that the device list is obtained ${data}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the device list. ${error}`);
}
```

### on('deviceChange')

on(type: 'deviceChange', deviceFlag: DeviceFlag, callback: Callback<DeviceChangeAction\>): void

设备更改。音频设备连接状态变化，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android

**参数：**

| 参数名     | 类型                                                 | 必填 | 说明                                       |
| :--------- | :--------------------------------------------------- | :--- | :----------------------------------------- |
| type       | string                                               | 是   | 订阅的事件的类型。支持事件：'deviceChange' |
| deviceFlag | [DeviceFlag](#deviceflag)                            | 是   | 设备类型的flag。                           |
| callback   | Callback<[DeviceChangeAction](#devicechangeaction)\> | 是   | 回调函数，返回设备更新详情。               |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioRoutingManager.on('deviceChange', audio.DeviceFlag.OUTPUT_DEVICES_FLAG, (deviceChanged: audio.DeviceChangeAction) => {
  console.info('device change type : ' + deviceChanged.type);
  console.info('device descriptor size : ' + deviceChanged.deviceDescriptors.length);
  console.info('device change descriptor : ' + deviceChanged.deviceDescriptors[0].deviceRole);
  console.info('device change descriptor : ' + deviceChanged.deviceDescriptors[0].deviceType);
});
```

### off('deviceChange')

off(type: 'deviceChange', callback?: Callback<DeviceChangeAction\>): void

取消订阅音频设备连接变化事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android

**参数：**

| 参数名   | 类型                                                | 必填 | 说明                                       |
| -------- | --------------------------------------------------- | ---- | ------------------------------------------ |
| type     | string                                              | 是   | 订阅的事件的类型。支持事件：'deviceChange' |
| callback | Callback<[DeviceChangeAction](#devicechangeaction)> | 否   | 回调函数，返回设备更新详情。               |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioRoutingManager.off('deviceChange');
```

### setCommunicationDevice

setCommunicationDevice(deviceType: CommunicationDeviceType, active: boolean, callback: AsyncCallback&lt;void&gt;): void

设置通信设备激活状态，使用callback方式异步返回结果。

该接口由于功能设计变化，将在后续版本废弃，不建议开发者使用。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                                | 必填 | 说明                                                         |
| ---------- | --------------------------------------------------- | ---- | ------------------------------------------------------------ |
| deviceType | [CommunicationDeviceType](#communicationdevicetype) | 是   | 音频设备类型。                                               |
| active     | boolean                                             | 是   | 设备激活状态，true激活，false未激活。                        |
| callback   | AsyncCallback&lt;void&gt;                           | 是   | 回调函数。当设置通信设备激活状态成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRoutingManager.setCommunicationDevice(audio.CommunicationDeviceType.SPEAKER, true, (err: BusinessError) => {
  if (err) {
    console.error(`Failed to set the active status of the device. ${err}`);
    return;
  }
  console.info('Callback invoked to indicate that the device is set to the active status.');
});
```

### setCommunicationDevice

setCommunicationDevice(deviceType: CommunicationDeviceType, active: boolean): Promise&lt;void&gt;

设置通信设备激活状态，使用Promise方式异步返回结果。

该接口由于功能设计变化，将在后续版本废弃，不建议开发者使用。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                                | 必填 | 说明                                  |
| ---------- | --------------------------------------------------- | ---- | ------------------------------------- |
| deviceType | [CommunicationDeviceType](#communicationdevicetype) | 是   | 活跃音频设备类型。                    |
| active     | boolean                                             | 是   | 设备激活状态，true激活，false未激活。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise对象，无返回结果。 |

**示例：**

```ts
audioRoutingManager.setCommunicationDevice(audio.CommunicationDeviceType.SPEAKER, true).then(() => {
  console.info('Promise returned to indicate that the device is set to the active status.');
});
```

### isCommunicationDeviceActive

isCommunicationDeviceActive(deviceType: CommunicationDeviceType, callback: AsyncCallback&lt;boolean&gt;): void

获取指定通信设备的激活状态，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                                | 必填 | 说明                                                         |
| ---------- | --------------------------------------------------- | ---- | ------------------------------------------------------------ |
| deviceType | [CommunicationDeviceType](#communicationdevicetype) | 是   | 活跃音频设备类型。                                           |
| callback   | AsyncCallback&lt;boolean&gt;                        | 是   | 回调函数。当获取指定通信设备的激活状态成功，err为undefined，data为true为激活，false为未激活；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRoutingManager.isCommunicationDeviceActive(audio.CommunicationDeviceType.SPEAKER, (err: BusinessError, value: boolean) => {
  if (err) {
    console.error(`Failed to obtain the active status of the device. ${err}`);
    return;
  }
  console.info('Callback invoked to indicate that the active status of the device is obtained.');
});
```

### isCommunicationDeviceActive

isCommunicationDeviceActive(deviceType: CommunicationDeviceType): Promise&lt;boolean&gt;

获取指定通信设备的激活状态，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                                | 必填 | 说明               |
| ---------- | --------------------------------------------------- | ---- | ------------------ |
| deviceType | [CommunicationDeviceType](#communicationdevicetype) | 是   | 活跃音频设备类型。 |

**返回值：**

| Type                   | Description                                              |
| ---------------------- | -------------------------------------------------------- |
| Promise&lt;boolean&gt; | Promise对象，返回设备的激活状态，true激活，false未激活。 |

**示例：**

```ts
audioRoutingManager.isCommunicationDeviceActive(audio.CommunicationDeviceType.SPEAKER).then((value: boolean) => {
  console.info(`Promise returned to indicate that the active status of the device is obtained ${value}.`);
});
```

### isCommunicationDeviceActiveSync

isCommunicationDeviceActiveSync(deviceType: CommunicationDeviceType): boolean

获取指定通信设备的激活状态，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Communication

**支持平台：** Android、iOS

**参数：**

| 参数名     | 类型                                                | 必填 | 说明               |
| ---------- | --------------------------------------------------- | ---- | ------------------ |
| deviceType | [CommunicationDeviceType](#communicationdevicetype) | 是   | 活跃音频设备类型。 |

**返回值：**

| Type    | Description                                 |
| ------- | ------------------------------------------- |
| boolean | 返回设备的激活状态，true激活，false未激活。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: boolean = audioRoutingManager.isCommunicationDeviceActiveSync(audio.CommunicationDeviceType.SPEAKER);
  console.info(`Indicate that the active status of the device is obtained ${value}.`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Failed to obtain the active status of the device ${error}.`);
}
```

### getPreferOutputDeviceForRendererInfo

getPreferOutputDeviceForRendererInfo(rendererInfo: AudioRendererInfo, callback: AsyncCallback&lt;AudioDeviceDescriptors&gt;): void

根据音频信息，返回优先级最高的输出设备，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                                         | 必填 | 说明                                                         |
| ------------ | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| rendererInfo | [AudioRendererInfo](#audiorendererinfo)                      | 是   | 表示渲染器信息。                                             |
| callback     | AsyncCallback&lt;[AudioDeviceDescriptors](#audiodevicedescriptors)&gt; | 是   | 回调函数。当获取优先级最高的输出设备成功，err为undefined，data为获取到的优先级最高的输出设备信息；否则为错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                         |
| -------- | ------------------------------------------------ |
| 6800101  | Input parameter value error. Return by callback. |
| 6800301  | System error. Return by callback.                |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let rendererInfo: audio.AudioRendererInfo = {
  // 使用STREAM_USAGE_MUSIC占位，但在iOS平台未使用  
  usage : audio.StreamUsage.STREAM_USAGE_MUSIC,
  rendererFlags : 0
}
async function getPreferOutputDevice() {
  audioRoutingManager.getPreferOutputDeviceForRendererInfo(rendererInfo, (err: BusinessError, desc: audio.AudioDeviceDescriptors) => {
    if (err) {
      console.error(`Result ERROR: ${err}`);
    } else {
      console.info(`device descriptor: ${desc}`);
    }
  });
}
```

### getPreferOutputDeviceForRendererInfo

getPreferOutputDeviceForRendererInfo(rendererInfo: AudioRendererInfo): Promise&lt;AudioDeviceDescriptors&gt;

根据音频信息，返回优先级最高的输出设备，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                    | 必填 | 说明             |
| ------------ | --------------------------------------- | ---- | ---------------- |
| rendererInfo | [AudioRendererInfo](#audiorendererinfo) | 是   | 表示渲染器信息。 |

**返回值：**

| 类型                                                         | 说明                                        |
| ------------------------------------------------------------ | ------------------------------------------- |
| Promise&lt;[AudioDeviceDescriptors](#audiodevicedescriptors)&gt; | Promise对象，返回优先级最高的输出设备信息。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                        |
| -------- | ----------------------------------------------- |
| 6800101  | Input parameter value error. Return by promise. |
| 6800301  | System error. Return by promise.                |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let rendererInfo: audio.AudioRendererInfo = {
  // 在iOS平台使用STREAM_USAGE_MUSIC占位，实际未使用  
  usage : audio.StreamUsage.STREAM_USAGE_MUSIC,
  rendererFlags : 0
}

async function getPreferOutputDevice() {
  audioRoutingManager.getPreferOutputDeviceForRendererInfo(rendererInfo).then((desc: audio.AudioDeviceDescriptors) => {
    console.info(`device descriptor: ${desc}`);
  }).catch((err: BusinessError) => {
    console.error(`Result ERROR: ${err}`);
  })
}
```

### getPreferredOutputDeviceForRendererInfoSync

getPreferredOutputDeviceForRendererInfoSync(rendererInfo: AudioRendererInfo): AudioDeviceDescriptors

根据音频信息，返回优先级最高的输出设备，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                    | 必填 | 说明             |
| ------------ | --------------------------------------- | ---- | ---------------- |
| rendererInfo | [AudioRendererInfo](#audiorendererinfo) | 是   | 表示渲染器信息。 |

**返回值：**

| 类型                                              | 说明                           |
| ------------------------------------------------- | ------------------------------ |
| [AudioDeviceDescriptors](#audiodevicedescriptors) | 返回优先级最高的输出设备信息。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let rendererInfo: audio.AudioRendererInfo = {
  // 在iOS平台使用STREAM_USAGE_MUSIC占位，实际未使用  
  usage : audio.StreamUsage.STREAM_USAGE_MUSIC,
  rendererFlags : 0
}

try {
  let desc: audio.AudioDeviceDescriptors = audioRoutingManager.getPreferredOutputDeviceForRendererInfoSync(rendererInfo);
  console.info(`device descriptor: ${desc}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Result ERROR: ${error}`);
}
```

### on('preferOutputDeviceChangeForRendererInfo')

on(type: 'preferOutputDeviceChangeForRendererInfo', rendererInfo: AudioRendererInfo, callback: Callback<AudioDeviceDescriptors\>): void

订阅最高优先级输出设备变化事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                                         | 必填 | 说明                                                         |
| :----------- | :----------------------------------------------------------- | :--- | :----------------------------------------------------------- |
| type         | string                                                       | 是   | 订阅的事件的类型。支持事件：'preferOutputDeviceChangeForRendererInfo' |
| rendererInfo | [AudioRendererInfo](#audiorendererinfo)                      | 是   | 表示渲染器信息。                                             |
| callback     | Callback<[AudioDeviceDescriptors](#audiodevicedescriptors)\> | 是   | 回调函数，返回优先级最高的输出设备信息。                     |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let rendererInfo: audio.AudioRendererInfo = {
  // 在iOS平台使用STREAM_USAGE_MUSIC占位，实际未使用  
  usage : audio.StreamUsage.STREAM_USAGE_MUSIC,
  rendererFlags : 0
}

audioRoutingManager.on('preferOutputDeviceChangeForRendererInfo', rendererInfo, (desc: audio.AudioDeviceDescriptors) => {
  console.info(`device descriptor: ${desc}`);
});
```

### off('preferOutputDeviceChangeForRendererInfo')

off(type: 'preferOutputDeviceChangeForRendererInfo', callback?: Callback<AudioDeviceDescriptors\>): void

取消订阅最高优先级输出音频设备变化事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名   | 类型                                                        | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                                      | 是   | 订阅的事件的类型。支持事件：'preferOutputDeviceChangeForRendererInfo' |
| callback | Callback<[AudioDeviceDescriptors](#audiodevicedescriptors)> | 否   | 回调函数，返回优先级最高的输出设备信息。                     |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
audioRoutingManager.off('preferOutputDeviceChangeForRendererInfo');
```

### getPreferredInputDeviceForCapturerInfo

getPreferredInputDeviceForCapturerInfo(capturerInfo: AudioCapturerInfo, callback: AsyncCallback&lt;AudioDeviceDescriptors&gt;): void

根据音频信息，返回优先级最高的输入设备，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                                         | 必填 | 说明                                                         |
| ------------ | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| capturerInfo | [AudioCapturerInfo](#audiocapturerinfo)                      | 是   | 表示采集器信息。                                             |
| callback     | AsyncCallback&lt;[AudioDeviceDescriptors](#audiodevicedescriptors)&gt; | 是   | 回调函数。当获取优先级最高的输入设备成功，err为undefined，data为获取到的优先级最高的输入设备信息；否则为错误对象。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                     |
| -------- | -------------------------------------------- |
| 6800101  | Invalid parameter error. Return by callback. |
| 6800301  | System error. Return by callback.            |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let capturerInfo: audio.AudioCapturerInfo = {
  source: audio.SourceType.SOURCE_TYPE_MIC,
  capturerFlags: 0
}

audioRoutingManager.getPreferredInputDeviceForCapturerInfo(capturerInfo, (err: BusinessError, desc: audio.AudioDeviceDescriptors) => {
  if (err) {
    console.error(`Result ERROR: ${err}`);
  } else {
    console.info(`device descriptor: ${desc}`);
  }
});
```

### getPreferredInputDeviceForCapturerInfo

getPreferredInputDeviceForCapturerInfo(capturerInfo: AudioCapturerInfo): Promise&lt;AudioDeviceDescriptors&gt;

根据音频信息，返回优先级最高的输入设备，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                    | 必填 | 说明             |
| ------------ | --------------------------------------- | ---- | ---------------- |
| capturerInfo | [AudioCapturerInfo](#audiocapturerinfo) | 是   | 表示采集器信息。 |

**返回值：**

| 类型                                                         | 说明                                        |
| ------------------------------------------------------------ | ------------------------------------------- |
| Promise&lt;[AudioDeviceDescriptors](#audiodevicedescriptors)&gt; | Promise对象，返回优先级最高的输入设备信息。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                    |
| -------- | ------------------------------------------- |
| 6800101  | Invalid parameter error. Return by promise. |
| 6800301  | System error. Return by promise.            |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let capturerInfo: audio.AudioCapturerInfo = {
  source: audio.SourceType.SOURCE_TYPE_MIC,
  capturerFlags: 0
}

audioRoutingManager.getPreferredInputDeviceForCapturerInfo(capturerInfo).then((desc: audio.AudioDeviceDescriptors) => {
  console.info(`device descriptor: ${desc}`);
}).catch((err: BusinessError) => {
  console.error(`Result ERROR: ${err}`);
});
```

### getPreferredInputDeviceForCapturerInfoSync

getPreferredInputDeviceForCapturerInfoSync(capturerInfo: AudioCapturerInfo): AudioDeviceDescriptors

根据音频信息，返回优先级最高的输入设备，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                    | 必填 | 说明             |
| ------------ | --------------------------------------- | ---- | ---------------- |
| capturerInfo | [AudioCapturerInfo](#audiocapturerinfo) | 是   | 表示采集器信息。 |

**返回值：**

| 类型                                              | 说明                           |
| ------------------------------------------------- | ------------------------------ |
| [AudioDeviceDescriptors](#audiodevicedescriptors) | 返回优先级最高的输入设备信息。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';
import { BusinessError } from '@ohos.base';

let capturerInfo: audio.AudioCapturerInfo = {
  source: audio.SourceType.SOURCE_TYPE_MIC,
  capturerFlags: 0
}

try {
  let desc: audio.AudioDeviceDescriptors = audioRoutingManager.getPreferredInputDeviceForCapturerInfoSync(capturerInfo);
  console.info(`device descriptor: ${desc}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Result ERROR: ${error}`);
}
```

### on('preferredInputDeviceChangeForCapturerInfo')

on(type: 'preferredInputDeviceChangeForCapturerInfo', capturerInfo: AudioCapturerInfo, callback: Callback<AudioDeviceDescriptors\>): void

订阅最高优先级输入设备变化事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名       | 类型                                                         | 必填 | 说明                                                         |
| :----------- | :----------------------------------------------------------- | :--- | :----------------------------------------------------------- |
| type         | string                                                       | 是   | 订阅的事件的类型。支持事件：'preferredInputDeviceChangeForCapturerInfo' |
| capturerInfo | [AudioCapturerInfo](#audiocapturerinfo)                      | 是   | 表示采集器信息。                                             |
| callback     | Callback<[AudioDeviceDescriptors](#audiodevicedescriptors)\> | 是   | 回调函数，返回优先级最高的输入设备信息。                     |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let capturerInfo: audio.AudioCapturerInfo = {
  source: audio.SourceType.SOURCE_TYPE_MIC,
  capturerFlags: 0
}

audioRoutingManager.on('preferredInputDeviceChangeForCapturerInfo', capturerInfo, (desc: audio.AudioDeviceDescriptors) => {
  console.info(`device descriptor: ${desc}`);
});
```

### off('preferredInputDeviceChangeForCapturerInfo')

off(type: 'preferredInputDeviceChangeForCapturerInfo', callback?: Callback<AudioDeviceDescriptors\>): void

取消订阅最高优先级输入音频设备变化事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名   | 类型                                                        | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                                      | 是   | 订阅的事件的类型。支持事件：'preferredInputDeviceChangeForCapturerInfo' |
| callback | Callback<[AudioDeviceDescriptors](#audiodevicedescriptors)> | 否   | 回调函数，返回优先级最高的输入设备信息。                     |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioRoutingManager.off('preferredInputDeviceChangeForCapturerInfo');
```

## AudioRendererChangeInfoArray

数组类型，AudioRenderChangeInfo数组，只读。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

## AudioRendererChangeInfo

描述音频渲染器更改信息。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

| 名称              | 类型                                              | 可读 | 可写 | 说明             | Android平台 | iOS平台 |
| ----------------- | ------------------------------------------------- | ---- | ---- | ---------------- | ----------- | ------- |
| streamId          | number                                            | 是   | 否   | 音频流唯一id。   | 不支持      | 支持    |
| rendererInfo      | [AudioRendererInfo](#audiorendererinfo)           | 是   | 否   | 音频渲染器信息。 | 支持        | 支持    |
| deviceDescriptors | [AudioDeviceDescriptors](#audiodevicedescriptors) | 是   | 否   | 音频设备描述。   | 支持        | 支持    |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

const audioManager = audio.getAudioManager();
let audioStreamManager = audioManager.getStreamManager();

audioStreamManager.on('audioRendererChange',  (AudioRendererChangeInfoArray) => {
  for (let i = 0; i < AudioRendererChangeInfoArray.length; i++) {
    console.info(`## RendererChange on is called for ${i} ##`);
    console.info(`StreamId for ${i} is: ${AudioRendererChangeInfoArray[i].streamId}`);
    console.info(`Stream for ${i} is: ${AudioRendererChangeInfoArray[i].rendererInfo.usage}`);
    console.info(`Flag ${i} is: ${AudioRendererChangeInfoArray[i].rendererInfo.rendererFlags}`);
    let devDescriptor = AudioRendererChangeInfoArray[i].deviceDescriptors;
    for (let j = 0; j < AudioRendererChangeInfoArray[i].deviceDescriptors.length; j++) {
      console.info(`Id: ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].id}`);
      console.info(`Type: ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].deviceType}`);
      console.info(`Role: ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].deviceRole}`);
      console.info(`Name: ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].name}`);
      console.info(`Addr: ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].address}`);
      console.info(`SR: ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].sampleRates[0]}`);
      console.info(`C ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].channelCounts[0]}`);
      console.info(`CM: ${i} : ${AudioRendererChangeInfoArray[i].deviceDescriptors[j].channelMasks[0]}`);
    }
  }
});
```


## AudioCapturerChangeInfoArray

数组类型，AudioCapturerChangeInfo数组，只读。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

## AudioCapturerChangeInfo

描述音频采集器更改信息。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

| 名称              | 类型                                              | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ----------------- | ------------------------------------------------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| streamId          | number                                            | 是   | 否   | 音频流唯一id。                                               | 支持        | 支持    |
| capturerInfo      | [AudioCapturerInfo](#audiocapturerinfo)           | 是   | 否   | 音频采集器信息。                                             | 支持        | 不支持  |
| deviceDescriptors | [AudioDeviceDescriptors](#audiodevicedescriptors) | 是   | 否   | 音频设备描述。                                               | 支持        | 支持    |
| muted             | boolean                                           | 是   | 否   | 音频采集器静音状态。true表示音频采集器为静音状态，false表示音频采集器为非静音状态。 | 支持        | 不支持  |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

const audioManager = audio.getAudioManager();
let audioStreamManager = audioManager.getStreamManager();

audioStreamManager.on('audioCapturerChange', (AudioCapturerChangeInfoArray) =>  {
  for (let i = 0; i < AudioCapturerChangeInfoArray.length; i++) {
    console.info(`## CapChange on is called for element ${i} ##`);
    console.info(`StrId for  ${i} is: ${AudioCapturerChangeInfoArray[i].streamId}`);
    console.info(`Src for ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.source}`);
    console.info(`Flag ${i} is: ${AudioCapturerChangeInfoArray[i].capturerInfo.capturerFlags}`);
    let devDescriptor = AudioCapturerChangeInfoArray[i].deviceDescriptors;
    for (let j = 0; j < AudioCapturerChangeInfoArray[i].deviceDescriptors.length; j++) {
      console.info(`Id: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].id}`);
      console.info(`Type: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceType}`);
      console.info(`Role: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].deviceRole}`);
      console.info(`Name: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].name}`);
      console.info(`Addr: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].address}`);
      console.info(`SR: ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].sampleRates[0]}`);
      console.info(`C ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelCounts[0]}`);
      console.info(`CM ${i} : ${AudioCapturerChangeInfoArray[i].deviceDescriptors[j].channelMasks[0]}`);
    }
  }
});
```

## AudioDeviceDescriptors

设备属性数组类型，为[AudioDeviceDescriptor](#audiodevicedescriptor)的数组，只读。

## AudioDeviceDescriptor

描述音频设备。

| 名称          | 类型                                                 | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------------- | ---------------------------------------------------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| deviceRole    | [DeviceRole](#devicerole)                            | 是   | 否   | 设备角色。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 支持    |
| deviceType    | [DeviceType](#devicetype)                            | 是   | 否   | 设备类型。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 支持    |
| id            | number                                               | 是   | 否   | 设备id，唯一。  <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 不支持  |
| name          | string                                               | 是   | 否   | 设备名称。<br>如果是蓝牙设备，需要申请权限ohos.permission.USE_BLUETOOTH。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 支持    |
| address       | string                                               | 是   | 否   | 设备地址。<br>如果是蓝牙设备，需要申请权限ohos.permission.USE_BLUETOOTH。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 不支持  |
| sampleRates   | Array&lt;number&gt;                                  | 是   | 否   | 支持的采样率。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 支持    |
| channelCounts | Array&lt;number&gt;                                  | 是   | 否   | 支持的通道数。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 不支持  |
| channelMasks  | Array&lt;number&gt;                                  | 是   | 否   | 支持的通道掩码。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 支持        | 支持    |
| displayName   | string                                               | 是   | 否   | 设备显示名。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Device | 不支持      | 支持    |
| encodingTypes | Array&lt;[AudioEncodingType](#audioencodingtype)&gt; | 是   | 否   | 支持的编码类型。 <br> **系统能力：** SystemCapability.Multimedia.Audio.Core | 支持        | 支持    |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

function displayDeviceProp(value: audio.AudioDeviceDescriptor) {
  deviceRoleValue = value.deviceRole;
  deviceTypeValue = value.deviceType;
}

let deviceRoleValue: audio.DeviceRole | undefined = undefined;;
let deviceTypeValue: audio.DeviceType | undefined = undefined;;
audio.getAudioManager().getDevices(1).then((value: audio.AudioDeviceDescriptors) => {
  console.info('AudioFrameworkTest: Promise: getDevices OUTPUT_DEVICES_FLAG');
  value.forEach(displayDeviceProp);
  if (deviceTypeValue != undefined && deviceRoleValue != undefined){
    console.info('AudioFrameworkTest: Promise: getDevices : OUTPUT_DEVICES_FLAG :  PASS');
  } else {
    console.error('AudioFrameworkTest: Promise: getDevices : OUTPUT_DEVICES_FLAG :  FAIL');
  }
});
```

## AudioRenderer

提供音频渲染的相关接口。在调用AudioRenderer的接口前，需要先通过[createAudioRenderer](#audiocreateaudiorenderer)创建实例。

### 属性

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

| 名称  | 类型                      | 可读 | 可写 | 说明               | Android平台 | iOS平台 |
| ----- | ------------------------- | ---- | ---- | ------------------ | ----------- | ------- |
| state | [AudioState](#audiostate) | 是   | 否   | 音频渲染器的状态。 | 支持        | 支持    |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let state: audio.AudioState = audioRenderer.state;
```

### **AudioDataCallbackResult**

枚举值，定义音频数据回调的结果。

**系统能力：** SystemCapability.Multimedia.Audio.Core

| 名称    | 值   | 说明                 | Android平台 | iOS平台 |
| ------- | ---- | -------------------- | ----------- | ------- |
| INVALID | -1   | 表示该回调数据无效。 | 支持        | 支持    |
| VALID   | 0    | 表示该回调数据有效。 | 支持        | 支持    |

### **AudioRendererWriteDataCallback**

type AudioRendererWriteDataCallback = (data: ArrayBuffer) => AudioDataCallbackResult | void

回调函数类型，用于音频渲染器的数据写入。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型        | 必填 | 说明                 |
| ------ | ----------- | ---- | -------------------- |
| data   | ArrayBuffer | 是   | 待写入缓冲区的数据。 |

**返回值：**

| 类型                           | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| AudioDataCallbackResult\| void | 如果返回 void 或 AudioDataCallbackResult.VALID ，表示数据有效并将被播放；如果返回 AudioDataCallbackResult.INVALID ，表示数据无效并将不会被播放。 |

### getRendererInfo

getRendererInfo(callback: AsyncCallback<AudioRendererInfo\>): void

获取当前被创建的音频渲染器的信息，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                    | 必填 | 说明                                                         |
| :------- | :------------------------------------------------------ | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<[AudioRendererInfo](#audiorendererinfo)\> | 是   | 回调函数。当获取音频渲染器的信息成功，err为undefined，data为获取到的音频渲染器的信息；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getRendererInfo((err: BusinessError, rendererInfo: audio.AudioRendererInfo) => {
  console.info('Renderer GetRendererInfo:');
  console.info(`Renderer usage: ${rendererInfo.usage}`);
  console.info(`Renderer flags: ${rendererInfo.rendererFlags}`);
});
```

### getRendererInfo

getRendererInfo(): Promise<AudioRendererInfo\>

获取当前被创建的音频渲染器的信息，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                                              | 说明                              |
| ------------------------------------------------- | --------------------------------- |
| Promise<[AudioRendererInfo](#audiorendererinfo)\> | Promise对象，返回音频渲染器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getRendererInfo().then((rendererInfo: audio.AudioRendererInfo) => {
  console.info('Renderer GetRendererInfo:');
  console.info(`Renderer usage: ${rendererInfo.usage}`);
  console.info(`Renderer flags: ${rendererInfo.rendererFlags}`)
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRenderLog: RendererInfo :ERROR: ${err}`);
});
```

### getRendererInfoSync

getRendererInfoSync(): AudioRendererInfo

获取当前被创建的音频渲染器的信息，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                                    | 说明                 |
| --------------------------------------- | -------------------- |
| [AudioRendererInfo](#audiorendererinfo) | 返回音频渲染器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let rendererInfo: audio.AudioRendererInfo = audioRenderer.getRendererInfoSync();
  console.info(`Renderer usage: ${rendererInfo.usage}`);
  console.info(`Renderer flags: ${rendererInfo.rendererFlags}`)
} catch (err) {
  let error = err as BusinessError;
  console.error(`AudioFrameworkRenderLog: RendererInfo :ERROR: ${error}`);
}
```

### getStreamInfo

getStreamInfo(callback: AsyncCallback<AudioStreamInfo\>): void

获取音频流信息，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                | 必填 | 说明                                                         |
| :------- | :-------------------------------------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<[AudioStreamInfo](#audiostreaminfo)\> | 是   | 回调函数。当获取音频流信息成功，err为undefined，data为获取到的音频流信息；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getStreamInfo((err: BusinessError, streamInfo: audio.AudioStreamInfo) => {
  console.info('Renderer GetStreamInfo:');
  console.info(`Renderer sampling rate: ${streamInfo.samplingRate}`);
  console.info(`Renderer channel: ${streamInfo.channels}`);
  console.info(`Renderer format: ${streamInfo.sampleFormat}`);
  console.info(`Renderer encoding type: ${streamInfo.encodingType}`);
});
```

### getStreamInfo

getStreamInfo(): Promise<AudioStreamInfo\>

获取音频流信息，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                                          | 说明                         |
| :-------------------------------------------- | :--------------------------- |
| Promise<[AudioStreamInfo](#audiostreaminfo)\> | Promise对象，返回音频流信息. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getStreamInfo().then((streamInfo: audio.AudioStreamInfo) => {
  console.info('Renderer GetStreamInfo:');
  console.info(`Renderer sampling rate: ${streamInfo.samplingRate}`);
  console.info(`Renderer channel: ${streamInfo.channels}`);
  console.info(`Renderer format: ${streamInfo.sampleFormat}`);
  console.info(`Renderer encoding type: ${streamInfo.encodingType}`);
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### getStreamInfoSync

getStreamInfoSync(): AudioStreamInfo

获取音频流信息，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明            |
| :---------------------------------- | :-------------- |
| [AudioStreamInfo](#audiostreaminfo) | 返回音频流信息. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let streamInfo: audio.AudioStreamInfo = audioRenderer.getStreamInfoSync();
  console.info(`Renderer sampling rate: ${streamInfo.samplingRate}`);
  console.info(`Renderer channel: ${streamInfo.channels}`);
  console.info(`Renderer format: ${streamInfo.sampleFormat}`);
  console.info(`Renderer encoding type: ${streamInfo.encodingType}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`ERROR: ${error}`);
}
```

### getAudioStreamId

getAudioStreamId(callback: AsyncCallback<number\>): void

获取音频流id，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| :------- | :--------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<number\> | 是   | 回调函数。当获取音频流id成功，err为undefined，data为获取到的音频流id；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getAudioStreamId((err: BusinessError, streamId: number) => {
  console.info(`Renderer GetStreamId: ${streamId}`);
});
```

### getAudioStreamId

getAudioStreamId(): Promise<number\>

获取音频流id，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型             | 说明                        |
| :--------------- | :-------------------------- |
| Promise<number\> | Promise对象，返回音频流id。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getAudioStreamId().then((streamId: number) => {
  console.info(`Renderer getAudioStreamId: ${streamId}`);
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### getAudioStreamIdSync

getAudioStreamIdSync(): number

获取音频流id，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明           |
| :----- | :------------- |
| number | 返回音频流id。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let streamId: number = audioRenderer.getAudioStreamIdSync();
  console.info(`Renderer getAudioStreamIdSync: ${streamId}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`ERROR: ${error}`);
}
```

### start

start(callback: AsyncCallback<void\>): void

启动音频渲染器。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<void> | 是   | 回调函数。当启动音频渲染器成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.start((err: BusinessError) => {
  if (err) {
    console.error('Renderer start failed.');
  } else {
    console.info('Renderer start success.');
  }
});
```

### start

start(): Promise<void\>

启动音频渲染器。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.start().then(() => {
  console.info('Renderer started');
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### pause

pause(callback: AsyncCallback\<void>): void

暂停渲染。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                       |
| -------- | -------------------- | ---- | ---------------------------------------------------------- |
| callback | AsyncCallback\<void> | 是   | 回调函数。当暂停渲染成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.pause((err: BusinessError) => {
  if (err) {
    console.error('Renderer pause failed');
  } else {
    console.info('Renderer paused.');
  }
});
```

### pause

pause(): Promise\<void>

暂停渲染。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.pause().then(() => {
  console.info('Renderer paused');
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### drain

drain(callback: AsyncCallback\<void>): void

检查缓冲区是否已被耗尽。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<void> | 是   | 回调函数。当检查缓冲区是否已被耗尽成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.drain((err: BusinessError) => {
  if (err) {
    console.error('Renderer drain failed');
  } else {
    console.info('Renderer drained.');
  }
});
```

### drain

drain(): Promise\<void>

检查缓冲区是否已被耗尽。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.drain().then(() => {
  console.info('Renderer drained successfully');
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### flush

flush(): Promise\<void>

清空缓冲区（[AudioState](#audiostate)为STATE_RUNNING、STATE_PAUSED、STATE_STOPPED状态下可用）。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | Promise对象，无返回结果。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                                  |
| -------- | --------------------------------------------------------- |
| 6800103  | Operation not permit at current state. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.flush().then(() => {
  console.info('Renderer flushed successfully');
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### stop

stop(callback: AsyncCallback\<void>): void

停止渲染。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                       |
| -------- | -------------------- | ---- | ---------------------------------------------------------- |
| callback | AsyncCallback\<void> | 是   | 回调函数。当停止渲染成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.stop((err: BusinessError) => {
  if (err) {
    console.error('Renderer stop failed');
  } else {
    console.info('Renderer stopped.');
  }
});
```

### stop

stop(): Promise\<void>

停止渲染。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.stop().then(() => {
  console.info('Renderer stopped successfully');
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### release

release(callback: AsyncCallback\<void>): void

释放音频渲染器。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<void> | 是   | 回调函数。当释放音频渲染器成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.release((err: BusinessError) => {
  if (err) {
    console.error('Renderer release failed');
  } else {
    console.info('Renderer released.');
  }
});
```

### release

release(): Promise\<void>

释放渲染器。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.release().then(() => {
  console.info('Renderer released successfully');
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### getAudioTime

getAudioTime(callback: AsyncCallback\<number>): void

获取时间戳（从 1970 年 1 月 1 日开始）。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| -------- | ---------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<number> | 是   | 回调函数。当获取时间戳成功，err为undefined，data为获取到的时间戳；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getAudioTime((err: BusinessError, timestamp: number) => {
  console.info(`Current timestamp: ${timestamp}`);
});
```

### getAudioTime

getAudioTime(): Promise\<number>

获取时间戳（从 1970 年 1 月 1 日开始）。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型             | 描述                      |
| ---------------- | ------------------------- |
| Promise\<number> | Promise对象，返回时间戳。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getAudioTime().then((timestamp: number) => {
  console.info(`Current timestamp: ${timestamp}`);
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### getAudioTimeSync

getAudioTimeSync(): number

获取时间戳（从 1970 年 1 月 1 日开始），同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 描述         |
| ------ | ------------ |
| number | 返回时间戳。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let timestamp: number = audioRenderer.getAudioTimeSync();
  console.info(`Current timestamp: ${timestamp}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`ERROR: ${error}`);
}
```

### getBufferSize

getBufferSize(callback: AsyncCallback\<number>): void

获取音频渲染器的最小缓冲区大小。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| -------- | ---------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<number> | 是   | 回调函数。当获取音频渲染器的最小缓冲区大小成功，err为undefined，data为获取到的最小缓冲区大小；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let bufferSize: number;
audioRenderer.getBufferSize((err: BusinessError, data: number) => {
  if (err) {
    console.error('getBufferSize error');
  } else {
    console.info(`AudioFrameworkRenderLog: getBufferSize: SUCCESS ${data}`);
    bufferSize = data;
  }
});
```

### getBufferSize

getBufferSize(): Promise\<number>

获取音频渲染器的最小缓冲区大小。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型             | 说明                          |
| ---------------- | ----------------------------- |
| Promise\<number> | Promise对象，返回缓冲区大小。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let bufferSize: number;
audioRenderer.getBufferSize().then((data: number) => {
  console.info(`AudioFrameworkRenderLog: getBufferSize: SUCCESS ${data}`);
  bufferSize = data;
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRenderLog: getBufferSize: ERROR: ${err}`);
});
```

### getBufferSizeSync

getBufferSizeSync(): number

获取音频渲染器的最小缓冲区大小，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| number | 返回缓冲区大小。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let bufferSize: number = 0;
try {
  bufferSize = audioRenderer.getBufferSizeSync();
  console.info(`AudioFrameworkRenderLog: getBufferSize: SUCCESS ${bufferSize}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`AudioFrameworkRenderLog: getBufferSize: ERROR: ${error}`);
}
```

### setSpeed

setSpeed(speed: number): void

设置播放倍速。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                     |
| ------ | ------ | ---- | ---------------------------------------- |
| speed  | number | 是   | 设置播放的倍速值（倍速范围：0.25-4.0）。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
audioRenderer.setSpeed(1.5);
```

### getSpeed

getSpeed(): number

获取播放倍速。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明               |
| ------ | ------------------ |
| number | 返回播放的倍速值。 |

**示例：**

```ts
let speed = audioRenderer.getSpeed();
```

### setInterruptMode

setInterruptMode(mode: InterruptMode): Promise&lt;void&gt;

设置应用的焦点模型。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Interrupt

**支持平台：** iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明       |
| ------ | ------------------------------- | ---- | ---------- |
| mode   | [InterruptMode](#interruptmode) | 是   | 焦点模型。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let mode = 0;
audioRenderer.setInterruptMode(mode).then(() => {
  console.info('setInterruptMode Success!');
}).catch((err: BusinessError) => {
  console.error(`setInterruptMode Fail: ${err}`);
});
```

### setInterruptMode

setInterruptMode(mode: InterruptMode, callback: AsyncCallback\<void>): void

设置应用的焦点模型。使用Callback回调返回执行结果。

**系统能力：** SystemCapability.Multimedia.Audio.Interrupt

**支持平台：** iOS

**参数：**

| 参数名   | 类型                            | 必填 | 说明                                                         |
| -------- | ------------------------------- | ---- | ------------------------------------------------------------ |
| mode     | [InterruptMode](#interruptmode) | 是   | 焦点模型。                                                   |
| callback | AsyncCallback\<void>            | 是   | 回调函数。当设置应用的焦点模型成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let mode = 1;
audioRenderer.setInterruptMode(mode, (err: BusinessError) => {
  if (err) {
    console.error(`setInterruptMode Fail: ${err}`);
  }
  console.info('setInterruptMode Success!');
});
```

### setInterruptModeSync

setInterruptModeSync(mode: InterruptMode): void

设置应用的焦点模型，同步设置。

**系统能力：** SystemCapability.Multimedia.Audio.Interrupt

**支持平台：** iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明       |
| ------ | ------------------------------- | ---- | ---------- |
| mode   | [InterruptMode](#interruptmode) | 是   | 焦点模型。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                |
| -------- | ----------------------- |
| 6800101  | invalid parameter error |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  audioRenderer.setInterruptModeSync(0);
  console.info('setInterruptMode Success!');
} catch (err) {
  let error = err as BusinessError;
  console.error(`setInterruptMode Fail: ${error}`);
}
```

### setVolume

setVolume(volume: number): Promise&lt;void&gt;

设置应用的音量。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                  |
| ------ | ------ | ---- | --------------------- |
| volume | number | 是   | 音量值范围为0.0-1.0。 |

**返回值：**

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise&lt;void&gt; | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.setVolume(0.5).then(() => {
  console.info('setVolume Success!');
}).catch((err: BusinessError) => {
  console.error(`setVolume Fail: ${err}`);
});
```

### setVolume

setVolume(volume: number, callback: AsyncCallback\<void>): void

设置应用的音量。使用Callback回调返回执行结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| -------- | -------------------- | ---- | ------------------------------------------------------------ |
| volume   | number               | 是   | 音量值范围为0.0-1.0。                                        |
| callback | AsyncCallback\<void> | 是   | 回调函数。当设置应用的音量成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.setVolume(0.5, (err: BusinessError) => {
  if (err) {
    console.error(`setVolume Fail: ${err}`);
    return;
  }
  console.info('setVolume Success!');
});
```

### getMinStreamVolume

getMinStreamVolume(callback: AsyncCallback&lt;number&gt;): void

获取应用基于音频流的最小音量。使用Callback回调返回。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                                         |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback&lt;number&gt; | 是   | 回调函数。当获取应用基于音频流的最小音量成功，err为undefined，data为获取到的应用基于音频流的最小音量（音量范围0-1）；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getMinStreamVolume((err: BusinessError, minVolume: number) => {
  if (err) {
    console.error(`getMinStreamVolume error: ${err}`);
  } else {
    console.info(`getMinStreamVolume Success! ${minVolume}`);
  }
});
```

### getMinStreamVolume

getMinStreamVolume(): Promise&lt;number&gt;

获取应用基于音频流的最小音量。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                  | 说明                                             |
| --------------------- | ------------------------------------------------ |
| Promise&lt;number&gt; | Promise对象，返回音频流最小音量（音量范围0-1）。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getMinStreamVolume().then((value: number) => {
  console.info(`Get min stream volume Success! ${value}`);
}).catch((err: BusinessError) => {
  console.error(`Get min stream volume Fail: ${err}`);
});
```

### getMinStreamVolumeSync

getMinStreamVolumeSync(): number

获取应用基于音频流的最小音量，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                                |
| ------ | ----------------------------------- |
| number | 返回音频流最小音量（音量范围0-1）。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: number = audioRenderer.getMinStreamVolumeSync();
  console.info(`Get min stream volume Success! ${value}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Get min stream volume Fail: ${error}`);
}
```

### getMaxStreamVolume

getMaxStreamVolume(callback: AsyncCallback&lt;number&gt;): void

获取应用基于音频流的最大音量。使用Callback回调返回。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                                         |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback&lt;number&gt; | 是   | 回调函数。当获取应用基于音频流的最大音量成功，err为undefined，data为获取到的应用基于音频流的最大音量（音量范围0-1）；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getMaxStreamVolume((err: BusinessError, maxVolume: number) => {
  if (err) {
    console.error(`getMaxStreamVolume Fail: ${err}`);
  } else {
    console.info(`getMaxStreamVolume Success! ${maxVolume}`);
  }
});
```

### getMaxStreamVolume

getMaxStreamVolume(): Promise&lt;number&gt;

获取应用基于音频流的最大音量。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型                  | 说明                                             |
| --------------------- | ------------------------------------------------ |
| Promise&lt;number&gt; | Promise对象，返回音频流最大音量（音量范围0-1）。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getMaxStreamVolume().then((value: number) => {
  console.info(`Get max stream volume Success! ${value}`);
}).catch((err: BusinessError) => {
  console.error(`Get max stream volume Fail: ${err}`);
});
```

### getMaxStreamVolumeSync

getMaxStreamVolumeSync(): number

获取应用基于音频流的最大音量，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                                |
| ------ | ----------------------------------- |
| number | 返回音频流最大音量（音量范围0-1）。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: number = audioRenderer.getMaxStreamVolumeSync();
  console.info(`Get max stream volume Success! ${value}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Get max stream volume Fail: ${error}`);
}
```

### getUnderflowCount

getUnderflowCount(callback: AsyncCallback&lt;number&gt;): void

获取当前播放音频流的欠载音频帧数量。使用Callback回调返回。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                                         |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback&lt;number&gt; | 是   | 回调函数。当获取当前播放音频流的欠载音频帧数量成功，err为undefined，data为获取到的当前播放音频流的欠载音频帧数量；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getUnderflowCount((err: BusinessError, underflowCount: number) => {
  if (err) {
    console.error(`getUnderflowCount Fail: ${err}`);
  } else {
    console.info(`getUnderflowCount Success! ${underflowCount}`);
  }
});
```

### getUnderflowCount

getUnderflowCount(): Promise&lt;number&gt;

获取当前播放音频流的欠载音频帧数量。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android

**返回值：**

| 类型                  | 说明                                      |
| --------------------- | ----------------------------------------- |
| Promise&lt;number&gt; | Promise对象，返回音频流的欠载音频帧数量。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getUnderflowCount().then((value: number) => {
  console.info(`Get underflow count Success! ${value}`);
}).catch((err: BusinessError) => {
  console.error(`Get underflow count Fail: ${err}`);
});
```

### getUnderflowCountSync

getUnderflowCountSync(): number

获取当前播放音频流的欠载音频帧数量，同步返回数据。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明                         |
| ------ | ---------------------------- |
| number | 返回音频流的欠载音频帧数量。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let value: number = audioRenderer.getUnderflowCountSync();
  console.info(`Get underflow count Success! ${value}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`Get underflow count Fail: ${error}`);
}
```

### getCurrentOutputDevices

getCurrentOutputDevices(callback: AsyncCallback&lt;AudioDeviceDescriptors&gt;): void

获取音频流输出设备描述符。使用Callback回调返回。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<[AudioDeviceDescriptors](#audiodevicedescriptors)> | 是   | 回调函数。当获取音频流输出设备描述符成功，err为undefined，data为获取到的音频流输出设备描述符；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getCurrentOutputDevices((err: BusinessError, deviceInfo: audio.AudioDeviceDescriptors) => {
  if (err) {
    console.error(`getCurrentOutputDevices Fail: ${err}`);
  } else {
    for (let i = 0; i < deviceInfo.length; i++) {
      console.info(`DeviceInfo id: ${deviceInfo[i].id}`);
      console.info(`DeviceInfo type: ${deviceInfo[i].deviceType}`);
      console.info(`DeviceInfo role: ${deviceInfo[i].deviceRole}`);
      console.info(`DeviceInfo name: ${deviceInfo[i].name}`);
      console.info(`DeviceInfo address: ${deviceInfo[i].address}`);
      console.info(`DeviceInfo samplerate: ${deviceInfo[i].sampleRates[0]}`);
      console.info(`DeviceInfo channelcount: ${deviceInfo[i].channelCounts[0]}`);
      console.info(`DeviceInfo channelmask: ${deviceInfo[i].channelMasks[0]}`);
    }
  }
});
```

### getCurrentOutputDevices

getCurrentOutputDevices(): Promise&lt;AudioDeviceDescriptors&gt;

获取音频流输出设备描述符。使用Promise异步回调。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**返回值：**

| 类型                                                         | 说明                                      |
| ------------------------------------------------------------ | ----------------------------------------- |
| Promise&lt;[AudioDeviceDescriptors](#audiodevicedescriptors)&gt; | Promise对象，返回音频流的输出设备描述信息 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioRenderer.getCurrentOutputDevices().then((deviceInfo: audio.AudioDeviceDescriptors) => {
  for (let i = 0; i < deviceInfo.length; i++) {
    console.info(`DeviceInfo id: ${deviceInfo[i].id}`);
    console.info(`DeviceInfo type: ${deviceInfo[i].deviceType}`);
    console.info(`DeviceInfo role: ${deviceInfo[i].deviceRole}`);
    console.info(`DeviceInfo name: ${deviceInfo[i].name}`);
    console.info(`DeviceInfo address: ${deviceInfo[i].address}`);
    console.info(`DeviceInfo samplerate: ${deviceInfo[i].sampleRates[0]}`);
    console.info(`DeviceInfo channelcount: ${deviceInfo[i].channelCounts[0]}`);
    console.info(`DeviceInfo channelmask: ${deviceInfo[i].channelMasks[0]}`);
  }
}).catch((err: BusinessError) => {
  console.error(`Get current output devices Fail: ${err}`);
});
```

### getCurrentOutputDevicesSync

getCurrentOutputDevicesSync(): AudioDeviceDescriptors

获取音频流输出设备描述符，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**返回值：**

| 类型                                              | 说明                         |
| ------------------------------------------------- | ---------------------------- |
| [AudioDeviceDescriptors](#audiodevicedescriptors) | 返回音频流的输出设备描述信息 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let deviceInfo: audio.AudioDeviceDescriptors = audioRenderer.getCurrentOutputDevicesSync();
  for (let i = 0; i < deviceInfo.length; i++) {
    console.info(`DeviceInfo id: ${deviceInfo[i].id}`);
    console.info(`DeviceInfo type: ${deviceInfo[i].deviceType}`);
    console.info(`DeviceInfo role: ${deviceInfo[i].deviceRole}`);
    console.info(`DeviceInfo name: ${deviceInfo[i].name}`);
    console.info(`DeviceInfo address: ${deviceInfo[i].address}`);
    console.info(`DeviceInfo samplerate: ${deviceInfo[i].sampleRates[0]}`);
    console.info(`DeviceInfo channelcount: ${deviceInfo[i].channelCounts[0]}`);
    console.info(`DeviceInfo channelmask: ${deviceInfo[i].channelMasks[0]}`);
  }
} catch (err) {
  let error = err as BusinessError;
  console.error(`Get current output devices Fail: ${error}`);
}
```

### setChannelBlendMode

setChannelBlendMode(mode: ChannelBlendMode): void

设置单双声道混合模式。使用同步方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android

**参数：**

| 参数名 | 类型                                  | 必填 | 说明               |
| ------ | ------------------------------------- | ---- | ------------------ |
| mode   | [ChannelBlendMode](#channelblendmode) | 是   | 声道混合模式类型。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 6800101  | Input parameter value error.           |
| 6800103  | Operation not permit at current state. |

**示例：**

```ts
let mode = audio.ChannelBlendMode.MODE_DEFAULT;

audioRenderer.setChannelBlendMode(mode);
console.info(`BlendMode: ${mode}`);
```

### setVolumeWithRamp

setVolumeWithRamp(volume: number, duration: number): void

设置音量渐变模式。使用同步方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** iOS

**参数：**

| 参数名   | 类型   | 必填 | 说明                                   |
| -------- | ------ | ---- | -------------------------------------- |
| volume   | number | 是   | 渐变目标音量值，音量范围为[0.0, 1.0]。 |
| duration | number | 是   | 渐变持续时间，单位为ms。               |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 6800101  | Input parameter value error.             |
| 401      | Input parameter type or number mismatch. |

**示例：**

```ts
let volume = 0.5;
let duration = 1000;

audioRenderer.setVolumeWithRamp(volume, duration);
console.info(`setVolumeWithRamp: ${volume}`);
```

### on('audioInterrupt')

on(type: 'audioInterrupt', callback: Callback\<InterruptEvent>): void

监听音频中断事件，使用callback方式返回结果。

用于监听焦点变化。AudioRenderer对象在start事件发生时会主动获取焦点，在pause、stop等事件发生时会主动释放焦点，不需要开发者主动发起获取焦点或释放焦点的申请。

**系统能力：** SystemCapability.Multimedia.Audio.Interrupt

**支持平台：** iOS

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                                                         |
| -------- | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                        | 是   | 事件回调类型，支持的事件为：'audioInterrupt'（中断事件被触发，音频渲染被中断。） |
| callback | Callback\<[InterruptEvent](#interruptevent)\> | 是   | 回调函数，返回播放中断时，应用接收的中断事件信息。           |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

### on('markReach')

on(type: 'markReach', frame: number, callback: Callback&lt;number&gt;): void

订阅到达标记的事件。 当渲染的帧数达到 frame 参数的值时，回调被调用，使用callback方式返回结果。

**系统能力:** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android

**参数：**

| 参数名   | 类型              | 必填 | 说明                                      |
| :------- | :---------------- | :--- | :---------------------------------------- |
| type     | string            | 是   | 事件回调类型，支持的事件为：'markReach'。 |
| frame    | number            | 是   | 触发事件的帧数。 该值必须大于 0。         |
| callback | Callback\<number> | 是   | 回调函数，返回frame 参数的值              |

**示例：**

```ts
audioRenderer.on('markReach', 1000, (position: number) => {
  if (position == 1000) {
    console.info('ON Triggered successfully');
  }
});
```


### off('markReach') 

off(type: 'markReach'): void

取消订阅标记事件。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android

**参数：**

| 参数名 | 类型   | 必填 | 说明                                              |
| :----- | :----- | :--- | :------------------------------------------------ |
| type   | string | 是   | 要取消订阅事件的类型。支持的事件为：'markReach'。 |

**示例：**

```ts
audioRenderer.off('markReach');
```

### on('periodReach') 

on(type: 'periodReach', frame: number, callback: Callback&lt;number&gt;): void

订阅到达标记的事件。 当渲染的帧数达到 frame 参数的值时，触发回调并返回设定的值，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android

**参数：**

| 参数名   | 类型              | 必填 | 说明                                        |
| :------- | :---------------- | :--- | :------------------------------------------ |
| type     | string            | 是   | 事件回调类型，支持的事件为：'periodReach'。 |
| frame    | number            | 是   | 触发事件的帧数。 该值必须大于 0。           |
| callback | Callback\<number> | 是   | 回调函数，返回frame 参数的值                |

**示例：**

```ts
audioRenderer.on('periodReach', 1000, (position: number) => {
  if (position == 1000) {
    console.info('ON Triggered successfully');
  }
});
```

### off('periodReach') 

off(type: 'periodReach'): void

取消订阅标记事件。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                |
| :----- | :----- | :--- | :-------------------------------------------------- |
| type   | string | 是   | 要取消订阅事件的类型。支持的事件为：'periodReach'。 |

**示例：**

```ts
audioRenderer.off('periodReach');
```

### on('stateChange') 

on(type: 'stateChange', callback: Callback<AudioState\>): void

订阅监听状态变化，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                 | 必填 | 说明                                        |
| :------- | :----------------------------------- | :--- | :------------------------------------------ |
| type     | string                               | 是   | 事件回调类型，支持的事件为：'stateChange'。 |
| callback | Callback\<[AudioState](#audiostate)> | 是   | 回调函数，返回当前音频的状态。              |

**示例：**

```ts
audioRenderer.on('stateChange', (state: audio.AudioState) => {
  if (state == 1) {
    console.info('audio renderer state is: STATE_PREPARED');
  }
  if (state == 2) {
    console.info('audio renderer state is: STATE_RUNNING');
  }
});
```

### on('outputDeviceChange') 

on(type: 'outputDeviceChange', callback: Callback\<AudioDeviceDescriptors>): void

订阅监听音频输出设备变化，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                               |
| :------- | :----------------------------------------------------------- | :--- | :------------------------------------------------- |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'outputDeviceChange'。 |
| callback | Callback\<[AudioDeviceDescriptors](#audiodevicedescriptors)> | 是   | 回调函数，返回当前音频流的输出设备描述信息。       |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioRenderer.on('outputDeviceChange', (deviceInfo: audio.AudioDeviceDescriptors) => {
  console.info(`DeviceInfo id: ${deviceInfo[0].id}`);
  console.info(`DeviceInfo name: ${deviceInfo[0].name}`);
  console.info(`DeviceInfo address: ${deviceInfo[0].address}`);
});
```

### off('outputDeviceChange') 

off(type: 'outputDeviceChange', callback?: Callback\<AudioDeviceDescriptors>): void

取消订阅监听音频输出设备变化，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                               |
| :------- | :----------------------------------------------------------- | :--- | :------------------------------------------------- |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'outputDeviceChange'。 |
| callback | Callback\<[AudioDeviceDescriptors](#audiodevicedescriptors)> | 否   | 回调函数，返回当前音频流的输出设备描述信息。       |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioRenderer.off('outputDeviceChange', (deviceInfo: audio.AudioDeviceDescriptors) => {
  console.info(`DeviceInfo id: ${deviceInfo[0].id}`);
  console.info(`DeviceInfo name: ${deviceInfo[0].name}`);
  console.info(`DeviceInfo address: ${deviceInfo[0].address}`);
});
```

### on('outputDeviceChangeWithInfo') 

on(type: 'outputDeviceChangeWithInfo', callback: Callback\<AudioStreamDeviceChangeInfo>): void

订阅监听音频流输出设备变化及原因，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                       |
| :------- | :----------------------------------------------------------- | :--- | :--------------------------------------------------------- |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'outputDeviceChangeWithInfo'。 |
| callback | Callback\<[AudioStreamDeviceChangeInfo](#audiostreamdevicechangeinfo)> | 是   | 回调函数，返回当前音频流的输出设备描述信息及变化原因。     |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                        |
| -------- | ------------------------------- |
| 6800101  | if input parameter value error. |

**示例：**

```ts
audioRenderer.on('outputDeviceChangeWithInfo', (deviceChangeInfo: audio.AudioStreamDeviceChangeInfo) => {
  console.info(`DeviceInfo id: ${deviceChangeInfo.devices[0].id}`);
  console.info(`DeviceInfo name: ${deviceChangeInfo.devices[0].name}`);
  console.info(`DeviceInfo address: ${deviceChangeInfo.devices[0].address}`);
  console.info(`Device change reason: ${deviceChangeInfo.changeReason}`);
});
```

### off('outputDeviceChangeWithInfo') 

off(type: 'outputDeviceChangeWithInfo', callback?: Callback\<AudioStreamDeviceChangeInfo>): void

取消订阅监听音频流输出设备变化及原因，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                       |
| :------- | :----------------------------------------------------------- | :--- | :--------------------------------------------------------- |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'outputDeviceChangeWithInfo'。 |
| callback | Callback\<[AudioStreamDeviceChangeInfo](#audiostreamdevicechangeinfo)> | 否   | 回调函数，返回当前音频流的输出设备描述信息及变化原因。     |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                        |
| -------- | ------------------------------- |
| 6800101  | if input parameter value error. |

**示例：**

```ts
audioRenderer.off('outputDeviceChangeWithInfo', (deviceChangeInfo: audio.AudioStreamDeviceChangeInfo) => {
  console.info(`DeviceInfo id: ${deviceChangeInfo.devices[0].id}`);
  console.info(`DeviceInfo name: ${deviceChangeInfo.devices[0].name}`);
  console.info(`DeviceInfo address: ${deviceChangeInfo.devices[0].address}`);
  console.info(`Device change reason: ${deviceChangeInfo.changeReason}`);
});
```

### on('writeData')

on(type: 'writeData', callback: AudioRendererWriteDataCallback): void

订阅监听音频数据写入回调，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                           | 必填 | 说明                                                         |
| :------- | :----------------------------- | :--- | :----------------------------------------------------------- |
| type     | string                         | 是   | 事件回调类型，支持的事件为：'writeData'。                    |
| callback | AudioRendererWriteDataCallback | 是   | 回调函数，入参代表应用接收待写入的数据缓冲区。<br/>API version 11 不支持返回回调结果，从 API version 12 开始支持返回回调结果AudioDataCallbackResult |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1.Mandatory parameters are left unspecified; 2.Incorrect parameter types. |
| 6800101  | Parameter verification failed.                               |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import {fileIo} from '@kit.CoreFileKit';

class Options {
  offset?: number;
  length?: number;
}

let bufferSize: number = 0;
let path = getContext().cacheDir;
// 确保该路径下存在该资源
let filePath = path + '/StarWars10s-2C-48000-4SW.wav';
let file: fileIo.File = fileIo.openSync(filePath, fileIo.OpenMode.READ_ONLY);
let writeDataCallback = (buffer: ArrayBuffer) => {
  let options: Options = {
    offset: bufferSize,
    length: buffer.byteLength
  };

  try {
    fileIo.readSync(file.fd, buffer, options);
    bufferSize += buffer.byteLength;
    // API version 11 不支持返回回调结果，从 API version 12 开始支持返回回调结果
    return audio.AudioDataCallbackResult.VALID;
  } catch (error) {
    console.error('Error reading file:', error);
    // API version 11 不支持返回回调结果，从 API version 12 开始支持返回回调结果
    return audio.AudioDataCallbackResult.INVALID;
  }
};

audioRenderer.on('writeData', writeDataCallback);
audioRenderer.start().then(() => {
  console.info('Renderer started');
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### off('writeData')

off(type: 'writeData', callback?: AudioRendererWriteDataCallback): void

取消订阅监听音频数据写入回调，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Renderer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                           | 必填 | 说明                                                         |
| :------- | :----------------------------- | :--- | :----------------------------------------------------------- |
| type     | string                         | 是   | 事件回调类型，支持的事件为：'writeData'。                    |
| callback | AudioRendererWriteDataCallback | 否   | 回调函数，入参代表应用接收待写入的数据缓冲区。<br/>API version 11 不支持返回回调结果，从 API version 12 开始支持返回回调结果AudioDataCallbackResult。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types. |
| 6800101  | Input parameter value error.                                 |

**示例：**

```ts
audioRenderer.off('writeData', (data: ArrayBuffer) => {
    console.info(`write data: ${data}`);
});
```

## AudioCapturer

提供音频采集的相关接口。在调用AudioCapturer的接口前，需要先通过[createAudioCapturer](#audiocreateaudiocapturer)创建实例。

### 属性

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

| 名称  | 类型                      | 可读 | 可写 | 说明             | Android平台 | iOS平台 |
| :---- | :------------------------ | :--- | :--- | :--------------- | ----------- | ------- |
| state | [AudioState](#audiostate) | 是   | 否   | 音频采集器状态。 | 支持        | 支持    |

**示例：**

```ts
import audio from '@ohos.multimedia.audio';

let state: audio.AudioState = audioCapturer.state;
```

### getCapturerInfo

getCapturerInfo(callback: AsyncCallback<AudioCapturerInfo\>): void

获取采集器信息。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                    | 必填 | 说明                                                         |
| :------- | :------------------------------------------------------ | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<[AudioCapturerInfo](#audiocapturerinfo)\> | 是   | 回调函数。当获取采集器信息成功，err为undefined，data为获取到的采集器信息；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getCapturerInfo((err: BusinessError, capturerInfo: audio.AudioCapturerInfo) => {
  if (err) {
    console.error('Failed to get capture info');
  } else {
    console.info('Capturer getCapturerInfo:');
    console.info(`Capturer source: ${capturerInfo.source}`);
    console.info(`Capturer flags: ${capturerInfo.capturerFlags}`);
  }
});
```


### getCapturerInfo

getCapturerInfo(): Promise<AudioCapturerInfo\>

获取采集器信息。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型                                              | 说明                          |
| :------------------------------------------------ | :---------------------------- |
| Promise<[AudioCapturerInfo](#audiocapturerinfo)\> | Promise对象，返回采集器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getCapturerInfo().then((audioParamsGet: audio.AudioCapturerInfo) => {
  if (audioParamsGet != undefined) {
    console.info('AudioFrameworkRecLog: Capturer CapturerInfo:');
    console.info(`AudioFrameworkRecLog: Capturer SourceType: ${audioParamsGet.source}`);
    console.info(`AudioFrameworkRecLog: Capturer capturerFlags: ${audioParamsGet.capturerFlags}`);
  } else {
    console.info(`AudioFrameworkRecLog: audioParamsGet is : ${audioParamsGet}`);
    console.info('AudioFrameworkRecLog: audioParams getCapturerInfo are incorrect');
  }
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRecLog: CapturerInfo :ERROR: ${err}`);
})
```

### getCapturerInfoSync

getCapturerInfoSync(): AudioCapturerInfo

获取采集器信息，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型                                    | 说明             |
| :-------------------------------------- | :--------------- |
| [AudioCapturerInfo](#audiocapturerinfo) | 返回采集器信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let audioParamsGet: audio.AudioCapturerInfo = audioCapturer.getCapturerInfoSync();
  console.info(`AudioFrameworkRecLog: Capturer SourceType: ${audioParamsGet.source}`);
  console.info(`AudioFrameworkRecLog: Capturer capturerFlags: ${audioParamsGet.capturerFlags}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`AudioFrameworkRecLog: CapturerInfo :ERROR: ${error}`);
}
```

### getStreamInfo

getStreamInfo(callback: AsyncCallback<AudioStreamInfo\>): void

获取采集器流信息。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                | 必填 | 说明                                                         |
| :------- | :-------------------------------------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<[AudioStreamInfo](#audiostreaminfo)\> | 是   | 回调函数。当获取采集器流信息成功，err为undefined，data为获取到的采集器流信息；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getStreamInfo((err: BusinessError, streamInfo: audio.AudioStreamInfo) => {
  if (err) {
    console.error('Failed to get stream info');
  } else {
    console.info('Capturer GetStreamInfo:');
    console.info(`Capturer sampling rate: ${streamInfo.samplingRate}`);
    console.info(`Capturer channel: ${streamInfo.channels}`);
    console.info(`Capturer format: ${streamInfo.sampleFormat}`);
    console.info(`Capturer encoding type: ${streamInfo.encodingType}`);
  }
});
```

### getStreamInfo

getStreamInfo(): Promise<AudioStreamInfo\>

获取采集器流信息。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型                                          | 说明                      |
| :-------------------------------------------- | :------------------------ |
| Promise<[AudioStreamInfo](#audiostreaminfo)\> | Promise对象，返回流信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getStreamInfo().then((audioParamsGet: audio.AudioStreamInfo) => {
  console.info('getStreamInfo:');
  console.info(`sampleFormat: ${audioParamsGet.sampleFormat}`);
  console.info(`samplingRate: ${audioParamsGet.samplingRate}`);
  console.info(`channels: ${audioParamsGet.channels}`);
  console.info(`encodingType: ${audioParamsGet.encodingType}`);
}).catch((err: BusinessError) => {
  console.error(`getStreamInfo :ERROR: ${err}`);
});
```

### getStreamInfoSync

getStreamInfoSync(): AudioStreamInfo

获取采集器流信息，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明         |
| :---------------------------------- | :----------- |
| [AudioStreamInfo](#audiostreaminfo) | 返回流信息。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let audioParamsGet: audio.AudioStreamInfo = audioCapturer.getStreamInfoSync();
  console.info(`sampleFormat: ${audioParamsGet.sampleFormat}`);
  console.info(`samplingRate: ${audioParamsGet.samplingRate}`);
  console.info(`channels: ${audioParamsGet.channels}`);
  console.info(`encodingType: ${audioParamsGet.encodingType}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`getStreamInfo :ERROR: ${error}`);
}
```

### getAudioStreamId

getAudioStreamId(callback: AsyncCallback<number\>): void

获取音频流id，使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| :------- | :--------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<number\> | 是   | 回调函数。当获取音频流id成功，err为undefined，data为获取到的音频流id；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getAudioStreamId((err: BusinessError, streamId: number) => {
  console.info(`audioCapturer GetStreamId: ${streamId}`);
});
```

### getAudioStreamId

getAudioStreamId(): Promise<number\>

获取音频流id，使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型             | 说明                        |
| :--------------- | :-------------------------- |
| Promise<number\> | Promise对象，返回音频流id。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getAudioStreamId().then((streamId: number) => {
  console.info(`audioCapturer getAudioStreamId: ${streamId}`);
}).catch((err: BusinessError) => {
  console.error(`ERROR: ${err}`);
});
```

### getAudioStreamIdSync

getAudioStreamIdSync(): number

获取音频流id，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明           |
| :----- | :------------- |
| number | 返回音频流id。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let streamId: number = audioCapturer.getAudioStreamIdSync();
  console.info(`audioCapturer getAudioStreamIdSync: ${streamId}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`ERROR: ${error}`);
}
```

### start

start(callback: AsyncCallback<void\>): void

启动音频采集器。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| :------- | :------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<void\> | 是   | 回调函数。当启动音频采集器成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.start((err: BusinessError) => {
  if (err) {
    console.error('Capturer start failed.');
  } else {
    console.info('Capturer start success.');
  }
});
```


### start

start(): Promise<void\>

启动音频采集器。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| :------------- | :------------------------ |
| Promise<void\> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.start().then(() => {
  console.info('AudioFrameworkRecLog: ---------START---------');
  console.info('AudioFrameworkRecLog: Capturer started: SUCCESS');
  console.info(`AudioFrameworkRecLog: AudioCapturer: STATE: ${audioCapturer.state}`);
  console.info('AudioFrameworkRecLog: Capturer started: SUCCESS');
  if ((audioCapturer.state == audio.AudioState.STATE_RUNNING)) {
    console.info('AudioFrameworkRecLog: AudioCapturer is in Running State');
  }
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRecLog: Capturer start :ERROR : ${err}`);
});
```

### stop

stop(callback: AsyncCallback<void\>): void

停止采集。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                       |
| :------- | :------------------- | :--- | :--------------------------------------------------------- |
| callback | AsyncCallback<void\> | 是   | 回调函数。当停止采集成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.stop((err: BusinessError) => {
  if (err) {
    console.error('Capturer stop failed');
  } else {
    console.info('Capturer stopped.');
  }
});
```


### stop

stop(): Promise<void\>

停止采集。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| :------------- | :------------------------ |
| Promise<void\> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.stop().then(() => {
  console.info('AudioFrameworkRecLog: ---------STOP RECORD---------');
  console.info('AudioFrameworkRecLog: Capturer stopped: SUCCESS');
  if ((audioCapturer.state == audio.AudioState.STATE_STOPPED)){
    console.info('AudioFrameworkRecLog: State is Stopped:');
  }
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRecLog: Capturer stop: ERROR: ${err}`);
});
```

### release

release(callback: AsyncCallback<void\>): void

释放采集器。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                                         |
| :------- | :------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<void\> | 是   | 回调函数。当释放采集器成功，err为undefined，否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.release((err: BusinessError) => {
  if (err) {
    console.error('capturer release failed');
  } else {
    console.info('capturer released.');
  }
});
```


### release

release(): Promise<void\>

释放采集器。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| :------------- | :------------------------ |
| Promise<void\> | Promise对象，无返回结果。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.release().then(() => {
  console.info('AudioFrameworkRecLog: ---------RELEASE RECORD---------');
  console.info('AudioFrameworkRecLog: Capturer release : SUCCESS');
  console.info(`AudioFrameworkRecLog: AudioCapturer : STATE : ${audioCapturer.state}`);
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRecLog: Capturer stop: ERROR: ${err}`);
});
```

### getAudioTime

getAudioTime(callback: AsyncCallback<number\>): void

获取时间戳（从1970年1月1日开始），单位为纳秒。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| :------- | :--------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<number\> | 是   | 回调函数。当获取时间戳成功，err为undefined，data为获取到的时间戳；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getAudioTime((err: BusinessError, timestamp: number) => {
  console.info(`Current timestamp: ${timestamp}`);
});
```

### getAudioTime

getAudioTime(): Promise<number\>

获取时间戳（从1970年1月1日开始），单位为纳秒。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型             | 说明                                                        |
| :--------------- | :---------------------------------------------------------- |
| Promise<number\> | Promise对象，返回时间戳（从1970年1月1日开始），单位为纳秒。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getAudioTime().then((audioTime: number) => {
  console.info(`AudioFrameworkRecLog: AudioCapturer getAudioTime : Success ${audioTime}`);
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRecLog: AudioCapturer Created : ERROR : ${err}`);
});
```

### getAudioTimeSync

getAudioTimeSync(): number

获取时间戳（从1970年1月1日开始），单位为纳秒，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明         |
| :----- | :----------- |
| number | 返回时间戳。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

try {
  let audioTime: number = audioCapturer.getAudioTimeSync();
  console.info(`AudioFrameworkRecLog: AudioCapturer getAudioTimeSync : Success ${audioTime}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`AudioFrameworkRecLog: AudioCapturer getAudioTimeSync : ERROR : ${error}`);
}
```

### getBufferSize

getBufferSize(callback: AsyncCallback<number\>): void

获取采集器合理的最小缓冲区大小。使用callback方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                                         |
| :------- | :--------------------- | :--- | :----------------------------------------------------------- |
| callback | AsyncCallback<number\> | 是   | 回调函数。当获取采集器合理的最小缓冲区大小成功，err为undefined，data为获取到的采集器合理的最小缓冲区大小；否则为错误对象。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

audioCapturer.getBufferSize((err: BusinessError, bufferSize: number) => {
  if (!err) {
    console.info(`BufferSize : ${bufferSize}`);
    audioCapturer.read(bufferSize, true).then((buffer: ArrayBuffer) => {
      console.info(`Buffer read is ${buffer.byteLength}`);
    }).catch((err: BusinessError) => {
      console.error(`AudioFrameworkRecLog: AudioCapturer Created : ERROR : ${err}`);
    });
  }
});
```

### getBufferSize

getBufferSize(): Promise<number\>

获取采集器合理的最小缓冲区大小。使用Promise方式异步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型             | 说明                          |
| :--------------- | :---------------------------- |
| Promise<number\> | Promise对象，返回缓冲区大小。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let bufferSize: number = 0;
audioCapturer.getBufferSize().then((data: number) => {
  console.info(`AudioFrameworkRecLog: getBufferSize :SUCCESS ${data}`);
  bufferSize = data;
}).catch((err: BusinessError) => {
  console.error(`AudioFrameworkRecLog: getBufferSize :ERROR : ${err}`);
});
```

### getBufferSizeSync

getBufferSizeSync(): number

获取采集器合理的最小缓冲区大小，同步返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**返回值：**

| 类型   | 说明             |
| :----- | :--------------- |
| number | 返回缓冲区大小。 |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let bufferSize: number = 0;
try {
  bufferSize = audioCapturer.getBufferSizeSync();
  console.info(`AudioFrameworkRecLog: getBufferSizeSync :SUCCESS ${bufferSize}`);
} catch (err) {
  let error = err as BusinessError;
  console.error(`AudioFrameworkRecLog: getBufferSizeSync :ERROR : ${error}`);
}
```

### getCurrentInputDevices

getCurrentInputDevices(): AudioDeviceDescriptors

获取录音流输入设备描述符。使用同步方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**返回值：**

| 类型                                              | 说明                                 |
| ------------------------------------------------- | ------------------------------------ |
| [AudioDeviceDescriptors](#audiodevicedescriptors) | 同步接口，返回设备属性数组类型数据。 |

**示例：**

```ts
let deviceDescriptors: audio.AudioDeviceDescriptors = audioCapturer.getCurrentInputDevices();
console.info(`Device id: ${deviceDescriptors[0].id}`);
console.info(`Device type: ${deviceDescriptors[0].deviceType}`);
console.info(`Device role: ${deviceDescriptors[0].deviceRole}`);
console.info(`Device name: ${deviceDescriptors[0].name}`);
console.info(`Device address: ${deviceDescriptors[0].address}`);
console.info(`Device samplerates: ${deviceDescriptors[0].sampleRates[0]}`);
console.info(`Device channelcounts: ${deviceDescriptors[0].channelCounts[0]}`);
console.info(`Device channelmask: ${deviceDescriptors[0].channelMasks[0]}`);
if (deviceDescriptors[0].encodingTypes) {
  console.info(`Device encodingTypes: ${deviceDescriptors[0].encodingTypes[0]}`);
}
```

### getCurrentAudioCapturerChangeInfo

getCurrentAudioCapturerChangeInfo(): AudioCapturerChangeInfo

获取录音流配置。使用同步方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**返回值：**

| 类型                                                | 说明                                   |
| :-------------------------------------------------- | :------------------------------------- |
| [AudioCapturerChangeInfo](#audiocapturerchangeinfo) | 同步接口，返回描述音频采集器更改信息。 |

**示例：**

```ts
let info: audio.AudioCapturerChangeInfo = audioCapturer.getCurrentAudioCapturerChangeInfo();
console.info(`Info streamId: ${info.streamId}`);
console.info(`Info source: ${info.capturerInfo.source}`);
console.info(`Info capturerFlags: ${info.capturerInfo.capturerFlags}`);
console.info(`Info muted: ${info.muted}`);
console.info(`Info type: ${info.deviceDescriptors[0].deviceType}`);
console.info(`Info role: ${info.deviceDescriptors[0].deviceRole}`);
console.info(`Info name: ${info.deviceDescriptors[0].name}`);
console.info(`Info address: ${info.deviceDescriptors[0].address}`);
console.info(`Info samplerates: ${info.deviceDescriptors[0].sampleRates[0]}`);
console.info(`Info channelcounts: ${info.deviceDescriptors[0].channelCounts[0]}`);
console.info(`Info channelmask: ${info.deviceDescriptors[0].channelMasks[0]}`);
if (info.deviceDescriptors[0].encodingTypes) {
  console.info(`Device encodingTypes: ${info.deviceDescriptors[0].encodingTypes[0]}`);
}
```

### on('audioInterrupt')

on(type: 'audioInterrupt', callback: Callback\<InterruptEvent>): void

监听音频中断事件，使用callback方式返回结果。

用于监听焦点变化。AudioCapturer对象在start事件发生时会主动获取焦点，在pause、stop等事件发生时会主动释放焦点，不需要开发者主动发起获取焦点或释放焦点的申请。

**系统能力：** SystemCapability.Multimedia.Audio.Interrupt

**支持平台：** iOS

**参数：**

| 参数名   | 类型                                          | 必填 | 说明                                                         |
| -------- | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                        | 是   | 事件回调类型，支持的事件为：'audioInterrupt'（中断事件被触发，音频采集被中断。） |
| callback | Callback\<[InterruptEvent](#interruptevent)\> | 是   | 回调函数，返回播放中断时，应用接收的中断事件信息。           |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

### off('audioInterrupt')

off(type: 'audioInterrupt'): void

取消订阅音频中断事件。

**系统能力：** SystemCapability.Multimedia.Audio.Interrupt

**支持平台：** iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                         |
| ------ | ------ | ---- | -------------------------------------------- |
| type   | string | 是   | 事件回调类型，支持的事件为：'audioInterrupt' |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 6800101  | Invalid parameter error. |

**示例：**

```ts
audioCapturer.off('audioInterrupt');
```

### on('inputDeviceChange')

on(type: 'inputDeviceChange', callback: Callback\<AudioDeviceDescriptors>): void

订阅监听音频输入设备变化，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| :------- | :----------------------------------------------------------- | :--- | :----------------------------------------------------------- |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'inputDeviceChange'。            |
| callback | Callback\<[AudioDeviceDescriptors](#audiodevicedescriptors)> | 是   | 回调函数，返回监听的音频输入设备变化(返回数据为切换后的设备信息)。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
audioCapturer.on('inputDeviceChange', (deviceChangeInfo: audio.AudioDeviceDescriptors) => {
  console.info(`inputDevice id: ${deviceChangeInfo[0].id}`);
  console.info(`inputDevice deviceRole: ${deviceChangeInfo[0].deviceRole}`);
  console.info(`inputDevice deviceType: ${deviceChangeInfo[0].deviceType}`);
});
```

### off('inputDeviceChange')

off(type: 'inputDeviceChange', callback?: Callback\<AudioDeviceDescriptors>): void

取消订阅音频输入设备更改事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Device

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                              |
| :------- | :----------------------------------------------------------- | :--- | :------------------------------------------------ |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'inputDeviceChange'。 |
| callback | Callback\<[AudioDeviceDescriptors](#audiodevicedescriptors)> | 否   | 回调函数，返回监听的音频输入设备信息。            |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
audioCapturer.off('inputDeviceChange');
```

### on('audioCapturerChange')

on(type: 'audioCapturerChange', callback: Callback\<AudioCapturerChangeInfo>): void

订阅监听录音流配置变化，使用callback方式返回结果。订阅内部是异步实现，是非精确回调，在录音流配置变化的同时注册回调，收到的返回结果存在变化可能性。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| :------- | :----------------------------------------------------------- | :--- | :----------------------------------------------------------- |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'audioCapturerChange'。          |
| callback | Callback\<[AudioCapturerChangeInfo](#audiocapturerchangeinfo)> | 是   | 回调函数，录音流配置或状态变化时返回监听的录音流当前配置和状态信息。 |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
audioCapturer.on('audioCapturerChange', (capturerChangeInfo: audio.AudioCapturerChangeInfo) => {
  console.info(`audioCapturerChange id: ${capturerChangeInfo[0].id}`);
  console.info(`audioCapturerChange deviceRole: ${capturerChangeInfo[0].deviceRole}`);
  console.info(`audioCapturerChange deviceType: ${capturerChangeInfo[0].deviceType}`);
});
```

### off('audioCapturerChange')

off(type: 'audioCapturerChange', callback?: Callback\<AudioCapturerChangeInfo>): void

取消订阅音频输入设备更改事件，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                |
| :------- | :----------------------------------------------------------- | :--- | :-------------------------------------------------- |
| type     | string                                                       | 是   | 事件回调类型，支持的事件为：'audioCapturerChange'。 |
| callback | Callback\<[AudioCapturerChangeInfo](#audiocapturerchangeinfo)> | 否   | 回调函数，返回取消监听的录音流配置或状态变化。      |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
audioCapturer.off('audioCapturerChange');
```

### on('markReach')

on(type: 'markReach', frame: number, callback: Callback&lt;number&gt;): void

订阅标记到达的事件。 当采集的帧数达到 frame 参数的值时，回调被触发，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android

**参数：**

| 参数名   | 类型              | 必填 | 说明                                      |
| :------- | :---------------- | :--- | :---------------------------------------- |
| type     | string            | 是   | 事件回调类型，支持的事件为：'markReach'。 |
| frame    | number            | 是   | 触发事件的帧数。 该值必须大于0。          |
| callback | Callback\<number> | 是   | 回调函数，返回frame 参数的值              |

**示例：**

```ts
audioCapturer.on('markReach', 1000, (position: number) => {
  if (position == 1000) {
    console.info('ON Triggered successfully');
  }
});
```

### off('markReach')

off(type: 'markReach'): void

取消订阅标记到达的事件。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android

**参数：**

| 参数名 | 类型   | 必填 | 说明                                          |
| :----- | :----- | :--- | :-------------------------------------------- |
| type   | string | 是   | 取消事件回调类型，支持的事件为：'markReach'。 |

**示例：**

```ts
audioCapturer.off('markReach');
```

### on('periodReach')

on(type: 'periodReach', frame: number, callback: Callback&lt;number&gt;): void

订阅到达标记的事件。 当采集的帧数达到 frame 参数的值时，触发回调并返回设定的值，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型              | 必填 | 说明                                        |
| :------- | :---------------- | :--- | :------------------------------------------ |
| type     | string            | 是   | 事件回调类型，支持的事件为：'periodReach'。 |
| frame    | number            | 是   | 触发事件的帧数。 该值必须大于0。            |
| callback | Callback\<number> | 是   | 回调函数，返回frame 参数的值。              |

**示例：**

```ts
audioCapturer.on('periodReach', 1000, (position: number) => {
  if (position == 1000) {
    console.info('ON Triggered successfully');
  }
});
```

### off('periodReach')

off(type: 'periodReach'): void

取消订阅标记到达的事件。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                            |
| :----- | :----- | :--- | :---------------------------------------------- |
| type   | string | 是   | 取消事件回调类型，支持的事件为：'periodReach'。 |

**示例：**

```ts
audioCapturer.off('periodReach');
```

### on('stateChange') 

on(type: 'stateChange', callback: Callback<AudioState\>): void

订阅监听状态变化，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                 | 必填 | 说明                                        |
| :------- | :----------------------------------- | :--- | :------------------------------------------ |
| type     | string                               | 是   | 事件回调类型，支持的事件为：'stateChange'。 |
| callback | Callback\<[AudioState](#audiostate)> | 是   | 回调函数，返回当前音频的状态。              |

**示例：**

```ts
audioCapturer.on('stateChange', (state: audio.AudioState) => {
  if (state == 1) {
    console.info('audio capturer state is: STATE_PREPARED');
  }
  if (state == 2) {
    console.info('audio capturer state is: STATE_RUNNING');
  }
});
```

### on('readData')

on(type: 'readData', callback: Callback\<ArrayBuffer>): void

订阅监听音频数据读入回调，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                     |
| :------- | :--------------------- | :--- | :--------------------------------------- |
| type     | string                 | 是   | 事件回调类型，支持的事件为：'readData'。 |
| callback | Callback\<ArrayBuffer> | 是   | 回调函数，返回读到的数据缓冲区。         |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';
import fs from '@ohos.file.fs';

let bufferSize: number = 0;
class Options {
  offset?: number;
  length?: number;
}

let readDataCallback = (buffer: ArrayBuffer) => {
  let path = getContext().cacheDir;
  let filePath = path + '/StarWars10s-2C-48000-4SW.wav';
  let file: fs.File = fs.openSync(filePath, fs.OpenMode.READ_WRITE);
  let options: Options = {
    offset: bufferSize,
    length: buffer.byteLength
  }
  fs.writeSync(file.fd, buffer, options);
  bufferSize += buffer.byteLength;
}
audioCapturer.on('readData', readDataCallback);
audioCapturer.start((err: BusinessError) => {
  if (err) {
    console.error('Capturer start failed.');
  } else {
    console.info('Capturer start success.');
  }
});
```

### off('readData')

off(type: 'readData', callback?: Callback\<ArrayBuffer>): void

取消订阅监听音频数据读入回调，使用callback方式返回结果。

**系统能力：** SystemCapability.Multimedia.Audio.Capturer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                                     |
| :------- | :--------------------- | :--- | :--------------------------------------- |
| type     | string                 | 是   | 事件回调类型，支持的事件为：'readData'。 |
| callback | Callback\<ArrayBuffer> | 否   | 回调函数，返回读到的数据缓冲区。         |

**错误码：**

以下错误码的详细介绍请参见[Audio错误码](../errorcodes/errorcode-audio.md)。

| 错误码ID | 错误信息                     |
| -------- | ---------------------------- |
| 6800101  | Input parameter value error. |

**示例：**

```ts
audioCapturer.off('readData', (data: ArrayBuffer) => {
    console.info(`read data: ${data}`);
});
```