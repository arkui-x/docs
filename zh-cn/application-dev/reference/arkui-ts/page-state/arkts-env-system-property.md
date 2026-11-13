# \@Env：系统环境变量


在手机、折叠屏、平板等的多设备开发场景中，开发者可以使用[\@Env](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/arkui-ts/ts-env-system-property.md)装饰器监听系统环境变量的改变，并根据系统环境变量的值进行相应的场景适配，以减少不同设备间的适配逻辑和重复开发。


## 概述

\@Env是响应式系统环境变量装饰器，其功能包括：

- 根据入参读取相应的环境变量信息，当前支持以下环境变量：
  - [`SystemProperties.BREAK_POINT`](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/arkui-ts/ts-env-system-property.md#systemproperties)：用于获取窗口不同宽高阈值下对应的断点值信息，类型为[`uiObserver.WindowSizeLayoutBreakpointInfo`](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/js-apis-arkui-observer.md#windowsizelayoutbreakpointinfo22)。
- 系统环境变量改变时（如窗口尺寸变化、横竖屏切换），会通知\@Env装饰的变量更新，并触发\@Env关联组件刷新，以实现界面内容的同步更新。
- \@Env装饰的变量不允许开发者初始化，其值由框架根据当前窗口的环境变量自动提供。


## 限制条件

- \@Env仅支持在[\@Component](arkts-create-custom-components.md)和[\@ComponentV2](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/ui/state-management/arkts-create-custom-components.md#componentv2)装饰的自定义组件中使用，否则会有编译时报错。
- \@Env使用`SystemProperties.BREAK_POINT`时，装饰的变量类型必须为`uiObserver.WindowSizeLayoutBreakpointInfo`类型，否则会有编译时报错。
- \@Env装饰的变量为只读属性，不允许开发者进行初始化或赋值操作，否则会有编译时报错。

  ```ts
  import { uiObserver } from '@kit.ArkUI';

  @Entry
  @Component
  struct Index {
    // 错误用法：@Env装饰的变量不允许本地初始化，编译时报错
    @Env(SystemProperties.BREAK_POINT) breakpoint: uiObserver.WindowSizeLayoutBreakpointInfo =
      new uiObserver.WindowSizeLayoutBreakpointInfo();

    build() {
    }
  }
  ```

- \@Env不能和其他状态变量装饰器联用，否则会有编译时报错。在\@Component中，可以通过[\@Watch](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/ui/state-management/arkts-watch.md)监听\@Env装饰变量的变化，具体示例见[\@Watch监听断点变化](#watch监听断点变化)。
- \@Env装饰的变量传递给子组件时，遵循以下规则：
  - 在\@Component装饰的自定义组件中，仅能用于初始化子组件中的常规变量（无装饰器修饰的变量）。
  - 在\@ComponentV2装饰的自定义组件中，仅能用于初始化子组件中[\@Param](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/ui/state-management/arkts-new-param.md)装饰的变量。


## 使用场景

### 获取窗口断点信息

\@Env使用`SystemProperties.BREAK_POINT`时，返回当前窗口尺寸对应的布局断点信息`WindowSizeLayoutBreakpointInfo`，包含以下属性：

| 属性 | 类型 | 说明 |
| -------- | -------- | -------- |
| widthBreakpoint | [WidthBreakpoint](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/arkui-ts/ts-appendix-enums.md#widthbreakpoint13) | 窗口宽度断点。WIDTH_XS：宽度小于320vp；WIDTH_SM：宽度大于等于320vp且小于600vp；WIDTH_MD：宽度大于等于600vp且小于840vp；WIDTH_LG：宽度大于等于840vp且小于1440vp；WIDTH_XL：宽度大于等于1440vp。 |
| heightBreakpoint | [HeightBreakpoint](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/reference/apis-arkui/arkui-ts/ts-appendix-enums.md#heightbreakpoint13) | 窗口高度断点。HEIGHT_SM：窗口宽高比小于0.8；HEIGHT_MD：窗口宽高比大于等于0.8且小于1.2；HEIGHT_LG：窗口宽高比大于等于1.2。 |

下面的示例中，在\@Component中声明\@Env装饰的变量`breakpoint`，并在build方法中读取其宽高断点值。窗口尺寸变化引起断点变化时，框架会通知`breakpoint`更新，并触发关联的Text组件刷新。

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @Env(SystemProperties.BREAK_POINT) breakpoint: uiObserver.WindowSizeLayoutBreakpointInfo;

  build() {
    Column() {
      Text(`widthBreakpoint: ${this.breakpoint.widthBreakpoint}`).fontSize(20)
      Text(`heightBreakpoint: ${this.breakpoint.heightBreakpoint}`).fontSize(20)
    }
    .width('100%')
    .height('100%')
  }
}
```


### 使用断点进行自适应布局

结合宽度断点值，开发者可以为不同窗口形态提供不同的布局结构，实现在手机、折叠屏、平板上的自适应布局。

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @Env(SystemProperties.BREAK_POINT) breakpoint: uiObserver.WindowSizeLayoutBreakpointInfo;

  @Builder
  SideBar() {
    Text('Sidebar').width(200).height('100%').backgroundColor('#f1f1f1')
  }

  @Builder
  Content() {
    Column() {
      Text(`widthBreakpoint: ${this.breakpoint.widthBreakpoint}`).fontSize(20)
    }
    .width('100%')
  }

  build() {
    Row() {
      if (this.breakpoint.widthBreakpoint === WidthBreakpoint.WIDTH_LG ||
        this.breakpoint.widthBreakpoint === WidthBreakpoint.WIDTH_XL) {
        // 宽窗口：左右结构
        this.SideBar()
        this.Content()
      } else {
        // 窄窗口：仅显示内容
        this.Content()
      }
    }
    .width('100%')
    .height('100%')
  }
}
```


### \@Watch监听断点变化

在\@Component中，可以通过[\@Watch](https://gitcode.com/openharmony/docs/blob/master/zh-cn/application-dev/ui/state-management/arkts-watch.md)监听\@Env装饰变量的变化。需要注意的是，仅当\@Env装饰的变量被整体赋值时才会触发\@Watch监听回调，其内部属性的变化不会触发回调。

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @Env(SystemProperties.BREAK_POINT) @Watch('onBreakpointChange') breakpoint: uiObserver.WindowSizeLayoutBreakpointInfo;

  onBreakpointChange(propName: string) {
    console.info(`the value of this.${propName} has changed`);
  }

  build() {
    Column() {
      Text(`widthBreakpoint: ${this.breakpoint.widthBreakpoint}`).fontSize(20)
      Text(`heightBreakpoint: ${this.breakpoint.heightBreakpoint}`).fontSize(20)
    }
    .width('100%')
    .height('100%')
  }
}
```
