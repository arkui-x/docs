# @ohos.graphics.drawing (绘制模块)

应用在开发中，经常需要针对不同的元素内容进行绘制，开发者通常可以选择直接使用ArkUI组件来绘制想要的元素或效果，但有些自定义图形或效果无法满足，此时可以选择使用Drawing来实现灵活的自定义绘制效果。Drawing模块提供基本的绘制能力，如绘制矩形、圆形、点、直线、自定义Path和字体等。

> **说明：**
>
> - 本模块首批接口从API version 20开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> - 本模块使用屏幕物理像素单位px。
>
> - 本模块为单线程模型策略，需要调用方自行管理线程安全和上下文状态的切换。

## 导入模块

```ts
import { drawing } from '@kit.ArkGraphics2D';
```

## BlendMode

混合模式枚举。混合模式会将两种颜色（源色、目标色）以特定的方式混合生成一种新的颜色，通常用于叠加、滤镜和遮罩等图形操作场景。混合操作会分别作用于红、绿、蓝三个颜色通道，采用相同的混合逻辑，而透明度（Alpha通道）则根据各模式的定义另行处理。

为简洁起见，我们使用以下缩写：

s : source 源的缩写。 d : destination 目标的缩写。 sa : source alpha 源透明度的缩写。 da : destination alpha 目标透明度的缩写。

计算结果用如下缩写表示：

r : 如果4个通道（透明度、红、绿、蓝）的计算方式相同，用r表示。 ra : 如果只操作透明度通道，用ra表示。 rc : 如果操作3个颜色通道，用rc表示。

以黄色矩形为源图像，蓝色圆形为目标图像，各混合模式枚举生成的效果示意图请参考下表。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称        | 值   | 说明                                                         | 示意图   |
| ----------- | ---- | ------------------------------------------------------------ | -------- |
| CLEAR       | 0    | 清除模式，r = 0，设置为全透明。                                | ![CLEAR](./figures/zh-ch_image_BlendMode_Clear.png) |
| SRC         | 1    | r = s（result的4个通道，都等于source的4个通道，即结果等于源。），使用源像素替换目标像素。 | ![SRC](./figures/zh-ch_image_BlendMode_Src.png) |
| DST         | 2    | r = d（result的4个通道，都等于destination的4个通道，即结果等于目标。），保持目标像素不变。 | ![DST](./figures/zh-ch_image_BlendMode_Dst.png) |
| SRC_OVER    | 3    | r = s + (1 - sa) * d，在目标像素上方绘制源像素，考虑源像素的透明度。 | ![SRC_OVER](./figures/zh-ch_image_BlendMode_SrcOver.png) |
| DST_OVER    | 4    | r = d + (1 - da) * s，在源像素上方绘制目标像素，考虑目标像素的透明度。 | ![DST_OVER](./figures/zh-ch_image_BlendMode_DstOver.png) |
| SRC_IN      | 5    | r = s * da，仅保留源像素与目标不透明部分的交集。 | ![SRC_IN](./figures/zh-ch_image_BlendMode_SrcIn.png) |
| DST_IN      | 6    | r = d * sa，仅保留目标像素与源不透明部分的交集。 | ![DST_IN](./figures/zh-ch_image_BlendMode_DstIn.png) |
| SRC_OUT     | 7    | r = s * (1 - da)，保留源像素中不与目标重叠的部分。 | ![SRC_OUT](./figures/zh-ch_image_BlendMode_SrcOut.png) |
| DST_OUT     | 8    | r = d * (1 - sa)，保留目标像素中不与源重叠的部分。 | ![DST_OUT](./figures/zh-ch_image_BlendMode_DstOut.png) |
| SRC_ATOP    | 9    | r = s * da + d * (1 - sa)，源像素覆盖在目标像素上，仅在目标不透明部分显示源像素。 | ![SRC_ATOP](./figures/zh-ch_image_BlendMode_SrcATop.png) |
| DST_ATOP    | 10   | r = d * sa + s * (1 - da)，目标像素覆盖在源像素上，仅在源不透明部分显示目标像素。 | ![DST_ATOP](./figures/zh-ch_image_BlendMode_DstATop.png) |
| XOR         | 11   | r = s * (1 - da) + d * (1 - sa)，仅显示源像素和目标像素中不重叠的部分。 | ![XOR](./figures/zh-ch_image_BlendMode_Xor.png) |
| PLUS        | 12   | r = min(s + d, 1)，源和目标像素的颜色值相加。                   | ![PLUS](./figures/zh-ch_image_BlendMode_Plus.png) |
| MODULATE    | 13   | r = s * d，源和目标像素的颜色值相乘。                           | ![MODULATE](./figures/zh-ch_image_BlendMode_Modulate.png) |
| SCREEN      | 14   | 滤色模式，r = s + d - s * d，反转源和目标像素的颜色值，相乘后再反转，结果通常更亮。 | ![SCREEN](./figures/zh-ch_image_BlendMode_Screen.png) |
| OVERLAY     | 15   | 叠加模式，根据目标像素的亮度，选择性地应用MULTIPLY或SCREEN模式，增强对比度。 | ![OVERLAY](./figures/zh-ch_image_BlendMode_Overlay.png) |
| DARKEN      | 16   | 变暗模式，rc = s + d - max(s * da, d * sa), ra = s + (1 - sa) * d，取源和目标像素中较暗的颜色值。 | ![DARKEN](./figures/zh-ch_image_BlendMode_Darken.png) |
| LIGHTEN     | 17   | 变亮模式，rc = s + d - min(s * da, d * sa), ra = s + (1 - sa) * d，取源和目标像素中较亮的颜色值。 | ![LIGHTEN](./figures/zh-ch_image_BlendMode_Lighten.png) |
| COLOR_DODGE | 18   | 颜色减淡模式，通过减小对比度使目标像素变亮以反映源像素。           | ![COLOR_DODGE](./figures/zh-ch_image_BlendMode_ColorDodge.png) |
| COLOR_BURN  | 19   | 颜色加深模式，通过增加对比度使目标像素变暗以反映源像素。           | ![COLOR_BURN](./figures/zh-ch_image_BlendMode_ColorBurn.png) |
| HARD_LIGHT  | 20   | 强光模式，根据源像素的亮度，选择性地应用MULTIPLY或SCREEN模式。    | ![HARD_LIGHT](./figures/zh-ch_image_BlendMode_HardLight.png) |
| SOFT_LIGHT  | 21   | 柔光模式，根据源像素的亮度，柔和地变亮或变暗目标像素。             | ![SOFT_LIGHT](./figures/zh-ch_image_BlendMode_SoftLight.png) |
| DIFFERENCE  | 22   | 差值模式，rc = s + d - 2 * (min(s * da, d * sa)), ra = s + (1 - sa) * d，计算源和目标像素颜色值的差异。 | ![DIFFERENCE](./figures/zh-ch_image_BlendMode_Difference.png) |
| EXCLUSION   | 23   | 排除模式，rc = s + d - two(s * d), ra = s + (1 - sa) * d，类似于DIFFERENCE，但对比度较低。 | ![EXCLUSION](./figures/zh-ch_image_BlendMode_Exclusion.png) |
| MULTIPLY    | 24   | 正片叠底，r = s * (1 - da) + d * (1 - sa) + s * d，源和目标像素的颜色值相乘，结果通常更暗。 | ![MULTIPLY](./figures/zh-ch_image_BlendMode_Multiply.png) |
| HUE         | 25   | 色相模式，使用源像素的色相，目标像素的饱和度和亮度。               | ![HUE](./figures/zh-ch_image_BlendMode_Hue.png) |
| SATURATION  | 26   | 饱和度模式，使用源像素的饱和度，目标像素的色相和亮度。             | ![SATURATION](./figures/zh-ch_image_BlendMode_Saturation.png) |
| COLOR       | 27   | 颜色模式，使用源像素的色相和饱和度，目标像素的亮度。               | ![COLOR](./figures/zh-ch_image_BlendMode_Color.png) |
| LUMINOSITY  | 28   | 亮度模式，使用源像素的亮度，目标像素的色相和饱和度。               | ![LUMINOSITY](./figures/zh-ch_image_BlendMode_Luminosity.png) |

## PathMeasureMatrixFlags<sup>20+</sup>

路径测量中的矩阵信息维度枚举，常用于控制物体沿路径移动的动画场景。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称        | 值   | 说明                                                         |
| ----------- | ---- | ------------------------------------------------------------ |
| GET_POSITION_MATRIX        | 0    | 获取位置信息对应的矩阵。                                            |
| GET_TANGENT_MATRIX          | 1    | 获取切线信息对应的矩阵。 |
| GET_POSITION_AND_TANGENT_MATRIX    | 2     | 获取位置和切线信息对应的矩阵。 |

## SrcRectConstraint<sup>20+</sup>

源矩形区域约束类型枚举，用于在画布绘制图像时指定是否将采样范围限制在源矩形区域内。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称        | 值   | 说明                                                         |
| ----------- | ---- | ------------------------------------------------------------ |
| STRICT         | 0    | 严格限制采样范围在源矩形区域内，速度较慢。                                            |
| FAST           | 1    | 允许采样范围超出源矩形范围，速度较快。 |

## ShadowFlag<sup>20+</sup>

控制阴影绘制行为的枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                         | 值    | 说明                 |
| -------------------------- | ---- | ------------------ |
| NONE      | 0    | 不使用任何阴影处理选项。        |
| TRANSPARENT_OCCLUDER | 1    | 遮挡物是半透明的。         |
| GEOMETRIC_ONLY    | 2    | 仅使用几何阴影效果。        |
| ALL           | 3    | 使用所有可用的阴影处理选项，以生成组合阴影效果，包括半透明遮挡和几何阴影效果。 |

## PathOp<sup>20+</sup>

路径操作类型枚举，可用于合并或裁剪路径等功能。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                   | 值   | 说明                           |
| ---------------------- | ---- | ------------------------------ |
| DIFFERENCE     | 0    | 差集操作。 |
| INTERSECT    | 1    | 交集操作。 |
| UNION    | 2    | 并集操作。 |
| XOR     | 3    | 异或操作。 |
| REVERSE_DIFFERENCE     | 4    | 反向差集操作。 |

## PathIteratorVerb<sup>20+</sup>

迭代器包含的路径操作类型枚举，可用于读取path的操作指令。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称  | 值   | 说明                           |
| ----- | ---- | ------------------------------ |
| MOVE  | 0    | 设置起始点。 |
| LINE  | 1    | 添加线段。 |
| QUAD  | 2    | 添加二阶贝塞尔圆滑曲线。 |
| CONIC | 3    | 添加圆锥曲线。 |
| CUBIC | 4    | 添加三阶贝塞尔圆滑曲线。 |
| CLOSE | 5    | 路径闭合。 |
| DONE  | CLOSE + 1    | 路径设置完成。 |

## PathIterator<sup>20+</sup>

表示路径操作迭代器，可通过遍历迭代器读取path的操作指令。

### constructor<sup>20+</sup>

constructor(path: Path)

构造迭代器并绑定路径。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| path | [Path](#path) | 是   | 迭代器绑定的路径对象。                 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
let iter: drawing.PathIterator = new drawing.PathIterator(path);
```

### next<sup>20+</sup>

next(points: Array<common2D.Point>, offset?: number): PathIteratorVerb

返回当前路径的下一个操作，并将迭代器置于该操作。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| points | Array\<[common2D.Point](js-apis-graphics-common2D.md#point20)>   | 是   | 坐标点数组，长度必须至少为偏移量加4，以确保能容纳所有类型的路径数据。操作执行后，该数组会被覆盖。填入的坐标点数量取决于操作类型，其中，MOVE填入1个坐标点，LINE填入2个坐标点，QUAD填入3个坐标点，CONIC填入3个坐标点 + 1个权重值（共3.5组），CUBIC填入4个坐标点，CLOSE和DONE不填入任何点。 |
| offset | number   | 否   | 数组中写入位置相对起始点的偏移量，默认为0，取值范围为[0, size-4]，size是指坐标点数组长度。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [PathIteratorVerb](#pathiteratorverb20) | 迭代器包含的路径操作类型。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(10, 20);
let iter: drawing.PathIterator = new drawing.PathIterator(path);
let verbStr: Array<string> = ["MOVE", "LINE", "QUAD", "CONIC", "CUBIC", "CLOSE", "DONE"];
let pointCount: Array<number> = [1,2,3,4,4,0,0]; //1,2,3,3.5,4,0,0
let points: Array<common2D.Point> = [{x: 0, y: 0}, {x: 0, y: 0}, {x: 0, y: 0}, {x: 0, y: 0}];
let offset = 0;
let verb = iter.next(points, offset);
let outputMessage: string = "pathIteratorNext: ";
outputMessage += "verb =" + verbStr[verb] + "; has " + pointCount[verb] + " pairs: ";
for (let j = 0; j < pointCount[verb] + offset; j++) {
  outputMessage += "[" + points[j].x + ", " + points[j].y + "]";
}
console.info(outputMessage);
```

### peek<sup>20+</sup>

peek(): PathIteratorVerb

返回当前路径的下一个操作，迭代器保持在原操作。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [PathIteratorVerb](#pathiteratorverb20) | 迭代器包含的路径操作类型。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
let iter: drawing.PathIterator = new drawing.PathIterator(path);
let res = iter.peek();
```

### hasNext<sup>20+</sup>

hasNext(): boolean

判断路径操作迭代器中是否还有下一个操作。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 判断路径操作迭代器中是否还有下一个操作。true表示有，false表示没有。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
let iter: drawing.PathIterator = new drawing.PathIterator(path);
let res = iter.hasNext();
```

## Path

由直线、圆弧、二阶贝塞尔、三阶贝塞尔组成的复合几何路径。

### constructor<sup>20+</sup>

constructor()

构造一个路径。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
```

### constructor<sup>20+</sup>

constructor(path: Path)

构造一个已有路径的副本。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| path | [Path](#path) | 是   | 待复制的路径对象。                 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(0, 0);
path.lineTo(0, 700);
path.lineTo(700, 0);
path.close();
let path1: drawing.Path =  new drawing.Path(path);
```

### set<sup>20+</sup>

set(src: Path): void

使用另一个路径对当前路径进行更新。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| src | [Path](#path) | 是   | 用于更新的路径。                 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
let path: drawing.Path = new drawing.Path();
path.moveTo(0, 0);
path.lineTo(0, 700);
path.lineTo(700, 0);
path.close();
let path1: drawing.Path = new drawing.Path();
path1.set(path);
```

### moveTo

moveTo(x: number, y: number) : void

设置自定义路径的起始点位置。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| x      | number | 是   | 起始点的x轴坐标，该参数为浮点数。 |
| y      | number | 是   | 起始点的y轴坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
```

### lineTo

lineTo(x: number, y: number) : void

添加一条从路径的最后点位置（若路径没有内容则默认为 (0, 0)）到目标点位置的线段。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| x      | number | 是   | 目标点的x轴坐标，该参数为浮点数。 |
| y      | number | 是   | 目标点的y轴坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
path.lineTo(10, 15);
```

### arcTo

arcTo(x1: number, y1: number, x2: number, y2: number, startDeg: number, sweepDeg: number): void

给路径添加一段弧线，绘制弧线的方式为角度弧，该方式首先会指定一个矩形边框，取其内切椭圆，然后会指定一个起始角度和扫描度数，从起始角度扫描截取的椭圆周长一部分即为绘制的弧线。另外会默认添加一条从路径的最后点位置到弧线起始点位置的线段。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型   | 必填 | 说明                       |
| -------- | ------ | ---- | -------------------------- |
| x1       | number | 是   | 矩形左上角的x坐标，该参数为浮点数。 |
| y1       | number | 是   | 矩形左上角的y坐标，该参数为浮点数。 |
| x2       | number | 是   | 矩形右下角的x坐标，该参数为浮点数。 |
| y2       | number | 是   | 矩形右下角的y坐标，该参数为浮点数。 |
| startDeg | number | 是   | 起始的角度。角度的起始方向（0°）为x轴正方向。 |
| sweepDeg | number | 是   | 扫描的度数，为正数时顺时针扫描，为负数时逆时针扫描。实际扫描的度数为该入参对360取模的结果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
path.arcTo(10, 15, 10, 10, 10, 10);
```

### quadTo

quadTo(ctrlX: number, ctrlY: number, endX: number, endY: number): void

添加从路径最后点位置（若路径没有内容则为 (0, 0)）到目标点位置的二阶贝塞尔曲线。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                  |
| ------ | ------ | ---- | --------------------- |
| ctrlX  | number | 是   | 控制点的x坐标，该参数为浮点数。 |
| ctrlY  | number | 是   | 控制点的y坐标，该参数为浮点数。 |
| endX   | number | 是   | 目标点的x坐标，该参数为浮点数。 |
| endY   | number | 是   | 目标点的y坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
path.quadTo(10, 15, 10, 10);
```

### conicTo<sup>20+</sup>

conicTo(ctrlX: number, ctrlY: number, endX: number, endY: number, weight: number): void

在当前路径上添加一条路径最后点位置（若路径没有内容则默认为 (0, 0)）到目标点位置的圆锥曲线，其控制点为 (ctrlX, ctrlY)，结束点为 (endX, endY)。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                  |
| ------ | ------ | ---- | --------------------- |
| ctrlX  | number | 是   | 控制点的x坐标，该参数为浮点数。 |
| ctrlY  | number | 是   | 控制点的y坐标，该参数为浮点数。 |
| endX   | number | 是   | 目标点的x坐标，该参数为浮点数。 |
| endY   | number | 是   | 目标点的y坐标，该参数为浮点数。 |
| weight | number | 是   | 表示曲线权重，决定了曲线的形状。值越大，曲线越接近控制点。小于等于0时，效果与[lineTo](#lineto)相同；值为1时，效果与[quadTo](#quadto)相同。该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.conicTo(200, 400, 100, 200, 0);
```

### cubicTo

cubicTo(ctrlX1: number, ctrlY1: number, ctrlX2: number, ctrlY2: number, endX: number, endY: number): void

添加一条从路径最后点位置（若路径没有内容则默认为 (0, 0)）到目标点位置的三阶贝塞尔圆滑曲线。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                        |
| ------ | ------ | ---- | --------------------------- |
| ctrlX1 | number | 是   | 第一个控制点的x坐标，该参数为浮点数。 |
| ctrlY1 | number | 是   | 第一个控制点的y坐标，该参数为浮点数。 |
| ctrlX2 | number | 是   | 第二个控制点的x坐标，该参数为浮点数。 |
| ctrlY2 | number | 是   | 第二个控制点的y坐标，该参数为浮点数。 |
| endX   | number | 是   | 目标点的x坐标，该参数为浮点数。 |
| endY   | number | 是   | 目标点的y坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
path.cubicTo(100, 100, 80, 150, 300, 150);
```

### rMoveTo<sup>20+</sup>

rMoveTo(dx: number, dy: number): void

设置一个相对于当前路径终点（若路径没有内容则默认为 (0, 0)）的路径起始点位置。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| dx     | number | 是   | 路径新起始点相对于当前路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| dy     | number | 是   | 路径新起始点相对于当前路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.rMoveTo(10, 10);
```

### rLineTo<sup>20+</sup>

rLineTo(dx: number, dy: number): void

使用相对位置在当前路径上添加一条当前路径终点（若路径没有内容则默认为 (0, 0)）到目标点位置的线段。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| dx     | number | 是   | 目标点相对于当前路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| dy     | number | 是   | 目标点相对于当前路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.rLineTo(400, 200);
```

### rQuadTo<sup>20+</sup>

rQuadTo(dx1: number, dy1: number, dx2: number, dy2: number): void

使用相对位置在当前路径上添加一条当前路径终点（若路径没有内容则默认为 (0, 0)）到目标点位置的二阶贝塞尔曲线。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                  |
| ------ | ------ | ---- | --------------------- |
| dx1  | number | 是   | 控制点相对于路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| dy1  | number | 是   | 控制点相对于路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |
| dx2   | number | 是   | 目标点相对于路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| dy2   | number | 是   | 目标点相对于路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.rQuadTo(100, 0, 0, 200);
```

### rConicTo<sup>20+</sup>

rConicTo(ctrlX: number, ctrlY: number, endX: number, endY: number, weight: number): void

使用相对位置在当前路径上添加一条路径终点（若路径没有内容则默认为 (0, 0)）到目标点位置的圆锥曲线。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                  |
| ------ | ------ | ---- | --------------------- |
| ctrlX  | number | 是   | 控制点相对于路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| ctrlY  | number | 是   | 控制点相对于路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |
| endX   | number | 是   | 目标点相对于路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| endY   | number | 是   | 目标点相对于路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |
| weight | number | 是   | 表示曲线的权重，决定了曲线的形状，越大越接近控制点。若小于等于0则等同于使用[rLineTo](#rlineto20)添加一条到结束点的线段，若为1则等同于[rQuadTo](#rquadto20)，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.rConicTo(200, 400, 100, 200, 0);
```

### rCubicTo<sup>20+</sup>

rCubicTo(ctrlX1: number, ctrlY1: number, ctrlX2: number, ctrlY2: number, endX: number, endY: number): void

使用相对位置在当前路径上添加一条当前路径终点（若路径没有内容则默认为 (0, 0)）到目标点位置的三阶贝塞尔曲线。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                        |
| ------ | ------ | ---- | --------------------------- |
| ctrlX1 | number | 是   | 第一个控制点相对于路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| ctrlY1 | number | 是   | 第一个控制点相对于路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |
| ctrlX2 | number | 是   | 第二个控制点相对于路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| ctrlY2 | number | 是   | 第二个控制点相对于路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |
| endX   | number | 是   | 目标点相对于路径终点的x轴偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| endY   | number | 是   | 目标点相对于路径终点的y轴偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.rCubicTo(200, 0, 0, 200, -20, 0);
```

### addArc<sup>20+</sup>

addArc(rect: common2D.Rect, startAngle: number, sweepAngle: number): void

向路径添加一段圆弧。
当startAngle和sweepAngle同时满足以下两种情况时，添加整个椭圆而不是圆弧:
1.startAngle对90取余接近于0；
2.sweepAngle不在(-360, 360)区间内。
其余情况sweepAngle会对360取余后添加圆弧。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| rect        | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是    | 包含弧的椭圆的矩形边界。      |
| startAngle   | number | 是   | 弧的起始角度，单位为度，0度为x轴正方向，该参数为浮点数。 |
| sweepAngle   | number | 是   | 扫描角度，单位为度。正数表示顺时针方向，负数表示逆时针方向，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
const rect: common2D.Rect = {left:100, top:100, right:500, bottom:500};
path.addArc(rect, 90, 180);
```

### addCircle<sup>20+</sup>

addCircle(x: number, y: number, radius: number, pathDirection?: PathDirection): void

按指定方向，向路径添加圆形，圆的起点位于(x + radius, y)。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| x   | number | 是   | 表示圆心的x轴坐标，该参数为浮点数。 |
| y   | number | 是   | 表示圆心的y轴坐标，该参数为浮点数。 |
| radius   | number | 是   | 表示圆形的半径，该参数为浮点数，小于等于0时不会有任何效果。 |
| pathDirection   | [PathDirection](#pathdirection20)  | 否   | 表示路径方向，默认为顺时针方向。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts

import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.addCircle(100, 200, 50, drawing.PathDirection.CLOCKWISE);
```

### addOval<sup>20+</sup>

addOval(rect: common2D.Rect, start: number, pathDirection?: PathDirection): void

按指定方向，将矩形的内切椭圆添加到路径中。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| rect        | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是    | 椭圆的矩形边界。      |
| start   | number | 是   | 表示椭圆初始点的索引，0，1，2，3分别对应椭圆的上端点，右端点，下端点，左端点，该参数为不小于0的整数，大于等于4时会对4取余。 |
| pathDirection   | [PathDirection](#pathdirection20)  | 否   | 表示路径方向，默认为顺时针方向。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed.|

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
const rect: common2D.Rect = {left:100, top:100, right:500, bottom:500};
path.addOval(rect, 5, drawing.PathDirection.CLOCKWISE);
```

### addRect<sup>20+</sup>

addRect(rect: common2D.Rect, pathDirection?: PathDirection): void

按指定方向，将矩形添加到路径中，添加的路径的起始点为矩形左上角。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| rect        | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是    | 向路径中添加的矩形轮廓。      |
| pathDirection   | [PathDirection](#pathdirection20)  | 否   | 表示路径方向，默认为顺时针方向。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed.|

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
const rect: common2D.Rect = {left:100, top:100, right:500, bottom:500};
path.addRect(rect, drawing.PathDirection.CLOCKWISE);
```

### addRoundRect<sup>20+</sup>

addRoundRect(roundRect: RoundRect, pathDirection?: PathDirection): void

按指定方向，向路径添加圆角矩形轮廓。路径添加方向为顺时针时，起始点位于圆角矩形左下方圆角与左边界的交点；路径添加方向为逆时针时，起始点位于圆角矩形左上方圆角与左边界的交点。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| roundRect        | [RoundRect](#roundrect20) | 是    | 圆角矩形对象。      |
| pathDirection   | [PathDirection](#pathdirection20)  | 否   | 表示路径方向，默认为顺时针方向。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed.|

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
const rect: common2D.Rect = {left:100, top:100, right:500, bottom:500};
let roundRect = new drawing.RoundRect(rect, 50, 50);
path.addRoundRect(roundRect, drawing.PathDirection.CLOCKWISE);
```

### addPath<sup>20+</sup>

addPath(path: Path, matrix?: Matrix | null): void

对源路径进行矩阵变换后，将其添加到当前路径中。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| path        | [Path](#path) | 是    | 表示源路径对象。      |
| matrix   | [Matrix](#matrix20)\|null  | 否   | 表示矩阵对象，默认为单位矩阵。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
let matrix = new drawing.Matrix();
const rect: common2D.Rect = {left:100, top:100, right:500, bottom:500};
let roundRect = new drawing.RoundRect(rect, 50, 50);
path.addRoundRect(roundRect, drawing.PathDirection.CLOCKWISE);
let dstPath = new drawing.Path();
dstPath.addPath(path, matrix);
```

### transform<sup>20+</sup>

transform(matrix: Matrix): void

对路径进行矩阵变换。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| matrix   | [Matrix](#matrix20)  | 是   | 表示矩阵对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
let matrix = new drawing.Matrix();
matrix.setScale(1.5, 1.5, 10, 10);
const rect: common2D.Rect = {left:100, top:100, right:500, bottom:500};
let roundRect = new drawing.RoundRect(rect, 50, 50);
path.addRoundRect(roundRect, drawing.PathDirection.CLOCKWISE);
path.transform(matrix);
```

### contains<sup>20+</sup>

contains(x: number, y: number): boolean

判断指定坐标点是否被路径包含，判定是否被路径包含的规则参考[PathFillType](#pathfilltype20)。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| x      | number | 是   | x轴上坐标点，该参数必须为浮点数。 |
| y      | number | 是   | y轴上坐标点，该参数必须为浮点数。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回指定坐标点是否在路径内。true表示点在路径内，false表示点不在路径内。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
let rect : common2D.Rect = {left: 50, top: 50, right: 250, bottom: 250};
path.addRect(rect, drawing.PathDirection.CLOCKWISE);
console.info("test contains: " + path.contains(0, 0));
console.info("test contains: " + path.contains(60, 60));
```

### setLastPoint<sup>20+</sup>

setLastPoint(x: number, y: number): void

修改路径的最后一个点。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| x      | number | 是   | 指定点的x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| y      | number | 是   | 指定点的y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
const path = new drawing.Path();
path.moveTo(0, 0);
path.lineTo(0, 700);
let isEmpty = path.isEmpty();
console.info('isEmpty:', isEmpty);
path.reset();
isEmpty = path.isEmpty();
console.info('isEmpty:', isEmpty);
path.setLastPoint(50, 50);
console.info('isEmpty:', isEmpty);
```

### setFillType<sup>20+</sup>

setFillType(pathFillType: PathFillType): void

设置路径的填充类型，决定路径内部区域的定义方式。例如，使用Winding填充类型时，路径内部区域由路径环绕的次数决定，而使用EvenOdd填充类型时，路径内部区域由路径环绕的次数是否为奇数决定。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| pathFillType   | [PathFillType](#pathfilltype20)  | 是   | 表示路径填充规则。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed.|

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.setFillType(drawing.PathFillType.WINDING);
```

### getFillType<sup>20+</sup>

getFillType(): PathFillType

获取路径的填充类型。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                                               | 说明                   |
| -------------------------------------------------- | ---------------------- |
| [PathFillType](#pathfilltype20) | 路径填充类型。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
const path = new drawing.Path();
path.setFillType(drawing.PathFillType.WINDING);
let type = path.getFillType();
console.info("type :" + type);
```

### getBounds<sup>20+</sup>

getBounds(): common2D.Rect

获取包含路径的最小矩形边界。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                                               | 说明                   |
| -------------------------------------------------- | ---------------------- |
| [common2D.Rect](js-apis-graphics-common2D.md#rect) | 包含路径的最小矩形区域。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.lineTo(50, 40)
let rect : common2D.Rect = {left: 0, top: 0, right: 0, bottom: 0};
rect = path.getBounds();
console.info("test rect.left: " + rect.left);
console.info("test rect.top: " + rect.top);
console.info("test rect.right: " + rect.right);
console.info("test rect.bottom: " + rect.bottom);
```

### addPolygon<sup>20+</sup>

addPolygon(points: Array\<common2D.Point>, close: boolean): void

通过坐标点列表添加多条连续的线段。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| points | Array\<[common2D.Point](js-apis-graphics-common2D.md#point20)>   | 是   | 坐标点数组。 |
| close  | boolean                                                        | 是   | 表示是否将路径闭合，即是否添加路径起始点到终点的连线。true表示将路径闭合，false表示不将路径闭合。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let pointsArray = new Array<common2D.Point>();
const point1: common2D.Point = { x: 200, y: 200 };
const point2: common2D.Point = { x: 400, y: 200 };
const point3: common2D.Point = { x: 100, y: 400 };
const point4: common2D.Point = { x: 300, y: 400 };
pointsArray.push(point1);
pointsArray.push(point2);
pointsArray.push(point3);
pointsArray.push(point4);
const path = new drawing.Path();
path.addPolygon(pointsArray, false);
```

### offset<sup>20+</sup>

offset(dx: number, dy: number): Path

将路径沿着x轴和y轴方向偏移一定距离并保存在返回的路径对象中。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| dx     | number        | 是   | x轴方向偏移量，正数往x轴正方向偏移，负数往x轴负方向偏移，该参数为浮点数。 |
| dy     | number        | 是   | y轴方向偏移量，正数往y轴正方向偏移，负数往y轴负方向偏移，该参数为浮点数。 |

**返回值：**

| 类型   | 说明                |
| ------ | ------------------ |
| [Path](#path) | 返回当前路径偏移(dx,dy)后生成的新路径对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
path.moveTo(200, 200);
path.lineTo(300, 300);
const dst = path.offset(200, 200);
```

### op<sup>20+</sup>

op(path: Path, pathOp: PathOp): boolean

将当前路径置为和path按照指定的路径操作类型合并后的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| path    | [Path](#path) | 是   | 路径对象，用于与当前路径合并。 |
| pathOp  | [PathOp](#pathop20)   | 是    | 路径操作类型枚举。    |

**返回值：**

| 类型   | 说明                |
| ------ | ------------------ |
| boolean | 返回路径合并是否成功的结果。true表示合并成功，false表示合并失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const path = new drawing.Path();
const path2 = new drawing.Path();
path.addCircle(100, 200, 100, drawing.PathDirection.CLOCKWISE);
console.info("get pathOp: ", path2.op(path, drawing.PathOp.DIFFERENCE));
```

### close

close(): void

闭合路径，会添加一条从路径起点位置到最后点位置的线段。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
path.cubicTo(10, 10, 10, 10, 15, 15);
path.close();
```

### reset

reset(): void

重置自定义路径数据。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
path.cubicTo(10, 10, 10, 10, 15, 15);
path.reset();
```

### rewind<sup>20+</sup>

rewind(): void

将路径内添加的各类点/线清空，但是保留内存空间。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
let path = new drawing.Path();
path.moveTo(10,10);
path.lineTo(20,20);
path.rewind();
let empty = path.isEmpty();
console.info('empty : ', empty);
```

### isEmpty<sup>20+</sup>

isEmpty(): boolean

判断路径是否为空。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型  | 说明 |
| ------ | ---- |
| boolean | 路径是否为空。true表示当前路径为空，false表示路径不为空。|

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
let path = new drawing.Path();
path.moveTo(10,10);
path.lineTo(20,20);
let isEmpty = path.isEmpty();
console.info('isEmpty:', isEmpty);
```

### isRect<sup>20+</sup>

isRect(rect: common2D.Rect | null): boolean

判断路径是否构成矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect)\| null | 是   | 矩形对象，作为出参使用，路径构成矩形时，会被改写为路径表示的矩形，否则不会改变。可以为null，表示无需获取路径表示的矩形。 |

**返回值：**

| 类型  | 说明 |
| ------ | ---- |
| boolean | 返回路径是否构成矩形。true表示路径构成矩形，false表示路径不构成矩形。|

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.moveTo(10,10);
path.lineTo(20,10);
let isRect = path.isRect(null);
console.info("isRect: ", isRect);
let rect: common2D.Rect = { left : 100, top : 100, right : 400, bottom : 500 };
path.lineTo(20, 20);
path.lineTo(10, 20);
path.lineTo(10, 10);
isRect = path.isRect(rect);
console.info('isRect: ', isRect);
```

### getLength<sup>20+</sup>

getLength(forceClosed: boolean): number

获取路径长度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名| 类型  | 必填| 说明     |
| ----- | ------ | ---- | --------- |
| forceClosed  | boolean | 是  | 表示是否按照闭合路径测量，true表示测量时路径会被强制视为已闭合，false表示会根据路径的实际闭合状态测量。|

**返回值：**

| 类型  | 说明 |
| ------ | ---- |
| number | 路径长度。|

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path = new drawing.Path();
path.arcTo(20, 20, 180, 180, 180, 90);
let len = path.getLength(false);
console.info("path length = " + len);
```

### getPositionAndTangent<sup>20+</sup>

getPositionAndTangent(forceClosed: boolean, distance: number, position: common2D.Point, tangent: common2D.Point): boolean

获取路径起始点指定距离处的坐标点和切线值。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| forceClosed | boolean | 是   | 表示是否按照闭合路径测量，true表示测量时路径会被强制视为已闭合，false表示会根据路径的实际闭合状态测量。                 |
| distance | number | 是   | 表示与路径起始点的距离，小于0时会被视作0，大于路径长度时会被视作路径长度。该参数为浮点数。               |
| position | [common2D.Point](js-apis-graphics-common2D.md#point20) | 是   | 存储获取到的距离路径起始点distance处的点的坐标。                  |
| tangent | [common2D.Point](js-apis-graphics-common2D.md#point20) | 是   | 存储获取到的距离路径起始点distance处的点的切线值，tangent.x表示该点切线的余弦值，tangent.y表示该点切线的正弦值。                 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean |表示是否成功获取距离路径起始点distance处的点的坐标和正切值的结果。true表示获取成功，false表示获取失败，position和tangent不会被改变。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(0, 0);
path.lineTo(0, 700);
path.lineTo(700, 0);
let position: common2D.Point = { x: 0.0, y: 0.0 };
let tangent: common2D.Point = { x: 0.0, y: 0.0 };
if (path.getPositionAndTangent(false, 0.1, position, tangent)) {
  console.info("getPositionAndTangent-----position:  "+ position.x);
  console.info("getPositionAndTangent-----position:  "+ position.y);
  console.info("getPositionAndTangent-----tangent:  "+ tangent.x);
  console.info("getPositionAndTangent-----tangent:  "+ tangent.y);
}
```

### getSegment<sup>20+</sup>

getSegment(forceClosed: boolean, start: number, stop: number, startWithMoveTo: boolean, dst: Path): boolean

截取路径的片段并追加到目标路径上。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| forceClosed | boolean | 是   | 表示是否按照闭合路径测量，true表示测量时路径会被强制视为已闭合，false表示会根据路径的实际闭合状态测量。                 |
| start | number | 是   | 表示与路径起始点的距离，距离路径起始点start距离的位置即为截取路径片段的起始点，小于0时会被视作0，大于等于stop时会截取失败。该参数为浮点数。               |
| stop | number | 是   | 表示与路径起始点的距离，距离路径起始点stop距离的位置即为截取路径片段的终点，小于等于start时会截取失败，大于路径长度时会被视作路径长度。该参数为浮点数。                  |
| startWithMoveTo | boolean | 是   | 表示是否在目标路径执行[moveTo](#moveto)移动到截取路径片段的起始点位置。true表示执行，false表示不执行。                |
| dst | [Path](#path) | 是   | 目标路径，截取成功时会将得到的路径片段追加到目标路径上，截取失败时不做改变。               |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean |表示是否成功截取路径片段。true表示截取成功，false表示截取失败。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(0, 0);
path.lineTo(0, 700);
path.lineTo(700, 0);
let dstPath: drawing.Path = new drawing.Path();
console.info("getSegment-----result:  "+ path.getSegment(true, 10.0, 20.0, true, dstPath));
```

### isClosed<sup>20+</sup>

isClosed(): boolean

获取路径是否闭合。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 表示当前路径是否闭合，true表示闭合，false表示不闭合。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(0, 0);
path.lineTo(0, 700);
if (path.isClosed()) {
  console.info("path is closed.");
} else {
  console.info("path is not closed.");
}
```

### getMatrix<sup>20+</sup>

getMatrix(forceClosed: boolean, distance: number, matrix: Matrix, flags: PathMeasureMatrixFlags): boolean

在路径上的某个位置，获取一个变换矩阵，用于表示该点的坐标和朝向。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| forceClosed | boolean | 是   | 表示是否按照闭合路径测量，true表示测量时路径会被强制视为已闭合，false表示会根据路径的实际闭合状态测量。                  |
| distance | number | 是   | 表示与路径起始点的距离，小于0时会被视作0，大于路径长度时会被视作路径长度。该参数为浮点数。                  |
| matrix | [Matrix](#matrix20) | 是   | 矩阵对象，用于存储得到的矩阵。                  |
| flags | [PathMeasureMatrixFlags](#pathmeasurematrixflags20) | 是   | 矩阵信息维度枚举。                  |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回是否成功获取变换矩阵的结果。true表示成功，false表示失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: Mandatory parameters are left unspecified. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
let matrix = new drawing.Matrix();
if(path.getMatrix(false, 10, matrix, drawing.PathMeasureMatrixFlags.GET_TANGENT_MATRIX)) {
  console.info("path.getMatrix return true");
} else {
  console.info("path.getMatrix return false");
}
```

### buildFromSvgString<sup>20+</sup>

buildFromSvgString(str: string): boolean

解析SVG字符串表示的路径。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| str | string | 是   | SVG格式的字符串，用于描述绘制路径。                 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回是否成功解析SVG字符串的结果。true表示解析成功，false表示解析失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: Mandatory parameters are left unspecified. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
let svgStr: string =  "M150 100 L75 300 L225 300 Z";
if(path.buildFromSvgString(svgStr)) {
  console.info("buildFromSvgString return true");
} else {
  console.info("buildFromSvgString return false");
}
```

### getPathIterator<sup>20+</sup>

getPathIterator(): PathIterator

返回该路径的操作迭代器。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [PathIterator](#pathiterator20) | 该路径的迭代器对象。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
let iter = path.getPathIterator();
```

### approximate<sup>20+</sup>

approximate(acceptableError: number): Array\<number>

将当前路径转化为由连续直线段构成的近似路径。

> **说明：**
>
> - 当acceptableError为0时，曲线路径被极度细分，会严重影响性能和内存消耗，不建议设置误差值为0。
> - 当acceptableError特别大时，路径会极度简化，保留少量关键点，可能会丢失原有形状。
> - 对于椭圆等曲线，当acceptableError过大时，拟合结果通常只包含椭圆的分段贝塞尔曲线的起止点，椭圆形会被极度简化为多边形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| acceptableError | number | 是 | 表示路径上每条线段的可接受误差。该参数为浮点数，不应小于0，当参数小于0时报错。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| Array\<number> | 返回包含近似路径的点的数组，至少包含两个点。每个点由三个值组成：<br>1. 该点所在的位置距离路径起点的长度比例值，范围为[0.0, 1.0]。<br>2. 点的x坐标。<br>3. 点的y坐标。 |

**错误码：**

以下错误码的详细介绍请参见[图形绘制与显示错误码](../apis-arkgraphics2d/errorcode-drawing.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 25900001 | Parameter error.Possible causes: Incorrect parameter range. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(100, 100);
path.lineTo(500, 500);
let points: number[] = path.approximate(0.5);
for (let i = 0; i < points.length; i += 3) {
  console.info("PathApproximate Fraction =" + points[i] + ", X =" + points[i + 1] + ", Y =" + points[i + 2] + "\n");
}
```

### interpolate<sup>20+</sup>

interpolate(other: Path, weight: number, interpolatedPath: Path): boolean

根据给定的权重，在当前路径和另一条路径之间进行插值，并将结果存储在目标路径对象中。两条路径点数相同即可插值成功，目标路径按照当前路径的结构进行创建。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| other | [Path](#path) | 是 | 表示另一条路径对象。 |
| weight | number | 是 | 表示插值权重，必须在[0.0, 1.0]范围内。该参数为浮点数。 |
| interpolatedPath | [Path](#path) | 是 | 表示用于存储插值结果的目标路径对象。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回插值操作是否成功的结果。true表示插值成功，false表示插值失败。 |

**错误码：**

以下错误码的详细介绍请参见[图形绘制与显示错误码](../apis-arkgraphics2d/errorcode-drawing.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 25900001 | Parameter error.Possible causes: Incorrect parameter range. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(50, 50);
path.lineTo(100, 100);
path.lineTo(200, 200);
let other: drawing.Path = new drawing.Path();
other.moveTo(80, 80);
other.lineTo(300, 300);
let interpolatedPath: drawing.Path = new drawing.Path();
if (path.interpolate(other, 0.0, interpolatedPath)) {
  console.info('interpolate return true');
} else {
  console.info('interpolate return false');
}
```

### isInterpolate<sup>20+</sup>

isInterpolate(other: Path): boolean

判断当前路径与另一条路径在结构和操作顺序上是否完全一致，以确定两条路径是否兼容插值。若路径中包含圆锥曲线（Conic）操作，则对应操作的权重值也必须一致，才能视为兼容插值。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| other | [Path](#path) | 是 | 表示另一条路径对象。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回当前路径与另一条路径是否兼容插值的结果。true表示兼容插值，false表示不兼容插值。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let path: drawing.Path = new drawing.Path();
path.moveTo(0, 0);
path.lineTo(100, 100);
let other: drawing.Path = new drawing.Path();
other.moveTo(0, 1);
other.lineTo(200, 200);
if (path.isInterpolate(other)) {
  console.info('isInterpolate return true');
} else {
  console.info('isInterpolate return false');
}
```

## Canvas

承载绘制内容与绘制状态的载体。

> **说明：**
>
> 画布自带一个默认画刷，该画刷为黑色，开启反走样，不具备其他任何样式效果。当画布中没有主动设置画刷和画笔时，该默认画刷生效。

### constructor

constructor(pixelmap: image.PixelMap)

创建一个以PixelMap作为绘制目标的Canvas对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明           |
| -------- | -------------------------------------------- | ---- | -------------- |
| pixelmap | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | 构造函数入参。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
import { image } from '@kit.ImageKit';

const color = new ArrayBuffer(96);
let opts : image.InitializationOptions = {
  editable: true,
  pixelFormat: 3,
  size: {
    height: 4,
    width: 6
  }
}
image.createPixelMap(color, opts).then((pixelMap) => {
  const canvas = new drawing.Canvas(pixelMap);
})
```

### drawRect

drawRect(rect: common2D.Rect): void

绘制一个矩形，默认使用黑色填充。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 绘制的矩形区域。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawRect({ left : 0, right : 10, top : 0, bottom : 10 });
    canvas.detachPen();
  }
}
```

### drawRect<sup>20+</sup>

drawRect(left: number, top: number, right: number, bottom: number): void

绘制一个矩形，默认使用黑色填充。性能优于[drawRect](#drawrect)接口，推荐使用本接口。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| left   | number | 是   | 矩形的左上角x轴坐标，该参数为浮点数。 |
| top    | number | 是   | 矩形的左上角y轴坐标，该参数为浮点数。 |
| right  | number | 是   | 矩形的右下角x轴坐标，该参数为浮点数。 |
| bottom | number | 是   | 矩形的右下角y轴坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {

  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawRect(0, 0, 10, 10);
    canvas.detachPen();
  }
}
```

### drawRoundRect<sup>20+</sup>

drawRoundRect(roundRect: RoundRect): void

画一个圆角矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名     | 类型                    | 必填 | 说明       |
| ---------- | ----------------------- | ---- | ------------ |
| roundRect  | [RoundRect](#roundrect20) | 是   | 圆角矩形对象。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let rect: common2D.Rect = { left : 100, top : 100, right : 400, bottom : 500 };
    let roundRect = new drawing.RoundRect(rect, 10, 10);
    canvas.drawRoundRect(roundRect);
  }
}
```

### drawNestedRoundRect<sup>20+</sup>

drawNestedRoundRect(outer: RoundRect, inner: RoundRect): void

绘制两个嵌套的圆角矩形，外部矩形边界必须包含内部矩形边界，否则无绘制效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名  | 类型                    | 必填 | 说明       |
| ------ | ----------------------- | ---- | ------------ |
| outer  | [RoundRect](#roundrect20) | 是   | 圆角矩形对象，表示外部圆角矩形边界。|
| inner  | [RoundRect](#roundrect20) | 是   | 圆角矩形对象，表示内部圆角矩形边界。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let inRect: common2D.Rect = { left : 200, top : 200, right : 400, bottom : 500 };
    let outRect: common2D.Rect = { left : 100, top : 100, right : 400, bottom : 500 };
    let outRoundRect = new drawing.RoundRect(outRect, 10, 10);
    let inRoundRect = new drawing.RoundRect(inRect, 10, 10);
    canvas.drawNestedRoundRect(outRoundRect, inRoundRect);
    canvas.drawRoundRect(outRoundRect);
  }
}
```

### drawBackground<sup>20+</sup>

drawBackground(brush: Brush): void

使用画刷填充画布的可绘制区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名 | 类型            | 必填 | 说明       |
| ------ | --------------- | ---- | ---------- |
| brush  | [Brush](#brush) | 是   | 画刷对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
    brush.setColor(color);
    canvas.drawBackground(brush);
  }
}
```

### drawShadow<sup>20+</sup>

drawShadow(path: Path, planeParams: common2D.Point3d, devLightPos: common2D.Point3d, lightRadius: number, ambientColor: common2D.Color, spotColor: common2D.Color, flag: ShadowFlag) : void

绘制射灯类型阴影，使用路径描述环境光阴影的轮廓。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型                                       | 必填   | 说明         |
| ------------ | ---------------------------------------- | ---- | ---------- |
| path | [Path](#path)                | 是    | 路径对象，可生成阴影。 |
| planeParams  | [common2D.Point3d](js-apis-graphics-common2D.md#point3d20) | 是    | 表示一个三维向量，用于计算遮挡物相对于画布在z轴上的偏移量，其值取决于x与y坐标。 |
| devLightPos  | [common2D.Point3d](js-apis-graphics-common2D.md#point3d20) | 是    | 光线相对于画布的位置。 |
| lightRadius   | number           | 是    | 圆形灯半径，该参数为浮点数。      |
| ambientColor  | [common2D.Color](js-apis-graphics-common2D.md#color) | 是    | 环境阴影颜色。 |
| spotColor  | [common2D.Color](js-apis-graphics-common2D.md#color) | 是    | 点阴影颜色。 |
| flag         | [ShadowFlag](#shadowflag20)                  | 是    | 阴影标志枚举。    |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const path = new drawing.Path();
    path.addCircle(100, 200, 100, drawing.PathDirection.CLOCKWISE);
    let pen = new drawing.Pen();
    pen.setAntiAlias(true);
    let pen_color : common2D.Color = { alpha: 0xFF, red: 0xFF, green: 0x00, blue: 0x00 };
    pen.setColor(pen_color);
    pen.setStrokeWidth(10.0);
    canvas.attachPen(pen);
    let brush = new drawing.Brush();
    let brush_color : common2D.Color = { alpha: 0xFF, red: 0x00, green: 0xFF, blue: 0x00 };
    brush.setColor(brush_color);
    canvas.attachBrush(brush);
    let point1 : common2D.Point3d = {x: 100, y: 80, z:80};
    let point2 : common2D.Point3d = {x: 200, y: 10, z:40};
    let color1 : common2D.Color = {alpha: 0xFF, red:0, green:0, blue:0xFF};
    let color2 : common2D.Color = {alpha: 0xFF, red:0xFF, green:0, blue:0};
    let shadowFlag : drawing.ShadowFlag = drawing.ShadowFlag.ALL;
    canvas.drawShadow(path, point1, point2, 30, color1, color2, shadowFlag);
  }
}
```

### drawShadow<sup>20+</sup>

drawShadow(path: Path, planeParams: common2D.Point3d, devLightPos: common2D.Point3d, lightRadius: number, ambientColor: common2D.Color | number, spotColor: common2D.Color | number, flag: ShadowFlag) : void

绘制射灯类型阴影，使用路径描述环境光阴影的轮廓。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型                                       | 必填   | 说明         |
| ------------ | ---------------------------------------- | ---- | ---------- |
| path | [Path](#path)                | 是    | 路径对象，可生成阴影。 |
| planeParams  | [common2D.Point3d](js-apis-graphics-common2D.md#point3d20) | 是    | 表示一个三维向量，用于计算z轴方向的偏移量。 |
| devLightPos  | [common2D.Point3d](js-apis-graphics-common2D.md#point3d20) | 是    | 光线相对于画布的位置。 |
| lightRadius   | number           | 是    | 圆形灯半径，该参数为浮点数。      |
| ambientColor  |[common2D.Color](js-apis-graphics-common2D.md#color) \| number | 是    | 环境阴影颜色，可以用16进制ARGB格式的32位无符号整数表示。 |
| spotColor  |[common2D.Color](js-apis-graphics-common2D.md#color) \| number | 是    | 点阴影颜色，可以用16进制ARGB格式的32位无符号整数表示。 |
| flag         | [ShadowFlag](#shadowflag20)                  | 是    | 阴影标志枚举。    |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const path = new drawing.Path();
    path.addCircle(300, 600, 100, drawing.PathDirection.CLOCKWISE);
    let point1 : common2D.Point3d = {x: 100, y: 80, z:80};
    let point2 : common2D.Point3d = {x: 200, y: 10, z:40};
    let shadowFlag : drawing.ShadowFlag = drawing.ShadowFlag.ALL;
    canvas.drawShadow(path, point1, point2, 30, 0xFF0000FF, 0xFFFF0000, shadowFlag);
  }
}
```

### getLocalClipBounds<sup>20+</sup>

getLocalClipBounds(): common2D.Rect

获取画布裁剪区域的边界。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                                       | 说明       |
| ---------------------------------------- | -------- |
| [common2D.Rect](js-apis-graphics-common2D.md#rect) | 返回画布裁剪区域的矩形边界。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let clipRect: common2D.Rect = {
      left : 150, top : 150, right : 300, bottom : 400
    };
    canvas.clipRect(clipRect,drawing.ClipOp.DIFFERENCE, true);
    console.info("test rect.left: " + clipRect.left);
    console.info("test rect.top: " + clipRect.top);
    console.info("test rect.right: " + clipRect.right);
    console.info("test rect.bottom: " + clipRect.bottom);
    canvas.getLocalClipBounds();
  }
}
```

### getTotalMatrix<sup>20+</sup>

getTotalMatrix(): Matrix

获取画布矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                | 说明       |
| ----------------- | -------- |
| [Matrix](#matrix20) |返回画布矩阵。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let matrix = new drawing.Matrix();
    matrix.setMatrix([5, 0, 0, 0, 1, 1, 0, 0, 1]);
    canvas.setMatrix(matrix);
    let matrixResult =canvas.getTotalMatrix();
  }
}
```

### drawCircle

drawCircle(x: number, y: number, radius: number): void

绘制一个圆形。如果半径小于等于零，则不绘制。默认使用黑色填充。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                |
| ------ | ------ | ---- | ------------------- |
| x      | number | 是   | 圆心的x坐标，该参数为浮点数。 |
| y      | number | 是   | 圆心的y坐标，该参数为浮点数。 |
| radius | number | 是   | 圆的半径，大于0的浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawCircle(10, 10, 2);
    canvas.detachPen();
  }
}
```

### drawImage

drawImage(pixelmap: image.PixelMap, left: number, top: number, samplingOptions?: SamplingOptions): void

画一张图片，图片的左上角坐标为(left, top)。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| pixelmap | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | 图片的PixelMap。                  |
| left     | number                                       | 是   | 图片位置的左上角x轴坐标，该参数为浮点数。 |
| top      | number                                       | 是   | 图片位置的左上角y轴坐标，该参数为浮点数。 |
| samplingOptions<sup>20+</sup>  | [SamplingOptions](#samplingoptions20)  | 否  | 采样选项对象，默认为不使用任何参数构造的原始采样选项对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { image } from '@kit.ImageKit';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  pixelMap: image.PixelMap | null = null;

  async draw(context : DrawContext) {
    const canvas = context.canvas;
    let options = new drawing.SamplingOptions(drawing.FilterMode.FILTER_MODE_NEAREST);
    if (this.pixelMap != null) {
      canvas.drawImage(this.pixelMap, 0, 0, options);
    }
  }
}
```

### drawImageRect<sup>20+</sup>

drawImageRect(pixelmap: image.PixelMap, dstRect: common2D.Rect, samplingOptions?: SamplingOptions): void

将图片绘制到画布的指定区域上。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| pixelmap | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | 图片的PixelMap。                 |
| dstRect     | [common2D.Rect](js-apis-graphics-common2D.md#rect)                               | 是   | 矩形对象，用于指定画布上图片的绘制区域。 |
| samplingOptions     | [SamplingOptions](#samplingoptions20)                           | 否   | 采样选项对象，默认为不使用任何参数构造的原始采样选项对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { image } from '@kit.ImageKit';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
pixelMap: image.PixelMap | null = null;
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let pen = new drawing.Pen();
    canvas.attachPen(pen);
    let rect: common2D.Rect = { left: 0, top: 0, right: 200, bottom: 200 };
    canvas.drawImageRect(this.pixelMap, rect);
    canvas.detachPen();
  }
}
```

### drawImageRectWithSrc<sup>20+</sup>

drawImageRectWithSrc(pixelmap: image.PixelMap, srcRect: common2D.Rect, dstRect: common2D.Rect, samplingOptions?: SamplingOptions, constraint?: SrcRectConstraint): void

将图片的指定区域绘制到画布的指定区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| pixelmap | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | 图片的PixelMap。                 |
| srcRect     | [common2D.Rect](js-apis-graphics-common2D.md#rect)                               | 是   | 矩形对象，用于指定图片的待绘制区域。 |
| dstRect     | [common2D.Rect](js-apis-graphics-common2D.md#rect)                               | 是   | 矩形对象，用于指定画布上图片的绘制区域。 |
| samplingOptions     | [SamplingOptions](#samplingoptions20)                           | 否   | 采样选项对象，默认为不使用任何参数构造的原始采样选项对象。 |
| constraint     | [SrcRectConstraint](#srcrectconstraint20)                        | 否   | 源矩形区域约束类型，默认为STRICT。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { image } from '@kit.ImageKit';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
pixelMap: image.PixelMap | null = null;
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let pen = new drawing.Pen();
    canvas.attachPen(pen);
    let srcRect: common2D.Rect = { left: 0, top: 0, right: 100, bottom: 100 };
    let dstRect: common2D.Rect = { left: 100, top: 100, right: 200, bottom: 200 };
    canvas.drawImageRectWithSrc(this.pixelMap, srcRect, dstRect);
    canvas.detachPen();
  }
}
```

### drawColor

drawColor(color: common2D.Color, blendMode?: BlendMode): void

使用指定颜色并按照指定的[BlendMode](#blendmode)对画布当前可绘制区域进行填充。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名    | 类型                                                 | 必填 | 说明                             |
| --------- | ---------------------------------------------------- | ---- | -------------------------------- |
| color     | [common2D.Color](js-apis-graphics-common2D.md#color) | 是   | ARGB格式的颜色，每个颜色通道的值是0到255之间的整数。                   |
| blendMode | [BlendMode](#blendmode)                              | 否   | 颜色混合模式，默认模式为SRC_OVER。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let color: common2D.Color = {
      alpha : 255,
      red: 0,
      green: 10,
      blue: 10
    }
    canvas.drawColor(color, drawing.BlendMode.CLEAR);
  }
}
```

### drawColor<sup>20+</sup>

drawColor(alpha: number, red: number, green: number, blue: number, blendMode?: BlendMode): void

使用指定颜色并按照指定的[BlendMode](#blendmode)对画布当前可绘制区域进行填充。性能优于[drawColor](#drawcolor)接口，推荐使用本接口。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型                    | 必填 | 说明                                               |
| --------- | ----------------------- | ---- | ------------------------------------------------- |
| alpha     | number                  | 是   | ARGB格式颜色的透明度通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。 |
| red       | number                  | 是   | ARGB格式颜色的红色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |
| green     | number                  | 是   | ARGB格式颜色的绿色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |
| blue      | number                  | 是   | ARGB格式颜色的蓝色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |
| blendMode | [BlendMode](#blendmode) | 否   | 颜色混合模式，默认模式为SRC_OVER。                   |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    canvas.drawColor(255, 0, 10, 10, drawing.BlendMode.CLEAR);
  }
}
```

### drawColor<sup>20+</sup>

drawColor(color: number, blendMode?: BlendMode): void

使用指定颜色并按照指定的[BlendMode](#blendmode)对画布当前可绘制区域进行填充。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名    | 类型                                                 | 必填 | 说明                             |
| --------- | ---------------------------------------------------- | ---- | -------------------------------- |
| color     | number | 是   | 16进制ARGB格式的颜色。                   |
| blendMode | [BlendMode](#blendmode)                              | 否   | 颜色混合模式，默认模式为SRC_OVER。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    canvas.drawColor(0xff000a0a, drawing.BlendMode.CLEAR);
  }
}
```

### drawPixelMapMesh<sup>20+</sup>

drawPixelMapMesh(pixelmap: image.PixelMap, meshWidth: number, meshHeight: number, vertices: Array\<number>, vertOffset: number, colors: Array\<number>, colorOffset: number): void

在网格上绘制像素图，网格均匀分布在像素图上。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名      | 类型            | 必填 | 说明                            |
| ----------- | -------------  | ---- | ------------------------------- |
| pixelmap    | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | 用于绘制网格的像素图。 |
| meshWidth   | number         | 是   | 网格中的列数，大于0的整数。 |
| meshHeight  | number         | 是   | 网格中的行数，大于0的整数。 |
| vertices    | Array\<number> | 是   | 顶点数组，指定网格的绘制位置，浮点数组，大小必须为((meshWidth+1) * (meshHeight+1) + vertOffset) * 2。 |
| vertOffset  | number         | 是   | 绘图前要跳过的vert元素数，大于等于0的整数。 |
| colors      | Array\<number> | 是   | 颜色数组，在每个顶点指定一种颜色，整数数组，可为null，大小必须为(meshWidth+1) * (meshHeight+1) + colorOffset。 |
| colorOffset | number         | 是   | 绘制前要跳过的颜色元素数，大于等于0的整数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { image } from '@kit.ImageKit';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  pixelMap: image.PixelMap | null = null;

  async draw(context : DrawContext) {
    const canvas = context.canvas;
    if (this.pixelMap != null) {
      const brush = new drawing.Brush(); // 只支持brush，使用pen没有绘制效果。
      canvas.attachBrush(brush);
      let verts : Array<number> = [0, 0, 50, 0, 410, 0, 0, 180, 50, 180, 410, 180, 0, 360, 50, 360, 410, 360]; // 18
      canvas.drawPixelMapMesh(this.pixelMap, 2, 2, verts, 0, null, 0);
      canvas.detachBrush();
    }
  }
}
```

### clear<sup>20+</sup>

clear(color: common2D.Color): void

使用指定颜色填充画布上的裁剪区域。效果等同于[drawColor](#drawcolor)。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名    | 类型                                                 | 必填 | 说明                             |
| --------- | ---------------------------------------------------- | ---- | -------------------------------- |
| color     | [common2D.Color](js-apis-graphics-common2D.md#color) | 是   | ARGB格式的颜色，每个颜色通道的值是0到255之间的整数。      |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let color: common2D.Color = {alpha: 255, red: 255, green: 0, blue: 0};
    canvas.clear(color);
  }
}
```

### clear<sup>20+</sup>

clear(color: common2D.Color | number): void

使用指定颜色填充画布上的裁剪区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名    | 类型                                                 | 必填 | 说明                             |
| --------- | ---------------------------------------------------- | ---- | -------------------------------- |
| color     | [common2D.Color](js-apis-graphics-common2D.md#color) \| number| 是   | 颜色，可以用16进制ARGB格式的无符号整数表示。  |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let color: number = 0xffff0000;
    canvas.clear(color);
  }
}
```

### getWidth<sup>20+</sup>

getWidth(): number

获取画布的宽度。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 必填 | 说明           |
| ------ | ---- | -------------- |
| number | 是   | 返回画布的宽度，该参数为浮点数。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let width = canvas.getWidth();
    console.info('get canvas width:' + width);
  }
}
```

### getHeight<sup>20+</sup>

getHeight(): number

获取画布的高度。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 必填 | 说明           |
| ------ | ---- | -------------- |
| number | 是   | 返回画布的高度，该参数为浮点数。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let height = canvas.getHeight();
    console.log('get canvas height:' + height);
  }
}
```

### drawOval<sup>20+</sup>

drawOval(oval: common2D.Rect): void

在画布上绘制一个椭圆，椭圆的形状和位置由椭圆的外切矩形给出。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| oval   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形区域，该矩形的内切椭圆即为待绘制椭圆。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
    pen.setColor(color);
    canvas.attachPen(pen);
    const rect: common2D.Rect = {left:100, top:50, right:400, bottom:500};
    canvas.drawOval(rect);
    canvas.detachPen();
  }
}
```

### drawArc<sup>20+</sup>

drawArc(arc: common2D.Rect, startAngle: number, sweepAngle: number): void

在画布上绘制圆弧。该方法允许指定起始角度、扫描角度。当扫描角度的绝对值大于360度时，则绘制椭圆。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| arc   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 包含要绘制的圆弧的椭圆的矩形边界。 |
| startAngle      | number | 是   | 弧的起始角度，单位为度，该参数为浮点数。0度时起始点位于椭圆的右端点，正数时以顺时针方向放置起始点，负数时以逆时针方向放置起始点。 |
| sweepAngle      | number | 是   | 弧的扫描角度，单位为度，该参数为浮点数。为正数时顺时针扫描，为负数时逆时针扫描。它的有效范围在-360度到360度之间，当绝对值大于360度时，该方法绘制的是一个椭圆。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
    pen.setColor(color);
    canvas.attachPen(pen);
    const rect: common2D.Rect = {left:100, top:50, right:400, bottom:200};
    canvas.drawArc(rect, 90, 180);
    canvas.detachPen();
  }
}
```

### drawPoint

drawPoint(x: number, y: number): void

绘制一个点。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                |
| ------ | ------ | ---- | ------------------- |
| x      | number | 是   | 点的x轴坐标，该参数为浮点数。 |
| y      | number | 是   | 点的y轴坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawPoint(10, 10);
    canvas.detachPen();
  }
}
```

### drawPoints<sup>20+</sup>

drawPoints(points: Array\<common2D.Point>, mode?: PointMode): void

在画布上绘制一组点、线段或多边形。通过指定点的数组和绘制模式来决定绘制方式。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型                                       | 必填   | 说明        |
| ---- | ---------------------------------------- | ---- | --------- |
| points  | Array\<[common2D.Point](js-apis-graphics-common2D.md#point20)> | 是    | 要绘制的点的数组。长度不能为0。   |
| mode | [PointMode](#pointmode20)                  | 否    | 绘制数组中的点的方式，默认为drawing.PointMode.POINTS。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed.|

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(30);
    const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
    pen.setColor(color);
    canvas.attachPen(pen);
    canvas.drawPoints([{x: 100, y: 200}, {x: 150, y: 230}, {x: 200, y: 300}], drawing.PointMode.POINTS);
    canvas.detachPen();
  }
}
```

### drawPath

drawPath(path: Path): void

绘制一个自定义路径，该路径包含了一组路径轮廓，每个路径轮廓可以是开放的或封闭的。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型          | 必填 | 说明               |
| ------ | ------------- | ---- | ------------------ |
| path   | [Path](#path) | 是   | 要绘制的路径对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    let path = new drawing.Path();
    path.moveTo(10,10);
    path.cubicTo(10, 10, 10, 10, 15, 15);
    path.close();
    canvas.attachPen(pen);
    canvas.drawPath(path);
    canvas.detachPen();
  }
}
```

### drawLine

drawLine(x0: number, y0: number, x1: number, y1: number): void

画一条直线段，从指定的起点到终点。如果直线段的起点和终点是同一个点，无法绘制。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| x0     | number | 是   | 线段起点的X坐标，该参数为浮点数。 |
| y0     | number | 是   | 线段起点的Y坐标，该参数为浮点数。 |
| x1     | number | 是   | 线段终点的X坐标，该参数为浮点数。 |
| y1     | number | 是   | 线段终点的Y坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawLine(0, 0, 20, 20);
    canvas.detachPen();
  }
}
```

### drawTextBlob

drawTextBlob(blob: TextBlob, x: number, y: number): void

绘制一段文字。若构造blob的字体不支持待绘制字符，则该部分字符无法绘制。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                  | 必填 | 说明                                       |
| ------ | --------------------- | ---- | ------------------------------------------ |
| blob   | [TextBlob](#textblob) | 是   | TextBlob对象。                             |
| x      | number                | 是   | 所绘制出的文字基线（下图蓝线）的左端点（下图红点）的横坐标，该参数为浮点数。 |
| y      | number                | 是   | 所绘制出的文字基线（下图蓝线）的左端点（下图红点）的纵坐标，该参数为浮点数。 |

![zh-ch_image_Text_Blob.png](figures/zh-ch_image_Text_Blob.png)

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    brush.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    const font = new drawing.Font();
    font.setSize(20);
    const textBlob = drawing.TextBlob.makeFromString("Hello, drawing", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    canvas.attachBrush(brush);
    canvas.drawTextBlob(textBlob, 20, 20);
    canvas.detachBrush();
  }
}
```

### drawSingleCharacter<sup>20+</sup>

drawSingleCharacter(text: string, font: Font, x: number, y: number): void

绘制单个字符。当前字型中的字体不支持待绘制字符时，退化到使用系统字体绘制字符。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名 | 类型                | 必填 | 说明        |
| ------ | ------------------- | ---- | ----------- |
| text   | string | 是   | 待绘制的单个字符，字符串的长度必须为1。  |
| font   | [Font](#font) | 是   | 字型对象。  |
| x      | number | 是   | 所绘制出的字符基线（下图蓝线）的左端点（下图红点）的横坐标，该参数为浮点数。 |
| y      | number | 是   | 所绘制出的字符基线（下图蓝线）的左端点（下图红点）的纵坐标，该参数为浮点数。 |

![zh-ch_image_Text_Blob.png](figures/zh-ch_image_Text_Blob.png)

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    brush.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    const font = new drawing.Font();
    font.setSize(20);
    canvas.attachBrush(brush);
    canvas.drawSingleCharacter("你", font, 100, 100);
    canvas.drawSingleCharacter("好", font, 120, 100);
    canvas.detachBrush();
  }
}
```

### drawRegion<sup>20+</sup>

drawRegion(region: Region): void

绘制一个区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名 | 类型                | 必填 | 说明        |
| ------ | ------------------- | ---- | ----------- |
| region   | [Region](#region20) | 是   | 绘制的区域。  |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    let region = new drawing.Region();
    pen.setStrokeWidth(10);
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    canvas.attachPen(pen);
    region.setRect(100, 100, 400, 400);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### attachPen

attachPen(pen: Pen): void

绑定画笔到画布上，在画布上进行绘制时，将使用画笔的样式去绘制图形形状的轮廓。

> **说明：**
>
> 执行该方法后，若pen的效果发生改变并且开发者希望该变化生效于接下来的绘制动作，需要再次执行该方法以确保变化生效。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型        | 必填 | 说明       |
| ------ | ----------- | ---- | ---------- |
| pen    | [Pen](#pen) | 是   | 画笔对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawRect({ left : 0, right : 10, top : 0, bottom : 10 });
    canvas.detachPen();
  }
}
```

### attachBrush

attachBrush(brush: Brush): void

绑定画刷到画布上，在画布上进行绘制时，将使用画刷的样式对绘制图形形状的内部进行填充。

> **说明：**
>
> 执行该方法后，若brush的效果发生改变并且开发者希望该变化生效于接下来的绘制动作，需要再次执行该方法以确保变化生效。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型            | 必填 | 说明       |
| ------ | --------------- | ---- | ---------- |
| brush  | [Brush](#brush) | 是   | 画刷对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    brush.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachBrush(brush);
    canvas.drawRect({ left : 0, right : 10, top : 0, bottom : 10 });
    canvas.detachBrush();
  }
}
```

### detachPen

detachPen(): void

将画笔与画布解绑，在画布上进行绘制时，不会再使用画笔去绘制图形形状的轮廓。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawRect({ left : 0, right : 10, top : 0, bottom : 10 });
    canvas.detachPen();
  }
}
```

### detachBrush

detachBrush(): void

将画刷与画布解绑，在画布上进行绘制时，不会再使用画刷对绘制图形形状的内部进行填充。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    brush.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachBrush(brush);
    canvas.drawRect({ left : 0, right : 10, top : 0, bottom : 10 });
    canvas.detachBrush();
  }
}
```

### clipPath<sup>20+</sup>

clipPath(path: Path, clipOp?: ClipOp, doAntiAlias?: boolean): void

使用自定义路径对画布的可绘制区域进行裁剪。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名       | 类型               | 必填 | 说明                                |
| ------------ | ----------------- | ---- | ------------------------------------|
| path         | [Path](#path)     | 是   | 路径对象。                                                 |
| clipOp       | [ClipOp](#clipop20) | 否   | 裁剪方式。默认为INTERSECT。                                     |
| doAntiAlias  | boolean           | 否   | 表示是否使能抗锯齿绘制。true表示使能，false表示不使能。默认为false。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let path = new drawing.Path();
    path.moveTo(10, 10);
    path.cubicTo(100, 100, 80, 150, 300, 150);
    path.close();
    canvas.clipPath(path, drawing.ClipOp.INTERSECT, true);
    canvas.clear({alpha: 255, red: 255, green: 0, blue: 0});
  }
}
```

### clipRect<sup>20+</sup>

clipRect(rect: common2D.Rect, clipOp?: ClipOp, doAntiAlias?: boolean): void

使用矩形对画布的可绘制区域进行裁剪。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| rect        | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是    | 需要裁剪的矩形区域。      |
| clipOp      | [ClipOp](#clipop20)                  | 否    | 裁剪方式。默认为INTERSECT。     |
| doAntiAlias | boolean           | 否   | 表示是否使能抗锯齿绘制。true表示使能，false表示不使能。默认为false。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    canvas.clipRect({left : 10, right : 500, top : 300, bottom : 900}, drawing.ClipOp.DIFFERENCE, true);
    canvas.clear({alpha: 255, red: 255, green: 0, blue: 0});
  }
}
```

### save<sup>20+</sup>

save(): number

保存当前画布状态（画布矩阵和可绘制区域）到栈顶。需要与恢复接口[restore](#restore20)配合使用。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                |
| ------ | ------------------ |
| number | 画布状态个数，该参数为正整数。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let rect: common2D.Rect = {left: 10, right: 200, top: 100, bottom: 300};
    canvas.drawRect(rect);
    let saveCount = canvas.save();
  }
}
```

### saveLayer<sup>20+</sup>

saveLayer(rect?: common2D.Rect | null, brush?: Brush | null): number

保存当前画布的矩阵和裁剪区域，并为后续绘制分配位图。调用恢复接口[restore](#restore20)将会舍弃对矩阵和裁剪区域做的更改，并绘制位图。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型     | 必填   | 说明         |
| ---- | ------ | ---- | ----------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect)\|null | 否   | 矩形对象，用于限制图层大小，默认为当前画布大小。 |
| brush  | [Brush](#brush)\|null | 否   | 画刷对象，绘制位图时会应用画刷对象的透明度，颜色滤波器效果和混合模式，默认不设置额外效果。 |

**返回值：**

| 类型   | 说明                |
| ------ | ------------------ |
| number | 返回调用前保存的画布状态数，该参数为正整数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: Mandatory parameters are left unspecified. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    canvas.saveLayer(null, null);
    const brushRect = new drawing.Brush();
    const colorRect: common2D.Color = {alpha: 255, red: 255, green: 255, blue: 0};
    brushRect.setColor(colorRect);
    canvas.attachBrush(brushRect);
    const rect: common2D.Rect = {left:100, top:100, right:500, bottom:500};
    canvas.drawRect(rect);

    const brush = new drawing.Brush();
    brush.setBlendMode(drawing.BlendMode.DST_OUT);
    canvas.saveLayer(rect, brush);

    const brushCircle = new drawing.Brush();
    const colorCircle: common2D.Color = {alpha: 255, red: 0, green: 0, blue: 255};
    brushCircle.setColor(colorCircle);
    canvas.attachBrush(brushCircle);
    canvas.drawCircle(500, 500, 200);
    canvas.restore();
    canvas.restore();
    canvas.detachBrush();
  }
}
```

### scale<sup>20+</sup>

scale(sx: number, sy: number): void

在当前画布矩阵（默认是单位矩阵）的基础上再叠加一个缩放矩阵，后续绘制操作和裁剪操作的形状和位置都会自动叠加一个缩放效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型     | 必填   | 说明         |
| ---- | ------ | ---- | ----------------- |
| sx   | number | 是   | x轴方向的缩放比例，该参数为浮点数。 |
| sy   | number | 是   | y轴方向的缩放比例，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.scale(2, 0.5);
    canvas.drawRect({left : 10, right : 500, top : 300, bottom : 900});
    canvas.detachPen();
  }
}
```

### skew<sup>20+</sup>

skew(sx: number, sy: number) : void

在当前画布矩阵（默认是单位矩阵）的基础上再叠加一个倾斜矩阵，后续绘制操作和裁剪操作的形状和位置都会自动叠加一个倾斜效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型     | 必填   | 说明         |
| ---- | ------ | ---- | ----------------- |
| sx   | number | 是   | x轴上的倾斜量，该参数为浮点数。正值会使绘制沿y轴增量方向向右倾斜；负值会使绘制沿y轴增量方向向左倾斜。    |
| sy   | number | 是   | y轴上的倾斜量，该参数为浮点数。正值会使绘制沿x轴增量方向向下倾斜；负值会使绘制沿x轴增量方向向上倾斜。    |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.skew(0.1, 0.1);
    canvas.drawRect({left : 10, right : 500, top : 300, bottom : 900});
    canvas.detachPen();
  }
}
```

### rotate<sup>20+</sup>

rotate(degrees: number, sx: number, sy: number) : void

在当前画布矩阵（默认是单位矩阵）的基础上再叠加一个旋转矩阵，后续绘制操作和裁剪操作的形状和位置都会自动叠加一个旋转效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型     | 必填   | 说明         |
| ---- | ------ | ------ | ------------------------ |
| degrees       | number | 是    | 旋转角度，单位为度，该参数为浮点数，正数为顺时针旋转，负数为逆时针旋转。  |
| sx            | number | 是    | 旋转中心的横坐标，该参数为浮点数。 |
| sy            | number | 是    | 旋转中心的纵坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.rotate(30, 100, 100);
    canvas.drawRect({left : 10, right : 500, top : 300, bottom : 900});
    canvas.detachPen();
  }
}
```

### translate<sup>20+</sup>

translate(dx: number, dy: number): void

在当前画布矩阵（默认是单位矩阵）的基础上再叠加一个平移矩阵，后续绘制操作和裁剪操作的形状和位置都会自动叠加一个平移效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                |
| ----- | ------ | ---- | ------------------- |
| dx    | number | 是   | x轴方向的移动距离，该参数为浮点数。   |
| dy    | number | 是   | y轴方向的移动距离，该参数为浮点数。   |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.translate(10, 10);
    canvas.drawRect({left : 10, right : 500, top : 300, bottom : 900});
    canvas.detachPen();
  }
}
```

### getSaveCount<sup>20+</sup>

getSaveCount(): number

获取栈中保存的画布状态（画布矩阵和裁剪区域）的数量。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明                                 |
| ------ | ------------------------------------ |
| number | 已保存的画布状态的数量，该参数为正整数。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawRect({left: 10, right: 200, top: 100, bottom: 300});
    canvas.save();
    canvas.drawRect({left : 10, right : 500, top : 300, bottom : 900});
    canvas.getSaveCount();
    canvas.detachPen();
  }
}
```

### restoreToCount<sup>20+</sup>

restoreToCount(count: number): void

恢复到指定数量的画布状态（画布矩阵和裁剪区域）。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型     | 必填   | 说明                    |
| ----- | ------ | ---- | ----------------------------- |
| count | number | 是   | 要恢复的画布状态深度，该参数为整数。小于等于1时，恢复为初始状态；大于已保存的画布状态数量时，不执行任何操作。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.drawRect({left: 10, right: 200, top: 100, bottom: 300});
    canvas.save();
    canvas.drawRect({left: 10, right: 200, top: 100, bottom: 500});
    canvas.save();
    canvas.drawRect({left: 100, right: 300, top: 100, bottom: 500});
    canvas.save();
    canvas.restoreToCount(2);
    canvas.drawRect({left : 10, right : 500, top : 300, bottom : 900});
    canvas.detachPen();
  }
}
```

### restore<sup>20+</sup>

restore(): void

恢复保存在栈顶的画布状态（画布矩阵和裁剪区域）。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    canvas.restore();
    canvas.detachPen();
  }
}
```

### concatMatrix<sup>20+</sup>

concatMatrix(matrix: Matrix): void

画布现有矩阵左乘传入矩阵，不影响之前的绘制操作，后续绘制操作和裁剪操作的形状和位置都会受到该矩阵的影响。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名    | 类型                | 必填   | 说明    |
| ------ | ----------------- | ---- | ----- |
| matrix | [Matrix](#matrix20) | 是    | 矩阵对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let matrix = new drawing.Matrix();
    matrix.setMatrix([5, 0, 0, 0, 1, 2, 0, 0, 1]);
    canvas.concatMatrix(matrix);
    canvas.drawRect({left: 10, right: 200, top: 100, bottom: 500});
  }
}
```

### setMatrix<sup>20+</sup>

setMatrix(matrix: Matrix): void

设置画布的矩阵，后续绘制操作和裁剪操作的形状和位置都会受到该矩阵的影响。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名    | 类型                | 必填   | 说明    |
| ------ | ----------------- | ---- | ----- |
| matrix | [Matrix](#matrix20) | 是    | 矩阵对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let matrix = new drawing.Matrix()
    matrix.setMatrix([5, 0, 0, 0, 1, 1, 0, 0, 1]);
    canvas.setMatrix(matrix);
    canvas.drawRect({left: 10, right: 200, top: 100, bottom: 500});
  }
}
```

### isClipEmpty<sup>20+</sup>

isClipEmpty(): boolean

判断裁剪后的可绘制区域是否为空。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回画布的可绘制区域是否为空的结果，true表示为空，false表示不为空。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    if (canvas.isClipEmpty()) {
      console.info("canvas.isClipEmpty() returned true");
    } else {
      console.info("canvas.isClipEmpty() returned false");
    }
  }
}
```

### clipRegion<sup>20+</sup>

clipRegion(region: Region, clipOp?: ClipOp): void

在画布上裁剪一个区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| region | [Region](#region20) | 是   | 区域对象，表示裁剪范围。 |
| clipOp | [ClipOp](#clipop20)   | 否   | 裁剪方式，默认为INTERSECT。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let region : drawing.Region = new drawing.Region();
    region.setRect(0, 0, 500, 500);
    canvas.clipRegion(region);
    let color: common2D.Color = {alpha: 255, red: 255, green: 0, blue: 0};
    canvas.clear(color);
  }
}
```

### clipRoundRect<sup>20+</sup>

clipRoundRect(roundRect: RoundRect, clipOp?: ClipOp, doAntiAlias?: boolean): void

在画布上裁剪一个圆角矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| roundRect | [RoundRect](#roundrect20) | 是   | 圆角矩形对象，表示裁剪范围。 |
| clipOp | [ClipOp](#clipop20)   | 否   | 裁剪方式，默认为INTERSECT。 |
| doAntiAlias | boolean | 否   | 表示是否使能抗锯齿。true表示使能，false表示不使能。默认为false。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let rect: common2D.Rect = { left: 10, top: 100, right: 200, bottom: 300 };
    let roundRect = new drawing.RoundRect(rect, 10, 10);
    canvas.clipRoundRect(roundRect);
    let color: common2D.Color = {alpha: 255, red: 255, green: 0, blue: 0};
    canvas.clear(color);
  }
}
```

### resetMatrix<sup>20+</sup>

resetMatrix(): void

将当前画布的矩阵重置为单位矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    canvas.scale(4, 6);
    canvas.resetMatrix();
  }
}
```

### quickRejectPath<sup>20+</sup>

quickRejectPath(path: Path): boolean

判断路径与画布区域是否不相交。画布区域包含边界。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型          | 必填 | 说明               |
| ------ | ------------- | ---- | ------------------ |
| path   | [Path](#path) | 是   | 路径对象。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回路径是否与画布区域不相交的结果。true表示路径与画布区域不相交，false表示路径与画布区域相交。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let path = new drawing.Path();
    path.moveTo(10, 10);
    path.cubicTo(10, 10, 10, 10, 15, 15);
    path.close();
    if (canvas.quickRejectPath(path)) {
      console.info("canvas and path do not intersect.");
    } else {
      console.info("canvas and path intersect.");
    }
  }
}
```

### quickRejectRect<sup>20+</sup>

quickRejectRect(rect: common2D.Rect): boolean

判断矩形和画布区域是否不相交。画布区域包含边界。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形区域。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回矩形是否与画布区域不相交的结果。true表示矩形与画布区域不相交，false表示矩形与画布区域相交。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let rect: common2D.Rect = { left : 10, top : 20, right : 50, bottom : 30 };
    if (canvas.quickRejectRect(rect)) {
      console.info("canvas and rect do not intersect.");
    } else {
      console.info("canvas and rect intersect.");
    }
  }
}
```

### drawArcWithCenter<sup>20+</sup>

drawArcWithCenter(arc: common2D.Rect, startAngle: number, sweepAngle: number, useCenter: boolean): void

在画布上绘制圆弧。该方法允许指定圆弧的起始角度、扫描角度以及圆弧的起点和终点是否连接圆弧的中心点。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| arc   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 包含要绘制的圆弧的椭圆的矩形边界。 |
| startAngle      | number | 是   | 弧的起始角度，单位为度，该参数为浮点数。0度时起始点位于椭圆的右端点，为正数时以顺时针方向放置起始点，为负数时以逆时针方向放置起始点。 |
| sweepAngle      | number | 是   | 弧的扫描角度，单位为度，该参数为浮点数。为正数时顺时针扫描，为负数时逆时针扫描。扫描角度可以超过360度，将绘制一个完整的椭圆。 |
| useCenter       | boolean | 是   | 绘制时弧形的起点和终点是否连接弧形的中心点。true表示连接，false表示不连接。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
    pen.setColor(color);
    canvas.attachPen(pen);
    const rect: common2D.Rect = { left: 100, top: 50, right: 400, bottom: 200 };
    canvas.drawArcWithCenter(rect, 90, 180, false);
    canvas.detachPen();
  }
}
```

### drawImageNine<sup>20+</sup>

drawImageNine(pixelmap: image.PixelMap, center: common2D.Rect, dstRect: common2D.Rect, filterMode: FilterMode): void

通过绘制两条水平线和两条垂直线将图像分割成9个部分：四个边，四个角和中心。<br>
若角落的4个区域尺寸不超过目标矩形，则会在不缩放的情况下被绘制在目标矩形，反之则会按比例缩放绘制在目标矩形；如果还有剩余空间，剩下的5个区域会通过拉伸或压缩来绘制，以便能够完全覆盖目标矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| pixelmap   | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | 用于绘制网格的像素图。 |
| center    | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 分割图像的中心矩形。矩形四条边所在的直线将图像分成了9个部分。 |
| dstRect  | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 在画布上绘制的目标矩形区域。 |
| filterMode | [FilterMode](#filtermode20) | 是   | 过滤模式。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';
import { image } from '@kit.ImageKit';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let pixelMap: image.PixelMap = globalThis.getInstance().getPixelMap("test_2.jpg");
    canvas.drawImage(pixelMap, 0, 0); // 原图
    let center: common2D.Rect = { left: 20, top: 10, right: 50, bottom: 40 };
    let dst: common2D.Rect = { left: 70, top: 0, right: 100, bottom: 30 };
    let dst1: common2D.Rect = { left: 110, top: 0, right: 200, bottom: 90 };
    canvas.drawImageNine(pixelMap, center, dst, drawing.FilterMode.FILTER_MODE_NEAREST); // 示例1
    canvas.drawImageNine(pixelMap, center, dst1, drawing.FilterMode.FILTER_MODE_NEAREST); // 示例2
  }
}
```
![zh-ch_image_Nine.png](figures/zh-ch_image_Nine.png)

### drawImageLattice<sup>20+</sup>

drawImageLattice(pixelmap: image.PixelMap, lattice: Lattice, dstRect: common2D.Rect, filterMode: FilterMode): void

将图像按照矩形网格对象的设置划分为多个网格，并把图像的每个部分按照网格对象的设置绘制到画布上的目标矩形区域。<br>
偶数行和列（起始计数为0）的每个交叉点都是固定的，若固定网格区域的尺寸不超过目标矩形，则会在不缩放的情况下被绘制在目标矩形，反之则会按比例缩放绘制在目标矩形；如果还有剩余空间，剩下的区域会通过拉伸或压缩来绘制，以便能够完全覆盖目标矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| pixelmap   | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7) | 是   | 用于绘制网格的像素图。 |
| lattice  | [Lattice](#lattice20) | 是   | 矩形网格对象。 |
| dstRect    | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 目标矩形区域。 |
| filterMode | [FilterMode](#filtermode20) | 是   | 过滤模式。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';
import { image } from '@kit.ImageKit';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let pixelMap: image.PixelMap = globalThis.getInstance().getPixelMap("test_3.jpg");
    canvas.drawImage(pixelMap, 0, 0); // 原图
    let xDivs: Array<number> = [28, 36, 44, 52];
    let yDivs: Array<number> = [28, 36, 44, 52];
    let lattice = drawing.Lattice.createImageLattice(xDivs, yDivs, 4, 4);
    let dst: common2D.Rect = { left: 100, top: 0, right: 164, bottom: 64 };
    let dst1: common2D.Rect = { left: 200, top: 0, right: 360, bottom: 160 };
    canvas.drawImageLattice(pixelMap, lattice, dst, drawing.FilterMode.FILTER_MODE_NEAREST); // 示例1
    canvas.drawImageLattice(pixelMap, lattice, dst1, drawing.FilterMode.FILTER_MODE_NEAREST); // 示例2
  }
}
```
![zh-ch_image_Lattice.png](figures/zh-ch_image_Lattice.png)

## ImageFilter<sup>20+</sup>

图像滤波器。

### createBlurImageFilter<sup>20+</sup>

static createBlurImageFilter(sigmaX: number, sigmaY: number, tileMode: TileMode, imageFilter?: ImageFilter | null ): ImageFilter

创建具有模糊效果的图像滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| sigmaX | number | 是   | 表示沿x轴方向上高斯模糊的标准差，必须大于0，该参数为浮点数。 |
| sigmaY | number | 是   | 表示沿y轴方向上高斯模糊的标准差，必须大于0，该参数为浮点数。 |
| tileMode | [TileMode](#tilemode20)| 是   | 表示在边缘处应用的平铺模式。 |
| imageFilter | [ImageFilter](#imagefilter20) \| null | 否   | 要与当前图像滤波器叠加的输入滤波器，默认为null，表示直接将当前图像滤波器作用于原始图像。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [ImageFilter](#imagefilter20) | 返回创建的图像滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let imgFilter = drawing.ImageFilter.createBlurImageFilter(5, 10, drawing.TileMode.CLAMP);
```
### createFromImage<sup>20+</sup>

static createFromImage(pixelmap: image.PixelMap, srcRect?: common2D.Rect | null, dstRect?: common2D.Rect | null): ImageFilter

基于给定的图像创建一个图像滤波器。此接口不建议用于录制类型的画布，会影响性能。

**系统能力**：SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| pixelmap | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7)  | 是   | 图片对象。 |
| srcRect      | [common2D.Rect](js-apis-graphics-common2D.md#rect) \| null           | 否   | 可选参数，默认为空。图片要被此滤波器使用的像素区域，如果为空，则使用pixelmap全部区域。 |
| dstRect      | [common2D.Rect](js-apis-graphics-common2D.md#rect) \| null           | 否   | 可选参数，默认为空。要进行渲染的区域，如果为空，则和srcRect保持一致。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [ImageFilter](#imagefilter20) | 返回创建的图像滤波器。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { image } from '@kit.ImageKit';
import { common2D, drawing } from '@kit.ArkGraphics2D';
class DrawingRenderNode extends RenderNode {
  pixelMap: image.PixelMap | null = null;

  async draw(context : DrawContext) {
    const canvas = context.canvas;
    let srcRect: common2D.Rect = { left: 10, top: 10, right: 80, bottom: 80 };
    let dstRect: common2D.Rect = { left: 200, top: 200, right: 400, bottom: 400 };
    if (this.pixelMap != null) {
      let filter = drawing.ImageFilter.createFromImage(this.pixelMap, srcRect, dstRect);
    }
  }
}
```
### createBlendImageFilter<sup>20+</sup>

static createBlendImageFilter(mode: BlendMode, background: ImageFilter, foreground: ImageFilter): ImageFilter

按照指定的混合模式对两个滤波器进行叠加，生成一个新的滤波器。

**系统能力**：SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| mode   | [BlendMode](#blendmode)                              | 是   | 颜色混合模式。 |
| background | [ImageFilter](#imagefilter20) | 是   | 在混合模式中作为目标色的滤波器。|
| foreground | [ImageFilter](#imagefilter20) | 是   | 在混合模式中作为源色的滤波器。 |

**返回值：**

| 类型                  | 说明            |
| --------------------- | -------------- |
| [ImageFilter](#imagefilter20) | 返回创建的图像滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[图形绘制与显示错误码](../apis-arkgraphics2d/errorcode-drawing.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 25900001 | Parameter error.Possible causes: Incorrect parameter range. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let dx = 15.0;
let dy = 10.0;
let offsetFilter1 = drawing.ImageFilter.createOffsetImageFilter(dx, dy, null);
let x = 15.0;
let y = 30.0;
let offsetFilter2 = drawing.ImageFilter.createOffsetImageFilter(x, y, null);
let blendImageFilter = drawing.ImageFilter.createBlendImageFilter(drawing.BlendMode.SRC_IN, offsetFilter1, offsetFilter2);
```
### createComposeImageFilter<sup>20+</sup>

static createComposeImageFilter(cOuter: ImageFilter, cInner: ImageFilter): ImageFilter

将两个图像滤波器进行级联生成新的图像滤波器，级联时会将第一级滤波器的输出作为第二级滤波器的输入，经过第二级滤波器处理后，输出最终的滤波结果。

**系统能力**：SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                          |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| cOuter | [ImageFilter](#imagefilter20) | 是   | 在级联中，作为第二级的滤波器，处理第一级滤波器的输出。如果第二级滤波器为空，第一级滤波器不为空，最后输出第一级滤波器的结果。两级滤波器不能同时为空。 |
| cInner | [ImageFilter](#imagefilter20) | 是   | 在级联中，作为第一级的滤波器，直接处理图像的原始内容。如果第一级滤波器为空，第二级滤波器不为空，最后输出第二级滤波器的结果。两级滤波器不能同时为空。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [ImageFilter](#imagefilter20) | 返回创建的图像滤波器。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let blurSigmaX = 10.0;
let blurSigmaY = 10.0;
let blurFilter = drawing.ImageFilter.createBlurImageFilter(blurSigmaX, blurSigmaY, drawing.TileMode.CLAMP, null);
let colorMatrix:Array<number> = [
  0, 0, 0, 0, 0,
  0, 1, 0, 0, 0,
  0, 0, 1, 0, 0,
  0, 0, 0, 1, 0
];
let redRemovalFilter = drawing.ColorFilter.createMatrixColorFilter(colorMatrix);
let colorFilter = drawing.ImageFilter.createFromColorFilter(redRemovalFilter, null);
let composedImageFilter = drawing.ImageFilter.createComposeImageFilter(colorFilter, blurFilter);
```
### createFromColorFilter<sup>20+</sup>

static createFromColorFilter(colorFilter: ColorFilter, imageFilter?: ImageFilter | null): ImageFilter

创建一个将颜色滤波器应用于传入的图像滤波器的图像滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| colorFilter | [ColorFilter](#colorfilter) | 是   | 表示颜色滤波器。 |
| imageFilter | [ImageFilter](#imagefilter20) \| null | 否   | 要与当前图像滤波器叠加的输入滤波器，默认为null，表示直接将当前图像滤波器作用于原始图像。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [ImageFilter](#imagefilter20) | 返回创建的图像滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes: 1.Mandatory parameters are left unspecified; 2.Incorrect parameter types.|

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
let imgFilter = drawing.ImageFilter.createBlurImageFilter(5, 10, drawing.TileMode.CLAMP);
let clolorfilter = drawing.ColorFilter.createSRGBGammaToLinear();
let imgFilter1 = drawing.ImageFilter.createFromColorFilter(clolorfilter, imgFilter);
```
### createOffsetImageFilter<sup>20+</sup>

static createOffsetImageFilter(dx: number, dy: number, input?: ImageFilter | null): ImageFilter

创建一个偏移滤波器，将输入的滤波器按照指定向量进行平移。

**系统能力**：SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| dx | number | 是   | 水平方向的平移距离， 该参数为浮点数。 |
| dy | number | 是   | 竖直方向的平移距离， 该参数为浮点数。 |
| input | [ImageFilter](#imagefilter20) \| null | 否   | 需进行平移的滤波器。默认为空，如果为空，则将无滤波效果的绘制结果进行平移。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [ImageFilter](#imagefilter20) | 返回创建的图像滤波器。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let dx = 15.0;
let dy = 10.0;
let offsetFilter = drawing.ImageFilter.createOffsetImageFilter(dx, dy, null);
```

### createFromShaderEffect<sup>20+</sup>

static createFromShaderEffect(shader: ShaderEffect): ImageFilter

基于着色器创建一个图像滤波器。

**系统能力**：SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| shader   | [ShaderEffect](#shadereffect20) | 是   | 表示应用于图像的着色器效果。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [ImageFilter](#imagefilter20) | 返回创建的图像滤波器。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let shaderEffect = drawing.ShaderEffect.createColorShader(0xFF00FF00);
let renderEffect = drawing.ImageFilter.createFromShaderEffect(shaderEffect);
```
## TextBlobRunBuffer

描述一行文字中具有相同属性的连续字形。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称      | 类型   | 只读 | 可选 | 说明                      |
| --------- | ------ | ---- | ---- | ------------------------- |
| glyph     | number | 否   | 否   | 存储文字的索引，该参数为整数，传入浮点类型时向下取整。 |
| positionX | number | 否   | 否   | 文本的起点x轴坐标，该参数为浮点数。 |
| positionY | number | 否   | 否   | 文本的起点y轴坐标，该参数为浮点数。 |

## TextEncoding

文本的编码类型枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                   | 值   | 说明                           |
| ---------------------- | ---- | ------------------------------ |
| TEXT_ENCODING_UTF8     | 0    | 使用1个字节表示UTF-8或ASCII。  |
| TEXT_ENCODING_UTF16    | 1    | 使用2个字节表示大部分unicode。 |
| TEXT_ENCODING_UTF32    | 2    | 使用4个字节表示全部unicode。   |
| TEXT_ENCODING_GLYPH_ID | 3    | 使用2个字节表示glyph index。   |

## ClipOp<sup>20+</sup>
画布裁剪方式的枚举。


**系统能力：** SystemCapability.Graphics.Drawing

| 名称                 | 值    | 说明           | 示意图   |
| ------------------ | ---- | ---------------- | -------- |
| DIFFERENCE | 0    | 将指定区域裁剪（取差集）。 | ![DIFFERENCE](./figures/zh-ch_image_ClipOp_Difference.png) |
| INTERSECT  | 1    | 将指定区域保留（取交集）。 | ![INTERSECT](./figures/zh-ch_image_ClipOp_Intersect.png) |

> **说明：**
>
> 示意图展示了以INTERSECT方式裁剪一个矩形后，使用不同枚举值继续裁剪一个圆形的结果，绿色区域为最终的裁剪区域。

## FilterMode<sup>20+</sup>

过滤模式枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                  | 值    | 说明      |
| ------------------- | ---- | ------- |
| FILTER_MODE_NEAREST | 0    | 邻近过滤模式。 |
| FILTER_MODE_LINEAR  | 1    | 线性过滤模式。 |

## PathDirection<sup>20+</sup>

添加闭合轮廓方向的枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                  | 值    | 说明      |
| ------------------- | ---- | ------- |
| CLOCKWISE   | 0    | 顺时针方向添加闭合轮廓。 |
| COUNTER_CLOCKWISE  | 1    | 逆时针方向添加闭合轮廓。 |

## PathFillType<sup>20+</sup>

定义路径的填充类型枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                  | 值    | 说明      |
| ------------------- | ---- | ------- |
| WINDING   | 0    | 绘制区域中的任意一点，向任意方向射出一条射线，对于射线和路径的所有交点，初始计数为0，遇到每个顺时针的交点（路径从射线的左边向右穿过），计数加1，遇到每个逆时针的交点（路径从射线的右边向左穿过），计数减1，若最终的计数结果不为0，则认为这个点在路径内部，需要被涂色；若计数为0则不被涂色。 |
| EVEN_ODD  | 1    | 绘制区域中的任意一点，向任意方向射出一条射线，若这条射线和路径相交的次数是奇数，则这个点被认为在路径内部，需要被涂色；若是偶数则不被涂色。 |
| INVERSE_WINDING  | 2    | WINDING涂色规则取反。 |
| INVERSE_EVEN_ODD  | 3    | EVEN_ODD涂色规则取反。 |

> **说明：**<br>
> ![WINDING&EVEN_ODD](./figures/zh-ch_image_PathFillType_Winding_Even_Odd.png)<br>
> 如图所示圆环为路径，箭头指示路径的方向，p为区域内任意一点，蓝色线条为点p出发的射线，黑色箭头所指为对应填充规则下使用蓝色填充路径的结果。WINDING填充规则下，射线与路径的交点计数为2，不为0，点p被涂色；EVEN_ODD填充规则下，射线与路径的相交次数为2，是偶数，点p不被涂色。

## PointMode<sup>20+</sup>

绘制数组点的方式的枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                 | 值    | 说明            |
| ------------------ | ---- | ------------- |
| POINTS  | 0    | 分别绘制每个点。      |
| LINES   | 1    | 将每对点绘制为线段。    |
| POLYGON | 2    | 将点阵列绘制为开放多边形。 |

## FontEdging<sup>20+</sup>

字型边缘效果类型枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                  | 值    | 说明      |
| ------------------- | ---- | ------- |
| ALIAS | 0    | 无抗锯齿处理。 |
| ANTI_ALIAS  | 1    | 使用抗锯齿来平滑字型边缘。 |
| SUBPIXEL_ANTI_ALIAS  | 2    | 使用次像素级别的抗锯齿平滑字型边缘，可以获得更平滑的字型渲染效果。 |

## FontHinting<sup>20+</sup>

字型轮廓效果类型枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                  | 值    | 说明      |
| ------------------- | ---- | ------- |
| NONE    | 0    | 不修改字型轮廓。 |
| SLIGHT  | 1    | 最小限度修改字型轮廓以改善对比度。 |
| NORMAL  | 2    | 修改字型轮廓以提高对比度。 |
| FULL    | 3    | 修改字型轮廓以获得最大对比度。 |

## TextBlob

由一个或多个具有相同字体的字符组成的字块。

### makeFromPosText<sup>20+</sup>

static makeFromPosText(text: string, len: number, points: common2D.Point[], font: Font): TextBlob

使用文本创建TextBlob对象，TextBlob对象中每个字形的坐标由points中对应的坐标信息决定。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明                                   |
| -------- | ----------------------------- | ---- | -------------------------------------- |
| text     | string             | 是   | 绘制字形的文本内容。                   |
| len      | number             | 是   | 字形个数，由[countText](#counttext20)获取，该参数为整数。 |
| points   |[common2D.Point](js-apis-graphics-common2D.md#point20)[]     | 是   |点数组，用于指定每个字形的坐标，长度必须为len。|
| font     | [Font](#font)      | 是   | 字型对象。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [TextBlob](#textblob) | TextBlob对象。 |


**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing,common2D} from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let text : string = 'makeFromPosText';
    let font : drawing.Font = new drawing.Font();
    font.setSize(100);
    let length = font.countText(text);
    let points : common2D.Point[] = [];
    for (let i = 0; i !== length; ++i) {
      points.push({ x: i * 35, y: i * 35 });
    }
    let textblob : drawing.TextBlob =drawing.TextBlob.makeFromPosText(text, points.length, points, font);
    canvas.drawTextBlob(textblob, 100, 100);
  }
}
```

### uniqueID<sup>20+</sup>

uniqueID(): number

获取该TextBlob对象的唯一的非零标识符。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| number | 返回TextBlob对象的唯一的非零标识符。 |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let text : string = 'TextBlobUniqueId';
let font : drawing.Font = new drawing.Font();
font.setSize(100);
let textBlob = drawing.TextBlob.makeFromString(text, font, 0);
let id = textBlob.uniqueID();
console.info("uniqueID---------------" +id);
```

### makeFromString

static makeFromString(text: string, font: Font, encoding?: TextEncoding): TextBlob

将string类型的值转化成TextBlob对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明                                   |
| -------- | ----------------------------- | ---- | -------------------------------------- |
| text     | string                        | 是   | 绘制字形的文本内容。                   |
| font     | [Font](#font)                 | 是   | 字型对象。           |
| encoding | [TextEncoding](#textencoding) | 否   | 编码类型，默认值为TEXT_ENCODING_UTF8。当前只有TEXT_ENCODING_UTF8生效，其余编码类型也会被视为TEXT_ENCODING_UTF8。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [TextBlob](#textblob) | TextBlob对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    brush.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    const font = new drawing.Font();
    font.setSize(20);
    const textBlob = drawing.TextBlob.makeFromString("drawing", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    canvas.attachBrush(brush);
    canvas.drawTextBlob(textBlob, 20, 20);
    canvas.detachBrush();
  }
}
```

### makeFromRunBuffer

static makeFromRunBuffer(pos: Array\<TextBlobRunBuffer>, font: Font, bounds?: common2D.Rect): TextBlob

基于RunBuffer信息创建Textblob对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明                           |
| ------ | -------------------------------------------------- | ---- | ------------------------------ |
| pos    | Array\<[TextBlobRunBuffer](#textblobrunbuffer)>    | 是   | TextBlobRunBuffer数组。        |
| font   | [Font](#font)                                      | 是   | 字型对象。   |
| bounds | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 否   | 可选，如果不设置，则无边界框。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [TextBlob](#textblob) | TextBlob对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const font = new drawing.Font();
    font.setSize(20);
    let runBuffer : Array<drawing.TextBlobRunBuffer> = [
      { glyph: 65, positionX: 0, positionY: 0 },
      { glyph: 227, positionX: 14.9, positionY: 0 },
      { glyph: 283, positionX: 25.84, positionY: 0 },
      { glyph: 283, positionX: 30.62, positionY: 0 },
      { glyph: 299, positionX: 35.4, positionY: 0}
    ];
    const textBlob = drawing.TextBlob.makeFromRunBuffer(runBuffer, font, null);
    const brush = new drawing.Brush();
    brush.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachBrush(brush);
    canvas.drawTextBlob(textBlob, 20, 20);
    canvas.detachBrush();
  }
}
```

### bounds

bounds(): common2D.Rect

获取文字边界框的矩形区域。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                                               | 说明                   |
| -------------------------------------------------- | ---------------------- |
| [common2D.Rect](js-apis-graphics-common2D.md#rect) | 文字边界框的矩形区域。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const font = new drawing.Font();
font.setSize(20);
const textBlob = drawing.TextBlob.makeFromString("drawing", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
let bounds = textBlob.bounds();
```

## TypefaceArguments<sup>20+</sup>

提供字体属性配置的结构体。

### constructor<sup>20+</sup>

constructor()

字体属性的构造函数。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
let typeFaceArgument = new drawing.TypefaceArguments();
```

### addVariation<sup>20+</sup>

addVariation(axis: string, value: number)

给字体属性设置字重值。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**
| 参数名         | 类型                                       | 必填   | 说明             |
| ----------- | ---------------------------------------- | ---- | -------------------   |
| axis  | string           | 是   | 字体属性对象可变维度字重的标签'wght'。具体是否支持的该标签取决于加载的字体文件。请打开对应的字体文件具体查看支持的属性。   |
| value | number           | 是   | 字体属性对象可变维度字重的标签'wght'对应的属性值，需要在字体文件支持的范围内，否则不会生效。如果属性值小于支持的最小值，则默认和最小值一致。如果属性值大于支持的最大值，则默认和最大值效果一致。请打开对应的字体文件具体查看支持的属性值。    |

**错误码：**

以下错误码的详细介绍请参见[图形绘制与显示错误码](../apis-arkgraphics2d/errorcode-drawing.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 25900001 | Parameter error.Possible causes: Incorrect parameter range. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let typeFaceArgument = new drawing.TypefaceArguments();
typeFaceArgument.addVariation('wght', 10);
```

## Typeface

字体，如宋体、楷体等。

### getFamilyName

getFamilyName(): string

获取字体的族名，即一套字体设计的名称。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| string | 返回字体的族名。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const font = new drawing.Font();
let typeface = font.getTypeface();
let familyName = typeface.getFamilyName();
```

### makeFromCurrent<sup>20+</sup>

makeFromCurrent(typefaceArguments: TypefaceArguments): Typeface

基于当前字体结合字体属性构造新的字体对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**
| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| typefaceArguments | [TypefaceArguments](#typefacearguments20)           | 是   | 字体属性。 |

**返回值：**

| 类型   | 说明                  |
| ------ | -------------------- |
| [Typeface](#typeface) | 返回字体对象（异常情况下会返回空指针）。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class TextRenderNode extends RenderNode {
  async draw(context: DrawContext) {
    const canvas = context.canvas;
    let typeArguments = new drawing.TypefaceArguments();
    typeArguments.addVariation("wght", 100);
    const myTypeFace = drawing.Typeface.makeFromFile("/system/fonts/HarmonyOS_Sans_SC.ttf");
    const typeFace1 = myTypeFace.makeFromCurrent(typeArguments);
    let font = new drawing.Font();
    font.setTypeface(typeFace1);
    const textBlob = drawing.TextBlob.makeFromString("Hello World", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    canvas.drawTextBlob(textBlob, 60, 100);
  }
}
```

### makeFromFile<sup>20+</sup>

static makeFromFile(filePath: string): Typeface

从指定字体文件构造字体。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| filePath | string           | 是   | 表示字体资源存放的路径。 |

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| [Typeface](#typeface) | 返回Typeface对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class TextRenderNode extends RenderNode {
  async draw(context: DrawContext) {
    const canvas = context.canvas;
    let font = new drawing.Font();
    let str = "/system/fonts/HarmonyOS_Sans_Italic.ttf";
    const mytypeface = drawing.Typeface.makeFromFile(str);
    font.setTypeface(mytypeface);
    const textBlob = drawing.TextBlob.makeFromString("Hello World", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    canvas.drawTextBlob(textBlob, 60, 100);
  }
}
```

```
### makeFromFileWithArguments<sup>20+</sup>

static makeFromFileWithArguments(filePath: string, typefaceArguments: TypefaceArguments): Typeface

根据字体文件路径和字体属性构造新的字体。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| filePath | string           | 是   | 表示字体资源存放的路径。 |
| typefaceArguments | [TypefaceArguments](#typefacearguments20) | 是  | 表示字体属性。 |

**返回值：**

| 类型   | 说明                  |
| ------ | -------------------- |
| [Typeface](#typeface) | 返回字体对象（异常情况下会返回空指针）。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class TextRenderNode extends RenderNode {
  async draw(context: DrawContext) {
    const canvas = context.canvas;
    let font = new drawing.Font();
    let str = "/system/fonts/HarmonyOS_Sans_Italic.ttf";
    let typeFaceArgument = new drawing.TypefaceArguments();
    const myTypeFace = drawing.Typeface.makeFromFileWithArguments(str, typeFaceArgument);
    font.setTypeface(myTypeFace);
    const textBlob = drawing.TextBlob.makeFromString("Hello World", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    canvas.drawTextBlob(textBlob, 60, 100);
  }
}
```

## Font

描述字型绘制时所使用的属性，如大小、字体等。

### isSubpixel<sup>20+</sup>

isSubpixel(): boolean

获取字型是否使用次像素渲染。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| boolean | 返回字型是否使用次像素渲染的结果，true表示使用，false表示不使用。 |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
font.enableSubpixel(true)
console.info("values=" + font.isSubpixel());
```

### isLinearMetrics<sup>20+</sup>

isLinearMetrics(): boolean

获取字型是否可以线性缩放。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| boolean | 返回字型是否可线性缩放的结果，true表示可线性缩放，false表示不可线性缩放。 |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
font.enableLinearMetrics(true)
console.info("values=" + font.isLinearMetrics());
```

### getSkewX<sup>20+</sup>

getSkewX(): number

获取字型在x轴方向上的倾斜度。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| number | 返回字型在x轴方向上的倾斜度。 |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
font.setSkewX(-1)
console.info("values=" + font.getSkewX());
```

### isEmbolden<sup>20+</sup>

isEmbolden(): boolean

获取字型是否设置了粗体效果。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| boolean  | 返回字型是否设置粗体效果的结果，true表示设置了粗体效果，false表示未设置粗体效果。 |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
font.enableEmbolden(true);
console.info("values=" + font.isEmbolden());
```

### getScaleX<sup>20+</sup>

getScaleX(): number

获取字型在x轴方向上的缩放比例。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| number  | 返回字型在x轴方向上的缩放比例。 |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
font.setScaleX(2);
console.info("values=" + font.getScaleX());
```

### getHinting<sup>20+</sup>

getHinting(): FontHinting

获取字型轮廓效果。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| [FontHinting](#fonthinting20)  | 返回字型轮廓效果。 |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
console.info("values=" + font.getHinting());
```

### getEdging<sup>20+</sup>

getEdging(): FontEdging

获取字型边缘效果。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| ------ | -------------------- |
| [FontEdging](#fontedging20)  | 返回字型边缘效果。 |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
console.info("values=" + font.getEdging());
```

### enableSubpixel

enableSubpixel(isSubpixel: boolean): void

使能字型亚像素级别的文字绘制，显示效果平滑。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型    | 必填 | 说明                                                         |
| ---------- | ------- | ---- | ------------------------------------------------------------ |
| isSubpixel | boolean | 是   | 表示是否使能字型亚像素级别的文字绘制。true表示使能，false表示不使能。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.enableSubpixel(true);
```

### enableEmbolden

enableEmbolden(isEmbolden: boolean): void

使能字型粗体。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型    | 必填 | 说明                                                  |
| ---------- | ------- | ---- | ----------------------------------------------------- |
| isEmbolden | boolean | 是   | 表示是否使能字型粗体。true表示使能，false表示不使能。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.enableEmbolden(true);
```

### enableLinearMetrics

enableLinearMetrics(isLinearMetrics: boolean): void

使能字型的线性缩放。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| isLinearMetrics | boolean | 是   | 表示是否使能字型的线性缩放。true表示使能，false表示不使能。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.enableLinearMetrics(true);
```

### setSize

setSize(textSize: number): void

设置字型大小。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型   | 必填 | 说明             |
| -------- | ------ | ---- | ---------------- |
| textSize | number | 是   | 字型大小，该参数为浮点数，为负数时字型大小会被置为0。字型大小为0时，绘制的文字不会显示。|

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.setSize(5);
```

### getSize

getSize(): number

获取字型大小。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| number | 字型大小，浮点数。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.setSize(5);
let fontSize = font.getSize();
```

### setTypeface

setTypeface(typeface: Typeface): void

为字型设置字体样式（包括字体名称、粗细、斜体等属性）。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                  | 必填 | 说明   |
| -------- | --------------------- | ---- | ------ |
| typeface | [Typeface](#typeface) | 是   | 字体样式，包括字体名称、粗细、斜体等属性。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.setTypeface(new drawing.Typeface());
```

### getTypeface

getTypeface(): Typeface

获取字体。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                  | 说明   |
| --------------------- | ------ |
| [Typeface](#typeface) | 字体。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
let typeface = font.getTypeface();
```

### getMetrics

getMetrics(): FontMetrics

获取与字体关联的FontMetrics属性。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明              |
| --------------------------- | ----------------- |
| [FontMetrics](#fontmetrics) | FontMetrics属性。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
let metrics = font.getMetrics();
```

### measureText

measureText(text: string, encoding: TextEncoding): number

测量文本的宽度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明       |
| -------- | ----------------------------- | ---- | ---------- |
| text     | string                        | 是   | 文本内容。 |
| encoding | [TextEncoding](#textencoding) | 是   | 编码格式。 |

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| number | 文本的宽度，浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.measureText("drawing", drawing.TextEncoding.TEXT_ENCODING_UTF8);
```

### measureSingleCharacter<sup>20+</sup>

measureSingleCharacter(text: string): number

测量单个字符的宽度。当前字型中的字体不支持待测量字符时，退化到使用系统字体测量字符宽度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数**

| 参数名 | 类型                | 必填 | 说明        |
| ------ | ------------------- | ---- | ----------- |
| text   | string | 是   | 待测量的单个字符，字符串的长度必须为1。  |

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| number | 字符的宽度，浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const font = new drawing.Font();
    font.setSize(20);
    let width = font.measureSingleCharacter("你");
  }
}
```

### setScaleX<sup>20+</sup>

setScaleX(scaleX: number): void

设置字型对象在x轴上的缩放比例。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明       |
| -------- | ----------------------------- | ---- | ---------- |
| scaleX     | number                      | 是   | 文本在x轴上的缩放比例，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    let font = new drawing.Font();
    font.setSize(100);
    font.setScaleX(2);
    const textBlob = drawing.TextBlob.makeFromString("hello", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    canvas.drawTextBlob(textBlob, 200, 200);
  }
}
```

### setSkewX<sup>20+</sup>

setSkewX(skewX: number): void

设置字型对象在x轴上的倾斜比例。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明       |
| -------- | ----------------------------- | ---- | ---------- |
| skewX     | number                      | 是   | 文本在x轴上的倾斜比例，正数表示往左边倾斜，负数表示往右边倾斜，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    canvas.attachPen(pen);
    let font = new drawing.Font();
    font.setSize(100);
    font.setSkewX(1);
    const textBlob = drawing.TextBlob.makeFromString("hello", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    canvas.drawTextBlob(textBlob, 200, 200);
  }
}
```

### setEdging<sup>20+</sup>

setEdging(edging: FontEdging): void

设置字型边缘效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明       |
| -------- | ----------------------------- | ---- | ---------- |
| edging | [FontEdging](#fontedging20) | 是   | 字型边缘效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.setEdging(drawing.FontEdging.SUBPIXEL_ANTI_ALIAS);
```

### setHinting<sup>20+</sup>

setHinting(hinting: FontHinting): void

设置字型轮廓效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明       |
| -------- | ----------------------------- | ---- | ---------- |
| hinting | [FontHinting](#fonthinting20) | 是   | 字型轮廓效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
font.setHinting(drawing.FontHinting.FULL);
```

### countText<sup>20+</sup>

countText(text: string): number

获取文本所表示的字符数量。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明       |
| -------- | ----------------------------- | ---- | ---------- |
| text     | string                        | 是   | 文本内容。 |

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| number | 返回文本所表示的字符数量，整数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font = new drawing.Font();
let resultNumber: number = font.countText('ABCDE');
console.info("count text number: " + resultNumber);
```

### setBaselineSnap<sup>20+</sup>

setBaselineSnap(isBaselineSnap: boolean): void

当前画布矩阵轴对齐时，设置字型基线是否与像素对齐。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                       |
| --------------- | ------- | ---- | ---------------------------------------- |
| isBaselineSnap | boolean | 是   | 指示字型基线是否和像素对齐，true表示对齐，false表示不对齐。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setBaselineSnap(true);
console.info("drawing font isBaselineSnap: " + font.isBaselineSnap());
```

### isBaselineSnap()<sup>20+</sup>

isBaselineSnap(): boolean

当前画布矩阵轴对齐时，获取字型基线是否与像素对齐的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| boolean | 返回字型基线是否与像素对齐，true为对齐，false为没有对齐。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setTypeface(new drawing.Typeface());
font.setBaselineSnap(true);
console.info("drawing font isBaselineSnap: " + font.isBaselineSnap());
```

### setEmbeddedBitmaps<sup>20+</sup>

setEmbeddedBitmaps(isEmbeddedBitmaps: boolean): void

设置字型是否转换成位图处理。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型   | 必填 | 说明             |
| -------- | ------ | ---- | ---------------- |
| isEmbeddedBitmaps | boolean | 是   | 设置字型是否转换成位图处理，true表示转换成位图处理，false表示不转换成位图处理。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setTypeface(new drawing.Typeface());
font.setEmbeddedBitmaps(false);
console.info("draw isEmbeddedBitmaps: " + font.isEmbeddedBitmaps());
```

### isEmbeddedBitmaps()<sup>20+</sup>

isEmbeddedBitmaps(): boolean

获取字型是否转换成位图处理的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| boolean | 返回字型是否转换成位图处理结果，true表示转换成位图处理，false表示不转换成位图处理。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setTypeface(new drawing.Typeface());
font.setEmbeddedBitmaps(true);
console.info("draw isEmbeddedBitmaps: " + font.isEmbeddedBitmaps());
```

### setForceAutoHinting<sup>20+</sup>

setForceAutoHinting(isForceAutoHinting: boolean): void

设置是否自动调整字型轮廓。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型   | 必填 | 说明             |
| -------- | ------ | ---- | ---------------- |
| isForceAutoHinting | boolean | 是   | 是否自动调整字型轮廓，true为自动调整，false为不自动调整。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setTypeface(new drawing.Typeface());
font.setForceAutoHinting(false);
console.info("drawing isForceAutoHinting:  " + font.isForceAutoHinting());
```

### isForceAutoHinting<sup>20+</sup>

isForceAutoHinting(): boolean

获取字型轮廓是否自动调整的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| boolean | 返回字型轮廓是否自动调整，true为自动调整，false为不自动调整。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setTypeface(new drawing.Typeface());
font.setForceAutoHinting(false);
console.info("drawing isForceAutoHinting:  " + font.isForceAutoHinting());
```

### getWidths<sup>20+</sup>

getWidths(glyphs: Array\<number>): Array\<number>

获取字形数组中每个字形对应的宽度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                  | 必填 | 说明   |
| -------- | --------------------- | ---- | ------ |
| glyphs | Array\<number> | 是   | 字形索引数组，可由[textToGlyphs](#texttoglyphs20)生成。 |

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| Array\<number> | 返回字形宽度数组。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
let text: string = 'hello world';
let glyphs: number[] = font.textToGlyphs(text);
let fontWidths: Array<number> = font.getWidths(glyphs);
for (let index = 0; index < fontWidths.length; index++) {
  console.info("get fontWidths[", index, "]:", fontWidths[index]);
}
```

### textToGlyphs<sup>20+</sup>

textToGlyphs(text: string, glyphCount?: number): Array\<number>

将文本转换为字形索引。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                          | 必填 | 说明       |
| -------- | ----------------------------- | ---- | ---------- |
| text     | string                        | 是   | 文本字符串。 |
| glyphCount | number | 否   | 文本表示的字符数量，必须与[countText](#counttext20)获取的值相等，默认为text的字符数量，该参数为整数。 |

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| Array\<number> | 返回转换得到的字形索引数组。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
let text : string = 'hello world';
let glyphs : number[] = font.textToGlyphs(text);
console.info("drawing text toglyphs OnTestFunction num =  " + glyphs.length );
```

### getBounds<sup>20+</sup>

getBounds(glyphs: Array\<number>): Array\<common2D.Rect>

获取字形数组中每个字形的边界矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                  | 必填 | 说明   |
| -------- | --------------------- | ---- | ------ |
| glyphs | Array\<number> | 是   | 字形索引数组，可由[textToGlyphs](#texttoglyphs20)生成。 |

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| Array\<[common2D.Rect](js-apis-graphics-common2D.md#rect)> | 返回字形边界矩形数组。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let font: drawing.Font = new drawing.Font();
let text: string = 'hello world';
let glyphs: number[] = font.textToGlyphs(text);
let fontBounds: Array<common2D.Rect> = font.getBounds(glyphs);
for (let index = 0; index < fontBounds.length; index++) {
  console.info("get fontWidths[", index, "] left:", fontBounds[index].left, " top:", fontBounds[index].top,
    " right:", fontBounds[index].right, " bottom:", fontBounds[index].bottom);
}
```

### getTextPath<sup>20+</sup>

getTextPath(text: string, byteLength: number, x: number, y: number): Path;

获取文字的轮廓路径。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名    | 类型                                               | 必填 | 说明                    |
| ------   | ------------------------------------------------   | ---- | ---------------------- |
|   text   |    string                                          | 是   | 表示存储UTF-8 文本编码的字符。|
|byteLength|    number                                          | 是   | 表示要获取对应文本路径的字节长度，按传入的字节长度和实际的文本字节大小之间的最小值来获取对应的文本路径。|
|    x     |    number                                          | 是   | 表示文本在绘图区域内以原点为起始位置的X坐标。|
|    y     |    number                                          | 是   | 表示文本在绘图区域内以原点为起始位置的Y坐标。|

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| [Path](#path) | 返回获取到的文本的路径轮廓。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
import { buffer } from '@kit.ArkTS';
import { RenderNode } from '@kit.ArkUI';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let font = new drawing.Font();
    font.setSize(50)
    let myString: string = "你好, HarmonyOS";
    let length = buffer.from(myString).length;
    let path = font.getTextPath(myString, length, 0, 100)
    canvas.drawPath(path)
  }
}
```

### createPathForGlyph<sup>20+</sup>

createPathForGlyph(index: number): Path

获取指定字形的路径轮廓。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                  | 必填 | 说明   |
| -------- | --------------------- | ---- | ------ |
| index | number | 是   | 字形索引。 |

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| [Path](#path) | 返回指定字形的路径轮廓。 |

**示例：**

```ts
import { FrameNode, NodeController, RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let font = new drawing.Font();
    font.setSize(50)
    let text: string = '你好';
    let glyphs: number[] = font.textToGlyphs(text);
    for (let index = 0; index < glyphs.length; index++) {
      let path: drawing.Path = font.createPathForGlyph(glyphs[index])
      canvas.drawPath(path)
    }
  }
}
```

### setThemeFontFollowed<sup>20+</sup>

setThemeFontFollowed(followed: boolean): void

设置字型中的字体是否跟随主题字体。设置跟随主题字体后，若系统启用主题字体并且字型未被设置字体，字型会使用该主题字体。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型   | 必填 | 说明             |
| -------- | ------ | ---- | ---------------- |
| followed | boolean | 是   | 字型中的字体是否跟随主题字体，true表示跟随主题字体，false表示不跟随主题字体。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setThemeFontFollowed(true);
console.info("font is theme font followed: " + font.isThemeFontFollowed());
```

### isThemeFontFollowed()<sup>20+</sup>

isThemeFontFollowed(): boolean

获取字型中的字体是否跟随主题字体。默认不跟随。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明             |
| ------ | ---------------- |
| boolean | 返回字型中的字体是否跟随主题字体的结果，true表示跟随主题字体，false表示不跟随主题字体。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let font : drawing.Font = new drawing.Font();
font.setThemeFontFollowed(true);
console.info("font is theme font followed: " + font.isThemeFontFollowed());
```

## FontMetricsFlags<sup>20+</sup>

字体度量标志枚举，指示字体度量中的各字段数据是否有效。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                          | 值        | 说明                           |
| ----------------------------- | --------- | ------------------------------ |
| UNDERLINE_THICKNESS_VALID     | 1 << 0    | 表示[FontMetrics](#fontmetrics)结构中的underlineThickness（下划线厚度）字有效。    |
| UNDERLINE_POSITION_VALID      | 1 << 1    | 表示[FontMetrics](#fontmetrics)结构中的underlinePosition（下划线位置）字段有效。  |
| STRIKETHROUGH_THICKNESS_VALID | 1 << 2    | 表示[FontMetrics](#fontmetrics)结构中strikethroughThickness（删除线厚度）是有效的。|
| STRIKETHROUGH_POSITION_VALID  | 1 << 3    | 表示[FontMetrics](#fontmetrics)结构中strikethroughPosition（删除线位置）字段有效。  |
| BOUNDS_INVALID                | 1 << 4    | 表示[FontMetrics](#fontmetrics)结构中的边界度量值（如top、bottom、xMin、xMax）无效。  |

## FontMetrics

描述字形大小和布局的属性信息，同一种字体中的字符属性大致相同。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称    | 类型   | 只读 | 可选 | 说明                                                         |
| ------- | ------ | ---- | ---- | ------------------------------------------------------------ |
| flags<sup>20+</sup>   | [FontMetricsFlags](#fontmetricsflags20) | 否   | 是   | 表明哪些字体度量标志有效。        |
| top     | number | 否   | 否   | 文字最高处到基线之间的最大距离，浮点数。                         |
| ascent  | number | 否   | 否   | 文字最高处到基线之间的距离，浮点数。                             |
| descent | number | 否   | 否   | 基线到文字最低处之间的距离，浮点数。                             |
| bottom  | number | 否   | 否   | 基线到文字最低处之间的最大距离，浮点数。                         |
| leading | number | 否   | 否   | 行间距，从上一行文字descent到下一行文字ascent之间的距离，浮点数。 |
| avgCharWidth<sup>20+</sup> | number | 否   | 是   | 平均字符宽度。                             |
| maxCharWidth<sup>20+</sup> | number | 否   | 是   | 最大字符宽度。                             |
| xMin<sup>20+</sup> | number | 否    | 是   | 字体中任意字形边界框最左边沿到原点的水平距离，这个值往往小于零，意味着字形在水平方向上的最小边界。                |
| xMax<sup>20+</sup> | number | 否   | 是   | 字体中任意字形边界框最右边沿到原点的水平距离，此值多为正数，指示了字形在水平方向上的最大延伸范围。        |
| xHeight<sup>20+</sup> | number | 否   | 是   | 小写字母x的高度，通常为负值。                     |
| capHeight<sup>20+</sup> | number | 否   | 是   | 大写字母的高度，通常为负值。                      |
| underlineThickness<sup>20+</sup> | number | 否   | 是   | 下划线的厚度。                                          |
| underlinePosition<sup>20+</sup>  | number | 否   | 是   | 文本基线到下划线顶部的垂直距离，通常是正数。             |
| strikethroughThickness<sup>20+</sup>  | number | 否   | 是   | 文本删除线的厚度，即贯穿文本字符的水平线的宽度。    |
| strikethroughPosition<sup>20+</sup>  | number | 否   | 是   | 文本基线到底部删除线的垂直距离，通常为负值。         |

## ColorFilter

颜色滤波器。

### createBlendModeColorFilter

createBlendModeColorFilter(color: common2D.Color, mode: BlendMode) : ColorFilter

创建指定的颜色和混合模式的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| color  | [common2D.Color](js-apis-graphics-common2D.md#color) | 是   | ARGB格式的颜色，每个颜色通道的值是0到255之间的整数。 |
| mode   | [BlendMode](#blendmode)                              | 是   | 颜色的混合模式。 |

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
let colorFilter = drawing.ColorFilter.createBlendModeColorFilter(color, drawing.BlendMode.SRC);
```

### createBlendModeColorFilter<sup>20+</sup>

static createBlendModeColorFilter(color: common2D.Color | number, mode: BlendMode) : ColorFilter

使用指定的颜色和混合模式创建颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| color  | [common2D.Color](js-apis-graphics-common2D.md#color) \| number | 是   | 颜色，可以用16进制ARGB格式的无符号整数表示。 |
| mode   | [BlendMode](#blendmode)                              | 是   | 颜色的混合模式。 |

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let colorFilter = drawing.ColorFilter.createBlendModeColorFilter(0xffff0000, drawing.BlendMode.SRC);
```

### createComposeColorFilter

createComposeColorFilter(outer: ColorFilter, inner: ColorFilter) : ColorFilter

创建一个先应用inner进行滤波，再应用outer进行滤波的组合颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                        | 必填 | 说明                             |
| ------ | --------------------------- | ---- | -------------------------------- |
| outer  | [ColorFilter](#colorfilter) | 是   | 组合滤波器中后生效的颜色滤波器。 |
| inner  | [ColorFilter](#colorfilter) | 是   | 组合滤波器中先生效的颜色滤波器。 |

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
let colorFilter1 = drawing.ColorFilter.createBlendModeColorFilter(color, drawing.BlendMode.SRC);
let colorFilter2 = drawing.ColorFilter.createBlendModeColorFilter(color, drawing.BlendMode.DST);
let colorFilter = drawing.ColorFilter.createComposeColorFilter(colorFilter1, colorFilter2);
```

### createLinearToSRGBGamma

createLinearToSRGBGamma() : ColorFilter

创建一个从线性颜色空间转换到SRGB颜色空间的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let colorFilter = drawing.ColorFilter.createLinearToSRGBGamma();
```

### createSRGBGammaToLinear

createSRGBGammaToLinear() : ColorFilter

创建一个从SRGB颜色空间转换到线性颜色空间的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let colorFilter = drawing.ColorFilter.createSRGBGammaToLinear();
```

### createLumaColorFilter

createLumaColorFilter() : ColorFilter

创建一个颜色滤波器将其输入的亮度值乘以透明度通道，并将红色、绿色和蓝色通道设置为零。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let colorFilter = drawing.ColorFilter.createLumaColorFilter();
```

### createMatrixColorFilter<sup>20+</sup>

static createMatrixColorFilter(matrix: Array\<number>): ColorFilter

创建颜色滤波器，通过4x5颜色矩阵变换颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| matrix | Array\<number> | 是   | 长度为20的数组，表示用于颜色变换的4*5矩阵。                 |

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix: Array<number> = [
  1, 0, 0, 0, 0,
  0, 1, 0, 0, 0,
  0, 0, 100, 0, 0,
  0, 0, 0, 1, 0
];
let colorFilter = drawing.ColorFilter.createMatrixColorFilter(matrix);
```
### createLightingColorFilter<sup>20+</sup>

static createLightingColorFilter(mutColor: common2D.Color | number, addColor: common2D.Color | number): ColorFilter

创建一个光照颜色滤波器，此滤波器会将RGB通道的颜色值乘以一种颜色值并加上另一种颜色值，计算结果会被限制在0到255范围内。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| mutColor | [common2D.Color](js-apis-graphics-common2D.md#color) \| number | 是   | 用来进行乘法运算的颜色，ARGB格式的颜色，每个颜色通道是0到255之间的整数。为number类型时必须是16进制ARGB格式的无符号整数。 |
| addColor | [common2D.Color](js-apis-graphics-common2D.md#color) \| number | 是   | 用来进行加法运算的颜色，ARGB格式的颜色，每个颜色通道是0到255之间的整数。为number类型时必须是16进制ARGB格式的无符号整数。 |

**返回值：**

| 类型                        | 说明                |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回一个颜色滤波器。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';
let mulColor : common2D.Color = { alpha: 0, red: 0, green: 0, blue: 20 };
let addColor : common2D.Color = { alpha: 0, red: 0, green: 0, blue: 125 };
let colorFilter = drawing.ColorFilter.createLightingColorFilter(mulColor, addColor);
```
## JoinStyle<sup>20+</sup>

定义线条转角样式的枚举，即画笔在绘制折线段时，在折线转角处的样式。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称        | 值   | 说明                                                         | 示意图   |
| ----------- | ---- | ----------------------------------------------------------- | -------- |
| MITER_JOIN | 0    | 转角类型为尖角，如果折线角度比较小，则尖角会很长，需要使用限制值（miter limit）进行限制。 | ![MITER_JOIN](./figures/zh-ch_image_JoinStyle_Miter_Join.png) |
| ROUND_JOIN | 1    | 转角类型为圆头。 | ![ROUND_JOIN](./figures/zh-ch_image_JoinStyle_Round_Join.png) |
| BEVEL_JOIN | 2    | 转角类型为平头。 | ![BEVEL_JOIN](./figures/zh-ch_image_JoinStyle_Bevel_Join.png) |

## CapStyle<sup>20+</sup>

定义线帽样式的枚举，即画笔在绘制线段时，在线段头尾端点的样式。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称        | 值   | 说明                                                         | 示意图   |
| ---------- | ---- | ----------------------------------------------------------- | -------- |
| FLAT_CAP   | 0    | 没有线帽样式，线条头尾端点处横切。 | ![FLAT_CAP](./figures/zh-ch_image_CapStyle_Flat_Cap.jpg) |
| SQUARE_CAP | 1    | 线帽的样式为方框，线条的头尾端点处多出一个方框，方框宽度和线段一样宽，高度是线段宽度的一半。 | ![SQUARE_CAP](./figures/zh-ch_image_CapStyle_Square_Cap.jpg) |
| ROUND_CAP  | 2    | 线帽的样式为圆弧，线条的头尾端点处多出一个半圆弧，半圆的直径与线段宽度一致。 | ![ROUND_CAP](./figures/zh-ch_image_CapStyle_Round_Cap.jpg) |

## BlurType<sup>20+</sup>

定义蒙版滤镜模糊中操作类型的枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称   | 值 | 说明               | 示意图   |
| ------ | - | ------------------ | -------- |
| NORMAL | 0 | 全面模糊，外圈边缘和内部实体一起模糊。 | ![NORMAL](./figures/zh-ch_image_BlueType_Normal.png) |
| SOLID  | 1 | 内部实体不变，只模糊外圈边缘部分。 | ![SOLID](./figures/zh-ch_image_BlueType_Solid.png) |
| OUTER  | 2 | 只有外圈边缘模糊，内部实体完全透明。 | ![OUTER](./figures/zh-ch_image_BlueType_Outer.png) |
| INNER  | 3 | 只有内部实体模糊，外圈边缘清晰。 | ![INNER](./figures/zh-ch_image_BlueType_Inner.png) |

## SamplingOptions<sup>20+</sup>

采样选项对象。

### constructor<sup>20+</sup>

constructor()

构造一个新的采样选项对象，[FilterMode](#filtermode20)的默认值为FILTER_MODE_NEAREST。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    let samplingOptions = new drawing.SamplingOptions();
  }
}
```

### constructor<sup>20+</sup>

constructor(filterMode: FilterMode)

构造一个新的采样选项对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型                   | 必填 | 说明                                 |
| ---------- | --------------------- | ---- | ----------------------------------- |
| filterMode | [FilterMode](#filtermode20)    | 是   | 过滤模式。                    |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let samplingOptions = new drawing.SamplingOptions(drawing.FilterMode.FILTER_MODE_NEAREST);
  }
}
```

## Lattice<sup>20+</sup>

矩形网格对象。该对象用于将图片按照矩形网格进行划分。

### createImageLattice<sup>20+</sup>

static createImageLattice(xDivs: Array\<number>, yDivs: Array\<number>, fXCount: number, fYCount: number, fBounds?: common2D.Rect | null, fRectTypes?: Array\<RectType> | null, fColors?: Array\<common2D.Color> | null): Lattice

创建矩形网格对象。将图像划分为矩形网格，同时处于偶数列和偶数行上的网格是固定的，如果目标网格足够大，则这些固定网格以其原始大小进行绘制。如果目标网格太小，无法容纳这些固定网格，则所有固定网格都会按比例缩小以适应目标网格。其余网格将进行缩放，来适应剩余的空间。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名       | 类型                                                                | 必填 | 说明                                                                               |
| ------------ | ------------------------------------------------------------------ | ---- | --------------------------------------------------------------------------------- |
| xDivs        | Array\<number>                                                     | 是   | 用于划分图像的X坐标值数组。该参数为整数。                                             |
| yDivs        | Array\<number>                                                     | 是   | 用于划分图像的Y坐标值数组。该参数为整数。                                             |
| fXCount      | number                                                             | 是   | X坐标值数组的大小。基于功能和性能的考虑，取值范围为[0, 5]。                            |
| fYCount      | number                                                             | 是   | Y坐标值数组的大小。基于功能和性能的考虑，取值范围为[0, 5]。                            |
| fBounds      | [common2D.Rect](js-apis-graphics-common2D.md#rect)\|null           | 否   | 可选，要绘制的原始边界矩形，矩形参数须为整数，默认为原始图像矩形大小（若矩形参数为小数，会直接舍弃小数部分，转为整数）。 |
| fRectTypes   | Array\<[RectType](#recttype20)>\|null                              | 否   | 可选，填充网格类型的数组，默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |
| fColors      | Array\<[common2D.Color](js-apis-graphics-common2D.md#color)>\|null | 否   | 可选，填充网格的颜色数组，默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |

**返回值：**

| 类型                       | 说明                                |
| ------------------------- | ----------------------------------- |
| [Lattice](#lattice20)     | 返回创建的矩形网格对象。              |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    let xDivs : Array<number> = [1, 2, 4];
    let yDivs : Array<number> = [1, 2, 4];
    let lattice = drawing.Lattice.createImageLattice(xDivs, yDivs, 3, 3); // 划分(3+1)*(3+1)的网格，下图蓝色填充矩形为固定网格
  }
}
```
![zh-ch_Lattice.png](figures/zh-ch_Lattice.png)

### createImageLattice<sup>20+</sup>

static createImageLattice(xDivs: Array\<number>, yDivs: Array\<number>, fXCount: number, fYCount: number, fBounds?: common2D.Rect | null, fRectTypes?: Array\<RectType> | null, fColors?: Array\<number> | null): Lattice

创建矩形网格对象。将图像划分为矩形网格，同时处于偶数列和偶数行上的网格是固定的，如果目标网格足够大，则这些固定网格以其原始大小进行绘制。如果目标网格太小，无法容纳这些固定网格，则所有固定网格都会按比例缩小以适应目标网格。其余网格将进行缩放，来适应剩余的空间。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名       | 类型                                                                | 必填 | 说明                                                                               |
| ------------ | ------------------------------------------------------------------ | ---- | --------------------------------------------------------------------------------- |
| xDivs        | Array\<number>                                                     | 是   | 用于划分图像的X坐标值数组。该参数为整数。                                             |
| yDivs        | Array\<number>                                                     | 是   | 用于划分图像的Y坐标值数组。该参数为整数。                                             |
| fXCount      | number                                                             | 是   | X坐标值数组的大小。基于功能和性能的考虑，取值范围为[0, 5]。                            |
| fYCount      | number                                                             | 是   | Y坐标值数组的大小。基于功能和性能的考虑，取值范围为[0, 5]。                            |
| fBounds      | [common2D.Rect](js-apis-graphics-common2D.md#rect)\|null           | 否   | 可选，要绘制的原始边界矩形，矩形参数须为整数，默认为原始图像矩形大小（若矩形参数为小数，会直接舍弃小数部分，转为整数）。 |
| fRectTypes   | Array\<[RectType](#recttype20)>\|null                              | 否   | 可选，填充网格类型的数组，默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |
| fColors      | Array\<number>\|null | 否   | 可选，填充网格的颜色数组，颜色用16进制ARGB格式的32位无符号整数表示，参数默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |

**返回值：**

| 类型                       | 说明                                |
| ------------------------- | ----------------------------------- |
| [Lattice](#lattice20)     | 返回创建的矩形网格对象。              |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    let xDivs : Array<number> = [1, 2, 4];
    let yDivs : Array<number> = [1, 2, 4];
    let colorArray :Array<number>=[0xffffffff,0x44444444,0x99999999,0xffffffff,0x44444444,0x99999999,0xffffffff,0x44444444,0x99999999,0x44444444,0x99999999,0xffffffff,0x44444444,0x99999999,0xffffffff,0x44444444];
    let lattice = drawing.Lattice.createImageLattice(xDivs, yDivs, 3, 3,null,null,colorArray);
  }
}
```

## RectType<sup>20+</sup>

定义填充网格的矩形类型的枚举。仅在[Lattice](#lattice20)中使用。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称         | 值   | 说明                                                             |
| ------------ | ---- | --------------------------------------------------------------- |
| DEFAULT      | 0    | 将图像绘制到矩形网格中。                                          |
| TRANSPARENT  | 1    | 将矩形网格设置为透明的。                                          |
| FIXEDCOLOR   | 2    | 将[Lattice](#lattice20)中fColors数组的颜色绘制到矩形网格中。       |

## MaskFilter<sup>20+</sup>

蒙版滤镜对象。

### createBlurMaskFilter<sup>20+</sup>

static createBlurMaskFilter(blurType: BlurType, sigma: number): MaskFilter

创建具有模糊效果的蒙版滤镜。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型                   | 必填 | 说明                                 |
| ---------- | --------------------- | ---- | ----------------------------------- |
| blurType   | [BlurType](#blurtype20) | 是   | 模糊类型。                           |
| sigma      | number                | 是   | 高斯模糊的标准偏差，必须为大于0的浮点数。 |

**返回值：**

| 类型                      | 说明                |
| ------------------------- | ------------------ |
| [MaskFilter](#maskfilter20) | 返回创建的蒙版滤镜对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let maskFilter = drawing.MaskFilter.createBlurMaskFilter(drawing.BlurType.OUTER, 10);
  }
}
```

## PathDashStyle<sup>20+</sup>

路径效果的绘制样式枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称   | 值 | 说明               |
| ------ | - | ------------------ |
| TRANSLATE | 0 | 不会随着路径旋转，只会平移。 |
| ROTATE  | 1 | 随着路径的旋转而旋转。 |
| MORPH  | 2 | 随着路径的旋转而旋转，并在转折处进行拉伸或压缩等操作以增加平滑度。 |

## PathEffect<sup>20+</sup>

路径效果对象。

### createDashPathEffect<sup>20+</sup>

static createDashPathEffect(intervals:  Array\<number>, phase: number): PathEffect

创建将路径变为虚线的路径效果对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型           | 必填    | 说明                                               |
| ---------- | ------------- | ------- | -------------------------------------------------- |
| intervals  | Array\<number> | 是      | 表示虚线的ON和OFF长度的数组，数组个数必须是偶数，且>=2，该参数为正整数。|
| phase      | number         | 是      | 绘制时的偏移量，该参数为浮点数。                                     |

**返回值：**

| 类型                      | 说明                   |
| ------------------------- | --------------------- |
| [PathEffect](#patheffect20) | 返回创建的路径效果对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let intervals = [10, 5];
    let effect = drawing.PathEffect.createDashPathEffect(intervals, 5);
  }
}
```

### createPathDashEffect<sup>20+</sup>

static createPathDashEffect(path: Path, advance: number, phase: number, style: PathDashStyle): PathEffect

通过路径描述的形状创建一个虚线路径效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型           | 必填    | 说明                                               |
| ---------- | ------------- | ------- | -------------------------------------------------- |
| path  | [Path](#path) | 是 | 通过该路径生成一个图形，用来填充每个虚线段。|
| advance | number | 是 | 虚线段的步长，该参数为大于0的浮点数，否则会抛错误码。 |
| phase | number | 是 | 表示虚线段内图形在虚线步长范围内的偏移量，该参数为浮点数，效果为先对偏移量取绝对值，然后对步长取模。 |
| style | [PathDashStyle](#pathdashstyle20) | 是 | 指定虚线效果的样式。 |

**返回值：**

| 类型                      | 说明                   |
| ------------------------- | --------------------- |
| [PathEffect](#patheffect20) | 返回创建的路径效果对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3. Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let pen = new drawing.Pen();
    const penColor: common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 }
    pen.setColor(penColor);
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    pen.setAntiAlias(true);

    const path = new drawing.Path();
    path.moveTo(100, 100);
    path.lineTo(150, 50);
    path.lineTo(200, 100);

    const path1 = new drawing.Path();
    path1.moveTo(0, 0);
    path1.lineTo(10, 0);
    path1.lineTo(20, 10);
    path1.lineTo(0,10);

    let pathEffect1: drawing.PathEffect = drawing.PathEffect.createPathDashEffect(path1, 50, -30,
        drawing.PathDashStyle.MORPH);
    pen.setPathEffect(pathEffect1);

    canvas.attachPen(pen);
    canvas.drawPath(path);
    canvas.detachPen();
  }
}
```

### createSumPathEffect<sup>20+</sup>

static createSumPathEffect(firstPathEffect: PathEffect, secondPathEffect: PathEffect): PathEffect

创建一个叠加的路径效果。与createComposePathEffect不同，此接口会分别对两个参数的效果各自独立进行表现，然后将两个效果简单重叠显示。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型           | 必填    | 说明                                               |
| ---------- | ------------- | ------- | -------------------------------------------------- |
| firstPathEffect | [PathEffect](#patheffect20) | 是 | 表示第一个路径效果。 |
| secondPathEffect | [PathEffect](#patheffect20) | 是 | 表示第二个路径效果。 |

**返回值：**

| 类型                      | 说明                   |
| ------------------------- | --------------------- |
| [PathEffect](#patheffect20) | 返回创建的路径效果对象。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let intervals = [10, 5];
    let pathEffectOne = drawing.PathEffect.createDashPathEffect(intervals, 5);
    let pathEffectTwo = drawing.PathEffect.createDashPathEffect(intervals, 10);
    let effect = drawing.PathEffect.createSumPathEffect(pathEffectOne, pathEffectTwo);
  }
}
```

### createCornerPathEffect<sup>20+</sup>

static createCornerPathEffect(radius: number): PathEffect

创建将路径的夹角变成指定半径的圆角的路径效果对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型           | 必填    | 说明                                               |
| ---------- | ------------- | ------- | -------------------------------------------------- |
| radius     | number        | 是      | 圆角的半径，必须大于0，该参数为浮点数。                |

**返回值：**

| 类型                      | 说明                   |
| ------------------------- | --------------------- |
| [PathEffect](#patheffect20) | 返回创建的路径效果对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let effect = drawing.PathEffect.createCornerPathEffect(30);
  }
}
```

### createDiscretePathEffect<sup>20+</sup>

static createDiscretePathEffect(segLength: number, dev: number, seedAssist?: number): PathEffect

创建一种将路径打散，并且在路径上产生不规则分布的效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型           | 必填    | 说明                                               |
| ---------- | ------------- | ------- | -------------------------------------------------- |
| segLength  | number        | 是      | 路径中每进行一次打散操作的长度，该长度为浮点数，负数和0时无效果。 |
| dev        | number        | 是      | 绘制时的末端点的最大移动偏离量，该偏移量为浮点数。 |
| seedAssist | number        | 否      | 生成效果伪随机种子辅助变量，默认值为0，该参数为32位无符号整数。 |

**返回值：**

| 类型                      | 说明                   |
| ------------------------- | --------------------- |
| [PathEffect](#patheffect20) | 返回创建的路径效果对象。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let effect = drawing.PathEffect.createDiscretePathEffect(100, -50, 0);
  }
}
```

### createComposePathEffect<sup>20+</sup>

static createComposePathEffect(outer: PathEffect, inner: PathEffect): PathEffect

创建路径组合的路径效果对象，首先应用内部路径效果，然后应用外部路径效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                        | 必填 | 说明                             |
| ------ | --------------------------- | ---- | -------------------------------- |
| outer  | [PathEffect](#patheffect20) | 是   | 组合路径效果中外部路径效果。 |
| inner  | [PathEffect](#patheffect20) | 是   | 组合路径效果中内部路径效果。 |

**返回值：**

| 类型                      | 说明                   |
| ------------------------- | --------------------- |
| [PathEffect](#patheffect20) | 返回创建的路径效果对象。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let pathEffect1 = drawing.PathEffect.createCornerPathEffect(100);
    let pathEffect2 = drawing.PathEffect.createCornerPathEffect(10);
    let effect = drawing.PathEffect.createComposePathEffect(pathEffect1, pathEffect2);
  }
}
```

## ShadowLayer<sup>20+</sup>

阴影层对象。

### create<sup>20+</sup>

static create(blurRadius: number, x: number, y: number, color: common2D.Color): ShadowLayer

创建阴影层对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型      | 必填 | 说明                                 |
| ---------- | -------- | ---- | ----------------------------------- |
| blurRadius  | number   | 是   | 阴影的半径，必须为大于零的浮点数。     |
| x           | number   | 是   | x轴上的偏移点，该参数为浮点数。        |
| y           | number   | 是   | Y轴上的偏移点，该参数为浮点数。        |
| color       | [common2D.Color](js-apis-graphics-common2D.md#color) | 是   | ARGB格式的颜色，每个颜色通道的值是0到255之间的整数。 |

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| [ShadowLayer](#shadowlayer20) | 返回创建的阴影层对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let color : common2D.Color = {alpha: 0xFF, red: 0x00, green: 0xFF, blue: 0x00};
    let shadowLayer = drawing.ShadowLayer.create(3, -3, 3, color);
  }
}
```

### create<sup>20+</sup>

static create(blurRadius: number, x: number, y: number, color: common2D.Color | number): ShadowLayer

创建阴影层对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名     | 类型      | 必填 | 说明                                 |
| ---------- | -------- | ---- | ----------------------------------- |
| blurRadius  | number   | 是   | 阴影的半径，必须为大于零的浮点数。     |
| x           | number   | 是   | x轴上的偏移点，该参数为浮点数。        |
| y           | number   | 是   | Y轴上的偏移点，该参数为浮点数。        |
| color       | [common2D.Color](js-apis-graphics-common2D.md#color) \| number   | 是   | 颜色，可以用16进制ARGB格式的无符号整数表示。  |

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| [ShadowLayer](#shadowlayer20) | 返回创建的阴影层对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let shadowLayer = drawing.ShadowLayer.create(3, -3, 3, 0xff00ff00);
  }
}
```

## Pen

画笔对象，描述所绘制图形形状的轮廓信息。

### constructor<sup>20+</sup>

constructor()

构造一个新的画笔对象。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
```

### constructor<sup>20+</sup>

constructor(pen: Pen)

复制构造一个新的画笔对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型        | 必填 | 说明              |
| ------| ----------- | ---- | ---------------- |
| pen     | [Pen](#pen) | 是   | 待复制的画笔对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
const penColor: common2D.Color = { alpha: 255, red: 0, green: 255, blue: 0 };
pen.setColor(penColor);
pen.setStrokeWidth(10);
const newPen = new drawing.Pen(pen);
```

### setMiterLimit<sup>20+</sup>

setMiterLimit(miter: number): void

设置折线尖角长度与线宽的最大比值，当画笔绘制一条折线，并且[JoinStyle](#joinstyle20)为MITER_JOIN时，若尖角长度与线宽的比值大于限制值，则该折角使用BEVEL_JOIN绘制。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明              |
| ------ | ------ | ---- | ---------------- |
| miter  | number | 是   | 折线尖角长度与线宽的最大比值，负数在绘制时会被视作4.0处理，非负数正常生效，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setMiterLimit(5);
```

### getMiterLimit<sup>20+</sup>

getMiterLimit(): number

获取折线尖角的限制值。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                 |
| -------| -------------------- |
| number | 返回折线尖角长度与线宽的最大比值。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
let miter = pen.getMiterLimit();
```

### setImageFilter<sup>20+</sup>

setImageFilter(filter: ImageFilter | null): void

设置画笔的图像滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| filter    | [ImageFilter](#imagefilter20) \| null | 是   |  图像滤波器，null表示清空画笔的图像滤波器效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types. |

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let colorfilter = drawing.ColorFilter.createSRGBGammaToLinear();
let imgFilter = drawing.ImageFilter.createFromColorFilter(colorfilter);
let pen = new drawing.Pen();
pen.setImageFilter(imgFilter);
pen.setImageFilter(null);
```

### getColorFilter<sup>20+</sup>

getColorFilter(): ColorFilter

获取画笔的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**示例：**

```ts 
import {drawing} from '@kit.ArkGraphics2D';

let pen = new drawing.Pen();
let colorfilter = drawing.ColorFilter.createLumaColorFilter();
pen.setColorFilter(colorfilter);
let filter = pen.getColorFilter();
```

### setColor

setColor(color: common2D.Color) : void

设置画笔的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| color  | [common2D.Color](js-apis-graphics-common2D.md#color) | 是   | ARGB格式的颜色，每个颜色通道的值是0到255之间的整数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
const pen = new drawing.Pen();
pen.setColor(color);
```

### setColor<sup>20+</sup>

setColor(alpha: number, red: number, green: number, blue: number): void

设置画笔的颜色。性能优于[setColor](#setcolor)接口，推荐使用本接口。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                |
| ------ | ------ | ---- | -------------------------------------------------- |
| alpha  | number | 是   | ARGB格式颜色的透明度通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。 |
| red    | number | 是   | ARGB格式颜色的红色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |
| green  | number | 是   | ARGB格式颜色的绿色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |
| blue   | number | 是   | ARGB格式颜色的蓝色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setColor(255, 255, 0, 0);
```

### setColor<sup>20+</sup>

setColor(color: number) : void

设置画笔的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| color  | number | 是   | 16进制ARGB格式的颜色。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setColor(0xffff0000);
```

### getColor<sup>20+</sup>

getColor(): common2D.Color

获取画笔的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型           | 说明            |
| -------------- | -------------- |
| common2D.Color | 返回画笔的颜色。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
const pen = new drawing.Pen();
pen.setColor(color);
let colorGet = pen.getColor();
```

### getColor4f<sup>20+</sup>

getColor4f(): common2D.Color4f

获取画笔的颜色，与[getColor](#getcolor20)的区别在于返回值类型为浮点数，适用于需要浮点数类型的场景。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型           | 说明            |
| -------------- | -------------- |
|[common2D.Color4f](js-apis-graphics-common2D.md#color4f20) | 返回画笔的颜色。 |

**示例：**

```ts
import { common2D, drawing, colorSpaceManager } from "@kit.ArkGraphics2D";

const pen = new drawing.Pen();
const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
pen.setColor(color);
let color4f = pen.getColor4f();
```

### getHexColor<sup>20+</sup>

getHexColor(): number

获取画笔的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型           | 说明            |
| -------------- | -------------- |
| number | 返回画笔的颜色，以16进制ARGB格式的32位无符号整数表示。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
let pen = new drawing.Pen();
pen.setColor(color);
let hex_color: number = pen.getHexColor();
console.info('getHexColor: ', hex_color.toString(16));
```

### setStrokeWidth

setStrokeWidth(width: number) : void

设置画笔的线宽。0线宽被视作特殊的极细线宽，在绘制时始终会被绘制为1像素，不随画布的缩放而改变；负数线宽在实际绘制时会被视作0线宽。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明             |
| ------ | ------ | ---- | ---------------- |
| width  | number | 是   | 表示线宽，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setStrokeWidth(5);
```

### getWidth<sup>20+</sup>

getWidth(): number

获取画笔的线宽属性，线宽描述了画笔绘制图形轮廓的宽度。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明            |
| ------ | -------------- |
| number | 返回画笔的线宽，单位为物理像素px。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
let width = pen.getWidth();
```

### setAntiAlias

setAntiAlias(aa: boolean) : void

设置画笔是否开启抗锯齿。开启后，可以使得图形的边缘在显示时更平滑。未调用此接口设置时，系统默认关闭抗锯齿。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明                                              |
| ------ | ------- | ---- | ------------------------------------------------- |
| aa     | boolean | 是   | 表示是否开启抗锯齿。true表示开启，false表示关闭。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setAntiAlias(true);
```

### isAntiAlias<sup>20+</sup>

isAntiAlias(): boolean

获取画笔是否开启抗锯齿属性。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean | 返回画笔是否开启抗锯齿属性，true表示开启，false表示关闭。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
let isAntiAlias = pen.isAntiAlias();
```

### setAlpha

setAlpha(alpha: number) : void

设置画笔的透明度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                                     |
| ------ | ------ | ---- | ---------------------------------------- |
| alpha  | number | 是   | 用于表示透明度的[0, 255]区间内的整数值，传入浮点类型时向下取整。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setAlpha(128);
```

### getAlpha<sup>20+</sup>

getAlpha(): number

获取画笔的透明度。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明              |
| ------ | ---------------- |
| number | 返回画笔的透明度，该返回值为0到255之间的整数。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
let alpha = pen.getAlpha();
```

### setColorFilter

setColorFilter(filter: ColorFilter) : void

给画笔添加额外的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                        | 必填 | 说明         |
| ------ | --------------------------- | ---- | ------------ |
| filter | [ColorFilter](#colorfilter) | 是   | 颜色滤波器。null表示清空颜色滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
let colorFilter = drawing.ColorFilter.createLinearToSRGBGamma();
pen.setColorFilter(colorFilter);
```

### setMaskFilter<sup>20+</sup>

setMaskFilter(filter: MaskFilter): void

给画笔添加额外的蒙版滤镜。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                       | 必填 | 说明      |
| ------ | ------------------------- | ---- | --------- |
| filter | [MaskFilter](#maskfilter20) | 是   | 蒙版滤镜。null表示清空蒙版滤镜。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    let maskFilter = drawing.MaskFilter.createBlurMaskFilter(drawing.BlurType.OUTER, 10);
    pen.setMaskFilter(maskFilter);
  }
}
```

### setPathEffect<sup>20+</sup>

setPathEffect(effect: PathEffect): void

设置画笔路径效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型                       | 必填 | 说明         |
| ------- | ------------------------- | ---- | ------------ |
| effect  | [PathEffect](#patheffect20) | 是   | 路径效果对象。null表示清空路径效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    let pathEffect = drawing.PathEffect.createDashPathEffect([30, 10], 0);
    pen.setPathEffect(pathEffect);
  }
}
```

### setShaderEffect<sup>20+</sup>

setShaderEffect(shaderEffect: ShaderEffect): void

设置画笔着色器效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型                       | 必填 | 说明         |
| ------- | ------------------------- | ---- | ------------ |
| shaderEffect  | [ShaderEffect](#shadereffect20) | 是   | 着色器对象。null表示清空着色器效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
let shaderEffect = drawing.ShaderEffect.createLinearGradient({x: 100, y: 100}, {x: 300, y: 300}, [0xFF00FF00, 0xFFFF0000], drawing.TileMode.REPEAT);
pen.setShaderEffect(shaderEffect);
```

### setShadowLayer<sup>20+</sup>

setShadowLayer(shadowLayer: ShadowLayer): void

设置画笔阴影层效果。当前仅在绘制文字时生效。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型                       | 必填 | 说明      |
| ------- | ------------------------- | ---- | --------- |
| shadowLayer  | [ShadowLayer](#shadowlayer20) | 是   | 阴影层对象。null表示清空阴影层效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let font = new drawing.Font();
    font.setSize(60);
    let textBlob = drawing.TextBlob.makeFromString("hello", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    let pen = new drawing.Pen();
    pen.setStrokeWidth(2.0);
    let pen_color : common2D.Color = {alpha: 0xFF, red: 0xFF, green: 0x00, blue: 0x00};
    pen.setColor(pen_color);
    canvas.attachPen(pen);
    canvas.drawTextBlob(textBlob, 100, 100);
    canvas.detachPen();
    let color : common2D.Color = {alpha: 0xFF, red: 0x00, green: 0xFF, blue: 0x00};
    let shadowLayer = drawing.ShadowLayer.create(3, -3, 3, color);
    pen.setShadowLayer(shadowLayer);
    canvas.attachPen(pen);
    canvas.drawTextBlob(textBlob, 100, 200);
    canvas.detachPen();
  }
}
```

### setBlendMode

setBlendMode(mode: BlendMode) : void

设置画笔的混合模式。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                    | 必填 | 说明             |
| ------ | ----------------------- | ---- | ---------------- |
| mode   | [BlendMode](#blendmode) | 是   | 颜色的混合模式。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setBlendMode(drawing.BlendMode.SRC);
```

### setJoinStyle<sup>20+</sup>

setJoinStyle(style: JoinStyle): void

设置画笔绘制转角的样式。未调用此接口设置时，系统默认的转角样式为MITER_JOIN。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                     | 必填 | 说明             |
| ------ | ----------------------- | ---- | --------------- |
| style  | [JoinStyle](#joinstyle20) | 是   | 折线转角样式。     |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    pen.setJoinStyle(drawing.JoinStyle.ROUND_JOIN);
  }
}
```

### getJoinStyle<sup>20+</sup>

getJoinStyle(): JoinStyle

获取画笔绘制转角的样式。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型          | 说明                    |
| ------------- | ---------------------- |
| JoinStyle | 返回折线转角的样式。         |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    pen.setJoinStyle(drawing.JoinStyle.ROUND_JOIN);
    let joinStyle = pen.getJoinStyle();
  }
}
```

### setCapStyle<sup>20+</sup>

setCapStyle(style: CapStyle): void

设置画笔的线帽样式。未调用此接口设置时，系统默认的线帽样式为FLAT_CAP。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                     | 必填 | 说明                   |
| ------ | ----------------------- | ---- | --------------------- |
| style  | [CapStyle](#capstyle20)   | 是   | 描述画笔的线帽样式。    |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    pen.setCapStyle(drawing.CapStyle.SQUARE_CAP);
  }
}
```

### getCapStyle<sup>20+</sup>

getCapStyle(): CapStyle

获取画笔的线帽样式。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型         | 说明                |
| ------------ | ------------------ |
| CapStyle     | 返回画笔的线帽样式。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setStrokeWidth(5);
    pen.setColor({alpha: 255, red: 255, green: 0, blue: 0});
    pen.setCapStyle(drawing.CapStyle.SQUARE_CAP);
    let capStyle = pen.getCapStyle();
  }
}
```

### setDither

setDither(dither: boolean) : void

开启画笔的抖动绘制效果。抖动绘制可以使得绘制出的颜色更加真实。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                      |
| ------ | ------- | ---- | --------------------------------------------------------- |
| dither | boolean | 是   | 是否开启画笔的抖动绘制效果。true表示开启，false表示关闭。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.setDither(true);
```

### getFillPath<sup>20+</sup>

getFillPath(src: Path, dst: Path): boolean

获取使用画笔绘制的源路径轮廓，并用目标路径表示。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| src | [Path](#path) | 是   | 源路径对象。                 |
| dst     | [Path](#path)                | 是   | 目标路径对象。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回获取源路径轮廓是否成功，true表示成功，false表示失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let pen = new drawing.Pen();
let pathSrc: drawing.Path = new drawing.Path();
let pathDst: drawing.Path = new drawing.Path();
pathSrc.moveTo(0, 0);
pathSrc.lineTo(700, 700);
let value = pen.getFillPath(pathSrc, pathDst);
```

### reset<sup>20+</sup>

reset(): void

重置当前画笔为初始状态。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const pen = new drawing.Pen();
pen.reset();
```

## Brush

画刷对象，描述所绘制图形的填充信息。

### constructor<sup>20+</sup>

constructor()

构造一个新的画刷对象。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
```

### constructor<sup>20+</sup>

constructor(brush: Brush)

复制构造一个新的画刷对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型        | 必填 | 说明              |
| ------| ----------- | ---- | ---------------- |
| brush     | [Brush](#brush) | 是   | 待复制的画刷对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
const brushColor: common2D.Color = { alpha: 255, red: 0, green: 255, blue: 0 };
brush.setColor(brushColor);
const newBrush = new drawing.Brush(brush);
```

### setColor

setColor(color: common2D.Color) : void

设置画刷的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| color  | [common2D.Color](js-apis-graphics-common2D.md#color) | 是   | ARGB格式的颜色，每个颜色通道的值是0到255之间的整数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
const brush = new drawing.Brush();
brush.setColor(color);
```

### setColor<sup>20+</sup>

setColor(alpha: number, red: number, green: number, blue: number): void

设置画刷的颜色。性能优于[setColor](#setcolor-1)接口，推荐使用本接口。

**系统能力：** SystemCapability.Graphics.Drawing
 
**参数：**

| 参数名 | 类型    | 必填 | 说明                                               |
| ------ | ------ | ---- | -------------------------------------------------- |
| alpha  | number | 是   | ARGB格式颜色的透明度通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。 |
| red    | number | 是   | ARGB格式颜色的红色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |
| green  | number | 是   | ARGB格式颜色的绿色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |
| blue   | number | 是   | ARGB格式颜色的蓝色通道值，该参数是0到255之间的整数，传入范围内的浮点数会向下取整。   |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
brush.setColor(255, 255, 0, 0);
```

### setColor<sup>20+</sup>

setColor(color: number) : void

设置画刷的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| color  | number | 是   | 16进制ARGB格式的颜色。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
brush.setColor(0xffff0000);
```

### getColor<sup>20+</sup>

getColor(): common2D.Color

获取画刷的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型           | 说明            |
| -------------- | -------------- |
| common2D.Color | 返回画刷的颜色。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
const brush = new drawing.Brush();
brush.setColor(color);
let colorGet = brush.getColor();
```

### getColor4f<sup>20+</sup>

getColor4f(): common2D.Color4f

获取画刷的颜色，与[getColor](#getcolor20-1)的区别是返回值类型为浮点数，适用于需要浮点数类型的场景。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型           | 说明            |
| -------------- | -------------- |
| [common2D.Color4f](js-apis-graphics-common2D.md#color4f20) | 返回画刷的颜色。 |

**示例：**

```ts
import { common2D, drawing, colorSpaceManager } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
const color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
brush.setColor(color);
let color4f = brush.getColor4f();
```

### getHexColor<sup>20+</sup>

getHexColor(): number

获取画刷的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型           | 说明            |
| -------------- | -------------- |
| number | 返回画刷的颜色，以16进制ARGB格式的32位无符号整数表示。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let color : common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
let brush = new drawing.Brush();
brush.setColor(color);
let hex_color: number = brush.getHexColor();
console.info('getHexColor: ', hex_color.toString(16));
```

### setAntiAlias

setAntiAlias(aa: boolean) : void

设置画刷是否开启抗锯齿。开启后，可以使得图形的边缘在显示时更平滑。未调用此接口设置时，系统默认关闭抗锯齿。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明                                              |
| ------ | ------- | ---- | ------------------------------------------------- |
| aa     | boolean | 是   | 表示是否开启抗锯齿，true表示开启，false表示关闭。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
brush.setAntiAlias(true);
```

### isAntiAlias<sup>20+</sup>

isAntiAlias(): boolean

获取画刷是否开启抗锯齿属性。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean | 返回画刷是否开启抗锯齿属性，true表示开启，false表示关闭。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
let isAntiAlias = brush.isAntiAlias();
```

### setAlpha

setAlpha(alpha: number) : void

设置画刷的透明度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                                     |
| ------ | ------ | ---- | ---------------------------------------- |
| alpha  | number | 是   | 用于表示透明度的[0, 255]区间内的整数值，传入浮点类型时向下取整。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
brush.setAlpha(128);
```

### getAlpha<sup>20+</sup>

getAlpha(): number

获取画刷的透明度。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明              |
| ------ | ---------------- |
| number | 返回画刷的透明度，该返回值为0到255之间的整数。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
let alpha = brush.getAlpha();
```

### setColorFilter

setColorFilter(filter: ColorFilter) : void

给画刷添加额外的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                        | 必填 | 说明         |
| ------ | --------------------------- | ---- | ------------ |
| filter | [ColorFilter](#colorfilter) | 是   | 颜色滤波器。null表示清空颜色滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
let colorFilter = drawing.ColorFilter.createLinearToSRGBGamma();
brush.setColorFilter(colorFilter);
```

### setMaskFilter<sup>20+</sup>

setMaskFilter(filter: MaskFilter): void

给画刷添加额外的蒙版滤镜。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                       | 必填 | 说明      |
| ------ | ------------------------- | ---- | --------- |
| filter | [MaskFilter](#maskfilter20) | 是   | 蒙版滤镜。null表示清空蒙版滤镜。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    let maskFilter = drawing.MaskFilter.createBlurMaskFilter(drawing.BlurType.OUTER, 10);
    brush.setMaskFilter(maskFilter);
  }
}
```

### setShaderEffect<sup>20+</sup>

setShaderEffect(shaderEffect: ShaderEffect): void

设置画刷着色器效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型                       | 必填 | 说明         |
| ------- | ------------------------- | ---- | ------------ |
| shaderEffect  | [ShaderEffect](#shadereffect20) | 是   | 着色器对象。null表示清空着色器效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
let shaderEffect = drawing.ShaderEffect.createLinearGradient({x: 100, y: 100}, {x: 300, y: 300}, [0xFF00FF00, 0xFFFF0000], drawing.TileMode.REPEAT);
brush.setShaderEffect(shaderEffect);
```

### setShadowLayer<sup>20+</sup>

setShadowLayer(shadowLayer: ShadowLayer): void

设置画刷阴影层效果。当前仅在绘制文字时生效。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名  | 类型                       | 必填 | 说明      |
| ------- | ------------------------- | ---- | --------- |
| shadowLayer  | [ShadowLayer](#shadowlayer20) | 是   | 阴影层对象。null表示清空阴影层效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { common2D, drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    let font = new drawing.Font();
    font.setSize(60);

    let textBlob = drawing.TextBlob.makeFromString("hello", font, drawing.TextEncoding.TEXT_ENCODING_UTF8);
    let pen = new drawing.Pen();
    pen.setStrokeWidth(2.0);

    let pen_color : common2D.Color = {alpha: 0xFF, red: 0xFF, green: 0x00, blue: 0x00};
    pen.setColor(pen_color);
    canvas.attachPen(pen);
    canvas.drawTextBlob(textBlob, 100, 100);
    canvas.detachPen();

    let color : common2D.Color = {alpha: 0xFF, red: 0x00, green: 0xFF, blue: 0x00};
    let shadowLayer = drawing.ShadowLayer.create(3, -3, 3, color);
    pen.setShadowLayer(shadowLayer);
    canvas.attachPen(pen);
    canvas.drawTextBlob(textBlob, 100, 200);
    canvas.detachPen();

    let brush = new drawing.Brush();
    let brush_color : common2D.Color = {alpha: 0xFF, red: 0xFF, green: 0x00, blue: 0x00};
    brush.setColor(brush_color);
    canvas.attachBrush(brush);
    canvas.drawTextBlob(textBlob, 300, 100);
    canvas.detachBrush();

    brush.setShadowLayer(shadowLayer);
    canvas.attachBrush(brush);
    canvas.drawTextBlob(textBlob, 300, 200);
    canvas.detachBrush();
  }
}
```

### setBlendMode

setBlendMode(mode: BlendMode) : void

设置画刷的混合模式。未调用此接口设置时，系统默认的混合模式为SRC_OVER。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                    | 必填 | 说明             |
| ------ | ----------------------- | ---- | ---------------- |
| mode   | [BlendMode](#blendmode) | 是   | 颜色的混合模式。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
brush.setBlendMode(drawing.BlendMode.SRC);
```

### setImageFilter<sup>20+</sup>

setImageFilter(filter: ImageFilter | null): void

为画刷设置图像滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| filter    | [ImageFilter](#imagefilter20) \| null | 是   | 图像滤波器，null表示清空图像滤波器效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types. | 

**示例：**

```ts
import {drawing} from '@kit.ArkGraphics2D';

let brush = new drawing.Brush();
let imgFilter = drawing.ImageFilter.createBlurImageFilter(5, 10, drawing.TileMode.DECAL);
brush.setImageFilter(imgFilter);
brush.setImageFilter(null);
```

### getColorFilter<sup>20+</sup>

getColorFilter(): ColorFilter

获取画刷的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明               |
| --------------------------- | ------------------ |
| [ColorFilter](#colorfilter) | 返回颜色滤波器。 |

**示例：**

```ts 
import {drawing} from '@kit.ArkGraphics2D';

let brush = new drawing.Brush();
let setColorFilter = drawing.ColorFilter.createSRGBGammaToLinear();
brush.setColorFilter(setColorFilter);
let filter = brush.getColorFilter();   
```

### reset<sup>20+</sup>

reset(): void

重置当前画刷为初始状态。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

const brush = new drawing.Brush();
brush.reset();
```

## ScaleToFit<sup>20+</sup>

源矩形到目标矩形的缩放方式枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                   | 值   | 说明                           |
| ---------------------- | ---- | ------------------------------ |
| FILL_SCALE_TO_FIT     | 0    | 将源矩形缩放以填充满整个目标矩形，可能会改变源矩形的长宽比。  |
| START_SCALE_TO_FIT    | 1    | 保持源矩形的长宽比进行缩放，并对齐到目标矩形的左上方。 |
| CENTER_SCALE_TO_FIT    | 2    | 保持源矩形的长宽比进行缩放，并居中对齐到目标矩形。   |
| END_SCALE_TO_FIT | 3    | 保持源矩形的长宽比进行缩放，并对齐到目标矩形的右下方。   |

## Matrix<sup>20+</sup>

矩阵对象。

表示为3*3的矩阵，如下图所示：

![matrix_3x3](figures/matrix3X3.PNG)

矩阵中的元素从左到右，从上到下分别表示水平缩放系数、水平倾斜系数、水平位移系数、垂直倾斜系数、垂直缩放系数、垂直位移系数、X轴透视系数、Y轴透视系数、透视缩放系数。
设(x<sub>1</sub>, y<sub>1</sub>)为源坐标点，(x<sub>2</sub>, y<sub>2</sub>)为源坐标点通过矩阵变换后的坐标点，则两个坐标点的关系如下：

![matrix_xy](figures/matrix_xy.PNG)

### constructor<sup>20+</sup>

constructor()

构造一个矩阵对象。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
```

### constructor<sup>20+</sup>

constructor(matrix: Matrix)

拷贝一个矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| matrix      | [Matrix](#matrix20)                  | 是    | 被拷贝的矩阵。|

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
let matrix2 = new drawing.Matrix(matrix);
```

### isAffine<sup>20+</sup>

isAffine(): boolean

判断当前矩阵是否为仿射矩阵。仿射矩阵是一种包括平移、旋转、缩放等变换的矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| boolean | 返回当前矩阵是否为仿射矩阵。true表示是仿射矩阵，false表示不是仿射矩阵。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
matrix.setMatrix([1, 0.5, 1, 0.5, 1, 1, 1, 1, 1]);
let isAff = matrix.isAffine();
console.info('isAff :', isAff);
```

### rectStaysRect<sup>20+</sup>

rectStaysRect(): boolean

判断经过该矩阵映射后的矩形的形状是否仍为矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| boolean | 返回经过该矩阵映射后的矩形的形状是否仍为矩形。true表示仍是矩形，false表示不是矩形。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
matrix.setMatrix([1, 0.5, 1, 0.5, 1, 1, 1, 1, 1]);
let matrix2 = new drawing.Matrix(matrix);
let isRect = matrix2.rectStaysRect();
console.info('isRect :', isRect);
```

### setSkew<sup>20+</sup>

setSkew(kx: number, ky: number, px: number, py: number): void

设置矩阵的倾斜系数。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                       |
| ----------- | ---------------------------------------- | ---- | -------------------             |
| kx          | number                  | 是    | x轴上的倾斜量，该参数为浮点数。正值会使绘制沿y轴增量方向向右倾斜；负值会使绘制沿y轴增量方向向左倾斜。        |
| ky          | number                  | 是    | y轴上的倾斜量，该参数为浮点数。正值会使绘制沿x轴增量方向向下倾斜；负值会使绘制沿x轴增量方向向上倾斜。        |
| px          | number                  | 是    | 倾斜中心点的x轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点右侧，负数表示位于坐标原点左侧。     |
| py          | number                  | 是    | 倾斜中心点的y轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点下侧，负数表示位于坐标原点上侧。     |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
matrix.setMatrix([1, 0.5, 1, 0.5, 1, 1, 1, 1, 1]);
matrix.setSkew(2, 0.5, 0.5, 2);
```

### setSinCos<sup>20+</sup>

setSinCos(sinValue: number, cosValue: number, px: number, py: number): void

设置矩阵，使其围绕旋转中心(px, py)以指定的正弦值和余弦值旋转。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明            |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| sinValue          | number                  | 是    | 旋转角度的正弦值。仅当正弦值和余弦值的平方和为1时，为旋转变换，否则矩阵可能包含平移缩放等其他变换。          |
| cosValue          | number                  | 是    | 旋转角度的余弦值。仅当正弦值和余弦值的平方和为1时，为旋转变换，否则矩阵可能包含平移缩放等其他变换。            |
| px          | number                  | 是    | 旋转中心的x轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点右侧，负数表示位于坐标原点左侧。     |
| py          | number                  | 是    | 旋转中心的y轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点下侧，负数表示位于坐标原点上侧。    |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
matrix.setMatrix([1, 0.5, 1, 0.5, 1, 1, 1, 1, 1]);
matrix.setSinCos(0, 1, 1, 0);
```
### setRotation<sup>20+</sup>

setRotation(degree: number, px: number, py: number): void

设置矩阵为单位矩阵，并围绕位于(px, py)的旋转轴点进行旋转。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| degree      | number                  | 是    | 角度，单位为度。正数表示顺时针旋转，负数表示逆时针旋转，该参数为浮点数。|
| px          | number                  | 是    | 旋转轴点的横坐标，该参数为浮点数。     |
| py          | number                  | 是    | 旋转轴点的纵坐标，该参数为浮点数。     |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
matrix.setRotation(90, 100, 100);
```

### setScale<sup>20+</sup>

setScale(sx: number, sy: number, px: number, py: number): void

设置矩阵为单位矩阵围绕位于(px, py)的中心点，以sx和sy进行缩放后的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| sx          | number                  | 是    | x轴方向缩放系数，为负数时可看作是先关于y = px作镜像翻转后再进行缩放，该参数为浮点数。     |
| sy          | number                  | 是    | y轴方向缩放系数，为负数时可看作是先关于x = py作镜像翻转后再进行缩放，该参数为浮点数。     |
| px          | number                  | 是    |  缩放中心点的横坐标，该参数为浮点数。      |
| py          | number                  | 是    |  缩放中心点的纵坐标，该参数为浮点数。      |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
matrix.setScale(100, 100, 150, 150);
```

### setTranslation<sup>20+</sup>

setTranslation(dx: number, dy: number): void

设置矩阵为单位矩阵平移(dx, dy)后的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| dx          | number                  | 是    | x轴方向平移距离，正数表示往x轴正方向平移，负数表示往x轴负方向平移，该参数为浮点数。     |
| dy          | number                  | 是    | y轴方向平移距离，正数表示往y轴正方向平移，负数表示往y轴负方向平移，该参数为浮点数。     |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
matrix.setTranslation(100, 100);
```

### setMatrix<sup>20+</sup>

setMatrix(values: Array\<number>): void

设置矩阵对象的各项参数。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| values  | Array\<number> | 是   | 长度为9的浮点数组，表示矩阵对象参数。数组中的值按下标从小，到大分别表示水平缩放系数、水平倾斜系数、水平位移系数、垂直倾斜系数、垂直缩放系数、垂直位移系数、X轴透视系数、Y轴透视系数、透视缩放系数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
let value : Array<number> = [2, 2, 2, 2, 2, 2, 2, 2, 2];
matrix.setMatrix(value);
```

### preConcat<sup>20+</sup>

preConcat(matrix: Matrix): void

将当前矩阵设置为当前矩阵左乘matrix的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| matrix  | [Matrix](#matrix20) | 是   | 表示矩阵对象，位于乘法表达式右侧。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix1 = new drawing.Matrix();
matrix1.setMatrix([2, 1, 3, 1, 2, 1, 3, 1, 2]);
let matrix2 = new drawing.Matrix();
matrix2.setMatrix([-2, 1, 3, 1, 0, -1, 3, -1, 2]);
matrix1.preConcat(matrix2);
```

### setMatrix<sup>20+</sup>

setMatrix(matrix: Array\<number\> \| Matrix): void

用一个矩阵对当前矩阵进行更新。

**系统能力：** SystemCapability.Graphics.Drawing

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| matrix | Array\<number\> \| [Matrix](#matrix20) | 是   | 用于更新的数组或矩阵。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix1 = new drawing.Matrix();
matrix1.setMatrix([2, 1, 3, 1, 2, 1, 3, 1, 2]);
let matrix2 = new drawing.Matrix();
matrix1.setMatrix(matrix2);
```

### setConcat<sup>20+</sup>

setConcat(matrixA: Matrix, matrixB: Matrix): void

用两个矩阵的乘积更新当前矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| matrixA  | [Matrix](#matrix20) | 是   | 用于运算的矩阵A。 |
| matrixB  | [Matrix](#matrix20) | 是   | 用于运算的矩阵B。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix1 = new drawing.Matrix();
matrix1.setMatrix([2, 1, 3, 1, 2, 1, 3, 1, 2]);
let matrix2 = new drawing.Matrix();
matrix2.setMatrix([-2, 1, 3, 1, 0, -1, 3, -1, 2]);
matrix1.setConcat(matrix2, matrix1);
```

### postConcat<sup>20+</sup>

postConcat(matrix: Matrix): void

用当前矩阵右乘一个矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| matrix | [Matrix](#matrix20) | 是   | 用于运算的矩阵。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
if (matrix.isIdentity()) {
  console.info("matrix is identity.");
} else {
  console.info("matrix is not identity.");
}
let matrix1 = new drawing.Matrix();
matrix1.setMatrix([2, 1, 3, 1, 2, 1, 3, 1, 2]);
let matrix2 = new drawing.Matrix();
matrix2.setMatrix([-2, 1, 3, 1, 0, -1, 3, -1, 2]);
matrix1.postConcat(matrix2);
```

### isEqual<sup>20+</sup>

isEqual(matrix: Matrix): Boolean

判断两个矩阵是否相等。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| matrix  | [Matrix](#matrix20) | 是   | 另一个矩阵。 |

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| Boolean | 返回两个矩阵的比较结果。true表示两个矩阵相等，false表示两个矩阵不相等。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix1 = new drawing.Matrix();
matrix1.setMatrix([2, 1, 3, 1, 2, 1, 3, 1, 2]);
let matrix2 = new drawing.Matrix();
matrix2.setMatrix([-2, 1, 3, 1, 0, -1, 3, -1, 2]);
if (matrix1.isEqual(matrix2)) {
  console.info("matrix1 and matrix2 are equal.");
} else {
  console.info("matrix1 and matrix2 are not equal.");
}
```

### invert<sup>20+</sup>

invert(matrix: Matrix): Boolean

将矩阵matrix设置为当前矩阵的逆矩阵，并返回是否设置成功的结果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| matrix  | [Matrix](#matrix20) | 是   | 矩阵对象，用于存储获取到的逆矩阵。 |

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| Boolean | 返回matrix是否被设置为逆矩阵的结果。true表示当前矩阵可逆，matrix被设置为逆矩阵，false表示当前矩阵不可逆，matrix不被设置。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix1 = new drawing.Matrix();
matrix1.setMatrix([2, 1, 3, 1, 2, 1, 3, 1, 2]);
let matrix2 = new drawing.Matrix();
matrix2.setMatrix([-2, 1, 3, 1, 0, -1, 3, -1, 2]);
if (matrix1.invert(matrix2)) {
  console.info("matrix1 is invertible and matrix2 is set as an inverse matrix of the matrix1.");
} else {
  console.info("matrix1 is not invertible and matrix2 is not changed.");
}
```

### isIdentity<sup>20+</sup>

isIdentity(): Boolean

判断矩阵是否是单位矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| Boolean | 返回矩阵是否是单位矩阵。true表示矩阵是单位矩阵，false表示矩阵不是单位矩阵。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let matrix = new drawing.Matrix();
if (matrix.isIdentity()) {
  console.info("matrix is identity.");
} else {
  console.info("matrix is not identity.");
}
```

### getValue<sup>20+</sup>

getValue(index: number): number

获取矩阵给定索引位的值。索引范围0-8。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| index | number | 是   | 索引位置，范围0-8，该参数为整数。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| number | 函数返回矩阵给定索引位对应的值，该返回值为整数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed.|

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
for (let i = 0; i < 9; i++) {
    console.info("matrix "+matrix.getValue(i).toString());
}
```

### postRotate<sup>20+</sup>

postRotate(degree: number, px: number, py: number): void

将矩阵设置为矩阵右乘围绕轴心点旋转一定角度的单位矩阵后得到的矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| degree | number | 是   | 旋转角度，单位为度。正数表示顺时针旋转，负数表示逆时针旋转，该参数为浮点数。 |
| px | number | 是   | 旋转中心点的横坐标，该参数为浮点数。 |
| py | number | 是   | 旋转中心点的纵坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
let degree: number = 2;
let px: number = 3;
let py: number = 4;
matrix.postRotate(degree, px, py);
console.info("matrix= "+matrix.getAll().toString());
```

### postScale<sup>20+</sup>

postScale(sx: number, sy: number, px: number, py: number): void

将矩阵设置为矩阵右乘围绕轴心点按一定缩放系数缩放后的单位矩阵后得到的矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| sx | number | 是   | x轴方向缩放系数，负数表示先关于y = px作镜像翻转后再进行缩放，该参数为浮点数。 |
| sy | number | 是   | y轴方向缩放系数，负数表示先关于x = py作镜像翻转后再进行缩放，该参数为浮点数。 |
| px | number | 是   | 缩放中心点的横坐标，该参数为浮点数。 |
| py | number | 是   | 缩放中心点的纵坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
let sx: number = 2;
let sy: number = 0.5;
let px: number = 1;
let py: number = 1;
matrix.postScale(sx, sy, px, py);
console.info("matrix= "+matrix.getAll().toString());
```

### postTranslate<sup>20+</sup>

postTranslate(dx: number, dy: number): void

将矩阵设置为矩阵右乘平移一定距离后的单位矩阵后得到的矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| dx | number | 是   | x轴方向平移距离，正数表示往x轴正方向平移，负数表示往x轴负方向平移，该参数为浮点数。 |
| dy | number | 是   | y轴方向平移距离，正数表示往y轴正方向平移，负数表示往y轴负方向平移，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
let dx: number = 3;
let dy: number = 4;
matrix.postTranslate(dx, dy);
console.info("matrix= "+matrix.getAll().toString());
```

### preRotate<sup>20+</sup>

preRotate(degree: number, px: number, py: number): void

将矩阵设置为矩阵左乘围绕轴心点旋转一定角度的单位矩阵后得到的矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| degree | number | 是   | 旋转角度，单位为度。正数表示顺时针旋转，负数表示逆时针旋转，该参数为浮点数。 |
| px | number | 是   | 旋转中心点的横坐标，该参数为浮点数。 |
| py | number | 是   | 旋转中心点的纵坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
let degree: number = 2;
let px: number = 3;
let py: number = 4;
matrix.preRotate(degree, px, py);
console.info("matrix= "+matrix.getAll().toString());
```

### postSkew<sup>20+</sup>

postSkew(kx: number, ky: number, px: number, py: number): void

当前矩阵右乘一个倾斜变换矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明             |
| ----------- | ---------------------------------------- | ---- | -------------------   |
| kx          | number                  | 是    | x轴上的倾斜量，该参数为浮点数。正值会使绘制沿y轴增量方向向右倾斜；负值会使绘制沿y轴增量方向向左倾斜。           |
| ky          | number                  | 是    | y轴上的倾斜量，该参数为浮点数。正值会使绘制沿x轴增量方向向下倾斜；负值会使绘制沿x轴增量方向向上倾斜。           |
| px          | number                  | 是    | 倾斜中心点的x轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点右侧，负数表示位于坐标原点左侧。    |
| py          | number                  | 是    | 倾斜中心点的y轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点下侧，负数表示位于坐标原点上侧。   |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D"
let matrix = new drawing.Matrix();
matrix.postSkew(2.0, 1.0, 2.0, 1.0);
```

### preSkew<sup>20+</sup>

 preSkew(kx: number, ky: number, px: number, py: number): void

当前矩阵左乘一个倾斜变换矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明             |
| ----------- | ---------------------------------------- | ---- | -------------------   |
| kx          | number                  | 是    | x轴上的倾斜量，该参数为浮点数。正值会使绘制沿y轴增量方向向右倾斜；负值会使绘制沿y轴增量方向向左倾斜。           |
| ky          | number                  | 是    | y轴上的倾斜量，该参数为浮点数。正值会使绘制沿x轴增量方向向下倾斜；负值会使绘制沿x轴增量方向向上倾斜。           |
| px          | number                  | 是    | 倾斜中心点的x轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点右侧，负数表示位于坐标原点左侧。        |
| py          | number                  | 是    | 倾斜中心点的y轴坐标，该参数为浮点数。0表示坐标原点，正数表示位于坐标原点下侧，负数表示位于坐标原点上侧。        |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D"
let matrix = new drawing.Matrix();
matrix.preSkew(2.0, 1.0, 2.0, 1.0);
```

### mapRadius<sup>20+</sup>

mapRadius(radius: number): number

返回半径为radius的圆经过当前矩阵映射形成的椭圆的平均半径。平均半径的平方为椭圆长轴长度和短轴长度的乘积。若当前矩阵包含透视变换，则该结果无意义。

**系统能力：** SystemCapability.Graphics.Drawing

| 参数名 | 类型                                                 | 必填 | 说明             |
| ------ | ---------------------------------------------------- | ---- | ---------------- |
| radius  | number | 是   | 用于计算的圆的半径，浮点数。如果是负数，则按照绝对值进行计算。 |

**返回值：**

| 类型                        | 说明                  |
| --------------------------- | -------------------- |
| number | 返回经过变换之后的平均半径。 |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D"

let matrix = new drawing.Matrix();
matrix.setMatrix([2, 1, 3, 1, 2, 1, 3, 1, 2]);
let radius = matrix.mapRadius(10);
console.info('radius', radius);
```

### preScale<sup>20+</sup>

preScale(sx: number, sy: number, px: number, py: number): void

将矩阵设置为矩阵左乘围绕轴心点按一定缩放系数缩放后的单位矩阵后得到的矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| sx | number | 是   | x轴方向缩放系数，为负数时可看作是先关于y = px作镜像翻转后再进行缩放，该参数为浮点数。 |
| sy | number | 是   | y轴方向缩放系数，为负数时可看作是先关于x = py作镜像翻转后再进行缩放，该参数为浮点数。 |
| px | number | 是   | 轴心点横坐标，该参数为浮点数。 |
| py | number | 是   | 轴心点纵坐标，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
let sx: number = 2;
let sy: number = 0.5;
let px: number = 1;
let py: number = 1;
matrix.preScale(sx, sy, px, py);
console.info("matrix"+matrix.getAll().toString());
```

### preTranslate<sup>20+</sup>

preTranslate(dx: number, dy: number): void

将矩阵设置为矩阵左乘平移一定距离后的单位矩阵后得到的矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| dx | number | 是   | x轴方向平移距离，正数表示往x轴正方向平移，负数表示往x轴负方向平移，该参数为浮点数。 |
| dy | number | 是   | y轴方向平移距离，正数表示往y轴正方向平移，负数表示往y轴负方向平移，该参数为浮点数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
let dx: number = 3;
let dy: number = 4;
matrix.preTranslate(dx, dy);
console.info("matrix"+matrix.getAll().toString());
```

### reset<sup>20+</sup>

reset(): void

重置当前矩阵为单位矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
matrix.postScale(2, 3, 4, 5);
matrix.reset();
console.info("matrix= "+matrix.getAll().toString());
```

### mapPoints<sup>20+</sup>

mapPoints(src: Array\<common2D.Point>): Array\<common2D.Point>

通过矩阵变换将源点数组映射到目标点数组。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| src | Array\<[common2D.Point](js-apis-graphics-common2D.md#point20)> | 是   | 源点数组。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| Array\<[common2D.Point](js-apis-graphics-common2D.md#point20)> | 源点数组经矩阵变换后的点数组。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing,common2D} from "@kit.ArkGraphics2D";

let src: Array<common2D.Point> = [];
src.push({x: 15, y: 20});
src.push({x: 20, y: 15});
src.push({x: 30, y: 10});
let matrix = new drawing.Matrix();
let dst: Array<common2D.Point> = matrix.mapPoints(src);
console.info("matrix= src: "+JSON.stringify(src));
console.info("matrix= dst: "+JSON.stringify(dst));
```

### getAll<sup>20+</sup>

getAll(): Array\<number>

获取矩阵的所有元素值。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| Array\<number> | 存储矩阵元素值的浮点数组，长度为9。 |

**示例：**

```ts
import {drawing} from "@kit.ArkGraphics2D";

let matrix = new drawing.Matrix();
console.info("matrix "+ matrix.getAll());
```

### mapRect<sup>20+</sup>

mapRect(dst: common2D.Rect, src: common2D.Rect): boolean

将目标矩形设置为源矩形通过矩阵变换后的图形的外接矩形。如下图所示，蓝色矩形为源矩形，假设黄色矩形为源矩形通过矩阵变换形成的图形，此时黄色矩形的边不与坐标轴平行，无法使用矩形对象表示，因此，将目标矩形设置为黄色矩形的外接矩形，即黑色矩形。

![mapRect](./figures/zh-ch_matrix_mapRect.png)

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| dst | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 目标矩形对象，用于存储源矩形经矩阵变换后的图形的外接矩形。 |
| src |[common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 源矩形对象。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回源矩形经过矩阵变换后的图形是否仍然是矩形，true表示是矩形，false表示不是矩形。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing,common2D} from "@kit.ArkGraphics2D";

let dst: common2D.Rect = { left: 100, top: 20, right: 130, bottom: 60 };
let src: common2D.Rect = { left: 100, top: 80, right: 130, bottom: 120 };
let matrix = new drawing.Matrix();
if (matrix.mapRect(dst, src)) {
    console.info("matrix= dst "+JSON.stringify(dst));
}
```

### setRectToRect<sup>20+</sup>

setRectToRect(src: common2D.Rect, dst: common2D.Rect, scaleToFit: ScaleToFit): boolean

将当前矩阵设置为能使源矩形映射到目标矩形的变换矩阵。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| src | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 源矩形。 |
| dst | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 目标矩形。 |
| scaleToFit | [ScaleToFit](#scaletofit20) | 是   | 源矩形到目标矩形的映射方式。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回矩阵是否可以表示矩形之间的映射，true表示可以，false表示不可以。如果源矩形的宽高任意一个小于等于0，则返回false，并将矩阵设置为单位矩阵；如果目标矩形的宽高任意一个小于等于0，则返回true，并将矩阵设置为除透视缩放系数为1外其余值皆为0的矩阵。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import {drawing,common2D} from "@kit.ArkGraphics2D";

let src: common2D.Rect = { left: 100, top: 100, right: 300, bottom: 300 };
let dst: common2D.Rect = { left: 200, top: 200, right: 600, bottom: 600 };
let scaleToFit: drawing.ScaleToFit = drawing.ScaleToFit.FILL_SCALE_TO_FIT
let matrix = new drawing.Matrix();
if (matrix.setRectToRect(src, dst, scaleToFit)) {
    console.info("matrix"+matrix.getAll().toString());
}
```

### setPolyToPoly<sup>20+</sup>

setPolyToPoly(src: Array\<common2D.Point>, dst: Array\<common2D.Point>, count: number): boolean

将当前矩阵设置为能够将源点数组映射到目标点数组的变换矩阵。源点和目标点的个数必须大于等于0，小于等于4。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名          | 类型    | 必填 | 说明                                                        |
| --------------- | ------- | ---- | ----------------------------------------------------------- |
| src | Array\<[common2D.Point](js-apis-graphics-common2D.md#point20)> | 是   | 源点数组，长度必须为count。 |
| dst | Array\<[common2D.Point](js-apis-graphics-common2D.md#point20)> | 是   | 目标点数组，长度必须为count。 |
| count | number | 是   | 在src和dst点的数量，该参数为整数。 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| boolean | 返回设置矩阵是否成功的结果，true表示设置成功，false表示设置失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import {drawing,common2D} from "@kit.ArkGraphics2D";

let srcPoints: Array<common2D.Point> = [ {x: 10, y: 20}, {x: 200, y: 150} ];
let dstPoints: Array<common2D.Point> = [{ x:0, y: 10 }, { x:300, y: 600 }];
let matrix = new drawing.Matrix();
if (matrix.setPolyToPoly(srcPoints, dstPoints, 2)) {
    console.info("matrix"+matrix.getAll().toString());
}
```

## RoundRect<sup>20+</sup>

圆角矩形对象。

### constructor<sup>20+</sup>

constructor(roundRect: RoundRect)

拷贝一个圆角矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| roundRect        | [RoundRect](#roundrect20) | 是    |  用于拷贝的圆角矩形。   |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let rect: common2D.Rect = {left : 100, top : 100, right : 500, bottom : 300};
let roundRect = new drawing.RoundRect(rect, 50, 50);
let roundRect2 = new drawing.RoundRect(roundRect);
```

### constructor<sup>20+</sup>

constructor(rect: common2D.Rect, xRadii: number, yRadii: number)

构造一个圆角矩形对象，当且仅当xRadii和yRadii均大于0时，圆角生效，否则只会构造一个矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| rect        | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是    | 需要创建的圆角矩形区域。      |
| xRadii        | number                  | 是    | X轴上的圆角半径，该参数为浮点数，小于等于0时无效。     |
| yRadii        | number                  | 是    | Y轴上的圆角半径，该参数为浮点数，小于等于0时无效。     |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';

let rect: common2D.Rect = {left : 100, top : 100, right : 500, bottom : 300};
let roundRect = new drawing.RoundRect(rect, 50, 50);
```
### setCorner<sup>20+</sup>

setCorner(pos: CornerPos, x: number, y: number): void

设置圆角矩形中指定圆角位置的圆角半径。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| pos | [CornerPos](#cornerpos20) | 是   | 圆角位置。                 |
| x     | number                 | 是   | x轴方向的圆角半径，该参数为浮点数，小于等于0时无效。 |
| y     | number      | 是   | y轴方向的圆角半径，该参数为浮点数，小于等于0时无效。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let roundRect : drawing.RoundRect = new drawing.RoundRect({left: 0, top: 0, right: 300, bottom: 300}, 50, 50);
roundRect.setCorner(drawing.CornerPos.TOP_LEFT_POS, 150, 150);
```

### getCorner<sup>20+</sup>

getCorner(pos: CornerPos): common2D.Point

获取圆角矩形中指定圆角位置的圆角半径。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| pos | [CornerPos](#cornerpos20) | 是   | 圆角位置。                 |

**返回值：**

| 类型                  | 说明           |
| --------------------- | -------------- |
| [common2D.Point](js-apis-graphics-common2D.md#point20)  | 返回一个点，其横坐标表示圆角x轴方向上的半径，纵坐标表示y轴方向上的半径。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let roundRect : drawing.RoundRect = new drawing.RoundRect({left: 0, top: 0, right: 300, bottom: 300}, 50, 50);
let cornerRadius = roundRect.getCorner(drawing.CornerPos.BOTTOM_LEFT_POS);
console.info("getCorner---"+cornerRadius.x)
console.info("getCorner---"+cornerRadius.y)
```

### offset<sup>20+</sup>

offset(dx: number, dy: number): void

将圆角矩形分别沿x轴方向和y轴方向平移dx,dy。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名   | 类型                                         | 必填 | 说明                            |
| -------- | -------------------------------------------- | ---- | ------------------------------- |
| dx | number | 是   | 表示x轴方向上的偏移量。正数表示向x轴正方向平移，负数表示向x轴负方向平移，该参数为浮点数。                 |
| dy | number | 是   | 表示y轴方向上的偏移量。正数表示向y轴正方向平移，负数表示向y轴负方向平移，该参数为浮点数。                 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let roundRect : drawing.RoundRect = new drawing.RoundRect({left: 0, top: 0, right: 300, bottom: 300}, 50, 50);
roundRect.offset(100, 100);
```

## Region<sup>20+</sup>

区域对象，用于描述所绘制图形的区域信息。

### constructor<sup>20+</sup>

constructor()

构造一个区域对象。

**系统能力：** SystemCapability.Graphics.Drawing

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';
class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    region.setRect(200, 200, 400, 400);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### constructor<sup>20+</sup>

constructor(region: Region)

拷贝一个区域对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| region     | [Region](#region20) | 是   | 用于拷贝的区域。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';
class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    region.setRect(200, 200, 400, 400);
    let region2 = new drawing.Region(region);
    canvas.drawRegion(region2);
    canvas.detachPen();
  }
}
```

### constructor<sup>20+</sup>

constructor(left: number, top: number, right: number, bottom: number)

构造矩形区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| left   | number | 是   | 矩形区域的左侧位置（矩形左上角横坐标）。该参数必须为整数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。|
| top    | number | 是   | 矩形区域的顶部位置（矩形左上角纵坐标）。该参数必须为整数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |
| right  | number | 是   | 矩形区域的右侧位置（矩形右下角横坐标）。该参数必须为整数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| bottom | number | 是   | 矩形区域的底部位置（矩形右下角纵坐标）。该参数必须为整数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';
class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region(100, 100, 200, 200);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### isEqual<sup>20+</sup>

isEqual(other: Region): boolean

用于判断其他区域是否与当前区域相等。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| other      | [Region](#region20) | 是   | 区域对象。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回其他区域是否与当前区域相等的结果。true表示相等，false表示不相等。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';
class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    let other = new drawing.Region();
    region.setRect(100, 100, 400, 400);
    other.setRect(150, 150, 250 ,250);
    let flag: boolean = false;
    flag = region.isEqual(other);
    console.info('flag: ', flag);
    canvas.drawRegion(region);
    canvas.drawRegion(other);
    canvas.detachPen();
  }
}
```

### isComplex<sup>20+</sup>

isComplex(): boolean

判断当前区域是否包含多个矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回当前区域是否包含多个矩形的结果。true表示当前区域包含多个矩形，false表示当前区域不包含多个矩形。 |

**示例：**

```ts
import { common2D, drawing } from '@kit.ArkGraphics2D';
import { RenderNode } from '@kit.ArkUI';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    let other = new drawing.Region();
    region.setRect(100, 100, 200, 200);
    region.op(new drawing.Region(220, 200, 280, 280), drawing.RegionOp.UNION);
    let flag: boolean = false;
    flag = region.isComplex();
    console.info('flag :', flag);
    canvas.drawRegion(region);
    canvas.drawRegion(other);
    canvas.detachPen();
  }
}
```

### isEmpty<sup>20+</sup>

isEmpty(): boolean

判断当前区域是否为空。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明                    |
| ------- | --------------         |
| boolean | 返回当前区域是否为空。true表示当前区域为空，false表示当前区域不为空。   |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    let flag: boolean = region.isEmpty();
    console.info('flag: ', flag);
    region.setRect(100, 100, 400, 400);
    flag = region.isEmpty();
    console.info('flag: ', flag);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### getBounds<sup>20+</sup>

getBounds(): common2D.Rect

获取区域的边界。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| [common2D.Rect](js-apis-graphics-common2D.md#rect) | 返回当前区域的边界矩形。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let region = new drawing.Region();
let rect = region.getBounds();
```

### getBoundaryPath<sup>20+</sup>

getBoundaryPath(): Path

返回一个新路径，该路径取自当前区域的边界。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| [Path](#path)  | 返回当前区域边界的路径。 |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';
let region = new drawing.Region();
let path = region.getBoundaryPath();
```

### isPointContained<sup>20+</sup>

isPointContained(x: number, y: number) : boolean

判断测试点是否在区域内。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| x      | number | 是   | 测试点的x轴坐标。该参数必须为整数。如果输入的数字包含小数部分，小数部分将被舍去。 |
| y      | number | 是   | 测试点的y轴坐标。该参数必须为整数。如果输入的数字包含小数部分，小数部分将被舍去。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回测试点是否在区域内的结果。true表示测试点在区域内，false表示测试点不在区域内。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    region.setRect(100, 100, 400, 400);
    let flag: boolean = false;
    flag = region.isPointContained(200,200);
    console.info("region isPointContained : " + flag);
    canvas.drawPoint(200,200);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### offset<sup>20+</sup>

offset(dx: number, dy: number): void

对区域进行平移。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| dx      | number | 是   | x轴方向平移量，正数往x轴正方向平移，负数往x轴负方向平移，该参数为整数。 |
| dy      | number | 是   | y轴方向平移量，正数往y轴正方向平移，负数往y轴负方向平移，该参数为整数。|

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';
class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    region.setRect(100, 100, 400, 400);
    region.offset(10, 20);
    canvas.drawPoint(200,200);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### isRegionContained<sup>20+</sup>

isRegionContained(other: Region) : boolean

判断其他区域是否在当前区域内。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| other      | [Region](#region20) | 是   | 区域对象。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回其他区域是否在当前区域内的结果。true表示其他区域在当前区域内，false表示其他区域不在当前区域内。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    let other = new drawing.Region();
    region.setRect(100, 100, 400, 400);
    other.setRect(150, 150, 250 ,250);
    let flag: boolean = false;
    flag = region.isRegionContained(other);
    console.info("region isRegionContained : " + flag);
    canvas.drawRegion(region);
    canvas.drawRegion(other);
    canvas.detachPen();
  }
}
```

### op<sup>20+</sup>

op(region: Region, regionOp: RegionOp) : boolean

将当前区域与指定区域进行运算，并替换为运算结果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| region      | [Region](#region20) | 是   | 区域对象。 |
| regionOp      | [RegionOp](#regionop20) | 是   | 区域合并操作类型。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回区域运算结果是否成功替换当前区域。true表示区域运算结果替换当前区域成功，false表示区域运算结果替换当前区域失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    region.setRect(200, 200, 400, 400);
    let othregion = new drawing.Region();
    othregion.setRect(110, 110, 240, 240);
    let flag: boolean = false;
    flag = region.op(othregion,drawing.RegionOp.REPLACE);
    console.info("region op : " + flag);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### quickReject<sup>20+</sup>

quickReject(left: number, top: number, right: number, bottom: number) : boolean

快速判断矩形和区域是否不相交，实际上比较的是矩形和区域的外接矩形是否不相交，因此会有误差。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| left   | number | 是   | 矩形区域的左侧位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |
| top    | number | 是   | 矩形区域的顶部位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |
| right  | number | 是   | 矩形区域的右侧位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |
| bottom | number | 是   | 矩形区域的底部位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回矩形是否与区域不相交的结果。true表示矩形与区域不相交，false表示矩形与区域相交。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    region.setRect(100, 100, 400, 400);
    let flag: boolean = false;
    flag = region.quickReject(50, 50, 70, 70);
    console.info("region quickReject : " + flag);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### quickRejectRegion<sup>20+</sup>

quickRejectRegion(region: Region): boolean

判断当前区域是否与另一个区域不相交。实际上比较的是两个区域的外接矩形是否不相交，因此会有误差。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| region      | [Region](#region20) | 是   | 指定的区域对象。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回是否当前区域与另外的区域不相交的结果。true表示不相交，false表示相交。仅点和边相交返回true。|

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    let region2 = new drawing.Region();
    region2.setRect(100, 100, 400, 400);
    let flag: boolean = false;
    flag = region.quickRejectRegion(region2);
    console.info("region quickRejectRegion: " + flag);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### setPath<sup>20+</sup>

setPath(path: Path, clip: Region) : boolean

设置一个与裁剪区域内路径轮廓相匹配的区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| path      | [Path](#path) | 是   | 路径对象。 |
| clip      | [Region](#region20) | 是   | 区域对象。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回设置一个与裁剪区域内路径轮廓相匹配的区域是否成功。true表示设置成功，false表示设置失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    let path = new drawing.Path();
    region.setRect(100, 100, 400, 400);
    path.arcTo(50, 50, 300, 300, 0, 359);
    let flag: boolean = false;
    flag = region.setPath(path,region);
    console.info("region setPath : " + flag);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

### setRegion<sup>20+</sup>

setRegion(region: Region): void

设置当前区域为另一块区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| region      | [Region](#region20) | 是   | 用于赋值的区域。 |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    region.setRect(100, 100, 200, 200);
    let region2 = new drawing.Region();
    region2.setRegion(region);
    canvas.drawRegion(region2);
    canvas.detachPen();
  }
}
```

### setEmpty<sup>20+</sup>

setEmpty(): void

设置当前区域为空。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    let region = new drawing.Region();
    region.setRect(100, 100, 200, 200);
    let isEmpty = region.isEmpty();
    console.info("isEmpty :" + isEmpty);
    region.setEmpty();
    isEmpty = region.isEmpty();
    console.info("isEmpty :" + isEmpty);
  }
}
```

### setRect<sup>20+</sup>

setRect(left: number, top: number, right: number, bottom: number) : boolean

设置一个矩形区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                    |
| ------ | ------ | ---- | ----------------------- |
| left   | number | 是   | 矩形区域的左侧位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |
| top    | number | 是   | 矩形区域的顶部位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |
| right  | number | 是   | 矩形区域的右侧位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |
| bottom | number | 是   | 矩形区域的底部位置。该参数必须为整数。当输入的数字带小数时，小数部分会被舍去。 |

**返回值：**

| 类型    | 说明           |
| ------- | -------------- |
| boolean | 返回设置矩形区域是否成功的结果。true表示设置矩形区域成功，false表示设置矩形区域失败。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { drawing } from '@kit.ArkGraphics2D';

class DrawingRenderNode extends RenderNode {
  draw(context : DrawContext) {
    const canvas = context.canvas;
    const pen = new drawing.Pen();
    pen.setColor({ alpha: 255, red: 255, green: 0, blue: 0 });
    pen.setStrokeWidth(10);
    canvas.attachPen(pen);
    let region = new drawing.Region();
    let flag: boolean = false;
    flag = region.setRect(50, 50, 300, 300);
    console.info("region setRect : " + flag);
    canvas.drawRegion(region);
    canvas.detachPen();
  }
}
```

## TileMode<sup>20+</sup>

着色器效果平铺模式的枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                   | 值   | 说明                           |
| ---------------------- | ---- | ------------------------------ |
| CLAMP     | 0    | 如果着色器效果超出其原始边界，剩余区域使用着色器的边缘颜色填充。 |
| REPEAT    | 1    | 在水平和垂直方向上重复着色器效果。 |
| MIRROR    | 2    | 在水平和垂直方向上重复着色器效果，交替镜像图像，以便相邻图像始终接合。 |
| DECAL     | 3    | 仅在其原始边界内渲染着色器效果。|

## ShaderEffect<sup>20+</sup>

着色器。画刷和画笔设置着色器后，会使用着色器效果而不是颜色属性去绘制，但此时画笔和画刷的透明度属性仍然生效。

### createComposeShader<sup>20+</sup>

static createComposeShader(dstShaderEffect: ShaderEffect, srcShaderEffect: ShaderEffect, blendMode: BlendMode): ShaderEffect

按照指定的混合模式对两个着色器进行叠加，生成一个新的着色器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| dstShaderEffect  | [ShaderEffect](#shadereffect20) | 是   | 在混合模式中作为目标色的着色器。 |
| srcShaderEffect  | [ShaderEffect](#shadereffect20) | 是   | 在混合模式中作为源色的着色器。   |
| blendMode  | [BlendMode](#blendmode) | 是   | 混合模式。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [ShaderEffect](#shadereffect20) | 返回创建的着色器对象。 |

**错误码：**

以下错误码的详细介绍请参见[图形绘制与显示错误码](../apis-arkgraphics2d/errorcode-drawing.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 25900001 | Parameter error.Possible causes: Incorrect parameter range. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let dstShader = drawing.ShaderEffect.createColorShader(0xFF0000FF);
let srcShader = drawing.ShaderEffect.createColorShader(0xFFFF0000);
let shader = drawing.ShaderEffect.createComposeShader(dstShader, srcShader, drawing.BlendMode.SRC);
```

### createImageShader<sup>20+</sup>

static createImageShader(pixelmap: image.PixelMap, tileX: TileMode, tileY: TileMode, samplingOptions: SamplingOptions, matrix?: Matrix | null): ShaderEffect

基于图片创建一个着色器。此接口不建议用于录制类型的画布，会影响性能。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| pixelmap  | [image.PixelMap](../../../application-dev/reference/apis/js-apis-image.md#pixelmap7)  | 是   | 进行采样的图片对象。 |
| tileX   | [TileMode](#tilemode20)  | 是   | 水平方向的平铺模式。 |
| tileY   | [TileMode](#tilemode20)  | 是   | 竖直方向的平铺模式。 |
| samplingOptions     | [SamplingOptions](#samplingoptions20)                           | 是   | 图片采样参数。 |
| matrix | [Matrix](#matrix20) \| null | 否   | 可选参数，对图片施加的矩阵变换，如果为空，则不施加任何变换。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [ShaderEffect](#shadereffect20) | 返回创建的着色器对象。 |

**错误码：**

以下错误码的详细介绍请参见[图形绘制与显示错误码](../apis-arkgraphics2d/errorcode-drawing.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 25900001 | Parameter error.Possible causes: Incorrect parameter range. |

**示例：**

```ts
import { RenderNode } from '@kit.ArkUI';
import { image } from '@kit.ImageKit';
import { drawing } from '@kit.ArkGraphics2D';
class DrawingRenderNode extends RenderNode {
  pixelMap: image.PixelMap | null = null;

  async draw(context : DrawContext) {
    let matrix = new drawing.Matrix();
    let options = new drawing.SamplingOptions(drawing.FilterMode.FILTER_MODE_NEAREST);
    if (this.pixelMap != null) {
      let imageShader = drawing.ShaderEffect.createImageShader(this.pixelMap, drawing.TileMode.REPEAT, drawing.TileMode.MIRROR, options, matrix);
    }
  }
}
```

### createColorShader<sup>20+</sup>

static createColorShader(color: number): ShaderEffect

创建具有单一颜色的着色器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| color   | number | 是   | 表示着色器的ARGB格式颜色，该参数为32位无符号整数。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [ShaderEffect](#shadereffect20) | 返回具有单一颜色的着色器对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from '@kit.ArkGraphics2D';

let shaderEffect = drawing.ShaderEffect.createColorShader(0xFFFF0000);
```

### createLinearGradient<sup>20+</sup>

static createLinearGradient(startPt: common2D.Point, endPt: common2D.Point, colors: Array
\<number>, mode: TileMode, pos?: Array\<number> | null, matrix?: Matrix | null): ShaderEffect

创建着色器，在两个指定点之间生成线性渐变。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| startPt  | [common2D.Point](js-apis-graphics-common2D.md#point20)  | 是   | 表示渐变的起点。 |
| endPt   | [common2D.Point](js-apis-graphics-common2D.md#point20)  | 是   | 表示渐变的终点。 |
| colors | Array\<number> | 是   | 表示在两个点之间分布的颜色数组，数组中的值为32位（ARGB）无符号整数。 |
| mode  | [TileMode](#tilemode20) | 是   | 着色器效果平铺模式。 |
| pos | Array\<number> \|null| 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在起点和终点之间。 |
| matrix | [Matrix](#matrix20) \| null | 否   | 矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![LinearGradient](./figures/zh-ch_image_createLinearGradient.png)

如上图所示，设置颜色数组为红绿蓝，位置数组为0.0、0.75和1.0后的显示效果。三角下标表示对应颜色的起始点和终点之间的相对位置，颜色之间使用渐变填充。

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [ShaderEffect](#shadereffect20) | 返回创建的着色器对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { common2D,drawing } from '@kit.ArkGraphics2D';

let startPt: common2D.Point = { x: 100, y: 100 };
let endPt: common2D.Point = { x: 300, y: 300 };
let shaderEffect = drawing.ShaderEffect.createLinearGradient(startPt, endPt, [0xFF00FF00, 0xFFFF0000], drawing.TileMode.REPEAT);
```

### createRadialGradient<sup>20+</sup>

static createRadialGradient(centerPt: common2D.Point, radius: number, colors: Array\<number>, mode: TileMode, pos?: Array\<number> | null, matrix?: Matrix | null): ShaderEffect;

创建着色器，使用给定的圆心和半径生成径向渐变。径向渐变是指颜色从圆心逐渐向外扩散形成的渐变。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| centerPt  | [common2D.Point](js-apis-graphics-common2D.md#point20)  | 是   | 表示渐变的圆心。 |
| radius   | number  | 是   | 表示渐变的半径，小于等于0时无效，该参数为浮点数。 |
| colors | Array\<number> | 是   | 表示在圆心和圆边界之间分布的颜色数组，数组中的值为32位（ARGB）无符号整数。 |
| mode  | [TileMode](#tilemode20) | 是   | 着色器效果平铺模式。 |
| pos | Array\<number> \| null | 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在圆心和圆边界之间。 |
| matrix | [Matrix](#matrix20) \| null | 否   | 矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![RadialGradient](./figures/zh-ch_image_createRadialGradient.png)

如上图所示，设置颜色数组为红绿蓝，位置数组为0.0、0.75和1.0后的显示效果。三角下标表示对应颜色所在圆心和圆边界之间的相对位置，颜色之间使用渐变填充。

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [ShaderEffect](#shadereffect20) | 返回创建的着色器对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { common2D,drawing } from '@kit.ArkGraphics2D';

let centerPt: common2D.Point = { x: 100, y: 100 };
let shaderEffect = drawing.ShaderEffect.createRadialGradient(centerPt, 100, [0xFF00FF00, 0xFFFF0000], drawing.TileMode.REPEAT);
```

### createSweepGradient<sup>20+</sup>

static createSweepGradient(centerPt: common2D.Point, colors: Array\<number>,
  mode: TileMode, startAngle: number, endAngle: number, pos?: Array\<number> | null,
  matrix?: Matrix | null): ShaderEffect;

创建着色器。该着色器以给定中心点为圆心，在顺时针或逆时针方向上生成颜色扫描渐变。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| centerPt  | [common2D.Point](js-apis-graphics-common2D.md#point20)  | 是   | 表示渐变的圆心。 |
| colors | Array\<number> | 是   | 表示在起始角度和结束角度之间分布的颜色数组，数组中的值为32位（ARGB）无符号整数。 |
| mode  | [TileMode](#tilemode20) | 是   | 着色器效果平铺模式。 |
| startAngle | number | 是   | 表示扇形渐变的起始角度，单位为度。0度时为x轴正方向，正数往顺时针方向偏移，负数往逆时针方向偏移。该参数为浮点数。 |
| endAngle | number | 是   | 表示扇形渐变的结束角度，单位为度。0度时为x轴正方向，正数往顺时针方向偏移，负数往逆时针方向偏移。小于起始角度时无效。该参数为浮点数。 |
| pos | Array\<number> \| null | 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在起始角度和结束角度之间。 |
| matrix | [Matrix](#matrix20) \| null | 否   |矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![SweepGradient](./figures/zh-ch_image_createSweepGradient.png)

如上图所示，设置颜色数组为红绿蓝，位置数组为0.0、0.75和1.0，起始角度设置为0度，结束角度设置为180度后的显示效果。0.0对应0度的位置，0.75对应135度的位置，1.0对应180度的位置，颜色之间使用渐变填充。

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [ShaderEffect](#shadereffect20) | 返回创建的着色器对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { common2D,drawing } from '@kit.ArkGraphics2D';

let centerPt: common2D.Point = { x: 100, y: 100 };
let shaderEffect = drawing.ShaderEffect.createSweepGradient(centerPt, [0xFF00FF00, 0xFFFF0000], drawing.TileMode.REPEAT, 100, 200);
```

### createConicalGradient<sup>20+</sup>

static createConicalGradient(startPt: common2D.Point, startRadius: number, endPt: common2D.Point, endRadius: number, colors: Array\<number>, mode: TileMode, 
pos?: Array\<number> | null, matrix?: Matrix | null): ShaderEffect;

创建着色器，在给定两个圆之间生成径向渐变。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                               | 必填 | 说明           |
| ------ | -------------------------------------------------- | ---- | -------------- |
| startPt  | [common2D.Point](js-apis-graphics-common2D.md#point20)  | 是   |表示渐变的起始圆的圆心。 |
| startRadius | number | 是   | 表示渐变的起始圆的半径，小于0时无效。该参数为浮点数。 |
| endPt  | [common2D.Point](js-apis-graphics-common2D.md#point20)  | 是   | 表示渐变的结束圆的圆心。 |
| endRadius | number | 是   | 表示渐变的结束圆的半径，小于0时无效。该参数为浮点数。 |
| colors | Array\<number> | 是   | 表示在起始圆和结束圆之间分布的颜色数组，数组中的值为32位（ARGB）无符号整数。 |
| mode  | [TileMode](#tilemode20) | 是   | 着色器效果平铺模式。 |
| pos | Array\<number> \| null | 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在起始圆和结束圆之间。|
| matrix | [Matrix](#matrix20) \| null | 否   | 矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![ConicalGradient](./figures/zh-ch_image_createConicalGradient.png)

如上图所示，设置颜色数组为红绿蓝，位置数组为0.0、0.5和1.0的绘制结果。左侧为起始圆不在结束圆内的绘制结果，右侧为起始圆在结束圆内的绘制结果。

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [ShaderEffect](#shadereffect20) | 返回创建的着色器对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码ID | 错误信息 |
| ------- | --------------------------------------------|
| 401 | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { common2D,drawing } from '@kit.ArkGraphics2D';

let startPt: common2D.Point = { x: 100, y: 100 };
let endPt: common2D.Point = {x: 200, y: 200};
let shaderEffect = drawing.ShaderEffect.createConicalGradient(startPt, 100, endPt, 50, [0xFF00FF00, 0xFFFF0000], drawing.TileMode.REPEAT);
```

## Tool<sup>20+</sup>

本模块定义的工具类，仅提供静态的方法，主要完成其他模块和[common2D](js-apis-graphics-common2D.md)中定义的数据结构的转换功能等操作。

## RegionOp<sup>20+</sup>

两个区域合并时的操作的枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                   | 值   | 说明                           | 示意图   |
| --------------------- | ---- | ------------------------------ | -------- |
| DIFFERENCE         | 0    | 两个区域的相减操作。  | ![CLEAR](./figures/zh-ch_image_RegionOp_Difference.png) |
| INTERSECT          | 1    | 两个区域的相交操作。 | ![INTERSECT](./figures/zh-ch_image_RegionOp_Intersect.png) |
| UNION              | 2    | 两个区域的联合操作。   | ![UNION](./figures/zh-ch_image_RegionOpe_Union.png) |
| XOR                | 3    | 两个区域的异或操作。   | ![XOR](./figures/zh-ch_image_RegionOp_Xor.png) |
| REVERSE_DIFFERENCE | 4    | 两个区域的反向相减操作。   | ![REVERSE_DIFFERENCE](./figures/zh-ch_image_RegionOp_Reverse_difference.png) |
| REPLACE            | 5    | 两个区域替换操作。   | ![REPLACE](./figures/zh-ch_image_RegionOp_Replace.png) |

> **说明：**
>
> 示意图展示了一个以红色区域为基础，使用不同枚举值与另一个蓝色区域合并后获得的结果，其中绿色区域为最终得到的区域。

## CornerPos<sup>20+</sup>

圆角位置枚举。

**系统能力：** SystemCapability.Graphics.Drawing

| 名称                   | 值   | 说明                           |
| --------------------- | ---- | ------------------------------ | 
| TOP_LEFT_POS          | 0    | 左上角圆角位置。  |
| TOP_RIGHT_POS         | 1    | 右上角圆角位置。 |
| BOTTOM_RIGHT_POS      | 2    | 右下角圆角位置。   |
| BOTTOM_LEFT_POS       | 3    | 左下角圆角位置。   |

## RectUtils<sup>20+</sup>

提供了处理矩形的工具。

主要的使用场景：

1. 矩形快速构建与获取基本属性，如构造新矩形、拷贝矩形、获取矩形的宽高以及中心点等。

2. 边界计算与调整，如获取包含关系、计算与更新矩形之间交集和并集，更新边界值等。

### makeEmpty<sup>20+</sup>

static makeEmpty(): common2D.Rect

创建一个上下左右边界坐标都是0的矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [common2D.Rect](js-apis-graphics-common2D.md#rect) | 创建的矩形对象。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';

let rect = drawing.RectUtils.makeEmpty();
```

### makeLtrb<sup>20+</sup>

static makeLtrb(left: number, top: number, right: number, bottom: number): common2D.Rect

创建指定上下左右边界的矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| left   | number | 是   | 矩形的左上角x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| top    | number | 是   | 矩形的左上角y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |
| right  | number | 是   | 矩形的右下角x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| bottom | number | 是   | 矩形的右下角y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [common2D.Rect](js-apis-graphics-common2D.md#rect) | 创建的矩形。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';

let rect = drawing.RectUtils.makeLtrb(10, 10, 20, 20);
```

### makeCopy<sup>20+</sup>

static makeCopy(src: common2D.Rect): common2D.Rect;

拷贝一个矩形。

**系统能力**：SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| src   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于拷贝的矩形。 |


**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| [common2D.Rect](js-apis-graphics-common2D.md#rect) | 创建的新矩形。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 10, 20, 20);
let rect2 = drawing.RectUtils.makeCopy(rect);
console.info('rect2.left:', rect2.left);
console.info('rect2.top: ', rect2.top);
console.info('rect2.right: ', rect2.right);
console.info('rect2.bottom: ', rect2.bottom);
```

### getWidth<sup>20+</sup>

static getWidth(rect: common2D.Rect): number

获取矩形的宽度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |


**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| number | 返回矩形的宽。如果矩形的左边界大于右边界，获取的宽度为负值，左边界小于右边界则为正值。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 10, 20, 20);
let width = drawing.RectUtils.getWidth(rect);
console.info('width ：', width);
```

### getHeight<sup>20+</sup>

static getHeight(rect: common2D.Rect): number

获取矩形的高度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| number | 返回矩形的高。如果矩形的上边界大于下边界，获取的高度为负值，上边界小于下边界则为正值。|

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 10, 20, 20);
let height = drawing.RectUtils.getHeight(rect);
```

### centerX<sup>20+</sup>

static centerX(rect: common2D.Rect): number

获取矩形中心的横坐标。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| number | 返回矩形中心的横坐标。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(20, 30, 30, 40);
let x = drawing.RectUtils.centerX(rect);
```

### centerY<sup>20+</sup>

static centerY(rect: common2D.Rect): number

获取矩形中心的纵坐标。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| number | 返回矩形中心的纵坐标。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(20, 30, 30, 40);
let x = drawing.RectUtils.centerY(rect);
```

### contains<sup>20+</sup>

static contains(rect: common2D.Rect, other: common2D.Rect): boolean

判断一个矩形是否完全包含另外一个矩形。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |
| other   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 判断是否被包含的矩形对象。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean | 返回矩形是否完全包含另一个矩形的结果。true表示指定矩形在另一个矩形内部或者相等，false表示指定矩形在另一个矩形外部。空的矩形不包含任何矩形。|

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 10, 20, 20);
let rect2 = drawing.RectUtils.makeLtrb(0, 0, 40, 40);
let isContains = drawing.RectUtils.contains(rect2, rect);
console.info('isContains: ', isContains);
```

### contains<sup>20+</sup>

static contains(rect: common2D.Rect, left: number, top: number, right: number, bottom: number): boolean

判断一个矩形是否完全包含另外一个矩形（另一个矩形分别用左上右下坐标表示）。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |
| left   | number | 是   | 矩形的左上角x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| top    | number | 是   | 矩形的左上角y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |
| right  | number | 是   | 矩形的右下角x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| bottom | number | 是   | 矩形的右下角y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean | 返回矩形是否完全包含由左上右下坐标组成的矩形的结果。true表示指定左上右下坐标组成的矩形在矩形的内部或者相等，false表示指定左上右下坐标组成的矩形在矩形的外部。空的矩形不包含任何矩形。|

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(0, 0, 100, 100);
let isContains = drawing.RectUtils.contains(rect, 10, 20, 30, 40);
console.info('isContains :', isContains);
```

### contains<sup>20+</sup>

static contains(rect: common2D.Rect, x: number, y: number): boolean

判断一个矩形是否完全包含一个点。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |
| x   | number | 是   | 要判断点的x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| y    | number | 是  | 要判断点的y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean | 返回矩形是否完全包含x、y组成的点的结果。true表示矩形完全包含x、y组成的点，false表示矩形不完全包含x、y组成的点。左边界和上边界属于矩形内部，右边界和下边界不属于矩形内部。空的矩形不包含任何点。|

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(0, 0, 100, 100);
let isContains = drawing.RectUtils.contains(rect, 10, 20);
console.info('isContains: ', isContains);
```

### inset<sup>20+</sup>

static inset(rect: common2D.Rect, left: number, top: number, right: number, bottom: number): void

将指定矩形的左边界、上边界、右边界和下边界分别和传入的"左上右下"的值相加。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |
| left   | number | 是   | 添加到矩形左边界的值（矩形左上角横坐标），该参数为浮点数。0表示不进行任何运算，正数表示进行相加运算，负数表示相减运算。 |
| top    | number | 是   | 添加到矩形上边界的值（矩形左上角纵坐标），该参数为浮点数。0表示不进行任何运算，正数表示进行相加运算，负数表示相减运算。 |
| right  | number | 是   | 添加到矩形右边界的值（矩形右下角横坐标），该参数为浮点数。0表示不进行任何运算，正数表示进行相加运算，负数表示相减运算。 |
| bottom | number | 是   | 添加到矩形下边界的值（矩形右下角纵坐标），该参数为浮点数。0表示不进行任何运算，正数表示进行相加运算，负数表示相减运算。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 10, 20, 20);
drawing.RectUtils.inset(rect, 10, -20, 30, 60);
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### intersect<sup>20+</sup>

static intersect(rect: common2D.Rect, other: common2D.Rect): boolean

计算两个矩形的交集区域，并将交集结果更新到第一个入参代表的矩形区域。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于计算交集的原矩形。 |
| other   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是  | 用于计算交集的另一个矩形。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean |  返回两个矩形是否有交集的结果。true表示两个矩形有交集，false表示两个矩形没有交集。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(0, 0, 20, 20);
let rect2 = drawing.RectUtils.makeLtrb(10, 10, 40, 40);
let isIntersect = drawing.RectUtils.intersect(rect, rect2);
console.info('isIntersect :', isIntersect);
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### isIntersect<sup>20+</sup>

static isIntersect(rect: common2D.Rect, other: common2D.Rect): boolean

判断两个矩形是否相交。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于计算交集的原矩形。 |
| other   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是  | 用于计算交集的另一个矩形。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean |  返回两个矩形是否有交集的结果。true表示指定矩形与原矩形相交，false表示指定矩形和原矩形没有交集。两矩形仅边重叠或点相交返回false。|

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(0, 0, 20, 20);
let rect2 = drawing.RectUtils.makeLtrb(10, 10, 40, 40);
let isIntersect = drawing.RectUtils.isIntersect(rect, rect2);
console.info('isIntersect :', isIntersect);
```

### union<sup>20+</sup>

static union(rect: common2D.Rect, other: common2D.Rect): void

计算矩形的并集区域，并将并集结果更新到第一个入参表示的矩形区域。如果第一个入参矩形为空，则将并集结果更新到第二个入参代表的矩形区域；如果第二个入参的矩形为空，则不进行任何操作。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于计算并集的原矩形。 |
| other   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是  | 用于计算并集的另一个矩形。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(0, 0, 20, 20);
let rect2 = drawing.RectUtils.makeLtrb(10, 10, 40, 40);
drawing.RectUtils.union(rect, rect2);
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### isEmpty<sup>20+</sup>

static isEmpty(rect: common2D.Rect): boolean

判断矩形是否为空（左边界大于等于右边界或者上边界大于等于下边界）。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明            |
| ------ | ------ | ---- | --------------  |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于判断的矩形对象。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean | 返回矩形是否为空的结果。true表示矩形是空，false表示矩形不为空。       |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeEmpty();
let isEmpty = drawing.RectUtils.isEmpty(rect);
console.info('isEmpty :', isEmpty);
let rect2 = drawing.RectUtils.makeLtrb(0, 0, 20, 20);
isEmpty = drawing.RectUtils.isEmpty(rect);
console.info('isEmpty :', isEmpty);
```

### offset<sup>20+</sup>

static offset(rect: common2D.Rect, dx: number, dy: number): void

对矩形进行平移。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 发生偏移的矩形区域。 |
| dx   | number | 是    | 水平方向平移的距离，该参数为浮点数。0表示不平移，负数表示向左平移，正数表示向右平移。 |
| dy    | number | 是   | 竖直方向平移的距离，该参数为浮点数。0表示不平移，负数表示向上平移，正数表示向右平移。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(0, 0, 20, 20);
drawing.RectUtils.offset(rect, 10, 20);
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### offsetTo<sup>20+</sup>

static offsetTo(rect: common2D.Rect, newLeft: number, newTop: number): void

将矩形平移到指定位置。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 发生偏移的矩形区域。 |
| newLeft   | number | 是   | 要平移到的对应位置的x轴坐标，浮点数。0表示坐标原点，负数位于坐标原点左侧，正数位于坐标原点右侧。 |
| newTop    | number | 是   | 要平移到的对应位置的y轴坐标，浮点数。0表示坐标原点，负数位于坐标原点上侧，正数位于坐标原点下侧。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(40, 40, 20, 20);
drawing.RectUtils.offsetTo(rect, 10, 20);
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### setRect<sup>20+</sup>

static setRect(rect: common2D.Rect, other: common2D.Rect): void

使用另一个矩形对当前矩形进行赋值。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   |  原矩形。 |
| other   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于赋值的矩形。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 20, 30, 40);
let rect2 = drawing.RectUtils.makeEmpty();
drawing.RectUtils.setRect(rect2, rect);
console.info('rect2.left:', rect2.left);
console.info('rect2.top: ', rect2.top);
console.info('rect2.right: ', rect2.right);
console.info('rect2.bottom: ', rect2.bottom);
```

### setLtrb<sup>20+</sup>

static setLtrb(rect: common2D.Rect, left: number, top: number, right: number, bottom: number): void

使用传入的"上下左右"的值更新当前矩形的上下左右边界值。

**系统能力**：SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 矩形对象。 |
| left   | number | 是   | 矩形的左上角x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| top    | number | 是   | 矩形的左上角y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |
| right  | number | 是   | 矩形的右下角x轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点左侧，正数表示位于坐标原点右侧。 |
| bottom | number | 是   | 矩形的右下角y轴坐标，该参数为浮点数。0表示坐标原点，负数表示位于坐标原点上侧，正数表示位于坐标原点下侧。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeEmpty();
drawing.RectUtils.setLtrb(rect, 10, 20, 30, 60);
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### setEmpty<sup>20+</sup>

static setEmpty(rect: common2D.Rect): void

将矩形的上下左右边界都设为0。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明            |
| ------ | ------ | ---- | --------------  |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于设置为空的矩形对象。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 20, 20, 30);
drawing.RectUtils.setEmpty(rect)
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### sort<sup>20+</sup>

static sort(rect: common2D.Rect): void

如果矩形存在反转的情况（即左边界大于右边界或上边界大于下边界），则对矩形的上下（左右）边界值进行交换，使得上边界小于下边界（左边界小于右边界）。

如果矩形不存在反转的情况（即左边界小于等于右边界或上边界小于等于下边界)，不做任何操作。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明            |
| ------ | ------ | ---- | --------------  |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 用于设置的矩形对象。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(20, 40, 30, 30);
drawing.RectUtils.sort(rect);
console.info('rect.left:', rect.left);
console.info('rect.top: ', rect.top);
console.info('rect.right: ', rect.right);
console.info('rect.bottom: ', rect.bottom);
```

### isEqual<sup>20+</sup>

static isEqual(rect: common2D.Rect, other: common2D.Rect): boolean

判断两个矩形是否相等。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明           |
| ------ | ------ | ---- | -------------- |
| rect   | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是    | 需要判断的原矩形。 |
| other  | [common2D.Rect](js-apis-graphics-common2D.md#rect) | 是   | 需要判断的另一矩形。 |

**返回值：**

| 类型    | 说明                       |
| ------- | ------------------------- |
| boolean | 返回两个矩形是否相等的结果。true表示两个矩形相等，false表示两个矩形不相等。 |

**示例：**

```ts
import { drawing, common2D } from '@kit.ArkGraphics2D';
let rect = drawing.RectUtils.makeLtrb(10, 20, 20, 30);
let rect2 = drawing.RectUtils.makeEmpty();
let isEqual = drawing.RectUtils.isEqual(rect, rect2);
console.info('isEqual :', isEqual);
```