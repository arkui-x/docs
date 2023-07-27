# Component Transition

You can configure the component transition animations through the **transition** attribute for when a component is inserted or deleted.

>  **NOTE**
>
>  The APIs of this module are supported since API version 7. Updates will be marked with a superscript to indicate their earliest API version.


## Attributes


| Name| Type| Description|
| -------- | -------- | -------- |
| transition | TransitionOptions \| TransitionEffect<sup>10+</sup>  | Transition effects when the component is inserted, displayed, deleted, or hidden.<br>Since API version 9, this API is supported in ArkTS widgets.<br>**NOTE**<br>For details, see the description of **TransitionOptions** and **TransitionEffect**.|

## TransitionOptions
Defines the transition effect by setting parameters in the struct.
| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| type | [TransitionType](ts-appendix-enums.md#transitiontype)  | No| Transition type.<br>Default value: **TransitionType.All**<br>Since API version 9, this API is supported in ArkTS widgets.<br>**NOTE**<br>If **type** is not specified, the default value **TransitionType.All** is used, which means that the transition effect works for both component addition and deletion.|
| opacity | number | No| Opacity of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>Value range: [0, 1]<br>Since API version 9, this API is supported in ArkTS widgets.<br>**NOTE**<br>A value less than 0 or greater than 1 evaluates to the value **1**.|
| translate | {<br>x? : number \| string,<br>y? : number \| string,<br>z? : number \| string<br>} | No| Translation of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>-**x**: distance to translate along the x-axis.<br>-**y**: distance to translate along the y-axis.<br>-**z**: distance to translate along the z-axis.<br>Since API version 9, this API is supported in ArkTS widgets.|
| scale | {<br>x? : number,<br>y? : number,<br>z? : number,<br>centerX? : number \| string,<br>centerY? : number \| string<br>} | No| Scaling of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>- **x**: scale factor along the x-axis.<br>- **y**: scale factor along the y-axis.<br>- **z**: scale factor along the z-axis.<br>- **centerX** and **centerY**: scale center point. The default values are both **"50%"**, indicating that the center point of the page.<br>- If the center point is (0, 0), it refers to the upper left corner of the component.<br>Since API version 9, this API is supported in ArkTS widgets.|
| rotate | {<br>x?: number,<br>y?: number,<br>z?: number,<br>angle: number \| string,<br>centerX?: number \| string,<br>centerY?: number \| string<br>} | No| Rotation of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>- **x**: X-component of the rotation vector.<br>- **y**: Y-component of the rotation vector.<br>- **z**: Z-component of the rotation vector.<br>- **centerX** and **centerY**: rotation center point. The default values are both **"50%"**, indicating that the center point of the page.<br>- If the center point is (0, 0), it refers to the upper left corner of the component.<br>Since API version 9, this API is supported in ArkTS widgets.|

>  **NOTE**
>
>  1. When set to a value of the **TransitionOptions** type, the **transition** attribute must work with [animateTo](ts-explicit-animation.md). The animation duration, curve, and delay follow the settings in **animateTo**.
>  2. If the value of the **TransitionOptions** type has only **type** specified, the transition effect will take on the default opacity. For example, **{type: TransitionType.Insert}** produces the same effect as **{type: TransitionType.Insert, opacity: 0}**. If a specific style is specified, the transition effect will not take on the default opacity.
>  3. For details about the scale and rotate effects, see [Transformation](ts-universal-attributes-transformation.md).

## TransitionEffect<sup>10+</sup>
Defines the transition effect by using the provided APIs, as listed below.
| API| Type| Static Function| Description|
| -------- | ---------- | -------- | -------- |
| opacity | number | Yes| Opacity of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>Value range: [0, 1]<br>**NOTE**<br>A value less than 0 or greater than 1 evaluates to the value **1**.|
| translate | {<br>x? : number \| string,<br>y? : number \| string,<br>z? : number \| string<br>} | Yes| Translation of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>-**x**: distance to translate along the x-axis.<br>-**y**: distance to translate along the y-axis.<br>-**z**: distance to translate along the z-axis.|
| scale | {<br>x? : number,<br>y? : number,<br>z? : number,<br>centerX? : number \| string,<br>centerY? : number \| string<br>} | Yes| Scaling of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>- **x**: scale factor along the x-axis.<br>- **y**: scale factor along the y-axis.<br>- **z**: scale factor along the z-axis.<br>- **centerX** and **centerY**: scale center point. The default values are both **"50%"**, indicating that the center point of the page.<br>- If the center point is (0, 0), it refers to the upper left corner of the component.|
| rotate | {<br>x?: number,<br>y?: number,<br>z?: number,<br>angle: number \| string,<br>centerX?: number \| string,<br>centerY?: number \| string<br>} | Yes| Rotation of the component during transition, which is the value of the start point of insertion and the end point of deletion.<br>- **x**: rotation vector along the x-axis.<br>- **y**: Y-component of the rotation vector.<br>- **z**: Z-component of the rotation vector.<br>- **centerX** and **centerY**: rotation center point. The default values are both **"50%"**, indicating that the center point of the page.<br>- If the center point is (0, 0), it refers to the upper left corner of the component.|
| move | [TransitionEdge](ts-appendix-enums.md#transitionedge10) | Yes| Slide-in and slide-out of the component from the screen edge during transition. It is essentially a translation effect, which is the value of the start point of insertion and the end point of deletion.|
| asymmetric | appear: TransitionEffect,<br>disappear: TransitionEffect<br>| Yes| Asymmetric transition effect.<br>The first parameter indicates the transition effect for appearance, and the second parameter indicates the transition effect for disappearance.<br>If the **asymmetric** function is not used for **TransitionEffect**, the transition effect takes effect for both appearance and disappearance of the component.|
| combine | TransitionEffect | No| Combination of transition effects.|
| animation | [AnimateParam](ts-explicit-animation.md#animateparam) | No| Animation settings.<br>The **onFinish** callback in **AnimateParam** does not work here.<br>If **combine** is used for combining transition effects, the animation settings of a transition effect are applicable to the one following it.|

The static functions listed in the preceding table are used to create a **TransitionEffect** object, while the non-static functions apply to the created **TransitionEffect** object.
**TransitionEffect** also provides some static member variables for transition effects:
| Static Member Variable| Description|
| -------- | -------- |
| IDENTITY | Disables the transition effect.|
| OPACITY | Applies a transition effect with the opacity of 0. It is equivalent to **TransitionEffect.opacity(0)**.|
| SLIDE | Applies a transition effect of sliding in from the left when the component appears and sliding out from the right when the component disappears. It is equivalent to **TransitionEffect.asymmetric(TransitionEffect.START, TrasitionEffect.END)**.|

>  **NOTE**
>
>  1. For transition effects combined through the **combine** function, animation settings can be configured through the **animation** parameter on a one-on-one basis. In addition, the animation settings of a transition effect are applicable to the one following it. For example, with **TransitionEffect.OPACITY.animation({duration: 1000}).combine(TransitionEffect.translate({x: 100}))**, the **duration** settings (1000 ms) apply to both the OPACITY and translate effects.
>  2. The animation settings take effect in the following sequence: animation settings specified in the current **TransitionEffect** > animation settings specified in the previous **TransitionEffect** > animation settings specified in **animateTo** that triggers the component to appear and disappear.
>  3. If **animateTo** is not used and **TransitionEffect** does not have the **animation** parameter specified, the component will appear or disappear without any transition animation.
>  4. If the value of an attribute specified in **TransitionEffect** is the same as the default value, no transition animation is applied for the attribute. For example, with **TransitionEffect.opacity(1).animation({duration:1000})**, because the default value of **opacity** is also **1**, no opacity animation will be applied, and the component appears or disappears without any transition animation.

## Example
The following is an example of using **TransitionOptions** for appearance and disappearance of the component.
```ts
// xxx.ets
@Entry
@Component
struct TransitionExample {
  @State flag: boolean = true
  @State show: string = 'show'

  build() {
    Column() {
      Button(this.show).width(80).height(30).margin(30)
        .onClick(() => {
          // Click the button to show or hide the image.
          if (this.flag) {
            this.show = 'hide';
          } else {
            this.show = 'show';
          }
          // When set to a value of the TransitionOptions type, the transition attribute must work with animateTo.
          animateTo({ duration: 1000 }, () => {
            this.flag = !this.flag
          })
        })
      if (this.flag) {
        // Apply different transition effects to the appearance and disappearance of the image.
        // When the image appears, it changes from the state where the scale factor is 0 along the x-axis and 1 along the y-axis to the state where the scale factor is both 1 along x-axis and y-axis.
        // When the image disappears, it rotates from 0° to 180° clockwise around the z-axis.
        Image($r('app.media.testImg')).width(200).height(200)
          .transition({ type: TransitionType.Insert, scale: { x: 0, y: 1.0 } })
          .transition({ type: TransitionType.Delete, rotate: { z: 1, angle: 180 } })
      }
    }.width('100%')
  }
}
```

Below you can see the example in action.<br>
![transitionComponent1](figures/transitionComponent1.gif)

The following is an example of using the same transition effect for the appearance and disappearance (which are inverse processes) of the component.
```ts
// xxx.ets
@Entry
@Component
struct TransitionEffectExample1 {
  @State flag: boolean = true;
  @State show: string = 'show';

  build() {
    Column() {
      Button(this.show).width(80).height(30).margin(30)
        .onClick(() => {
          // Click the button to show or hide the image.
          if (this.flag) {
            this.show = 'hide';
          } else {
            this.show = 'show';
          }
          this.flag = !this.flag;
        })
      if (this.flag) {
        // Apply the same transition effect to the appearance and disappearance of the image.
        // When the image appears, it changes from the state where the opacity is 0 and the rotation angle is 180° around the z-axis to the state where the opacity is 1 and the rotation angle is 0°. The durations of the opacity and rotation animations are both 2000 ms.
        // When the image disappears, it changes from the state where the opacity is 1 and the rotation angle is 0° to the state where the opacity is 0 and the rotation angle is 180° around the z-axis. The durations of the opacity and rotation animations are both 2000 ms.
        Image($r('app.media.testImg')).width(200).height(200)
          .transition(TransitionEffect.OPACITY.animation({ duration: 2000, curve: Curve.Ease }).combine(
            TransitionEffect.rotate({ z: 1, angle: 180 })
          ))
      }
    }.width('100%')
  }
}
```
Below you can see the example in action.<br>
![transitionComponent2](figures/transitionComponent2.gif)

The following is an example of using different transition effects for the appearance and disappearance of the component.
```ts
// xxx.ets
@Entry
@Component
struct TransitionEffectExample2 {
  @State flag: boolean = true;
  @State show: string = 'show';

  build() {
    Column() {
      Button(this.show).width(80).height(30).margin(30)
        .onClick(() => {
          // Click the button to show or hide the image.
          if (this.flag) {
            this.show = 'hide';
          } else {
            this.show = 'show';
          }
          animateTo({ duration: 2000 }, () => {
            // In the first image, **TransitionEffect** contains **animation**, and therefore the animation settings are those configured in **TransitionEffect**.
            // In the second image, **TransitionEffect** does not contain **animation**, and therefore the animation settings are those configured in **animateTo**.
            this.flag = !this.flag;
          });
        })
      if (this.flag) {
        // Apply different transition effects to the appearance and disappearance of the image.
        // When the image appears, its opacity changes 0 to 1 (default value) over the duration of 1000 ms, and after 1000 ms has elapsed, its rotation angle changes from 180° around the z-axis to 0° (default value) over the duration of 1000 ms.
        // When the image disappears, after 1000 ms has elapsed, its opacity changes 1 (default value) to 0 over the duration of 1000 ms, and its rotation angle changes from 0° (default value) to 180° around the z-axis over the duration of 1000 ms.
        Image($r('app.media.testImg')).width(200).height(200)
          .transition(
            TransitionEffect.asymmetric(
              TransitionEffect.OPACITY.animation({ duration: 1000 }).combine(
              TransitionEffect.rotate({ z: 1, angle: 180 }).animation({ delay: 1000, duration: 1000 }))
              ,
              TransitionEffect.OPACITY.animation({ delay: 1000, duration: 1000 }).combine(
              TransitionEffect.rotate({ z: 1, angle: 180 }).animation({ duration: 1000 }))
            )
          )
        // When the image appears, the scale along the x- and y- axes is changed from 0 to 1 (default value). The animation duration is 2000 ms specified in **animateTo**.
        // When the image disappears, no transition effect is applied.
        Image($r('app.media.testImg')).width(200).height(200).margin({ top: 100 })
          .transition(
            TransitionEffect.asymmetric(
              TransitionEffect.scale({ x: 0, y: 0 }),
              TransitionEffect.IDENTITY
            )
          )
      }
    }.width('100%')
  }
}
```
Below you can see the example in action.<br>
![transitionComponent3](figures/transitionComponent3.gif)
