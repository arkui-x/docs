# TextArea

The **\<TextArea>** component provides multi-line text input and automatically wraps text so that each line does not have more than the width of the component.

>  **NOTE**
>
>  This component is supported since API version 7. Updates will be marked with a superscript to indicate their earliest API version.


## Child Components

Not supported


## APIs

TextArea(value?:{placeholder?: ResourceStr, text?: ResourceStr, controller?: TextAreaController})

**Parameters**

| Name                    | Type                                    | Mandatory  | Description          |
| ----------------------- | ---------------------------------------- | ---- | -------------- |
| placeholder      | [ResourceStr](ts-types.md#resourcestr)  | No   | Placeholder text displayed when no text input is set.<br>When only the **placeholder** attribute is set, the text selection handle is still available; the caret stays at the beginning of the placeholder text when the handle is released.    |
| text             | [ResourceStr](ts-types.md#resourcestr)  | No   | Current text input.<br>If the component has [stateStyles](ts-universal-attributes-polymorphic-style.md) or any other attribute that may trigger updating configured, you are advised to bind the state variable to the text in real time through the **onChange** event,<br>so as to prevent display errors when the component is updated.<br>Since API version 10, this parameter supports [$$](../../quick-start/arkts-two-way-sync.md) for two-way binding of variables.|
| controller<sup>8+</sup> | [TextAreaController](#textareacontroller8) | No   | Text area controller.|


## Attributes

Among the [universal attributes](ts-universal-attributes-size.md) and [universal text attributes](ts-universal-attributes-text-style.md), **fontColor**, **fontSize**, **fontStyle**, **fontWeight**, and **fontFamily** are supported. In addition, the following attributes are supported.

| Name                     | Type                                                    | Description                                                        |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| placeholderColor          | [ResourceColor](ts-types.md#resourcecolor)                   | Placeholder text color.<br>The default value follows the theme.              |
| placeholderFont           | [Font](ts-types.md#font)                                     | Placeholder text style, including the font size, font width, font family, and font style. Currently, only the default font family is supported.|
| textAlign                 | [TextAlign](ts-appendix-enums.md#textalign)                  | Horizontal alignment of the text.<br>Default value: **TextAlign.Start**<br>**NOTE**<br>Available options are **TextAlign.Start**, **TextAlign.Center**, and **TextAlign.End**.<br>To set vertical alignment for the text, use the [align](ts-universal-attributes-location.md) attribute. The **align** attribute alone does not control the horizontal position of the text. In other words, **Alignment.TopStart**, **Alignment.Top**, and **Alignment.TopEnd** produce the same effect, top-aligning the text; **Alignment.Start**, **Alignment.Center**, and **Alignment.End** produce the same effect, centered-aligning the text vertically; **Alignment.BottomStart**, **Alignment.Bottom**, and **Alignment.BottomEnd** produce the same effect, bottom-aligning the text.|
| caretColor                | [ResourceColor](ts-types.md#resourcecolor)                   | Color of the caret in the text box.<br>Default value: **'#007DFF'**                |
| inputFilter<sup>8+</sup>  | {<br>value: [ResourceStr](ts-types.md#resourcestr),<br>error?: (value: string) => void<br>} | Regular expression for input filtering. Only inputs that comply with the regular expression can be displayed. Other inputs are filtered out. The specified regular expression can match single characters, but not strings.<br>- **value**: regular expression to set.<br>- **error**: filtered-out content to return when regular expression matching fails.|
| copyOption<sup>9+</sup>   | [CopyOptions](ts-appendix-enums.md#copyoptions9)             | Whether copy and paste is allowed.<br>Default value: **CopyOptions.LocalDevice**<br>If this attribute is set to **CopyOptions.None**, the paste operation is allowed, but the copy and cut operations are not.|
| enableKeyboardOnFocus<sup>10+</sup> | boolean | Whether to enable the input method when the component obtains focus.<br>Default value: **true**  |
| selectionMenuHidden<sup>10+</sup> | boolean | Whether to display the text selection menu when the text box is long-pressed or right-clicked.<br>Default value: **false**|

>  **NOTE**
>
>  The default value of the universal attribute [padding](ts-universal-attributes-size.md) is as follows: <br>{<br> top: 8 vp,<br> right: 16 vp,<br> bottom: 8 vp,<br> left: 16 vp<br> }

## Events

In addition to the [universal events](ts-universal-events-click.md), the following events are supported.

| Name                                                         | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| onChange(callback: (value: string) =&gt; void)               | Triggered when the input in the text box changes.<br>- **value**: text entered. |
| onEditChange(callback: (isEditing: boolean) =&gt; void)<sup>10+</sup> | Triggered when the input status changes.                     |
| onCopy<sup>8+</sup>(callback:(value: string) =&gt; void)     | Triggered when the copy button on the pasteboard, which displays when the text box is long pressed, is clicked.<br>- **value**: text to be copied. |
| onCut<sup>8+</sup>(callback:(value: string) =&gt; void)      | Triggered when the cut button on the pasteboard, which displays when the text box is long pressed, is clicked.<br>- **value**: text to be cut. |
| onPaste<sup>8+</sup>(callback:(value: string) =&gt; void)    | Triggered when the paste button on the pasteboard, which displays when the text box is long pressed, is clicked.<br>- **value**: text to be pasted. |
| onTextSelectionChange(callback: (selectionStart: number, selectionEnd: number) => void)<sup>10+</sup> | Triggered when the text selection position changes.<br>**selectionStart**: start position of the text selection area. The start position of text in the text box is **0**.<br>**selectionEnd**: end position of the text selection area.|
| onContentScroll(callback: (totalOffsetX: number, totalOffsetY: number) => void)<sup>10+</sup> | Triggered when the text content is scrolled.<br>**totalOffsetX**: X coordinate offset of the text in the content area.<br>**totalOffsetY**: Y coordinate offset of the text in the content area.|

## TextAreaController<sup>8+</sup>

Defines the controller for controlling the **\<TextArea>** component. Currently, the controller can be used to control the caret position.

### Objects to Import

```
controller: TextAreaController = new TextAreaController()
```

### caretPosition<sup>8+</sup>

caretPosition(value: number): void

Sets the position of the caret.

**Parameters**

| Name| Type| Mandatory| Description                              |
| ------ | -------- | ---- | -------------------------------------- |
| value  | number   | Yes  | Length from the start of the string to the position where the caret is located.|

### setTextSelection<sup>10+</sup>

setTextSelection(selectionStart: number, selectionEnd: number): void

Sets the text selection range and highlights the selected text when the component is focused. This API works only when the value of **selectionStart** is less than that of **selectionEnd**.

**Parameters**

| Name        | Type| Mandatory| Description                                                    |
| -------------- | -------- | ---- | ------------------------------------------------------------ |
| selectionStart | number   | Yes  | Start position of the text selection range. The start position of text in the text box is 0.<br>A value less than 0 evaluates to the value **0**. A value greater than the maximum text length evaluates to the maximum text length.<br>|
| selectionEnd   | number   | Yes  | End position of the text selection range.<br>A value less than 0 evaluates to the value **0**. A value greater than the maximum text length evaluates to the maximum text length.<br>|

### stopEditing<sup>10+</sup>

stopEditing(): void

Exits the editing state.

## Example

```ts
// xxx.ets
@Entry
@Component
struct TextAreaExample {
  @State text: string = ''
  controller: TextAreaController = new TextAreaController()

  build() {
    Column() {
      TextArea({
        text: this.text,
        placeholder: 'The text area can hold an unlimited amount of text. input your word...',
        controller: this.controller
      })
        .placeholderFont({ size: 16, weight: 400 })
        .width(336)
        .height(56)
        .margin(20)
        .fontSize(16)
        .fontColor('#182431')
        .backgroundColor('#FFFFFF')
        .onChange((value: string) => {
          this.text = value
        })
      Text(this.text)
      Button('Set caretPosition 1')
        .backgroundColor('#007DFF')
        .margin(15)
        .onClick(() => {
          // Move the caret to after the first entered character.
          this.controller.caretPosition(1)
        })
    }.width('100%').height('100%').backgroundColor('#F1F3F5')
  }
}
```

![textArea](figures/textArea.gif)
