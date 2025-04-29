# @ohos.arkui.observer (无感监听)

提供UI组件行为变化的无感监听能力。

> **说明：**
>
> 从API Version 11开始支持。后续版本如有新增内容，则采用上角标单独标记该内容的起始版本。
>

## 导入模块

```ts
import { uiObserver } from '@kit.ArkUI';
```

## NavDestinationState

NavDestination组件状态。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称      | 值  | 说明                     | Android平台         | iOS平台                |
| --------- | --- | ------------------------ | ------------------------ | ------------------------ |
| ON_SHOWN  | 0   | NavDestination组件显示。 | 支持 | 支持 |
| ON_HIDDEN | 1   | NavDestination组件隐藏。 | 支持 | 支持 |
| ON_APPEAR<sup>12+</sup> | 2   | NavDestination从组件树上挂载。 | 支持 | 支持 |
| ON_DISAPPEAR<sup>12+</sup> | 3   | NavDestination从组件树上卸载。 | 支持 | 支持 |
| ON_WILL_SHOW<sup>12+</sup> | 4   | NavDestination组件显示之前。 | 支持 | 支持 |
| ON_WILL_HIDE<sup>12+</sup> | 5   | NavDestination组件隐藏之前。 | 支持 | 支持 |
| ON_WILL_APPEAR<sup>12+</sup>| 6   | NavDestination挂载到组件树之前。 | 支持 | 支持 |
| ON_WILL_DISAPPEAR<sup>12+</sup>| 7   | NavDestination从组件树上卸载之前。 | 支持 | 支持 |
| ON_ACTIVE<sup>17+</sup> | 8 | NavDestination组件处于激活态。 | 支持 | 支持 |
| ON_INACTIVE<sup>17+</sup> | 9 | NavDestination组件处于非激活态。 | 支持 | 支持 |
| ON_BACKPRESS<sup>12+</sup> | 100   | NavDestination组件返回。 | 支持 | 支持 |

## ScrollEventType<sup>12+</sup>

滚动事件的类型。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称      | 值  | 说明                     | Android平台            | iOS平台       |
| --------- | --- | ------------------------ | ------------------------ | ------------------------ |
| SCROLL_START  | 0   | 滚动事件开始。 | 支持 | 支持 |
| SCROLL_STOP   | 1   | 滚动事件结束。 | 支持 | 支持 |

## RouterPageState

routerPage生命周期触发时对应的状态。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称              | 值  | 说明                    | Android平台        | iOS平台               |
| ----------------- | --- | ----------------------- | ----------------------- | ----------------------- |
| ABOUT_TO_APPEAR       | 0   | page即将显示。            | 支持          | 支持          |
| ABOUT_TO_DISAPPEAR    | 1   | page即将销毁。         | 支持       | 支持       |
| ON_PAGE_SHOW          | 2   | page显示。                | 支持              | 支持              |
| ON_PAGE_HIDE          | 3   | page隐藏。                | 支持              | 支持              |
| ON_BACK_PRESS         | 4   | page返回时。              | 支持            | 支持            |

## TabContentState<sup>12+</sup>

TabContent组件的状态。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称    | 值   | 说明                 | Android平台 | iOS平台 |
| ------- | ---- | -------------------- | ----------- | ------- |
| ON_SHOW | 0    | TabContent组件显示。 | 支持        | 支持    |
| ON_HIDE | 1    | TabContent组件隐藏。 | 支持        | 支持    |

## NavDestinationInfo

NavDestination组件信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称         | 类型                                               | 必填 | 说明                                         | Android平台                                | iOS平台                                    |
| ------------ | -------------------------------------------------- | ---- | -------------------------------------------- | -------------------------------------------- | -------------------------------------------- |
| navigationId | ResourceStr | 是   | 包含NavDestination组件的Navigation组件的id。 | 支持 | 支持 |
| name         | ResourceStr | 是   | NavDestination组件的名称。 | 支持 | 支持 |
| state        | [NavDestinationState](#navdestinationstate)        | 是   | NavDestination组件的状态。 | 支持 | 支持 |
| index<sup>12+</sup>        | number        | 是   | NavDestination在页面栈中的索引。   <br/> 取值范围：[0, +∞)      | 支持    | 支持    |
| param<sup>12+</sup>        | Object        | 否   | NavDestination组件的参数。               | 支持             | 支持             |
| navDestinationId<sup>12+</sup>        | string        | 是   | NavDestination组件的唯一标识ID。            | 支持          | 支持          |
| mode<sup>15+</sup>        | NavDestinationMode        | 否   | NavDestination类型。               | 支持             | 支持             |
| uniqueId<sup>15+</sup>        | number        | 否   | NavDestination组件的uniqueId。             | 支持           | 支持           |

## NavigationInfo<sup>12+</sup>

Navigation组件信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称         | 类型                                               | 必填 | 说明                                         | Android平台                                | iOS平台                                    |
| ------------ | -------------------------------------------------- | ---- | -------------------------------------------- | -------------------------------------------- | -------------------------------------------- |
| navigationId | string | 是   | Navigation组件的id。 | 支持 | 支持 |
| pathStack         | NavPathStack | 是   | Navigation组件的路由栈。                   | 支持                 | 支持                 |

## ScrollEventInfo<sup>12+</sup>

ScrollEvent滚动信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称         | 类型                                               | 必填 | 说明                                         | Android平台                               | iOS平台                                    |
| ------------ | -------------------------------------------------- | ---- | -------------------------------------------- | -------------------------------------------- | -------------------------------------------- |
| id           | string                                             | 是   | 滚动组件的id。                               | 支持                             | 支持                             |
| uniqueId           | number                                             | 是   | 滚动组件的uniqueId。                               | 支持                             | 支持                             |
| scrollEvent    | [ScrollEventType](#scrolleventtype12)                | 是   | 滚动事件的类型。                             | 支持                           | 支持                           |
| offset       | number                                             | 是   | 滚动组件的当前偏移量。                        | 支持                      | 支持                      |

## ObserverOptions<sup>12+</sup>

Observer选项。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称 | 类型   | 必填 | 说明       | Android平台 | iOS平台 |
| ---- | ------ | ---- | ---------- | ----------- | ------- |
| id   | string | 是   | 组件的id。 | 支持        | 支持    |

## RouterPageInfo

RouterPageInfo包含的信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称                 | 类型                                                         | 必填 | 说明                                                         | Android平台 | iOS平台 |
| -------------------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| context              | UIAbilityContext \| [UIContext](./js-apis-arkui-UIContext.md) | 是   | 触发生命周期的routerPage页面对应的上下文信息。               | 支持        | 支持    |
| index                | number                                                       | 是   | 触发生命周期的routerPage在栈中的位置。<br/> 取值范围：[0, +∞) | 支持        | 支持    |
| name                 | string                                                       | 是   | 触发生命周期的routerPage页面的名称。                         | 支持        | 支持    |
| path                 | string                                                       | 是   | 触发生命周期的routerPage页面的路径。                         | 支持        | 支持    |
| state                | [RouterPageState](#routerpagestate)                          | 是   | 触发生命周期的routerPage页面的状态。                         | 支持        | 支持    |
| pageId<sup>12+</sup> | string                                                       | 是   | 触发生命周期的routerPage页面的唯一标识。                     | 支持        | 支持    |

## DensityInfo<sup>12+</sup>

屏幕像素密度变化回调包含的信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**注意：** 有部分Android机器不支持。

| 名称    | 类型                                      | 必填 | 说明                                   | Android平台                          | iOS平台                              |
| ------- | ----------------------------------------- | ---- | -------------------------------------- | -------------------------------------- | -------------------------------------- |
| context | [UIContext](./js-apis-arkui-UIContext.md) | 是   | 屏幕像素密度变化时页面对应的上下文信息。 | 支持 | 不支持 |
| density | number                                    | 是   | 变化后的屏幕像素密度。<br/>取值范围：[0, +∞)                 | 支持               | 不支持              |

## NavDestinationSwitchInfo<sup>12+</sup>

Navigation组件页面切换的信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称      | 类型                                                         | 必填 | 说明                                       | Android平台 | iOS平台 |
| --------- | ------------------------------------------------------------ | ---- | ------------------------------------------ | ----------- | ------- |
| context   | UIAbilityContext\| [UIContext](./js-apis-arkui-UIContext.md) | 是   | 触发页面切换的Navigation对应的上下文信息。 | 支持        | 支持    |
| from      | [NavDestinationInfo](#navdestinationinfo) \| NavBar          | 是   | 页面切换的源页面。                         | 支持        | 支持    |
| to        | [NavDestinationInfo](#navdestinationinfo) \| NavBar          | 是   | 页面切换的目的页面。                       | 支持        | 支持    |
| operation | NavigationOperation                                          | 是   | 页面切换操作类型。                         | 支持        | 支持    |

## NavDestinationSwitchObserverOptions<sup>12+</sup>

Navigation组件页面切换事件的监听选项。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称         | 类型        | 必填 | 说明                         | Android平台 | iOS平台 |
| ------------ | ----------- | ---- | ---------------------------- | ----------- | ------- |
| navigationId | ResourceStr | 是   | 指定需要监听的Navigation的ID | 支持        | 支持    |

## TabContentInfo<sup>12+</sup>

TabContent页面的切换信息。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

| 名称         | 类型                                               | 必填 | 说明                                          | Android平台                                 | iOS平台                                     |
| ------------ | -------------------------------------------------- | ---- | -------------------------------------------- | -------------------------------------------- | -------------------------------------------- |
| tabContentId | string                                             | 是   | TabContent组件的id。                          | 支持                        | 支持                        |
| tabContentUniqueId | number                                       | 是   | TabContent组件的uniqueId。                    | 支持                  | 支持                  |
| state        | [TabContentState](#tabcontentstate12)              | 是   | TabContent组件的状态。                        | 支持                      | 支持                      |
| index        | number                                             | 是   | TabContent组件的下标索引。索引从0开始。        | 支持      | 支持      |
| id           | string                                             | 是   | Tabs组件的id。                                | 支持                              | 支持                              |
| uniqueId     | number                                             | 是   | Tabs组件的uniqueId。                          | 支持                        | 支持                        |

## uiObserver.on('navDestinationUpdate')

on(type: 'navDestinationUpdate', callback: Callback\<NavDestinationInfo\>): void

监听NavDestination组件的状态变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                  | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                                | 是   | 监听事件，固定为'navDestinationUpdate'，即NavDestination组件的状态变化。 |
| callback | Callback\<[NavDestinationInfo](#navdestinationinfo)\> | 是   | 回调函数。返回当前的NavDestination组件状态。                 |

**示例：**

```ts
// Index.ets
// 演示 uiObserver.on('navDestinationUpdate', callback)
// uiObserver.off('navDestinationUpdate', callback)
import { uiObserver } from '@kit.ArkUI';

@Component
struct PageOne {
  build() {
    NavDestination() {
      Text("pageOne")
    }.title("pageOne")
  }
}

@Entry
@Component
struct Index {
  private stack: NavPathStack = new NavPathStack();

  @Builder
  PageBuilder(name: string) {
    PageOne()
  }

  aboutToAppear() {
    uiObserver.on('navDestinationUpdate', (info) => {
      console.info('NavDestination state update', JSON.stringify(info));
    });
  }

  aboutToDisappear() {
    uiObserver.off('navDestinationUpdate');
  }

  build() {
    Column() {
      Navigation(this.stack) {
        Button("push").onClick(() => {
          this.stack.pushPath({ name: "pageOne" });
        })
      }
      .title("Navigation")
      .navDestination(this.PageBuilder)
    }
    .width('100%')
    .height('100%')
  }
}
```

## uiObserver.off('navDestinationUpdate')

off(type: 'navDestinationUpdate', callback?: Callback\<NavDestinationInfo\>): void

取消监听NavDestination组件的状态变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                  | 必填 | 说明                                                                     |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------------------ |
| type     | string                                                | 是   | 监听事件，固定为'navDestinationUpdate'，即NavDestination组件的状态变化。 |
| callback | Callback\<[NavDestinationInfo](#navdestinationinfo)\> | 否   | 回调函数。返回当前的NavDestination组件状态。                             |

**示例：**

参考[uiObserver.on('navDestinationUpdate')](#uiobserveronnavdestinationupdate)示例。

## uiObserver.on('navDestinationUpdate')

on(type: 'navDestinationUpdate', options: { navigationId: ResourceStr }, callback: Callback\<NavDestinationInfo\>): void

监听NavDestination组件的状态变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                  | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                                | 是   | 监听事件，固定为'navDestinationUpdate'，即NavDestination组件的状态变化。 |
| options  | { navigationId: ResourceStr }                         | 是   | 指定监听的Navigation的id。                                   |
| callback | Callback\<[NavDestinationInfo](#navdestinationinfo)\> | 是   | 回调函数。返回当前的NavDestination组件状态。                 |

**示例：**

```ts
// Index.ets
// 演示 uiObserver.on('navDestinationUpdate', navigationId, callback)
// uiObserver.off('navDestinationUpdate', navigationId, callback)
import { uiObserver } from '@kit.ArkUI';

@Component
struct PageOne {
  build() {
    NavDestination() {
      Text("pageOne")
    }.title("pageOne")
  }
}

@Entry
@Component
struct Index {
  private stack: NavPathStack = new NavPathStack();

  @Builder
  PageBuilder(name: string) {
    PageOne()
  }

  aboutToAppear() {
    uiObserver.on('navDestinationUpdate', { navigationId: "testId" }, (info) => {
      console.info('NavDestination state update', JSON.stringify(info));
    });
  }

  aboutToDisappear() {
    uiObserver.off('navDestinationUpdate', { navigationId: "testId" });
  }

  build() {
    Column() {
      Navigation(this.stack) {
        Button("push").onClick(() => {
          this.stack.pushPath({ name: "pageOne" });
        })
      }
      .id("testId")
      .title("Navigation")
      .navDestination(this.PageBuilder)
    }
    .width('100%')
    .height('100%')
  }
}
```

## uiObserver.off('navDestinationUpdate')

off(type: 'navDestinationUpdate', options: { navigationId: ResourceStr }, callback?: Callback\<NavDestinationInfo\>): void

取消监听NavDestination组件的状态变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                  | 必填 | 说明                                                         |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------ |
| type     | string                                                | 是   | 监听事件，固定为'navDestinationUpdate'，即NavDestination组件的状态变化。 |
| options  | { navigationId: ResourceStr }                         | 是   | 指定监听的Navigation的id。                                   |
| callback | Callback\<[NavDestinationInfo](#navdestinationinfo)\> | 否   | 回调函数。返回当前的NavDestination组件状态。                 |

**示例：**

参考[uiObserver.on('navDestinationUpdate')](#uiobserveronnavdestinationupdate-1)示例。

## uiObserver.on('scrollEvent')<sup>12+</sup>

on(type: 'scrollEvent', callback: Callback\<ScrollEventInfo\>): void

监听滚动事件的开始和结束。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                  | 必填 | 说明                                                                     |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------------------ |
| type     | string                                                | 是   | 监听事件，固定为'scrollEvent'，即滚动事件的开始和结束。                   |
| callback | Callback\<[ScrollEventInfo](#scrolleventinfo12)\>       | 是   | 回调函数。返回滚动事件的信息。                                           |

## uiObserver.off('scrollEvent')<sup>12+</sup>

off(type: 'scrollEvent', callback?: Callback\<ScrollEventInfo\>): void

取消监听滚动事件的开始和结束。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                  | 必填 | 说明                                                                     |
| -------- | ----------------------------------------------------- | ---- | ------------------------------------------------------------------------ |
| type     | string                                                | 是   | 监听事件，固定为'scrollEvent'，即滚动事件的开始和结束。                   |
| callback | Callback\<[ScrollEventInfo](#scrolleventinfo12)\>       | 否   | 回调函数。返回滚动事件的信息。                                           |

## uiObserver.on('scrollEvent')<sup>12+</sup>

on(type: 'scrollEvent', options: ObserverOptions, callback: Callback\<ScrollEventInfo\>): void

监听滚动事件的开始和结束。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                                 | 必填 | 说明                                                                     |
| -------- | -------------------------------------------------------------------- | ---- | ------------------------------------------------------------------------ |
| type     | string                                                               | 是   | 监听事件，固定为'scrollEvent'，即滚动事件的开始和结束。                   |
| options  | [ObserverOptions](#observeroptions12)                                  | 是   | 指定监听的滚动组件的id。                                                 |
| callback | Callback\<[ScrollEventInfo](#scrolleventinfo12)\>                      | 是   | 回调函数。返回滚动事件的信息。                                            |

## uiObserver.off('scrollEvent')<sup>12+</sup>

off(type: 'scrollEvent', options: ObserverOptions, callback?: Callback\<ScrollEventInfo\>): void

取消监听滚动事件的开始和结束。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                                 | 必填 | 说明                                                                     |
| -------- | -------------------------------------------------------------------- | ---- | ------------------------------------------------------------------------ |
| type     | string                                                               | 是   | 监听事件，固定为'scrollEvent'，即滚动事件的开始和结束。                   |
| options  | [ObserverOptions](#observeroptions12)                                  | 是   | 指定监听的滚动组件的id。                                                 |
| callback | Callback\<[ScrollEventInfo](#scrolleventinfo12)\>                      | 否   | 回调函数。返回滚动事件的信息。                                            |

**示例：**

```ts
import { uiObserver } from '@kit.ArkUI'

@Entry
@Component
struct Index {
  scroller: Scroller = new Scroller();
  options: uiObserver.ObserverOptions = { id: "testId" };
  private arr: number[] = [0, 1, 2, 3, 4, 5, 6, 7]

  build() {
    Column() {
      Column() {
        Scroll(this.scroller) {
          Column() {
            ForEach(this.arr, (item: number) => {
              Text(item.toString())
                .width('90%')
                .height(150)
                .backgroundColor(0xFFFFFF)
                .borderRadius(15)
                .fontSize(16)
                .textAlign(TextAlign.Center)
                .margin({ top: 10 })
            }, (item: string) => item)
          }.width('100%')
        }
        .id("testId")
        .height('80%')
      }
      .width('100%')

      Row() {
        Button('UIObserver on')
          .onClick(() => {
            uiObserver.on('scrollEvent', (info) => {
              console.info('scrollEventInfo', JSON.stringify(info));
            });
          })
        Button('UIObserver off')
          .onClick(() => {
            uiObserver.off('scrollEvent');
          })
      }

      Row() {
        Button('UIObserverWithId on')
          .onClick(() => {
            uiObserver.on('scrollEvent', this.options, (info) => {
              console.info('scrollEventInfo', JSON.stringify(info));
            });
          })
        Button('UIObserverWithId off')
          .onClick(() => {
            uiObserver.off('scrollEvent', this.options);
          })
      }
    }
    .height('100%')
  }
}
```

## uiObserver.on('routerPageUpdate')<sup>11+</sup>

on(type: 'routerPageUpdate', context: UIAbilityContext | UIContext, callback: Callback\<RouterPageInfo\>): void

监听router中page页面的状态变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'routerPageUpdate'，即router中page页面的状态变化。 |
| context  | UIAbilityContext \|&nbsp;[UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。                         |
| callback | Callback\<[RouterPageInfo](#routerpageinfo)\>                | 是   | 回调函数。携带pageInfo，返回当前的page页面状态。             |

**示例：**

```ts
// used in UIAbility
import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { UIContext, window, uiObserver } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

export default class EntryAbility extends UIAbility {
  private uiContext: UIContext | null = null;

  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    // 注册监听，范围是abilityContext内的page
    uiObserver.on('routerPageUpdate', this.context, (info: uiObserver.RouterPageInfo) => {
      console.info('[uiObserver][abilityContext] got info: ' + JSON.stringify(info))
    })
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    windowStage.loadContent('pages/Index', (err) => {
      windowStage.getMainWindow((err: BusinessError, data) => {
        let windowInfo: window.Window = data;
        // 获取UIContext实例
        this.uiContext = windowInfo.getUIContext();
        // 注册监听，范围是uiContext内的page
        uiObserver.on('routerPageUpdate', this.uiContext, (info: uiObserver.RouterPageInfo)=>{
          console.info('[uiObserver][uiContext] got info: ' + JSON.stringify(info))
        })
      })
    });
  }

  // ... other function in EntryAbility
}
```

## uiObserver.off('routerPageUpdate')<sup>11+</sup>

off(type: 'routerPageUpdate', context: UIAbilityContext | UIContext, callback?: Callback\<RouterPageInfo\>): void

取消监听router中page页面的状态变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'routerPageUpdate'，即router中page页面的状态变化。 |
| context  | UIAbilityContext&nbsp;\|&nbsp;[UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。                         |
| callback | Callback\<[RouterPageInfo](#routerpageinfo)\>                | 否   | 需要被注销的回调函数。                                       |

**示例：**

```ts
// used in UIAbility
import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { uiObserver, UIContext } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  // 实际使用前uiContext需要被赋值。参见示例uiObserver.on('routerPageUpdate')
  private uiContext: UIContext | null = null;

  onDestroy(): void {
    // 注销当前abilityContext上的所有routerPageUpdate监听
    uiObserver.off('routerPageUpdate', this.context)
  }

  onWindowStageDestroy(): void {
    // 注销在uiContext上的所有routerPageUpdate监听
    if (this.uiContext) {
      uiObserver.off('routerPageUpdate', this.uiContext);
    }
  }

  // ... other function in EntryAbility
}
```

## uiObserver.on('densityUpdate')<sup>12+</sup>

on(type: 'densityUpdate', context: UIContext, callback: Callback\<DensityInfo\>): void

监听屏幕像素密度变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台**： Android

**注意**： 只有部分Android机器支持。

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'densityUpdate'，即屏幕像素密度变化。 |
| context  | [UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。 |
| callback | Callback\<[DensityInfo](#densityinfo12)\>        | 是   | 回调函数。携带densityInfo，返回变化后的屏幕像素密度。                 |

**示例：**

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State density: number = 0;
  @State message: string = '未注册监听'

  densityUpdateCallback = (info: uiObserver.DensityInfo) => {
    this.density = info.density;
    this.message = '变化后的DPI：' + this.density.toString();
  }

  build() {
    Column() {
      Text(this.message)
        .fontSize(24)
        .fontWeight(FontWeight.Bold)
      Button('注册屏幕像素密度变化监听')
        .onClick(() => {
          this.message = '已注册监听'
          uiObserver.on('densityUpdate', this.getUIContext(), this.densityUpdateCallback);
        })
    }
  }
}
```

## uiObserver.off('densityUpdate')<sup>12+</sup>

off(type: 'densityUpdate', context: UIContext, callback?: Callback\<DensityInfo\>): void

取消监听屏幕像素密度的变化。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台**： Android

**注意**： 只有部分Android机器支持。

**参数：** 

| 参数名   | 类型                                      | 必填 | 说明                                                                                           |
| -------- | ----------------------------------------- | ---- | ---------------------------------------------------------------------------------------------- |
| type     | string                                    | 是   | 监听事件，固定为'densityUpdate'，即屏幕像素密度变化。                                          |
| context  | [UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。                                                             |
| callback | Callback\<[DensityInfo](#densityinfo12)\> | 否   | 需要被注销的回调函数。若不指定具体的回调函数，则注销指定UIContext下所有densityUpdate事件监听。 |

```ts
import { uiObserver, UIContext } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  @State density: number = 0;
  @State message: string = '未注册监听'

  densityUpdateCallback = (info: uiObserver.DensityInfo) => {
    this.density = info.density;
    this.message = '变化后的DPI：' + this.density.toString();
  }

  build() {
    Column() {
      Text(this.message)
        .fontSize(24)
        .fontWeight(FontWeight.Bold)
      Button('注册屏幕像素密度变化监听')
        .margin({ bottom: 10 })
        .onClick(() => {
          this.message = '已注册监听'
          uiObserver.on('densityUpdate', this.getUIContext(), this.densityUpdateCallback);
        })
      Button('解除注册屏幕像素密度变化监听')
        .onClick(() => {
          this.message = '未注册监听'
          uiObserver.off('densityUpdate', this.getUIContext(), this.densityUpdateCallback);
        })
    }
  }
}
```

## uiObserver.on('willDraw')<sup>12+</sup>

on(type: 'willDraw', context: UIContext, callback: Callback\<void\>): void

监听每一帧绘制指令下发情况。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'willDraw'，即是否将要绘制。 |
| context  | [UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。 |
| callback | Callback\<void\>        | 是   | 回调函数。                 |

**示例：**

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  willDrawCallback = () => {
    console.info("willDraw指令下发");
  }
  build() {
    Column() {
      Button('注册绘制指令下发监听')
        .onClick(() => {
          uiObserver.on('willDraw', this.getUIContext(), this.willDrawCallback);
        })
    }
  }
}
```

## uiObserver.off('willDraw')<sup>12+</sup>

off(type: 'willDraw', context: UIContext, callback?: Callback\<void\>): void

取消监听每一帧绘制指令下发情况。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                      | 必填 | 说明                                                  |
| -------- | ----------------------------------------- | ---- | ----------------------------------------------------- |
| type     | string                                    | 是   | 监听事件，固定为'willDraw'，即是否将要绘制。 |
| context  | [UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。                    |
| callback | Callback\<void\>   | 否   | 需要被注销的回调函数。                                |

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  willDrawCallback = () => {
    console.info("willDraw指令下发")
  }

  build() {
    Column() {
      Button('注册绘制指令下发监听')
        .margin({ bottom: 10 })
        .onClick(() => {
          uiObserver.on('willDraw', this.getUIContext(), this.willDrawCallback);
        })
      Button('解除注册绘制指令下发监听')
        .onClick(() => {
          uiObserver.off('willDraw', this.getUIContext(), this.willDrawCallback);
        })
    }
  }
}
```

## uiObserver.on('didLayout')<sup>12+</sup>

on(type: 'didLayout', context: UIContext, callback: Callback\<void\>): void

监听每一帧布局完成情况。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'didLayout'，即是否布局完成。 |
| context  | [UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。 |
| callback | Callback\<void\>        | 是   | 回调函数。                 |

**示例：**

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  didLayoutCallback = () => {
    console.info("Layout布局完成");
  }
  build() {
    Column() {
      Button('注册布局完成监听')
        .onClick(() => {
          uiObserver.on('didLayout', this.getUIContext(), this.didLayoutCallback);
        })
    }
  }
}
```

## uiObserver.off('didLayout')<sup>12+</sup>

off(type: 'didLayout', context: UIContext, callback?: Callback\<void\>): void

取消监听每一帧布局完成情况。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                      | 必填 | 说明                                                  |
| -------- | ----------------------------------------- | ---- | ----------------------------------------------------- |
| type     | string                                    | 是   | 监听事件，固定为'didLayout'，即是否布局完成。 |
| context  | [UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面的范围。                    |
| callback | Callback\<void\>   | 否   | 需要被注销的回调函数。                                |

```ts
import { uiObserver } from '@kit.ArkUI';

@Entry
@Component
struct Index {
  didLayoutCallback = () => {
    console.info("Layout布局完成")
  }

  build() {
    Column() {
      Button('注册布局完成监听')
        .margin({ bottom: 10 })
        .onClick(() => {
          uiObserver.on('didLayout', this.getUIContext(), this.didLayoutCallback);
        })
      Button('解除布局完成s监听')
        .onClick(() => {
          uiObserver.off('didLayout', this.getUIContext(), this.didLayoutCallback);
        })
    }
  }
}
```

## uiObserver.on('navDestinationSwitch')<sup>12+</sup>

on(type: 'navDestinationSwitch', context: UIAbilityContext | UIContext, callback: Callback\<NavDestinationSwitchInfo\>): void

监听Navigation的页面切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'navDestinationSwitch'，即Navigation的页面切换事件。 |
| context  | UIAbilityContext&nbsp;\|&nbsp;[UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面切换事件的范围。                 |
| callback | Callback\<[NavDestinationSwitchInfo](#navdestinationswitchinfo12)\> | 是   | 回调函数。携带NavDestinationSwitchInfo，返回页面切换事件的信息。 |

**示例：**

```ts
// EntryAbility.ets
// 演示 uiObserver.on('navDestinationSwitch', UIAbilityContext, callback)
// uiObserver.off('navDestinationSwitch', UIAbilityContext, callback)
import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { uiObserver, window } from '@kit.ArkUI';
import { hilog } from "@kit.PerformanceAnalysisKit"

function callBackFunc(info: uiObserver.NavDestinationSwitchInfo) {
  console.info(`testTag navDestinationSwitch from: ${JSON.stringify(info.from)} to: ${JSON.stringify(info.to)}`)
}

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
    uiObserver.on('navDestinationSwitch', this.context, callBackFunc);
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
    uiObserver.off('navDestinationSwitch', this.context, callBackFunc);
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err, data) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}
```

```ts
// Index.ets
// 演示 uiObserver.on('navDestinationSwitch', UIContext, callback)
// uiObserver.off('navDestinationSwitch', UIContext, callback)
import { uiObserver } from '@kit.ArkUI';

@Component
struct PageOne {
  build() {
    NavDestination() {
      Text("pageOne")
    }.title("pageOne")
  }
}

function callBackFunc(info: uiObserver.NavDestinationSwitchInfo) {
  console.info(`testTag navDestinationSwitch from: ${JSON.stringify(info.from)} to: ${JSON.stringify(info.to)}`)
}

@Entry
@Component
struct Index {
  private stack: NavPathStack = new NavPathStack();

  @Builder
  PageBuilder(name: string) {
    PageOne()
  }

  aboutToAppear() {
    uiObserver.on('navDestinationSwitch', this.getUIContext(), callBackFunc)
  }

  aboutToDisappear() {
    uiObserver.off('navDestinationSwitch', this.getUIContext(), callBackFunc)
  }

  build() {
    Column() {
      Navigation(this.stack) {
        Button("push").onClick(() => {
          this.stack.pushPath({ name: "pageOne" });
        })
      }
      .title("Navigation")
      .navDestination(this.PageBuilder)
    }
    .width('100%')
    .height('100%')
  }
}
```

## uiObserver.off('navDestinationSwitch')<sup>12+</sup>

off(type: 'navDestinationSwitch', context: UIAbilityContext | UIContext, callback?: Callback\<NavDestinationSwitchInfo\>): void

取消监听Navigation的页面切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'navDestinationSwitch'，即Navigation的页面切换事件。 |
| context  | UIAbilityContext&nbsp;\|&nbsp;[UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面切换事件的范围。                 |
| callback | Callback\<[NavDestinationSwitchInfo](#navdestinationswitchinfo12)\> | 否   | 需要被注销的回调函数。                                       |

**示例：**

参考[uiObserver.on('navDestinationSwitch')](#uiobserveronnavdestinationswitch12)示例。

## uiObserver.on('navDestinationSwitch')<sup>12+</sup>

on(type: 'navDestinationSwitch', context: UIAbilityContext | UIContext, observerOptions: NavDestinationSwitchObserverOptions, callback: Callback\<NavDestinationSwitchInfo\>): void

监听Navigation的页面切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'navDestinationSwitch'，即Navigation的页面切换事件。 |
| context  | UIAbilityContext&nbsp;\|&nbsp;[UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面切换事件的范围。 |
| observerOptions | [NavDestinationSwitchObserverOptions](#navdestinationswitchobserveroptions12)        | 是   | 监听选项。   |
| callback | Callback\<[NavDestinationSwitchInfo](#navdestinationswitchinfo12)\>        | 是   | 回调函数。携带NavDestinationSwitchInfo，返回页面切换事件的信息。                 |

**示例：**

```ts
// EntryAbility.ets
// 演示 uiObserver.on('navDestinationSwitch', UIAbilityContext, NavDestinationSwitchObserverOptions, callback)
// uiObserver.off('navDestinationSwitch', UIAbilityContext, NavDestinationSwitchObserverOptions, callback)
import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
import { uiObserver, window } from '@kit.ArkUI';
import { hilog } from "@kit.PerformanceAnalysisKit"

function callBackFunc(info: uiObserver.NavDestinationSwitchInfo) {
  console.info(`testTag navDestinationSwitch from: ${JSON.stringify(info.from)} to: ${JSON.stringify(info.to)}`)
}

export default class EntryAbility extends UIAbility {
  onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onCreate');
    uiObserver.on('navDestinationSwitch', this.context, {
      navigationId: "myNavId"
    }, callBackFunc);
  }

  onDestroy(): void {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onDestroy');
    uiObserver.off('navDestinationSwitch', this.context, {
      navigationId: "myNavId"
    }, callBackFunc);
  }

  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err, data) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
    });
  }

  onWindowStageDestroy(): void {
    // Main window is destroyed, release UI related resources
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageDestroy');
  }

  onForeground(): void {
    // Ability has brought to foreground
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onForeground');
  }

  onBackground(): void {
    // Ability has back to background
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onBackground');
  }
}
```

```ts
// Index.ets
// 演示 uiObserver.on('navDestinationSwitch', UIContext, NavDestinationSwitchObserverOptions, callback)
// uiObserver.off('navDestinationSwitch', UIContext, NavDestinationSwitchObserverOptions, callback)
import { uiObserver } from '@kit.ArkUI';

@Component
struct PageOne {
  build() {
    NavDestination() {
      Text("pageOne")
    }.title("pageOne")
  }
}

function callBackFunc(info: uiObserver.NavDestinationSwitchInfo) {
  console.info(`testTag navDestinationSwitch from: ${JSON.stringify(info.from)} to: ${JSON.stringify(info.to)}`)
}

@Entry
@Component
struct Index {
  private stack: NavPathStack = new NavPathStack();

  @Builder
  PageBuilder(name: string) {
    PageOne()
  }

  aboutToAppear() {
    uiObserver.on('navDestinationSwitch', this.getUIContext(), { navigationId: "myNavId" }, callBackFunc)
  }

  aboutToDisappear() {
    uiObserver.off('navDestinationSwitch', this.getUIContext(), { navigationId: "myNavId" }, callBackFunc)
  }

  build() {
    Column() {
      Navigation(this.stack) {
        Button("push").onClick(() => {
          this.stack.pushPath({ name: "pageOne" });
        })
      }
      .id("myNavId")
      .title("Navigation")
      .navDestination(this.PageBuilder)
    }
    .width('100%')
    .height('100%')
  }
}
```

## uiObserver.off('navDestinationSwitch')<sup>12+</sup>

off(type: 'navDestinationSwitch', context: UIAbilityContext | UIContext, observerOptions: NavDestinationSwitchObserverOptions, callback?: Callback\<NavDestinationSwitchInfo\>): void

取消监听Navigation的页面切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'navDestinationSwitch'，即Navigation的页面切换事件。 |
| context  | UIAbilityContext&nbsp;\|&nbsp;[UIContext](./js-apis-arkui-UIContext.md) | 是   | 上下文信息，用以指定监听页面切换事件的范围。 |
| observerOptions | [NavDestinationSwitchObserverOptions](#navdestinationswitchobserveroptions12)        | 是   | 监听选项。   |
| callback | Callback\<[NavDestinationSwitchInfo](#navdestinationswitchinfo12)\>        | 否   | 需要被注销的回调函数。                 |

**示例：**

参考[uiObserver.on('navDestinationSwitch')](#uiobserveronnavdestinationswitch12-1)接口示例。

## uiObserver.on('tabContentUpdate')<sup>12+</sup>

on(type: 'tabContentUpdate', callback: Callback\<TabContentInfo\>): void

监听TabContent页面的切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'tabContentUpdate'，即TabContent页面的切换事件。 |
| callback | Callback\<[TabContentInfo](#tabcontentinfo12)\>              | 是   | 回调函数。携带TabContentInfo，返回TabContent页面切换事件的信息。 |

**示例：**

```ts
import { uiObserver } from '@kit.ArkUI';

function callbackFunc(info: uiObserver.TabContentInfo) {
  console.info('tabContentUpdate', JSON.stringify(info));
}

@Entry
@Component
struct TabsExample {

  aboutToAppear(): void {
    uiObserver.on('tabContentUpdate', callbackFunc);
  }

  aboutToDisappear(): void {
    uiObserver.off('tabContentUpdate', callbackFunc);
  }

  build() {
    Column() {
      Tabs() {
        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#00CB87')
        }.tabBar('green').id('tabContentId0')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#007DFF')
        }.tabBar('blue').id('tabContentId1')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#FFBF00')
        }.tabBar('yellow').id('tabContentId2')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#E67C92')
        }.tabBar('pink').id('tabContentId3')
      }
      .width(360)
      .height(296)
      .backgroundColor('#F1F3F5')
      .id('tabsId')
    }.width('100%')
  }
}
```

## uiObserver.off('tabContentUpdate')<sup>12+</sup>

off(type: 'tabContentUpdate', callback?: Callback\<TabContentInfo\>): void

取消监听TabContent页面的切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'tabContentUpdate'，即TabContent页面的切换事件。 |
| callback | Callback\<[TabContentInfo](#tabcontentinfo12)\>              | 否   | 需要被注销的回调函数。 |

**示例：**

参考[uiObserver.on('tabContentUpdate')](#uiobserverontabcontentupdate12)接口示例。

## uiObserver.on('tabContentUpdate')<sup>12+</sup>

on(type: 'tabContentUpdate', options: ObserverOptions, callback: Callback\<TabContentInfo\>): void

监听TabContent页面的切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'tabContentUpdate'，即TabContent页面的切换事件。 |
| options  | [ObserverOptions](#observeroptions12)                        | 是   | 指定监听的Tabs组件的id。 |
| callback | Callback\<[TabContentInfo](#tabcontentinfo12)\>              | 是   | 回调函数。携带TabContentInfo，返回TabContent页面切换事件的信息。 |

**示例：**

```ts
import { uiObserver } from '@kit.ArkUI';

function callbackFunc(info: uiObserver.TabContentInfo) {
  console.info('tabContentUpdate', JSON.stringify(info));
}

@Entry
@Component
struct TabsExample {

  aboutToAppear(): void {
    uiObserver.on('tabContentUpdate', { id: 'tabsId' }, callbackFunc);
  }

  aboutToDisappear(): void {
    uiObserver.off('tabContentUpdate', { id: 'tabsId' }, callbackFunc);
  }

  build() {
    Column() {
      Tabs() {
        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#00CB87')
        }.tabBar('green').id('tabContentId0')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#007DFF')
        }.tabBar('blue').id('tabContentId1')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#FFBF00')
        }.tabBar('yellow').id('tabContentId2')

        TabContent() {
          Column().width('100%').height('100%').backgroundColor('#E67C92')
        }.tabBar('pink').id('tabContentId3')
      }
      .width(360)
      .height(296)
      .backgroundColor('#F1F3F5')
      .id('tabsId')
    }.width('100%')
  }
}
```

## uiObserver.off('tabContentUpdate')<sup>12+</sup>

off(type: 'tabContentUpdate', options: ObserverOptions, callback?: Callback\<TabContentInfo\>): void

取消监听TabContent页面的切换事件。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：** 

| 参数名   | 类型                                                         | 必填 | 说明                                                         |
| -------- | ------------------------------------------------------------ | ---- | ------------------------------------------------------------ |
| type     | string                                                       | 是   | 监听事件，固定为'tabContentUpdate'，即TabContent页面的切换事件。 |
| options  | [ObserverOptions](#observeroptions12)                        | 是   | 指定监听的Tabs组件的id。 |
| callback | Callback\<[TabContentInfo](#tabcontentinfo12)\>              | 否   | 需要被注销的回调函数。 |

**示例：**

参考[uiObserver.on('tabContentUpdate')](#uiobserverontabcontentupdate12-1)接口示例。