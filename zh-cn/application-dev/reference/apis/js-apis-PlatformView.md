# @arkui-x.platformview(平台视图)

本模块是ArkUI-X框架提供的**平台视图**接口，其核心机制在于实现原生视图与ArkUI组件的混合渲染。<br>通过直接嵌入原生平台（Android、iOS）的UI组件，开发者能够在跨平台应用中复用现有原生平台的功能与视图，提升跨平台开发效率。<br>

> **说明：**
>
> **平台视图渲染模式分为：**
>
> 1. **Surface 模式**
>
>    ArkUI-X将原生视图作为一个独立的图层（Surface）直接叠加在自身组件树之上，在相应区域为原生组件"开洞"，使得下方的原生视图得以直接显示。这提供了近乎原生的渲染性能。<br>
>
>    渲染路径最短，接近纯原生性能，尤其适用于视频播放、地图、相机预览等对帧率要求极高的场景。<br>
>
>    **平台约束**：由于是独立的图层，在深度嵌套的可滚动组件（如 `List`、`Scroll`）中使用时，可能出现**触摸事件“不跟手”**，**与ArkUI-X组件动画/滚动不同步**，以及**设备旋转时布局可能不同步**的问题。因此，**不建议在复杂的嵌套滚动容器中使用此模式**。<br>
>
> 2. **Texture 模式**
>
>    原生视图被渲染到一个纹理（Texture）中，随后该纹理作为一张普通的"图片"被集成到ArkUI-X的组件树中进行合成与渲染。原生视图在渲染层面上与ArkUI-X组件成为一体。<br>
>
>    由于已成为UI树的一部分，因此能够完美跟随ArkUI-X的布局、动画、滚动和旋转，无同步问题。<br>
>
>    相比Surface模式，需要额外的纹理复制和合成步骤，会带来一定的性能开销，在极端复杂的UI或高频更新的场景中可能影响流畅度，适用于静态视图对帧率、性能要求不高的场景。<br>
>
> 平台视图渲染模式默认为：Texture模式。<br>

## 导入模块

```ts
import PlatformView, { PlatformViewAttribute, PlatformViewType } from '@arkui-x.platformview';
```

## PlatformViewType

此枚举定义了平台视图的渲染模式。<br>

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

| 名称         | 值   | 说明                         |
| :----------- | :--- | :--------------------------- |
| TEXTURE_TYPE | 0    | Surface 模式                |
| SURFACE_TYPE | 1    | Texture 模式                |

## PlatformView

PlatformView(value: {id: string, data?: string, type?: PlatformViewType})

用于在ArkUI中创建并嵌入原生平台视图（Android或iOS原生组件）的接口组件，通过指定唯一标识、渲染模式和初始化数据，实现高性能或高兼容性的混合渲染。<br>

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数:**

| 参数名 | 类型                                  | 必填 | 说明                                                         |
| ------ | ------------------------------------- | ---- | ------------------------------------------------------------ |
| id     | string                                | 是   | 视图的唯一标识符。<br/>在同一应用中，每个PlatformView实例的ID应保持唯一，用于在视图树中正确识别和管理该实例。 |
| type   | [PlatformViewType](#PlatformViewType) | 否   | 指定视图的渲染模式，这直接影响性能和表现。包括Surface模式和Texture模式。<br/>默认模式为：Texture 模式。 |
| data   | string                                | 否   | 传递给原生视图的初始化数据。                                 |

**返回值:**

| 类型                  | 描述                                                         |
| --------------------- | ------------------------------------------------------------ |
| PlatformViewAttribute | 返回一个平台视图属性配置对象。 |

**示例：**

```tsx
PlatformView({id:'MapView', data:'text', type:PlatformViewType.SURFACE_TYPE})
  .width('90%')
  .height('90%')
```

## 开发指南

[Android平台视图开发指南](../../tutorial/how-to-use-platformview-on-android.md)

[iOS平台视图开发指南](../../tutorial/how-to-use-platformview-on-ios.md)