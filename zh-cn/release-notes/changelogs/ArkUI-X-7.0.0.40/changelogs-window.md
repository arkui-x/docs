# 窗口子系统变更说明

## cl.window.1 setPreferredOrientation接口适用范围变更说明

**访问级别**

公开接口

**变更原因**

明确 API version 26.1.0 中 `setPreferredOrientation` 接口的适用窗口范围，统一窗口旋转行为。

**变更影响**

该变更为不兼容变更。

- 变更前：`setPreferredOrientation` 接口未明确限定调用窗口类型。

- 变更后：从 API version 26.1.0 开始，`setPreferredOrientation` 接口仅对应用主窗口生效，其它窗口类型均不生效。

**起始 API Level**

26.1.0

**变更发生版本**

从 ArkUI-X SDK 7.0.0.40 开始。

**变更的接口/组件**

`window.Window.setPreferredOrientation`

**适配指导**

应用请在主窗口对象上调用 `setPreferredOrientation` 设置显示方向。其它窗口调用该接口不生效。