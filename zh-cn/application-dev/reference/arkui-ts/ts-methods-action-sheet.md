# 列表选择弹窗

列表弹窗。

>  **说明：**
>
>  从API Version 8开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>
> 本模块功能依赖UI的执行上下文，不可在UI上下文不明确的地方使用，参见[UIContext](../apis/js-apis-arkui-UIContext.md#uicontext)说明。
>
> 从API version 10开始，可以通过使用[UIContext](../apis/js-apis-arkui-UIContext.md#uicontext)中的[showActionSheet](../apis/js-apis-arkui-UIContext.md#showactionsheet)来明确UI的执行上下文。

## ActionSheet.show

show(value: {&nbsp;title: string&nbsp;|&nbsp;Resource,&nbsp;message: string&nbsp;|&nbsp;Resource,&nbsp;confirm?: {value: string&nbsp;|&nbsp;Resource,&nbsp;action:() => void},&nbsp;cancel?:()=>void,&nbsp;sheets: Array&lt;SheetInfo&gt;,&nbsp;autoCancel?:boolean,&nbsp;alignment?: DialogAlignment,&nbsp;offset?: { dx: number | string | Resource; dy: number | string | Resource } })

定义列表弹窗并弹出。

**参数：**

| 参数名        | 参数类型                    | 必填  | 参数描述                          |
| ---------- | -------------------------- | ------- | ----------------------------- |
| title      | [Resource](ts-types.md#resource)&nbsp;\|&nbsp;string | 是     |  弹窗标题。 |
| message    | [Resource](ts-types.md#resource)&nbsp;\|&nbsp;string | 是     | 弹窗内容。  |
| autoCancel | boolean                           | 否     | 点击遮障层时，是否关闭弹窗。<br>默认值：true |
| confirm    | {<br/>value:&nbsp;[ResourceStr](ts-types.md#resourcestr),<br/>action:&nbsp;()&nbsp;=&gt;&nbsp;void<br/>} | 否  | 确认按钮的文本内容和点击回调。<br>默认值：<br/>value：按钮文本内容。<br/>action:&nbsp;按钮选中时的回调。 |
| cancel     | ()&nbsp;=&gt;&nbsp;void           | 否     | 点击遮障层关闭dialog时的回调。   |
| alignment  | [DialogAlignment](ts-methods-alert-dialog-box.md#dialogalignment枚举说明) | 否     |  弹窗在竖直方向上的对齐方式。<br>默认值：DialogAlignment.Bottom |
| offset     | {<br/>dx:&nbsp;Length,<br/>dy:&nbsp;Length<br/>} | 否      | 弹窗相对alignment所在位置的偏移量。{<br/>dx:&nbsp;0,<br/>dy:&nbsp;0<br/>} |
| sheets     | Array&lt;SheetInfo&gt; | 是       | 设置选项内容，每个选择项支持设置图片、文本和选中的回调。 |

## SheetInfo接口说明

| 参数名 | 参数类型                                                     | 必填 | 参数描述          |
| ------ | ------------------------------------------------------------ | ---- | ----------------- |
| title  | [ResourceStr](ts-types.md#resourcestr) | 是   | 选项的文本内容。       |
| icon   | [ResourceStr](ts-types.md#resourcestr) | 否   | 选项的图标，默认无图标显示。     |
| action | ()=&gt;void                                          | 是   | 选项选中的回调。 |


## 示例


```ts
@Entry
@Component
struct ActionSheetExample {
  build() {
    Flex({ direction: FlexDirection.Column, alignItems: ItemAlign.Center, justifyContent: FlexAlign.Center }) {
      Button('Click to Show ActionSheet')
        .onClick(() => {
          ActionSheet.show({
            title: 'ActionSheet title',
            message: 'message',
            autoCancel: true,
            confirm: {
              value: 'Confirm button',
              action: () => {
                console.log('Get Alert Dialog handled')
              }
            },
            cancel: () => {
              console.log('actionSheet canceled')
            },
            alignment: DialogAlignment.Bottom,
            offset: { dx: 0, dy: -10 },
            sheets: [
              {
                title: 'apples',
                action: () => {
                  console.log('apples')
                }
              },
              {
                title: 'bananas',
                action: () => {
                  console.log('bananas')
                }
              },
              {
                title: 'pears',
                action: () => {
                  console.log('pears')
                }
              }
            ]
          })
        })
    }.width('100%')
    .height('100%')
  }
}
```

![zh-cn_image_0000001241668363](figures/zh-cn_image_0000001241668363.gif)
