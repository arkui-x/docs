# 像素单位

ArkUI为开发者提供4种像素单位，框架采用vp为基准数据单位。

>**说明：**
>
>本模块首批接口从API version 7开始支持，后续版本的新增接口，采用上角标单独标记接口的起始版本。


| 名称   | 描述                                       |
| ---- | ---------------------------------------- |
| px   | 屏幕物理像素单位。                                |
| vp   | 屏幕密度相关像素，根据屏幕像素密度转换为屏幕物理像素，当数值不带单位时，默认单位vp。在实际宽度为1440物理像素的屏幕上，1vp约等于3px。 |
| fp   | 字体像素，与vp类似适用屏幕密度变化，随系统字体大小设置变化。          |
| lpx  | 视窗逻辑像素单位，lpx单位为实际屏幕宽度与逻辑宽度（通过designWidth配置）的比值，designWidth默认值为720。当designWidth为720时，在实际宽度为1440物理像素的屏幕上，1lpx为2px大小。 |


## 像素单位转换

提供其他单位与px单位互相转换的方法。

| 接口                                       | 描述                                       |
| ---------------------------------------- | ---------------------------------------- |
| vp2px(value&nbsp;:&nbsp;number)&nbsp;:&nbsp;number | 将vp单位的数值转换为以px为单位的数值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| px2vp(value&nbsp;:&nbsp;number)&nbsp;:&nbsp;number | 将px单位的数值转换为以vp为单位的数值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| fp2px(value&nbsp;:&nbsp;number)&nbsp;:&nbsp;number | 将fp单位的数值转换为以px为单位的数值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| px2fp(value&nbsp;:&nbsp;number)&nbsp;:&nbsp;number | 将px单位的数值转换为以fp为单位的数值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| lpx2px(value&nbsp;:&nbsp;number)&nbsp;:&nbsp;number | 将lpx单位的数值转换为以px为单位的数值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
| px2lpx(value&nbsp;:&nbsp;number)&nbsp;:&nbsp;number | 将px单位的数值转换为以lpx为单位的数值。<br/>从API version 9开始，该接口支持在ArkTS卡片中使用。 |
