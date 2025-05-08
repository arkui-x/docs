# XComponent

提供用于图形绘制和媒体数据写入的Surface，XComponent负责将其嵌入到视图中，支持应用自定义Surface位置和大小。

> **说明：**
>
> 该组件从API Version 11 开始支持跨平台。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 子组件
无


## 接口

### XComponent<sup>20+</sup>

XComponent(options: XComponentOptions)

创建XComponent组件，支持在ArkTS侧获取SurfaceId、注册XComponent持有的Surface的生命周期回调和触摸、鼠标、按键等组件事件回调。

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                | 必填 | 说明                           |
| ------- | --------------------------------------- | ---- | ------------------------------ |
| options | [XComponentOptions](#xcomponentoptions20) | 是   | 定义XComponent的具体配置参数。 |

### XComponent<sup>(deprecated)</sup>

XComponent(value: {id: string, type: string, libraryname?: string, controller?: XComponentController})

**说明：**

从API version 12开始废弃，建议使用[XComponent(options: XComponentOptions)](#xcomponent20)替代。

**参数:** 

| 参数名      | 参数类型                                      | 必填 | 描述                                                         |
| ----------- | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| id          | string                                        | 是   | 组件的唯一标识，支持最大的字符串长度128。                    |
| type        | string                                        | 是   | 用于指定XComponent组件类型，可选值仅有两个为：<br/>-"surface"：用于EGL/OpenGLES和媒体数据写入，开发者定制的绘制内容单独展示到屏幕上。<br/>-"component"<sup>9+</sup> ：XComponent将变成一个容器组件，并可在其中执行非UI逻辑以动态加载显示内容。<br/>其它值均会被视为"surface"类型 |
| libraryname | string                                        | 否   | 应用Native层编译输出动态库名称，仅XComponent类型为"surface"时有效。 |
| controller  | [XComponentcontroller](#xcomponentcontroller) | 否   | 给组件绑定一个控制器，通过控制器调用组件方法，仅XComponent类型为"surface"时有效。 |

### XComponent

XComponent(value: {id: string, type: XComponentType, libraryname?: string, controller?: XComponentController})

**支持平台：** Android、iOS

**参数:** 

| 参数名      | 参数类型                                      | 必填 | 描述                                                         |
| ----------- | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| id          | string                                        | 是   | 组件的唯一标识，支持最大的字符串长度128。                    |
| type        | [XComponentType](#xcomponenttype)  | 是   | 用于指定XComponent组件类型（跨平台仅支持SURFACE）。 |
| libraryname | string                                        | 否   | 用Native层编译输出动态库名称，仅类型为SURFACE或TEXTURE时有效。 |
| controller  | [XComponentcontroller](#xcomponentcontroller) | 否   | 给组件绑定一个控制器，通过控制器调用组件方法，仅类型为SURFACE或TEXTURE时有效。 |

## XComponentOptions<sup>20+</sup>

定义XComponent的具体配置参数。

| 名称 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| type | [XComponentType](#xcomponenttype)         | 是   | 用于指定XComponent组件类型。 |
| controller | [XComponentController](#xcomponentcontroller) | 是 | 给组件绑定一个控制器，通过控制器调用组件方法，仅类型为SURFACE或TEXTURE时有效。 |

## XComponentType

| 名称      | 描述                                                         | Android平台 | iOS平台 |
| --------- | ------------------------------------------------------------ | --------- | --------- |
| SURFACE   | 用于EGL/OpenGLES和媒体数据写入，开发者定制的绘制内容单独展示到屏幕上。 | 支持 | 支持 |
| COMPONENT<sup>(deprecated)</sup> | XComponent将变成一个容器组件，并可在其中执行非UI逻辑以动态加载显示内容。<br/>**说明：**<br/>从API version 12 开始，该接口废弃，建议使用其它容器组件替代。 | 不支持 | 不支持 |
| TEXTURE   | 用于EGL/OpenGLES和媒体数据写入，开发者定制的绘制内容会和XComponent组件的内容合成后展示到屏幕上（跨平台暂不支持）。 | 不支持 | 不支持 |

## 属性
除支持通用属性外，还有限支持以下属性：
  > 
  > **说明：**
  >
  > 不支持foregroundColor、obscured和pixelStretchEffect属性，并且type为SURFACE类型时也不支持动态属性设置、自定义绘制、背景设置(backgroundColor除外)、图像效果(shadow除外)、maskShape和foregroundEffect属性。
  >
  > 对于TEXTURE和SURFACE类型的XComponent组件，当不设置[renderFit](ts-universal-attributes-renderfit.md)属性时，取默认值为RenderFit.RESIZE_FILL。
  > 
  > 对于SURFACE类型的XComponent组件，当组件背景色为不透明的纯黑色时，其[renderFit](ts-universal-attributes-renderfit.md)通用属性仅支持设置为RenderFit.RESIZE_FILL，不推荐设置为其它的RenderFit枚举值。


## 事件

从API version 12开始，type为SURFACE或TEXTURE时，支持通用事件。

> **说明：** 
>
> 当配置libraryname参数时，[点击事件](ts-universal-events-click.md)、[触摸事件](ts-universal-events-touch.md)、[挂载卸载事件](ts-universal-events-show-hide.md)、[按键事件](ts-universal-events-key.md)、[焦点事件](ts-universal-focus-event.md)、[鼠标事件](ts-universal-mouse-key.md)仅响应C-API侧事件接口。

仅type为SURFACE或TEXTURE时以下事件有效：

### onLoad

onLoad(callback: (event?: object) => void )

插件加载完成时回调事件。

**支持平台：** Android、iOS

**参数:**

| 参数名   | 参数类型   | 必填   | 描述                                       |
| ----- | ------ | ---- | ---------------------------------------- |
| event | object | 否    | 获取XComponent实例对象的context，context上挂载的方法由开发者在c++层定义。 |

### onDestroy

onDestroy(event: () => void )

插件卸载完成时回调事件。

**支持平台：** Android、iOS

## XComponentController

xcomponent 组件的控制器，可以将此对象绑定至XComponent组件，然后通过控制器来调用组件方法。

### 创建对象

```ets
xcomponentController: XComponentController = new XComponentController()
```

### getXComponentSurfaceId

getXComponentSurfaceId(): string

获取XComponent对应Surface的ID，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

**支持平台：** Android、iOS

**返回值:**

| 类型     | 描述                      |
| ------ | ----------------------- |
| string | XComponent持有Surface的ID。 |


### setXComponentSurfaceSize<sup>(deprecated)</sup>

setXComponentSurfaceSize(value: {surfaceWidth: number, surfaceHeight: number}): void

设置XComponent持有Surface的宽度和高度，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

该接口从API Version 12开始废弃。

**参数:**

| 参数名           | 参数类型   | 必填   | 描述                      |
| ------------- | ------ | ---- | ----------------------- |
| surfaceWidth  | number | 是    | XComponent持有Surface的宽度。 |
| surfaceHeight | number | 是    | XComponent持有Surface的高度。 |


### getXComponentContext

getXComponentContext(): Object

获取XComponent实例对象的context，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

**支持平台：** Android、iOS

**返回值:**

| 类型   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| Object | 获取XComponent实例对象的context，context包含的具体接口方法由开发者自定义，context内容与onLoad回调中的第一个参数一致。 |

### setXComponentSurfaceRect<sup>20+</sup>

setXComponentSurfaceRect(rect: SurfaceRect): void

设置XComponent持有Surface的显示区域，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

**支持平台：** iOS

**参数：**

| 参数名 | 类型                             | 必填 | 说明                              |
| ------ | ------------------------------------ | ---- | --------------------------------- |
| rect   | [SurfaceRect](#surfacerect20对象说明) | 是   | XComponent持有Surface的显示区域。 |

> **说明：**
>
> rect参数中的offsetX/offsetY不设置时，Surface显示区域相对于XComponent左上角x/y轴的偏移效果默认按照居中显示。
>
> rect参数中的surfaceWidth和surfaceHeight存在0或负数时，调用该接口设置显示区域不生效。
>
> 该方法优先级高于[border](ts-universal-attributes-border.md#border)、[padding](ts-universal-attributes-size.md#padding)等可以改变内容偏移和大小的属性。

### getXComponentSurfaceRect<sup>20+</sup>

getXComponentSurfaceRect(): SurfaceRect

获取XComponent持有Surface的显示区域，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

**支持平台：** Android、iOS

**返回值：**

| 类型                                 | 描述                                  |
| ------------------------------------ | ------------------------------------- |
| [SurfaceRect](#surfacerect20对象说明) | 获取XComponent持有Surface的显示区域。 |

### onSurfaceCreated<sup>20+</sup>

onSurfaceCreated(surfaceId: string): void

当XComponent持有的Surface创建后进行该回调，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型 | 必填 | 说明                                              |
| --------- | -------- | ---- | ------------------------------------------------- |
| surfaceId | string   | 是   | 回调该方法的时候，绑定XComponent持有Surface的ID。 |

> **说明：**
>
> 仅当XComponent组件未设置libraryname参数时，会进行该回调。

### onSurfaceChanged<sup>20+</sup>

onSurfaceChanged(surfaceId: string, rect: SurfaceRect): void

当XComponent持有的Surface大小改变后（包括创建时的大小改变）进行该回调，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型                              | 必填 | 说明                                                    |
| --------- | ------------------------------------- | ---- | ------------------------------------------------------- |
| surfaceId | string                                | 是   | 回调该方法的时候，绑定XComponent持有Surface的ID。       |
| rect      | [SurfaceRect](#surfacerect20对象说明) | 是   | 回调该方法的时候，绑定XComponent持有Surface的显示区域。 |

> **说明：**
>
> 仅当XComponent组件未设置libraryname参数时，会进行该回调。

### onSurfaceDestroyed<sup>20+</sup>

onSurfaceDestroyed(surfaceId: string): void

当XComponent持有的Surface销毁后进行该回调，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

**支持平台：** Android、iOS

**参数：**

| 参数名    | 类型 | 必填 | 说明                                              |
| --------- | -------- | ---- | ------------------------------------------------- |
| surfaceId | string   | 是   | 回调该方法的时候，绑定XComponent持有Surface的ID。 |

> **说明：**
>
> 仅当XComponent组件未设置libraryname参数时，会进行该回调。

### setXComponentSurfaceRotation<sup>20+</sup>

setXComponentSurfaceRotation(rotationOptions: SurfaceRotationOptions): void

设置XComponent持有Surface在屏幕旋转时是否锁定方向，仅XComponent类型为SURFACE("surface")时有效。

**支持平台：** iOS

**参数：**

| 参数名 | 类型                             | 必填 | 说明                              |
| ------ | ------------------------------------ | ---- | --------------------------------- |
| rotationOptions   | [SurfaceRotationOptions](#surfacerotationoptions20对象说明) | 是 | 设置XComponent持有Surface在屏幕旋转时是否锁定方向。 |

> **说明：**
>
> rotationOptions未配置时，默认XComponent持有Surface在屏幕旋转时不锁定方向，跟随屏幕进行旋转。
>
> 仅在屏幕旋转90°，即发生横竖屏切换时生效。
>
> 锁定旋转后的Buffer宽高需要保持不变，否则会有拉伸问题。

### getXComponentSurfaceRotation<sup>20+</sup>

getXComponentSurfaceRotation(): Required\<SurfaceRotationOptions>

获取XComponent持有Surface在屏幕旋转时是否锁定方向的设置，仅XComponent类型为SURFACE("surface")时有效。

**支持平台：** Android、iOS

**返回值：**

| 类型                                 | 描述                                  |
| ------------------------------------ | ------------------------------------- |
| [SurfaceRotationOptions](#surfacerotationoptions20对象说明) | 获取XComponent持有Surface在屏幕旋转时是否锁定方向的设置。 |

## SurfaceRotationOptions<sup>20+</sup>对象说明

用于描述XComponent持有Surface在屏幕旋转时是否锁定方向的设置。

| 名称          | 类型   | 必填 | 说明                                                         |
| ------------- | ------ | ---- | ------------------------------------------------------------ |
| lock       | boolean | 否   | Surface在屏幕旋转时是否锁定方向，未设置时默认取值为false，即不锁定方向。<br/>true：锁定方向；false：不锁定方向。 |

## SurfaceRect<sup>20+</sup>对象说明

用于描述XComponent持有Surface的显示区域。

| 名称          | 类型   | 必填 | 说明                                                         |
| ------------- | ------ | ---- | ------------------------------------------------------------ |
| offsetX       | number | 否   | Surface显示区域相对于XComponent组件左上角的x轴坐标，单位：px。 |
| offsetY       | number | 否   | Surface显示区域相对于XComponent组件左上角的y轴坐标，单位：px。 |
| surfaceWidth  | number | 是   | Surface显示区域的宽度，单位：px。                            |
| surfaceHeight | number | 是   | Surface显示区域的高度，单位：px。                            |

> **说明：**
>
> surfaceWidth和surfaceHeight属性在未调用[setXComponentSurfaceRect](#setxcomponentsurfacerect20)也未设置[border](ts-universal-attributes-border.md#border)和[padding](ts-universal-attributes-size.md#padding)等属性时，其取值大小为XComponent组件的大小。
>


## 示例

示例效果请以真机运行为准，当前DevEco Studio预览器不支持。


```ts
// xxx.ets
@Entry
@Component
struct PreviewArea {
  private surfaceId: string = ''
  private xComponentContext: Record<string, () => void> = {}
  xComponentController: XComponentController = new XComponentController()

  build() {
    Column() {
      Text(JSON.stringify(this.rect))
        .fontSize(12)
      XComponent({
        id: 'xcomponent',
        type: XComponentType.SURFACE,
        controller: this.xComponentController
      })
        .onLoad(() => {
          this.surfaceId = this.xComponentController.getXComponentSurfaceId()
          this.xComponentContext = this.xComponentController.getXComponentContext() as Record<string, () => void>
        })
        .width('640px')
        .height('480px')
    }
    .position({ x: 0, y: 48 })
  }
}
```