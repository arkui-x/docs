# Class (Lattice)

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
| fRectTypes   | Array\<[RectType](arkts-apis-graphics-drawing-e.md#recttype20)>\|null                              | 否   | 可选，填充网格类型的数组，默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |
| fColors      | Array\<[common2D.Color](js-apis-graphics-common2D.md#color)>\|null | 否   | 可选，填充网格的颜色数组，默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |

**返回值：**

| 类型                       | 说明                                |
| ------------------------- | ----------------------------------- |
| [Lattice](arkts-apis-graphics-drawing-Lattice.md)     | 返回创建的矩形网格对象。              |

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
| fRectTypes   | Array\<[RectType](arkts-apis-graphics-drawing-e.md#recttype20)>\|null                              | 否   | 可选，填充网格类型的数组，默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |
| fColors      | Array\<number>\|null | 否   | 可选，填充网格的颜色数组，颜色用16进制ARGB格式的32位无符号整数表示，参数默认为空。如果设置，大小必须为(fXCount + 1) * (fYCount + 1)。 |

**返回值：**

| 类型                       | 说明                                |
| ------------------------- | ----------------------------------- |
| [Lattice](arkts-apis-graphics-drawing-Lattice.md)     | 返回创建的矩形网格对象。              |

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