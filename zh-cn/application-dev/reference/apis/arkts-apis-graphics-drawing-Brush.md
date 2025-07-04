# Class (Brush)

> **说明：**
>
> - 本模块首批接口从 API version 20 开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> - 本模块使用屏幕物理像素单位 px。
>
> - 本模块为单线程模型策略，需要调用方自行管理线程安全和上下文状态的切换。

## 导入模块

```ts
import { drawing } from "@kit.ArkGraphics2D";
```

## Brush

画刷对象，描述所绘制图形的填充信息。

### constructor<sup>20+</sup>

constructor()

构造一个新的画刷对象。

**系统能力：** SystemCapability.Graphics.Drawing

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
```

### constructor<sup>20+</sup>

constructor(brush: Brush)

复制构造一个新的画刷对象。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型            | 必填 | 说明               |
| ------ | --------------- | ---- | ------------------ |
| brush  | [Brush](#brush) | 是   | 待复制的画刷对象。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { common2D, drawing } from "@kit.ArkGraphics2D";

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

| 参数名 | 类型                                                 | 必填 | 说明                                                      |
| ------ | ---------------------------------------------------- | ---- | --------------------------------------------------------- |
| color  | [common2D.Color](js-apis-graphics-common2D.md#color) | 是   | ARGB 格式的颜色，每个颜色通道的值是 0 到 255 之间的整数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                                                 |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { common2D, drawing } from "@kit.ArkGraphics2D";

const color: common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
const brush = new drawing.Brush();
brush.setColor(color);
```

### setColor<sup>20+</sup>

setColor(alpha: number, red: number, green: number, blue: number): void

设置画刷的颜色。性能优于[setColor](#setcolor)接口，推荐使用本接口。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                                                      |
| ------ | ------ | ---- | ----------------------------------------------------------------------------------------- |
| alpha  | number | 是   | ARGB 格式颜色的透明度通道值，该参数是 0 到 255 之间的整数，传入范围内的浮点数会向下取整。 |
| red    | number | 是   | ARGB 格式颜色的红色通道值，该参数是 0 到 255 之间的整数，传入范围内的浮点数会向下取整。   |
| green  | number | 是   | ARGB 格式颜色的绿色通道值，该参数是 0 到 255 之间的整数，传入范围内的浮点数会向下取整。   |
| blue   | number | 是   | ARGB 格式颜色的蓝色通道值，该参数是 0 到 255 之间的整数，传入范围内的浮点数会向下取整。   |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                                                 |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
brush.setColor(255, 255, 0, 0);
```

### setColor<sup>20+</sup>

setColor(color: number) : void

设置画刷的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                      |
| ------ | ------ | ---- | ------------------------- |
| color  | number | 是   | 16 进制 ARGB 格式的颜色。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                                                 |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
brush.setColor(0xffff0000);
```

### getColor<sup>20+</sup>

getColor(): common2D.Color

获取画刷的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型           | 说明             |
| -------------- | ---------------- |
| common2D.Color | 返回画刷的颜色。 |

**示例：**

```ts
import { common2D, drawing } from "@kit.ArkGraphics2D";

const color: common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
const brush = new drawing.Brush();
brush.setColor(color);
let colorGet = brush.getColor();
```

### getColor4f<sup>20+</sup>

getColor4f(): common2D.Color4f

获取画刷的颜色，与[getColor](#getcolor20)的区别是返回值类型为浮点数，适用于需要浮点数类型的场景。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                                                       | 说明             |
| ---------------------------------------------------------- | ---------------- |
| [common2D.Color4f](js-apis-graphics-common2D.md#color4f20) | 返回画刷的颜色。 |

**示例：**

```ts
import { common2D, drawing, colorSpaceManager } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
const color: common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
brush.setColor(color);
let color4f = brush.getColor4f();
```

### getHexColor<sup>20+</sup>

getHexColor(): number

获取画刷的颜色。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| number | 返回画刷的颜色，以 16 进制 ARGB 格式的 32 位无符号整数表示。 |

**示例：**

```ts
import { common2D, drawing } from "@kit.ArkGraphics2D";

let color: common2D.Color = { alpha: 255, red: 255, green: 0, blue: 0 };
let brush = new drawing.Brush();
brush.setColor(color);
let hex_color: number = brush.getHexColor();
console.info("getHexColor: ", hex_color.toString(16));
```

### setAntiAlias

setAntiAlias(aa: boolean) : void

设置画刷是否开启抗锯齿。开启后，可以使得图形的边缘在显示时更平滑。未调用此接口设置时，系统默认关闭抗锯齿。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型    | 必填 | 说明                                                |
| ------ | ------- | ---- | --------------------------------------------------- |
| aa     | boolean | 是   | 表示是否开启抗锯齿，true 表示开启，false 表示关闭。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
brush.setAntiAlias(true);
```

### isAntiAlias<sup>20+</sup>

isAntiAlias(): boolean

获取画刷是否开启抗锯齿属性。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型    | 说明                                                        |
| ------- | ----------------------------------------------------------- |
| boolean | 返回画刷是否开启抗锯齿属性，true 表示开启，false 表示关闭。 |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
let isAntiAlias = brush.isAntiAlias();
```

### setAlpha

setAlpha(alpha: number) : void

设置画刷的透明度。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型   | 必填 | 说明                                                             |
| ------ | ------ | ---- | ---------------------------------------------------------------- |
| alpha  | number | 是   | 用于表示透明度的[0, 255]区间内的整数值，传入浮点类型时向下取整。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                                                 |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
brush.setAlpha(128);
```

### getAlpha<sup>20+</sup>

getAlpha(): number

获取画刷的透明度。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型   | 说明                                               |
| ------ | -------------------------------------------------- |
| number | 返回画刷的透明度，该返回值为 0 到 255 之间的整数。 |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
let alpha = brush.getAlpha();
```

### setColorFilter

setColorFilter(filter: ColorFilter) : void

给画刷添加额外的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                      | 必填 | 说明                                  |
| ------ | --------------------------------------------------------- | ---- | ------------------------------------- |
| filter | [ColorFilter](arkts-apis-graphics-drawing-ColorFilter.md) | 是   | 颜色滤波器。null 表示清空颜色滤波器。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
let colorFilter = drawing.ColorFilter.createLinearToSRGBGamma();
brush.setColorFilter(colorFilter);
```

### setMaskFilter<sup>20+</sup>

setMaskFilter(filter: MaskFilter): void

给画刷添加额外的蒙版滤镜。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                    | 必填 | 说明                              |
| ------ | ------------------------------------------------------- | ---- | --------------------------------- |
| filter | [MaskFilter](arkts-apis-graphics-drawing-MaskFilter.md) | 是   | 蒙版滤镜。null 表示清空蒙版滤镜。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from "@kit.ArkUI";
import { common2D, drawing } from "@kit.ArkGraphics2D";

class DrawingRenderNode extends RenderNode {
  draw(context: DrawContext) {
    const canvas = context.canvas;
    const brush = new drawing.Brush();
    let maskFilter = drawing.MaskFilter.createBlurMaskFilter(
      drawing.BlurType.OUTER,
      10
    );
    brush.setMaskFilter(maskFilter);
  }
}
```

### setShaderEffect<sup>20+</sup>

setShaderEffect(shaderEffect: ShaderEffect): void

设置画刷着色器效果。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名       | 类型                                                        | 必填 | 说明                                  |
| ------------ | ----------------------------------------------------------- | ---- | ------------------------------------- |
| shaderEffect | [ShaderEffect](arkts-apis-graphics-drawing-ShaderEffect.md) | 是   | 着色器对象。null 表示清空着色器效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
let shaderEffect = drawing.ShaderEffect.createLinearGradient(
  { x: 100, y: 100 },
  { x: 300, y: 300 },
  [0xff00ff00, 0xffff0000],
  drawing.TileMode.REPEAT
);
brush.setShaderEffect(shaderEffect);
```

### setShadowLayer<sup>20+</sup>

setShadowLayer(shadowLayer: ShadowLayer): void

设置画刷阴影层效果。当前仅在绘制文字时生效。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名      | 类型                                                      | 必填 | 说明                                  |
| ----------- | --------------------------------------------------------- | ---- | ------------------------------------- |
| shadowLayer | [ShadowLayer](arkts-apis-graphics-drawing-ShadowLayer.md) | 是   | 阴影层对象。null 表示清空阴影层效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                 |
| --------- | -------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types. |

**示例：**

```ts
import { RenderNode } from "@kit.ArkUI";
import { common2D, drawing } from "@kit.ArkGraphics2D";

class DrawingRenderNode extends RenderNode {
  draw(context: DrawContext) {
    const canvas = context.canvas;
    let font = new drawing.Font();
    font.setSize(60);

    let textBlob = drawing.TextBlob.makeFromString(
      "hello",
      font,
      drawing.TextEncoding.TEXT_ENCODING_UTF8
    );
    let pen = new drawing.Pen();
    pen.setStrokeWidth(2.0);

    let pen_color: common2D.Color = {
      alpha: 0xff,
      red: 0xff,
      green: 0x00,
      blue: 0x00,
    };
    pen.setColor(pen_color);
    canvas.attachPen(pen);
    canvas.drawTextBlob(textBlob, 100, 100);
    canvas.detachPen();

    let color: common2D.Color = {
      alpha: 0xff,
      red: 0x00,
      green: 0xff,
      blue: 0x00,
    };
    let shadowLayer = drawing.ShadowLayer.create(3, -3, 3, color);
    pen.setShadowLayer(shadowLayer);
    canvas.attachPen(pen);
    canvas.drawTextBlob(textBlob, 100, 200);
    canvas.detachPen();

    let brush = new drawing.Brush();
    let brush_color: common2D.Color = {
      alpha: 0xff,
      red: 0xff,
      green: 0x00,
      blue: 0x00,
    };
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

设置画刷的混合模式。未调用此接口设置时，系统默认的混合模式为 SRC_OVER。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                    | 必填 | 说明             |
| ------ | ------------------------------------------------------- | ---- | ---------------- |
| mode   | [BlendMode](arkts-apis-graphics-drawing-e.md#blendmode) | 是   | 颜色的混合模式。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                                                 |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| 401       | Parameter error.Possible causes:1.Mandatory parameters are left unspecified;2.Incorrect parameter types;3.Parameter verification failed. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
brush.setBlendMode(drawing.BlendMode.SRC);
```

### setImageFilter<sup>20+</sup>

setImageFilter(filter: ImageFilter | null): void

为画刷设置图像滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名 | 类型                                                              | 必填 | 说明                                      |
| ------ | ----------------------------------------------------------------- | ---- | ----------------------------------------- |
| filter | [ImageFilter](arkts-apis-graphics-drawing-ImageFilter.md) \| null | 是   | 图像滤波器，null 表示清空图像滤波器效果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)。

| 错误码 ID | 错误信息                                                                                                     |
| --------- | ------------------------------------------------------------------------------------------------------------ |
| 401       | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified;2. Incorrect parameter types. |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

let brush = new drawing.Brush();
let imgFilter = drawing.ImageFilter.createBlurImageFilter(
  5,
  10,
  drawing.TileMode.DECAL
);
brush.setImageFilter(imgFilter);
brush.setImageFilter(null);
```

### getColorFilter<sup>20+</sup>

getColorFilter(): ColorFilter

获取画刷的颜色滤波器。

**系统能力：** SystemCapability.Graphics.Drawing

**返回值：**

| 类型                                                      | 说明             |
| --------------------------------------------------------- | ---------------- |
| [ColorFilter](arkts-apis-graphics-drawing-ColorFilter.md) | 返回颜色滤波器。 |

**示例：**

```ts
import { drawing } from "@kit.ArkGraphics2D";

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
import { drawing } from "@kit.ArkGraphics2D";

const brush = new drawing.Brush();
brush.reset();
```
