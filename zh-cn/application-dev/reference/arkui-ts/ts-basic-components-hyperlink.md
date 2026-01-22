# Hyperlink
超链接组件，组件宽高范围内可通过点击实现跳转。

>  **说明：**
>
>  - 该组件从API Version 23 开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。

## 子组件

可以包含[Image](ts-basic-components-image.md)子组件。

## 接口

Hyperlink(address: string | Resource, content?: string | Resource)

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名  | 类型                                                 | 必填 | 说明                                                         |
| ------- | ---------------------------------------------------- | ---- | ------------------------------------------------------------ |
| address | string&nbsp;\|&nbsp;[Resource](ts-types.md#resource) | 是   | Hyperlink组件跳转的网页。                                    |
| content | string&nbsp;\|&nbsp;[Resource](ts-types.md#resource) | 否   | Hyperlink组件中超链接显示文本。<br/>**说明：** <br/>组件内有子组件时，不显示超链接文本。 |

## 属性

除支持[通用属性](README.md)外，还支持以下属性：

### color

color(value: Color | number | string | Resource)

设置超链接文本的颜色。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型                                                         | 必填 | 说明                                   |
| ------ | ------------------------------------------------------------ | ---- | -------------------------------------- |
| value  | [Color](ts-appendix-enums.md#color) \| number \| string \| [Resource](ts-types.md#resource) | 是   | 超链接文本的颜色。 默认值：'#ff007dff' |

## 示例

该示例展示了超链接图片和文本跳转的效果。

```tsx
@Entry
@Component
struct HyperlinkExample {
  build() {
    Column() {
      Column() {
        Hyperlink('https://www.baidu.com/') {
          Image($r('app.media.startIcon'))
            .width(200)
            .height(100)
        }
      }

      Column() {
        Hyperlink('https://www.baidu.com/', 'Go to the developer website') {
        }
        .color(Color.Blue)
      }
    }.width('100%').height('100%').justifyContent(FlexAlign.Center)
  }
}
```

![hyperlink](figures/hyperlink.PNG)



## 平台差异性

Hyperlink的跳转功能实际依赖系统平台（OH、Android、iOS）的隐式跳转能力。不同系统平台的隐式跳转能力的差异性导致Hyperlink组件的跳转行为存在三端（OH、Android、iOS）不一致现象。具体体现在如下方面：<br>

- 设置相同的address，Hyperlink成功跳转至其它应用后，发现打开的应用不一致。<br>

  Hyperlink跳转功能基于隐式跳转能力，重点在于设置行为意图，由系统解析并挑选合适的应用执行。不同系统的系统行为存在差异；同系统下，不同厂商、不同型号的真机设备的应用列表必然存在差异性。导致系统的选择也必然存在差异。<br>

- 实现统一的行为意图，需要根据不同系统平台、甚至不同设备型号构造不同的address。<br>

  根因在于系统差异性。而Android系统方面，不同厂家持有自有系统，比如小米的澎湃OS，VIVO的OriginOS等，各厂家针对自有系统的差异化处理也导致了即使同为Android系统平台，不同厂家设备行为也会存在差异性。<br>

  举例，开发者想要实现该行为：打开应用市场并搜索微信，在不同系统和设备构造的address。<br>

  ```tsx
  // Android平台 小米（XiaoMi RedMi K30）
  Hyperlink('https://app.xiaomi.com/details?id=com.tencent.mm', '应用市场') {}
  
  // Android平台 华为（HUAWEI LYA-AL00）
  Hyperlink('appmarket://details?id=com.tencent.mm', '应用市场') {}
  
  // iOS平台 IPhone 11
  Hyperlink('https://apps.apple.com/cn/app/%E5%BE%AE%E4%BF%A1/id414478124', '应用市场') {}
  ```

  **注1**：示例address仅供参考。相同的系统时，不同厂家设备的address可能也有差异。比如在小米的设备上运行示例代码中华为的address就可能会打开失败。<br>

  **注2**：oh设备实现目标行为需要采用[拉起应用市场并跳转至应用详情方案](https://developer.huawei.com/consumer/cn/doc/architecture-guides/tools-v1_2-ts_129-0000002367622362)，无法依靠Hyperlink组件实现。<br>

- 列举一些常见行为在Android系统平台、iOS系统平台的Hyperlink构造。

  - 跳转浏览器打开http类型的网址。

    ```tsx
    // Android平台
    Hyperlink('http://hcl.baidu.com/') {}
    
    // iOS平台
    Hyperlink('http://hcl.baidu.com/') {}
    ```

  - 跳转浏览器打开https类型的网址。

    ```tsx
    // Android平台
    Hyperlink('https://www.gov.cn/') {}
    
    // iOS平台
    Hyperlink('https://www.gov.cn/') {}
    ```

  - 跳转电话应用

    ```tsx
    // Android平台
    Hyperlink('tel:+8615012345678') {}
    
    // iOS平台
    Hyperlink('tel:+8615012345678') {}
    ```

  - 跳转短信应用

    ```tsx
    // Android平台
    Hyperlink('sms:+8612345678910') {}
    
    // iOS平台
    Hyperlink('sms:+8612345678910') {}
    ```
