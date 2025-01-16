# 触摸事件

当手指在组件上按下、滑动、抬起时触发。

> **说明：**
>
> 从API Version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 事件

| 名称                                                         | 是否冒泡 | 功能描述                                                     |
| ------------------------------------------------------------ | -------- | ------------------------------------------------------------ |
| onTouch(event:&nbsp;(event?:&nbsp;TouchEvent)&nbsp;=&gt;&nbsp;void) | 是       | 手指触摸动作触发该回调，event返回值见[TouchEvent](#touchevent对象说明)介绍。 |


## TouchEvent对象说明

| 名称                | 类型                                       | 描述           |
| ------------------- | ---------------------------------------- | ------------ |
| type                | [TouchType](ts-appendix-enums.md#touchtype)      | 触摸事件的类型。     |
| touches             | Array&lt;[TouchObject](#touchobject对象说明)&gt; | 全部手指信息。      |
| changedTouches      | Array&lt;[TouchObject](#touchobject对象说明)&gt; | 当前发生变化的手指信息。 |
| stopPropagation      | () => void | 阻塞事件冒泡。 |
| timestamp<sup>8+</sup> | number | 事件时间戳，触发事件时距离系统启动的时间间隔。<br/>例如，当系统启动时间为2023/10/12 11:33, 在2023/10/12 11:34时触发触摸事件，时间戳返回的值为60,000,000,000ns。<br>单位：纳秒 |
| target<sup>8+</sup> | [EventTarget](ts-universal-events-click.md#eventtarget8对象说明) | 触发事件的元素对象显示区域。 |
| source<sup>8+</sup> | [SourceType](ts-gesture-settings.md#sourcetype枚举说明) | 事件输入设备。 |
| getHistoricalPoints<sup>10+</sup> | Array&lt;[HistoricalPoint](#historicalpoint10对象说明)&gt;| 获取当前帧所有的历史点。不同设备每帧的触摸事件频率不同，当前帧所有的触摸事件被称为历史点。 |


## TouchObject对象说明

| 名称    | 类型                                        | 描述                                  |
| ------- | ------------------------------------------- | ------------------------------------- |
| type    | [TouchType](ts-appendix-enums.md#touchtype) | 触摸事件的类型。                      |
| id      | number                                      | 手指唯一标识符。                      |
| x       | number                                      | 触摸点相对于被触摸元素左上角的X坐标。 |
| y       | number                                      | 触摸点相对于被触摸元素左上角的Y坐标。 |
| windowX<sup>10+</sup>  | number                       | 触摸点相对于应用窗口左上角的X坐标。   |
| windowY<sup>10+</sup>  | number                       | 触摸点相对于应用窗口左上角的Y坐标。   |
| displayX<sup>10+</sup> | number                       | 触摸点相对于应用屏幕左上角的X坐标。   |
| displayY<sup>10+</sup> | number                       | 触摸点相对于应用屏幕左上角的Y坐标。   |

## HistoricalPoint<sup>10+</sup>对象说明

| 名称         | 类型                                 | 描述                                                                         |
| ----------- | ----------------------------------- | ----------------------------------------------------------------------------- |
| touchObject | [TouchObject](#touchobject对象说明)  | 历史点对应触摸事件的基础信息。                                                   |
| size        | number                              | 历史点对应触摸事件的手指与屏幕的触摸区域大小。<br/>默认值：0                                     |
| force       | number                              | 历史点对应触摸事件的压力大小。<br/>默认值：0<br/>取值范围：[0,65535]，压力越大值越大。<br/>此接口根据硬件设备不同，支持情况不同。当前只支持Tablet。|
| timestamp   | number                              | 历史点对应触摸事件的时间戳。触发事件时距离系统启动的时间间隔。<br>单位：毫秒             |
## 示例

```ts
// xxx.ets
@Entry
@Component
struct TouchExample {
  @State text: string = ''
  @State eventType: string = ''

  build() {
    Column() {
      Button('Touch').height(40).width(100)
        .onTouch((event?: TouchEvent) => {
          if(event){
            if (event.type === TouchType.Down) {
              this.eventType = 'Down'
            }
            if (event.type === TouchType.Up) {
              this.eventType = 'Up'
            }
            if (event.type === TouchType.Move) {
              this.eventType = 'Move'
            }
            this.text = 'TouchType:' + this.eventType + '\nDistance between touch point and touch element:\nx: '
            + event.touches[0].x + '\n' + 'y: ' + event.touches[0].y + '\nComponent globalPos:('
            + event.target.area.globalPosition.x + ',' + event.target.area.globalPosition.y + ')\nwidth:'
            + event.target.area.width + '\nheight:' + event.target.area.height
          }
        })
      Button('Touch').height(50).width(200).margin(20)
        .onTouch((event?: TouchEvent) => {
          if(event){
            if (event.type === TouchType.Down) {
              this.eventType = 'Down'
            }
            if (event.type === TouchType.Up) {
              this.eventType = 'Up'
            }
            if (event.type === TouchType.Move) {
              this.eventType = 'Move'
            }
            this.text = 'TouchType:' + this.eventType + '\nDistance between touch point and touch element:\nx: '
            + event.touches[0].x + '\n' + 'y: ' + event.touches[0].y + '\nComponent globalPos:('
            + event.target.area.globalPosition.x + ',' + event.target.area.globalPosition.y + ')\nwidth:'
            + event.target.area.width + '\nheight:' + event.target.area.height
          }
        })
      Text(this.text)
    }.width('100%').padding(30)
  }
}
```

![zh-cn_image_0000001209874754](figures/zh-cn_image_0000001209874754.gif)
