# SplitLayout


上下结构布局介绍了常用的页面布局样式。主要分为上下文本和上下图文两种类型。


> **说明：**
>
> 该组件从API version 22开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 导入模块

```
import { SplitLayout } from '@ohos.arkui.advanced.SplitLayout'
```


## 子组件

无

## 属性

不支持[通用属性](../../../application-dev/reference/arkui-ts/ts-universal-attributes-size.md)。


## SplitLayout

SplitLayout({mainImage: Resource, primaryText: string, secondaryText?: string, tertiaryText?: string, container: ()&nbsp;=&gt;&nbsp;void })

**装饰器类型：**@Component

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称          | 类型                                   | 必填 | 装饰器类型    | 说明         |
| ------------- | -------------------------------------- | ---- | ------------- | ------------ |
| mainImage     | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 是   | @State        | 传入图片。   |
| primaryText   | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 是   | @Prop         | 标题内容。   |
| secondaryText | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 否   | @Prop         | 副标题内容。 |
| tertiaryText  | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 否   | @Prop         | 辅助文本。   |
| container     | ()&nbsp;=&gt;&nbsp;void                | 是   | @BuilderParam | 容器内组件。 |

## 事件

不支持[通用事件](../../../application-dev/reference/arkui-ts/README.md)。

## 示例

该示例通过SplitLayout实现了页面布局，并具备自适应能力。

```ts
import { SplitLayout } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State demoImage: Resource = $r("app.media.background");

  build() {
    Column() {
      SplitLayout({
        mainImage: this.demoImage,
        primaryText: '新歌推荐',
        secondaryText: '私人订制新歌精选站，为你推荐专属优质新歌;',
        tertiaryText: '每日更新',
      }) {
        Text('示例：空白区域容器内可添加组件')
          .margin({ top: 36 })
      }
    }
    .justifyContent(FlexAlign.SpaceBetween)
    .height('100%')
    .width('100%')
  }
}
```


小于等于600vp布局：


![zh-cn_image_0000001665553957](figures/zh-cn_image_0000001665553957.png)


大于600vp且小于等于840vp的布局：


![zh-cn_image_0000001616957408](figures/zh-cn_image_0000001616957408.png)


大于840vp布局：


![zh-cn_image_0000001617116972](figures/zh-cn_image_0000001617116972.png)