# 组件内隐式共享元素转场

geometryTransition用于组件内隐式共享元素转场，在组件显示切换过程中提供平滑过渡效果。通用transition机制提供了opacity、scale等转场动效，geometryTransition通过id绑定in/out组件(in指入场组件、out指出场组件)，使得组件原本独立的transition动画在空间位置上发生联系，从而将视觉焦点由出场组件位置引导到入场组件位置。

> **说明：**
>
> 从API Version 10开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。

## 属性

| 名称               | 参数类型 | 参数描述                                                     |
| ------------------ | -------- | ------------------------------------------------------------ |
| geometryTransition | string   | 设置geometryTransition的id，用于设置绑定关系，id置为空字符串""可清除绑定关系避免参与共享行为，id动态修改可重新建立绑定关系。同一个id只能有两个组件绑定且分别是in/out组件。 |

**说明：**

geometryTransition必须配合animateTo使用才有动画效果，动效时长、曲线跟随animateTo中的配置，不支持.animation隐式动画。


