# 拖拽控制

设置组件是否可以响应拖拽事件。

> **说明：**
> 
> 从API Version 10开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。

ArkUI框架对以下组件实现了默认的拖拽能力，支持对数据的拖出或拖入响应，开发者只需要将这些组件的draggable属性设置为true，即可使用默认拖拽能力。

- 默认支持拖出能力的组件（可从组件上拖出数据）：Search、TextInput、TextArea、RichEditor、Text、Image、FormComponent、Hyperlink

- 默认支持拖入能力的组件（目标组件可响应拖入数据）：Search、TextInput、TextArea、Video

开发者也可以通过实现通用拖拽事件来自定义拖拽响应。

其他组件需要开发者将draggable属性设置为true，并在onDragStart等接口中实现数据传输相关内容，才能正确处理拖拽。


## 属性

| 名称 | 参数类型 | 描述 |
| -------- | -------- | -------- |
| draggable | boolean | 设置该组件是否允许进行拖拽。<br/>默认值：false<br/> |
