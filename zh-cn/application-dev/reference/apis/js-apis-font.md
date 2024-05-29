# @ohos.font (注册自定义字体)

本模块提供注册自定义字体。

> **说明**
>
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
> 本模块功能依赖UI的执行上下文，不可在UI上下文不明确的地方使用，参见[UIContext](./js-apis-arkui-UIContext.md#uicontext)说明。

## 导入模块

```ts
import font from '@ohos.font'
```

## font.registerFont

registerFont(options: FontOptions): void

在字体管理中注册自定义字体。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名     | 类型                          | 必填   | 说明          |
| ------- | --------------------------- | ---- | ----------- |
| options | [FontOptions](#fontoptions) | 是    | 注册的自定义字体信息。 |

## FontOptions

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称         | 类型     | 必填   | 说明           |
| ---------- | ------ | ---- | ------------ |
| familyName | string \| Resource | 是    | 设置注册的字体名称。   |
| familySrc  | string \| Resource | 是    | 设置注册字体文件的路径。 |

**示例：**

```ts
// xxx.ets
import font from '@ohos.font';

@Entry
@Component
struct FontExample {
  @State message: string = 'Hello World'

  // iconFont示例，假设0000为指定icon的Unicode，实际需要开发者从注册的iconFont的ttf文件里面获取Unicode
  @State unicode: string = '\u0000'
  @State codePoint: string = String.fromCharCode(0x0000)

  aboutToAppear() {
    // familyName和familySrc都支持系统Resource
    font.registerFont({
      familyName: $r('app.string.font_name'),
      familySrc: $r('app.string.font_src')
    })

    // familySrc支持RawFile
    font.registerFont({
      familyName: 'mediumRawFile',
      familySrc: $rawfile('font/medium.ttf')
    })

    // 注册iconFont
    font.registerFont({
      familyName: 'iconFont',
      familySrc: '/font/iconFont.ttf'
    })

    // familyName和familySrc都支持string
    font.registerFont({
      familyName: 'medium',
      familySrc: '/font/medium.ttf' // font文件夹与pages目录同级
    })
  }

  build() {
    Column() {
      Text(this.message)
        .align(Alignment.Center)
        .fontSize(20)
        .fontFamily('medium') // medium：注册自定义字体的名字（$r('app.string.mediumFamilyName')、'mediumRawFile'等已注册字体也能正常使用）

      // 使用iconFont的两种方式
      Text(this.unicode)
        .align(Alignment.Center)
        .fontSize(20)
        .fontFamily('iconFont')
      Text(this.codePoint)
        .align(Alignment.Center)
        .fontSize(20)
        .fontFamily('iconFont')
    }.width('100%')
  }
}
```

## font.getSystemFontList

getSystemFontList(): Array\<string>

获取系统字体列表。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**返回值：**

| 类型                 | 说明               |
| -------------------- | ----------------- |
| Array\<string>       | 系统的字体名列表。  |

**示例：**

```ts
// xxx.ets
import font from '@ohos.font';

@Entry
@Component
struct FontExample {
  fontList: Array<string> = new Array<string>();
  build() {
    Column() {
      Button("getSystemFontList")
        .width('60%')
        .height('6%')
        .onClick(()=>{
          this.fontList = font.getSystemFontList()
        })
    }.width('100%')
  }
}
```

## font.getFontByName

getFontByName(fontName: string): FontInfo

根据传入的系统字体名称获取系统字体的相关信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：**

| 参数名      | 类型      | 必填    | 说明          |
| ---------- | --------- | ------- | ------------ |
| fontName   | string    | 是      | 系统的字体名。 |

**返回值：**

| 类型                   | 说明                    |
| --------------------- | ----------------------- |
| [FontInfo](#fontinfo) | 字体的详细信息           |

## FontInfo

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称            | 类型    | 必填  | 说明                       |
| -------------- | ------- | ------------------------- | ------------------------- |
| path           | string  | 是 | 系统字体的文件路径。        |
| postScriptName | string  | 是 | 系统字体的postScript名称。 |
| fullName       | string  | 是 | 系统字体的名称。           |
| family         | string  | 是 | 系统字体的字体家族。       |
| subfamily      | string  | 是 | 系统字体的子字体家族。      |
| weight         | number  | 是 | 系统字体的粗细程度，单位px。        |
| width          | number  | 是 | 系统字体的宽窄风格属性，单位px。    |
| italic         | boolean | 是 | 系统字体是否倾斜。          |
| monoSpace      | boolean | 是 | 系统字体是否紧凑。         |
| symbolic       | boolean | 是 | 系统字体是否支持符号字体。  |

**示例：**

```ts
// xxx.ets
import font from '@ohos.font';

@Entry
@Component
struct FontExample {
  fontList: Array<string> = new Array<string>();
  fontInfo: font.FontInfo = font.getFontByName('');
  build() {
    Column() {
      Button("getFontByName")
        .onClick(() => {
          this.fontInfo = font.getFontByName('HarmonyOS Sans Italic')
          console.log("getFontByName(): path = " + this.fontInfo.path)
          console.log("getFontByName(): postScriptName = " + this.fontInfo.postScriptName)
          console.log("getFontByName(): fullName = " + this.fontInfo.fullName)
          console.log("getFontByName(): Family = " + this.fontInfo.family)
          console.log("getFontByName(): Subfamily = " + this.fontInfo.subfamily)
          console.log("getFontByName(): weight = " + this.fontInfo.weight)
          console.log("getFontByName(): width = " + this.fontInfo.width)
          console.log("getFontByName(): italic = " + this.fontInfo.italic)
          console.log("getFontByName(): monoSpace = " + this.fontInfo.monoSpace)
          console.log("getFontByName(): symbolic = " + this.fontInfo.symbolic)
        })
    }.width('100%')
  }
}
```
