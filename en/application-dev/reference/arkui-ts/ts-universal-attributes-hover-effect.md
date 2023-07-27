# Hover Effect

The hover effect is applied to a component in hover state.

>  **NOTE**
>
> The APIs of this module are supported since API version 8. Updates will be marked with a superscript to indicate their earliest API version.


## Attributes

| Name         | Type                                              | Description                                               |
| ----------- | --------------------------------------------------| ------------------------------------------------ |
| hoverEffect | [HoverEffect](ts-appendix-enums.md#hovereffect8)  | Hover effect of the component in hover state.<br>Default value: **HoverEffect.Auto**|


## Example

```ts
// xxx.ets
@Entry
@Component
struct HoverExample {
  @State isHoverVal: boolean = false

  build() {
    Column({ space: 5 }) {
      Column({ space: 5 }) {
        Text('Scale').fontSize(20).fontColor(Color.Gray).width('90%').position({ x: 0, y: 80 })
        Column()
          .width('80%').height(200).backgroundColor(Color.Gray)
          .position({ x: 40, y: 120 })
          .hoverEffect(HoverEffect.Scale)
          .onHover((isHover: boolean) => {
            console.info('Scale isHover: ' + isHover)
            this.isHoverVal = isHover
          })

        Text('Board').fontSize(20).fontColor(Color.Gray).width('90%').position({ x: 0, y: 380 })
        Column()
          .width('80%').height(200).backgroundColor(Color.Gray)
          .hoverEffect(HoverEffect.Highlight)
          .position({ x: 40, y: 420 })
          .onHover((isHover: boolean) => {
            console.info('Highlight isHover: ' +isHover )
            this.isHoverVal = isHover
          })
      }
      .hoverEffect(HoverEffect.None)
      .width('100%').height('100%').border({ width: 1 })
      .onHover((isHover: boolean) => {
        console.info('HoverEffect.None')
        this.isHoverVal = isHover
      })
    }
  }
}
```
