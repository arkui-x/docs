# ProgressButton

文本下载按钮，可显示具体下载进度。


> **说明：**
>
> 该组件从API version 18开始支持跨平台。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>
> 该组件不支持在Wearable设备上使用。


## 导入模块

```
import { ProgressButton } from '@kit.ArkUI';
```

## 属性
不支持[通用属性](../../../application-dev/reference/arkui-ts/README.md)。

## ProgressButton<sup>22+<sup>

ProgressButton({progress: number, content: ResourceStr, progressButtonWidth?: Length, clickCallback: () => void, enable:
boolean, colorOptions?: ProgressButtonColorOptions, progressButtonRadius?: LengthMetrics})

**装饰器类型：**\@Component

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                             | 类型                                                      | 必填 | 装饰器类型 | 说明                                                         |
| -------------------------------- | --------------------------------------------------------- | ---- | ---------- | ------------------------------------------------------------ |
| progress<sup>22+<sup>            | number                                                    | 是   | \@Prop     | 下载按钮的当前进度值。<br/>取值范围：[0,100]。设置小于0的数值时置为0，设置大于100的数值置为100。<br/>默认值：0<br/> |
| content<sup>20+<sup>             | [ResourceStr](../arkui-ts/ts-types.md#resourcestr)        | 是   | \@Prop     | 下载按钮的文本。<br/>默认值：空字符串。<br/>**说明**：最长显示组件宽度，超出部分用省略号代替。从API version 20开始，支持Resource类型。<br/> |
| progressButtonWidth<sup>22+<sup> | [Length](../arkui-ts/ts-types.md#length)                  | 否   | -          | 下载按钮的宽度，单位vp。<br/>取值范围：大于等于44vp。<br/>默认值：44vp。小于默认值和非法值设置为默认值。当取值为非Resource类型且小于默认值或取值为非法值时，识别值为默认值。当取值为Resource类型且小于默认值时识别为默认值，为非法值时下载按钮的宽度显示为容器宽度的100%。<br/> |
| clickCallback<sup>22+<sup>       | () => void                                                | 是   | -          | 下载按钮的点击回调。<br/>                                    |
| enable<sup>22+<sup>              | boolean                                                   | 是   | \@Prop     | 下载按钮是否可以点击。<br> enable为true时，表示可以点击。<br> enable为false时，表示不可点击。<br/> |
| colorOptions<sup>22+<sup>        | [ProgressButtonColorOptions](#progressbuttoncoloroptions22+) | 否   | \@Prop     | 下载按钮颜色。<br/>                                          |
| progressButtonRadius             | LengthMetrics                                             | 否   | \@Prop     | 下载按钮的圆角（不支持百分比设置）。<br/>取值范围：[0, height/2]<br/>默认值：height/2<br/>设置值小于0时按照0处理，设置其他非法数值时，按照默认值处理。当直接入参为undefined时，按照默认值处理，入参为LengthMetrics.vp时，建议传入具体数值，传入null/undefined会导致显示异常。<br/> |

## ProgressButtonColorOptions<sup>22+<sup>

下载按钮颜色选项

**支持平台：** Android、iOS

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称            | 类型                                                   | 必填 | 说明                                                         |
| --------------- | ------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| progressColor<sup>22+<sup>   | [ResourceColor](../arkui-ts/ts-types.md#resourcecolor) | 否   | 进度条颜色。<br/>默认值：#330A59F7                           |
| borderColor<sup>22+<sup>     | [ResourceColor](../arkui-ts/ts-types.md#resourcecolor) | 否   | 按钮描边颜色。<br/>默认值：#330A59F7                         |
| textColor<sup>22+<sup>       | [ResourceColor](../arkui-ts/ts-types.md#resourcecolor) | 否   | 按钮文本颜色。<br/>默认值：系统默认值（#CE000000）           |
| backgroundColor<sup>22+<sup> | [ResourceColor](../arkui-ts/ts-types.md#resourcecolor) | 否   | 按钮背景色。<br/>默认值：\$r('sys.color.ohos_id_color_foreground_contrary') |

## 事件
不支持[通用事件](../../../application-dev/reference/arkui-ts/README.md)。

## 示例

### 示例1（进度条下载按钮）
该示例实现了一个简单的带加载进度的文本下载按钮。
```ts
import { ProgressButton } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State progressIndex: number = 0;
  @State textState: string = '下载';
  @State buttonWidth: number = 200;
  @State isRunning: boolean = false;
  @State enableState: boolean = true;

  build() {
    Column() {
      Scroll() {
        Column({ space: 20 }) {
          ProgressButton({
            progress: this.progressIndex,
            progressButtonWidth: this.buttonWidth,
            content: this.textState,
            enable: this.enableState,
            clickCallback: () => {
              if (this.textState && !this.isRunning && this.progressIndex < 100) {
                this.textState = '继续';
              }
              this.isRunning = !this.isRunning;
              let timer = setInterval(() => {
                if (this.isRunning) {
                  if (this.progressIndex === 100) {
                  } else {
                    this.progressIndex++;
                    if (this.progressIndex === 100) {
                      this.textState = '已完成';
                      this.enableState = false;
                    }
                  }
                } else {
                  clearInterval(timer);
                }
              }, 20)
            }
          })
        }.alignItems(HorizontalAlign.Center).width('100%').margin({ top: 20 })
      }
    }
  }
}
```


![img.png](./figures/advanced-progressButton-1.png)
### 示例2（自定义颜色按钮）
该示例实现了一个简单的自定义颜色的文本下载按钮。
```ts
import { ProgressButton } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State progressIndex: number = 0;
  @State textState: string = '下载';
  @State buttonWidth: number = 200;
  @State isRunning: boolean = false;
  @State enableState: boolean = true;

  build() {
    Column() {
      Scroll() {
        Column({ space: 20 }) {
          ProgressButton({
            //设置下载按钮颜色
            colorOptions: {
              progressColor: Color.Orange,
              borderColor: Color.Black,
              textColor: Color.Blue,
              backgroundColor: Color.Pink
            },
            progress: this.progressIndex,
            progressButtonWidth: this.buttonWidth,
            content: this.textState,
            enable: this.enableState,
            clickCallback: () => {
              if (this.textState && !this.isRunning && this.progressIndex < 100) {
                this.textState = '继续';
              }
              this.isRunning = !this.isRunning;
              let timer = setInterval(() => {
                if (this.isRunning) {
                  if (this.progressIndex === 100) {
                  } else {
                    this.progressIndex++;
                    if (this.progressIndex === 100) {
                      this.textState = '已完成';
                      this.enableState = false;
                    }
                  }
                } else {
                  clearInterval(timer);
                }
              }, 20)
            }
          })
        }.alignItems(HorizontalAlign.Center).width('100%').margin({ top: 20 })
      }
    }
  }
}
```
![img.png](./figures/advanced-progressButton-2.png)

### 示例3（自定义圆角按钮）
该示例实现了一个简单的自定义圆角的文本下载按钮。
```ts
import { ProgressButton, LengthMetrics } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State progressIndex: number = 0;
  @State textState: string = '下载';
  @State buttonWidth: number = 200;
  @State isRunning: boolean = false;
  @State enableState: boolean = true;

  build() {
    Column() {
      Scroll() {
        Column({ space: 20 }) {
          ProgressButton({
            progressButtonRadius: LengthMetrics.vp(8), //自定义圆角值为8vp
            progress: this.progressIndex,
            progressButtonWidth: this.buttonWidth,
            content: this.textState,
            enable: this.enableState,
            clickCallback: () => {
              if (this.textState && !this.isRunning && this.progressIndex < 100) {
                this.textState = '继续';
              }
              this.isRunning = !this.isRunning;
              let timer = setInterval(() => {
                if (this.isRunning) {
                  if (this.progressIndex === 100) {
                  } else {
                    this.progressIndex++;
                    if (this.progressIndex === 100) {
                      this.textState = '已完成';
                      this.enableState = false;
                    }
                  }
                } else {
                  clearInterval(timer);
                }
              }, 20)
            }
          })
        }.alignItems(HorizontalAlign.Center).width('100%').margin({ top: 20 })
      }
    }
  }
}
```
![img.png](./figures/advanced-progressButton-3.png)