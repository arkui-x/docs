# Class (Font)

> **说明：**
>
> - 本模块首批接口从API version 11开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> - 本模块使用屏幕物理像素单位px。
>
> - 本模块为单线程模型策略，需要调用方自行管理线程安全和上下文状态的切换。

## 导入模块

```ts
import { drawing } from '@kit.ArkGraphics2D';
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
| [FontHinting](arkts-apis-graphics-drawing-e.md#fonthinting20)  | 返回字型轮廓效果。 |

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
| [FontEdging](arkts-apis-graphics-drawing-e.md#fontedging20)  | 返回字型边缘效果。 |

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
| typeface | [Typeface](arkts-apis-graphics-drawing-Typeface.md) | 是   | 字体样式，包括字体名称、粗细、斜体等属性。 |

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
| [Typeface](arkts-apis-graphics-drawing-Typeface.md) | 字体。 |

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
| [FontMetrics](arkts-apis-graphics-drawing-i.md#fontmetrics) | FontMetrics属性。 |

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
| encoding | [TextEncoding](arkts-apis-graphics-drawing-e.md#textencoding) | 是   | 编码格式。 |

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
| edging | [FontEdging](arkts-apis-graphics-drawing-e.md#fontedging20) | 是   | 字型边缘效果。 |

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
| hinting | [FontHinting](arkts-apis-graphics-drawing-e.md#fonthinting20) | 是   | 字型轮廓效果。 |

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
| [Path](arkts-apis-graphics-drawing-Path.md) | 返回获取到的文本的路径轮廓。 |

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
| [Path](arkts-apis-graphics-drawing-Path.md) | 返回指定字形的路径轮廓。 |

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