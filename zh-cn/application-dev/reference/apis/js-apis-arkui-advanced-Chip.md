# Chip
Chip用于搜索框历史记录、邮件发送列表等场景。

> **说明：**
>
> 该组件从API Version 11开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>

## 导入模块

```ts
import { Chip, ChipOptions, ChipSize } from '@ohos.arkui.advanced.Chip'
```

## 子组件

无

## Chip

Chip(options:ChipOptions): void

**装饰器类型：**@Builder

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数**：

| 参数名  | 类型                        | 必填 | 说明                 |
| ------- | --------------------------- | ---- | -------------------- |
| options | [ChipOptions](#chipoptions) | 是   | 定义Chip组件的参数。 |

## ChipOptions

ChipOptions定义Chip的样式及具体式样参数。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                                    | 类型                                                                                                        | 只读 | 可选 | 说明                                                                                                                                                                                                                                                                                                                                         |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ---- | ---- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| size                                    | [ChipSize](#chipsize) \| [SizeOptions](../../../application-dev/reference/arkui-ts/ts-types.md#sizeoptions) | 否   | 是   | Chip尺寸。<br/>默认值：ChipSize.NORMAL<br/>SizeOptions类型参数不支持百分比设置。 <br/>**说明**：[适老化](../../../ui/arkui-support-for-aging-adaptation.md)在size指定具体宽高时不生效，size设置为{ height: 0, width: 0 }除外。                                                                                                               |
| enabled                                 | boolean                                                                                                     | 否   | 是   | Chip是否可选中。<br>默认值：true。<br>true：操作块可选中；false：操作块不可选中。                                                                                                                                                                                                                                                            |
| activated<sup>12+</sup>                 | boolean                                                                                                     | 否   | 是   | Chip是否为激活态。<br>默认值：false。<br>true：操作块为激活态；false：操作块为非激活态。                                                                                                                                                                                                                                                     |
| prefixIcon                              | [PrefixIconOptions](#prefixiconoptions)                                                                     | 否   | 是   | 前缀图标属性。                                                                                                                                                                                                                                                                                                                               |
| prefixSymbol<sup>12+</sup>              | [ChipSymbolGlyphOptions](#chipsymbolglyphoptions12)                                                         | 否   | 是   | 前缀图标属性，symbol类型。                                                                                                                                                                                                                                                                                                                   |
| label                                   | [LabelOptions](#labeloptions)                                                                               | 否   | 否   | 文本属性。                                                                                                                                                                                                                                                                                                                                   |
| suffixIcon                              | [SuffixIconOptions](#suffixiconoptions)                                                                     | 否   | 是   | 后缀图标属性。                                                                                                                                                                                                                                                                                                                               |
| suffixSymbol<sup>12+</sup>              | [ChipSymbolGlyphOptions](#chipsymbolglyphoptions12)                                                         | 否   | 是   | 后缀图标属性，symbol类型。                                                                                                                                                                                                                                                                                                                   |
| suffixSymbolOptions<sup>14+</sup>       | [ChipSuffixSymbolGlyphOptions](#chipsuffixsymbolglyphoptions14)                                             | 否   | 是   | symbol类型后缀图标属性的无障碍朗读功能属性。                                                                                                                                                                                                                                                                                                 |
| backgroundColor                         | [ResourceColor](../../../application-dev/reference/arkui-ts/ts-types.md#resourcecolor)                      | 否   | 是   | Chip背景颜色。<br/>默认值：$r('sys.color.ohos_id_color_button_normal')。                                                                                                                                                                                                                                                                     |
| activatedBackgroundColor<sup>12+</sup>  | [ResourceColor](../../../application-dev/reference/arkui-ts/ts-types.md#resourcecolor)                      | 否   | 是   | Chip激活时的背景颜色。<br/>默认值：$r('sys.color.ohos_id_color_emphasize')。                                                                                                                                                                                                                                                                 |
| borderRadius                            | [Dimension](../../../application-dev/reference/arkui-ts/ts-types.md#dimension10)                            | 否   | 是   | Chip背景圆角半径大小，不支持百分比。<br/>默认值：$r('sys.float.ohos_id_corner_radius_button')。                                                                                                                                                                                                                                              |
| allowClose                              | boolean                                                                                                     | 否   | 是   | 关闭图标是否显示。<br/>默认值：true<br>true：删除图标显示；false：删除图标不显示。                                                                                                                                                                                                                                                           |
| onClose                                 | ()=>void                                                                                                    | 否   | 是   | 默认关闭图标点击事件。                                                                                                                                                                                                                                                                                                                       |
| onClicked<sup>12+</sup>                 | Callback\<void>                                                                                             | 否   | 是   | Chip点击事件。                                                                                                                                                                                                                                                                                                                               |
| direction<sup>12+</sup>                 | [Direction](js-apis-app-ability-configurationConstant.md#configurationconstantdirection)                    | 否   | 是   | 布局方向。<br/>默认值：Direction.Auto。                                                                                                                                                                                                                                                                                                      |
| closeOptions<sup>14+</sup>              | [CloseOptions](#closeoptions14)                                                                             | 否   | 是   | 默认关闭图标的无障碍朗读功能属性。                                                                                                                                                                                                                                                                                                           |
| accessibilityDescription<sup>14+</sup>  | [ResourceStr](../arkui-ts/ts-types.md#resourcestr)                                                          | 否   | 是   | Chip组件的无障碍描述。此描述用于向用户详细解释当前组件，开发人员应为组件的这一属性提供较为详尽的文本说明，以协助用户理解即将执行的操作及其可能产生的后果。特别是当这些后果无法仅从组件的属性和无障碍文本中直接获知时。如果组件同时具备文本属性和无障碍说明属性，当组件被选中时，系统将首先播报组件的文本属性，随后播报无障碍说明属性的内容。 |
| accessibilityLevel<sup>14+</sup>        | string                                                                                                      | 否   | 是   | Chip组件无障碍重要性。用于控制后缀图标是否可被无障碍辅助服务所识别。<br>支持的值为:<br>"auto"：当前组件会转化为“yes”。<br>"yes"：当前组件可被无障碍辅助服务所识别。<br>"no"：当前组件不可被无障碍辅助服务所识别。<br>"no-hide-descendants"：当前组件及其所有子组件不可被无障碍辅助服务所识别。<br>默认值："auto"。                           |
| accessibilitySelectedType<sup>14+</sup> | [AccessibilitySelectedType](#accessibilityselectedtype14)                                                   | 否   | 是   | Chip组件选中态类型。<br>默认值：当设置了activated属性但未指定accessibilitySelectedType时，默认使用CHECKED类型。当未设置activated属性时，默认使用CLICKED类型。                                                                                                                                                                                |

> **说明：**
>
> 1. 当`suffixSymbol`有传入参数时，`suffixIcon`和`allowClose`不生效；当`suffixSymbol`没有传入参数而`suffixIcon`有传入参数时，`allowClose`不生效；当`suffixSymbol`和`suffixIcon`都没有传入参数时，`allowClose`决定是否显示删除图标。
>2. `backgroundColor`和`activatedBackgroundColor`赋值为`undefined`时，显示默认背景颜色；赋值为非法值时，背景颜色透明。
> 3. prefixSymbol/suffixSymbol的fontColor默认值为：normalFontColor: `[$r('sys.color.ohos_id_color_primary')]`、activatedFontColor: `[$r('sys.color.ohos_id_color_text_primary_contrary')]`。fontColor默认值为16。
>
> 4. prefixIcon的fillColor默认值为：`$r('sys.color.ohos_id_color_secondary')`，suffixIcon的fillColor默认值为：`$r('sys.color.ohos_id_color_primary')`。fillColor对颜色的解析与Image组件保持一致。
>5. prefixIcon和suffixIcon的activatedFillColor默认值均为：`$r('sys.color.ohos_id_color_text_primary_contrary')`。activatedFillColor对颜色的解析与Image组件保持一致。
>6. IOS端chip组件底层，由于最外层button组件accessibilityGroup属性设置为true，button该组件及其所有子组件为一整个可以选中的组件，无障碍服务将不再关注其子组件内容。

## ChipSize

ChipSize是Chip可指定的尺寸类型，如普通型Chip。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称   | 值       | 说明               |
| ------ | -------- | ------------------ |
| NORMAL | "NORMAL" | normal尺寸操作块。 |
| SMALL  | "SMALL"  | small尺寸操作块。  |

## AccessibilitySelectedType<sup>14+</sup>

AccessibilitySelectedType是Chip可指定的选中态类型，用于控制无障碍服务如何向用户传达组件的选中状态。不同的选中态类型提供了不同的语义和用户体验。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称     | 值  | 说明                                                                                                                                                                                              |
| -------- | --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| CLICKED  | 0   | 单击型。组件不向无障碍服务报告任何选中状态，仅作为可单击组件使用。适用于执行某个操作但不保持状态的场景，如普通按钮。                                                                              |
| CHECKED  | 1   | 复选型。组件通过 [accessibilityChecked](ts-universal-attributes-accessibility.md#accessibilitychecked13) 属性向无障碍服务报告选中状态。适用于多选场景，如标签筛选、属性选择等。                   |
| SELECTED | 2   | 单选型。组件通过 [accessibilitySelected](ts-universal-attributes-accessibility.md#accessibilityselected13) 属性向无障碍服务报告选中状态。适用于表示当前选中项的场景，如导航栏标签、单选列表项等。 |

## IconCommonOptions

IconCommonOptions定义图标的共通属性。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                             | 类型                                                                                   | 只读 | 可选 | 说明                                                         |
| -------------------------------- | -------------------------------------------------------------------------------------- | ---- | ---- | ------------------------------------------------------------ |
| src                              | [ResourceStr](../arkui-ts/ts-types.md#resourcestr)                                     | 否   | 否   | 图标图片或图片地址引用。                                     |
| size                             | [SizeOptions](../../../application-dev/reference/arkui-ts/ts-types.md#sizeoptions)     | 否   | 是   | 图标大小，不支持百分比。<br/>默认值：{width: 16, height: 16} |
| fillColor                        | [ResourceColor](../../../application-dev/reference/arkui-ts/ts-types.md#resourcecolor) | 否   | 是   | 图标填充颜色。                                               |
| activatedFillColor<sup>12+</sup> | [ResourceColor](../../../application-dev/reference/arkui-ts/ts-types.md#resourcecolor) | 否   | 是   | 操作块激活时图标填充颜色。                                   |

> **说明：**
>
> 仅在图片格式为SVG时，fillColor和activatedFillColor属性才生效。
>

## PrefixIconOptions

PrefixIconOptions定义前缀图标的属性。

继承于[IconCommonOptions](#iconcommonoptions)。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

## SuffixIconOptions

SuffixIconOptions定义后缀图标的属性。

继承于[IconCommonOptions](#iconcommonoptions)。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                                   | 类型                                               | 只读 | 可选 | 说明                                                                                                                                                                                                                                                                                                                                                             |
| -------------------------------------- | -------------------------------------------------- | ---- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| action                                 | () => void                                         | 否   | 是   | 后缀图标设定事件。                                                                                                                                                                                                                                                                                                                                               |
| accessibilityText<sup>14+</sup>        | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 否   | 是   | 后缀图标无障碍文本属性。当后缀图标不包含文本属性时，屏幕朗读选中后缀图标时不播报，使用者无法清楚地知道当前是否选中了后缀图标。为了解决此场景，开发人员可为不包含文字信息的后缀图标设置无障碍文本，当屏幕朗读选中后缀图标时播报无障碍文本的内容，帮助屏幕朗读的使用者清楚地知道自己是否选中了后缀图标。                                                           |
| accessibilityDescription<sup>14+</sup> | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 否   | 是   | 后缀图标的无障碍描述。此描述用于向用户详细解释后缀图标，开发人员应为后缀图标的这一属性提供较为详尽的文本说明，以协助用户理解即将执行的操作及其可能产生的后果。特别是当这些后果无法仅从后缀图标的属性和无障碍文本中直接获知时。如果后缀图标同时具备文本属性和无障碍说明属性，当后缀图标被选中时，系统将首先播报后缀图标的文本属性，随后播报无障碍说明属性的内容。 |
| accessibilityLevel<sup>14+</sup>       | string                                             | 否   | 是   | 后缀图标的无障碍重要性。用于控制后缀图标是否可被无障碍辅助服务识别。<br>支持的值为:<br>"auto"：当前组件存在action时转化为“yes”，不存在action时，转化为“no”。<br>"yes"：当前组件可被无障碍辅助服务所识别。<br>"no"：当前组件不可被无障碍辅助服务所识别。<br>"no-hide-descendants"：当前组件及其所有子组件不可被无障碍辅助服务所识别。<br>默认值："auto"。         |

## AccessibilityOptions<sup>14+</sup>

后缀图标的无障碍朗读功能属性。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                     | 类型                                               | 只读 | 可选 | 说明                                                                                                                                                                                                                                                                                                 |
| ------------------------ | -------------------------------------------------- | ---- | ---- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| accessibilityText        | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 否   | 是   | 无障碍文本属性。当组件无文本属性时，屏幕朗读选中此组件不会播报，导致使用者无法清楚了解当前选中的组件。开发人员可为此类组件设置无障碍文本，屏幕朗读时将播报该文本，帮助使用者明确选中了什么组件。                                                                                                     |
| accessibilityDescription | [ResourceStr](../arkui-ts/ts-types.md#resourcestr) | 否   | 是   | 无障碍描述。此描述用于向用户详细解释当前组件，开发人员应提供详尽的文本说明，以协助用户理解即将执行的操作及其后果。特别是当这些后果无法仅从组件的属性和无障碍文本中直接获知时。如果组件同时具备文本属性和无障碍说明属性，当组件被选中时，系统将首先播报组件的文本属性，随后播报无障碍说明属性的内容。 |
| accessibilityLevel       | string                                             | 否   | 是   | 无障碍重要性。用于控制组件是否可被无障碍辅助服务识别。<br>支持的值为:<br>"auto"：当前组件会转换为“yes”。<br>"yes"：当前组件可被无障碍辅助服务所识别。<br>"no"：当前组件不可被无障碍辅助服务所识别。<br>"no-hide-descendants"：当前组件及其所有子组件不可被无障碍辅助服务所识别。<br>默认值："auto"。 |

## ChipSuffixSymbolGlyphOptions<sup>14+</sup>

symbol类型后缀图标属性的无障碍朗读功能属性类型。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                   | 类型                                            | 只读 | 可选 | 说明                         |
| ---------------------- | ----------------------------------------------- | ---- | ---- | ---------------------------- |
| action                 | VoidCallback                                    | 否   | 是   | 后缀图标设定事件。           |
| normalAccessibility    | [AccessibilityOptions](#accessibilityoptions14) | 否   | 是   | 非激活态无障碍朗读功能属性。 |
| activatedAccessibility | [AccessibilityOptions](#accessibilityoptions14) | 否   | 是   | 激活态无障碍朗读功能属性。   |

## ChipSymbolGlyphOptions<sup>12+</sup>

ChipSymbolGlyphOptions定义前缀图标和后缀图标的属性。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称      | 类型                                                                 | 只读 | 可选 | 说明             |
| --------- | -------------------------------------------------------------------- | ---- | ---- | ---------------- |
| normal    | [SymbolGlyphModifier](ts-universal-attributes-attribute-modifier.md) | 否   | 是   | 图标设定。       |
| activated | [SymbolGlyphModifier](ts-universal-attributes-attribute-modifier.md) | 否   | 是   | 激活时图标设定。 |

> **说明：**
>
> 不支持使用symbolEffect修改动效类型及effectStrategy设置动效。
>

## LabelOptions

LabelOptions定义文本属性。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                               | 类型                                                                                   | 只读 | 可选 | 说明                                                                                                          |
| ---------------------------------- | -------------------------------------------------------------------------------------- | ---- | ---- | ------------------------------------------------------------------------------------------------------------- |
| text                               | string                                                                                 | 否   | 否   | 文本文字内容。                                                                                                |
| fontSize                           | [Dimension](../../../application-dev/reference/arkui-ts/ts-types.md#dimension10)       | 否   | 是   | 文字字号，不支持百分比。<br/>默认值：$r('sys.float.ohos_id_text_size_button2')                                |
| fontColor                          | [ResourceColor](../../../application-dev/reference/arkui-ts/ts-types.md#resourcecolor) | 否   | 是   | 文字颜色。<br/>默认值：$r('sys.color.ohos_id_color_text_primary')                                             |
| activatedFontColor<sup>12+</sup>   | [ResourceColor](../../../application-dev/reference/arkui-ts/ts-types.md#resourcecolor) | 否   | 是   | 操作块激活时的文字颜色。<br/>默认值：$r('sys.color.ohos_id_color_text_primary_contrary')                      |
| fontFamily                         | string                                                                                 | 否   | 是   | 文字字体。<br/>默认值："HarmonyOS Sans"                                                                       |
| labelMargin                        | [LabelMarginOptions](#labelmarginoptions)                                              | 否   | 是   | 文本与左右侧图标之间间距。                                                                                    |
| localizedLabelMargin<sup>12+</sup> | [LocalizedLabelMarginOptions](#localizedlabelmarginoptions12)                          | 否   | 是   | 本地化文本与左右侧图标之间间距。<br/>默认值：{<br/>start: LengthMetrics.vp(6), end: LengthMetrics.vp(6)<br/>} |

## CloseOptions<sup>14+</sup>

默认关闭图标的无障碍朗读功能属性，accessibilityText默认为"删除"。

继承于[AccessibilityOptions](#accessibilityoptions14)。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

## LabelMarginOptions

LabelMarginOptions用于定义文本与左右侧图标之间间距。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称  | 类型                                                                             | 只读 | 可选 | 说明                                   |
| ----- | -------------------------------------------------------------------------------- | ---- | ---- | -------------------------------------- |
| left  | [Dimension](../../../application-dev/reference/arkui-ts/ts-types.md#dimension10) | 否   | 是   | 文本与左侧图标之间间距，不支持百分比。 |
| right | [Dimension](../../../application-dev/reference/arkui-ts/ts-types.md#dimension10) | 否   | 是   | 文本与右侧图标之间间距，不支持百分比。 |

## LocalizedLabelMarginOptions<sup>12+</sup>

LocalizedLabelMarginOptions用于定义本地化文本与左右侧图标之间间距。

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称  | 类型          | 只读 | 可选 | 说明                                   |
| ----- | ------------- | ---- | ---- | -------------------------------------- |
| start | LengthMetrics | 否   | 是   | 文本与左侧图标之间间距，不支持百分比。 |
| end   | LengthMetrics | 否   | 是   | 文本与右侧图标之间间距，不支持百分比。 |

## 示例

### 示例1（自定义后缀图标）

通过配置suffixIcon实现自定义操作块的后缀图标。

```ts
import { Chip, ChipSize } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  build() {
    Column({ space: 10 }) {
      Chip({
        prefixIcon: {
          // 'app.media.chips'仅作示例，请替换为实际使用图片。
          src: $r('app.media.chips'),
          size: { width: 16, height: 16 },
          fillColor: Color.Red
        },
        label: {
          text: "操作块",
          fontSize: 12,
          fontColor: Color.Blue,
          fontFamily: "HarmonyOS Sans",
          labelMargin: { left: 20, right: 30 }
        },
        suffixIcon: {
          // 'app.media.close'仅作示例，请替换为实际使用图片。
          src: $r('app.media.close'),
          size: { width: 16, height: 16 },
          fillColor: Color.Red
        },
        size: ChipSize.NORMAL,
        allowClose: false,
        enabled: true,
        backgroundColor: $r('sys.color.ohos_id_color_button_normal'),
        borderRadius: $r('sys.float.ohos_id_corner_radius_button')
      })
    }
  }
}
```


![](figures/chip1.png)

### 示例2（设置默认后缀图标）

配置allowClose为true，显示后缀移除图标。

```ts
import { Chip, ChipSize } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  build() {
    Column({ space: 10 }) {
      Chip({
        prefixIcon: {
          // 'app.media.chips'仅作示例，请替换为实际使用图片。
          src: $r('app.media.chips'),
          size: { width: 16, height: 16 },
          fillColor: Color.Blue
        },
        label: {
          text: "操作块",
          fontSize: 12,
          fontColor: Color.Blue,
          fontFamily: "HarmonyOS Sans",
          labelMargin: { left: 20, right: 30 }
        },
        size: ChipSize.NORMAL,
        allowClose: true,
        enabled: true,
        backgroundColor: $r('sys.color.ohos_id_color_button_normal'),
        borderRadius: $r('sys.float.ohos_id_corner_radius_button')
      })
    }
  }
}
```


![](figures/chip2.png)

### 示例3（不显示后缀图标）

配置allowClose为false，隐藏后缀移除图标。

```ts
import { Chip, ChipSize } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  build() {
    Column({ space: 10 }) {
      Chip({
        prefixIcon: {
          // 'app.media.chips'仅作示例，请替换为实际使用图片。
          src: $r('app.media.chips'),
          size: { width: 16, height: 16 },
          fillColor: Color.Blue
        },
        label: {
          text: "操作块",
          fontSize: 12,
          fontColor: Color.Blue,
          fontFamily: "HarmonyOS Sans",
          labelMargin: { left: 20, right: 30 }
        },
        size: ChipSize.SMALL,
        allowClose: false,
        enabled: true,
        backgroundColor: $r('sys.color.ohos_id_color_button_normal'),
        borderRadius: $r('sys.float.ohos_id_corner_radius_button'),
        onClose: () => {
          console.info("chip on close")
        }
      })
    }
  }
}
```


![](figures/chip3.png)

### 示例4（激活态操作块）

该示例通过配置activated实现激活态操作块。

```ts
import { Chip, ChipSize } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State isActivated: boolean = false;

  build() {
    Column({ space: 10 }) {
      Chip({
        prefixIcon: {
          // 'app.media.chips'仅作示例，请替换为实际使用图片。
          src: $r('app.media.chips'),
          size: { width: 16, height: 16 },
          fillColor: Color.Blue,
          activatedFillColor: $r('sys.color.ohos_id_color_text_primary_contrary')
        },
        label: {
          text: "操作块",
          fontSize: 12,
          fontColor: Color.Blue,
          activatedFontColor: $r('sys.color.ohos_id_color_text_primary_contrary'),
          fontFamily: "HarmonyOS Sans",
          labelMargin: { left: 20, right: 30 }
        },
        size: ChipSize.NORMAL,
        allowClose: true,
        enabled: true,
        activated: this.isActivated,
        backgroundColor: $r('sys.color.ohos_id_color_button_normal'),
        activatedBackgroundColor: $r('sys.color.ohos_id_color_emphasize'),
        borderRadius: $r('sys.float.ohos_id_corner_radius_button'),
        onClose: () => {
          console.info("chip on close")
        },
        onClicked: () => {
          console.info("chip on clicked")
        }
      })

      Button('改变激活状态').onClick(() => {
        this.isActivated = !this.isActivated
      })
    }
  }
}
```


![](figures/chip4.gif)

### 示例5（设置symbol类型图标）

Chip组件的前缀、后缀图标使用symbol类型资源展示。

```ts
import { Chip, ChipSize, SymbolGlyphModifier } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State isActivated: boolean = false

  build() {
    Column({ space: 10 }) {
      Chip({
        prefixIcon: {
          // 'app.media.chips'仅作示例，请替换为实际使用图片。
          src: $r('app.media.chips'),
          size: { width: 16, height: 16 },
          fillColor: Color.Blue,
          activatedFillColor: $r('sys.color.ohos_id_color_text_primary_contrary')
        },
        prefixSymbol: {
          normal: new SymbolGlyphModifier($r('sys.symbol.ohos_star')).fontSize(16).fontColor([Color.Green]),
          activated: new SymbolGlyphModifier($r('sys.symbol.ohos_star')).fontSize(16).fontColor([Color.Red]),
        },
        label: {
          text: "操作块",
          fontSize: 12,
          fontColor: Color.Blue,
          activatedFontColor: $r('sys.color.ohos_id_color_text_primary_contrary'),
          fontFamily: "HarmonyOS Sans",
          labelMargin: { left: 20, right: 30 },
        },
        size: ChipSize.NORMAL,
        allowClose: true,
        enabled: true,
        activated: this.isActivated,
        backgroundColor: $r('sys.color.ohos_id_color_button_normal'),
        activatedBackgroundColor: $r('sys.color.ohos_id_color_emphasize'),
        borderRadius: $r('sys.float.ohos_id_corner_radius_button'),
        onClose: () => {
          console.info("chip on close")
        },
        onClicked: () => {
          console.info("chip on clicked")
        }
      })

      Button('改变激活状态').onClick(() => {
        this.isActivated = !this.isActivated
      })
    }
  }
}
```

![](figures/chip5.gif)

### 示例6（设置镜像效果）

配置direction实现Chip布局镜像化展示。

```ts
import { Chip, ChipSize, LengthMetrics } from '@kit.ArkUI';

@Entry
@Component
struct ChipPage {
  build() {
    Column() {
      Chip({
        direction: Direction.Rtl,
        prefixIcon: {
          // 'app.media.chips'仅作示例，请替换为实际使用图片。
          src: $r('app.media.chips'),
          size: { width: 16, height: 16 },
          fillColor: Color.Red,
        },
        label: {
          text: "操作块",
          fontSize: 12,
          fontColor: Color.Blue,
          fontFamily: "HarmonyOS Sans",
          localizedLabelMargin: { start: LengthMetrics.vp(20), end: LengthMetrics.vp(20) },
        },
        suffixIcon: {
          // 'app.media.close'仅作示例，请替换为实际使用图片。
          src: $r('app.media.close'),
          size: { width: 16, height: 16 },
          fillColor: Color.Red,
        },
        size: ChipSize.NORMAL,
        allowClose: false,
        enabled: true,
        backgroundColor: $r('sys.color.ohos_id_color_button_normal'),
        borderRadius: $r('sys.float.ohos_id_corner_radius_button')
      })
    }.justifyContent(FlexAlign.Center)
    .width('100%')
    .height('100%')
  }
}
```


![](figures/chip6.png)

### 示例7（Image类型无障碍朗读）

该示例代码实现Chip组件Image类型后缀图标的无障碍朗读功能。

```ts
import { Chip } from '@kit.ArkUI';

@Builder
function DefaultFunction(): void {
}

@Component
struct SectionGroup {
  @Prop
  @Require
  title: ResourceStr;
  @BuilderParam
  @Require
  content: () => void = DefaultFunction;

  build() {
    Column({ space: 4 }) {
      Text(this.title)
        .fontColor('#FF666666')
        .fontSize(12)
      Column({ space: 8 }) {
        this.content()
      }
    }
    .alignItems(HorizontalAlign.Start)
    .width('100%')
  }
}

@Component
struct SectionItem {
  @Prop
  @Require
  title: ResourceStr;
  @BuilderParam
  @Require
  content: () => void = DefaultFunction;

  build() {
    Column({ space: 12 }) {
      Text(this.title)
      this.content()
    }
    .backgroundColor('#FFFFFFFF')
    .borderRadius(12)
    .padding(12)
    .width('100%')
  }
}

@Entry
@Component
struct ChipExample2 {
  @State activated: boolean = false;

  build() {
    NavDestination() {
      Scroll() {
        SectionGroup({ title: '后缀图标播报' }) {
          SectionItem({ title: '自定义播报' }) {
            Chip({
              label: { text: '操作块' },
              suffixIcon: {
                src: $r('sys.media.ohos_ic_public_cut'),
                accessibilityText: '图标',
                accessibilityDescription: '新手提醒',
                action: () => {
                  this.getUIContext().getPromptAction().showToast({
                    message: '后缀图标被点击！'
                  });
                }
              },
              onClicked: () => {
                this.getUIContext().getPromptAction().showToast({
                  message: '操作块被点击！'
                });
              }
            })
          }
        }
      }
    }
  }
}
```

### 示例8（symbol类型无障碍朗读）

该示例代码实现Chip组件symbol类型后缀图标的无障碍朗读功能。

```ts
import { Chip, SymbolGlyphModifier } from '@kit.ArkUI';

@Builder
function DefaultFunction(): void {
}

@Component
struct SectionGroup {
  @Prop
  @Require
  title: ResourceStr;
  @BuilderParam
  @Require
  content: () => void = DefaultFunction;

  build() {
    Column({ space: 4 }) {
      Text(this.title)
        .fontColor('#FF666666')
        .fontSize(12)
      Column({ space: 8 }) {
        this.content()
      }
    }
    .alignItems(HorizontalAlign.Start)
    .width('100%')
  }
}

@Component
struct SectionItem {
  @Prop
  @Require
  title: ResourceStr;
  @BuilderParam
  @Require
  content: () => void = DefaultFunction;

  build() {
    Column({ space: 12 }) {
      Text(this.title)
      this.content()
    }
    .backgroundColor('#FFFFFFFF')
    .borderRadius(12)
    .padding(12)
    .width('100%')
  }
}

@Entry
@Component
struct ChipExample2 {
  @State activated: boolean = false;

  build() {
    NavDestination() {
      Scroll() {
        SectionGroup({ title: '后缀Symbol播报' }) {
          SectionItem({ title: 'activatedAccessibility' }) {
            Chip({
              label: { text: '操作块' },
              activated: true,
              suffixSymbol: {
                activated: new SymbolGlyphModifier($r('sys.symbol.media_sound'))
                  .fontSize(72),
              },
              suffixSymbolOptions: {
                activatedAccessibility: {
                  accessibilityText: '音乐',
                  accessibilityDescription: '新手提醒'
                },
                action: () => {
                  this.getUIContext().getPromptAction().showToast({
                    message: '后缀Symbol被点击！'
                  });
                }
              },
              onClicked: () => {
                this.getUIContext().getPromptAction().showToast({
                  message: '操作块被点击！'
                });
              }
            })
          }

          SectionItem({ title: 'normalAccessibility' }) {
            Chip({
              label: { text: '操作块' },
              suffixSymbol: {
                normal: new SymbolGlyphModifier($r('sys.symbol.media_sound'))
                  .fontSize(72),
              },
              suffixSymbolOptions: {
                normalAccessibility: {
                  accessibilityText: '音乐',
                  accessibilityDescription: '新手提醒'
                },
                action: () => {
                  this.getUIContext().getPromptAction().showToast({
                    message: '后缀Symbol被点击！'
                  });
                }
              },
              onClicked: () => {
                this.getUIContext().getPromptAction().showToast({
                  message: '操作块被点击！'
                });
              }
            })
          }
        }
      }
    }
    .padding({
      top: 8,
      bottom: 8,
      left: 16,
      right: 16,
    })
  }
}
```

### 示例9（Chip组件无障碍朗读）

示例展示Chip组件的无障碍属性设置，包括不同的accessibilitySelectedType类型和各种无障碍属性。

```ts
import { AccessibilitySelectedType, Chip, ChipSize } from '@kit.ArkUI';

@Entry
@Component
struct ChipAccessibilityExample {
  @State clickedChipActivated: boolean = false;
  @State checkedChipActivated: boolean = false;
  @State selectedChipActivated: boolean = false;

  build() {
    Column({ space: 20 }) {
      Text("Chip组件无障碍属性示例").fontSize(20).fontWeight(FontWeight.Bold)

      // 点击型Chip - CLICKED类型
      Chip({
        label: { text: "点击型Chip" },
        prefixIcon: {
          src: $r('sys.media.ohos_app_icon'),
          fillColor: Color.Blue
        },
        size: ChipSize.NORMAL,
        accessibilitySelectedType: AccessibilitySelectedType.CLICKED, // 点击型
        accessibilityDescription: "这是一个点击型Chip", // 整体无障碍描述
        accessibilityLevel: "yes", // 确保可被无障碍服务识别
        closeOptions: {
          accessibilityDescription: "删除此Chip，此操作无法撤销" // 为删除按钮提供详细说明
        },
        activated: this.clickedChipActivated,
        onClicked: () => {
          this.clickedChipActivated = !this.clickedChipActivated;
          this.getUIContext().getPromptAction().showToast({ message: "点击型Chip被点击" });
        },
        onClose: () => {
          this.getUIContext().getPromptAction().showToast({ message: "点击型Chip的关闭按钮被点击" });
        }
      })

      // 复选型Chip - CHECKED类型
      Chip({
        label: { text: "复选型Chip" },
        prefixIcon: {
          src: $r('sys.media.ohos_app_icon'),
          fillColor: Color.Green
        },
        size: ChipSize.NORMAL,
        accessibilitySelectedType: AccessibilitySelectedType.CHECKED, // 复选型
        accessibilityDescription: "这是一个复选型Chip", // 整体无障碍描述
        activated: this.checkedChipActivated,
        onClicked: () => {
          this.checkedChipActivated = !this.checkedChipActivated;
          this.getUIContext().getPromptAction().showToast({
            message: this.checkedChipActivated ? "复选型Chip被选中" : "复选型Chip被取消选中"
          });
        }
      })

      // 单选型Chip - SELECTED类型
      Chip({
        label: { text: "单选型Chip" },
        prefixIcon: {
          src: $r('sys.media.ohos_app_icon'),
          fillColor: Color.Red
        },
        size: ChipSize.NORMAL,
        accessibilitySelectedType: AccessibilitySelectedType.SELECTED, // 单选型
        accessibilityDescription: "这是一个单选型Chip", // 整体无障碍描述
        activated: this.selectedChipActivated,
        onClicked: () => {
          this.selectedChipActivated = !this.selectedChipActivated;
          this.getUIContext().getPromptAction().showToast({
            message: this.selectedChipActivated ? "单选型Chip被选中" : "单选型Chip被取消选中"
          });
        }
      })

      // 无障碍级别设置示例
      Chip({
        label: { text: "无障碍级别为no" },
        size: ChipSize.NORMAL,
        accessibilityLevel: "no", // 此Chip不能被无障碍服务识别
        closeOptions: {
          accessibilityLevel: "no"
        },
        backgroundColor: '#CCCCCC',
        onClicked: () => {
          this.getUIContext().getPromptAction().showToast({ message: "此Chip无法被无障碍服务识别" });
        }
      })
    }
    .width('100%')
    .padding(16)
  }
}
```