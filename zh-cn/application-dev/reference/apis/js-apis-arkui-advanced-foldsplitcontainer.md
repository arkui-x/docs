# FoldSplitContainer


FoldSplitContainer分栏布局，实现折叠屏二分栏、三分栏在展开态、悬停态以及折叠态的区域控制。


> **说明：**
>
> 该组件从API version 22开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>
> 该组件不支持在Wearable设备上使用。
>
> 该高级组件依赖窗口未适配跨平台的枚举值和接口（display.FoldStatus.FOLD_STATUS_FOLDED 和 display.getCurrentFoldCreaseRegion），因此在Android平台适配时有如下影响：
>
>  折叠时：折叠状态识别错误、布局错乱、无法将方向限制在竖屏自动旋转
>
>  悬停时：计算折痕区域错误、更改不了水平宽度比例。


## 导入模块

```ts
import { FoldSplitContainer } from '@kit.ArkUI';
```

## 子组件

无

## FoldSplitContainer

FoldSplitContainer({
  primary: Callback&lt;void&gt;,
  secondary: Callback&lt;void&gt;,
  extra?: Callback&lt;void&gt;,
  expandedLayoutOptions: ExpandedRegionLayoutOptions,
  hoverModeLayoutOptions: HoverModeRegionLayoutOptions,
  foldedLayoutOptions: FoldedRegionLayoutOptions,
  animationOptions?: AnimateParam,
  onHoverStatusChange?: OnHoverStatusChangeHandler
})

实现折叠屏二分栏、三分栏在展开态、悬停态以及折叠态的区域控制的分栏布局。

**装饰器类型：**\@Component

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                         | 类型                                                         | 必填 | 装饰器类型    | 说明                                           | **Android平台** | **iOS平台** |
| ---------------------------- | ------------------------------------------------------------ | ---- | ------------- | ---------------------------------------------- | --------------- | ----------- |
| primary                      | Callback\<void>                                              | 是   | @BuilderParam | 主要区域回调函数。                             | 支持            | 不支持      |
| secondary                    | Callback\<void>                                              | 是   | @BuilderParam | 次要区域回调函数。                             | 支持            | 不支持      |
| extra                        | Callback\<void>                                              | 否   | @BuilderParam | 扩展区域回调函数，不传入的情况，没有对应区域。 | 支持            | 不支持      |
| expandedLayoutOptions        | [ExpandedRegionLayoutOptions](#expandedregionlayoutoptions)  | 是   | @Prop         | 展开态布局信息。                               | 支持            | 不支持      |
| hoverModeLayoutOptions不支持 | [HoverModeRegionLayoutOptions](#hovermoderegionlayoutoptions) | 是   | @Prop         | 悬停态布局信息。                               | 支持            | 不支持      |
| foldedLayoutOptions          | [FoldedRegionLayoutOptions](#foldedregionlayoutoptions)      | 是   | @Prop         | 折叠态布局信息。                               | 支持            | 不支持      |
| animationOptions             | [AnimateParam](ts-explicit-animation.md#animateparam对象说明) \| null | 否   | @Prop         | 设置动画效果相关的参数，null表示关闭动效。     | 支持            | 不支持      |
| onHoverStatusChange          | [OnHoverStatusChangeHandler](#onhoverstatuschangehandler)    | 否   | -             | 折叠屏进入或退出悬停模式时触发的回调函数。     | 支持            | 不支持      |

## ExpandedRegionLayoutOptions

展开态布局信息。

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                       | 类型                                        | 必填 | 说明                                                         | Android平台 | iOS平台 |
| -------------------------- | ------------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| isExtraRegionPerpendicular | boolean                                     | 否   | 扩展区域是否从上到下贯穿整个组件，当且仅当extra有效时此字段才生效。设置为true时表示扩展区域从上到下贯穿整个组件，设置为false时表示扩展区域不从上到下贯穿整个组件。<br/>默认值：true | 支持        | 不支持  |
| verticalSplitRatio         | number                                      | 否   | 主要区域与次要区域之间的高度比例。<br/>默认值：PresetSplitRatio.LAYOUT_1V1 | 支持        | 不支持  |
| horizontalSplitRatio       | number                                      | 否   | 主要区域与扩展区域之间的宽度比例，当且仅当extra有效时此字段才生效。<br/>默认值：PresetSplitRatio.LAYOUT_3V2 | 支持        | 不支持  |
| extraRegionPosition        | [ExtraRegionPosition](#extraregionposition) | 否   | 扩展区域的位置信息，当且仅当isExtraRegionPerpendicular = false有效时此字段才生效。<br/>默认值：ExtraRegionPosition.top | 支持        | 不支持  |

## HoverModeRegionLayoutOptions

悬停态布局信息。

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                 | 类型                                        | 必填 | 说明                                                         | Android平台 | iOS平台 |
| -------------------- | ------------------------------------------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| showExtraRegion      | boolean                                     | 否   | 可折叠屏幕在半折叠状态下是否显示扩展区域。设置为true时表示显示扩展区域，设置为false时表示不显示扩展区域。<br/>默认值：false | 支持        | 不支持  |
| horizontalSplitRatio | number                                      | 否   | 主要区域与扩展区域之间的宽度比例，当且仅当extra有效时此字段才生效。<br/>默认值：PresetSplitRatio.LAYOUT_3V2 | 支持        | 不支持  |
| extraRegionPosition  | [ExtraRegionPosition](#extraregionposition) | 否   | 扩展区域的位置信息，当且仅当showExtraRegion时此字段才生效。<br/>默认值：ExtraRegionPosition.top | 支持        | 不支持  |

> **说明：**
>
> 1.设备处于悬停态时，存在避让区域，布局计算需要考虑避让区域对布局的影响。
> 2.在悬停模式下，屏幕上半部分用于显示，下半部分用于操作。

## FoldedRegionLayoutOptions

折叠态布局信息。

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称               | 类型   | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------------ | ------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| verticalSplitRatio | number | 否   | 主要区域与次要区域之间的高度比例。默认值：PresetSplitRatio.LAYOUT_1V1 | 支持        | 不支持  |

## OnHoverStatusChangeHandler

type OnHoverStatusChangeHandler = (status: HoverModeStatus) => void

onHoverStatusChange事件处理。

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**参数：** 

| 参数名 | 类型                                | 必填 | 说明                                       | Android平台 | iOS平台 |
| ------ | ----------------------------------- | ---- | ------------------------------------------ | ----------- | ------- |
| status | [HoverModeStatus](#hovermodestatus) | 是   | 折叠屏进入或退出悬停模式时触发的回调函数。 | 支持        | 不支持  |

## HoverModeStatus

设备或应用的折叠、旋转、窗口状态信息。

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称             | 类型                                                         | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ---------------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| foldStatus       | [display.FoldStatus](../apis/js-apis-display.md#foldstatus20) | 是   | 设备的折叠状态。                                             | 支持        | 不支持  |
| isHoverMode      | boolean                                                      | 是   | app当前是否处于悬停态。设置为true时表示当前为悬停态，设置为false时表示当前为非悬停态。 | 支持        | 不支持  |
| appRotation      | number                                                       | 是   | 应用旋转角度。                                               | 支持        | 不支持  |
| windowStatusType | [window.WindowStatusType](../apis/js-apis-window.md#windowstatustype20) | 是   | 窗口模式。                                                   | 支持        | 不支持  |

## ExtraRegionPosition

扩展区域位置信息。

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称   | 值   | 说明                     | Android平台 | iOS平台 |
| ------ | ---- | ------------------------ | ----------- | ------- |
| TOP    | 1    | 扩展区域在组件上半区域。 | 支持        | 不支持  |
| BOTTOM | 2    | 扩展区域在组件下半区域。 | 支持        | 不支持  |

## PresetSplitRatio

区域比例。

**支持平台：** Android

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称       | 值                 | 说明      | Android平台 | iOS平台 |
| ---------- | ------------------ | --------- | ----------- | ------- |
| LAYOUT_1V1 | 1                  | 1:1比例。 | 支持        | 不支持  |
| LAYOUT_3V2 | 1.5                | 3:2比例。 | 支持        | 不支持  |
| LAYOUT_2V3 | 0.6666666666666666 | 2:3比例。 | 支持        | 不支持  |

## 示例

### 示例1（设置二分栏）

该示例实现了折叠屏二分栏在展开态、悬停态以及折叠态的区域控制。

```ts
import { FoldSplitContainer } from '@kit.ArkUI';

@Entry
@Component
struct TwoColumns {
  @Builder
  privateRegion() {
    Text("Primary")
      .backgroundColor('rgba(255, 0, 0, 0.1)')
      .fontSize(28)
      .textAlign(TextAlign.Center)
      .height('100%')
      .width('100%')
  }

  @Builder
  secondaryRegion() {
    Text("Secondary")
      .backgroundColor('rgba(0, 255, 0, 0.1)')
      .fontSize(28)
      .textAlign(TextAlign.Center)
      .height('100%')
      .width('100%')
  }

  build() {
    RelativeContainer() {
      FoldSplitContainer({
        primary: () => {
          this.privateRegion()
        },
        secondary: () => {
          this.secondaryRegion()
        }
      })
    }
    .height('100%')
    .width('100%')
  }
}
```

| 折叠态                                | 展开态                                | 悬停态                                |
| ------------------------------------- | ------------------------------------- | ------------------------------------- |
| ![](figures/foldsplitcontainer-1.png) | ![](figures/foldsplitcontainer-2.png) | ![](figures/foldsplitcontainer-3.png) |

### 示例2（设置三分栏）

该示例实现了折叠屏三分栏在展开态、悬停态以及折叠态的区域控制。

```ts
import { FoldSplitContainer } from '@kit.ArkUI';

@Entry
@Component
struct ThreeColumns {
  @Builder
  privateRegion() {
    Text("Primary")
      .backgroundColor('rgba(255, 0, 0, 0.1)')
      .fontSize(28)
      .textAlign(TextAlign.Center)
      .height('100%')
      .width('100%')
  }

  @Builder
  secondaryRegion() {
    Text("Secondary")
      .backgroundColor('rgba(0, 255, 0, 0.1)')
      .fontSize(28)
      .textAlign(TextAlign.Center)
      .height('100%')
      .width('100%')
  }

  @Builder
  extraRegion() {
    Text("Extra")
      .backgroundColor('rgba(0, 0, 255, 0.1)')
      .fontSize(28)
      .textAlign(TextAlign.Center)
      .height('100%')
      .width('100%')
  }

  build() {
    RelativeContainer() {
      FoldSplitContainer({
        primary: () => {
          this.privateRegion()
        },
        secondary: () => {
          this.secondaryRegion()
        },
        extra: () => {
          this.extraRegion()
        }
      })
    }
    .height('100%')
    .width('100%')
  }
}
```

| 折叠态                                | 展开态                                | 悬停态                                |
| ------------------------------------- | ------------------------------------- | ------------------------------------- |
| ![](figures/foldsplitcontainer-4.png) | ![](figures/foldsplitcontainer-5.png) | ![](figures/foldsplitcontainer-6.png) |

### 示例3（展开态布局信息）

该示例通过配置ExpandedRegionLayoutOptions实现折叠屏展开态的布局信息。

```ts
import {
  FoldSplitContainer,
  PresetSplitRatio,
  ExtraRegionPosition,
  ExpandedRegionLayoutOptions,
  HoverModeRegionLayoutOptions,
  FoldedRegionLayoutOptions
} from '@kit.ArkUI';

@Component
struct Region {
  @Prop title: string;
  @BuilderParam content: () => void;
  @Prop compBackgroundColor: string;

  build() {
    Column({ space: 8 }) {
      Text(this.title)
        .fontSize("24fp")
        .fontWeight(600)

      Scroll() {
        this.content()
      }
      .layoutWeight(1)
      .width("100%")
    }
    .backgroundColor(this.compBackgroundColor)
    .width("100%")
    .height("100%")
    .padding(12)
  }
}

const noop = () => {
};

@Component
struct SwitchOption {
  @Prop label: string = ""
  @Prop value: boolean = false
  public onChange: (checked: boolean) => void = noop;

  build() {
    Row() {
      Text(this.label)
      Blank()
      Toggle({ type: ToggleType.Switch, isOn: this.value })
        .onChange((isOn) => {
          this.onChange(isOn);
        })
    }
    .backgroundColor(Color.White)
    .borderRadius(8)
    .padding(8)
    .width("100%")
  }
}

interface RadioOptions {
  label: string;
  value: Object | undefined | null;
  onChecked: () => void;
}

@Component
struct RadioOption {
  @Prop label: string;
  @Prop value: Object | undefined | null;
  @Prop options: Array<RadioOptions>;

  build() {
    Row() {
      Text(this.label)
      Blank()
      Column({ space: 4 }) {
        ForEach(this.options, (option: RadioOptions) => {
          Row() {
            Radio({
              group: this.label,
              value: JSON.stringify(option.value),
            })
              .onChange((checked) => {
                if (checked) {
                  option.onChecked();
                }
              })
            Text(option.label)
          }
        })
      }
      .alignItems(HorizontalAlign.Start)
    }
    .alignItems(VerticalAlign.Top)
    .backgroundColor(Color.White)
    .borderRadius(8)
    .padding(8)
    .width("100%")
  }
}

@Entry
@Component
struct Index {
  @State expandedRegionLayoutOptions: ExpandedRegionLayoutOptions = {
    horizontalSplitRatio: PresetSplitRatio.LAYOUT_3V2,
    verticalSplitRatio: PresetSplitRatio.LAYOUT_1V1,
    isExtraRegionPerpendicular: true,
    extraRegionPosition: ExtraRegionPosition.TOP
  };
  @State foldingRegionLayoutOptions: HoverModeRegionLayoutOptions = {
    horizontalSplitRatio: PresetSplitRatio.LAYOUT_3V2,
    showExtraRegion: false,
    extraRegionPosition: ExtraRegionPosition.TOP
  };
  @State foldedRegionLayoutOptions: FoldedRegionLayoutOptions = {
    verticalSplitRatio: PresetSplitRatio.LAYOUT_1V1
  };

  @Builder
  MajorRegion() {
    Region({
      title: "折叠态配置",
      compBackgroundColor: "rgba(255, 0, 0, 0.1)",
    }) {
      Column({ space: 4 }) {
        RadioOption({
          label: "折叠态垂直高度度比",
          value: this.foldedRegionLayoutOptions.verticalSplitRatio,
          options: [
            {
              label: "1:1",
              value: PresetSplitRatio.LAYOUT_1V1,
              onChecked: () => {
                this.foldedRegionLayoutOptions.verticalSplitRatio = PresetSplitRatio.LAYOUT_1V1
              }
            },
            {
              label: "2:3",
              value: PresetSplitRatio.LAYOUT_2V3,
              onChecked: () => {
                this.foldedRegionLayoutOptions.verticalSplitRatio = PresetSplitRatio.LAYOUT_2V3
              }
            },
            {
              label: "3:2",
              value: PresetSplitRatio.LAYOUT_3V2,
              onChecked: () => {
                this.foldedRegionLayoutOptions.verticalSplitRatio = PresetSplitRatio.LAYOUT_3V2
              }
            },
            {
              label: "未定义",
              value: undefined,
              onChecked: () => {
                this.foldedRegionLayoutOptions.verticalSplitRatio = undefined
              }
            }
          ]
        })
      }
      .constraintSize({ minHeight: "100%" })
    }
  }

  @Builder
  MinorRegion() {
    Region({
      title: "悬停态配置",
      compBackgroundColor: "rgba(0, 255, 0, 0.1)"
    }) {
      Column({ space: 4 }) {
        RadioOption({
          label: "悬停态水平宽度比",
          value: this.foldingRegionLayoutOptions.horizontalSplitRatio,
          options: [
            {
              label: "1:1",
              value: PresetSplitRatio.LAYOUT_1V1,
              onChecked: () => {
                this.foldingRegionLayoutOptions.horizontalSplitRatio = PresetSplitRatio.LAYOUT_1V1
              }
            },
            {
              label: "2:3",
              value: PresetSplitRatio.LAYOUT_2V3,
              onChecked: () => {
                this.foldingRegionLayoutOptions.horizontalSplitRatio = PresetSplitRatio.LAYOUT_2V3
              }
            },
            {
              label: "3:2",
              value: PresetSplitRatio.LAYOUT_3V2,
              onChecked: () => {
                this.foldingRegionLayoutOptions.horizontalSplitRatio = PresetSplitRatio.LAYOUT_3V2
              }
            },
            {
              label: "未定义",
              value: undefined,
              onChecked: () => {
                this.foldingRegionLayoutOptions.horizontalSplitRatio = undefined
              }
            },
          ]
        })

        SwitchOption({
          label: "悬停态是否显示扩展区",
          value: this.foldingRegionLayoutOptions.showExtraRegion,
          onChange: (checked) => {
            this.foldingRegionLayoutOptions.showExtraRegion = checked;
          }
        })

        if (this.foldingRegionLayoutOptions.showExtraRegion) {
          RadioOption({
            label: "悬停态扩展区位置",
            value: this.foldingRegionLayoutOptions.extraRegionPosition,
            options: [
              {
                label: "顶部",
                value: ExtraRegionPosition.TOP,
                onChecked: () => {
                  this.foldingRegionLayoutOptions.extraRegionPosition = ExtraRegionPosition.TOP
                }
              },
              {
                label: "底部",
                value: ExtraRegionPosition.BOTTOM,
                onChecked: () => {
                  this.foldingRegionLayoutOptions.extraRegionPosition = ExtraRegionPosition.BOTTOM
                }
              },
              {
                label: "未定义",
                value: undefined,
                onChecked: () => {
                  this.foldingRegionLayoutOptions.extraRegionPosition = undefined
                }
              },
            ]
          })
        }
      }
      .constraintSize({ minHeight: "100%" })
    }
  }

  @Builder
  ExtraRegion() {
    Region({
      title: "展开态配置",
      compBackgroundColor: "rgba(0, 0, 255, 0.1)"
    }) {
      Column({ space: 4 }) {
        RadioOption({
          label: "展开态水平宽度比",
          value: this.expandedRegionLayoutOptions.horizontalSplitRatio,
          options: [
            {
              label: "1:1",
              value: PresetSplitRatio.LAYOUT_1V1,
              onChecked: () => {
                this.expandedRegionLayoutOptions.horizontalSplitRatio = PresetSplitRatio.LAYOUT_1V1
              }
            },
            {
              label: "2:3",
              value: PresetSplitRatio.LAYOUT_2V3,
              onChecked: () => {
                this.expandedRegionLayoutOptions.horizontalSplitRatio = PresetSplitRatio.LAYOUT_2V3
              }
            },
            {
              label: "3:2",
              value: PresetSplitRatio.LAYOUT_3V2,
              onChecked: () => {
                this.expandedRegionLayoutOptions.horizontalSplitRatio = PresetSplitRatio.LAYOUT_3V2
              }
            },
            {
              label: "未定义",
              value: undefined,
              onChecked: () => {
                this.expandedRegionLayoutOptions.horizontalSplitRatio = undefined
              }
            },
          ]
        })

        RadioOption({
          label: "展开态垂直高度度比",
          value: this.expandedRegionLayoutOptions.verticalSplitRatio,
          options: [
            {
              label: "1:1",
              value: PresetSplitRatio.LAYOUT_1V1,
              onChecked: () => {
                this.expandedRegionLayoutOptions.verticalSplitRatio = PresetSplitRatio.LAYOUT_1V1
              }
            },
            {
              label: "2:3",
              value: PresetSplitRatio.LAYOUT_2V3,
              onChecked: () => {
                this.expandedRegionLayoutOptions.verticalSplitRatio = PresetSplitRatio.LAYOUT_2V3
              }
            },
            {
              label: "3:2",
              value: PresetSplitRatio.LAYOUT_3V2,
              onChecked: () => {
                this.expandedRegionLayoutOptions.verticalSplitRatio = PresetSplitRatio.LAYOUT_3V2
              }
            },
            {
              label: "未定义",
              value: undefined,
              onChecked: () => {
                this.expandedRegionLayoutOptions.verticalSplitRatio = undefined
              }
            }
          ]
        })

        SwitchOption({
          label: "展开态扩展区是否上下贯穿",
          value: this.expandedRegionLayoutOptions.isExtraRegionPerpendicular,
          onChange: (checked) => {
            this.expandedRegionLayoutOptions.isExtraRegionPerpendicular = checked;
          }
        })

        if (!this.expandedRegionLayoutOptions.isExtraRegionPerpendicular) {
          RadioOption({
            label: "展开态扩展区位置",
            value: this.expandedRegionLayoutOptions.extraRegionPosition,
            options: [
              {
                label: "顶部",
                value: ExtraRegionPosition.TOP,
                onChecked: () => {
                  this.expandedRegionLayoutOptions.extraRegionPosition = ExtraRegionPosition.TOP
                }
              },
              {
                label: "底部",
                value: ExtraRegionPosition.BOTTOM,
                onChecked: () => {
                  this.expandedRegionLayoutOptions.extraRegionPosition = ExtraRegionPosition.BOTTOM
                }
              },
              {
                label: "未定义",
                value: undefined,
                onChecked: () => {
                  this.expandedRegionLayoutOptions.extraRegionPosition = undefined
                }
              },
            ]
          })
        }
      }
      .constraintSize({ minHeight: "100%" })
    }
  }

  build() {
    Column() {
      FoldSplitContainer({
        primary: () => {
          this.MajorRegion()
        },
        secondary: () => {
          this.MinorRegion()
        },
        extra: () => {
          this.ExtraRegion()
        },
        expandedLayoutOptions: this.expandedRegionLayoutOptions,
        hoverModeLayoutOptions: this.foldingRegionLayoutOptions,
        foldedLayoutOptions: this.foldedRegionLayoutOptions,
      })
    }
    .width("100%")
    .height("100%")
  }
}
```

| 折叠态                                | 展开态                                 | 悬停态                                 |
| ------------------------------------- | -------------------------------------- | -------------------------------------- |
| ![](figures/foldsplitcontainer-7.png) | ![](figures/foldsplitcontainer-8.png)  | ![](figures/foldsplitcontainer-11.png) |
|                                       | ![](figures/foldsplitcontainer-9.png)  | ![](figures/foldsplitcontainer-12.png) |
|                                       | ![](figures/foldsplitcontainer-10.png) | ![](figures/foldsplitcontainer-13.png) |

