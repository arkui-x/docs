# 日历选择器弹窗

点击日期弹出日历选择器弹窗，可选择弹窗内任意日期。

> **说明：**
>
> 该组件从API Version 10开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>
> 本模块功能依赖UI的执行上下文，不可在UI上下文不明确的地方使用，参见[UIContext](../apis/js-apis-arkui-UIContext.md#uicontext)说明。

## CalendarPickerDialog.show

show(options?: [CalendarDialogOptions](#calendardialogoptions))

定义日历选择器弹窗并弹出。

## CalendarDialogOptions

| 参数名     | 参数类型                                  | 必填 | 参数描述                                                     |
| ---------- | ----------------------------------------- | ---- | ------------------------------------------------------------ |
| selected   | Date                                      | 否   | 设置当前选中的日期。 默认值：当前系统日期                    |
| hintRadius | number \|[Resource](ts-types.md#resource) | 否   | 描述日期选中态底板样式。<br/>默认值：底板样式为圆形。<br />**说明：**<br />hintRadius为0，底板样式为直角矩形。hintRadius为0 ~ 16，底板样式为圆角矩形。hintRadius>=16，底板样式为圆形 |
| onAccept   | (value: Date) => void                     | 否   | 点击弹窗中的“确定”按钮时触发该回调。<br/>value：选中的日期值 |
| onCancel   | () => void                                | 否   | 点击弹窗中的“取消”按钮时触发该回调。                         |
| onChange   | (value: Date) => void                     | 否   | 选择弹窗中日期使当前选中项改变时触发该回调。<br/>value：选中的日期值 |
