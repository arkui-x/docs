

# PanGesture

用于触发拖动手势事件，滑动的最小距离为5vp时拖动手势识别成功。

>  **说明：**
>
>  从API Version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 接口

PanGesture(value?: { fingers?: number; direction?: PanDirection; distance?: number } | [PanGestureOptions](#pangestureoptions))

**参数：**

| 参数名称 | 参数类型 | 必填 | 参数描述 |
| -------- | -------- | -------- | -------- |
| fingers | number | 否 | 触发拖动的最少手指数，最小为1指，&nbsp;最大取值为10指。<br/>默认值：1<br/>取值范围：[1,10]<br/>**说明：** <br/>当设置的值小于1或不设置时，会被转化为默认值。 |
| direction | PanDirection | 否 | 触发拖动的手势方向，此枚举值支持逻辑与(&amp;)和逻辑或（\|）运算。<br/>默认值：PanDirection.All |
| distance | number | 否 | 最小拖动识别距离，单位为vp。<br/>默认值：5<br/>**说明：**<br/>[Tabs组件](ts-container-tabs.md)滑动与该拖动手势事件同时存在时，可将distance值设为1，使拖动更灵敏，避免造成事件错乱。 |

## PanDirection枚举说明

| 名称 | 描述 |
| -------- | -------- |
| All | 所有方向。 |
| Horizontal | 水平方向。 |
| Vertical | 竖直方向。 |
| Left | 向左拖动。 |
| Right | 向右拖动。 |
| Up | 向上拖动。 |
| Down | 向下拖动。 |
| None | 任何方向都不可触发拖动手势事件。 |


## PanGestureOptions

通过PanGestureOptions对象接口可以动态修改滑动手势识别器的属性，从而避免通过状态变量修改属性（状态变量修改会导致UI刷新）。

PanGestureOptions(value?: { fingers?: number; direction?: PanDirection; distance?: number })

**参数：**

| 参数名称  | 参数类型     | 必填 | 参数描述                                                     |
| --------- | ------------ | ---- | ------------------------------------------------------------ |
| fingers   | number       | 否   | 触发滑动的最少手指数，最小为1指，&nbsp;最大取值为10指。<br/>默认值：1 |
| direction | PanDirection | 否   | 设置滑动方向，此枚举值支持逻辑与(&amp;)和逻辑或（\|）运算。<br/>默认值：All |
| distance  | number       | 否   | 最小滑动识别距离，单位为vp。<br/>默认值：5.0<br/>**说明：**<br/>> [Tabs组件](ts-container-tabs.md)滑动与该拖动手势事件同时存在时，可将distance值设为1，使拖动更灵敏，避免造成事件错乱。 |

**接口**

| 名称 | 功能描述 |
| -------- | -------- |
| setDirection(value:&nbsp;PanDirection) | 设置direction属性。 |
| setDistance(value:&nbsp;number) | 设置distance属性。 |
| setFingers(value:&nbsp;number) | 设置fingers属性。 |


## 事件

| 名称 | 功能描述 |
| -------- | -------- |
| onActionStart(event:&nbsp;(event?:&nbsp;[GestureEvent](ts-gesture-settings.md#gestureevent对象说明))&nbsp;=&gt;&nbsp;void) | Pan手势识别成功回调。 |
| onActionUpdate(event:&nbsp;(event?:&nbsp;[GestureEvent](ts-gesture-settings.md#gestureevent对象说明))&nbsp;=&gt;&nbsp;void) | Pan手势移动过程中回调。 |
| onActionEnd(event:&nbsp;(event?:&nbsp;[GestureEvent](ts-gesture-settings.md#gestureevent对象说明))&nbsp;=&gt;&nbsp;void) | Pan手势识别成功，手指抬起后触发回调。 |
| onActionCancel(event:&nbsp;()&nbsp;=&gt;&nbsp;void) | Pan手势识别成功，接收到触摸取消事件触发回调。 |


## 示例

```ts
// xxx.ets
@Entry
@Component
struct PanGestureExample {
  @State offsetX: number = 0
  @State offsetY: number = 0
  @State positionX: number = 0
  @State positionY: number = 0
  private panOption: PanGestureOptions = new PanGestureOptions({ direction: PanDirection.Left | PanDirection.Right })

  build() {
    Column() {
      Column() {
        Text('PanGesture offset:\nX: ' + this.offsetX + '\n' + 'Y: ' + this.offsetY)
      }
      .height(200)
      .width(300)
      .padding(20)
      .border({ width: 3 })
      .margin(50)
      .translate({ x: this.offsetX, y: this.offsetY, z: 0 })
      // 左右拖动触发该手势事件
      .gesture(
      PanGesture(this.panOption)
        .onActionStart((event: GestureEvent) => {
          console.info('Pan start')
        })
        .onActionUpdate((event: GestureEvent) => {
          this.offsetX = this.positionX + event.offsetX
          this.offsetY = this.positionY + event.offsetY
        })
        .onActionEnd(() => {
          this.positionX = this.offsetX
          this.positionY = this.offsetY
          console.info('Pan end')
        })
      )

      Button('修改PanGesture触发条件')
        .onClick(() => {
          // 将PanGesture手势事件触发条件改为双指以任意方向拖动
          this.panOption.setDirection(PanDirection.All)
          this.panOption.setFingers(2)
        })
    }
  }
}
```

示意图：

向左拖动：

![zh-cn_image_0000001174264374](figures/zh-cn_image_0000001174264374.png) 

点击按钮修改PanGesture触发条件，双指向左下方拖动：

 ![zh-cn_image1_0000001174264374](figures/zh-cn_image1_0000001174264374.png) 