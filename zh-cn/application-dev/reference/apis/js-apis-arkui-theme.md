# @ohos.arkui.theme(主题换肤)

支持自定义主题风格，实现App组件风格跟随Theme切换。

> **说明：**
>
> 本模块首批接口从API version 12开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { Theme, ThemeControl, CustomColors, Colors, CustomTheme } from '@kit.ArkUI';
```

## Theme

当前生效的主题风格对象。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

| 名称 | 类型                | 可读 | 可写 | 说明       | Android平台 | iOS平台  |
| ------ |-------------------|-----|-----|----------|----------|----------|
| colors | [Colors](#colors) | 否   | 否   |  主题颜色资源。 | 支持 | 支持 |

## Colors

主题颜色资源。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

| 名称                           | 类型                                                 | 可读 | 可写 | 说明               | Android平台      | iOS平台          |
|-------------------------------|-----------------------------------------------------|-----|-----|------------------|------------------|------------------|
| brand                         | ResourceColor | 否   | 否   | 品牌色。             | 支持           | 支持           |
| warning                       | ResourceColor | 否   | 否   | 一级警示色。           | 支持         | 支持         |
| alert                         | ResourceColor | 否   | 否   | 二级提示色。           | 支持         | 支持         |
| confirm                       | ResourceColor | 否   | 否   | 确认色。             | 支持           | 支持           |
| fontPrimary                   | ResourceColor | 否   | 否   | 一级文本字体颜色。        | 支持      | 支持      |
| fontSecondary                 | ResourceColor | 否   | 否   | 二级文本字体颜色。        | 支持      | 支持      |
| fontTertiary                  | ResourceColor | 否   | 否   | 三级文本字体颜色。        | 支持      | 支持      |
| fontFourth                    | ResourceColor | 否   | 否   | 四级文本字体颜色。        | 支持      | 支持      |
| fontEmphasize                 | ResourceColor | 否   | 否   | 高亮字体颜色。          | 支持        | 支持        |
| fontOnPrimary                 | ResourceColor | 否   | 否   | 一级文本反转颜色，用于彩色背景。 | 支持 | 支持 |
| fontOnSecondary               | ResourceColor | 否   | 否   | 二级文本反转颜色，用于彩色背景。 | 支持 | 支持 |
| fontOnTertiary                | ResourceColor | 否   | 否   | 三级文本反转颜色，用于彩色背景。 | 支持 | 支持 |
| fontOnFourth                  | ResourceColor | 否   | 否   | 四级文本反转颜色，用于彩色背景。 | 支持 | 支持 |
| iconPrimary                   | ResourceColor                                       | 否   | 否   | 一级图标颜色。          | 支持        | 支持        |
| iconSecondary                 | ResourceColor | 否   | 否   | 二级图标颜色。          | 支持        | 支持        |
| iconTertiary                  | ResourceColor | 否   | 否   | 三级图标颜色。          | 支持        | 支持        |
| iconFourth                    | ResourceColor                                       | 否   | 否   | 四级图标颜色。          | 支持        | 支持        |
| iconEmphasize                 | ResourceColor | 否   | 否   | 高亮图标颜色。          | 支持        | 支持        |
| iconSubEmphasize              | ResourceColor | 否   | 否   | 高亮辅助图标颜色。        | 支持      | 支持      |
| iconOnPrimary                 | ResourceColor | 否   | 否   | 一级图标反转颜色，用于彩色背景。 | 支持 | 支持 |
| iconOnSecondary               | ResourceColor | 否   | 否   | 二级图标反转颜色，用于彩色背景。 | 支持 | 支持 |
| iconOnTertiary                | ResourceColor | 否   | 否   | 三级图标反转颜色，用于彩色背景。 | 支持 | 支持 |
| iconOnFourth                  | ResourceColor | 否   | 否   | 四级图标反转颜色，用于彩色背景。 | 支持 | 支持 |
| backgroundPrimary             | ResourceColor | 否   | 否   | 一级背景颜色（实色，不透明）。  | 支持 | 支持 |
| backgroundSecondary           | ResourceColor | 否   | 否   | 二级背景颜色（实色，不透明）。  | 支持 | 支持 |
| backgroundTertiary            | ResourceColor | 否   | 否   | 三级背景颜色（实色，不透明）。  | 支持 | 支持 |
| backgroundFourth              | ResourceColor | 否   | 否   | 四级背景颜色（实色，不透明）。  | 支持 | 支持 |
| backgroundEmphasize           | ResourceColor | 否   | 否   | 高亮背景颜色（实色，不透明）。  | 支持 | 支持 |
| compForegroundPrimary         | ResourceColor | 否   | 否   | 前背景。             | 支持           | 支持           |
| compBackgroundPrimary         | ResourceColor | 否   | 否   | 白色背景。            | 支持          | 支持          |
| compBackgroundPrimaryTran     | ResourceColor | 否   | 否   | 白色透明背景。          | 支持        | 支持        |
| compBackgroundPrimaryContrary | ResourceColor | 否   | 否   | 常亮背景。            | 支持          | 支持          |
| compBackgroundGray            | ResourceColor | 否   | 否   | 灰色背景。            | 支持          | 支持          |
| compBackgroundSecondary       | ResourceColor | 否   | 否   | 二级背景。            | 支持          | 支持          |
| compBackgroundTertiary        | ResourceColor | 否   | 否   | 三级背景。            | 支持          | 支持          |
| compBackgroundEmphasize       | ResourceColor | 否   | 否   | 高亮背景。            | 支持          | 支持          |
| compBackgroundNeutral         | ResourceColor | 否   | 否   | 黑色中性高亮背景颜色。      | 支持    | 支持    |
| compEmphasizeSecondary        | ResourceColor | 否   | 否   | 20%高亮背景颜色。       | 支持     | 支持     |
| compEmphasizeTertiary         | ResourceColor | 否   | 否   | 10%高亮背景颜色。       | 支持     | 支持     |
| compDivider                   | ResourceColor | 否   | 否   | 通用分割线颜色。         | 支持       | 支持       |
| compCommonContrary            | ResourceColor | 否   | 否   | 通用反转颜色。          | 支持        | 支持        |
| compBackgroundFocus           | ResourceColor | 否   | 否   | 获焦态背景颜色。         | 支持       | 支持       |
| compFocusedPrimary            | ResourceColor | 否   | 否   | 获焦态一级反转颜色。       | 支持     | 支持     |
| compFocusedSecondary          | ResourceColor | 否   | 否   | 获焦态二级反转颜色。       | 支持     | 支持     |
| compFocusedTertiary           | ResourceColor | 否   | 否   | 获焦态三级反转颜色。       | 支持     | 支持     |
| interactiveHover              | ResourceColor | 否   | 否   | 通用悬停交互式颜色。       | 支持     | 支持     |
| interactivePressed            | ResourceColor | 否   | 否   | 通用按压交互式颜色。       | 支持     | 支持     |
| interactiveFocus              | ResourceColor | 否   | 否   | 通用获焦交互式颜色。       | 支持     | 支持     |
| interactiveActive             | ResourceColor | 否   | 否   | 通用激活交互式颜色。       | 支持     | 支持     |
| interactiveSelect             | ResourceColor | 否   | 否   | 通用选择交互式颜色。       | 支持     | 支持     |
| interactiveClick              | ResourceColor | 否   | 否   | 通用点击交互式颜色。       | 支持     | 支持     |

## CustomTheme

自定义主题风格对象。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

| 名称                           | 类型                                                 | 只读  | 可选  | 说明         | Android平台 | iOS平台    |
|-------------------------------|-----------------------------------------------------|-----|-----|------------|------------|------------|
| colors | [CustomColors](#customcolors) | 否   | 是   | 自定义主题颜色资源。 | 支持 | 支持 |

## CustomColors

type CustomColors = Partial\<Colors>

自定义主题颜色资源类型。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

| 类型  | 说明           | Android平台 | iOS平台      |
|-----|--------------|--------------|--------------|
| Partial<[Colors](#colors)>   | 自定义主题颜色资源类型。 | 支持 | 支持 |

## ThemeControl

ThemeControl将自定义Theme应用于App组件内，实现App组件风格跟随Theme切换。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

### setDefaultTheme

setDefaultTheme(theme: [CustomTheme](#customtheme)): void

将用户自定义Theme设置应用级默认主题，实现应用风格跟随Theme切换。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

| 参数名       | 类型                           | 必填 | 说明             | Android平台    | iOS平台        |
|--------------|------------------------------|------|----------------|----------------|----------------|
| theme | [CustomTheme](#customtheme)  | 是    | 表示设置的自定义主题风格。 | 支持 | 支持 |

**示例**

```ts
import { CustomTheme, CustomColors, ThemeControl } from '@kit.ArkUI';
// 自定义主题颜色
class BlueColors implements CustomColors {
  fontPrimary = Color.White;
  backgroundPrimary = Color.Blue;
  brand = Color.Blue; //品牌色
}

class PageCustomTheme implements CustomTheme {
  colors?: CustomColors;

  constructor(colors: CustomColors) {
    this.colors = colors;
  }
}
// 创建实例
const BlueColorsTheme = new PageCustomTheme(new BlueColors());
// 在页面build之前执行ThemeControl.setDefaultTheme，设置App默认样式风格为BlueColorsTheme。
ThemeControl.setDefaultTheme(BlueColorsTheme);
```
