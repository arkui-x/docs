# OffscreenCanvasRenderingContext2D对象

使用OffscreenCanvasRenderingContext2D在Canvas上进行离屏绘制，绘制对象可以是矩形、文本、图片等。离屏绘制是指将需要绘制的内容先绘制在缓存区，然后将其转换成图片，一次性绘制到canvas上，加快了绘制速度。

>  **说明：**
>
>  从 API Version 8 开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。



## 接口

OffscreenCanvasRenderingContext2D(width: number, height: number, settings?: RenderingContextSettings)

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数名      | 参数类型                                     | 必填   | 参数描述                           |
| -------- | ---------------------------------------- | ---- | ------------------------------ |
| width    | number                                   | 是    | 离屏画布的宽度                        |
| height   | number                                   | 是    | 离屏画布的高度                        |
| settings | [RenderingContextSettings](ts-canvasrenderingcontext2d.md#renderingcontextsettings) | 否    | 见RenderingContextSettings接口描述。 |


## 属性

| 名称                                                  | 类型                  | 描述                                                         |
| ----------------------------------------------------- | --------------------- | ------------------------------------------------------------ |
| lineWidth                                             | number                | 设置绘制线条的宽度。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| lineCap                                               | CanvasLineCap         | 指定线端点的样式，可选值为：<br/>-&nbsp;'butt'：线端点以方形结束。<br/>-&nbsp;'round'：线端点以圆形结束。<br/>-&nbsp;'square'：线端点以方形结束，该样式下会增加一个长度和线段厚度相同，宽度是线段厚度一半的矩形。<br/>-&nbsp;默认值：'butt'。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| lineJoin                                              | CanvasLineJoin        | 指定线段间相交的交点样式，可选值为：<br/>-&nbsp;'round'：在线段相连处绘制一个扇形，扇形的圆角半径是线段的宽度。<br/>-&nbsp;'bevel'：在线段相连处使用三角形为底填充，&nbsp;每个部分矩形拐角独立。<br/>-&nbsp;'miter'：在相连部分的外边缘处进行延伸，使其相交于一点，形成一个菱形区域，该属性可以通过设置miterLimit属性展现效果。<br/>-&nbsp;默认值：'miter'。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| miterLimit                                            | number                | 设置斜接面限制值，该值指定了线条相交处内角和外角的距离。  <br/>-&nbsp;默认值：10。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| [font](#font)                                         | string                | 设置文本绘制中的字体样式。<br/>语法：ctx.font='font-size&nbsp;font-family'<br/>-&nbsp;font-size(可选)，指定字号和行高，单位只支持px。<br/>-&nbsp;font-family(可选)，指定字体系列。<br/>语法：ctx.font='font-style&nbsp;font-weight&nbsp;font-size&nbsp;font-family'<br/>-&nbsp;font-style(可选)，用于指定字体样式，支持如下几种样式：'normal','italic'。<br/>-&nbsp;font-weight(可选)，用于指定字体的粗细，支持如下几种类型：'normal',&nbsp;'bold',&nbsp;'bolder',&nbsp;'lighter',&nbsp;100,&nbsp;200,&nbsp;300,&nbsp;400,&nbsp;500,&nbsp;600,&nbsp;700,&nbsp;800,&nbsp;900。<br/>-&nbsp;font-size(可选)，指定字号和行高，单位支持px、vp。使用时需要添加单位。<br/>-&nbsp;font-family(可选)，指定字体系列，支持如下几种类型：'sans-serif',&nbsp;'serif',&nbsp;'monospace'。<br/>-&nbsp;默认值：'normal normal 14px sans-serif'。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| textBaseline                                          | CanvasTextBaseline    | 设置文本绘制中的水平对齐方式，可选值为：<br/>-&nbsp;'alphabetic'：文本基线是标准的字母基线。<br/>-&nbsp;'top'：文本基线在文本块的顶部。<br/>-&nbsp;'hanging'：文本基线是悬挂基线。<br/>-&nbsp;'middle'：文本基线在文本块的中间。<br/>-&nbsp;'ideographic'：文字基线是表意字基线；如果字符本身超出了alphabetic基线，那么ideograhpic基线位置在字符本身的底部。<br/>-&nbsp;'bottom'：文本基线在文本块的底部。&nbsp;与ideographic基线的区别在于ideographic基线不需要考虑下行字母。<br/>-&nbsp;默认值：'alphabetic'。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| globalAlpha                                           | number                | 设置透明度，0.0为完全透明，1.0为完全不透明。                 |
| [lineDashOffset](#linedashoffset)                     | number                | 设置画布的虚线偏移量，精度为float。    <br/>-&nbsp;默认值：0.0。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| [globalCompositeOperation](#globalcompositeoperation) | string                | 设置合成操作的方式。类型字段可选值有'source-over'，'source-atop'，'source-in'，'source-out'，'destination-over'，'destination-atop'，'destination-in'，'destination-out'，'lighter'，'copy'，'xor'。<br/>-&nbsp;默认值：'source-over'。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| shadowBlur                                            | number                | 设置绘制阴影时的模糊级别，值越大越模糊，精度为float。   <br/>-&nbsp;默认值：0.0。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| shadowColor                                           | string                | 设置绘制阴影时的阴影颜色。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| shadowOffsetX                                         | number                | 设置绘制阴影时和原有对象的水平偏移值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| shadowOffsetY                                         | number                | 设置绘制阴影时和原有对象的垂直偏移值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| [imageSmoothingEnabled](#imagesmoothingenabled)       | boolean               | 用于设置绘制图片时是否进行图像平滑度调整，true为启用，false为不启用。 <br/>-&nbsp;默认值：true。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| [imageSmoothingQuality](#imagesmoothingquality)       | ImageSmoothingQuality | imageSmoothingEnabled为true时，用于设置图像平滑度。可选值为：<br/>- 'low'：低画质<br/>- 'medium'：中画质<br/>- 'high'：高画质。<br/>默认值：low<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| [direction](#direction)                               | CanvasDirection       | 用于设置绘制文字时使用的文字方向。可选值为：<br/>- 'inherit'：继承canvas组件已设定的文本方向<br/>- 'ltr'：从左往右<br/>- 'rtl'：从右往左。<br/>默认值：inherit<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| [filter](#filter)                                     | string                | 用于设置图像的滤镜。支持的滤镜效果如下：<br/>- 'none': 无滤镜效果<br/>- 'blur'：给图像设置高斯模糊<br/>- 'brightness'：给图片应用一种线性乘法，使其看起来更亮或更暗<br/>- 'contrast'：调整图像的对比度<br/>- 'grayscale'：将图像转换为灰度图像<br/>- 'hue-rotate'：给图像应用色相旋转<br/>- 'invert'：反转输入图像<br/>- 'opacity'：转化图像的透明程度<br/>- 'saturate'：转换图像饱和度<br/>- 'sepia'：将图像转换为深褐色<br/>默认值：'none'<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |


### globalCompositeOperation

| 名称               | 描述                       |
| ---------------- | ------------------------ |
| source-over      | 在现有绘制内容上显示新绘制内容，属于默认值。   |
| source-atop      | 在现有绘制内容顶部显示新绘制内容。        |
| source-in        | 在现有绘制内容中显示新绘制内容。         |
| source-out       | 在现有绘制内容之外显示新绘制内容。        |
| destination-over | 在新绘制内容上方显示现有绘制内容。        |
| destination-atop | 在新绘制内容顶部显示现有绘制内容。        |
| destination-in   | 在新绘制内容中显示现有绘制内容。         |
| destination-out  | 在新绘制内容外显示现有绘制内容。         |
| lighter          | 显示新绘制内容和现有绘制内容。          |
| copy             | 显示新绘制内容而忽略现有绘制内容。        |
| xor              | 使用异或操作对新绘制内容与现有绘制内容进行融合。 |


### fillRect

fillRect(x: number, y: number, w: number, h: number): void

填充一个矩形。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数     | 类型     | 必填   | 默认值  | 说明            |
| ------ | ------ | ---- | ---- | ------------- |
| x      | number | 是    | 0    | 指定矩形左上角点的x坐标。 |
| y      | number | 是    | 0    | 指定矩形左上角点的y坐标。 |
| width  | number | 是    | 0    | 指定矩形的宽度。      |
| height | number | 是    | 0    | 指定矩形的高度。      |


### strokeRect

strokeRect(x: number, y: number, w: number, h: number): void

绘制具有边框的矩形，矩形内部不填充。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数     | 类型     | 必填   | 默认值  | 说明           |
| ------ | ------ | ---- | ---- | ------------ |
| x      | number | 是    | 0    | 指定矩形的左上角x坐标。 |
| y      | number | 是    | 0    | 指定矩形的左上角y坐标。 |
| width  | number | 是    | 0    | 指定矩形的宽度。     |
| height | number | 是    | 0    | 指定矩形的高度。     |


### clearRect

clearRect(x: number, y: number, w: number, h: number): void

删除指定区域内的绘制内容。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数     | 类型     | 必填   | 默认值  | 描述            |
| ------ | ------ | ---- | ---- | ------------- |
| x      | number | 是    | 0    | 指定矩形上的左上角x坐标。 |
| y      | number | 是    | 0    | 指定矩形上的左上角y坐标。 |
| width  | number | 是    | 0    | 指定矩形的宽度。      |
| height | number | 是    | 0    | 指定矩形的高度。      |


### fillText

fillText(text: string, x: number, y: number, maxWidth?: number): void

绘制填充类文本。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数       | 类型     | 必填   | 默认值  | 说明              |
| -------- | ------ | ---- | ---- | --------------- |
| text     | string | 是    | ""   | 需要绘制的文本内容。      |
| x        | number | 是    | 0    | 需要绘制的文本的左下角x坐标。 |
| y        | number | 是    | 0    | 需要绘制的文本的左下角y坐标。 |
| maxWidth | number | 否    | -    | 指定文本允许的最大宽度。    |


### strokeText

strokeText(text: string, x: number, y: number): void

绘制描边类文本。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数       | 类型     | 必填   | 默认值  | 描述              |
| -------- | ------ | ---- | ---- | --------------- |
| text     | string | 是    | ""   | 需要绘制的文本内容。      |
| x        | number | 是    | 0    | 需要绘制的文本的左下角x坐标。 |
| y        | number | 是    | 0    | 需要绘制的文本的左下角y坐标。 |
| maxWidth | number | 否    | -    | 需要绘制的文本的最大宽度 。  |


### measureText

measureText(text: string): TextMetrics

该方法返回一个文本测算的对象，通过该对象可以获取指定文本的宽度值。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 说明         |
| ---- | ------ | ---- | ---- | ---------- |
| text | string | 是    | ""   | 需要进行测量的文本。 |

 **返回值：**

| 类型          | 说明                                       |
| ----------- | ---------------------------------------- |
| TextMetrics | 文本的尺寸信息<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |

**TextMetrics类型描述:**

| 属性                       | 类型     | 描述                                       |
| ------------------------ | ------ | ---------------------------------------- |
| width                    | number | 字符串的宽度。                                  |
| height                   | number | 字符串的高度。                                  |
| actualBoundingBoxAscent  | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到渲染文本的矩形边界顶部的距离，当前值为0。 |
| actualBoundingBoxDescent | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到渲染文本的矩形边界底部的距离，当前值为0。 |
| actualBoundingBoxLeft    | number | 平行于基线，从CanvasRenderingContext2D.textAlign 属性确定的对齐点到文本矩形边界左侧的距离，当前值为0。 |
| actualBoundingBoxRight   | number | 平行于基线，从CanvasRenderingContext2D.textAlign 属性确定的对齐点到文本矩形边界右侧的距离，当前值为0。 |
| alphabeticBaseline       | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到线框的 alphabetic 基线的距离，当前值为0。 |
| emHeightAscent           | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到线框中 em 方块顶部的距离，当前值为0。 |
| emHeightDescent          | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到线框中 em 方块底部的距离，当前值为0。 |
| fontBoundingBoxAscent    | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到渲染文本的所有字体的矩形最高边界顶部的距离，当前值为0。 |
| fontBoundingBoxDescent   | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到渲染文本的所有字体的矩形边界最底部的距离，当前值为0。 |
| hangingBaseline          | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到线框的 hanging 基线的距离，当前值为0。 |
| ideographicBaseline      | number | 从CanvasRenderingContext2D.textBaseline 属性标明的水平线到线框的 ideographic 基线的距离，当前值为0。 |


### stroke

stroke(path?: Path2D): void

进行边框绘制操作。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型                                       | 必填   | 默认值  | 描述           |
| ---- | ---------------------------------------- | ---- | ---- | ------------ |
| path | [Path2D](ts-components-canvas-path2d.md) | 否    | null | 需要绘制的Path2D。 |


### beginPath

beginPath(): void

创建一个新的绘制路径。

从API version 9开始，该接口支持在ArkTS卡片中使用。


### createPattern

createPattern(image: ImageBitmap, repetition: string | null): CanvasPattern | null

通过指定图像和重复方式创建图片填充的模板。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数         | 类型                                       | 必填   | 默认值  | 描述                                       |
| ---------- | ---------------------------------------- | ---- | ---- | ---------------------------------------- |
| image      | [ImageBitmap](ts-components-canvas-imagebitmap.md) | 是    | null | 图源对象，具体参考ImageBitmap对象。                  |
| repetition | string                                   | 是    | “”   | 设置图像重复的方式，取值为：'repeat'、'repeat-x'、&nbsp;'repeat-y'、'no-repeat'、'clamp'、'mirror'。 |

**返回值：**

| 类型                                       | 说明                      |
| ---------------------------------------- | ----------------------- |
| [CanvasPattern](ts-components-canvas-canvaspattern.md#canvaspattern) | 通过指定图像和重复方式创建图片填充的模板对象。 |


### rect

rect(x: number, y: number, w: number, h: number): void

创建矩形路径。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述            |
| ---- | ------ | ---- | ---- | ------------- |
| x    | number | 是    | 0    | 指定矩形的左上角x坐标值。 |
| y    | number | 是    | 0    | 指定矩形的左上角y坐标值。 |
| w    | number | 是    | 0    | 指定矩形的宽度。      |
| h    | number | 是    | 0    | 指定矩形的高度。      |


### fill

fill(fillRule?: CanvasFillRule): void

对封闭路径进行填充。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数:** 

| 参数       | 类型             | 必填   | 默认值       | 描述                                       |
| -------- | -------------- | ---- | --------- | ---------------------------------------- |
| fillRule | CanvasFillRule | 否    | "nonzero" | 指定要填充对象的规则。<br/>可选参数为："nonzero", "evenodd"。 |


fill(path: Path2D, fillRule?: CanvasFillRule): void

对封闭路径进行填充。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数:** 

| 参数       | 类型             | 必填   | 默认值       | 描述                                       |
| -------- | -------------- | ---- | --------- | ---------------------------------------- |
| path     | Path2D         | 是    |           | Path2D填充路径。                              |
| fillRule | CanvasFillRule | 否    | "nonzero" | 指定要填充对象的规则。<br/>可选参数为："nonzero", "evenodd"。 |

### clip

clip(fillRule?: CanvasFillRule): void

设置当前路径为剪切路径。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数:** 

| 参数       | 类型             | 必填   | 默认值       | 描述                                       |
| -------- | -------------- | ---- | --------- | ---------------------------------------- |
| fillRule | CanvasFillRule | 否    | "nonzero" | 指定要剪切对象的规则。<br/>可选参数为："nonzero", "evenodd"。 |

### filter

filter(filter: string): void

为Canvas图形设置各类滤镜效果。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数:**

| 参数     | 类型     | 必填   | 默认值  | 说明                                       |
| ------ | ------ | ---- | ---- | ---------------------------------------- |
| filter | string | 是    | -    | 接受各类滤镜效果的函数。支持的滤镜效果如下：<br/>- 'none': 无滤镜效果<br/>- 'blur'：给图像设置高斯模糊<br/>- 'brightness'：给图片应用一种线性乘法，使其看起来更亮或更暗<br/>- 'contrast'：调整图像的对比度<br/>- 'grayscale'：将图像转换为灰度图像<br/>- 'hue-rotate'：给图像应用色相旋转<br/>- 'invert'：反转输入图像<br/>- 'opacity'：转化图像的透明程度<br/>- 'saturate'：转换图像饱和度<br/>- 'sepia'：将图像转换为深褐色<br/>默认值：'none' |


### getTransform

getTransform(): Matrix2D

获取当前被应用到上下文的转换矩阵。该接口为空接口。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**返回值：**

| 类型                                       | 说明    |
| ---------------------------------------- | ----- |
| [Matrix2D](ts-components-canvas-matrix2d.md#Matrix2D) | 矩阵对象。 |

### resetTransform

resetTransform(): void

使用单位矩阵重新设置当前变形。该接口为空接口。

从API version 9开始，该接口支持在ArkTS卡片中使用。


### direction

direction(direction: CanvasDirection): void

绘制文本时，描述当前文本方向的属性。

从API version 9开始，该接口支持在ArkTS卡片中使用。

### rotate

rotate(angle: number): void

针对当前坐标轴进行顺时针旋转。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数    | 类型     | 必填   | 默认值  | 描述                                       |
| ----- | ------ | ---- | ---- | ---------------------------------------- |
| angle | number | 是    | 0    | 设置顺时针旋转的弧度值，可以通过Math.PI&nbsp;/&nbsp;180将角度转换为弧度值。 |


### scale

scale(x: number, y: number): void

设置canvas画布的缩放变换属性，后续的绘制操作将按照缩放比例进行缩放。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述          |
| ---- | ------ | ---- | ---- | ----------- |
| x    | number | 是    | 0    | 设置水平方向的缩放值。 |
| y    | number | 是    | 0    | 设置垂直方向的缩放值。 |


### transform

transform(a: number, b: number, c: number, d: number, e: number, f: number): void

transform方法对应一个变换矩阵，想对一个图形进行变化的时候，只要设置此变换矩阵相应的参数，对图形的各个定点的坐标分别乘以这个矩阵，就能得到新的定点的坐标。矩阵变换效果可叠加。

从API version 9开始，该接口支持在ArkTS卡片中使用。

> **说明：**
> 变换后的坐标计算方式（x和y为变换前坐标，x'和y'为变换后坐标)：
>
> - x' = scaleX \* x + skewY \* y + translateX
>
> - y' = skewX \* x + scaleY \* y + translateY

**参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述                   |
| ---- | ------ | ---- | ---- | -------------------- |
| a    | number | 是    | 0    | scaleX: 指定水平缩放值。     |
| b    | number | 是    | 0    | skewX: 指定水平倾斜值。      |
| c    | number | 是    | 0    | skewY: 指定垂直倾斜值。      |
| d    | number | 是    | 0    | scaleY: 指定垂直缩放值。     |
| e    | number | 是    | 0    | translateX: 指定水平移动值。 |
| f    | number | 是    | 0    | translateY: 指定垂直移动值。 |


### setTransform

setTransform(a: number, b: number, c: number, d: number, e: number, f: number): void

setTransform方法使用的参数和transform()方法相同，但setTransform()方法会重置现有的变换矩阵并创建新的变换矩阵。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述                   |
| ---- | ------ | ---- | ---- | -------------------- |
| a    | number | 是    | 0    | scaleX: 指定水平缩放值。     |
| b    | number | 是    | 0    | skewX: 指定水平倾斜值。      |
| c    | number | 是    | 0    | skewY: 指定垂直倾斜值。      |
| d    | number | 是    | 0    | scaleY: 指定垂直缩放值。     |
| e    | number | 是    | 0    | translateX: 指定水平移动值。 |
| f    | number | 是    | 0    | translateY: 指定垂直移动值。 |


### translate

translate(x: number, y: number): void

移动当前坐标系的原点。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述       |
| ---- | ------ | ---- | ---- | -------- |
| x    | number | 是    | 0    | 设置水平平移量。 |
| y    | number | 是    | 0    | 设置竖直平移量。 |


### drawImage

drawImage(image: ImageBitmap | PixelMap, dx: number, dy: number): void

drawImage(image: ImageBitmap | PixelMap, dx: number, dy: number, dw: number, dh: number): void

drawImage(image: ImageBitmap | PixelMap, sx: number, sy: number, sw: number, sh: number, dx: number, dy: number, dw: number, dh: number):void

进行图像绘制。

从API version 9开始，该接口支持在ArkTS卡片中使用，卡片中不支持PixelMap对象。

 **参数：**

| 参数  | 类型                                                         | 必填 | 默认值 | 描述                                    |
| ----- | ------------------------------------------------------------ | ---- | ------ | --------------------------------------- |
| image | [ImageBitmap](ts-components-canvas-imagebitmap.md) 或[PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | null   | 图片资源，请参考ImageBitmap或PixelMap。 |
| sx    | number                                                       | 否   | 0      | 裁切源图像时距离源图像左上角的x坐标值。 |
| sy    | number                                                       | 否   | 0      | 裁切源图像时距离源图像左上角的y坐标值。 |
| sw    | number                                                       | 否   | 0      | 裁切源图像时需要裁切的宽度。            |
| sh    | number                                                       | 否   | 0      | 裁切源图像时需要裁切的高度。            |
| dx    | number                                                       | 是   | 0      | 绘制区域左上角在x轴的位置。             |
| dy    | number                                                       | 是   | 0      | 绘制区域左上角在y&nbsp;轴的位置。       |
| dw    | number                                                       | 否   | 0      | 绘制区域的宽度。                        |
| dh    | number                                                       | 否   | 0      | 绘制区域的高度。                        |


### createImageData

createImageData(sw: number, sh: number): ImageData

根据宽高创建ImageData对象，请参考[ImageData](ts-components-canvas-imagedata.md)。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型     | 必填   | 默认   | 描述            |
| ---- | ------ | ---- | ---- | ------------- |
| sw   | number | 是    | 0    | ImageData的宽度。 |
| sh   | number | 是    | 0    | ImageData的高度。 |


createImageData(imageData: ImageData): ImageData

根据已创建的ImageData对象创建新的ImageData对象，请参考[ImageData](ts-components-canvas-imagedata.md)。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数        | 类型                                       | 必填   | 默认   | 描述               |
| --------- | ---------------------------------------- | ---- | ---- | ---------------- |
| imagedata | [ImageData](ts-components-canvas-imagedata.md) | 是    | null | 被复制的ImageData对象。 |

 **返回值：**

| 类型                                       | 说明            |
| ---------------------------------------- | ------------- |
| [ImageData](ts-components-canvas-imagedata.md) | 新的ImageData对象 |

### getPixelMap

getPixelMap(sx: number, sy: number, sw: number, sh: number): PixelMap

以当前canvas指定区域内的像素创建[PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7)对象。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述              |
| ---- | ------ | ---- | ---- | --------------- |
| sx   | number | 是    | 0    | 需要输出的区域的左上角x坐标。 |
| sy   | number | 是    | 0    | 需要输出的区域的左上角y坐标。 |
| sw   | number | 是    | 0    | 需要输出的区域的宽度。     |
| sh   | number | 是    | 0    | 需要输出的区域的高度。     |

**返回值：**

| 类型                                                         | 说明             |
| ------------------------------------------------------------ | ---------------- |
| [PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7) | 新的PixelMap对象 |

### setPixelMap

setPixelMap(value?: PixelMap): void

将当前传入[PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7)对象绘制在画布上。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述              |
| ---- | ------ | ---- | ---- | --------------- |
| sx   | number | 是    | 0    | 需要输出的区域的左上角x坐标。 |
| sy   | number | 是    | 0    | 需要输出的区域的左上角y坐标。 |
| sw   | number | 是    | 0    | 需要输出的区域的宽度。     |
| sh   | number | 是    | 0    | 需要输出的区域的高度。     |

**返回值：**

| 类型                                                         | 说明             |
| ------------------------------------------------------------ | ---------------- |
| [PixelMap](https://gitee.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis/js-apis-image.md#pixelmap7) | 新的PixelMap对象 |


### getImageData

getImageData(sx: number, sy: number, sw: number, sh: number): ImageData

以当前canvas指定区域内的像素创建[ImageData](ts-components-canvas-imagedata.md)对象。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述              |
| ---- | ------ | ---- | ---- | --------------- |
| sx   | number | 是    | 0    | 需要输出的区域的左上角x坐标。 |
| sy   | number | 是    | 0    | 需要输出的区域的左上角y坐标。 |
| sw   | number | 是    | 0    | 需要输出的区域的宽度。     |
| sh   | number | 是    | 0    | 需要输出的区域的高度。     |

   **返回值：**

| 类型                                       | 说明            |
| ---------------------------------------- | ------------- |
| [ImageData](ts-components-canvas-imagedata.md) | 新的ImageData对象 |


### putImageData

putImageData(imageData: Object, dx: number | string, dy: number | string): void

putImageData(imageData: Object, dx: number | string, dy: number | string, dirtyX: number | string, dirtyY: number | string, dirtyWidth?: number | string, dirtyHeight: number | string): void

使用[ImageData](ts-components-canvas-imagedata.md)数据填充新的矩形区域。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数          | 类型                                       | 必填   | 默认值          | 描述                            |
| ----------- | ---------------------------------------- | ---- | ------------ | ----------------------------- |
| imagedata   | Object                                   | 是    | null         | 包含像素值的ImageData对象。            |
| dx          | number&nbsp;\|&nbsp;string<sup>10+</sup> | 是    | 0            | 填充区域在x轴方向的偏移量。                |
| dy          | number&nbsp;\|&nbsp;string<sup>10+</sup> | 是    | 0            | 填充区域在y轴方向的偏移量。                |
| dirtyX      | number&nbsp;\|&nbsp;string<sup>10+</sup> | 否    | 0            | 源图像数据矩形裁切范围左上角距离源图像左上角的x轴偏移量。 |
| dirtyY      | number&nbsp;\|&nbsp;string<sup>10+</sup> | 否    | 0            | 源图像数据矩形裁切范围左上角距离源图像左上角的y轴偏移量。 |
| dirtyWidth  | number&nbsp;\|&nbsp;string<sup>10+</sup> | 否    | imagedata的宽度 | 源图像数据矩形裁切范围的宽度。               |
| dirtyHeight | number&nbsp;\|&nbsp;string<sup>10+</sup> | 否    | imagedata的高度 | 源图像数据矩形裁切范围的高度。               |

### setLineDash

setLineDash(segments: number[]): void

设置画布的虚线样式。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：** 

| 参数       | 类型       | 描述                  |
| -------- | -------- | ------------------- |
| segments | number[] | 描述线段如何交替和线段间距长度的数组。 |


### getLineDash

getLineDash(): number[]

获得当前画布的虚线样式。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**返回值：** 

| 类型     | 说明                                             |
| -------- | ------------------------------------------------ |
| number[] | 返回数组，该数组用来描述线段如何交替和间距长度。 |

### toDataURL

toDataURL(type?: string, quality?: number): string

生成一个包含图片展示的URL。

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：** 

| 参数名     | 参数类型   | 必填   | 描述                                       |
| ------- | ------ | ---- | ---------------------------------------- |
| type    | string | 否    | 可选参数，用于指定图像格式，默认格式为image/png。            |
| quality | number | 否    | 在指定图片格式为image/jpeg或image/webp的情况下，可以从0到1的区间内选择图片的质量。如果超出取值范围，将会使用默认值0.92。 |

**返回值：** 

| 类型     | 说明        |
| ------ | --------- |
| string | 图像的URL地址。 |


### imageSmoothingQuality

imageSmoothingQuality(quality: imageSmoothingQuality)

用于设置图像平滑度。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：** 

| 参数      | 类型                    | 描述                                       |
| ------- | --------------------- | ---------------------------------------- |
| quality | imageSmoothingQuality | 支持如下三种类型：<br/>- 'low'：低画质<br/>- 'medium'：中画质<br/>- 'high'：高画质 |

### transferToImageBitmap

transferToImageBitmap(): ImageBitmap

在离屏画布最近渲染的图像上创建一个ImageBitmap对象。

**返回值：** 

| 类型                                               | 说明                           |
| -------------------------------------------------- | ------------------------------ |
| [ImageBitmap](ts-components-canvas-imagebitmap.md) | 存储离屏画布上渲染的像素数据。 |

### restore

restore(): void

对保存的绘图上下文进行恢复。

从API version 9开始，该接口支持在ArkTS卡片中使用。


### save

save(): void

对当前的绘图上下文进行保存。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 createLinearGradient

createLinearGradient(x0: number, y0: number, x1: number, y1: number): void

创建一个线性渐变色。

从API version 9开始，该接口支持在ArkTS卡片中使用。

 **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述       |
| ---- | ------ | ---- | ---- | -------- |
| x0   | number | 是    | 0    | 起点的x轴坐标。 |
| y0   | number | 是    | 0    | 起点的y轴坐标。 |
| x1   | number | 是    | 0    | 终点的x轴坐标。 |
| y1   | number | 是    | 0    | 终点的y轴坐标。 |


### createRadialGradient

createRadialGradient(x0: number, y0: number, r0: number, x1: number, y1: number, r1: number): void

创建一个径向渐变色。

从API version 9开始，该接口支持在ArkTS卡片中使用。

  **参数：**

| 参数   | 类型     | 必填   | 默认值  | 描述                |
| ---- | ------ | ---- | ---- | ----------------- |
| x0   | number | 是    | 0    | 起始圆的x轴坐标。         |
| y0   | number | 是    | 0    | 起始圆的y轴坐标。         |
| r0   | number | 是    | 0    | 起始圆的半径。必须是非负且有限的。 |
| x1   | number | 是    | 0    | 终点圆的x轴坐标。         |
| y1   | number | 是    | 0    | 终点圆的y轴坐标。         |
| r1   | number | 是    | 0    | 终点圆的半径。必须为非负且有限的。 |

### createConicGradient<sup>10+</sup>

createConicGradient(startAngle: number, x: number, y: number): CanvasGradient

创建一个圆锥渐变色。

**参数：**

| 参数         | 类型     | 必填   | 默认值  | 描述                                  |
| ---------- | ------ | ---- | ---- | ----------------------------------- |
| startAngle | number | 是    | 0    | 开始渐变的角度，以弧度为单位。角度测量从中心右侧水平开始，顺时针移动。 |
| x          | number | 是    | 0    | 圆锥渐变的中心x轴坐标。单位：vp                   |
| y          | number | 是    | 0    | 圆锥渐变的中心y轴坐标。单位：vp                   |

<!--no_check-->