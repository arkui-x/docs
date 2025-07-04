# Class (PathEffect)

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
| path  | [Path](arkts-apis-graphics-drawing-Path.md) | 是 | 通过该路径生成一个图形，用来填充每个虚线段。|
| advance | number | 是 | 虚线段的步长，该参数为大于0的浮点数，否则会抛错误码。 |
| phase | number | 是 | 表示虚线段内图形在虚线步长范围内的偏移量，该参数为浮点数，效果为先对偏移量取绝对值，然后对步长取模。 |
| style | [PathDashStyle](arkts-apis-graphics-drawing-e.md#pathdashstyle20) | 是 | 指定虚线效果的样式。 |

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