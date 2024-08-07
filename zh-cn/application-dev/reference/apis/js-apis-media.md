# @ohos.multimedia.media (媒体服务)

> **说明：**
> 本模块首批接口从API version 12开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

媒体子系统为开发者提供一套简单且易于理解的接口，使得开发者能够方便接入系统并使用系统的媒体资源。

媒体子系统包含了音视频相关媒体业务，提供以下常用功能：

- 音视频播放（[AVPlayer](#avplayer)）

- 音视频录制（[AVRecorder](#avrecorder)）

- 获取音视频元数据（[AVMetadataExtractor](#avmetadataextractor)）

## 导入模块

```ts
import media from '@ohos.multimedia.media';
```

## media.createAVPlayer

createAVPlayer(callback: AsyncCallback\<AVPlayer>): void

异步方式创建音视频播放实例，通过注册回调函数获取返回值。

> **说明：**
>
> - 可创建的视频播放实例不能超过13个。
> - 可创建的音视频播放实例（即音频、视频、音视频三类相加）不能超过16个。
> - 可创建的音视频播放实例数量依赖于设备芯片的支持情况，如芯片支持创建的数量少于上述情况，请以芯片规格为准。如RK3568仅支持创建6个以内的视频播放实例。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                  | 必填 | 说明                                                         |
| -------- | ------------------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<[AVPlayer](#avplayer)> | 是   | 回调函数。异步返回AVPlayer实例，失败时返回null。可用于音视频播放。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 5400101  | No memory. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let avPlayer: media.AVPlayer;
media.createAVPlayer((error: BusinessError, video: media.AVPlayer) => {
  if (video != null) {
    avPlayer = video;
    console.info('createAVPlayer success');
  } else {
    console.error(`createAVPlayer fail, error message:${error.message}`);
  }
});
```

## media.createAVPlayer

createAVPlayer(): Promise\<AVPlayer>

异步方式创建音视频播放实例，通过Promise获取返回值。

> **说明：**
>
> - 可创建的视频播放实例不能超过13个。
> - 可创建的音视频播放实例（即音频、视频、音视频三类相加）不能超过16个。
> - 可创建的音视频播放实例数量依赖于设备芯片的支持情况，如芯片支持创建的数量少于上述情况，请以芯片规格为准。如RK3568仅支持创建6个以内的视频播放实例。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型                            | 说明                                                         |
| ------------------------------- | ------------------------------------------------------------ |
| Promise\<[AVPlayer](#avplayer)> | Promise对象。异步返回AVPlayer实例，失败时返回null。可用于音视频播放。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                      |
| -------- | ----------------------------- |
| 5400101  | No memory. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let avPlayer: media.AVPlayer;
media.createAVPlayer().then((video: media.AVPlayer) => {
  if (video != null) {
    avPlayer = video;
    console.info('createAVPlayer success');
  } else {
    console.error('createAVPlayer fail');
  }
}).catch((error: BusinessError) => {
  console.error(`AVPlayer catchCallback, error message:${error.message}`);
});
```

## media.createAVRecorder

createAVRecorder(callback: AsyncCallback\<AVRecorder>): void

异步方式创建音视频录制实例。通过注册回调函数获取返回值。

> **说明：**
>
> - 可创建的音视频录制实例不能超过2个。
> - 由于设备共用音频通路，一个设备仅能有一个实例进行音频录制。创建第二个实例录制音频时，将会因为音频通路冲突导致创建失败。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                      | 必填 | 说明                                                         |
| -------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<[AVRecorder](#avrecorder)> | 是   | 回调函数。异步返回AVRecorder实例，失败时返回null。可用于录制音视频媒体。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 5400101  | No memory. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';
let avRecorder: media.AVRecorder;

media.createAVRecorder((error: BusinessError, recorder: media.AVRecorder) => {
  if (recorder != null) {
    avRecorder = recorder;
    console.info('createAVRecorder success');
  } else {
    console.error(`createAVRecorder fail, error message:${error.message}`);
  }
});
```

## media.createAVRecorder

createAVRecorder(): Promise\<AVRecorder>

异步方式创建音视频录制实例。通过Promise获取返回值。

> **说明：**
>
> - 可创建的音视频录制实例不能超过2个。
> - 由于设备共用音频通路，一个设备仅能有一个实例进行音频录制。创建第二个实例录制音频时，将会因为音频通路冲突导致创建失败。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明                                                         |
| ----------------------------------- | ------------------------------------------------------------ |
| Promise\<[AVRecorder](#avrecorder)> | Promise对象。异步返回AVRecorder实例，失败时返回null。可用于录制音视频媒体。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                      |
| -------- | ----------------------------- |
| 5400101  | No memory. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let avRecorder: media.AVRecorder;
media.createAVRecorder().then((recorder: media.AVRecorder) => {
  if (recorder != null) {
    avRecorder = recorder;
    console.info('createAVRecorder success');
  } else {
    console.error('createAVRecorder fail');
  }
}).catch((error: BusinessError) => {
  console.error(`createAVRecorder catchCallback, error message:${error.message}`);
});
```

## media.createAVMetadataExtractor

createAVMetadataExtractor(callback: AsyncCallback\<AVMetadataExtractor>): void

异步方式创建AVMetadataExtractor实例，通过注册回调函数获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                        | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback\<[AVMetadataExtractor](#avmetadataextractor)> | 是   | 回调函数。异步返回AVMetadataExtractor实例，失败时返回null。可用于获取音视频元信息。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 5400101  | No memory. Returned by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let avMetadataExtractor: media.AVMetadataExtractor;
media.createAVMetadataExtractor((error: BusinessError, extractor: media.AVMetadataExtractor) => {
  if (extractor != null) {
    avMetadataExtractor = extractor;
    console.info('createAVMetadataExtractor success');
  } else {
    console.error(`createAVMetadataExtractor fail, error message:${error.message}`);
  }
});
```

## media.createAVMetadataExtractor

createAVMetadataExtractor(): Promise\<AVMetadataExtractor>

异步方式创建AVMetadataExtractor对象，通过Promise获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**返回值：**

| 类型                                                  | 说明                                                         |
| ----------------------------------------------------- | ------------------------------------------------------------ |
| Promise\<[AVMetadataExtractor](#avmetadataextractor)> | Promise对象。异步返回AVMetadataExtractor实例，失败时返回null。可用于获取音视频元数据。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                        |
| -------- | ------------------------------- |
| 5400101  | No memory. Returned by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

let avMetadataExtractor: media.AVMetadataExtractor;
media.createAVMetadataExtractor().then((extractor: media.AVMetadataExtractor) => {
  if (extractor != null) {
    avMetadataExtractor = extractor;
    console.info('createAVMetadataExtractor success');
  } else {
    console.error('createAVMetadataExtractor fail');
  }
}).catch((error: BusinessError) => {
  console.error(`AVMetadataExtractor catchCallback, error message:${error.message}`);
});
```

## AVErrorCode

枚举值，定义[媒体错误码](../errorcodes/errorcode-media.md)类型。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称                                  | 值      | 说明                                 | Android平台 | iOS平台 |
| :------------------------------------ | ------- | ------------------------------------ | ----------- | ------- |
| AVERR_OK                              | 0       | 表示操作成功。                       | 支持        | 支持    |
| AVERR_NO_PERMISSION                   | 201     | 表示无权限执行此操作。               | 支持        | 支持    |
| AVERR_INVALID_PARAMETER               | 401     | 表示传入入参无效。                   | 支持        | 支持    |
| AVERR_UNSUPPORT_CAPABILITY            | 801     | 表示当前版本不支持该API能力。        | 支持        | 支持    |
| AVERR_NO_MEMORY                       | 5400101 | 表示系统内存不足或服务数量达到上限。 | 支持        | 支持    |
| AVERR_OPERATE_NOT_PERMIT              | 5400102 | 表示当前状态不允许或无权执行此操作。 | 支持        | 支持    |
| AVERR_IO                              | 5400103 | 表示数据流异常信息。                 | 支持        | 支持    |
| AVERR_TIMEOUT                         | 5400104 | 表示系统或网络响应超时。             | 支持        | 支持    |
| AVERR_SERVICE_DIED                    | 5400105 | 表示服务进程死亡。                   | 支持        | 支持    |
| AVERR_UNSUPPORT_FORMAT                | 5400106 | 表示不支持当前媒体资源的格式。       | 支持        | 支持    |
| AVERR_AUDIO_INTERRUPTED<sup>11+</sup> | 5400107 | 表示音频焦点被抢占                   | 支持        | 支持    |

## MediaType

枚举值，定义媒体类型枚举。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称           | 值   | 说明       | Android平台 | iOS平台 |
| -------------- | ---- | ---------- | ----------- | ------- |
| MEDIA_TYPE_AUD | 0    | 表示音频。 | 支持        | 支持    |
| MEDIA_TYPE_VID | 1    | 表示视频。 | 支持        | 支持    |

## CodecMimeType

枚举值，定义Codec MIME类型。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称                     | 值                    | 说明                     | Android平台 | iOS平台 |
| ------------------------ | --------------------- | ------------------------ | ----------- | ------- |
| VIDEO_H263               | 'video/h263'          | 表示视频/h263类型。      | 支持        | 支持    |
| VIDEO_AVC                | 'video/avc'           | 表示视频/avc类型。       | 支持        | 支持    |
| VIDEO_MPEG2              | 'video/mpeg2'         | 表示视频/mpeg2类型。     | 支持        | 支持    |
| VIDEO_MPEG4              | 'video/mp4v-es'       | 表示视频/mpeg4类型。     | 支持        | 支持    |
| VIDEO_VP8                | 'video/x-vnd.on2.vp8' | 表示视频/vp8类型。       | 支持        | 支持    |
| VIDEO_HEVC<sup>11+</sup> | 'video/hevc'          | 表示视频/H265类型。      | 支持        | 支持    |
| AUDIO_AAC                | 'audio/mp4a-latm'     | 表示音频/mp4a-latm类型。 | 支持        | 支持    |
| AUDIO_VORBIS             | 'audio/vorbis'        | 表示音频/vorbis类型。    | 支持        | 支持    |
| AUDIO_FLAC               | 'audio/flac'          | 表示音频/flac类型。      | 支持        | 支持    |

## MediaDescriptionKey

枚举值，定义媒体信息描述。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称                     | 值              | 说明                                                         | Android平台 | iOS平台 |
| ------------------------ | --------------- | ------------------------------------------------------------ | ----------- | ------- |
| MD_KEY_TRACK_INDEX       | 'track_index'   | 表示轨道序号，其对应键值类型为number。                       | 支持        | 支持    |
| MD_KEY_TRACK_TYPE        | 'track_type'    | 表示轨道类型，其对应键值类型为number，参考[MediaType](#mediatype)。 | 支持        | 支持    |
| MD_KEY_CODEC_MIME        | 'codec_mime'    | 表示codec_mime类型，其对应键值类型为string。                 | 支持        | 支持    |
| MD_KEY_DURATION          | 'duration'      | 表示媒体时长，其对应键值类型为number，单位为毫秒（ms）。     | 支持        | 支持    |
| MD_KEY_BITRATE           | 'bitrate'       | 表示比特率，其对应键值类型为number，单位为比特率（bps）。    | 支持        | 支持    |
| MD_KEY_WIDTH             | 'width'         | 表示视频宽度，其对应键值类型为number，单位为像素（px）。     | 支持        | 支持    |
| MD_KEY_HEIGHT            | 'height'        | 表示视频高度，其对应键值类型为number，单位为像素（px）。     | 支持        | 支持    |
| MD_KEY_FRAME_RATE        | 'frame_rate'    | 表示视频帧率，其对应键值类型为number，单位为100帧每秒（100fps）。 | 支持        | 支持    |
| MD_KEY_AUD_CHANNEL_COUNT | 'channel_count' | 表示声道数，其对应键值类型为number。                         | 支持        | 支持    |
| MD_KEY_AUD_SAMPLE_RATE   | 'sample_rate'   | 表示采样率，其对应键值类型为number，单位为赫兹（Hz）。       | 支持        | 支持    |

## BufferingInfoType

枚举值，定义缓存事件类型。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称              | 值   | 说明                             | Android平台 | iOS平台 |
| ----------------- | ---- | -------------------------------- | ----------- | ------- |
| BUFFERING_START   | 1    | 表示开始缓存。                   | 支持        | 支持    |
| BUFFERING_END     | 2    | 表示结束缓存。                   | 支持        | 支持    |
| BUFFERING_PERCENT | 3    | 表示缓存百分比。                 | 支持        | 支持    |
| CACHED_DURATION   | 4    | 表示缓存时长，单位为毫秒（ms）。 | 支持        | 支持    |

## StateChangeReason

枚举值，定义播放或录制实例状态机切换原因，伴随state一起上报。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称       | 值   | 说明                                                         | Android平台 | iOS平台 |
| ---------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| USER       | 1    | 表示用户行为造成的状态切换，由用户或客户端主动调用接口产生。 | 支持        | 支持    |
| BACKGROUND | 2    | 表示后台系统行为造成的状态切换，比如应用未注册播控中心权限，退到后台时被系统强制暂停或停止。 | 支持        | 支持    |

## AVPlayer

播放管理类，用于管理和播放媒体资源。在调用AVPlayer的方法前，需要先通过[createAVPlayer()](#mediacreateavplayer)构建一个AVPlayer实例。

### 属性

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

| 名称                  | 类型                                        | 可读 | 可写 | 说明                                                         | Android平台                                                | iOS平台                                                    |
| --------------------- | ------------------------------------------- | ---- | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| url                   | string                                      | 是   | 是   | 媒体URL，只允许在**idle**状态下设置。<br/>支持的视频格式(mp4、mpeg-ts、mkv)。<br>支持的音频格式(m4a、aac、mp3、ogg、wav、flac)。<br/>**支持路径示例**：<br>1. fd类型播放：fd://xx。<br>![](figures/zh-cn_image_url.png)<br>2. http网络播放: http\://xx。<br/>3. https网络播放: https\://xx。<br/>4. hls网络播放路径：http\://xx或者https\://xx。 | 支持 | 支持 |
| fdSrc                 | [AVFileDescriptor](#avfiledescriptor)       | 是   | 是   | 媒体文件描述，只允许在**idle**状态下设置。<br/>使用场景：应用中的媒体资源被连续存储在同一个文件中。<br/>支持的视频格式(mp4、mpeg-ts、mkv)。<br>支持的音频格式(m4a、aac、mp3、ogg、wav、flac)。<br/>**使用示例**：<br/>假设一个连续存储的媒体文件: <br/>视频1(地址偏移:0，字节长度:100)；<br/>视频2(地址偏移:101，字节长度:50)；<br/>视频3(地址偏移:151，字节长度:150)；<br/>1. 播放视频1：AVFileDescriptor { fd = 资源句柄; offset = 0; length = 100; }。<br/>2. 播放视频2：AVFileDescriptor { fd = 资源句柄; offset = 101; length = 50; }。<br/>3. 播放视频3：AVFileDescriptor { fd = 资源句柄; offset = 151; length = 150; }。<br/>假设是一个独立的媒体文件: 请使用src=fd://xx。 | 支持 | 支持 |
| dataSrc | [AVDataSrcDescriptor](#avdatasrcdescriptor) | 是   | 是   | 流式媒体资源描述，只允许在**idle**状态下设置。<br/>使用场景：应用播放从远端下载到本地的文件，在应用未下载完整音视频资源时，提前播放已获取的资源文件。<br/>支持的视频格式(mp4、mpeg-ts、mkv)。<br>支持的音频格式(m4a、aac、mp3、ogg、wav、flac)。<br/>**使用示例**：<br/>假设用户正在从远端服务器获取音视频媒体文件，希望下载到本地的同时播放已经下载好的部分: <br/>1.用户需要获取媒体文件的总大小size（单位为字节），获取不到时设置为-1。<br/>2.用户需要实现回调函数func用于填写数据，如果size = -1，则func形式为：func(buffer: ArrayBuffer, length: number)，此时播放器只会按照顺序获取数据；否则func形式为：func(buffer: ArrayBuffer, length: number, pos: number)，播放器会按需跳转并获取数据。<br/>3.用户设置AVDataSrcDescriptor {fileSize = size, callback = func}。<br/>**注意事项**：<br/>如果播放的是mp4/m4a格式用户需要保证moov字段（媒体信息字段）在mdat字段（媒体数据字段）之前，或者moov之前的字段小于10M，否则会导致解析失败无法播放。 | 支持 | 不支持 |
| surfaceId             | string                                      | 是   | 是   | 视频窗口ID，默认无窗口。<br/>支持在**initialized**状态下设置。<br/>支持在**prepared**/**playing**/**paused**/**completed**/**stopped**状态下重新设置，重新设置时确保已经在**initialized**状态下进行设置，否则重新设置失败，重新设置后视频播放在新的窗口渲染。<br/>使用场景：视频播放的窗口渲染，纯音频播放不用设置。 | 支持 | 支持 |
| loop                  | boolean                                     | 是   | 是   | 视频循环播放属性，默认'false'，设置为'true'表示循环播放，动态属性。<br/>只允许在**prepared**/**playing**/**paused**/**completed**状态下设置。<br/>直播场景不支持loop设置。 | 支持 | 支持 |
| videoScaleType        | [VideoScaleType](#videoscaletype)           | 是   | 是   | 视频缩放模式，默认VIDEO_SCALE_TYPE_FIT，动态属性。<br/>只允许在**prepared**/**playing**/**paused**/**completed**状态下设置。 | 支持 | 支持 |
| state                 | [AVPlayerState](#avplayerstate)            | 是   | 否   | 音视频播放的状态，全状态有效，可查询参数。                   | 支持                 | 支持                 |
| currentTime           | number                                      | 是   | 否   | 视频的当前播放位置，单位为毫秒（ms），可查询参数。<br/>返回为(-1)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。<br/>直播场景默认返回(-1)。 | 支持 | 支持 |
| duration              | number                                      | 是   | 否   | 视频时长，单位为毫秒（ms），可查询参数。<br/>返回为(-1)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。<br/>直播场景默认返回(-1)。 | 支持 | 支持 |
| width                 | number                                      | 是   | 否   | 视频宽，单位为像素（px），可查询参数。<br/>返回为(0)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。 | 支持 | 支持 |
| height                | number                                      | 是   | 否   | 视频高，单位为像素（px），可查询参数。<br/>返回为(0)表示无效值，**prepared**/**playing**/**paused**/**completed**状态下有效。 | 支持 | 支持 |

**说明：**

将资源句柄（fd）传递给媒体播放器之后，请不要通过该资源句柄做其他读写操作，包括但不限于将同一个资源句柄传递给多个媒体播放器。同一时间通过同一个资源句柄读写文件时存在竞争关系，将导致播放异常。

### OnAVPlayerStateChangeHandle

type OnAVPlayerStateChangeHandle = (state: AVPlayerState, reason: StateChangeReason) => void

回调函数类型，用于状态机切换事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                                    | 必填 | 说明                 |
| ------ | --------------------------------------- | ---- | -------------------- |
| state  | [AVPlayerState](#avplayerstate)         | 是   | 播放器状态。         |
| reason | [StateChangeReason](#statechangereason) | 是   | 播放器状态改变原因。 |

### OnBufferingUpdateHandler

type OnBufferingUpdateHandler = (infoType: BufferingInfoType, value: number) => void

回调函数类型，定义播放缓存事件回调。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                    | 必填 | 说明               |
| -------- | --------------------------------------- | ---- | ------------------ |
| infoType | [BufferingInfoType](#bufferinginfotype) | 是   | 缓冲事件类型。     |
| value    | number                                  | 是   | 缓存事件类型的值。 |

### OnVideoSizeChangeHandler

type OnVideoSizeChangeHandler = (width: number, height: number) => void

回调函数类型，视频播放宽高变化事件回调。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| width  | number | 是   | 视频播放宽度。 |
| height | number | 是   | 视频播放高度。 |

### on('stateChange') 

on(type: 'stateChange', callback: OnAVPlayerStateChangeHandle): void

监听播放状态机[AVPlayerState](#avplayerstate)切换的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                                         |
| -------- | --------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                      | 是   | 状态机切换事件回调类型，支持的事件：'stateChange'，用户操作和系统都会触发此事件。 |
| callback | OnAVPlayerStateChangeHandle | 是   | 状态机切换事件回调方法。                                     |

**示例：**

```ts
avPlayer.on('stateChange', async (state: string, reason: media.StateChangeReason) => {
  switch (state) {
    case 'idle':
      console.info('state idle called');
      break;
    case 'initialized':
      console.info('initialized prepared called');
      break;
    case 'prepared':
      console.info('state prepared called');
      break;
    case 'playing':
      console.info('state playing called');
      break;
    case 'paused':
      console.info('state paused called');
      break;
    case 'completed':
      console.info('state completed called');
      break;
    case 'stopped':
      console.info('state stopped called');
      break;
    case 'released':
      console.info('state released called');
      break;
    case 'error':
      console.info('state error called');
      break;
    default:
      console.info('unknown state :' + state);
      break;
  }
})
```

### off('stateChange')

off(type: 'stateChange', callback?: OnAVPlayerStateChangeHandle): void

取消监听播放状态机[AVPlayerState](#avplayerstate)切换的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                        | 必填 | 说明                                                    |
| -------- | --------------------------- | ---- | ------------------------------------------------------- |
| type     | string                      | 是   | 状态机切换事件回调类型，取消注册的事件：'stateChange'。 |
| callback | OnAVPlayerStateChangeHandle | 否   | 状态机切换事件回调方法。                                |

**示例：**

```ts
avPlayer.off('stateChange')
```

### on('error')

on(type: 'error', callback: ErrorCallback): void

监听[AVPlayer](#avplayer)的错误事件，该事件仅用于错误提示，不需要用户停止播控动作。如果此时[AVPlayerState](#avplayerstate)也切至error状态，用户需要通过[reset()](#reset)或者[release()](#release)退出播放操作。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型          | 必填 | 说明                                                         |
| -------- | ------------- | ---- | ------------------------------------------------------------ |
| type     | string        | 是   | 错误事件回调类型，支持的事件：'error'，用户操作和系统都会触发此事件。 |
| callback | ErrorCallback | 是   | 错误事件回调方法：使用播放器的过程中发生错误，会提供错误码ID和错误信息。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                       |
| -------- | ------------------------------ |
| 201      | Permission denied              |
| 401      | The parameter check failed.    |
| 801      | Capability not supported.      |
| 5400101  | No Memory. Return by callback. |
| 5400102  | Operation not allowed.         |
| 5400103  | I/O error                      |
| 5400104  | Time out                       |
| 5400105  | Service Died.                  |
| 5400106  | Unsupport Format.              |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.on('error', (error: BusinessError) => {
  console.info('error happened,and error message is :' + error.message)
  console.info('error happened,and error code is :' + error.code)
})
```

### off('error')

off(type: 'error', callback?: ErrorCallback): void

取消监听播放的错误事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型          | 必填 | 说明                                                         |
| -------- | ------------- | ---- | ------------------------------------------------------------ |
| type     | string        | 是   | 错误事件回调类型，取消注册的事件：'error'。                  |
| callback | ErrorCallback | 否   | 错误事件回调方法：使用播放器的过程中发生错误，会提供错误码ID和错误信息。 |

**示例：**

```ts
avPlayer.off('error')
```

### prepare

prepare(callback: AsyncCallback\<void>): void

通过回调方式准备播放音频/视频，需在[stateChange](#onstatechange)事件成功触发至initialized状态后，才能调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                 |
| -------- | -------------------- | ---- | -------------------- |
| callback | AsyncCallback\<void> | 是   | 准备播放的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |
| 5400106  | Unsupport format. Return by callback.      |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.prepare((err: BusinessError) => {
  if (err == null) {
    console.info('prepare success');
  } else {
    console.error('prepare filed,error message is :' + err.message)
  }
})
```

### prepare

prepare(): Promise\<void>

通过Promise方式准备播放音频/视频，需在[stateChange](#onstatechange)事件成功触发至initialized状态后，才能调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | 准备播放的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                  |
| -------- | ----------------------------------------- |
| 5400102  | Operation not allowed. Return by promise. |
| 5400106  | Unsupport format. Return by promise.      |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.prepare().then(() => {
  console.info('prepare success');
}, (err: BusinessError) => {
  console.error('prepare filed,error message is :' + err.message)
})
```

### play

play(callback: AsyncCallback\<void>): void

通过回调方式开始播放音视频资源，只能在prepared/paused/completed状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                 |
| -------- | -------------------- | ---- | -------------------- |
| callback | AsyncCallback\<void> | 是   | 开始播放的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.play((err: BusinessError) => {
  if (err == null) {
    console.info('play success');
  } else {
    console.error('play filed,error message is :' + err.message)
  }
})
```

### play

play(): Promise\<void>

通过Promise方式开始播放音视频资源，只能在prepared/paused/completed状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | 开始播放的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                  |
| -------- | ----------------------------------------- |
| 5400102  | Operation not allowed. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.play().then(() => {
  console.info('play success');
}, (err: BusinessError) => {
  console.error('play filed,error message is :' + err.message)
})
```

### pause

pause(callback: AsyncCallback\<void>): void

通过回调方式暂停播放音视频资源，只能在playing状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                 |
| -------- | -------------------- | ---- | -------------------- |
| callback | AsyncCallback\<void> | 是   | 暂停播放的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.pause((err: BusinessError) => {
  if (err == null) {
    console.info('pause success');
  } else {
    console.error('pause filed,error message is :' + err.message)
  }
})
```

### pause

pause(): Promise\<void>

通过Promise方式暂停播放音视频资源，只能在playing状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | 暂停播放的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                  |
| -------- | ----------------------------------------- |
| 5400102  | Operation not allowed. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.pause().then(() => {
  console.info('pause success');
}, (err: BusinessError) => {
  console.error('pause filed,error message is :' + err.message)
})
```

### stop

stop(callback: AsyncCallback\<void>): void

通过回调方式停止播放音视频资源，只能在prepared/playing/paused/completed状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                 |
| -------- | -------------------- | ---- | -------------------- |
| callback | AsyncCallback\<void> | 是   | 停止播放的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.stop((err: BusinessError) => {
  if (err == null) {
    console.info('stop success');
  } else {
    console.error('stop filed,error message is :' + err.message)
  }
})
```

### stop

stop(): Promise\<void>

通过Promise方式停止播放音视频资源，只能在prepared/playing/paused/completed状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | 停止播放的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                  |
| -------- | ----------------------------------------- |
| 5400102  | Operation not allowed. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.stop().then(() => {
  console.info('stop success');
}, (err: BusinessError) => {
  console.error('stop filed,error message is :' + err.message)
})
```

### reset

reset(callback: AsyncCallback\<void>): void

通过回调方式重置播放，只能在initialized/prepared/playing/paused/completed/stopped/error状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                 |
| -------- | -------------------- | ---- | -------------------- |
| callback | AsyncCallback\<void> | 是   | 重置播放的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.reset((err: BusinessError) => {
  if (err == null) {
    console.info('reset success');
  } else {
    console.error('reset filed,error message is :' + err.message)
  }
})
```

### reset

reset(): Promise\<void>

通过Promise方式重置播放，只能在initialized、prepared、playing、paused、completed、stopped、error状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | 重置播放的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                  |
| -------- | ----------------------------------------- |
| 5400102  | Operation not allowed. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.reset().then(() => {
  console.info('reset success');
}, (err: BusinessError) => {
  console.error('reset filed,error message is :' + err.message)
})
```

### release

release(callback: AsyncCallback\<void>): void

通过回调方式销毁播放资源，除released状态，都可以调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                 |
| -------- | -------------------- | ---- | -------------------- |
| callback | AsyncCallback\<void> | 是   | 销毁播放的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.release((err: BusinessError) => {
  if (err == null) {
    console.info('release success');
  } else {
    console.error('release filed,error message is :' + err.message)
  }
})
```

### release

release(): Promise\<void>

通过Promise方式销毁播放，除released状态，都可以调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                      |
| -------------- | ------------------------- |
| Promise\<void> | 销毁播放的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                  |
| -------- | ----------------------------------------- |
| 5400102  | Operation not allowed. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.release().then(() => {
  console.info('release success');
}, (err: BusinessError) => {
  console.error('release filed,error message is :' + err.message)
})
```

### getTrackDescription

getTrackDescription(callback: AsyncCallback\<Array\<MediaDescription>>): void

通过回调方式获取音视频轨道信息，可以在prepared/playing/paused状态调用。获取所有音视轨道信息，应在数据加载回调后调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                        | 必填 | 说明                                         |
| -------- | ----------------------------------------------------------- | ---- | -------------------------------------------- |
| callback | AsyncCallback<Array<[MediaDescription](#mediadescription)>> | 是   | 音视频轨道信息MediaDescription数组回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.getTrackDescription((error: BusinessError, arrList: Array<media.MediaDescription>) => {
  if ((arrList) != null) {
    console.info('getTrackDescription success');
  } else {
    console.error(`video getTrackDescription fail, error:${error}`);
  }
});
```

### getTrackDescription

getTrackDescription(): Promise\<Array\<MediaDescription>>

通过Promise方式获取音视频轨道信息，可以在prepared/playing/paused状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**返回值：**

| 类型                                                  | 说明                                              |
| ----------------------------------------------------- | ------------------------------------------------- |
| Promise<Array<[MediaDescription](#mediadescription)>> | 音视频轨道信息MediaDescription数组Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                  |
| -------- | ----------------------------------------- |
| 5400102  | Operation not allowed. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avPlayer.getTrackDescription().then((arrList: Array<media.MediaDescription>) => {
  console.info('getTrackDescription success');
}).catch((error: BusinessError) => {
  console.error(`video catchCallback, error:${error}`);
});
```

### seek

seek(timeMs: number, mode?:SeekMode): void

跳转到指定播放位置，只能在prepared/playing/paused/completed状态调用，可以通过[seekDone事件](#onseekdone)确认是否生效。注：直播场景不支持seek。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android，iOS。注：参数SeekMode不支持iOS平台。

**参数：**

| 参数名 | 类型                  | 必填 | 说明                                                         |
| ------ | --------------------- | ---- | ------------------------------------------------------------ |
| timeMs | number                | 是   | 指定的跳转时间节点，单位毫秒（ms），取值范围为[0, [duration](#属性)]。 |
| mode   | [SeekMode](#seekmode) | 否   | 基于视频I帧的跳转模式，默认为SEEK_PREV_SYNC模式，**仅在视频资源播放时设置**。 |

**示例：**

```ts
let seekTime: number = 1000
avPlayer.seek(seekTime, media.SeekMode.SEEK_PREV_SYNC)
```

### on('seekDone')

on(type: 'seekDone', callback: Callback\<number>): void

监听seek生效的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型              | 必填 | 说明                                                         |
| -------- | ----------------- | ---- | ------------------------------------------------------------ |
| type     | string            | 是   | seek生效的事件回调类型，支持的事件：'seekDone'，每次调用seek后都会回调此事件。 |
| callback | Callback\<number> | 是   | seek生效的事件回调方法，只会上报用户请求的time位置。<br/>**视频播放：**[SeekMode](#seekmode)会造成实际跳转位置与用户设置产生偏差，精准位置需要通过currentTime获取，事件回调的time仅代表完成用户某一次请求。 |

**示例：**

```ts
avPlayer.on('seekDone', (seekDoneTime:number) => {
  console.info('seekDone success,and seek time is:' + seekDoneTime)
})
```

### off('seekDone')

off(type: 'seekDone'): void

取消监听seek生效的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                 |
| ------ | ------ | ---- | ---------------------------------------------------- |
| type   | string | 是   | seek生效的事件回调类型，取消注册的事件：'seekDone'。 |

**示例：**

```ts
avPlayer.off('seekDone')
```

### setSpeed

setSpeed(speed: PlaybackSpeed): void

设置倍速模式，只能在prepared/playing/paused/completed状态调用，可以通过[speedDone事件](#onspeeddone)确认是否生效。注：直播场景不支持setSpeed。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明               |
| ------ | ------------------------------- | ---- | ------------------ |
| speed  | [PlaybackSpeed](#playbackspeed) | 是   | 指定播放倍速模式。 |

**示例：**

```ts
avPlayer.setSpeed(media.PlaybackSpeed.SPEED_FORWARD_2_00_X)
```

### on('speedDone')

on(type: 'speedDone', callback: Callback\<number>): void

监听setSpeed生效的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型              | 必填 | 说明                                                         |
| -------- | ----------------- | ---- | ------------------------------------------------------------ |
| type     | string            | 是   | setSpeed生效的事件回调类型，支持的事件：'speedDone'，每次调用setSpeed后都会回调此事件。 |
| callback | Callback\<number> | 是   | setSpeed生效的事件回调方法，上报生效的倍速模式，具体见[PlaybackSpeed](#playbackspeed)。 |

**示例：**

```ts
avPlayer.on('speedDone', (speed:number) => {
  console.info('speedDone success,and speed value is:' + speed)
})
```

### off('speedDone')

off(type: 'speedDone'): void

取消监听setSpeed生效的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                      |
| ------ | ------ | ---- | --------------------------------------------------------- |
| type   | string | 是   | setSpeed生效的事件回调类型，取消注册的事件：'speedDone'。 |

**示例：**

```ts
avPlayer.off('speedDone')
```

### setBitrate

setBitrate(bitrate: number): void

选择要播放的指定比特率，仅对**HLS协议网络流**有效，默认情况下，播放器会根据网络连接速度选择合适的比特率，只能在prepared/playing/paused/completed状态调用。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android

**参数：**

| 参数名  | 类型   | 必填 | 说明                                                         |
| ------- | ------ | ---- | ------------------------------------------------------------ |
| bitrate | number | 是   | 指定比特率，如果用户指定的比特率不在当前HLS协议流可用的比特率列表中，则播放器将从可用比特率列表中选择最小和最接近的比特率。 |

**示例：**

```ts
let bitrate: number = 96000
avPlayer.setBitrate(bitrate)
```

### setVolume

setVolume(volume: number): void

设置媒体播放音量，只能在prepared/playing/paused/completed状态调用，可以通过[volumeChange事件](#onvolumechange)确认是否生效。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| volume | number | 是   | 指定的相对音量大小，取值范围为[0.00-1.00]，1表示最大音量，即100%。 |

**示例：**

```ts
let volume: number = 1.0
avPlayer.setVolume(volume)
```

### on('volumeChange')

on(type: 'volumeChange', callback: Callback\<number>): void

监听setVolume生效的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型              | 必填 | 说明                                                         |
| -------- | ----------------- | ---- | ------------------------------------------------------------ |
| type     | string            | 是   | setVolume生效的事件回调类型，支持的事件：'volumeChange'，每次调用setVolume后都会回调此事件。 |
| callback | Callback\<number> | 是   | setVolume生效的事件回调方法，上报生效的媒体音量。            |

**示例：**

```ts
avPlayer.on('volumeChange', (vol: number) => {
  console.info('volumeChange success,and new volume is :' + vol)
})
```

### off('volumeChange')

off(type: 'volumeChange'): void

取消监听setVolume生效的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| type   | string | 是   | setVolume生效的事件回调类型，取消注册的事件：'volumeChange'。 |

**示例：**

```ts
avPlayer.off('volumeChange')
```

### on('endOfStream')

on(type: 'endOfStream', callback: Callback\<void>): void

监听资源播放至结尾的事件；如果用户设置[loop](#属性)=true，播放会跳转至开头重播；如果用户没有设置loop，会通过[stateChange](#onstatechange)上报completed状态。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型            | 必填 | 说明                                                         |
| -------- | --------------- | ---- | ------------------------------------------------------------ |
| type     | string          | 是   | 资源播放至结尾的事件回调类型，支持的事件：'endOfStream'，当播放至结尾时会上报此事件。 |
| callback | Callback\<void> | 是   | 资源播放至结尾的事件回调方法。                               |

**示例：**

```ts
avPlayer.on('endOfStream', () => {
  console.info('endOfStream success')
})
```

### off('endOfStream')

off(type: 'endOfStream'): void

取消监听资源播放至结尾的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                         |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| type   | string | 是   | 资源播放至结尾的事件回调类型，取消注册的事件：'endOfStream'。 |

**示例：**

```ts
avPlayer.off('endOfStream')
```

### on('timeUpdate')

on(type: 'timeUpdate', callback: Callback\<number>): void

监听资源播放当前时间，单位为毫秒（ms），用于刷新进度条当前位置，默认间隔100ms时间上报，因用户操作(seek)产生的时间变化会立刻上报。
注：直播场景不支持timeUpdate上报。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型              | 必填 | 说明                                           |
| -------- | ----------------- | ---- | ---------------------------------------------- |
| type     | string            | 是   | 时间更新的回调类型，支持的事件：'timeUpdate'。 |
| callback | Callback\<number> | 是   | 当前时间。                                     |

**示例：**

```ts
avPlayer.on('timeUpdate', (time:number) => {
  console.info('timeUpdate success,and new time is :' + time)
})
```

### off('timeUpdate')

off(type: 'timeUpdate'): void

取消监听资源播放当前时间。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                               |
| ------ | ------ | ---- | -------------------------------------------------- |
| type   | string | 是   | 时间更新的回调类型，取消注册的事件：'timeUpdate'。 |

**示例：**

```ts
avPlayer.off('timeUpdate')
```

### on('durationUpdate')


on(type: 'durationUpdate', callback: Callback\<number>): void

监听资源播放资源的时长，单位为毫秒（ms），用于刷新进度条长度，默认只在prepared上报一次，同时允许一些特殊码流刷新多次时长。
注：直播场景不支持durationUpdate上报。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型              | 必填 | 说明                                               |
| -------- | ----------------- | ---- | -------------------------------------------------- |
| type     | string            | 是   | 时长更新的回调类型，支持的事件：'durationUpdate'。 |
| callback | Callback\<number> | 是   | 资源时长。                                         |

**示例：**

```ts
avPlayer.on('durationUpdate', (duration: number) => {
  console.info('durationUpdate success,new duration is :' + duration)
})
```

### off('durationUpdate')

off(type: 'durationUpdate'): void

取消监听资源播放资源的时长。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                   |
| ------ | ------ | ---- | ------------------------------------------------------ |
| type   | string | 是   | 时长更新的回调类型，取消注册的事件：'durationUpdate'。 |

**示例：**

```ts
avPlayer.off('durationUpdate')
```

### on('bufferingUpdate')

on(type: 'bufferingUpdate', callback: OnBufferingUpdateHandler): void

订阅音视频缓存更新事件，仅网络播放支持该订阅事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android

**参数：**

| 参数名   | 类型                                                  | 必填 | 说明                                                  |
| -------- | ----------------------------------------------------- | ---- | ----------------------------------------------------- |
| type     | string                                                | 是   | 播放缓存事件回调类型，支持的事件：'bufferingUpdate'。 |
| callback | [OnBufferingUpdateHandler](#onbufferingupdatehandler) | 是   | 播放缓存事件回调方法。                                |

**示例：**

```ts
avPlayer.on('bufferingUpdate', (infoType: media.BufferingInfoType, value: number) => {
  console.info('bufferingUpdate success,and infoType value is:' + infoType + ', value is :' + value)
})
```

### off('bufferingUpdate')

off(type: 'bufferingUpdate', callback?: OnBufferingUpdateHandler): void

取消监听音视频缓存更新事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android

**参数：**

| 参数名   | 类型                                                  | 必填 | 说明                                                      |
| -------- | ----------------------------------------------------- | ---- | --------------------------------------------------------- |
| type     | string                                                | 是   | 播放缓存事件回调类型，取消注册的事件：'bufferingUpdate'。 |
| callback | [OnBufferingUpdateHandler](#onbufferingupdatehandler) | 否   | 播放缓存事件回调方法。                                    |

**示例：**

```ts
avPlayer.off('bufferingUpdate')
```

### on('startRenderFrame')

on(type: 'startRenderFrame', callback: Callback\<void>): void

订阅视频播放开始首帧渲染的更新事件，仅视频播放支持该订阅事件，该事件仅代表播放服务将第一帧画面送显示模块，实际效果依赖显示服务渲染性能。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android

**参数：**

| 参数名   | 类型            | 必填 | 说明                                                         |
| -------- | --------------- | ---- | ------------------------------------------------------------ |
| type     | string          | 是   | 视频播放开始首帧渲染事件回调类型，支持的事件：'startRenderFrame'。 |
| callback | Callback\<void> | 是   | 视频播放开始首帧渲染事件回调方法。                           |

**示例：**

```ts
avPlayer.on('startRenderFrame', () => {
  console.info('startRenderFrame success')
})
```

### off('startRenderFrame')

off(type: 'startRenderFrame', callback?: Callback\<void>): void

取消监听视频播放开始首帧渲染的更新事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android

**参数：**

| 参数名   | 类型            | 必填 | 说明                                                         |
| -------- | --------------- | ---- | ------------------------------------------------------------ |
| type     | string          | 是   | 视频播放开始首帧渲染事件回调类型，取消注册的事件：'startRenderFrame'。 |
| callback | Callback\<void> | 否   | 视频播放开始首帧渲染事件回调方法。                           |

**示例：**

```ts
avPlayer.off('startRenderFrame')
```

### on('videoSizeChange')

on(type: 'videoSizeChange', callback: OnVideoSizeChangeHandler): void

监听视频播放宽高变化事件，仅视频播放支持该订阅事件，默认只在prepared状态上报一次，但HLS协议码流会在切换分辨率时上报；

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                  | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                                | 是   | 视频播放宽高变化事件回调类型，支持的事件：'videoSizeChange'。 |
| callback | [OnVideoSizeChangeHandler](#onvideosizechangehandler) | 是   | 视频播放宽高变化事件回调方法。                               |

**示例：**

```ts
avPlayer.on('videoSizeChange', (width: number, height: number) => {
  console.info('videoSizeChange success,and width is:' + width + ', height is :' + height)
})
```

### off('videoSizeChange')

off(type: 'videoSizeChange', callback?: OnVideoSizeChangeHandler): void

取消监听视频播放宽高变化事件。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                  | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                                | 是   | 视频播放宽高变化事件回调类型，取消注册的事件：'videoSizeChange'。 |
| callback | [OnVideoSizeChangeHandler](#onvideosizechangehandler) | 否   | 视频播放宽高变化事件回调方法。                               |

**示例：**

```ts
avPlayer.off('videoSizeChange')
```

## AVPlayerState

[AVPlayer](#avplayer)的状态机，可通过state属性主动获取当前状态，也可通过监听[stateChange](#onstatechange)事件上报当前状态，状态机之间的切换规则。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

|    名称     |  类型  | 说明                                                         | Android平台 | iOS平台 |
| :---------: | :----: | :----------------------------------------------------------- | ----------- | ------- |
|    idle     | string | 闲置状态，AVPlayer刚被创建[createAVPlayer()](#mediacreateavplayer)或者调用了[reset()](#reset)方法之后，进入Idle状态。<br/>首次创建[createAVPlayer()](#mediacreateavplayer)，所有属性都为默认值。<br/>调用[reset()](#reset)方法，url 或 fdSrc或dataSrc属性及loop属性会被重置，其他用户设置的属性将被保留。 | 支持        | 支持    |
| initialized | string | 资源初始化，在Idle 状态设置 url 或 fdSrc属性，AVPlayer会进入initialized状态，此时可以配置窗口、音频等静态属性。 | 支持        | 支持    |
|  prepared   | string | 已准备状态，在initialized状态调用[prepare()](#prepare)方法，AVPlayer会进入prepared状态，此时播放引擎的资源已准备就绪。 | 支持        | 支持    |
|   playing   | string | 正在播放状态，在prepared/paused/completed状态调用[play()](#play)方法，AVPlayer会进入playing状态。 | 支持        | 支持    |
|   paused    | string | 暂停状态，在playing状态调用pause方法，AVPlayer会进入paused状态。 | 支持        | 支持    |
|  completed  | string | 播放至结尾状态，当媒体资源播放至结尾时，如果用户未设置循环播放（loop = true），AVPlayer会进入completed状态，此时调用[play()](#play)会进入playing状态和重播，调用[stop()](#stop)会进入stopped状态。 | 支持        | 支持    |
|   stopped   | string | 停止状态，在prepared/playing/paused/completed状态调用[stop()](#stop)方法，AVPlayer会进入stopped状态，此时播放引擎只会保留属性，但会释放内存资源，可以调用[prepare()](#prepare)重新准备，也可以调用[reset()](#reset)重置，或者调用[release()](#release)彻底销毁。 | 支持        | 支持    |
|  released   | string | 销毁状态，销毁与当前AVPlayer关联的播放引擎，无法再进行状态转换，调用[release()](#release)方法后，会进入released状态，结束流程。 | 支持        | 支持    |
|    error    | string | 错误状态，当**播放引擎**发生**不可逆的错误**（详见[媒体错误码](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-media-kit/errorcode-media.md)），则会转换至当前状态，可以调用[reset()](#reset)重置，也可以调用[release()](#release)销毁重建。<br/>**注意：** 区分error状态和 [on('error')](#onerror) ：<br/>1、进入error状态时，会触发on('error')监听事件，可以通过on('error')事件获取详细错误信息；<br/>2、处于error状态时，播放服务进入不可播控的状态，要求客户端设计容错机制，使用[reset()](#reset)重置或者[release()](#release)销毁重建；<br/>3、如果客户端收到on('error')，但未进入error状态：<br/>原因1：客户端未按状态机调用API或传入参数错误，被AVPlayer拦截提醒，需要客户端调整代码逻辑；<br/>原因2：播放过程发现码流问题，导致容器、解码短暂异常，不影响连续播放和播控操作的，不需要客户端设计容错机制。 | 支持        | 支持    |

## AVFileDescriptor

音视频文件资源描述，一种特殊资源的播放方式，使用场景：应用中的音频资源被连续存储在同一个文件中，需要根据偏移量和长度进行播放。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称   | 类型   | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------ | ------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| fd     | number | 是   | 资源句柄，通过[resourceManager.getRawFd](./js-apis-resource-manager.md#getrawfd9)获取。 | 支持        | 支持    |
| offset | number | 否   | 资源偏移量，默认值为0，需要基于预置资源的信息输入，非法值会造成音视频资源解析错误。 | 支持        | 支持    |
| length | number | 否   | 资源长度，默认值为文件中从偏移量开始的剩余字节，需要基于预置资源的信息输入，非法值会造成音视频资源解析错误。 | 支持        | 支持    |

## AVDataSrcDescriptor

音视频文件资源描述，用于DataSource播放方式，使用场景：应用在未获取完整音视频资源时，允许用户创建播放实例并开始播放，达到提前播放的目的。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

| 名称     | 类型     | 必填 | 说明                                                         | Android平台 | iOS平台 |
| -------- | -------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| fileSize | number   | 是   | 待播放文件大小（字节），-1代表大小未知。如果fileSize设置为-1, 播放模式类似于直播，不能进行seek及setSpeed操作，不能设置loop属性，因此不能重新播放。 | 支持        | 支持    |
| callback | function | 是   | 用户设置的回调函数，用于填写数据。<br>- 函数列式：callback: (buffer: ArrayBuffer, length: number, pos?:number) => number;<br>- buffer，ArrayBuffer类型，表示被填写的内存，必选。<br>- length，number类型，表示被填写内存的最大长度，必选。<br>- pos，number类型，表示填写的数据在资源文件中的位置，可选，当fileSize设置为-1时，该参数禁止被使用。 <br>- 返回值，number类型，返回要填充数据的长度。 | 支持        | 支持    |


## SeekMode

枚举值，定义视频播放的Seek模式，可通过seek方法作为参数传递下去。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称           | 值   | 说明                                                         | Android平台 | iOS平台 |
| -------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| SEEK_NEXT_SYNC | 0    | 表示跳转到指定时间点的下一个关键帧，建议向后快进的时候用这个枚举值。 | 支持        | 不支持  |
| SEEK_PREV_SYNC | 1    | 表示跳转到指定时间点的上一个关键帧，建议向前快进的时候用这个枚举值。 | 支持        | 不支持  |

## PlaybackSpeed

枚举值，定义视频播放的倍速，可通过setSpeed方法作为参数传递下去。

**系统能力：** SystemCapability.Multimedia.Media.VideoPlayer

| 名称                 | 值   | 说明                           | Android平台 | iOS平台 |
| -------------------- | ---- | ------------------------------ | ----------- | ------- |
| SPEED_FORWARD_0_75_X | 0    | 表示视频播放正常播速的0.75倍。 | 支持        | 支持    |
| SPEED_FORWARD_1_00_X | 1    | 表示视频播放正常播速。         | 支持        | 支持    |
| SPEED_FORWARD_1_25_X | 2    | 表示视频播放正常播速的1.25倍。 | 支持        | 支持    |
| SPEED_FORWARD_1_75_X | 3    | 表示视频播放正常播速的1.75倍。 | 支持        | 支持    |
| SPEED_FORWARD_2_00_X | 4    | 表示视频播放正常播速的2.00倍。 | 支持        | 支持    |

## VideoScaleType

枚举值，定义视频缩放模式。

**系统能力：** SystemCapability.Multimedia.Media.VideoPlayer

| 名称                      | 值   | 说明                                             | Android平台 | iOS平台 |
| ------------------------- | ---- | ------------------------------------------------ | ----------- | ------- |
| VIDEO_SCALE_TYPE_FIT      | 0    | 默认比例类型，视频拉伸至与窗口等大。             | 支持        | 支持    |
| VIDEO_SCALE_TYPE_FIT_CROP | 1    | 保持视频宽高比拉伸至填满窗口，内容可能会有裁剪。 | 支持        | 支持    |

## MediaDescription

通过key-value方式获取媒体信息。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称          | 类型   | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------- | ------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| [key: string] | Object | 是   | 该键值对支持的key取值范围，请参考[MediaDescriptionKey](#mediadescriptionkey);每个key值的Object类型和范围，请参考[MediaDescriptionKey](#mediadescriptionkey)对应Key值的说明 | 支持        | 支持    |

**示例：**

```ts
import { BusinessError } from '@ohos.base';
import media from '@ohos.multimedia.media';

function printfItemDescription(obj: media.MediaDescription, key: string) {
  let property: Object = obj[key];
  console.info('audio key is ' + key); // 通过key值获取对应的value。key值具体可见[MediaDescriptionKey]
  console.info('audio value is ' + property); // 对应key值得value。其类型可为任意类型，具体key对应value的类型可参考[MediaDescriptionKey]
}

let avPlayer: media.AVPlayer | undefined = undefined;
media.createAVPlayer((err: BusinessError, player: media.AVPlayer) => {
  if(player != null) {
    avPlayer = player;
    console.info(`createAVPlayer success`);
    avPlayer.getTrackDescription((error: BusinessError, arrList: Array<media.MediaDescription>) => {
      if (arrList != null) {
        for (let i = 0; i < arrList.length; i++) {
          printfItemDescription(arrList[i], media.MediaDescriptionKey.MD_KEY_TRACK_TYPE);  // 打印出每条轨道MD_KEY_TRACK_TYPE的值
        }
      } else {
        console.error(`audio getTrackDescription fail, error:${error}`);
      }
    });
  } else {
    console.error(`createAVPlayer fail, error message:${err.message}`);
  }
});
```

## AVRecorder

音视频录制管理类，用于音视频媒体录制。在调用AVRecorder的方法前，需要先通过[createAVRecorder()](#mediacreateavrecorder)构建一个AVRecorder实例。

### 属性

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

| 名称  | 类型                                | 可读 | 可写 | 说明               | Android平台 | iOS平台 |
| ----- | ----------------------------------- | ---- | ---- | ------------------ | ----------- | ------- |
| state | [AVRecorderState](#avrecorderstate) | 是   | 否   | 音视频录制的状态。 | 支持        | 支持    |

### AVRecorderState

type AVRecorderState = 'idle' | 'prepared' | 'started' | 'paused' | 'stopped' | 'released' | 'error'

音视频录制的状态。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**状态：**

| 状态           | 类型   | 说明           |
| -------------- | ------ | -------------- |
| &#39;idle&#39; | string | 空闲状态。     |
| 'prepared'     | string | 准备状态。     |
| 'started'      | string | 开始录制状态。 |
| 'paused'       | string | 暂停录制状态。 |
| 'stopped'      | string | 结束录制状态。 |
| 'released'     | string | 销毁状态。     |
| 'error'        | string | 出错状态。     |

### OnAVRecorderStateChangeHandler

type OnAVRecorderStateChangeHandler = (state: AVRecorderState, reason: StateChangeReason) => void

回调函数类型，用于状态机切换事件回调。

**系统能力：** SystemCapability.Multimedia.Media.AVPlayer

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                                    | 必填 | 说明                         |
| ------ | --------------------------------------- | ---- | ---------------------------- |
| state  | [AVRecorderState](#avrecorderstate)     | 是   | 表示当前播放状态。           |
| reason | [StateChangeReason](#statechangereason) | 是   | 表示当前播放状态的切换原因。 |

### prepare

prepare(config: AVRecorderConfig, callback: AsyncCallback\<void>): void

异步方式进行音视频录制的参数设置。通过注册回调函数获取返回值。

**需要权限：** ohos.permission.MICROPHONE

不涉及音频录制时，可以不需要获取ohos.permission.MICROPHONE权限。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                  | 必填 | 说明                                  |
| -------- | ------------------------------------- | ---- | ------------------------------------- |
| config   | [AVRecorderConfig](#avrecorderconfig) | 是   | 配置音视频录制的相关参数。            |
| callback | AsyncCallback\<void>                  | 是   | 异步音视频录制prepare方法的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                |
| -------- | --------------------------------------- |
| 201      | Permission denied. Return by callback.  |
| 401      | Parameter error. Return by callback.    |
| 5400102  | Operate not permit. Return by callback. |
| 5400105  | Service died. Return by callback.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

// 配置参数以实际硬件设备支持的范围为准
let avRecorderProfile: media.AVRecorderProfile = {
  audioBitrate : 48000,
  audioChannels : 2,
  audioCodec : media.CodecMimeType.AUDIO_AAC,
  audioSampleRate : 48000,
  fileFormat : media.ContainerFormatType.CFT_MPEG_4,
  videoBitrate : 2000000,
  videoCodec : media.CodecMimeType.VIDEO_AVC,
  videoFrameWidth : 640,
  videoFrameHeight : 480,
  videoFrameRate : 30
}
let avRecorderConfig: media.AVRecorderConfig = {
  audioSourceType : media.AudioSourceType.AUDIO_SOURCE_TYPE_MIC,
  videoSourceType : media.VideoSourceType.VIDEO_SOURCE_TYPE_SURFACE_YUV,
  profile : avRecorderProfile,
  url : 'fd://', // 文件需先由调用者创建，赋予读写权限，将文件fd传给此参数，eg.fd://45
  rotation : 0, // 合理值0、90、180、270，非合理值prepare接口将报错
  location : { latitude : 30, longitude : 130 }
}

avRecorder.prepare(avRecorderConfig, (err: BusinessError) => {
  if (err == null) {
    console.info('prepare success');
  } else {
    console.error('prepare failed and error is ' + err.message);
  }
})
```

### prepare

prepare(config: AVRecorderConfig): Promise\<void>

异步方式进行音视频录制的参数设置。通过Promise获取返回值。

**需要权限：** ohos.permission.MICROPHONE

不涉及音频录制时，可以不需要获ohos.permission.MICROPHONE权限。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                                  | 必填 | 说明                       |
| ------ | ------------------------------------- | ---- | -------------------------- |
| config | [AVRecorderConfig](#avrecorderconfig) | 是   | 配置音视频录制的相关参数。 |

**返回值：**

| 类型           | 说明                                       |
| -------------- | ------------------------------------------ |
| Promise\<void> | 异步音视频录制prepare方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 201      | Permission denied. Return by promise.  |
| 401      | Parameter error. Return by promise.    |
| 5400102  | Operate not permit. Return by promise. |
| 5400105  | Service died. Return by promise.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

// 配置参数以实际硬件设备支持的范围为准
let avRecorderProfile: media.AVRecorderProfile = {
  audioBitrate : 48000,
  audioChannels : 2,
  audioCodec : media.CodecMimeType.AUDIO_AAC,
  audioSampleRate : 48000,
  fileFormat : media.ContainerFormatType.CFT_MPEG_4,
  videoBitrate : 2000000,
  videoCodec : media.CodecMimeType.VIDEO_AVC,
  videoFrameWidth : 640,
  videoFrameHeight : 480,
  videoFrameRate : 30
}
let avRecorderConfig: media.AVRecorderConfig = {
  audioSourceType : media.AudioSourceType.AUDIO_SOURCE_TYPE_MIC,
  videoSourceType : media.VideoSourceType.VIDEO_SOURCE_TYPE_SURFACE_YUV,
  profile : avRecorderProfile,
  url : 'fd://',  // 文件需先由调用者创建，赋予读写权限，将文件fd传给此参数，eg.fd://45
  rotation : 0, // 合理值0、90、180、270，非合理值prepare接口报错
  location : { latitude : 30, longitude : 130 }
}

avRecorder.prepare(avRecorderConfig).then(() => {
  console.info('prepare success');
}).catch((err: BusinessError) => {
  console.error('prepare failed and catch error is ' + err.message);
});
```

### getInputSurface

getInputSurface(callback: AsyncCallback\<string>): void

异步方式获得录制需要的surface。通过注册回调函数获取返回值。此surface提供给调用者，调用者从此surface中获取surfaceBuffer，填入相应的视频数据。

应当注意，填入的视频数据需要携带时间戳（单位ns）和buffersize。时间戳的起始时间请以系统启动时间为基准。

需在[prepare()](#prepare-3)事件成功触发后，才能调用getInputSurface()方法。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                   | 必填 | 说明                        |
| -------- | ---------------------- | ---- | --------------------------- |
| callback | AsyncCallback\<string> | 是   | 异步获得surface的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                |
| -------- | --------------------------------------- |
| 5400102  | Operate not permit. Return by callback. |
| 5400103  | IO error. Return by callback.           |
| 5400105  | Service died. Return by callback.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';
let surfaceID: string; // 该surfaceID用于传递给相机接口创造videoOutput

avRecorder.getInputSurface((err: BusinessError, surfaceId: string) => {
  if (err == null) {
    console.info('getInputSurface success');
    surfaceID = surfaceId;
  } else {
    console.error('getInputSurface failed and error is ' + err.message);
  }
});

```

### getInputSurface

getInputSurface(): Promise\<string>

异步方式获得录制需要的surface。通过Promise获取返回值。此surface提供给调用者，调用者从此surface中获取surfaceBuffer，填入相应的视频数据。

应当注意，填入的视频数据需要携带时间戳（单位ns）和buffersize。时间戳的起始时间请以系统启动时间为基准。

需在[prepare()](#prepare-3)事件成功触发后，才能调用getInputSurface方法。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型             | 说明                             |
| ---------------- | -------------------------------- |
| Promise\<string> | 异步获得surface的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 5400102  | Operate not permit. Return by promise. |
| 5400103  | IO error. Return by promise.           |
| 5400105  | Service died. Return by promise.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';
let surfaceID: string; // 该surfaceID用于传递给相机接口创造videoOutput

avRecorder.getInputSurface().then((surfaceId: string) => {
  console.info('getInputSurface success');
  surfaceID = surfaceId;
}).catch((err: BusinessError) => {
  console.error('getInputSurface failed and catch error is ' + err.message);
});
```

### start

start(callback: AsyncCallback\<void>): void

异步方式开始视频录制。通过注册回调函数获取返回值。

纯音频录制需在[prepare()](#prepare-3)事件成功触发后，才能调用start方法。纯视频录制，音视频录制需在[getInputSurface()](#getinputsurface)事件成功触发后，才能调用start方法。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                         |
| -------- | -------------------- | ---- | ---------------------------- |
| callback | AsyncCallback\<void> | 是   | 异步开始视频录制的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                |
| -------- | --------------------------------------- |
| 5400102  | Operate not permit. Return by callback. |
| 5400103  | IO error. Return by callback.           |
| 5400105  | Service died. Return by callback.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.start((err: BusinessError) => {
  if (err == null) {
    console.info('start AVRecorder success');
  } else {
    console.error('start AVRecorder failed and error is ' + err.message);
  }
});
```

### start

start(): Promise\<void>

异步方式开始视频录制。通过Promise获取返回值。

纯音频录制需在[prepare()](#prepare-3)事件成功触发后，才能调用start方法。纯视频录制，音视频录制需在[getInputSurface()](#getinputsurface)事件成功触发后，才能调用start方法。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                                  |
| -------------- | ------------------------------------- |
| Promise\<void> | 异步开始视频录制方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 5400102  | Operate not permit. Return by promise. |
| 5400103  | IO error. Return by promise.           |
| 5400105  | Service died. Return by promise.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.start().then(() => {
  console.info('start AVRecorder success');
}).catch((err: BusinessError) => {
  console.error('start AVRecorder failed and catch error is ' + err.message);
});
```

### pause

pause(callback: AsyncCallback\<void>): void

异步方式暂停视频录制。通过注册回调函数获取返回值。

需要[start()](#start)事件成功触发后，才能调用pause方法，可以通过调用[resume()](#resume)接口来恢复录制。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                        |
| -------- | -------------------- | ---- | --------------------------- |
| callback | AsyncCallback\<void> | 是   | 异步获得surface的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                |
| -------- | --------------------------------------- |
| 5400102  | Operate not permit. Return by callback. |
| 5400103  | IO error. Return by callback.           |
| 5400105  | Service died. Return by callback.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.pause((err: BusinessError) => {
  if (err == null) {
    console.info('pause AVRecorder success');
  } else {
    console.error('pause AVRecorder failed and error is ' + err.message);
  }
});
```

### pause

pause(): Promise\<void>

异步方式暂停视频录制。通过Promise获取返回值。

需要[start()](#start)事件成功触发后，才能调用pause方法，可以通过调用[resume()](#resume)接口来恢复录制。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                                  |
| -------------- | ------------------------------------- |
| Promise\<void> | 异步暂停视频录制方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 5400102  | Operate not permit. Return by promise. |
| 5400103  | IO error. Return by promise.           |
| 5400105  | Service died. Return by promise.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.pause().then(() => {
  console.info('pause AVRecorder success');
}).catch((err: BusinessError) => {
  console.error('pause AVRecorder failed and catch error is ' + err.message);
});
```

### resume

resume(callback: AsyncCallback\<void>): void

异步方式恢复视频录制。通过注册回调函数获取返回值。

需要在[pause()](#pause-3)事件成功触发后，才能调用resume方法。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                         |
| -------- | -------------------- | ---- | ---------------------------- |
| callback | AsyncCallback\<void> | 是   | 异步恢复视频录制的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                |
| -------- | --------------------------------------- |
| 5400102  | Operate not permit. Return by callback. |
| 5400103  | IO error. Return by callback.           |
| 5400105  | Service died. Return by callback.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.resume((err: BusinessError) => {
  if (err == null) {
    console.info('resume AVRecorder success');
  } else {
    console.error('resume AVRecorder failed and error is ' + err.message);
  }
});
```

### resume

resume(): Promise\<void>

异步方式恢复视频录制。通过Promise获取返回值。

需要在[pause()](#pause-3)事件成功触发后，才能调用resume方法。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                                  |
| -------------- | ------------------------------------- |
| Promise\<void> | 异步恢复视频录制方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 5400102  | Operate not permit. Return by promise. |
| 5400103  | IO error. Return by promise.           |
| 5400105  | Service died. Return by promise.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.resume().then(() => {
  console.info('resume AVRecorder success');
}).catch((err: BusinessError) => {
  console.error('resume AVRecorder failed and catch error is ' + err.message);
});
```

### stop

stop(callback: AsyncCallback\<void>): void

异步方式停止视频录制。通过注册回调函数获取返回值。

需要在[start()](#start)或[pause()](#pause-3)事件成功触发后，才能调用stop方法。

纯音频录制时，需要重新调用[prepare()](#prepare-3)接口才能重新录制。纯视频录制，音视频录制时，需要重新调用[prepare()](#prepare-3)和[getInputSurface()](#getinputsurface)接口才能重新录制。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                         |
| -------- | -------------------- | ---- | ---------------------------- |
| callback | AsyncCallback\<void> | 是   | 异步停止视频录制的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                |
| -------- | --------------------------------------- |
| 5400102  | Operate not permit. Return by callback. |
| 5400103  | IO error. Return by callback.           |
| 5400105  | Service died. Return by callback.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.stop((err: BusinessError) => {
  if (err == null) {
    console.info('stop AVRecorder success');
  } else {
    console.error('stop AVRecorder failed and error is ' + err.message);
  }
});
```

### stop

stop(): Promise\<void>

异步方式停止视频录制。通过Promise获取返回值。

需要在[start()](#start)或[pause()](#pause-3)事件成功触发后，才能调用stop方法。

纯音频录制时，需要重新调用[prepare()](#prepare-3)接口才能重新录制。纯视频录制，音视频录制时，需要重新调用[prepare()](#prepare-3)和[getInputSurface()](#getinputsurface)接口才能重新录制。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                                  |
| -------------- | ------------------------------------- |
| Promise\<void> | 异步停止视频录制方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                               |
| -------- | -------------------------------------- |
| 5400102  | Operate not permit. Return by promise. |
| 5400103  | IO error. Return by promise.           |
| 5400105  | Service died. Return by promise.       |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.stop().then(() => {
  console.info('stop AVRecorder success');
}).catch((err: BusinessError) => {
  console.error('stop AVRecorder failed and catch error is ' + err.message);
});
```

### reset

reset(callback: AsyncCallback\<void>): void

异步方式重置音视频录制。通过注册回调函数获取返回值。

纯音频录制时，需要重新调用[prepare()](#prepare-3)接口才能重新录制。纯视频录制，音视频录制时，需要重新调用[prepare()](#prepare-3)和[getInputSurface()](#getinputsurface)接口才能重新录制。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                           |
| -------- | -------------------- | ---- | ------------------------------ |
| callback | AsyncCallback\<void> | 是   | 异步重置音视频录制的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                          |
| -------- | --------------------------------- |
| 5400103  | IO error. Return by callback.     |
| 5400105  | Service died. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.reset((err: BusinessError) => {
  if (err == null) {
    console.info('reset AVRecorder success');
  } else {
    console.error('reset AVRecorder failed and error is ' + err.message);
  }
});
```

### reset

reset(): Promise\<void>

异步方式重置音视频录制。通过Promise获取返回值。

纯音频录制时，需要重新调用[prepare()](#prepare-3)接口才能重新录制。纯视频录制，音视频录制时，需要重新调用[prepare()](#prepare-3)和[getInputSurface()](#getinputsurface)接口才能重新录制。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                                    |
| -------------- | --------------------------------------- |
| Promise\<void> | 异步重置音视频录制方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                         |
| -------- | -------------------------------- |
| 5400103  | IO error. Return by promise.     |
| 5400105  | Service died. Return by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.reset().then(() => {
  console.info('reset AVRecorder success');
}).catch((err: BusinessError) => {
  console.error('reset AVRecorder failed and catch error is ' + err.message);
});
```

### release

release(callback: AsyncCallback\<void>): void

异步方式释放音视频录制资源。通过注册回调函数获取返回值。

释放音视频录制资源之后，该AVRecorder实例不能再进行任何操作。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                               |
| -------- | -------------------- | ---- | ---------------------------------- |
| callback | AsyncCallback\<void> | 是   | 异步释放音视频录制资源的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                          |
| -------- | --------------------------------- |
| 5400105  | Service died. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.release((err: BusinessError) => {
  if (err == null) {
    console.info('release AVRecorder success');
  } else {
    console.error('release AVRecorder failed and error is ' + err.message);
  }
});
```

### release

release(): Promise\<void>

异步方式释放音视频录制资源。通过Promise获取返回值。

释放音视频录制资源之后，该AVRecorder实例不能再进行任何操作。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                                        |
| -------------- | ------------------------------------------- |
| Promise\<void> | 异步释放音视频录制资源方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                          |
| -------- | --------------------------------- |
| 5400105  | Service died. Return by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.release().then(() => {
  console.info('release AVRecorder success');
}).catch((err: BusinessError) => {
  console.error('release AVRecorder failed and catch error is ' + err.message);
});
```

### on('stateChange')

on(type: 'stateChange', callback: OnAVRecorderStateChangeHandler): void

订阅录制状态机AVRecorderState切换的事件，当 AVRecorderState状态机发生变化时，会通过订阅的回调方法通知用户。用户只能订阅一个状态机切换事件的回调方法，当用户重复订阅时，以最后一次订阅的回调接口为准。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 状态机切换事件回调类型，支持的事件：'stateChange'，用户操作和系统都会触发此事件。 |
| callback | [OnAVRecorderStateChangeHandler](#onavrecorderstatechangehandler) | 是   | 状态机切换事件回调方法。                                     |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                          |
| -------- | --------------------------------- |
| 5400103  | IO error. Return by callback.     |
| 5400105  | Service died. Return by callback. |

**示例：**

```ts
avRecorder.on('stateChange', async (state: media.AVRecorderState, reason: media.StateChangeReason) => {
  console.info('case state has changed, new state is :' + state + ',and new reason is : ' + reason);
});
```

### off('stateChange')

off(type: 'stateChange', callback?: OnAVRecorderStateChangeHandler): void

取消订阅播放状态机[AVRecorderState](#avrecorderstate)切换的事件。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型   | 必填                                                         | 说明                                                         |
| -------- | ------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| type     | string | 是                                                           | 状态机切换事件回调类型，支持的事件：'stateChange'，用户操作和系统都会触发此事件。 |
| callback |        | [OnAVRecorderStateChangeHandler](#onavrecorderstatechangehandler) | 状态机切换事件回调方法。                                     |

**示例：**

```ts
avRecorder.off('stateChange');
```

### on('error')

on(type: 'error', callback: ErrorCallback): void

订阅AVRecorder的错误事件，该事件仅用于错误提示，不需要用户停止播控动作。如果此时[AVRecorderState](#avrecorderstate)也切至error状态，用户需要通过[reset()](#reset-3)或者[release()](#release-3)退出录制操作。

用户只能订阅一个错误事件的回调方法，当用户重复订阅时，以最后一次订阅的回调接口为准。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型          | 必填 | 说明                                                         |
| -------- | ------------- | ---- | ------------------------------------------------------------ |
| type     | string        | 是   | 录制错误事件回调类型'error'。 <br>- 'error'：录制过程中发生错误，触发该事件。 |
| callback | ErrorCallback | 是   | 录制错误事件回调方法。                                       |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)。

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400101  | No memory. Return by callback.             |
| 5400102  | Operation not allowed. Return by callback. |
| 5400103  | I/O error. Return by callback.             |
| 5400104  | Time out. Return by callback.              |
| 5400105  | Service died. Return by callback.          |
| 5400106  | Unsupport format. Return by callback.      |
| 5400107  | Audio interrupted. Return by callback.     |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avRecorder.on('error', (err: BusinessError) => {
  console.info('case avRecorder.on(error) called, errMessage is ' + err.message);
});
```

### off('error')

off(type: 'error', callback?: ErrorCallback): void

取消订阅录制错误事件，取消后不再接收到AVRecorder的错误事件。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型          | 必填 | 说明                                                         |
| -------- | ------------- | ---- | ------------------------------------------------------------ |
| type     | string        | 是   | 录制错误事件回调类型'error'。 <br>- 'error'：录制过程中发生错误，触发该事件。 |
| callback | ErrorCallback | 否   | 录制错误事件回调方法。                                       |

**示例：**

```ts
avRecorder.off('error');
```

## AVRecorderState

音视频录制的状态机。可通过state属性获取当前状态。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

| 名称     | 类型   | 说明                                                         | Android平台 | iOS平台 |
| -------- | ------ | ------------------------------------------------------------ | ----------- | ------- |
| idle     | string | 闲置状态。此时可以调用[AVRecorder.prepare()](#prepare-3)方法设置录制参数，进入prepared状态。AVRecorder刚被创建，或者在任何非released状态下调用[AVRecorder.reset()](#reset-3)方法，均进入idle状态。 | 支持        | 支持    |
| prepared | string | 参数设置完成。此时可以调用[AVRecorder.start()](#start)方法开始录制，进入started状态。 | 支持        | 支持    |
| started  | string | 正在录制。此时可以调用[AVRecorder.pause()](#pause-3)方法暂停录制，进入paused状态。也可以调用[AVRecorder.stop()](#stop-3)方法结束录制，进入stopped状态。 | 支持        | 支持    |
| paused   | string | 录制暂停。此时可以调用[AVRecorder.resume()](#resume)方法继续录制，进入started状态。也可以调用[AVRecorder.stop()](#stop-3)方法结束录制，进入stopped状态。 | 支持        | 支持    |
| stopped  | string | 录制停止。此时可以调用[AVRecorder.prepare()](#prepare-3)方法设置录制参数，重新进入prepared状态。 | 支持        | 支持    |
| released | string | 录制资源释放。此时不能再进行任何操作。在任何其他状态下，均可以通过调用[AVRecorder.release()](#release-3)方法进入released状态。 | 支持        | 支持    |
| error    | string | 错误状态。当AVRecorder实例发生不可逆错误，会转换至当前状态。切换至error状态时会伴随[AVRecorder.on('error')事件](#onerror-2)，该事件会上报详细错误原因。在error状态时，用户需要调用[AVRecorder.reset()](#reset-3)方法重置AVRecorder实例，或者调用[AVRecorder.release()](#release)方法释放资源。 | 支持        | 支持    |

## AVRecorderConfig

表示音视频录制的参数设置。

通过audioSourceType和videoSourceType区分纯音频录制、纯视频录制或音视频录制。纯音频录制时，仅需要设置audioSourceType；纯视频录制时，仅需要设置videoSourceType；音视频录制时，audioSourceType和videoSourceType均需要设置。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

| 名称            | 类型                                    | 必填 | 说明                                                         | Android平台 | iOS平台 |
| --------------- | --------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| audioSourceType | [AudioSourceType](#audiosourcetype)     | 否   | 选择录制的音频源类型。选择音频录制时必填。                   | 支持        | 支持    |
| videoSourceType | [VideoSourceType](#videosourcetype)     | 否   | 选择录制的视频源类型。选择视频录制时必填。                   | 支持        | 支持    |
| profile         | [AVRecorderProfile](#avrecorderprofile) | 是   | 录制的profile，必要参数。                                    | 支持        | 支持    |
| url             | string                                  | 是   | 录制输出URL：fd://xx (fd number) ![img](figures/zh-cn_image_url.png)，必要参数。 | 支持        | 支持    |

## AVRecorderProfile

音视频录制的配置文件。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

| 名称                              | 类型                                        | 必填 | 说明                                                         | Android平台 | iOS平台 |
| --------------------------------- | ------------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| audioBitrate                      | number                                      | 否   | 音频编码比特率，选择音频录制时必填，支持范围[8000 - 384000]。 | 支持        | 支持    |
| audioChannels                     | number                                      | 否   | 音频采集声道数，选择音频录制时必填，支持范围[1 - 2]。        | 支持        | 支持    |
| audioCodec                        | [CodecMimeType](#codecmimetype)             | 否   | 音频编码格式，选择音频录制时必填。当前仅支持AUDIO_AAC。      | 支持        | 支持    |
| audioSampleRate                   | number                                      | 否   | 音频采样率，选择音频录制时必填，支持范围[8000, 11025, 12000, 16000, 22050, 24000, 32000, 44100, 48000, 64000, 96000]。 | 支持        | 支持    |
| fileFormat                        | [ContainerFormatType](#containerformattype) | 是   | 文件的容器格式，必要参数。                                   | 支持        | 支持    |
| videoBitrate                      | number                                      | 否   | 视频编码比特率，选择视频录制时必填，支持范围[1 - 3000000]。  | 支持        | 支持    |
| videoCodec                        | [CodecMimeType](#codecmimetype)             | 否   | 视频编码格式，选择视频录制时必填。当前支持VIDEO_AVC。        | 支持        | 支持    |
| videoFrameWidth                   | number                                      | 否   | 视频帧的宽，选择视频录制时必填，支持范围[2 - 1920]。         | 支持        | 支持    |
| videoFrameHeight                  | number                                      | 否   | 视频帧的高，选择视频录制时必填，支持范围[2 - 1080]。         | 支持        | 支持    |
| videoFrameRate                    | number                                      | 否   | 视频帧率，选择视频录制时必填，支持范围[1 - 30]。             | 支持        | 支持    |
| isHdr<sup>11+</sup>               | boolean                                     | 否   | HDR编码，选择视频录制时选填，isHdr默认为false，对应编码格式没有要求，isHdr为true时，对应的编码格式必须为video/hevc。 | 支持        | 支持    |
| enableTemporalScale<sup>12+</sup> | boolean                                     | 否   | 视频录制是否支持分层编码功能，选择视频录制时选填，enableTemporalScale默认为false。 | 支持        | 支持    |

## AudioSourceType

枚举值，定义视频录制中音频源类型。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

| 名称                      | 值   | 说明                   | Android平台 | iOS平台 |
| ------------------------- | ---- | ---------------------- | ----------- | ------- |
| AUDIO_SOURCE_TYPE_DEFAULT | 0    | 默认的音频输入源类型。 | 支持        | 支持    |
| AUDIO_SOURCE_TYPE_MIC     | 1    | 表示MIC的音频输入源。  | 支持        | 支持    |

## VideoSourceType

枚举值，定义视频录制中视频源类型。

**系统能力：** SystemCapability.Multimedia.Media.AVRecorder

| 名称                          | 值   | 说明                            | Android平台 | iOS平台 |
| ----------------------------- | ---- | ------------------------------- | ----------- | ------- |
| VIDEO_SOURCE_TYPE_SURFACE_YUV | 0    | 输入surface中携带的是raw data。 | 支持        | 支持    |
| VIDEO_SOURCE_TYPE_SURFACE_ES  | 1    | 输入surface中携带的是ES data。  | 支持        | 支持    |

## ContainerFormatType

枚举值，定义容器格式类型，缩写为CFT。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称        | 值    | 说明                  | Android平台 | iOS平台 |
| ----------- | ----- | --------------------- | ----------- | ------- |
| CFT_MPEG_4  | 'mp4' | 视频的容器格式，MP4。 | 支持        | 支持    |
| CFT_MPEG_4A | 'm4a' | 音频的容器格式，M4A。 | 支持        | 支持    |

## Location

视频录制的地理位置。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称      | 类型   | 必填 | 说明             | Android平台 | iOS平台 |
| --------- | ------ | ---- | ---------------- | ----------- | ------- |
| latitude  | number | 是   | 地理位置的纬度。 | 支持        | 支持    |
| longitude | number | 是   | 地理位置的经度。 | 支持        | 支持    |

## HdrType

枚举值，定义视频HDR的类型。

**系统能力：** SystemCapability.Multimedia.Media.Core

| 名称              | 值   | 说明                  | Android平台 | iOS平台 |
| ----------------- | ---- | --------------------- | ----------- | ------- |
| AV_HDR_TYPE_NONE  | 0    | 表示无HDR类型。       | 支持        | 支持    |
| AV_HDR_TYPE_VIVID | 1    | 表示为HDR VIVID类型。 | 支持        | 支持    |

## AVMetadataExtractor

元数据获取类，用于从媒体资源中获取元数据。在调用AVMetadataExtractor的方法前，需要先通过[createAVMetadataExtractor()](#mediacreateavmetadataextractor)构建一个AVMetadataExtractor实例。

### 属性

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

| 名称    | 类型                                        | 可读 | 可写 | 说明                                                         | Android平台 | iOS平台 |
| ------- | ------------------------------------------- | ---- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| fdSrc   | [AVFileDescriptor](#avfiledescriptor)       | 是   | 是   | 媒体文件描述，通过该属性设置数据源。在获取元信息之前，必须设置数据源属性，只能设置fdSrc和dataSrc的其中一个。<br/> **使用示例**：<br/>假设一个连续存储的媒体文件，地址偏移:0，字节长度:100。其文件描述为 AVFileDescriptor { fd = 资源句柄; offset = 0; length = 100; }。 | 支持        | 支持    |
| dataSrc | [AVDataSrcDescriptor](#avdatasrcdescriptor) | 是   | 是   | 流式媒体资源描述，通过该属性设置数据源。在获取元信息之前，必须设置数据源属性，只能设置fdSrc和dataSrc的其中一个。<br/> 当应用从远端获取音视频媒体文件，在应用未下载完整音视频资源时，可以设置dataSrc提前获取该资源的元信息。 | 支持        | 不支持  |

### fetchMetadata

fetchMetadata(callback: AsyncCallback\<AVMetadata>): void

异步方式获取媒体元数据。通过注册回调函数获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                      | 必填 | 说明                                               |
| -------- | ----------------------------------------- | ---- | -------------------------------------------------- |
| callback | AsyncCallback\<[AVMetadata](#avmetadata)> | 是   | 回调函数。异步返回音视频元数据对象（AVMetadata）。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                     |
| -------- | -------------------------------------------- |
| 5400102  | Operation not allowed. Returned by callback. |
| 5400106  | Unsupported format. Returned by callback.    |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avMetadataExtractor.fetchMetadata((error: BusinessError, metadata: media.AVMetadata) => {
  if (error) {
    console.error(`fetchMetadata callback failed, err = ${JSON.stringify(error)}`);
    return;
  }
  console.info(`fetchMetadata callback success, genre: ${metadata.genre}`);
});
```

### fetchMetadata

fetchMetadata(): Promise\<AVMetadata>

异步方式获取媒体元数据。通过Promise获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**返回值：**

| 类型                                | 说明                                                  |
| ----------------------------------- | ----------------------------------------------------- |
| Promise\<[AVMetadata](#avmetadata)> | Promise对象。异步返回音视频元数据对象（AVMetadata）。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                    |
| -------- | ------------------------------------------- |
| 5400102  | Operation not allowed. Returned by promise. |
| 5400106  | Unsupported format. Returned by promise.    |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avMetadataExtractor.fetchMetadata().then((metadata: media.AVMetadata) => {
  console.info(`fetchMetadata callback success, genre: ${metadata.genre}`)
}).catch((error: BusinessError) => {
  console.error(`fetchMetadata catchCallback, error message:${error.message}`);
});
```

### fetchAlbumCover

fetchAlbumCover(callback: AsyncCallback\<image.PixelMap>): void

异步方式获取音频专辑封面。通过注册回调函数获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                                                         | 必填 | 说明                         |
| -------- | ------------------------------------------------------------ | ---- | ---------------------------- |
| callback | AsyncCallback\<[image.PixelMap](./js-apis-image.md#pixelmap7)> | 是   | 回调函数。异步返回专辑封面。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 5400102  | Operation not allowed. Return by callback. |
| 5400106  | Unsupported format. Returned by callback.  |

**示例：**

```ts
import { BusinessError } from '@ohos.base';
import image from '@ohos.multimedia.image';

let pixel_map : image.PixelMap | undefined = undefined;

avMetadataExtractor.fetchAlbumCover((error: BusinessError, pixelMap: image.PixelMap) => {
  if (error) {
    console.error(`fetchAlbumCover callback failed, error = ${JSON.stringify(error)}`);
    return;
  }
  pixel_map = pixelMap;
});
```

### fetchAlbumCover

fetchAlbumCover(): Promise\<image.PixelMap>

异步方式获取专辑封面。通过Promise获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**返回值：**

| 类型                                                     | 说明                            |
| -------------------------------------------------------- | ------------------------------- |
| Promise\<[image.PixelMap](./js-apis-image.md#pixelmap7)> | Promise对象。异步返回专辑封面。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                    |
| -------- | ------------------------------------------- |
| 5400102  | Operation not allowed. Returned by promise. |
| 5400106  | Unsupported format. Returned by promise.    |

**示例：**

```ts
import { BusinessError } from '@ohos.base';
import image from '@ohos.multimedia.image';

let pixel_map : image.PixelMap | undefined = undefined;

avMetadataExtractor.fetchAlbumCover().then((pixelMap: image.PixelMap) => {
  pixel_map = pixelMap;
}).catch((error: BusinessError) => {
  console.error(`fetchAlbumCover catchCallback, error message:${error.message}`);
});
```

### release

release(callback: AsyncCallback\<void>): void

异步方式释放资源。通过注册回调函数获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                |
| -------- | -------------------- | ---- | ----------------------------------- |
| callback | AsyncCallback\<void> | 是   | 异步释放资源release方法的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                     |
| -------- | -------------------------------------------- |
| 5400102  | Operation not allowed. Returned by callback. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avMetadataExtractor.release((error: BusinessError) => {
  if (error) {
    console.error(`release failed, err = ${JSON.stringify(error)}`);
    return;
  }
  console.info(`release success.`);
});
```

### release

release(): Promise\<void>

异步方式释放资源。通过Promise获取返回值。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

**支持平台：** Android、iOS

**返回值：**

| 类型           | 说明                                         |
| -------------- | -------------------------------------------- |
| Promise\<void> | 异步方式释放资源release方法的Promise返回值。 |

**错误码：**

以下错误码的详细介绍请参见[媒体错误码](../errorcodes/errorcode-media.md)

| 错误码ID | 错误信息                                    |
| -------- | ------------------------------------------- |
| 5400102  | Operation not allowed. Returned by promise. |

**示例：**

```ts
import { BusinessError } from '@ohos.base';

avMetadataExtractor.release().then(() => {
  console.info(`release success.`);
}).catch((error: BusinessError) => {
  console.error(`release catchCallback, error message:${error.message}`);
});
```

## AVMetadata

音视频元数据，包含各个元数据字段。

**系统能力：** SystemCapability.Multimedia.Media.AVMetadataExtractor

| 名称             | 类型   | 必填 | 说明                                                | Android平台 | iOS平台 |
| ---------------- | ------ | ---- | --------------------------------------------------- | ----------- | ------- |
| album            | string | 否   | 专辑的标题。                                        | 支持        | 支持    |
| albumArtist      | string | 否   | 专辑的艺术家。                                      | 支持        | 支持    |
| artist           | string | 否   | 媒体资源的艺术家。                                  | 支持        | 支持    |
| author           | string | 否   | 媒体资源的作者。                                    | 支持        | 支持    |
| dateTime         | string | 否   | 媒体资源的创建时间。                                | 支持        | 支持    |
| dateTimeFormat   | string | 否   | 媒体资源的创建时间，按YYYY-MM-DD HH:mm:ss格式输出。 | 支持        | 支持    |
| composer         | string | 否   | 媒体资源的作曲家。                                  | 支持        | 支持    |
| duration         | string | 否   | 媒体资源的时长。                                    | 支持        | 支持    |
| genre            | string | 否   | 媒体资源的类型或体裁。                              | 支持        | 支持    |
| hasAudio         | string | 否   | 媒体资源是否包含音频。                              | 支持        | 支持    |
| hasVideo         | string | 否   | 媒体资源是否包含视频。                              | 支持        | 支持    |
| mimeType         | string | 否   | 媒体资源的mime类型。                                | 支持        | 支持    |
| trackCount       | string | 否   | 媒体资源的轨道数量。                                | 支持        | 支持    |
| sampleRate       | string | 否   | 音频的采样率，单位为赫兹（Hz）。                    | 支持        | 支持    |
| title            | string | 否   | 媒体资源的标题。                                    | 支持        | 支持    |
| videoHeight      | string | 否   | 视频的高度，单位为像素。                            | 支持        | 支持    |
| videoWidth       | string | 否   | 视频的宽度，单位为像素。                            | 支持        | 支持    |
| videoOrientation | string | 否   | 视频的旋转方向，单位为度（°）。                     | 支持        | 支持    |
| hdrType          | HdrTyp | 否   | 媒体资源的HDR类型。                                 | 支持        | 支持    |
