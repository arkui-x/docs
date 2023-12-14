# @ohos.convertxml (xml转换JavaScript)

本模块提供转换xml文本为JavaScript对象的功能。

> **说明：**
>
> 本模块首批接口从API version 8开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```js
import convertxml from '@ohos.convertxml';
```

## ConvertXML

### convertToJSObject<sup>9+</sup>

convertToJSObject(xml: string, options?: ConvertOptions) : Object

转换xml文本为JavaScript对象。

**系统能力：** SystemCapability.Utils.Lang

**参数：**

| 参数名  | 类型                              | 必填 | 说明            |
| ------- | --------------------------------- | ---- | --------------- |
| xml     | string                            | 是   | 传入的xml文本。 |
| options | [ConvertOptions](#convertoptions) | 否   | 转换选项 , 默认值是ConvertOptions对象 , 由其中各个属性的默认值组成。  |

**返回值：**

| 类型   | 说明                         |
| ------ | ---------------------------- |
| Object | 处理后返回的JavaScript对象。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息 |
| -------- | -------- |
| 10200002 | Invalid xml string. |

**示例：**

```js
try {
    let xml =
        '<?xml version="1.0" encoding="utf-8"?>' +
        '<note importance="high" logged="true">' +
        '    <title>Happy</title>' +
        '    <todo>Work</todo>' +
        '    <todo>Play</todo>' +
        '</note>';
    let conv = new convertxml.ConvertXML()
    let options = {
        trim: false, declarationKey: "_declaration",
        instructionKey: "_instruction", attributesKey: "_attributes",
        textKey: "_text", cdataKey: "_cdata", doctypeKey: "_doctype",
        commentKey: "_comment", parentKey: "_parent", typeKey: "_type",
        nameKey: "_name", elementsKey: "_elements"
    }
    let result = JSON.stringify(conv.convertToJSObject(xml, options));
    console.log(result);
} catch (e) {
    console.log(e.toString());
}
// 输出(宽泛型)
// {"_declaration":{"_attributes":{"version":"1.0","encoding":"utf-8"}},"_elements":[{"_type":"element","_name":"note","_attributes":{"importance":"high","logged":"true"},"_elements":[{"_type":"element","_name":"title","_elements":[{"_type":"text","_text":"Happy"}]},{"_type":"element","_name":"todo","_elements":[{"_type":"text","_text":"Work"}]},{"_type":"element","_name":"todo","_elements":[{"_type":"text","_text":"Play"}]}]}]}
```


## ConvertOptions

转换选项。

**系统能力：** SystemCapability.Utils.Lang

| 名称              | 类型 | 必填 | 说明                                                        |
| ----------------- | -------- | ---- | ----------------------------------------------------------- |
| trim              | boolean  | 是   | 是否修剪位于文本前后的空白字符，默认false。                 |
| ignoreDeclaration | boolean  | 否   | 是否忽略xml写入声明指示，默认false。                        |
| ignoreInstruction | boolean  | 否   | 是否忽略xml的写入处理指令，默认false。                      |
| ignoreAttributes  | boolean  | 否   | 是否跨多行打印属性并缩进属性，默认false。                   |
| ignoreComment     | boolean  | 否   | 是否忽略元素的注释信息，默认false。                         |
| ignoreCDATA       | boolean  | 否   | 是否忽略元素的CDATA信息，默认false。                        |
| ignoreDoctype     | boolean  | 否   | 是否忽略元素的Doctype信息，默认false。                      |
| ignoreText        | boolean  | 否   | 是否忽略元素的文本信息，默认false。                         |
| declarationKey    | string   | 是   | 用于输出对象中declaration的属性键的名称，默认_declaration。 |
| instructionKey    | string   | 是   | 用于输出对象中instruction的属性键的名称，默认_instruction。 |
| attributesKey     | string   | 是   | 用于输出对象中attributes的属性键的名称，默认_attributes。   |
| textKey           | string   | 是   | 用于输出对象中text的属性键的名称，默认_text。               |
| cdataKey          | string   | 是   | 用于输出对象中cdata的属性键的名称，默认_cdata。             |
| doctypeKey        | string   | 是   | 用于输出对象中doctype的属性键的名称，默认_doctype。         |
| commentKey        | string   | 是   | 用于输出对象中comment的属性键的名称，默认_comment。         |
| parentKey         | string   | 是   | 用于输出对象中parent的属性键的名称，默认_parent。           |
| typeKey           | string   | 是   | 用于输出对象中type的属性键的名称，默认_type。               |
| nameKey           | string   | 是   | 用于输出对象中name的属性键的名称，默认_name。               |
| elementsKey       | string   | 是   | 用于输出对象中elements的属性键的名称，默认_elements。       |