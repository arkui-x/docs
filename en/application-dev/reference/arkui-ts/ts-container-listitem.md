# ListItem

The **\<ListItem>** component displays specific items in the list. It must be used together with **\<List>**.

>  **NOTE**
>
>  This component is supported since API version 7. Updates will be marked with a superscript to indicate their earliest API version.


## Child Components

This component can contain a single child component.

## APIs

Since API version 9, this API is supported in ArkTS widgets.

**API 1**: ListItem(value?: ListItemOptions)<sup>10+</sup>

**Parameters**

| Name| Type                                     | Mandatory| Description                                                    |
| ------ | --------------------------------------------- | ---- | ------------------------------------------------------------ |
| value  | [ListItemOptions](#listitemoptions10) | No  | Value of the list item, containing the **style** parameter of the **ListItemStyle**enum type.|

**API 2**: ListItem(value?: string)<sup>(deprecated)</sup>

This API is deprecated since API version 10. You are advised to use API 1 instead.

**Parameters**

| Name| Type                     | Mandatory| Description|
| ------ | ----------------------------- | ---- | -------- |
| value  | string<sup>(deprecated)</sup> | No  | N/A      |

## Attributes

In addition to the [universal attributes](ts-universal-attributes-size.md), the following attributes are supported.

| Name| Type| Description|
| -------- | -------- | -------- |
| selectable<sup>8+</sup> | boolean | Whether the current list item is selectable by mouse drag.<br>**NOTE**<br>This attribute takes effect only when mouse frame selection is enabled for the parent **\<List>** container.<br>Default value: **true**|
| selected<sup>10+</sup> | boolean | Whether the list item is selected. This attribute supports [$$](../../quick-start/arkts-two-way-sync.md) for two-way binding of variables.<br>**NOTE**<br>This attribute must be used before the [style for the selected state](./ts-universal-attributes-polymorphic-style.md) is set. Otherwise, the style settings will not take effect.<br>Default value: **false**|
| swipeAction<sup>9+</sup> | {<br>start?: CustomBuilder \| [SwipeActionItem](#swipeactionitem10),<br>end?:CustomBuilder \| [SwipeActionItem](#swipeactionitem10),<br>edgeEffect?: [SwipeEdgeEffect](#swipeedgeeffect9),<br>} | Swipe action displayed when the list item is swiped out from the screen edge.<br>- **start**: swipe action displayed on the left of the list item when the item is swiped right (in vertical list layout) or above the list item when the item is swiped down (in horizontal list layout).<br>- **end**: swipe action displayed on the right of the list item when the item is swiped left (in vertical list layout) or below the list item when the item is swiped up (in horizontal list layout).<br>- **edgeEffect**: scroll effect.<br>**NOTE**<br>- The top level of the **@builder** function corresponding to **start** and **end** must be a single component and cannot be an **if/else**, **ForEach**, or **LazyForEach** statement.<br> - The swipe gesture works only in the list item area. If a swipe causes a child component to extend beyond the list item area, the portion outside the area does not respond to the swipe. In light of this, avoid setting **swipeAction** to a component too wide in a multi-column list. |

## SwipeEdgeEffect<sup>9+</sup>
| Name| Description|
| -------- | -------- |
| Spring | When the list item scrolls to the edge of the list, it can continue to scroll for a distance. If the delete area is set, the list item can continue to scroll after the scroll distance reaches the delete threshold and, after being released, rebound following the spring curve.|
| None | The list item cannot scroll beyond the edge of the list. If the delete area is set, the list item cannot continue to scroll after the scroll distance reaches the delete threshold. If the delete callback is set, it is triggered when the delete threshold is reached and the list item is released.|

## SwipeActionItem<sup>10+</sup>
Describes the swipe action item.

For a list in vertical layout, it refers to the delete item displayed on the left (or right) of the list item when the list item is swiped right (or left).

For a list in horizontal layout, it refers to the delete item displayed below (or above) the list item when the list item is swiped up (or down).

| Name                | Type                                                    | Mandatory| Description                                                        |
| -------------------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| deleteAreaDistance | [Length](ts-types.md#length) | No| Swipe distance threshold for deleting the list item.<br>Default value: **56vp**<br>**NOTE**<br>This parameter cannot be set in percentage.<br>If the value is greater than the list item width minus the width of **swipeAction**, or is less than or equal to 0, the delete area will not be set.|
| onDelete | () => void | No| Callback invoked when the list item is released while in the delete area.<br>**NOTE**<br> This callback is invoked only when the list item is released in a position that meets or goes beyond the specified swipe distance threshold (which must be valid) for deleting the list item.|
| onEntryDeleteArea | () => void | No| Callback invoked each time the list item enters the delete area.|
| onExitDeleteArea | () => void | No|Callback invoked each time the list item exits the delete area.|
| builder |  CustomBuilder | No|Swipe action item displayed when the list item is swiped left or right (in vertical list layout) or up or down (in horizontal list layout).|
| useDefaultDeleteAnimation | boolean | No|Whether to use the default delete animation.<br>Default value: **true**|

## ListItemOptions<sup>10+</sup>

| Name | Type                                 | Mandatory| Description                                                        |
| ----- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| style | [ListItemStyle](#listitemstyle10) | No  | Style of the list item.<br>Default value: **ListItemStyle.NONE**<br>If this parameter is set to **ListItemStyle.NONE**, no style is applied.<br>If this parameter is set to **ListItemStyle.CARD**, the default card style is applied, but only when **ListItemGroupStyle.CARD** is set for [\<ListItemGroup>](ts-container-listitemgroup.md).<br>In the default card style, the list item has a 48 vp height and 100% width. It can be in focus, hover, press, selected, or disable style depending on the state.<br>**NOTE**<br>In the default card style, the list has its **listDirection** attribute fixed at **Axis.Vertical** and **alignListItem** attribute at **ListItemAlign.Center**. |

## ListItemStyle<sup>10+</sup>

| Name| Description              |
| ---- | ------------------ |
| NONE | No style.          |
| CARD | Default card style.|

## Events

| Name| Description|
| -------- | -------- |
| onSelect(event: (isSelected: boolean) =&gt; void)<sup>8+</sup> | Triggered when the selected state of the **\<ListItem>** changes.<br>**isSelected**: Returns **true** if the **\<ListItem>** is selected by mouse drag; returns **false** otherwise.|

## Example

### Example 1

```ts
// xxx.ets
@Entry
@Component
struct ListItemExample {
  private arr: number[] = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

  build() {
    Column() {
      List({ space: 20, initialIndex: 0 }) {
        ForEach(this.arr, (item) => {
          ListItem() {
            Text('' + item)
              .width('100%').height(100).fontSize(16)
              .textAlign(TextAlign.Center).borderRadius(10).backgroundColor(0xFFFFFF)
          }
        }, item => item)
      }.width('90%')
      .scrollBar(BarState.Off)
    }.width('100%').height('100%').backgroundColor(0xDCDCDC).padding({ top: 5 })
  }
}
```

![en-us_image_0000001219864159](figures/en-us_image_0000001219864159.gif)

### Example 2


```ts
// xxx.ets
@Entry
@Component
struct ListItemExample2 {
  @State message: string = 'Hello World'
  @State arr: number[] = [0, 1, 2, 3, 4]
  @State enterEndDeleteAreaString: string = "not enterEndDeleteArea"
  @State exitEndDeleteAreaString: string = "not exitEndDeleteArea"

  @Builder itemEnd() {
    Row() {
      Button("Delete").margin("4vp")
      Button("Set").margin("4vp")
    }.padding("4vp").justifyContent(FlexAlign.SpaceEvenly)
  }

  build() {
    Column() {
      List({ space: 10 }) {
        ForEach(this.arr, (item) => {
          ListItem() {
            Text("item" + item)
              .width('100%')
              .height(100)
              .fontSize(16)
              .textAlign(TextAlign.Center)
              .borderRadius(10)
              .backgroundColor(0xFFFFFF)
          }
          .transition({ type: TransitionType.Delete, opacity: 0 })
          .swipeAction({
            end: {
              builder: this.itemEnd.bind(this, item),
              useDefaultDeleteAnimation: true,
              onDelete: () => {
                animateTo({ duration: 1000 }, () => {
                  let index = this.arr.indexOf(item)
                  this.arr.splice(index, 1)
                })
              },
              deleteAreaDistance: 80,
              onEnterDeleteArea: () => {
                this.enterEndDeleteAreaString = "enterEndDeleteArea"
                this.exitEndDeleteAreaString = "not exitEndDeleteArea"
              },
              onExitDeleteArea: () => {
                this.enterEndDeleteAreaString = "not enterEndDeleteArea"
                this.exitEndDeleteAreaString = "exitEndDeleteArea"
              }
            }
          })
        }, item => item)
      }
      Text(this.enterEndDeleteAreaString).fontSize(20)
      Text(this.exitEndDeleteAreaString).fontSize(20)
    }
    .padding(10)
    .backgroundColor(0xDCDCDC)
    .width('100%')
    .height('100%')
  }
}
```
![deleteListItem](figures/deleteListItem.gif)
