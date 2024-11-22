#  @ohos.arkui.drawableDescriptor (DrawableDescriptor)

本模块提供获取pixelMap的能力，包括前景、背景、蒙版和分层图标。

> **说明：**
>
> 本模块首批接口从API version 10开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 示例效果请以真机运行为准，当前IDE预览器不支持。

## 导入模块

```ts
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor';
```

## DrawableDescriptor

### constructor()

创建DrawableDescriptor或LayeredDrawableDescriptor对象。对象构造需要使用全球化接口[getDrawableDescriptor](js-apis-resource-manager.md#getdrawabledescriptor10)或[getDrawableDescriptorByName](js-apis-resource-manager.md#getdrawabledescriptorbyname10)。

**系统接口：** 此接口为系统接口。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

### getPixelMap

getPixelMap(): image.PixelMap

获取pixelMap。

**元服务API：** 从API version 11开始，该接口支持在元服务中使用。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                                         | 说明     |
| -------------------------------------------- | -------- |
| [image.PixelMap](js-apis-image.md#pixelmap7) | PixelMap |

**示例：**

```ts
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor'
let resManager = getContext().resourceManager
let pixmap: DrawableDescriptor = (resManager.getDrawableDescriptor($r('app.media.icon')
  .id)) as DrawableDescriptor;
let pixmapNew: object = pixmap.getPixelMap()
```

当传入资源id或name为普通图片时，生成DrawableDescriptor对象。

## LayeredDrawableDescriptor

当传入资源id或name为包含前景和背景资源的json文件时，生成LayeredDrawableDescriptor对象。继承自[DrawableDescriptor](#drawabledescriptor)

drawable.json位于项目工程entry/src/main/resources/base/media目录下。定义请参考：

```ts
{
  "layered-image":
  {
    "background" : "$media:background",
    "foreground" : "$media:foreground"
  }
}
```

**示例：**

通过json文件创建LayeredDrawableDescriptor

```ts
// xxx.ets
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor'

@Entry
@Component
struct Index {
  private resManager = getContext().resourceManager

  build() {
    Row() {
      Column() {
        Image((this.resManager.getDrawableDescriptor($r('app.media.drawable').id) as LayeredDrawableDescriptor))
        Image(((this.resManager.getDrawableDescriptor($r('app.media.drawable')
          .id) as LayeredDrawableDescriptor).getForeground()).getPixelMap())
      }.height('50%')
    }.width('50%')
  }
}
```

### getPixelMap

getPixelMap(): image.PixelMap

获取前景、背景和蒙版融合裁剪后的pixelMap。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                                         | 说明     |
| -------------------------------------------- | -------- |
| [image.PixelMap](js-apis-image.md#pixelmap7) | PixelMap |

**示例：**

```ts
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor'
let resManager = getContext().resourceManager
let pixmap: LayeredDrawableDescriptor = (resManager.getDrawableDescriptor($r('app.media.drawable')
  .id)) as LayeredDrawableDescriptor;
let pixmapNew: object = pixmap.getPixelMap()
```

### getForeground

getForeground(): DrawableDescriptor;

获取前景的DrawableDescriptor对象。

**元服务API：** 从API version 11开始，该接口支持在元服务中使用。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                                      | 说明                   |
| ----------------------------------------- | ---------------------- |
| [DrawableDescriptor](#drawabledescriptor) | DrawableDescriptor对象 |

**示例：**

```ts
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor'
let resManager = getContext().resourceManager
let drawable: LayeredDrawableDescriptor = (resManager.getDrawableDescriptor($r('app.media.drawable')
  .id)) as LayeredDrawableDescriptor;
let drawableNew: object = drawable.getForeground()
```

### getBackground

getBackground(): DrawableDescriptor;

获取背景的DrawableDescriptor对象。

**元服务API：** 从API version 11开始，该接口支持在元服务中使用。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                                      | 说明                   |
| ----------------------------------------- | ---------------------- |
| [DrawableDescriptor](#drawabledescriptor) | DrawableDescriptor对象 |

**示例：**

```ts
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor'
let resManager = getContext().resourceManager
let drawable: LayeredDrawableDescriptor = (resManager.getDrawableDescriptor($r('app.media.drawable')
  .id)) as LayeredDrawableDescriptor;
let drawableNew: object = drawable.getBackground()
```

### getMask

getMask(): DrawableDescriptor

获取蒙版的DrawableDescriptor对象。

**元服务API：** 从API version 11开始，该接口支持在元服务中使用。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                                      | 说明                   |
| ----------------------------------------- | ---------------------- |
| [DrawableDescriptor](#drawabledescriptor) | DrawableDescriptor对象 |

**示例：**

```ts
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor'
let resManager = getContext().resourceManager
let drawable: LayeredDrawableDescriptor = (resManager.getDrawableDescriptor($r('app.media.drawable')
  .id)) as LayeredDrawableDescriptor;
let drawableNew: object = drawable.getMask()
```

### getMaskClipPath

static getMaskClipPath(): string

LayeredDrawableDescriptor的静态方法，获取系统内置的裁切路径参数。

**元服务API：** 从API version 11开始，该接口支持在元服务中使用。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 返回裁切路径的命令字符串 |

**示例：**

```ts
// xxx.ets
import { DrawableDescriptor, LayeredDrawableDescriptor } from '@ohos.arkui.drawableDescriptor'

@Entry
@Component
struct Index {
build() {
  Row() {
    Column() {
      Image($r('app.media.icon'))
        .width('200px').height('200px')
        .clip(new Path({commands:LayeredDrawableDescriptor.getMaskClipPath()}))
      Text(`获取系统内置的裁剪路径参数：`)
        .fontWeight(800)
      Text(JSON.stringify(LayeredDrawableDescriptor.getMaskClipPath()))
        .padding({ left: 20, right: 20 })
    }.height('100%').justifyContent(FlexAlign.Center)
  }.width('100%')
}
}
```

