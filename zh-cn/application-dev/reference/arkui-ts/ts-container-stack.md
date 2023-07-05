# Stack

堆叠容器，子组件按照顺序依次入栈，后一个子组件覆盖前一个子组件。

>  **说明：**
>
>  该组件从API Version 7开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。


## 子组件

可以包含子组件。


## 接口

Stack(value?: { alignContent?: Alignment })

从API version 9开始，该接口支持在ArkTS卡片中使用。

**参数：**

| 参数名       | 参数类型                                    | 必填 | 参数描述                                                    |
| ------------ | ------------------------------------------- | ---- | ----------------------------------------------------------- |
| alignContent | [Alignment](ts-appendix-enums.md#alignment) | 否   | 设置子组件在容器内的对齐方式。<br/>默认值：Alignment.Center |

## 属性

除支持[通用属性](ts-universal-attributes-size.md)外，还支持以下属性：

| 名称         | 参数类型                                    | 描述                                                         |
| ------------ | ------------------------------------------- | ------------------------------------------------------------ |
| alignContent | [Alignment](ts-appendix-enums.md#alignment) | 设置子组件在容器内的对齐方式。<br/>默认值：Alignment.Center<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。<br/>**说明：** <br/>该属性与[通用属性align](ts-universal-attributes-location.md)同时设置时，后设置的属性生效。 |


## 示例

```ts
// xxx.ets
@Entry
@Component
struct StackExample {
  build() {
    Stack({ alignContent: Alignment.Bottom }) {
      Text('First child, show in bottom').width('90%').height('100%').backgroundColor(0xd2cab3).align(Alignment.Top)
      Text('Second child, show in top').width('70%').height('60%').backgroundColor(0xc1cbac).align(Alignment.Top)
    }.width('100%').height(150).margin({ top: 5 })
  }
}
```

![zh-cn_image_0000001219982699](figures/zh-cn_image_0000001219982699.PNG)
