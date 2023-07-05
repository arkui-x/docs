# Line

直线绘制组件。

>  **说明：**
>
> 该组件从API Version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。

## 子组件

无


## 接口

Line(value?: {width?: string | number, height?: string | number})

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数名 | 参数类型 | 必填 | 默认值 | 参数描述 |
| -------- | -------- | -------- | -------- | -------- |
| width | string \| number | 否 | 0 | 宽度。<br/>**说明：**<br/>异常值按照默认值处理。 |
| height | string \| number | 否 | 0 | 高度。<br/>**说明：**<br/>异常值按照默认值处理。 |



## 属性

除支持[通用属性](ts-universal-attributes-size.md)外，还支持以下属性：

| 名称 | 类型 | 默认值 | 描述 |
| -------- | -------- | -------- | -------- |
| startPoint | Array&lt;Length&gt; | [0,&nbsp;0] | 直线起点坐标点(相对坐标)，单位vp。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| endPoint   | Array&lt;Length&gt; | [0,&nbsp;0] | 直线终点坐标点(相对坐标)，单位vp。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| fill | [ResourceColor](ts-types.md#resourcecolor) | Color.Black | 设置填充区域颜色。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>Line组件无法形成闭合区域，该属性设置无效。 |
| fillOpacity | number&nbsp;\|&nbsp;string&nbsp;\|&nbsp;[Resource](ts-types.md#resource类型) | 1 | 设置填充区域透明度。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>Line组件无法形成闭合区域，该属性设置无效。 |
| stroke | [ResourceColor](ts-types.md) | - | 设置边框颜色，不设置时，默认没有边框线条。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值不会绘制边框线条。 |
| strokeDashArray | Array&lt;Length&gt; | [] | 设置线条间隙。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| strokeDashOffset | number&nbsp;\|&nbsp;string | 0 | 线条绘制起点的偏移量。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| strokeLineCap | [LineCapStyle](ts-appendix-enums.md#linecapstyle) | LineCapStyle.Butt | 设置线条端点绘制样式。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| strokeLineJoin | [LineJoinStyle](ts-appendix-enums.md#linejoinstyle) | LineJoinStyle.Miter | 设置线条拐角绘制样式。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>Line组件无法形成拐角，该属性设置无效。 |
| strokeMiterLimit | number&nbsp;\|&nbsp;string | 4 | 设置锐角绘制成斜角的极限值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>Line组件无法设置锐角图形，该属性设置无效。 |
| strokeOpacity | number&nbsp;\|&nbsp;string&nbsp;\|&nbsp;[Resource](ts-types.md#resource类型) | 1 | 设置线条透明度。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>该属性的取值范围是[0.0, 1.0]，若给定值小于0.0，则取值为0.0；若给定值大于1.0，则取值为1.0，其余异常值按1.0处理 。 |
| strokeWidth | Length | 1 | 设置线条宽度。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 <br/>**说明：**<br/>该属性若为string类型, 暂不支持百分比。<br/>异常值按照默认值处理。 |
| antiAlias | boolean | true | 是否开启抗锯齿效果。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |



## 示例

### 示例1

```ts
// xxx.ets
@Entry
@Component
struct LineExample {
  build() {
    Column({ space: 10 }) {
      // 线条绘制的起止点坐标均是相对于Line组件本身绘制区域的坐标
      Line()
        .width(200)
        .height(150)
        .startPoint([0, 0])
        .endPoint([50, 100])
        .stroke(Color.Black)
        .backgroundColor('#F5F5F5')
      Line()
        .width(200)
        .height(150)
        .startPoint([50, 50])
        .endPoint([150, 150])
        .strokeWidth(5)
        .stroke(Color.Orange)
        .strokeOpacity(0.5)
        .backgroundColor('#F5F5F5')
      // strokeDashOffset用于定义关联虚线strokeDashArray数组渲染时的偏移
      Line()
        .width(200)
        .height(150)
        .startPoint([0, 0])
        .endPoint([100, 100])
        .stroke(Color.Black)
        .strokeWidth(3)
        .strokeDashArray([10, 3])
        .strokeDashOffset(5)
        .backgroundColor('#F5F5F5')
      // 当坐标点设置的值超出Line组件的宽高范围时，线条会画出组件绘制区域
      Line()
        .width(50)
        .height(50)
        .startPoint([0, 0])
        .endPoint([100, 100])
        .stroke(Color.Black)
        .strokeWidth(3)
        .strokeDashArray([10, 3])
        .backgroundColor('#F5F5F5')
    }
  }
}
```

![zh-cn_image_0000001219982725](figures/zh-cn_image_0000001219982725.png)

### 示例2

```ts
// xxx.ets
@Entry
@Component
struct LineExample1 {
  build() {
    Row({ space: 10 }) {
      // 当LineCapStyle值为Butt时
      Line()
        .width(100)
        .height(200)
        .startPoint([50, 50])
        .endPoint([50, 200])
        .stroke(Color.Black)
        .strokeWidth(20)
        .strokeLineCap(LineCapStyle.Butt)
        .backgroundColor('#F5F5F5').margin(10)
      // 当LineCapStyle值为Round时
      Line()
        .width(100)
        .height(200)
        .startPoint([50, 50])
        .endPoint([50, 200])
        .stroke(Color.Black)
        .strokeWidth(20)
        .strokeLineCap(LineCapStyle.Round)
        .backgroundColor('#F5F5F5')
      // 当LineCapStyle值为Square时
      Line()
        .width(100)
        .height(200)
        .startPoint([50, 50])
        .endPoint([50, 200])
        .stroke(Color.Black)
        .strokeWidth(20)
        .strokeLineCap(LineCapStyle.Square)
        .backgroundColor('#F5F5F5')
    }
  }
}
```

![zh-cn_image1_0000001219982725](figures/zh-cn_image1_0000001219982725.png)

### 示例3

```ts
// xxx.ets
@Entry
@Component
struct LineExample {
  build() {
    Column() {
      Line()
        .width(300)
        .height(30)
        .startPoint([50, 30])
        .endPoint([300, 30])
        .stroke(Color.Black)
        .strokeWidth(10)
      // 设置strokeDashArray的数组间隔为 50
      Line()
        .width(300)
        .height(30)
        .startPoint([50, 20])
        .endPoint([300, 20])
        .stroke(Color.Black)
        .strokeWidth(10)
        .strokeDashArray([50])
      // 设置strokeDashArray的数组间隔为 50, 10
      Line()
        .width(300)
        .height(30)
        .startPoint([50, 20])
        .endPoint([300, 20])
        .stroke(Color.Black)
        .strokeWidth(10)
        .strokeDashArray([50, 10])
      // 设置strokeDashArray的数组间隔为 50, 10, 20
      Line()
        .width(300)
        .height(30)
        .startPoint([50, 20])
        .endPoint([300, 20])
        .stroke(Color.Black)
        .strokeWidth(10)
        .strokeDashArray([50, 10, 20])
      // 设置strokeDashArray的数组间隔为 50, 10, 20, 30
      Line()
        .width(300)
        .height(30)
        .startPoint([50, 20])
        .endPoint([300, 20])
        .stroke(Color.Black)
        .strokeWidth(10)
        .strokeDashArray([50, 10, 20, 30])

    }
  }
}
```

![zh-cn_image2_0000001219982725](figures/zh-cn_image2_0000001219982725.PNG)
