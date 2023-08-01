# TextPicker

The **\<TextPicker>** component allows users to scroll to select text.

>  **NOTE**
>
>  This component is supported since API version 8. Updates will be marked with a superscript to indicate their earliest API version.


## Child Components

Not supported


## APIs

TextPicker(options?: {range: string[] | string[][] | Resource | [TextPickerRangeContent](#textpickerrangecontent10)[], selected?: number, value?: string})

Creates a text picker based on the selection range specified by **range**.

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| range | string[] \| string[][]<sup>10+</sup> \| [Resource](ts-types.md#resource) | Yes| Data selection range of the picker. This parameter cannot be set to an empty array. If set to an empty array, it will not be displayed. If it is dynamically changed to an empty array, the current value remains displayed.<br>**NOTE**<br>For a single-column picker, use a value of the string[] or Resource type.<br>For a multi-column picker, use a value of the string[] type.<br>The Resource type supports only [strarray.json](../../quick-start/resource-categories-and-access.md#resource-group-subdirectories). |
| selected | number \| number[]<sup>10+</sup> | No| Index of the default item in the range.<br>Default value: **0**<br>**NOTE**<br>For a single-column picker, use a value of the number type.<br>For a multi-column (linked) picker, use a value of the number[] type.<br>Since API version 10, this parameter supports [$$](../../quick-start/arkts-two-way-sync.md) for two-way binding of variables.|
| value | string \| string[]<sup>10+</sup> | No| Value of the default item in the range. The priority of this parameter is lower than that of **selected**.<br>Default value: value of the first item<br>**NOTE**<br>This parameter works only when the picker contains text only.  <br>For a single-column picker, use a value of the string type.<br>For a multi-column (linked) picker, use a value of the string[] type.<br>Since API version 10, this parameter supports [$$](../../quick-start/arkts-two-way-sync.md) for two-way binding of variables. |

## TextPickerRangeContent<sup>10+</sup>

| Name| Type                                                | Mandatory| Description  |
| ------ | -------------------------------------------------------- | ---- | ---------- |
| icon   | string \| [Resource](ts-types.md#resource) | Yes  | Image resource.|
| text   | string \| [Resource](ts-types.md#resource) | No  | Text information.|

## Attributes

In addition to the [universal attributes](ts-universal-attributes-size.md), the following attributes are supported.

| Name| Type| Description|
| -------- | -------- | -------- |
| defaultPickerItemHeight | number \| string | Height of each item in the picker.|
| disappearTextStyle<sup>10+</sup> | [PickerTextStyle](ts-basic-components-datepicker.md#pickertextstyle10) | Font color, font size, and font width for the top and bottom items.<br>Default value:<br>{<br>color: '#ff182431',<br>font: {<br>size: '14fp', <br>weight: FontWeight.Regular<br>}<br>} |
| textStyle<sup>10+</sup> | [PickerTextStyle](ts-basic-components-datepicker.md#pickertextstyle10) | Font color, font size, and font width of all items except the top, bottom, and selected items.<br>Default value:<br>{<br>color: '#ff182431',<br>font: {<br>size: '16fp', <br>weight: FontWeight.Regular<br>}<br>} |
| selectedTextStyle<sup>10+</sup> | [PickerTextStyle](ts-basic-components-datepicker.md#pickertextstyle10) | Font color, font size, and font width of the selected item.<br>Default value:<br>{<br>color: '#ff007dff',<br>font: {<br>size: '20vp', <br>weight: FontWeight.Medium<br>}<br>} |
| selectedIndex<sup>10+</sup> | number \| number[] | Sets the index value of the default selected item in the array. The priority is higher than that of the selected value in options.<br>**NOTE**<br>For a single-column picker, use a value of the number type. For a multi-column (linked) picker, use a value of the number[] type.|

## Events

In addition to the [universal events](ts-universal-events-click.md), the following events are supported.

| Name| Description|
| -------- | -------- |
| onChange(callback: (value: string \| string[]<sup>10+</sup>, index: number \| number[]<sup>10+</sup>) =&gt; void) | Triggered when an item in the picker is selected.<br>- **value**: value of the selected item. (For a multi-column picker, **value** is of the array type.)<br>- **index**: index of the selected item. (For a multi-column picker, **index** is of the array type.)<br>**NOTE**<br>When the picker contains text only or both text and imagery, **value** indicates the text value of the selected item.When the picker contains imagery only, **value** is empty.|


## Example

```ts
// xxx.ets
@Entry
@Component
struct TextPickerExample {
  private select: number = 1
  private fruits: string[] = ['apple1', 'orange2', 'peach3', 'grape4']

  build() {
    Column() {
      TextPicker({ range: this.fruits, selected: this.select })
        .onChange((value: string, index: number) => {
          console.info('Picker item changed, value: ' + value + ', index: ' + index)
        })
    }
  }
}
```

![en-us_image_0000001257058417](figures/en-us_image_0000001257058417.png)
