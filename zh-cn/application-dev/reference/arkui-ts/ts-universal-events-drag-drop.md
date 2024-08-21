# 拖拽事件

拖拽事件指组件被长按后拖拽时触发的事件。

>  **说明：**
>
>  从API Version 13开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>
>  应用本身预置的资源文件（即应用在安装前的HAP包中已经存在的资源文件）仅支持本地应用内拖拽。

ArkUI框架对以下组件实现了默认的拖拽能力，支持对数据的拖出或拖入响应，开发者只需要将这些组件的[draggable](ts-universal-attributes-drag-drop.md)属性设置为true，即可使用默认拖拽能力。

- 默认支持拖出能力的组件（可从组件上拖出数据）：[Search](ts-basic-components-search.md)、[TextInput](ts-basic-components-textinput.md)、[TextArea](ts-basic-components-textarea.md)、[Text](ts-basic-components-text.md)、[Image](ts-basic-components-image.md)

- 默认支持拖入能力的组件（目标组件可响应拖入数据）：[Search](ts-basic-components-search.md)、[TextInput](ts-basic-components-textinput.md)、[TextArea](ts-basic-components-textarea.md)、[Video](ts-media-components-video.md)

开发者也可以通过实现通用拖拽事件来自定义拖拽响应。

其他组件需要开发者将draggable属性设置为true，并在onDragStart等接口中实现数据传输相关内容，才能正确处理拖拽。

## onDragStart

onDragStart(event: (event: DragEvent, extraParams?: string) => CustomBuilder | DragItemInfo)

第一次拖拽此事件绑定的组件时，长按时间 >= 500ms，然后手指移动距离 >= 10vp，触发回调。

针对默认支持拖出能力的组件，如果开发者设置了onDragStart，优先执行开发者的onDragStart，并根据执行情况决定是否使用系统默认的拖出能力，具体为：
- 如果开发者返回了自定义背板图，则不再使用系统默认的拖拽背板图；
- 如果开发者设置了拖拽数据，则不再使用系统默认填充的拖拽数据。

文本类组件[Text](ts-basic-components-text.md)、[Search](ts-basic-components-search.md)、[TextInput](ts-basic-components-textinput.md)、[TextArea](ts-basic-components-textarea.md)对选中的文本内容进行拖拽时，不支持背板图的自定义。

**事件优先级：** 长按触发时间 < 500ms，长按事件优先拖拽事件响应，长按触发时间 >= 500ms，拖拽事件优先长按事件响应。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名 | 类型                                                         | 必填 | 说明                                                         |
| ------ | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| event  | (event: [DragEvent](#dragevent), extraParams?: string) => [CustomBuilder](ts-types.md#custombuilder8) &nbsp;\|&nbsp; [DragItemInfo](#dragiteminfo说明) | 是   | 回调函数。<br/> **说明：**<br/> event为拖拽事件信息。<br/> extraParams为拖拽事件额外信息。需要解析为Json格式，参考[extraParams](#extraparams说明)说明。 |

**返回值：**

| 类型                                                         | 说明                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| [CustomBuilder](ts-types.md#custombuilder8)&nbsp;\|&nbsp;[DragItemInfo](#dragiteminfo说明) | 拽过程中显示的组件信息。<br/>**说明：** 不支持全局builder。 |

## onDragEnter

onDragEnter(event: (event: DragEvent, extraParams?: string) => void)

拖拽进入组件范围内时，触发回调，当监听了[onDrop](#ondrop)事件时，此事件才有效。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名 | 类型                                                         | 必填 | 说明                                                         |
| ------ | ------------------------------------------------------------ | ---- | :----------------------------------------------------------- |
| event  | (event: [DragEvent](#dragevent), extraParams?: string) => void | 是   | 回调函数。<br/>**说明：**<br/> event为拖拽事件信息，包括拖拽点坐标。<br/> extraParams为拖拽事件额外信息，需要解析为Json格式，参考[extraParams](#extraparams说明)说明。 |

## onDragMove

onDragMove(event: (event: DragEvent, extraParams?: string) => void)

拖拽在组件范围内移动时，触发回调，当监听了[onDrop](#ondrop)事件时，此事件才有效。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名 | 类型                                                         | 必填 | 说明                                                         |
| ------ | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| event  | (event: [DragEvent](#dragevent), extraParams?: string) => void | 是   | 回调函数。<br/>**说明：**<br/> event为拖拽事件信息，包括拖拽点坐标。<br/> extraParams为拖拽事件额外信息，需要解析为Json格式，参考[extraParams](#extraparams说明)说明。 |

## onDragLeave

onDragLeave(event: (event: DragEvent, extraParams?: string) => void)

拖拽离开组件范围内时，触发回调，当监听了[onDrop](#ondrop)事件时，此事件才有效。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名 | 类型                                                         | 必填 | 说明                                                         |
| ------ | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| event  | (event: [DragEvent](#dragevent), extraParams?: string) => void | 是   | 回调函数。<br/>**说明：**<br/> event为拖拽事件信息，包括拖拽点坐标。<br/> extraParams为拖拽事件额外信息，需要解析为Json格式，参考[extraParams](#extraparams说明)说明。 |

## onDrop

onDrop(event: (event: DragEvent, extraParams?: string) => void)

绑定此事件的组件可作为拖拽释放目标，当在本组件范围内停止拖拽行为时，触发回调。如果没有显式使用event.setResult()，则默认result为DRAG_SUCCESSFUL。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名 | 类型                                                         | 必填 | 说明                                                         |
| ------ | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| event  | (event: [DragEvent](#dragevent), extraParams?: string) => void | 是   | 回调函数。<br/>**说明：**<br/> event为拖拽事件信息，包括拖拽点坐标。<br/> extraParams为拖拽事件额外信息，需要解析为Json格式，参考[extraParams](#extraparams说明)说明。 |

## onDragEnd

onDragEnd(event: (event: DragEvent, extraParams?: string) => void)

绑定此事件的组件触发的拖拽结束后，触发回调。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名 | 类型                                                         | 必填 | 说明                                                         |
| ------ | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| event  | (event: [DragEvent](#dragevent), extraParams?: string) => void | 是   | 回调函数。<br/>**说明：**<br/> event为拖拽事件信息，不包括拖拽点坐标。<br/> extraParams为拖拽事件额外信息，需要解析为Json格式，参考[extraParams](#extraparams说明)说明。 |

## DragItemInfo说明

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称      | 类型                                           | 必填 | 描述                                                         | Android平台 | iOS平台 |
| --------- | ---------------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| pixelMap  | [PixelMap](../apis/js-apis-image.md#pixelmap7) | 否   | 设置拖拽过程中显示的图片。                                   | 支持        | 支持    |
| builder   | [CustomBuilder](ts-types.md#custombuilder8)    | 否   | 拖拽过程中显示自定义组件，如果设置了pixelMap，则忽略此值。<br /> **说明：** <br/>不支持全局builder。如果builder中使用了[Image](ts-basic-components-image.md)组件，应尽量开启同步加载，即配置Image的[syncLoad](ts-basic-components-image.md#syncload8)为true。该builder只用于生成当次拖拽中显示的图片，builder的修改不会同步到当前正在拖拽的图片，对builder的修改需要在下一次拖拽时生效。 | 支持        | 支持    |
| extraInfo | string                                         | 否   | 拖拽项的描述。                                               | 支持        | 支持    |


## extraParams说明

  用于返回组件在拖拽中需要用到的额外信息。

  extraParams是Json对象转换的string字符串，可以通过Json.parse转换的Json对象获取如下属性。

| 名称          | 类型   | 描述                                                         | Android平台 | iOS平台 |
| ------------- | ------ | ------------------------------------------------------------ | ----------- | ------- |
| selectedIndex | number | 当拖拽事件设在父容器的子元素时，selectedIndex表示当前被拖拽子元素是父容器第selectedIndex个子元素，selectedIndex从0开始。<br/>仅在ListItem组件的拖拽事件中生效。 | 支持        | 支持    |
| insertIndex   | number | 当前拖拽元素在List组件中放下时，insertIndex表示被拖拽元素插入该组件的第insertIndex个位置，insertIndex从0开始。<br/>仅在List组件的拖拽事件中生效。 | 支持        | 支持    |

## DragEvent

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

### 属性

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                                 | 类型                            | 描述                                                         | Android平台 | iOS平台 |
| ------------------------------------ | ------------------------------- | ------------------------------------------------------------ | ----------- | ------- |
| useCustomDropAnimation<sup>10+</sup> | boolean                         | 当拖拽结束时，是否使能并使用系统默认落位动效。<br/>应用可将该值设定为true来禁用系统默认落位动效，并实现自己的自定义动效。<br/>当不配置或设置为false时，系统默认落位动效生效，此情况下，应用不应再实现自定义动效，以避免动效上的冲突。 | 支持        | 支持    |
| dragBehavior<sup>10+</sup>           | [DragBehavior](#dragbehavior10) | 切换复制和剪贴模式的角标显示状态。                           | 支持        | 支持    |

### 方法

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                                                         | 返回值类型                                                   | 描述                                                         | Android平台 | iOS平台 |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ----------- | ------- |
| setData(unifiedData: [UnifiedData](../apis/js-apis-data-unifiedDataChannel.md#unifieddata))<sup>10+</sup> | void                                                         | 向DragEvent中设置拖拽相关数据。                              | 支持        | 支持    |
| getData()<sup>10+</sup>                                      | [UnifiedData](../apis/js-apis-data-unifiedDataChannel.md#unifieddata) | 从DragEvent中获取拖拽相关数据。数据获取结果请参考错误码说明。 | 支持        | 支持    |
| setResult(dragRect: [DragResult](#dragresult10枚举说明))<sup>10+</sup> | void                                                         | 向DragEvent中设置拖拽结果。                                  | 支持        | 支持    |
| getResult()<sup>10+</sup>                                    | [DragResult](#dragresult10枚举说明)                          | 从DragEvent中获取拖拽结果。                                  | 支持        | 支持    |
| getPreviewRect()<sup>10+</sup>                               | [Rectangle](ts-universal-attributes-touch-target.md#rectangle对象说明) | 获取预览图所在的Rectangle。                                  | 支持        | 支持    |
| getVelocityX()<sup>10+</sup>                                 | number                                                       | 获取当前拖拽的x轴方向拖动速度。坐标轴原点为屏幕左上角，单位为vp，分正负方向速度，从左往右为正，反之为负。 | 支持        | 支持    |
| getVelocityY()<sup>10+</sup>                                 | number                                                       | 获取当前拖拽的y轴方向拖动速度。坐标轴原点为屏幕左上角，单位为vp，分正负方向速度，从上往下为正，反之为负。 | 支持        | 支持    |
| getVelocity()<sup>10+</sup>                                  | number                                                       | 获取当前拖拽的主方向拖动速度。为xy轴方向速度的平方和的算术平方根。 | 支持        | 支持    |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[drag-event(拖拽事件)](errorcode-drag-event.md)错误码。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes: 1. Incorrect parameter types. 2. Parameter verification failed. |
| 190001   | Data not found.                                              |
| 190002   | Data error.                                                  |

## DragResult<sup>10+</sup>枚举说明

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称            | 描述                                 | Android平台 | iOS平台 |
| --------------- | ------------------------------------ | ----------- | ------- |
| DRAG_SUCCESSFUL | 拖拽成功，在onDrop中使用。           | 支持        | 支持    |
| DRAG_FAILED     | 拖拽失败，在onDrop中使用。           | 支持        | 支持    |
| DRAG_CANCELED   | 拖拽取消，在onDrop中使用。           | 支持        | 支持    |
| DROP_ENABLED    | 组件允许落入，在onDragMove中使用。   | 支持        | 支持    |
| DROP_DISABLED   | 组件不允许落入，在onDragMove中使用。 | 支持        | 支持    |

## DragBehavior<sup>10+</sup>

需要设置[DragResult](#dragresult10枚举说明)为DROP_ENABLED，并实现[onDrop](#ondrop)回调时才能够生效。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称 | 描述           | Android平台 | iOS平台 |
| ---- | -------------- | ----------- | ------- |
| COPY | 复制模式角标。 | 支持        | 支持    |
| MOVE | 剪贴模式角标。 | 支持        | 支持    |

## 示例

```ts
// xxx.ets
import unifiedDataChannel from '@ohos.data.unifiedDataChannel';
import uniformTypeDescriptor from '@ohos.data.uniformTypeDescriptor';
import promptAction from '@ohos.promptAction';
import { BusinessError } from '@ohos.base';

@Entry
@Component
struct Index {
  @State targetImage: string = '';
  @State targetText: string = 'Drag Text';
  @State imageWidth: number = 100;
  @State imageHeight: number = 100;
  @State imgState: Visibility = Visibility.Visible;
  @State videoSrc: string = 'resource://RAWFILE/02.mp4';
  @State abstractContent: string = "abstract";
  @State textContent: string = "";
  @State backGroundColor: Color = Color.Transparent;

  @Builder
  pixelMapBuilder() {
    Column() {
      Image($r('app.media.startIcon'))
        .width(120)
        .height(120)
        .backgroundColor(Color.Yellow)
    }
  }

  getDataFromUdmfRetry(event: DragEvent, callback: (data: DragEvent) => void) {
    try {
      let data: UnifiedData = event.getData();
      if (!data) {
        return false;
      }
      let records: Array<unifiedDataChannel.UnifiedRecord> = data.getRecords();
      if (!records || records.length <= 0) {
        return false;
      }
      callback(event);
      return true;
    } catch (e) {
      console.log("getData failed, code = " + (e as BusinessError).code + ", message = " + (e as BusinessError).message);
      return false;
    }
  }

  getDataFromUdmf(event: DragEvent, callback: (data: DragEvent) => void) {
    if (this.getDataFromUdmfRetry(event, callback)) {
      return;
    }
    setTimeout(() => {
      this.getDataFromUdmfRetry(event, callback);
    }, 1500);
  }

  private PreDragChange(preDragStatus: PreDragStatus): void {
    if (preDragStatus == PreDragStatus.READY_TO_TRIGGER_DRAG_ACTION) {
      this.backGroundColor = Color.Red;
    } else if (preDragStatus == PreDragStatus.ACTION_CANCELED_BEFORE_DRAG
      || preDragStatus == PreDragStatus.PREVIEW_LANDING_FINISHED) {
      this.backGroundColor = Color.Blue;
    }
  }

  build() {
    Row() {
      Column() {
        Text('start Drag')
          .fontSize(18)
          .width('100%')
          .height(40)
          .margin(10)
          .backgroundColor('#008888')
        Image($r('app.media.startIcon'))
          .width(100)
          .height(100)
          .draggable(true)
          .margin({ left: 15 })
          .visibility(this.imgState)
          .onDragEnd((event) => {
            // onDragEnd里取到的result值在接收方onDrop设置
            if (event.getResult() === DragResult.DRAG_SUCCESSFUL) {
              promptAction.showToast({ duration: 100, message: 'Drag Success' });
            } else if (event.getResult() === DragResult.DRAG_FAILED) {
              promptAction.showToast({ duration: 100, message: 'Drag failed' });
            }
          })
        Text('test drag event')
          .width('100%')
          .height(100)
          .draggable(true)
          .margin({ left: 15 })
          .copyOption(CopyOptions.InApp)
        TextArea({ placeholder: 'please input words' })
          .copyOption(CopyOptions.InApp)
          .width('100%')
          .height(50)
          .draggable(true)
        Search({ placeholder: 'please input you word' })
          .searchButton('Search')
          .width('100%')
          .height(80)
          .textFont({ size: 20 })
        Column() {
          Text('change video source')
        }.draggable(true)
        .onDragStart((event) => {
          let video: unifiedDataChannel.Video = new unifiedDataChannel.Video();
          video.videoUri = '/resources/rawfile/01.mp4';
          let data: unifiedDataChannel.UnifiedData = new unifiedDataChannel.UnifiedData(video);
          (event as DragEvent).setData(data);
          return { builder: () => {
            this.pixelMapBuilder()
          }, extraInfo: 'extra info' };
        })

        Column() {
          Text('this is abstract')
            .fontSize(20)
            .width('100%')
        }.margin({ left: 40, top: 20 })
        .width('100%')
        .height(100)
        .onDragStart((event) => {
          this.backGroundColor = Color.Transparent;
          let data: unifiedDataChannel.PlainText = new unifiedDataChannel.PlainText();
          data.abstract = 'this is abstract';
          data.textContent = 'this is content this is content';
          (event as DragEvent).setData(new unifiedDataChannel.UnifiedData(data));
        })
        .onPreDrag((status: PreDragStatus) => {
          this.PreDragChange(status);
        })
        .backgroundColor(this.backGroundColor)
      }.width('45%')
      .height('100%')

      Column() {
        Text('Drag Target Area')
          .fontSize(20)
          .width('100%')
          .height(40)
          .margin(10)
          .backgroundColor('#008888')
        Image(this.targetImage)
          .width(this.imageWidth)
          .height(this.imageHeight)
          .draggable(true)
          .margin({ left: 15 })
          .border({ color: Color.Black, width: 1 })
          .allowDrop([uniformTypeDescriptor.UniformDataType.IMAGE])
          .onDrop((dragEvent?: DragEvent) => {
            this.getDataFromUdmf((dragEvent as DragEvent), (event: DragEvent) => {
              let records: Array<unifiedDataChannel.UnifiedRecord> = event.getData().getRecords();
              let rect: Rectangle = event.getPreviewRect();
              this.imageWidth = Number(rect.width);
              this.imageHeight = Number(rect.height);
              this.targetImage = (records[0] as unifiedDataChannel.Image).imageUri;
              event.useCustomDropAnimation = false;
              animateTo({ duration: 1000 }, () => {
                this.imageWidth = 100;
                this.imageHeight = 100;
                this.imgState = Visibility.None;
              })
              // 显式设置result为successful，则将该值传递给拖出方的onDragEnd
              event.setResult(DragResult.DRAG_SUCCESSFUL);
            })
          })

        Text(this.targetText)
          .width('100%')
          .height(100)
          .border({ color: Color.Black, width: 1 })
          .margin(15)
          .allowDrop([uniformTypeDescriptor.UniformDataType.PLAIN_TEXT])
          .onDrop((dragEvent?: DragEvent) => {
            this.getDataFromUdmf((dragEvent as DragEvent), (event: DragEvent) => {
              let records: Array<unifiedDataChannel.UnifiedRecord> = event.getData().getRecords();
              let plainText: unifiedDataChannel.PlainText = records[0] as unifiedDataChannel.PlainText;
              this.targetText = plainText.textContent;
            })
          })

        Video({ src: this.videoSrc, previewUri: $r('app.media.startIcon') })
          .width('100%')
          .height(200)
          .controls(true)
          .allowDrop([uniformTypeDescriptor.UniformDataType.VIDEO])

        Column() {
          Text(this.abstractContent).fontSize(20).width('100%')
          Text(this.textContent).fontSize(15).width('100%')
        }
        .width('100%')
        .height(100)
        .margin(20)
        .border({ color: Color.Black, width: 1 })
        .allowDrop([uniformTypeDescriptor.UniformDataType.PLAIN_TEXT])
        .onDrop((dragEvent?: DragEvent) => {
          this.getDataFromUdmf((dragEvent as DragEvent), (event: DragEvent) => {
            let records: Array<unifiedDataChannel.UnifiedRecord> = event.getData().getRecords();
            let plainText: unifiedDataChannel.PlainText = records[0] as unifiedDataChannel.PlainText;
            this.abstractContent = plainText.abstract as string;
            this.textContent = plainText.textContent;
          })
        })
      }.width('45%')
      .height('100%')
      .margin({ left: '5%' })
    }
    .height('100%')
  }
} 
```
![figures/drag-drop.png](figures/drag-drop.png)
