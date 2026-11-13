# \@Env: System Environment Variables


In multi-device development scenarios (such as phones, foldables, and tablets), you can use the [\@Env](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/reference/apis-arkui/arkui-ts/ts-env-system-property.md) decorator to listen for changes to system environment variables and adapt your application to the corresponding scenarios, reducing adaptation logic and redundant development across devices.


## Overview

\@Env is a reactive system environment variable decorator. It provides the following capabilities:

- Reads the corresponding environment variable information based on the decorator argument. The following environment variable is currently supported:
  - [`SystemProperties.BREAK_POINT`](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/reference/apis-arkui/arkui-ts/ts-env-system-property.md#systemproperties): obtains the breakpoint information corresponding to the window's width and height thresholds, of the [`uiObserver.WindowSizeLayoutBreakpointInfo`](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/reference/apis-arkui/js-apis-arkui-observer.md#windowsizelayoutbreakpointinfo22) type.
- When a system environment variable changes (for example, when the window size changes or the screen orientation switches), the \@Env decorated variable is notified to update, and the components associated with \@Env are refreshed, keeping the UI content up to date.
- Variables decorated by \@Env must not be initialized by developers. Their values are automatically provided by the framework based on the environment variables of the current window.


## Constraints

- \@Env can only be used in custom components decorated by [\@Component](arkts-create-custom-components.md) or [\@ComponentV2](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/ui/state-management/arkts-create-custom-components.md#componentv2). Otherwise, a compile-time error occurs.
- When \@Env uses `SystemProperties.BREAK_POINT`, the decorated variable must be of the `uiObserver.WindowSizeLayoutBreakpointInfo` type. Otherwise, a compile-time error occurs.
- Variables decorated by \@Env are read-only. Initializing or assigning values to them is not allowed. Otherwise, a compile-time error occurs.

  ```ts
  import { uiObserver } from '@kit.ArkUI';

  @Entry
  @Component
  struct Index {
    // Incorrect: initializing an @Env decorated variable locally causes a compile-time error
    @Env(SystemProperties.BREAK_POINT) breakpoint: uiObserver.WindowSizeLayoutBreakpointInfo =
      new uiObserver.WindowSizeLayoutBreakpointInfo();

    build() {
    }
  }
  ```

- \@Env cannot be used together with other state variable decorators. Otherwise, a compile-time error occurs. In \@Component, you can use [\@Watch](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/ui/state-management/arkts-watch.md) to listen for changes of \@Env decorated variables. For details, see [Listening for Breakpoint Changes with \@Watch](#listening-for-breakpoint-changes-with-watch).
- When an \@Env decorated variable is passed to a child component, the following rules apply:
  - In a custom component decorated by \@Component, it can only be used to initialize regular variables (not decorated by any state decorator) of the child component.
  - In a custom component decorated by \@ComponentV2, it can only be used to initialize variables decorated by [\@Param](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/ui/state-management/arkts-new-param.md) of the child component.


## Application Scenarios

### Obtaining Window Breakpoint Information

When \@Env uses `SystemProperties.BREAK_POINT`, it returns the layout breakpoint information `WindowSizeLayoutBreakpointInfo` of the current window size, which contains the following attributes:

| Attribute | Type | Description |
| -------- | -------- | -------- |
| widthBreakpoint | [WidthBreakpoint](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/reference/apis-arkui/arkui-ts/ts-appendix-enums.md#widthbreakpoint13) | Window width breakpoint. WIDTH_XS: width less than 320 vp; WIDTH_SM: width greater than or equal to 320 vp and less than 600 vp; WIDTH_MD: width greater than or equal to 600 vp and less than 840 vp; WIDTH_LG: width greater than or equal to 840 vp and less than 1440 vp; WIDTH_XL: width greater than or equal to 1440 vp. |
| heightBreakpoint | [HeightBreakpoint](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/reference/apis-arkui/arkui-ts/ts-appendix-enums.md#heightbreakpoint13) | Window height breakpoint. HEIGHT_SM: window aspect ratio less than 0.8; HEIGHT_MD: window aspect ratio greater than or equal to 0.8 and less than 1.2; HEIGHT_LG: window aspect ratio greater than or equal to 1.2. |

In the following example, an \@Env decorated variable `breakpoint` is declared in \@Component, and its width and height breakpoint values are read in the build method. When a window size change causes the breakpoint to change, the framework notifies `breakpoint` to update and refreshes the associated Text components.

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


### Implementing Adaptive Layout with Breakpoints

Based on the width breakpoint value, you can provide different layout structures for different window forms to implement adaptive layout across phones, foldables, and tablets.

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
        // Wide window: sidebar on the left and content on the right
        this.SideBar()
        this.Content()
      } else {
        // Narrow window: content only
        this.Content()
      }
    }
    .width('100%')
    .height('100%')
  }
}
```


### Listening for Breakpoint Changes with \@Watch

In \@Component, you can use [\@Watch](https://gitcode.com/openharmony/docs/blob/master/en/application-dev/ui/state-management/arkts-watch.md) to listen for changes of \@Env decorated variables. Note that the \@Watch callback is triggered only when the \@Env decorated variable is assigned as a whole. Changes of its internal attributes do not trigger the callback.

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
