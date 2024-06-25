# XComponent

可用于EGL/OpenGLES和媒体数据写入，并显示在XComponent组件。

> **说明：**
>
> 该组件从API Version 11 开始支持跨平台。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 子组件
  构造参数type为"surface"时不支持。


## 接口

### XComponent

XComponent(value: {id: string, type: string, libraryname?: string, controller?: XComponentController})

**参数:** 

| 参数名      | 参数类型                                      | 必填 | 描述                                                         |
| ----------- | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| id          | string                                        | 是   | 组件的唯一标识，支持最大的字符串长度128。                    |
| type        | string                                        | 是   | 用于指定XComponent组件类型，可选值仅有两个为：<br/>-"surface"：用于EGL/OpenGLES和媒体数据写入，开发者定制的绘制内容单独展示到屏幕上。<br/>-"component"<sup>9+</sup> ：XComponent将变成一个容器组件，并可在其中执行非UI逻辑以动态加载显示内容。<br/>其他值均会被视为"surface"类型 |
| libraryname | string                                        | 否   | 应用Native层编译输出动态库名称，仅XComponent类型为"surface"时有效。 |
| controller  | [XComponentcontroller](#xcomponentcontroller) | 否   | 给组件绑定一个控制器，通过控制器调用组件方法，仅XComponent类型为"surface"时有效。 |

### XComponent

XComponent(value: {id: string, type: XComponentType, libraryname?: string, controller?: XComponentController})

**参数:** 

| 参数名      | 参数类型                                      | 必填 | 描述                                                         |
| ----------- | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| id          | string                                        | 是   | 组件的唯一标识，支持最大的字符串长度128。                    |
| type        | XComponentType  | 是   | 用于指定XComponent组件类型（跨平台仅支持SURFACE，COMPONENT）。 |
| libraryname | string                                        | 否   | 用Native层编译输出动态库名称，仅类型为SURFACE或TEXTURE时有效。 |
| controller  | [XComponentcontroller](#xcomponentcontroller) | 否   | 给组件绑定一个控制器，通过控制器调用组件方法，仅类型为SURFACE或TEXTURE时有效。 |

## XComponentType枚举说明

| 名称      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| SURFACE   | 用于EGL/OpenGLES和媒体数据写入，开发者定制的绘制内容单独展示到屏幕上。 |
| COMPONENT | XComponent将变成一个容器组件，并可在其中执行非UI逻辑以动态加载显示内容。 |
| TEXTURE   | 用于EGL/OpenGLES和媒体数据写入，开发者定制的绘制内容会和XComponent组件的内容合成后展示到屏幕上（跨平台暂不支持）。 |

> **说明：**
>
> type为COMPONENT("component")时，XComponent作为容器，子组件沿垂直方向布局：
>
> - 垂直方向上对齐格式：[FlexAlign](ts-appendix-enums.md#flexalign).Start
> - 水平方向上对齐格式：[FlexAlign](ts-appendix-enums.md#flexalign).Center
>
> 所有的事件响应均不支持。
>
> 布局方式更改和事件响应均可通过挂载子组件来设置。
>
> 内部所写的非UI逻辑需要封装在一个或多个函数内。

## 属性
- XComponent显示的内容，可由开发者自定义绘制，通用属性中的[背景设置](ts-universal-attributes-background.md)、[透明度设置](ts-universal-attributes-opacity.md)和[图像效果](ts-universal-attributes-image-effect.md)按照type类型有限支持。

- type为SURFACE("surface")时仅支持[图像效果](ts-universal-attributes-image-effect.md)中的shadow属性，建议使用EGL/OpenGLES提供的接口设置相关内容。

  > **说明：**
  >
  > 从API version 11开始，type为SURFACE("surface")时支持[背景颜色设置](ts-universal-attributes-background.md#backgroundcolor)。

- type为COMPONENT("component")时仅支持[图像效果](ts-universal-attributes-image-effect.md)中的shadow属性，建议使用挂载子组件的方式进行设置相关内容。


## 事件

仅type为SURFACE("surface")或TEXTURE时以下事件有效。不支持[通用事件](ts-universal-events-click.md)。

### onLoad

onLoad(callback: (event?: object) => void )

插件加载完成时回调事件。

**参数:**

| 参数名   | 参数类型   | 必填   | 描述                                       |
| ----- | ------ | ---- | ---------------------------------------- |
| event | object | 否    | 获取XComponent实例对象的context，context上挂载的方法由开发者在c++层定义。 |

### onDestroy

onDestroy(event: () => void )

插件卸载完成时回调事件。

## XComponentController

xcomponent 组件的控制器，可以将此对象绑定至XComponent组件，然后通过控制器来调用组件方法。

### 创建对象

```
xcomponentController: XComponentController = new XComponentController()
```

### getXComponentSurfaceId

getXComponentSurfaceId(): string

获取XComponent对应Surface的ID，仅XComponent类型为SURFACE("surface")或TEXTURE时有效。

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

**返回值:**

| 类型   | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| Object | 获取XComponent实例对象的context，context包含的具体接口方法由开发者自定义，context内容与onLoad回调中的第一个参数一致。 |

示例效果请以真机运行为准，当前IDE预览器不支持。

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