# AlphabetIndexer

The **\<Indexer>** component can create a logically indexed array of items in a container for instant location.

>  **NOTE**
>
>  This component is supported since API version 7. Updates will be marked with a superscript to indicate their earliest API version.


## Child Components

Not supported


## APIs

AlphabetIndexer(value: {arrayValue: Array&lt;string&gt;, selected: number})

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| arrayValue | Array&lt;string&gt; | Yes| Array of strings to be displayed in the alphabetic index bar. The value cannot be null.|
| selected   | number              | Yes   | Index of the initially selected item. If the value exceeds the value range, the default value 0 is used.<br>Since API version 10, this parameter supports [$$](../../quick-start/arkts-two-way-sync.md) for two-way binding of variables.|

## Attributes

In addition to the [universal attributes](ts-universal-attributes-size.md), the following attributes are supported.

| Name                 | Type    | Description                                                                   |
| ----------------------- | --------------------| ------------------------------------------------------------------|
| color                   | [ResourceColor](ts-types.md#resourcecolor)       | Font color.<br>Default value: **0x99000000**                          |
| selectedColor           | [ResourceColor](ts-types.md#resourcecolor)     | Font color of the selected text.<br>Default value: **0xFF254FF7**                          |
| popupColor              | [ResourceColor](ts-types.md#resourcecolor)        | Font color of the pop-up text.<br>Default value: **0xFF254FF7**                        |
| selectedBackgroundColor | [ResourceColor](ts-types.md#resourcecolor)       | Background color of the selected item.<br>Default value: **0x1F0A59F7**                          |
| popupBackground         | [ResourceColor](ts-types.md#resourcecolor)        | Background color of the pop-up text.<br>Default value: **0xFFFFFFFF**                           |
| usingPopup              | boolean                                  | Whether to use pop-up text.<br>Default value: **false**                        |
| selectedFont            | [Font](ts-types.md#font) | Font style of the selected text.<br>Default value:<br>{<br>size:'12.0fp',<br> style:FontStyle.Normal,<br> weight:FontWeight.Normal,<br> family:'HarmonyOS Sans'<br>}                          |
| popupFont               | [Font](ts-types.md#font) | Font style of the pop-up text.<br>Default value:<br>{<br>size:'24.0vp',<br> style:FontStyle.Normal,<br> weight:FontWeight.Normal,<br> family:'HarmonyOS Sans'<br>}                         |
| font                    | [Font](ts-types.md#font) | Default font style of the alphabetic index bar.<br>Default value:<br>{<br>size:'12.0fp',<br> style:FontStyle.Normal,<br> weight:FontWeight.Normal,<br> family:'HarmonyOS Sans'<br>}                      |
| itemSize                | string \| number            | Size of an item in the alphabetic index bar. The item is a square, and the side length needs to be set. This attribute cannot be set to a percentage.<br>Default value: **16.0**<br>Unit: vp|
| alignStyle              | value: [IndexerAlign](#indexeralign),<br>offset<sup>10+</sup>?: [Length](ts-types.md#length) | Alignment style of the alphabetic index bar.<br>**value**: alignment of the alphabetic index bar with the pop-up window, which can be left-aligned or right-aligned.<br>Default value: **IndexerAlign.Right**<br>**offset**: spacing between the pop-up window and the alphabetic index bar. A value greater than or equal to 0 is valid. If this parameter is set to a value less than 0 or is not set, the spacing is the same as **popupPosition.x**. If this parameter and **popupPosition** are set at the same time, **offset** takes effect in the horizontal direction and **popupPosition.y** takes effect in the vertical direction.|
| selected | number | Index of the selected item.<br>Default value: **0**<br>Since API version 10, this parameter supports [$$](../../quick-start/arkts-two-way-sync.md) for two-way binding of variables.|
| popupPosition | [Position](ts-types.md#position8) | Position of the pop-up window relative to the center of the indexer bar's top border.<br>Default value: **{x:60.0, y:48.0}**|
| popupSelectedColor<sup>10+</sup> | [ResourceColor](ts-types.md#resourcecolor) | Color of the selected text excluding the initial letter in the pop-up window.<br>Default value: **#FF182431**|
| popupUnselectedColor<sup>10+</sup> | [ResourceColor](ts-types.md#resourcecolor) | Color of the unselected text in the pop-up window.<br>Default value: **#FF182431**|
| popupItemFont<sup>10+</sup> | [Font](ts-types.md#font) | Font of the text excluding the initial letter in the pop-up window.<br>Default value:<br>{<br>size:24,<br>style:FontStyle.Medium<br>}|
| popupItemBackgroundColor<sup>10+</sup> | [ResourceColor](ts-types.md#resourcecolor) | Background color of the portion excluding the initial letter in the pop-up window.<br>Default value: **#FFFFFF**|

## IndexerAlign

| Name| Description|
| -------- | -------- |
| Left | The pop-up window is displayed on the right of the alphabetic indexer bar.|
| Right | The pop-up window is displayed on the left of the alphabetic indexer bar.|

## Events

In addition to the [universal events](ts-universal-events-click.md), the following events are supported.

| Name| Description|
| -------- | -------- |
| onSelect(callback: (index: number) =&gt; void)<sup>8+</sup> | Invoked when an item in the alphabetic indexer bar is selected. The return value is the index of the selected item.                                |
| onRequestPopupData(callback: (index: number) =&gt; Array&lt;string&gt;)<sup>8+</sup> | Invoked when a request for displaying content in the index prompt window is sent after an item in the alphabetic indexer bar is selected.<br>The return value is a string array corresponding to the selected index. The string array is displayed vertically in the pop-up window. It can display up to five strings at once and allows scrolling.|
| onPopupSelect(callback: (index: number) =&gt; void)<sup>8+</sup> | Invoked when an item in the index pop-up window is selected.                           |


## Example

```ts
// xxx.ets
@Entry
@Component
struct AlphabetIndexerSample {
  private arrayA:string[] = ['Ann']
  private arrayB: string[] = ['Ben', 'Bob']
  private arrayC: string[] = ['Calvin', 'Cameron', 'Charlie', 'Charlotte']
  private arrayL: string[] = ['Daisy', 'Daniel', 'Darla', 'David', 'Derek', 'Dorothy', 'Duke']
  private value: string[] = ['#', 'A', 'B', 'C', 'D', 'E', 'F', 'G',
  'H', 'I', 'J', 'K', 'L', 'M', 'N',
  'O', 'P', 'Q', 'R', 'S', 'T', 'U',
  'V', 'W', 'X', 'Y', 'Z']

  build() {
    Stack({ alignContent: Alignment.Start }) {
      Row() {
        List({ space: 20, initialIndex: 0 }) {
          ForEach(this.arrayA, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(30)
                .textAlign(TextAlign.Center)
            }.editable(true)
          }, (item: string) => item)

          ForEach(this.arrayB, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(30)
                .textAlign(TextAlign.Center)
            }.editable(true)
          }, (item: string) => item)

          ForEach(this.arrayC, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(30)
                .textAlign(TextAlign.Center)
            }.editable(true)
          }, (item: string) => item)

          ForEach(this.arrayL, (item: string) => {
            ListItem() {
              Text(item)
                .width('80%')
                .height('5%')
                .fontSize(30)
                .textAlign(TextAlign.Center)
            }.editable(true)
          }, (item: string) => item)
        }
        .width('50%')
        .height('100%')

        AlphabetIndexer({ arrayValue: this.value, selected: 0 })
          .selectedColor(0xFFFFFF) // Font color of the selected text.
          .popupColor(0xFFFAF0) // Font color of the pop-up text.
          .selectedBackgroundColor(0xCCCCCC) // Background color of the selected item.
          .popupBackground(0xD2B48C) // Background color of the pop-up text.
          .usingPopup(true) // Whether to use pop-up text.
          .selectedFont({size: 16, weight: FontWeight.Bolder}) // Font style of the selected text.
          .popupFont({ size: 30, weight: FontWeight.Bolder}) // Font style of the pop-up text.
          .itemSize(28) // Size of an item in the alphabetic index bar.
          .alignStyle(IndexerAlign.Left) // The pop-up window is displayed on the right of the alphabetic index bar.
          .popupSelectedColor(0x00FF00)
          .popupUnselectedColor(0x0000FF)
          .popupItemFont({ size: 30, style: FontStyle.Normal })
          .popupItemBackgroundColor(0xCCCCCC)
          .onSelect((index: number) => {
            console.info(this.value[index] + ' Selected!')
          })
          .onRequestPopupData((index: number) => {
            if (this.value[index] == 'A') {
              return this.arrayA // When index A is selected, the pop-up window displays arrayA corresponding to index A. The same applies when other indexes are selected.
            } else if (this.value[index] == 'B') {
              return this.arrayB
            } else if (this.value[index] == 'C') {
              return this.arrayC
            } else if (this.value[index] == 'L') {
              return this.arrayL
            } else {
              return [] // When no array is available for the selected index, the pop-up window is empty.
            }
          })
          .onPopupSelect((index: number) => {
            console.info('onPopupSelected:' + index)
          })
      }
      .width('100%')
      .height('100%')
    }
  }
}
```

![alphabet](figures/alphabet.gif)
