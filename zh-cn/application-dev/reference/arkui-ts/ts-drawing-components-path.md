# Path

路径绘制组件，根据绘制路径生成封闭的自定义形状。

> **说明：**
>
> 该组件从API Version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 子组件

无

## 接口

Path(value?: { width?: number | string; height?: number | string; commands?: string })

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数:**

| 参数名   | 参数类型         | 必填 | 参数描述                                                     |
| -------- | ---------------- | ---- | ------------------------------------------------------------ |
| width    | number \| string | 否   | 路径所在矩形的宽度<br/>默认值：0<br/>**说明：**<br/>异常值按照默认值处理。 |
| height   | number \| string | 否   | 路径所在矩形的高度<br/>默认值：0<br/>**说明：**<br/>异常值按照默认值处理。 |
| commands | string           | 否   | 路径绘制的命令字符串<br/>默认值：''<br/>**说明：**<br/>异常值按照默认值处理。 |



## 属性

除支持[通用属性](ts-universal-attributes-size.md)外，还支持以下属性：

| 名称     | 类型                                | 默认值  | 描述                                     |
| -------- | ----------------------------------- | ---- | ---------------------------------------- |
| commands | string                              | ''   | 路径绘制的命令字符串，单位为px。像素单位转换方法请参考[像素单位转换](ts-pixel-units.md)。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| fill | [ResourceColor](ts-types.md) | Color.Black | 设置填充区域颜色。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| fillOpacity | number&nbsp;\|&nbsp;string&nbsp;\|&nbsp;[Resource](ts-types.md#resource类型) | 1 | 设置填充区域透明度。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| stroke | [ResourceColor](ts-types.md) | - |设置边框颜色，不设置时，默认没有边框。 <br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：** <br/>异常值不会绘制边框。|
| strokeDashArray | Array&lt;Length&gt; | [] | 设置线条间隙。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| strokeDashOffset | number&nbsp;\|&nbsp;string | 0 | 线条绘制起点的偏移量。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>异常值按照默认值处理。 |
| strokeLineCap | [LineCapStyle](ts-appendix-enums.md#linecapstyle) | LineCapStyle.Butt | 设置线条端点绘制样式。 <br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| strokeLineJoin | [LineJoinStyle](ts-appendix-enums.md#linejoinstyle) | LineJoinStyle.Miter | 设置线条拐角绘制样式。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| strokeMiterLimit | number&nbsp;\|&nbsp;string | 4 | 设置斜接长度与边框宽度比值的极限值。斜接长度表示外边框外边交点到内边交点的距离，边框宽度即strokeWidth属性的值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>该属性取值需在strokeLineJoin属性取值LineJoinStyle.Miter时生效。 <br/>该属性的合法值范围应当大于等于1.0，当取值范围在[0,1)时按1.0处理，其余异常值按默认值处理。 |
| strokeOpacity | number&nbsp;\|&nbsp;string&nbsp;\|&nbsp;[Resource](ts-types.md#resource类型) | 1 | 设置线条透明度。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>该属性的取值范围是[0.0, 1.0]，若给定值小于0.0，则取值为0.0；若给定值大于1.0，则取值为1.0，其余异常值按1.0处理 。 |
| strokeWidth | Length | 1 | 设置线条宽度。 <br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：**<br/>该属性若为string类型, 暂不支持百分比。<br/>异常值按照默认值处理。 |
| antiAlias | boolean | true | 是否开启抗锯齿效果。 <br/>从API version 9开始，该接口支持在ArkTS卡片中使用。|

commands支持的绘制命令如下：

| 命令   | 名称                               | 参数                                       | 说明                                       |
| ---- | -------------------------------- | ---------------------------------------- | ---------------------------------------- |
| M    | moveto                           | （x y）                                    | 在给定的 (x, y) 坐标处开始一个新的子路径。例如，`M 0 0` 表示将（0, 0）点作为新子路径的起始点。 |
| L    | lineto                           | （x y）                                    | 从当前点到给定的 (x, y) 坐标画一条线，该坐标成为新的当前点。例如，`L 50 50` 表示绘制当前点到（50, 50）点的直线，并将（50, 50）点作为新子路径的起始点。 |
| H    | horizontal lineto                | x                                        | 从当前点绘制一条水平线，等效于将y坐标指定为0的L命令。例如，`H 50 ` 表示绘制当前点到（50, 0）点的直线，并将（50, 0）点作为新子路径的起始点。 |
| V    | vertical lineto                  | y                                        | 从当前点绘制一条垂直线，等效于将x坐标指定为0的L命令。例如，`V 50 ` 表示绘制当前点到（0, 50）点的直线，并将（0, 50）点作为新子路径的起始点。 |
| C    | curveto                          | (x1 y1 x2 y2 x y)                        | 使用 (x1, y1) 作为曲线起点的控制点， (x2, y2) 作为曲线终点的控制点，从当前点到 (x, y) 绘制三次贝塞尔曲线。例如，`C100 100 250 100 250 200 ` 表示绘制当前点到（250, 200）点的三次贝塞尔曲线，并将（250, 200）点作为新子路径的起始点。 |
| S    | smooth curveto                   | (x2 y2 x y)                              | (x2, y2) 作为曲线终点的控制点，绘制从当前点到 (x, y) 绘制三次贝塞尔曲线。若前一个命令是C或S，则起点控制点是上一个命令的终点控制点相对于起点的映射。 例如，`C100 100 250 100 250 200 S400 300 400 200`第二段贝塞尔曲线的起点控制点为（250, 300）。如果没有前一个命令或者前一个命令不是 C或S，则第一个控制点与当前点重合。 |
| Q    | quadratic Belzier curve          | (x1 y1 x y)                              | 使用 (x1, y1) 作为控制点，从当前点到 (x, y) 绘制二次贝塞尔曲线。例如，`Q400 50 600 300 ` 表示绘制当前点到（600, 300）点的二次贝塞尔曲线，并将（600, 300）点作为新子路径的起始点。 |
| T    | smooth quadratic Belzier curveto | (x y)                                    | 绘制从当前点到 (x, y) 绘制二次贝塞尔曲线。若前一个命令是Q或T，则控制点是上一个命令的终点控制点相对于起点的映射。 例如，`Q400 50 600 300 T1000 300`第二段贝塞尔曲线的控制点为（800, 350）。 如果没有前一个命令或者前一个命令不是 Q或T，则第一个控制点与当前点重合。 |
| A    | elliptical Arc                   | (rx ry x-axis-rotation large-arc-flag sweep-flag x y) | 从当前点到 (x, y) 绘制一条椭圆弧。椭圆的大小和方向由两个半径 (rx, ry) 和x-axis-rotation定义，指示整个椭圆相对于当前坐标系如何旋转（以度为单位）。 large-arc-flag 和 sweep-flag确定弧的绘制方式。 |
| Z    | closepath                        | none                                     | 通过将当前路径连接回当前子路径的初始点来关闭当前子路径。             |

例如： commands('M0 20 L50 50 L50 100 Z')定义了一个三角形，起始于位置（0，20），接着绘制点（0，20）到点（50，50）的直线，再绘制点（50，50）到点（50，100）的直线，最后绘制点（50，100）到（0，20）的直线关闭路径，形成封闭三角形。

## 示例

```ts
// xxx.ets
@Entry
@Component
struct PathExample {
  build() {
    Column({ space: 10 }) {
      Text('Straight line')
        .fontSize(11)
        .fontColor(0xCCCCCC)
        .width('90%')
      // 绘制一条长600px，宽3vp的直线
      Path()
        .width('600px')
        .height('10px')
        .commands('M0 0 L600 0')
        .stroke(Color.Black)
        .strokeWidth(3)

      Text('Straight line graph')
        .fontSize(11)
        .fontColor(0xCCCCCC)
        .width('90%')
      // 绘制直线图形
      Flex({ justifyContent: FlexAlign.SpaceBetween }) {
        Path()
          .width('210px')
          .height('310px')
          .commands('M100 0 L200 240 L0 240 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .width('210px')
          .height('310px')
          .commands('M0 0 H200 V200 H0 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .width('210px')
          .height('310px')
          .commands('M100 0 L0 100 L50 200 L150 200 L200 100 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
      }.width('95%')

      Text('Curve graphics').fontSize(11).fontColor(0xCCCCCC).width('90%')
      // 绘制弧线图形
      Flex({ justifyContent: FlexAlign.SpaceBetween }) {
        Path()
          .width('250px')
          .height('310px')
          .commands("M0 300 S100 0 240 300 Z")
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .width('210px')
          .height('310px')
          .commands('M0 150 C0 100 140 0 200 150 L100 300 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
        Path()
          .width('210px')
          .height('310px')
          .commands('M0 100 A30 20 20 0 0 200 100 Z')
          .fillOpacity(0)
          .stroke(Color.Black)
          .strokeWidth(3)
      }.width('95%')
    }.width('100%')
    .margin({ top: 5 })
  }
}
```

![zh-cn_image_0000001219744193](figures/zh-cn_image_0000001219744193.png)