# ImageAnimator

提供帧动画组件来实现逐帧播放图片的能力，可以配置需要播放的图片列表，每张图片可以配置时长。

>  **说明：**
>
> 该组件从API Version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。



## 子组件

无


## 接口

ImageAnimator()

从API version 10开始，该接口支持在ArkTS卡片中使用。

## 属性

除支持[通用属性](ts-universal-attributes-size.md)外，还支持以下属性：

| 参数名称     | 参数类型                  |参数描述                   |
| ---------- | ----------------------- |-------- |
| images     | Array&lt;[ImageFrameInfo](#imageframeinfo对象说明)&gt; | 设置图片帧信息集合。每一帧的帧信息(ImageFrameInfo)包含图片路径、图片大小、图片位置和图片播放时长信息，详见ImageFrameInfo属性说明。<br/>默认值：[]<br/>**说明：**<br>不支持动态更新。<br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| state      | [AnimationStatus](ts-appendix-enums.md#animationstatus) |  默认为初始状态，用于控制播放状态。<br/>默认值：AnimationStatus.Initial <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| duration   | number  | 单位为毫秒，默认时长为1000ms；duration为0时，不播放图片；值的改变只会在下一次循环开始时生效；当images中任意一帧图片设置了单独的duration后，该属性设置无效。<br/>默认值：1000 <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| reverse    | boolean | 设置播放顺序。false表示从第1张图片播放到最后1张图片；&nbsp;true表示从最后1张图片播放到第1张图片。<br/>默认值：false <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| fixedSize  | boolean | 设置图片大小是否固定为组件大小。&nbsp;true表示图片大小与组件大小一致，此时设置图片的width&nbsp;、height&nbsp;、top&nbsp;和left属性是无效的。false表示每一张图片的width&nbsp;、height&nbsp;、top和left属性都要单独设置。<br/>默认值：true <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| fillMode   | [FillMode](ts-appendix-enums.md#fillmode) | 设置动画开始前和结束后的状态，可选值参见FillMode说明。<br/>默认值：FillMode.Forwards <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| iterations | number  | 默认播放一次，设置为-1时表示无限次播放。<br/>默认值：1  |

## ImageFrameInfo对象说明

| 参数名称   | 参数类型   | 必填 | 参数描述 |
| -------- | -------------- | -------- | -------- |
| src      | string \| [Resource](ts-types.md#resource)<sup>9+</sup> | 是    | 图片路径，图片格式为svg，png和jpg，从API Version9开始支持[Resource](ts-types.md#resource)类型的路径。 <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。|
| width    | number&nbsp;\|&nbsp;string | 否  | 图片宽度。<br/>默认值：0   <br/>从API version 10开始，该接口支持在ArkTS卡片中使用       |
| height   | number&nbsp;\|&nbsp;string | 否  | 图片高度。<br/>默认值：0     <br/>从API version 10开始，该接口支持在ArkTS卡片中使用        |
| top      | number&nbsp;\|&nbsp;string | 否  | 图片相对于组件左上角的纵向坐标。<br/>默认值：0  <br/>从API version 10开始，该接口支持在ArkTS卡片中使用  |
| left     | number&nbsp;\|&nbsp;string | 否  | 图片相对于组件左上角的横向坐标。<br/>默认值：0 <br/>从API version 10开始，该接口支持在ArkTS卡片中使用   |
| duration | number          | 否     | 每一帧图片的播放时长，单位毫秒。<br/>默认值：0         |

## 事件

除支持[通用事件](ts-universal-events-click.md)外，还支持以下事件：

| 名称 | 功能描述 |
| -------- | -------- |
| onStart(event:&nbsp;()&nbsp;=&gt;&nbsp;void)  | 状态回调，动画开始播放时触发。 <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| onPause(event:&nbsp;()&nbsp;=&gt;&nbsp;void)  | 状态回调，动画暂停播放时触发。 <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| onRepeat(event:&nbsp;()&nbsp;=&gt;&nbsp;void) | 状态回调，动画重复播放时触发。  |
| onCancel(event:&nbsp;()&nbsp;=&gt;&nbsp;void) | 状态回调，动画取消播放时触发。 <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |
| onFinish(event:&nbsp;()&nbsp;=&gt;&nbsp;void) | 状态回调，动画播放完成时触发。 <br/>从API version 10开始，该接口支持在ArkTS卡片中使用。 |


## 示例

```ts
// xxx.ets
@Entry
@Component
struct ImageAnimatorExample {
  @State state: AnimationStatus = AnimationStatus.Initial
  @State reverse: boolean = false
  @State iterations: number = 1

  build() {
    Column({ space: 10 }) {
      ImageAnimator()
        .images([
          {
            src: $r('app.media.img1')
          },
          {
            src: $r('app.media.img2')
          },
          {
            src: $r('app.media.img3')
          },
          {
            src: $r('app.media.img4')
          }
        ])
        .duration(2000)
        .state(this.state).reverse(this.reverse)
        .fillMode(FillMode.None).iterations(this.iterations).width(340).height(240)
        .margin({ top: 100 })
        .onStart(() => {
          console.info('Start')
        })
        .onPause(() => {
          console.info('Pause')
        })
        .onRepeat(() => {
          console.info('Repeat')
        })
        .onCancel(() => {
          console.info('Cancel')
        })
        .onFinish(() => {
          console.info('Finish')
          this.state = AnimationStatus.Stopped
        })
      Row() {
        Button('start').width(100).padding(5).onClick(() => {
          this.state = AnimationStatus.Running
        }).margin(5)
        Button('pause').width(100).padding(5).onClick(() => {
          this.state = AnimationStatus.Paused     // 显示当前帧图片
        }).margin(5)
        Button('stop').width(100).padding(5).onClick(() => {
          this.state = AnimationStatus.Stopped    // 显示动画的起始帧图片
        }).margin(5)
      }

      Row() {
        Button('reverse').width(100).padding(5).onClick(() => {
          this.reverse = !this.reverse
        }).margin(5)
        Button('once').width(100).padding(5).onClick(() => {
          this.iterations = 1
        }).margin(5)
        Button('infinite').width(100).padding(5).onClick(() => {
          this.iterations = -1 // 无限循环播放
        }).margin(5)
      }
    }.width('100%').height('100%')
  }
}
```

![imageAnimator](figures/imageAnimator.gif)