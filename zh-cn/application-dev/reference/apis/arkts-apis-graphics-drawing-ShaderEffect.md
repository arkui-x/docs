# Class (ShaderEffect)

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
| blendMode  | [BlendMode](arkts-apis-graphics-drawing-e.md#blendmode) | 是   | 混合模式。 |

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
| tileX   | [TileMode](arkts-apis-graphics-drawing-e.md#tilemode20)  | 是   | 水平方向的平铺模式。 |
| tileY   | [TileMode](arkts-apis-graphics-drawing-e.md#tilemode20)  | 是   | 竖直方向的平铺模式。 |
| samplingOptions     | [SamplingOptions](arkts-apis-graphics-drawing-SamplingOptions.md)                           | 是   | 图片采样参数。 |
| matrix | [Matrix](arkts-apis-graphics-drawing-Matrix.md) \| null | 否   | 可选参数，对图片施加的矩阵变换，如果为空，则不施加任何变换。 |

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
| mode  | [TileMode](arkts-apis-graphics-drawing-e.md#tilemode20) | 是   | 着色器效果平铺模式。 |
| pos | Array\<number> \|null| 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在起点和终点之间。 |
| matrix | [Matrix](arkts-apis-graphics-drawing-Matrix.md) \| null | 否   | 矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![LinearGradient](figures/zh-ch_image_createLinearGradient.png)

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
| mode  | [TileMode](arkts-apis-graphics-drawing-e.md#tilemode20) | 是   | 着色器效果平铺模式。 |
| pos | Array\<number> \| null | 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在圆心和圆边界之间。 |
| matrix | [Matrix](arkts-apis-graphics-drawing-Matrix.md) \| null | 否   | 矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![RadialGradient](figures/zh-ch_image_createRadialGradient.png)

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
| mode  | [TileMode](arkts-apis-graphics-drawing-e.md#tilemode20) | 是   | 着色器效果平铺模式。 |
| startAngle | number | 是   | 表示扇形渐变的起始角度，单位为度。0度时为x轴正方向，正数往顺时针方向偏移，负数往逆时针方向偏移。该参数为浮点数。 |
| endAngle | number | 是   | 表示扇形渐变的结束角度，单位为度。0度时为x轴正方向，正数往顺时针方向偏移，负数往逆时针方向偏移。小于起始角度时无效。该参数为浮点数。 |
| pos | Array\<number> \| null | 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在起始角度和结束角度之间。 |
| matrix | [Matrix](arkts-apis-graphics-drawing-Matrix.md) \| null | 否   |矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![SweepGradient](figures/zh-ch_image_createSweepGradient.png)

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
| mode  | [TileMode](arkts-apis-graphics-drawing-e.md#tilemode20) | 是   | 着色器效果平铺模式。 |
| pos | Array\<number> \| null | 否   | 表示每种对应颜色在颜色数组中的相对位置。数组长度需和colors保持一致，数组的首个元素应当是0.0，末尾元素应当是1.0，中间的元素应当在0与1之间并且逐下标递增，表示colors中每个对应颜色的相对位置。默认为null，表示颜色均匀分布在起始圆和结束圆之间。|
| matrix | [Matrix](arkts-apis-graphics-drawing-Matrix.md) \| null | 否   | 矩阵对象，用于对着色器做矩阵变换。默认为null，表示单位矩阵。 |

![ConicalGradient](figures/zh-ch_image_createConicalGradient.png)

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