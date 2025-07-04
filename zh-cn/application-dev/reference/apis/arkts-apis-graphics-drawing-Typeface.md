# Class (Typeface)

> **说明：**
>
> - 本模块首批接口从API version 20开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> - 本模块使用屏幕物理像素单位px。
>
> - 本模块为单线程模型策略，需要调用方自行管理线程安全和上下文状态的切换。

## 导入模块

```ts
import { drawing } from '@kit.ArkGraphics2D';
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
| typefaceArguments | [TypefaceArguments](arkts-apis-graphics-drawing-TypefaceArguments.md)           | 是   | 字体属性。 |

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

### makeFromFileWithArguments<sup>20+</sup>

static makeFromFileWithArguments(filePath: string, typefaceArguments: TypefaceArguments): Typeface

根据字体文件路径和字体属性构造新的字体。

**系统能力：** SystemCapability.Graphics.Drawing

**参数：**

| 参数名         | 类型                                       | 必填   | 说明                  |
| ----------- | ---------------------------------------- | ---- | ------------------- |
| filePath | string           | 是   | 表示字体资源存放的路径。 |
| typefaceArguments | [TypefaceArguments](arkts-apis-graphics-drawing-TypefaceArguments.md) | 是  | 表示字体属性。 |

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