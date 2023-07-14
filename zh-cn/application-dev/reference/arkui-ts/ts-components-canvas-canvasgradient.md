# CanvasGradient对象

渐变对象。

>  **说明：**
> 
>  从 API Version 8 开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。



## addColorStop

addColorStop(offset: number, color: string): void

设置渐变断点值，包括偏移和颜色。

从API version 9开始，该接口支持在ArkTS卡片中使用。


**参数：**

| 参数     | 类型     | 必填   | 默认值       | 描述                           |
| ------ | ------ | ---- | --------- | ---------------------------- |
| offset | number | 是    | 0         | 设置渐变点距离起点的位置占总体长度的比例，范围为0到1。 |
| color  | string | 是    | '#ffffff' | 设置渐变的颜色。颜色格式参考[ResourceColor](ts-types.md#resourcecolor)中string类型说明                     |


**示例：**

  ```ts
// xxx.ets
@Entry
@Component
struct Page45 {
  private settings: RenderingContextSettings = new RenderingContextSettings(true)
  private context: CanvasRenderingContext2D = new CanvasRenderingContext2D(this.settings)

  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Canvas(this.context)
        .width('100%')
        .height('100%')
        .backgroundColor('#ffff00')
        .onReady(() => {
          var grad = this.context.createLinearGradient(50, 0, 300, 100)
          grad.addColorStop(0.0, '#ff0000')
          grad.addColorStop(0.5, '#ffffff')
          grad.addColorStop(1.0, '#00ff00')
          this.context.fillStyle = grad
          this.context.fillRect(0, 0, 400, 400)
        })
    }
    .width('100%')
    .height('100%')
  }
}
  ```
  ![zh-cn_image_0000001194032516](figures/zh-cn_image_0000001194032516.png)

 