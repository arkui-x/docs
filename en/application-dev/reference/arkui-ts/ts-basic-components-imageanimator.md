# ImageAnimator

The **\<ImageAnimator>** component enables images to be played frame by frame. The list of images to be played as well as the duration of each image can be configured.

>  **NOTE**
>
> This component is supported since API version 7. Updates will be marked with a superscript to indicate their earliest API version.



## Child Components

Not supported


## APIs

ImageAnimator()

Since API version 10, this API is supported in ArkTS widgets.

## Attributes

In addition to the [universal attributes](ts-universal-attributes-size.md), the following attributes are supported.

| Name    | Type                 |Description                  |
| ---------- | ----------------------- |-------- |
| images     | Array&lt;[ImageFrameInfo](#imageframeinfo)&gt; | Image frame information. The information of each frame includes the image path, image size, image position, and image playback duration. For details, see **ImageFrameInfo**.<br>Default value: **[]**<br>**NOTE**<br>Dynamic update is not supported.<br>Since API version 10, this API is supported in ArkTS widgets.|
| state      | [AnimationStatus](ts-appendix-enums.md#animationstatus) |  Playback status of the animation. The default status is **Initial**.<br>Default value: **AnimationStatus.Initial**<br>Since API version 10, this API is supported in ArkTS widgets.|
| duration   | number  | Playback duration, in ms. The default duration is 1000 ms. When the duration is **0**, no image is played. The value change takes effect only at the beginning of the next cycle. When a separate duration is set in **images**, the setting of this attribute is invalid.<br>Default value: **1000**<br>Since API version 10, this API is supported in ArkTS widgets.|
| reverse    | boolean | Playback sequence. The value **false** indicates that images are played from the first one to the last one, and **true** indicates that images are played from the last one to the first one.<br>Default value: **false**<br>Since API version 10, this API is supported in ArkTS widgets.|
| fixedSize  | boolean | Whether the image size is the same as the component size.<br> **true**: The image size is the same as the component size. In this case, the width, height, top, and left attributes of the image are invalid.<br> **false**: The width, height, top, and left attributes of each image must be set separately.<br>Default value: **true**<br>Since API version 10, this API is supported in ArkTS widgets.|
| fillMode   | [FillMode](ts-appendix-enums.md#fillmode) | Status before and after the animation starts. For details about the options, see **FillMode**.<br>Default value: **FillMode.Forwards**<br>Since API version 10, this API is supported in ArkTS widgets.|
| iterations | number  | Number of times that the animation is played. By default, the animation is played once. The value **-1** indicates that the animation is played for an unlimited number of times.<br>Default value: **1** |

## ImageFrameInfo

| Name  | Type  | Mandatory| Description|
| -------- | -------------- | -------- | -------- |
| src      | string \| [Resource](ts-types.md#resource)<sup>9+</sup> | Yes   | Image path. The image format can be .svg, .png, or .jpg. Since API version 9, this attribute accepts paths of the [Resource](ts-types.md#resource) type.<br>Since API version 10, this API is supported in ArkTS widgets.|
| width    | number \| string | No | Image width.<br>Default value: **0**<br>Since API version 10, this API is supported in ArkTS widgets.      |
| height   | number \| string | No | Image height.<br>Default value: **0**<br>Since API version 10, this API is supported in ArkTS widgets.       |
| top      | number \| string | No | Vertical coordinate of the image relative to the upper left corner of the widget<br>Default value: **0**<br>Since API version 10, this API is supported in ArkTS widgets. |
| left     | number \| string | No | Horizontal coordinate of the image relative to the upper left corner of the widget<br>Default value: **0**<br>Since API version 10, this API is supported in ArkTS widgets.  |
| duration | number          | No    | Playback duration of each image frame, in milliseconds.<br>Default value: **0**        |

## Events

In addition to the [universal events](ts-universal-events-click.md), the following events are supported.

| Name| Description|
| -------- | -------- |
| onStart(event: () =&gt; void)  | Triggered when the animation starts to play.<br>Since API version 10, this API is supported in ArkTS widgets.|
| onPause(event: () =&gt; void)  | Triggered when the animation playback is paused.<br>Since API version 10, this API is supported in ArkTS widgets.|
| onRepeat(event: () =&gt; void) | Triggered when the animation playback is repeated. |
| onCancel(event: () =&gt; void) | Triggered when the animation playback is canceled.<br>Since API version 10, this API is supported in ArkTS widgets.|
| onFinish(event: () =&gt; void) | Triggered when the animation playback is complete.<br>Since API version 10, this API is supported in ArkTS widgets.|


## Example

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
          this.state = AnimationStatus.Paused     // Display the image of the current frame.
        }).margin(5)
        Button('stop').width(100).padding(5).onClick(() => {
          this.state = AnimationStatus.Stopped    // Display the image of the initial frame.
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
          this.iterations = -1 // The animation is played for an unlimited number of times.
        }).margin(5)
      }
    }.width('100%').height('100%')
  }
}
```

![imageAnimator](figures/imageAnimator.gif)
