# ArkTS声明式开发范式跨平台支持列表

## 通用事件

| 支持项                                                       | 支持情况 | 具体规格项                               | 是否平台强相关                        |
| ------------------------------------------------------------ | -------- | ---------------------------------------- | ---------------------------------------- |
| [点击事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-events-click.md/) | 支持     |                                          | 是，需要对接平台事件 |
| [触摸事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-events-touch.md/) | 支持     |                                          | 是，需要对接平台事件 |
| [挂载卸载事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-events-show-hide.md/) | 支持     |                                          |  |
| [拖拽事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-events-drag-drop.md/) | 部分支持 | 仅支持应用内拖拽  | 是，需要对接平台事件 |
| [按键事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-events-key.md/) | 支持     |                                          | 是，需要对接平台事件 |
| [焦点事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-focus-event.md/) | 支持     |                                          | 是，需要对接平台事件 |
| [组件区域变化事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-component-area-change-event.md/) | 支持     |                                          |  |
| [组件可见区域变化事件](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-component-visible-area-change-event.md/) | 支持     |                                          |  |

## 通用属性

| 支持项                                                       | 支持情况 | 具体规格项                                | 是否平台强相关                         |
| ------------------------------------------------------------ | -------- | ----------------------------------------- | ----------------------------------------- |
| [尺寸设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-size.md/) | 支持     |                                           |                                           |
| [位置设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-location.md/) | 支持     |                                           |                                           |
| [布局约束](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-layout-constraints.md/) | 支持     |                                           |                                           |
| [Flex布局](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-flex-layout.md/) | 支持     |                                           |                                           |
| [边框设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-border.md/) | 支持     |                                           |                                           |
| [图片边框设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-border-image.md/) | 支持     |                                           |                                           |
| [背景设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-background.md/) | 支持     |                                           |                                           |
| [透明度设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-opacity.md/) | 支持     |                                           |                                           |
| [显隐控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-visibility.md/) | 支持     |                                           |                                           |
| [禁用控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-enable.md/) | 支持     |                                           |                                           |
| [浮层](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-overlay.md/) | 支持     |                                           |                                           |
| [Z序控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-z-order.md/) | 支持     |                                           |                                           |
| [图形变换](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-transformation.md/) | 支持     |                                           |                                           |
| [图像效果](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-image-effect.md/) | 支持     |                                           |                                           |
| [形状裁剪](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-sharp-clipping.md/) | 支持     |                                           |                                           |
| [文本样式设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-text-style.md/) | 支持     |                                           | 是，跟平台默认字体相关 |
| [栅格设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-grid.md/) | 支持     |                                           |                                           |
| [颜色渐变](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-gradient-color.md/) | 支持     |                                           |                                           |
| [Popup控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-popup.md/) | 支持     | Popup非独立窗口，显示区域无法超过当前窗口 |  |
| [Menu控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-menu.md/) | 支持     | Menu非独立窗口，显示区域无法超过当前窗口  |   |
| [点击控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-click.md/) | 支持     |                                           | 是，需要对接平台事件 |
| [焦点控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-focus.md/) | 支持     |                                           | 是，需要对接平台事件 |
| [组件标识](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-component-id.md/) | 支持     |                                           |                                           |
| [触摸热区设置](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-touch-target.md/) | 支持     |                                           | 是，需要对接平台事件 |
| [多态样式](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-polymorphic-style.md/) | 支持     |                                           |                                           |
| [触摸测试控制](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-hit-test-behavior.md/) | 支持     |                                           |                                           |
| [组件背景模糊](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-universal-attributes-backgroundBlurStyle.md/) | 支持     | 非跨窗口模糊                              |                               |


## 手势处理

| 支持项                                                       | 支持情况 | 具体规格项 | 是否平台强相关 |
| ------------------------------------------------------------ | -------- | ---------- | -------------- |
| [绑定手势方法](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-gesture-settings.md/) | 支持     |            |                |
| [基础手势](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-gestures-tapgesture.md/) | 支持     |            |                |
| [组合手势](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-combined-gestures.md/) | 支持     |            |                |

## 全局UI方法

| 支持项                                                       | 支持情况 | 具体规格项 | 是否平台强相关 |
| ------------------------------------------------------------ | -------- | ---------- | -------------- |
| [弹窗](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-methods-alert-dialog-box.md/) | 支持     |            |                |
| [菜单](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-methods-menu.md/) | 支持     |            |                |


## 基础组件

| 支持项                                                       | 支持情况 | 具体规格项                               | 是否平台强相关                        |
| ------------------------------------------------------------ | -------- | ---------------------------------------- | ---------------------------------------- |
| [AlphabetIndexer](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-alphabet-indexer.md/) | 支持     |                                          |                                          |
| [Blank](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-blank.md/) | 支持     |                                          |                                          |
| [Button](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-button.md/) | 支持     |                                          |                                          |
| [Checkbox](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-checkbox.md/) | 支持     |                                          |                                          |
| [CheckboxGroup](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-checkboxgroup.md/) | 支持     |                                          |                                          |
| [DataPanel](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-datapanel.md/) | 支持     |                                          |                                          |
| [DatePicker](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-datepicker.md/) | 支持     |                                          | 是，日期显示依赖国际化库 |
| [Divider](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-divider.md/) | 支持     |                                          |                                          |
| [Gauge](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-gauge.md/) | 支持     |                                          |                                          |
| [Image](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-image.md/) | 支持     |                    | 是，图片加载依赖平台：包括本地资源、网络资源读取 |
| [ImageAnimator](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-imageanimator.md/) | 支持     |                                          | 是，图片加载依赖平台：包括本地资源、网络资源读取 |
| [LoadingProgress](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-loadingprogress.md/) | 支持     |                                          |                                          |
| [Marquee](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-marquee.md/) | 支持     |                                          |                                          |
| [Menu](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-menu.md/) | 支持     | Menu非独立窗口，显示区域无法超过当前窗口 |  |
| [MenuItem](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-menuitem.md/) | 支持     | Menu非独立窗口，显示区域无法超过当前窗口 |  |
| [MenuItemGroup](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-menuitemgroup.md/) | 支持     |                                          |                                          |
| [Navigation](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-navigation.md/) | 支持     |                                          |  |
| [NavRouter](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-navrouter.md/) | 支持     |                                          |                                          |
| [NavDestination](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-navdestination.md/) | 支持     |                                          |                                          |
| [PatternLock](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-patternlock.md/) | 支持     |                                          |                                          |
| [Progress](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-progress.md/) | 支持     |                                          |                                          |
| [QRCode](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-qrcode.md/) | 支持     |                                          |                                          |
| [Radio](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-radio.md/) | 支持     |                                          |                                          |
| [Rating](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-rating.md/) | 支持     |                                          |                                          |
| [ScrollBar](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-scrollbar.md/) | 支持     |                                          |                                          |
|  [Select](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-select.md/) | 支持     |                                          |                                          |
| [Slider](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-slider.md/) | 支持     |                                          |                                          |
| [Span](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-span.md/) | 支持     |                                          |                                          |
| [Stepper](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-stepper.md/) | 支持     |                                          |                                          |
| [StepperItem](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-stepperitem.md/) | 支持     |                                          |                                          |
| [Text](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-text.md/) | 支持     |                                          | 是，依赖平台字体库 |
| [TextClock](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-textclock.md/) | 支持     |                                          |  |
| [TextPicker](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-textpicker.md/) | 支持     |                                          |                                          |
| [TextTimer](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-texttimer.md/) | 支持     |                                          |                                          |
| [TimePicker](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-timepicker.md/) | 支持     |                                          |                                          |
| [Toggle](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-toggle.md/) | 支持     |                                          |                                          |
| [Badge](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-badge.md/) | 支持     |            |                                    |
| [Column](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-column.md/) | 支持     |            |                                    |
| [ColumnSplit](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-columnsplit.md/) | 支持     |            |                                    |
| [Counter](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-counter.md/) | 支持     |            |                                    |
| [Flex](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-flex.md/) | 支持     |            |                                    |
| [GridCol](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-gridcol.md/) | 支持     |            | 是，依赖设备相关信息               |
| [GridRow](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-gridrow.md/) | 支持     |            | 是，依赖设备相关信息               |
| [Grid](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-grid.md/) | 支持     |            |                                    |
| [GridItem](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-griditem.md/) | 支持     |            |                                    |
| [List](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-list.md/) | 支持     |            |                                    |
| [ListItem](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-listitem.md/) | 支持     |            |                                    |
| [ListItemGroup](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-listitemgroup.md/) | 支持     |            |                                    |
| [Navigator](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-navigator.md/) | 支持     |            | 是，返回到最后一级动作依赖平台能力 |
| [Panel](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-panel.md/) | 支持     |            |                                    |
| [Refresh](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-refresh.md/) | 支持     |            |                                    |
| [RelativeContainer](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-relativecontainer.md/) | 支持     |            |                                    |
| [Row](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-row.md/) | 支持     |            |                                    |
| [RowSplit](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-rowsplit.md/) | 支持     |            |                                    |
| [Scroll](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-scroll.md/) | 支持     |            |                                    |
| [SideBarContainer](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-sidebarcontainer.md/) | 支持     |            |                                    |
| [Stack](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-stack.md/) | 支持     |            |                                    |
| [Swiper](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-swiper.md/) | 支持     |            |                                    |
| [Tabs](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-tabs.md/) | 支持     |            |                                    |
| [TabContent](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-container-tabcontent.md/) | 支持     |            |                                    |

## 高级组件

| 支持项                                                       | 支持情况 | 具体规格项                    | 是否平台强相关          |
| ------------------------------------------------------------ | -------- | ----------------------------- | ----------------------- |
| [Video](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-media-components-video.md/) | 支持 |  **依赖平台Surface、MediaPlayer高级组件**<br>**补齐具体规格项** | 是，依赖平台独立Surface |
| [Search](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-search.md/) | 支持     | **依赖输入法高级组件**                       | 是，依赖平台输入事件             |
| [TextArea](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-textarea.md/) | 支持     | **依赖输入法高级组件**                       | 是，依赖平台字体库，依赖平台输入事件     |
| [TextInput](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-basic-components-textinput.md/) | 支持     | **依赖输入法高级组件**                       | 是，依赖平台字体库，依赖平台输入事件     |


## 绘制组件

| 支持项                                                       | 支持情况 | 具体规格项 | 是否平台强相关 |
| ------------------------------------------------------------ | -------- | ---------- | -------------- |
| [Circle](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-circle.md/) | 支持     |            |                |
| [Ellipse](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-ellipse.md/) | 支持     |            |                |
| [Line](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-line.md/) | 支持     |            |                |
| [Polyline](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-polyline.md/) | 支持     |            |                |
| [Polygon](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-polygon.md/) | 支持     |            |                |
| [Path](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-path.md/) | 支持     |            |                |
| [Rect](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-rect.md/) | 支持     |            |                |
| [Shape](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-drawing-components-shape.md/) | 支持     |            |                |
| [Canvas](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-components-canvas-canvas.md/) | 支持     |            |                |

## 动画

| 支持项                                                       | 支持情况 | 具体规格项 | 是否平台强相关 |
| ------------------------------------------------------------ | -------- | ---------- | -------------- |
| [属性动画](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-animatorproperty.md/) | 支持     |            |                |
| [显式动画](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-explicit-animation.md/) | 支持     |            |                |
| [转场动画](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-page-transition-animation.md/) | 支持     |            |                |
| [路径动画](https://docs.openharmony.cn/pages/v3.2/zh-cn/application-dev/reference/arkui-ts/ts-motion-path-animation.md/) | 支持     |            |                |

## 状态管理

| 数据管理          | 支持情况 | 是否平台强相关 |
| ------------------- | -------------- | -------------- |
| AppStorage        | 支持           |            |
| LocalStorage      | 支持          |           |
| PersistentStorage | 支持           | 是，依赖平台存储能力对接 |
| Environment       | 支持          | 是，依赖平台环境信息对接 |
| @Component                | 支持 |  |
| @Entry                    | 支持 |  |
| @Preview                  | 支持 |  |
| @Builder                  | 支持 |  |
| @Extend                   | 支持 |  |
| @CustomDialog             | 支持 |  |
| @Styles                   | 支持 |  |
| @State                    | 支持 |  |
| @Prop                     | 支持 |  |
| @Link                     | 支持 |  |
| @StorageLink/@StorageProp | 支持 |  |
| @LocalStorageLink         | 支持 |  |
| @LocalStorageProp         | 支持 |  |
| @Observed/@ObjectLink     | 支持 |  |
| @Consume/@Provide         | 支持 |  |
| @Watch                    | 支持 |  |
