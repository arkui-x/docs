# Radio

单选框，提供相应的用户交互选择项。

>  **说明：**
>
>  该组件从API Version 8开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 子组件

无


## 接口

Radio(options: {value: string, group: string})

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数:**

| 参数名 | 参数类型 | 必填 | 参数描述 |
| -------- | -------- | -------- | -------- |
| value | string | 是 | 当前单选框的值。|
| group | string | 是 | 当前单选框的所属群组名称，相同group的Radio只能有一个被选中。|

## 属性

除支持[通用属性](ts-universal-attributes-size.md)外，还支持以下属性：

| 名称 | 参数类型 | 描述 |
| -------- | -------- | -------- |
| checked | boolean | 设置单选框的选中状态。<br/>默认值：false <br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br />从API version 10开始，该属性支持[$$](../../quick-start/arkts-two-way-sync.md)双向绑定变量。 |
| radioStyle<sup>10+</sup> | [RadioStyle](#radiostyle对象说明) | 设置单选框选中状态和非选中状态的样式。 <br/>从API version 10开始，该接口支持在ArkTS组件中使用。|

## 事件

除支持[通用事件](ts-universal-events-click.md)外，还支持以下事件：

| 名称 | 功能描述 |
| -------- | -------- |
| onChange(callback: (isChecked: boolean) => void) | 单选框选中状态改变时触发回调。<br> -&nbsp;isChecked为true时，表示从未选中变为选中。<br> -&nbsp;isChecked为false时，表示从选中变为未选中。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |

## RadioStyle对象说明

| 名称                   | 类型                                       | 必填 | 默认值  | 描述                   |
| ---------------------- | ------------------------------------------ | ---- | ------- | ---------------------- |
| checkedBackgroundColor | [ResourceColor](ts-types.md#resourcecolor) | 否   | #007DFF | 开启状态底板颜色。     |
| uncheckedBorderColor   | [ResourceColor](ts-types.md#resourcecolor) | 否   | #182431 | 关闭状态描边颜色。     |
| indicatorColor         | [ResourceColor](ts-types.md#resourcecolor) | 否   | #FFFFFF | 开启状态内部圆饼颜色。 |

## 示例

```ts
// xxx.ets
@Entry
@Component
struct RadioExample {
  build() {
    Flex({ direction: FlexDirection.Row, justifyContent: FlexAlign.Center, alignItems: ItemAlign.Center }) {
      Column() {
        Text('Radio1')
        Radio({ value: 'Radio1', group: 'radioGroup' }).checked(true)
          .radioStyle({
            checkedBackgroundColor: Color.Pink
          })
          .height(50)
          .width(50)
          .onChange((isChecked: boolean) => {
            console.log('Radio1 status is ' + isChecked)
          })
      }
      Column() {
        Text('Radio2')
        Radio({ value: 'Radio2', group: 'radioGroup' }).checked(false)
          .radioStyle({
            checkedBackgroundColor: Color.Pink
          })
          .height(50)
          .width(50)
          .onChange((isChecked: boolean) => {
            console.log('Radio2 status is ' + isChecked)
          })
      }
      Column() {
        Text('Radio3')
        Radio({ value: 'Radio3', group: 'radioGroup' }).checked(false)
          .radioStyle({
            checkedBackgroundColor: Color.Pink
          })
          .height(50)
          .width(50)
          .onChange((isChecked: boolean) => {
            console.log('Radio3 status is ' + isChecked)
          })
      }
    }.padding({ top: 30 })
  }
}
```
![radio](figures/radio.gif)
