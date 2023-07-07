# ArkTS声明式开发范式跨平台支持列表

## 通用事件

| 支持项                                                       | 支持情况 | 具体规格项                               | 是否平台强相关                        |
| ------------------------------------------------------------ | -------- | ---------------------------------------- | ---------------------------------------- |
| [点击事件](ts-universal-events-click.md) | 支持     |                                          | 是，需要对接平台事件 |
| [触摸事件](ts-universal-events-touch.md) | 支持     |                                          | 是，需要对接平台事件 |
| [挂载卸载事件](ts-universal-events-show-hide.md) | 支持     |                                          |  |
| [拖拽事件](ts-universal-events-drag-drop.md) | 部分支持 | 仅支持应用内拖拽  | 是，需要对接平台事件 |
| [按键事件](ts-universal-events-key.md) | 支持     |                                          | 是，需要对接平台事件 |
| [焦点事件](ts-universal-focus-event.md) | 支持     |                                          | 是，需要对接平台事件 |
| [鼠标事件](ts-universal-mouse-key.md) | 支持 | |  |
| [组件区域变化事件](ts-universal-component-area-change-event.md) | 支持     |                                          |  |
| [组件可见区域变化事件](ts-universal-component-visible-area-change-event.md) | 支持     |                                          |  |

## 通用属性

| 支持项                                                       | 支持情况 | 具体规格项                                | 是否平台强相关                         |
| ------------------------------------------------------------ | -------- | ----------------------------------------- | ----------------------------------------- |
| [尺寸设置](ts-universal-attributes-size.md) | 支持     |                                           |                                           |
| [位置设置](ts-universal-attributes-location.md) | 支持     |                                           |                                           |
| [布局约束](ts-universal-attributes-layout-constraints.md) | 支持     |                                           |                                           |
| [Flex布局](ts-universal-attributes-flex-layout.md) | 支持     |                                           |                                           |
| [边框设置](ts-universal-attributes-border.md) | 支持     |                                           |                                           |
| [图片边框设置](ts-universal-attributes-border-image.md) | 支持     |                                           |                                           |
| [背景设置](ts-universal-attributes-background.md) | 支持     |                                           |                                           |
| [透明度设置](ts-universal-attributes-opacity.md) | 支持     |                                           |                                           |
| [显隐控制](ts-universal-attributes-visibility.md) | 支持     |                                           |                                           |
| [禁用控制](ts-universal-attributes-enable.md) | 支持     |                                           |                                           |
| [浮层](ts-universal-attributes-overlay.md) | 支持     |                                           |                                           |
| [Z序控制](ts-universal-attributes-z-order.md) | 支持     |                                           |                                           |
| [图形变换](ts-universal-attributes-transformation.md) | 支持     |                                           |                                           |
| [图像效果](ts-universal-attributes-image-effect.md) | 支持     |                                           |                                           |
| [形状裁剪](ts-universal-attributes-sharp-clipping.md) | 支持     |                                           |                                           |
| [文本通用](ts-universal-attributes-text-style.md) | 支持     |                                           | 是，跟平台默认字体相关 |
| [栅格设置](ts-universal-attributes-grid.md) | 支持     |                                           |                                           |
| [颜色渐变](ts-universal-attributes-gradient-color.md) | 支持     |                                           |                                           |
| [Popup控制](ts-universal-attributes-popup.md) | 支持     | Popup非独立窗口，显示区域无法超过当前窗口 |  |
| [菜单控制](ts-universal-attributes-menu.md) | 支持     | Menu非独立窗口，显示区域无法超过当前窗口  |   |
| [焦点控制](ts-universal-attributes-focus.md) | 支持     |                                           | 是，需要对接平台事件 |
| [悬浮态效果](ts-universal-attributes-hover-effect.md) | 支持 | |  |
| [组件标识](ts-universal-attributes-component-id.md) | 支持     |                                           |                                           |
| [触摸热区设置](ts-universal-attributes-touch-target.md) | 支持     |                                           | 是，需要对接平台事件 |
| [多态样式](ts-universal-attributes-polymorphic-style.md) | 支持     |                                           |                                           |
| [触摸测试控制](ts-universal-attributes-hit-test-behavior.md) | 支持     |                                           |                                           |
| [前景色设置](ts-universal-attributes-foreground-color.md) | 支持     | 非跨窗口模糊                              |                               |
| [组件内容模糊](ts-universal-attributes-foreground-blur-style.md) | 支持 |  | |
| [全屏模态转场](ts-universal-attributes-modal-transition.md) | 支持 |  | |
| [半模态转场](ts-universal-attributes-sheet-transition.md) | 支持 |  | |
| [隐私遮罩](ts-universal-attributes-obscured.md) | 支持 |  | |


## 手势处理

| 支持项                                                    | 支持情况 | 具体规格项 | 是否平台强相关 |
| --------------------------------------------------------- | -------- | ---------- | -------------- |
| [绑定手势方法](ts-gesture-settings.md)                    | 支持     |            |                |
| [TapGesture](ts-basic-gestures-tapgesture.md)             | 支持     |            |                |
| [LongPressGesture](ts-basic-gestures-longpressgesture.md) | 支持     |            |                |
| [PanGesture](ts-basic-gestures-pangesture.md)             | 支持     |            |                |
| [PinchGesture](ts-basic-gestures-pinchgesture.md)         | 支持     |            |                |
| [RotationGesture](ts-basic-gestures-rotationgesture.md)   | 支持     |            |                |
| [SwipeGesture](ts-basic-gestures-swipegesture.md)         | 支持     |            |                |
| [组合手势](ts-combined-gestures.md)                       | 支持     |            |                |

## 全局UI方法

| 支持项                                                | 支持情况 | 具体规格项 | 是否平台强相关 |
| ----------------------------------------------------- | -------- | ---------- | -------------- |
| [警告弹窗](ts-methods-alert-dialog-box.md)            | 支持     |            |                |
| [列表选择弹窗](ts-methods-action-sheet.md)            | 支持     |            |                |
| [自定义弹窗](ts-methods-custom-dialog-box.md)         | 支持     |            |                |
| [日期滑动选择器弹窗](ts-methods-datepicker-dialog.md) | 支持     |            |                |
| [时间滑动选择器弹窗](ts-methods-timepicker-dialog.md) | 支持     |            |                |
| [文本滑动选择器弹窗](ts-methods-textpicker-dialog.md) | 支持     |            |                |
| [菜单](ts-methods-menu.md)                            | 支持     |            |                |


## 基础组件

| 支持项                                                       | 支持情况 | 具体规格项                               | 是否平台强相关                        |
| ------------------------------------------------------------ | -------- | ---------------------------------------- | ---------------------------------------- |
| [AlphabetIndexer](ts-container-alphabet-indexer.md) | 支持     |                                          |                                          |
| [Blank](ts-basic-components-blank.md) | 支持     |                                          |                                          |
| [Button](ts-basic-components-button.md) | 支持     |                                          |                                          |
| [Checkbox](ts-basic-components-checkbox.md) | 支持     |                                          |                                          |
| [CheckboxGroup](ts-basic-components-checkboxgroup.md) | 支持     |                                          |                                          |
| [DataPanel](ts-basic-components-datapanel.md) | 支持     |                                          |                                          |
| [DatePicker](ts-basic-components-datepicker.md) | 支持     |                                          | 是，日期显示依赖国际化库 |
| [Divider](ts-basic-components-divider.md) | 支持     |                                          |                                          |
| [Gauge](ts-basic-components-gauge.md) | 支持     |                                          |                                          |
| [Image](ts-basic-components-image.md) | 支持     |                    | 是，图片加载依赖平台：包括本地资源、网络资源读取 |
| [ImageAnimator](ts-basic-components-imageanimator.md) | 支持     |                                          | 是，图片加载依赖平台：包括本地资源、网络资源读取 |
| [ImageSpan](ts-basic-components-imagespan.md) | 支持 | |  |
| [LoadingProgress](ts-basic-components-loadingprogress.md) | 支持     |                                          |                                          |
| [Marquee](ts-basic-components-marquee.md) | 支持     |                                          |                                          |
| [Menu](ts-basic-components-menu.md) | 支持     | Menu非独立窗口，显示区域无法超过当前窗口 |  |
| [MenuItem](ts-basic-components-menuitem.md) | 支持     | Menu非独立窗口，显示区域无法超过当前窗口 |  |
| [MenuItemGroup](ts-basic-components-menuitemgroup.md) | 支持     |                                          |                                          |
| [Navigation](ts-basic-components-navigation.md) | 支持     |                                          |  |
| [NavRouter](ts-basic-components-navrouter.md) | 支持     |                                          |                                          |
| [NavDestination](ts-basic-components-navdestination.md) | 支持     |                                          |                                          |
| [PatternLock](ts-basic-components-patternlock.md) | 支持     |                                          |                                          |
| [Progress](ts-basic-components-progress.md) | 支持     |                                          |                                          |
| [QRCode](ts-basic-components-qrcode.md) | 支持     |                                          |                                          |
| [Radio](ts-basic-components-radio.md) | 支持     |                                          |                                          |
| [Rating](ts-basic-components-rating.md) | 支持     |                                          |                                          |
| [ScrollBar](ts-basic-components-scrollbar.md) | 支持     |                                          |                                          |
| [Select](ts-basic-components-select.md) | 支持     |                                          |                                          |
| [Slider](ts-basic-components-slider.md) | 支持     |                                          |                                          |
| [Span](ts-basic-components-span.md) | 支持     |                                          |                                          |
| [Stepper](ts-basic-components-stepper.md) | 支持     |                                          |                                          |
| [StepperItem](ts-basic-components-stepperitem.md) | 支持     |                                          |                                          |
| [Text](ts-basic-components-text.md) | 支持     |                                          | 是，依赖平台字体库 |
| [TextClock](ts-basic-components-textclock.md) | 支持     |                                          |  |
| [TextPicker](ts-basic-components-textpicker.md) | 支持     |                                          |                                          |
| [TextTimer](ts-basic-components-texttimer.md) | 支持     |                                          |                                          |
| [TimePicker](ts-basic-components-timepicker.md) | 支持     |                                          |                                          |
| [Toggle](ts-basic-components-toggle.md) | 支持     |                                          |                                          |
| [Badge](ts-container-badge.md) | 支持     |            |                                    |
| [Column](ts-container-column.md) | 支持     |            |                                    |
| [ColumnSplit](ts-container-columnsplit.md) | 支持     |            |                                    |
| [Counter](ts-container-counter.md) | 支持     |            |                                    |
| [Flex](ts-container-flex.md) | 支持     |            |                                    |
| [FlowItem](ts-container-flowitem.md) | 支持 | | |
| [GridCol](ts-container-gridcol.md) | 支持     |            | 是，依赖设备相关信息               |
| [GridRow](ts-container-gridrow.md) | 支持     |            | 是，依赖设备相关信息               |
| [Grid](ts-container-grid.md) | 支持     |            |                                    |
| [GridItem](ts-container-griditem.md) | 支持     |            |                                    |
| [List](ts-container-list.md) | 支持     |            |                                    |
| [ListItem](ts-container-listitem.md) | 支持     |            |                                    |
| [ListItemGroup](ts-container-listitemgroup.md) | 支持     |            |                                    |
| [Navigator](ts-container-navigator.md) | 支持     |            | 是，返回到最后一级动作依赖平台能力 |
| [Panel](ts-container-panel.md) | 支持     |            |                                    |
| [Refresh](ts-container-refresh.md) | 支持     |            |                                    |
| [RelativeContainer](ts-container-relativecontainer.md) | 支持     |            |                                    |
| [Row](ts-container-row.md) | 支持     |            |                                    |
| [RowSplit](ts-container-rowsplit.md) | 支持     |            |                                    |
| [Scroll](ts-container-scroll.md) | 支持     |            |                                    |
| [SideBarContainer](ts-container-sidebarcontainer.md) | 支持     |            |                                    |
| [Stack](ts-container-stack.md) | 支持     |            |                                    |
| [Swiper](ts-container-swiper.md) | 支持     |            |                                    |
| [Tabs](ts-container-tabs.md) | 支持     |            |                                    |
| [TabContent](ts-container-tabcontent.md) | 支持     |            |                                    |
| [WaterFlow](ts-container-waterflow.md) | 支持 | | |

## 高级组件

| 支持项                                                       | 支持情况 | 具体规格项                    | 是否平台强相关          |
| ------------------------------------------------------------ | -------- | ----------------------------- | ----------------------- |
| [Video](ts-media-components-video.md) | 支持 |  **依赖平台Surface、MediaPlayer高级组件**<br>**补齐具体规格项** | 是，依赖平台独立Surface |
| [Search](ts-basic-components-search.md) | 支持     | **依赖输入法高级组件**                       | 是，依赖平台输入事件             |
| [TextArea](ts-basic-components-textarea.md) | 支持     | **依赖输入法高级组件**                       | 是，依赖平台字体库，依赖平台输入事件     |
| [TextInput](ts-basic-components-textinput.md) | 支持     | **依赖输入法高级组件**                       | 是，依赖平台字体库，依赖平台输入事件     |


## 绘制组件

| 支持项                                        | 支持情况 | 具体规格项 | 是否平台强相关 |
| --------------------------------------------- | -------- | ---------- | -------------- |
| [Circle](ts-drawing-components-circle.md)     | 支持     |            |                |
| [Ellipse](ts-drawing-components-ellipse.md)   | 支持     |            |                |
| [Line](ts-drawing-components-line.md)         | 支持     |            |                |
| [Polyline](ts-drawing-components-polyline.md) | 支持     |            |                |
| [Polygon](ts-drawing-components-polygon.md)   | 支持     |            |                |
| [Path](ts-drawing-components-path.md)         | 支持     |            |                |
| [Rect](ts-drawing-components-rect.md)         | 支持     |            |                |
| [Shape](ts-drawing-components-shape.md)       | 支持     |            |                |
| [Canvas](ts-components-canvas-canvas.md)      | 支持     |            |                |
| [Matrix2D](ts-components-canvas-matrix2d.md)  | 支持     |            |                |

## 动画

| 支持项                                                     | 支持情况 | 具体规格项 | 是否平台强相关 |
| ---------------------------------------------------------- | -------- | ---------- | -------------- |
| [属性动画](ts-animatorproperty.md)                         | 支持     |            |                |
| [显式动画](ts-explicit-animation.md)                       | 支持     |            |                |
| [页面间转场](ts-page-transition-animation.md)              | 支持     |            |                |
| [共享元素转场](ts-transition-animation-shared-elements.md) | 支持     |            |                |
| [组件内转场](ts-transition-animation-component.md)         | 支持     |            |                |
| [路径动画](ts-motion-path-animation.md)                    | 支持     |            |                |

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
