# @ohos.accessibility (辅助应用)

本模块提供Android和iOS平台辅助应用查询能力，包括获取辅助应用列表、获取辅助应用启用状态、获取无障碍字幕配置等。

> **说明：**
>
> - 本模块首批接口从 API version 22 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { accessibility } from '@kit.AccessibilityKit';
```

## AbilityState

type AbilityState = 'enable' | 'disable' | 'install'

辅助应用状态类型。

**系统能力：**以下各项对应的系统能力均为 SystemCapability.BarrierFree.Accessibility.Core

| 类型      | 说明       | Android 平台 | iOS 平台 |
| ------- | -------- | ------- | ------- |
| 'enable'  | 表示辅助应用已启用。 | 支持 | 不支持 |
| 'disable'  | 辅助应用已禁用。 | 支持 | 不支持 |
| 'install'  | 辅助应用已安装。 | 支持 | 不支持 |

## AbilityType

type AbilityType = 'audible' | 'generic' | 'haptic' | 'spoken' | 'visual' | 'all'

无障碍辅助应用类型。

**系统能力：**以下各项对应的系统能力均为 SystemCapability.BarrierFree.Accessibility.Core

| 类型      | 说明               | Android 平台 | iOS 平台 |
| --------- | ------------------ | ------------ | -------- |
| 'audible' | 表示具有听觉反馈。 | 支持         | 不支持   |
| 'generic' | 表示具有通用反馈。 | 支持         | 不支持   |
| 'haptic'  | 表示具有触觉反馈。 | 支持         | 不支持   |
| 'spoken'  | 表示具有语音反馈。 | 支持         | 不支持   |
| 'visual'  | 表示具有视觉反馈。 | 支持         | 不支持   |
| 'all'     | 表示以上所有类别。 | 支持         | 不支持   |

## AccessibilityAbilityInfo

辅助应用信息。

**系统能力：**以下各项对应的系统能力均为 SystemCapability.BarrierFree.Accessibility.Core

### 属性

| 名称              | 类型                                     | 只读 | 可选 | 说明                           | Android 平台 | iOS 平台 |
| ----------------- | ---------------------------------------- | ---- | ---- | ------------------------------ | ------------ | -------- |
| id                | string                                   | 是   | 否   | ability&nbsp;id。              | 支持         | 不支持   |
| name              | string                                   | 是   | 否   | ability 名。                   | 支持         | 不支持   |
| bundleName        | string                                   | 是   | 否   | Bundle名称。                   | 支持         | 不支持   |
| targetBundleNames | Array&lt;string&gt;                      | 是   | 否   | 关注的目标Bundle名称。         | 支持         | 不支持   |
| abilityTypes      | Array&lt;[AbilityType](#abilitytype)&gt; | 是   | 否   | 辅助应用类型。                 | 支持         | 不支持   |
| capabilities      | Array&lt;[Capability](#capability)&gt;   | 是   | 否   | 辅助应用能力列表。             | 支持         | 不支持   |
| description       | string                                   | 是   | 否   | 辅助应用描述。                 | 支持         | 不支持   |
| eventTypes        | Array&lt;[EventType](#eventtype)&gt;     | 是   | 否   | 辅助应用关注的无障碍事件列表。 | 支持         | 不支持   |

## Capability

type Capability = 'retrieve' | 'touchGuide' | 'keyEventObserver' | 'zoom' | 'gesture'

辅助应用能力类型。

**系统能力：**以下各项对应的系统能力均为 SystemCapability.BarrierFree.Accessibility.Core

| 类型               | 说明                                       | Android 平台 | iOS 平台 |
| ------------------ | ------------------------------------------ | ------------ | -------- |
| 'retrieve'         | 具有检索窗口内容的能力。                   | 支持         | 不支持   |
| 'touchGuide'       | 具有触摸探索模式的能力。                   | 支持         | 不支持   |
| 'keyEventObserver' | 具有过滤按键事件的能力。                   | 支持         | 不支持   |
| 'zoom'             | 具有控制显示放大的能力，当前版本暂不支持。 | 支持         | 不支持   |
| 'gesture'          | 具有执行手势动作的能力。                   | 支持         | 不支持   |

## EventInfo

界面变更事件。

**系统能力：**以下各项对应的系统能力均为 SystemCapability.BarrierFree.Accessibility.Core

### 属性

| 名称             | 类型                                   | 只读 | 可选 | 说明            | Android平台   | iOS平台       |
| ---------------- | ------------------------------------- |----- |------|-----------------------|-----------------------|-----------------------|
| type | [EventType](#eventtype)             | 否   | 否   | 无障碍事件类型，不可缺省。         | 支持       | 支持     |
| bundleName       | string                                | 否   | 否   | 目标应用名；不可缺省。**注：** 跨平台未使用，可填写空。 | 支持      | 支持       |
| triggerAction    | Action                     | 否   | 否   | 触发事件的Action，不可缺省。    | 支持 | 支持 |
| textAnnouncedForAccessibility    | string     | 否   | 是   | 主动播报的内容。当应用需要主动播报时，根据实际场景设置播报内容，无特殊限制。 | 支持 | 支持 |
| customId       | string                                | 否   | 是   | 主动聚焦的组件ID。        | 支持      | 支持      |

### constructor

constructor(jsonObject: Object)

构造函数。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| jsonObject | Object | 是   | 包含 type、bundleName 和 triggerAction 三个字段的 JSON对象，详见示例。 |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| 参数名     | 类型   | 必填 | 说明                                                         |

**示例：**

  ```ts
  import { accessibility } from '@kit.AccessibilityKit';

  let eventInfo: accessibility.EventInfo = ({
    type: 'click',
    bundleName: 'com.example.MyApplication',
    triggerAction: 'click',
  });
  ```

## EventType

type EventType = 'accessibilityFocus' | 'accessibilityFocusClear' |
'click' | 'longClick' | 'focus' | 'select' | 'hoverEnter' | 'hoverExit' |
'textUpdate' | 'textSelectionUpdate' | 'scroll' | 'requestFocusForAccessibility' |
'announceForAccessibility' | 'requestFocusForAccessibilityNotInterrupt' |
'announceForAccessibilityNotInterrupt' | 'scrolling'

无障碍事件类型。

**系统能力：**以下各项对应的系统能力均为 SystemCapability.BarrierFree.Accessibility.Core

| 类型                      | 说明                     | Android              | iOS                  |
| ----------------------- |------------------------|------------------------|------------------------|
| 'requestFocusForAccessibility'     | 表示主动聚焦的事件。 | 支持 | 支持 |
| 'announceForAccessibility'         | 表示主动播报的事件。 | 支持 | 支持 |

## accessibility.getAccessibilityExtensionList

getAccessibilityExtensionList(abilityType: AbilityType, stateType: AbilityState): Promise&lt;Array&lt;AccessibilityAbilityInfo&gt;&gt;

查询辅助应用列表，使用Promise异步回调。

**支持平台：**  Android

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名      | 类型                          | 必填 | 说明             |
| ----------- | ----------------------------- | ---- | ---------------- |
| abilityType | [AbilityType](#abilitytype)   | 是   | 辅助应用的类型。 |
| stateType   | [AbilityState](#abilitystate) | 是   | 辅助应用的状态。 |

**返回值：**

| 类型                                                         | 说明                                |
| ------------------------------------------------------------ | ----------------------------------- |
| Promise&lt;Array&lt;[AccessibilityAbilityInfo](#accessibilityabilityinfo)&gt;&gt; | Promise对象，返回辅助应用信息列表。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**参数示例：**
| 辅助应用类型 \ 辅助应用状态      | enable       | disable |install|
| ------- | -------- |----|----|
| **audible**  | 查询已启用的具有听觉反馈的辅助应用 |查询已禁用的具有听觉反馈的辅助应用|查询已安装的具有听觉反馈的辅助应用|
|**generic**| 查询已启用的具有通用反馈的辅助应用 |查询已禁用的具有通用反馈的辅助应用|查询已安装的具有通用反馈的辅助应用|
|**haptic**| 查询已启用的具有触觉反馈的辅助应用 |查询已禁用的具有触觉反馈的辅助应用|查询已安装的具有触觉反馈的辅助应用|
|**spoken**| 查询已启用的具有语音反馈的辅助应用 |查询已禁用的具有语音反馈的辅助应用|查询已安装的具有语音反馈的辅助应用|
|**visual**| 查询已启用的具有视觉反馈的辅助应用 |查询已禁用的具有视觉反馈的辅助应用|查询已安装的具有视觉反馈的辅助应用|
|**all**| 查询所有已启用的辅助应用 |查询所有已禁用的辅助应用|查询所有已安装的辅助应用|

**查询所有已安装的辅助应用示例：**
```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let abilityType: accessibility.AbilityType = 'all'; // 辅助应用类型为所有类型
let abilityState: accessibility.AbilityState = 'install'; // 辅助应用状态为已安装

accessibility.getAccessibilityExtensionList(abilityType, abilityState).then((data: accessibility.AccessibilityAbilityInfo[]) => {
  console.info(`Succeeded in get accessibility extension list, ${JSON.stringify(data)}`);
}).catch((err: BusinessError) => {
  console.error(`failed to get accessibility extension list, Code is ${err.code}, message is ${err.message}`);
});
```

**查询所有已启用的具有语音反馈的辅助应用示例：**
```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let abilityType: accessibility.AbilityType = 'spoken'; // 辅助应用类型为具有语音反馈类型
let abilityState: accessibility.AbilityState = 'enable'; // 辅助应用状态为已启用

accessibility.getAccessibilityExtensionList(abilityType, abilityState).then((data: accessibility.AccessibilityAbilityInfo[]) => {
  console.info(`Succeeded in get accessibility extension list, ${JSON.stringify(data)}`);
}).catch((err: BusinessError) => {
  console.error(`failed to get accessibility extension list, Code is ${err.code}, message is ${err.message}`);
});
```

## accessibility.getAccessibilityExtensionList

getAccessibilityExtensionList(abilityType: AbilityType, stateType: AbilityState, callback: AsyncCallback&lt;Array&lt;AccessibilityAbilityInfo&gt;&gt;): void

查询辅助应用列表，使用callback异步回调。

**支持平台：** Android

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名      | 类型                                                         | 必填 | 说明                             |
| ----------- | ------------------------------------------------------------ | ---- | -------------------------------- |
| abilityType | [AbilityType](#abilitytype)                                  | 是   | 辅助应用的类型。                 |
| stateType   | [AbilityState](#abilitystate)                                | 是   | 辅助应用的状态。                 |
| callback    | AsyncCallback&lt;Array&lt;[AccessibilityAbilityInfo](#accessibilityabilityinfo)&gt;&gt; | 是   | 回调函数，返回辅助应用信息列表。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**参数示例：**
| 辅助应用类型 \ 辅助应用状态      | enable       | disable |install|
| ------- | -------- |----|----|
| **audible**  | 查询已启用的具有听觉反馈的辅助应用 |查询已禁用的具有听觉反馈的辅助应用|查询已安装的具有听觉反馈的辅助应用|
|**generic**| 查询已启用的具有通用反馈的辅助应用 |查询已禁用的具有通用反馈的辅助应用|查询已安装的具有通用反馈的辅助应用|
|**haptic**| 查询已启用的具有触觉反馈的辅助应用 |查询已禁用的具有触觉反馈的辅助应用|查询已安装的具有触觉反馈的辅助应用|
|**spoken**| 查询已启用的具有语音反馈的辅助应用 |查询已禁用的具有语音反馈的辅助应用|查询已安装的具有语音反馈的辅助应用|
|**visual**| 查询已启用的具有视觉反馈的辅助应用 |查询已禁用的具有视觉反馈的辅助应用|查询已安装的具有视觉反馈的辅助应用|
|**all**| 查询所有已启用的辅助应用 |查询所有已禁用的辅助应用|查询所有已安装的辅助应用|

**查询所有已安装的辅助应用示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let abilityType: accessibility.AbilityType = 'all'; // 辅助应用类型为所有类型
let abilityState: accessibility.AbilityState = 'install'; // 辅助应用状态为已安装

accessibility.getAccessibilityExtensionList(abilityType, abilityState,(err: BusinessError, data: accessibility.AccessibilityAbilityInfo[]) => {
  if (err) {
    console.error(`failed to get accessibility extension list, Code is ${err.code}, message is ${err.message}`);
    return;
  }
  console.info(`Succeeded in get accessibility extension list, ${JSON.stringify(data)}`);
});
```

**查询所有已启用的具有语音反馈的辅助应用示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let abilityType: accessibility.AbilityType = 'spoken'; // 辅助应用类型为具有语音反馈类型
let abilityState: accessibility.AbilityState = 'enable'; // 辅助应用状态为已启用

accessibility.getAccessibilityExtensionList(abilityType, abilityState,(err: BusinessError, data: accessibility.AccessibilityAbilityInfo[]) => {
  if (err) {
    console.error(`failed to get accessibility extension list, Code is ${err.code}, message is ${err.message}`);
    return;
  }
  console.info(`Succeeded in get accessibility extension list, ${JSON.stringify(data)}`);
});
```

## accessibility.getAccessibilityExtensionListSync

getAccessibilityExtensionListSync(abilityType: AbilityType, stateType: AbilityState): Array&lt;AccessibilityAbilityInfo&gt;

查询当前系统内辅助应用列表，支持按条件查询。

**支持平台：** Android

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名      | 类型                          | 必填 | 说明             |
| ----------- | ----------------------------- | ---- | ---------------- |
| abilityType | [AbilityType](#abilitytype)   | 是   | 辅助应用的类型。 |
| stateType   | [AbilityState](#abilitystate) | 是   | 辅助应用的状态。 |

**返回值：**

| 类型                                       | 说明                    |
| ---------------------------------------- | --------------------- |
| Array&lt;[AccessibilityAbilityInfo](#accessibilityabilityinfo)&gt; | 返回辅助应用信息列表。 |

**参数示例：**

| 辅助应用类型 \ 辅助应用状态      | enable       | disable |install|
| ------- | -------- |----|----|
| **audible**  | 查询已启用的具有听觉反馈的辅助应用 |查询已禁用的具有听觉反馈的辅助应用|查询已安装的具有听觉反馈的辅助应用|
|**generic**| 查询已启用的具有通用反馈的辅助应用 |查询已禁用的具有通用反馈的辅助应用|查询已安装的具有通用反馈的辅助应用|
|**haptic**| 查询已启用的具有触觉反馈的辅助应用 |查询已禁用的具有触觉反馈的辅助应用|查询已安装的具有触觉反馈的辅助应用|
|**spoken**| 查询已启用的具有语音反馈的辅助应用 |查询已禁用的具有语音反馈的辅助应用|查询已安装的具有语音反馈的辅助应用|
|**visual**| 查询已启用的具有视觉反馈的辅助应用 |查询已禁用的具有视觉反馈的辅助应用|查询已安装的具有视觉反馈的辅助应用|
|**all**| 查询所有已启用的辅助应用 |查询所有已禁用的辅助应用|查询所有已安装的辅助应用|

**查询所有已安装的辅助应用示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let abilityType: accessibility.AbilityType = 'all'; // 辅助应用类型为所有类型
let abilityState: accessibility.AbilityState = 'install'; // 辅助应用状态为已安装
let data: accessibility.AccessibilityAbilityInfo[];

try {
  data = accessibility.getAccessibilityExtensionListSync(abilityType, abilityState);
  console.info(`Succeeded in get accessibility extension list, ${JSON.stringify(data)}`);
} catch (error) {
  let err = error as BusinessError;
  console.error(`error code: ${err.code}`);
}
```

**查询所有已启用的具有语音反馈的辅助应用示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let abilityType: accessibility.AbilityType = 'spoken'; // 辅助应用类型为具有语音反馈类型
let abilityState: accessibility.AbilityState = 'enable'; // 辅助应用状态为已启用
let data: accessibility.AccessibilityAbilityInfo[];

try {
  data = accessibility.getAccessibilityExtensionListSync(abilityType, abilityState);
  console.info(`Succeeded in get accessibility extension list, ${JSON.stringify(data)}`);
} catch (error) {
  let err = error as BusinessError;
  console.error(`error code: ${err.code}`);
}
```

## accessibility.on('accessibilityStateChange')

on(type: 'accessibilityStateChange', callback: Callback&lt;boolean&gt;): void

监听辅助应用启用状态变化事件，使用callback异步回调。如需获取系统内辅助应用信息，推荐使用[accessibility.getAccessibilityExtensionListSync](#accessibilitygetaccessibilityextensionlistsync)。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                         |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| type     | string                  | 是   | 监听的事件名，固定为‘accessibilityStateChange’，即辅助应用启用状态变化事件。 |
| callback | Callback&lt;boolean&gt; | 是   | 回调函数，在辅助应用启用状态变化时将状态通过此函数进行通知。此状态为全局辅助应用启用状态。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

// 系统内已安装一个或多个辅助应用时:
// 1. 启用辅助应用场景：第一个辅助应用启用后，回调函数会返回true
// 2. 禁用辅助应用场景：若一个或多个辅助应用已启用，最后一个已启用的辅助应用被禁用时，回调函数会返回false
accessibility.on('accessibilityStateChange', (data: boolean) => {
  console.info(`subscribe accessibility state change, result: ${JSON.stringify(data)}`);
});
```

## accessibility.on('touchGuideStateChange')

on(type: 'touchGuideStateChange', callback: Callback&lt;boolean&gt;): void

监听触摸浏览功能启用状态变化事件，使用callback异步回调。如需获取系统内辅助应用信息，推荐使用[accessibility.getAccessibilityExtensionListSync](#accessibilitygetaccessibilityextensionlistsync)。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Vision

**参数：**

| 参数名      | 类型                      | 必填   | 说明                                       |
| -------- | ----------------------- | ---- | ---------------------------------------- |
| type     | string                  | 是    | 监听的事件名，固定为‘touchGuideStateChange’，即触摸浏览启用状态变化事件。 |
| callback | Callback&lt;boolean&gt; | 是    | 回调函数，在触摸浏览启用状态变化时将状态通过此函数进行通知。           |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

// 系统内已安装一个或多个具备触摸浏览能力的辅助应用（Capability配置中含有'touchGuide'的辅助应用）时：
// 1. 启用触摸浏览辅助应用场景：第一个触摸浏览辅助应用启用后，回调函数会返回true
// 2. 禁用触摸浏览辅助应用场景：若一个或多个触摸浏览辅助应用已启用，最后一个已启用的触摸浏览辅助应用被禁用时，回调函数会返回false
accessibility.on('touchGuideStateChange', (data: boolean) => {
  console.info(`subscribe touch guide state change, result: ${JSON.stringify(data)}`);
});
```

## accessibility.on('screenReaderStateChange')

on(type: 'screenReaderStateChange', callback: Callback&lt;boolean&gt;): void

监听屏幕朗读功能启用状态变化事件，使用callback异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名      | 类型                      | 必填   | 说明                                       |
| -------- | ----------------------- | ---- | ---------------------------------------- |
| type     | string                  | 是    | 监听的事件名，固定为‘screenReaderStateChange’，即屏幕朗读启用状态变化事件。 |
| callback | Callback&lt;boolean&gt; | 是    | 回调函数，在屏幕朗读启用状态变化时将状态通过此函数进行通知。           |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

accessibility.on('screenReaderStateChange', (data: boolean) => {
  console.info(`subscribe screen reader state change, result: ${JSON.stringify(data)}`);
});
```

## accessibility.on('touchModeChange')

on(type: 'touchModeChange', callback: Callback&lt;string&gt;): void

监听触摸浏览功能下的单击（不支持）/双击操作模式变化事件，使用callback异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**注：**跨平台不支持单击模式

**参数：**

| 参数名      | 类型                      | 必填   | 说明                                       |
| -------- | ----------------------- | ---- | ---------------------------------------- |
| type     | string                  | 是    | 监听的事件名，固定为‘touchModeChange’，即触摸浏览功能下的单击（不支持）/双击操作模式变化事件。 |
| callback | Callback&lt;string&gt; | 是    | 回调函数，在触摸浏览功能下的单击（不支持）/双击操作模式变化时将状态通过此函数进行通知。      |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

@Entry
@Component
struct Index {
  callback: (mode: string) => void = this.eventCallback;
  eventCallback(mode: string): void {
    console.info(`current touch mode: ${JSON.stringify(mode)}`);
  }

  aboutToAppear(): void {
    accessibility.on('touchModeChange', this.callback);
  }

  build() {
    Column() {
    }
  }
}
```

## accessibility.off('accessibilityStateChange')

off(type: 'accessibilityStateChange', callback?: Callback&lt;boolean&gt;): void

取消监听辅助应用启用状态变化事件，使用callback异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                         |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| type     | string                  | 是   | 取消监听的事件名，固定为‘accessibilityStateChange’，即辅助应用启用状态变化事件。 |
| callback | Callback&lt;boolean&gt; | 否   | 回调函数，取消指定callback对象的事件响应。需与accessibility.on('accessibilityStateChange')的callback一致。缺省时，表示注销所有已注册事件。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

accessibility.off('accessibilityStateChange', (data: boolean) => {
  console.info(`Unsubscribe accessibility state change, result: ${JSON.stringify(data)}`);
});
```

## accessibility.off('touchGuideStateChange')

off(type: 'touchGuideStateChange', callback?: Callback&lt;boolean&gt;): void

取消监听触摸浏览启用状态变化事件，使用callback异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                         |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| type     | string                  | 是   | 取消监听的事件名，固定为‘touchGuideStateChange’，即触摸浏览启用状态变化事件。 |
| callback | Callback&lt;boolean&gt; | 否   | 回调函数，取消指定callback对象的事件响应。需与accessibility.on('touchGuideStateChange')的callback一致。缺省时，表示注销所有已注册事件。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

accessibility.off('touchGuideStateChange', (data: boolean) => {
  console.info(`Unsubscribe touch guide state change, result: ${JSON.stringify(data)}`);
});
```

## accessibility.off('screenReaderStateChange')

off(type: 'screenReaderStateChange', callback?: Callback&lt;boolean&gt;): void

取消监听屏幕朗读启用状态变化事件，使用callback异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                         |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| type     | string                  | 是   | 取消监听的事件名，固定为‘screenReaderStateChange’，即屏幕朗读启用状态变化事件。 |
| callback | Callback&lt;boolean&gt; | 否   | 回调函数，取消指定callback对象的事件响应。需与accessibility.on('screenReaderStateChange')的callback一致。缺省时，表示注销所有已注册事件。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

accessibility.off('screenReaderStateChange', (data: boolean) => {
  console.info(`Unsubscribe screen reader state change, result: ${JSON.stringify(data)}`);
});
```

## accessibility.off('touchModeChange')

off(type: 'touchModeChange', callback?: Callback&lt;string&gt;): void

取消监听触摸浏览功能下的单击/双击操作模式变化事件，使用callback异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名   | 类型                    | 必填 | 说明                                                         |
| -------- | ----------------------- | ---- | ------------------------------------------------------------ |
| type     | string                  | 是   | 取消监听的事件名，固定为‘touchModeChange’，即触摸浏览功能下的单击/双击操作模式变化事件。 |
| callback | Callback&lt;string&gt; | 否   | 回调函数，取消指定callback对象的事件响应。需与[accessibility.on('touchModeChange')](#accessibilityontouchmodechange)的callback一致。缺省时，表示注销所有已注册事件。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

@Entry
@Component
struct Index {
  callback: (mode: string) => void = this.eventCallback;
  eventCallback(mode: string): void {
    console.info(`current touch mode: ${JSON.stringify(mode)}`);
  }

  aboutToAppear(): void {
    accessibility.on('touchModeChange', this.callback);
  }

  aboutToDisappear(): void {
    accessibility.off('touchModeChange', this.callback);
  }

  build() {
    Column() {
    }
  }
}
```

## accessibility.isOpenAccessibilitySync

isOpenAccessibilitySync(): boolean

查询当前系统内是否存在已开启的辅助应用。如需获取系统内辅助应用信息，推荐使用[accessibility.getAccessibilityExtensionListSync](#accessibilitygetaccessibilityextensionlistsync)。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 表示当前系统内是否有辅助应用开启。true表示启用了一个或多个辅助应用，false表示未启用任何辅助应用。 |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 1、系统内已安装多个辅助应用，若都没有开启，返回false
// 2、系统内已安装多个辅助应用，若开启任意一个，返回true
let status: boolean = accessibility.isOpenAccessibilitySync();
```

## accessibility.isOpenTouchGuideSync

isOpenTouchGuideSync(): boolean

是否开启了触摸浏览模式。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Vision

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 表示是否开启了触摸浏览模式。true表示开启了触摸浏览，false表示未开启触摸浏览。 |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

let status: boolean = accessibility.isOpenTouchGuideSync();
```

## accessibility.isScreenReaderOpenSync

isScreenReaderOpenSync(): boolean

是否开启了屏幕朗读模式。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Vision

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 表示是否开启了屏幕朗读。true表示开启了屏幕朗读，false表示未开启屏幕朗读。 |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';

let status: boolean = accessibility.isScreenReaderOpenSync();
```

## accessibility.sendAccessibilityEvent

sendAccessibilityEvent(event: EventInfo): Promise&lt;void&gt;

发送无障碍事件，使用Promise异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名 | 类型                    | 必填 | 说明             |
| ------ | ----------------------- | ---- | ---------------- |
| event  | [EventInfo](#eventinfo) | 是   | 无障碍事件对象。 |

**返回值：**

| 类型                  | 说明               |
| ------------------- | ---------------- |
| Promise&lt;void&gt; | Promise对象，无返回结果。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let eventInfo: accessibility.EventInfo = ({
  type: 'click',
  bundleName: 'com.example.MyApplication',
  triggerAction: 'click',
});

accessibility.sendAccessibilityEvent(eventInfo).then(() => {
  console.info(`Succeeded in send event,eventInfo is ${eventInfo}`);
}).catch((err: BusinessError) => {
  console.error(`failed to send event , Code is ${err.code}, message is ${err.message}`);
});
```

## accessibility.sendAccessibilityEvent

sendAccessibilityEvent(event: EventInfo, callback: AsyncCallback&lt;void&gt;): void

发送无障碍事件，使用callback异步回调。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**参数：**

| 参数名   | 类型                      | 必填 | 说明                                                         |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| event    | [EventInfo](#eventinfo)   | 是   | 辅助事件对象。                                               |
| callback | AsyncCallback&lt;void&gt; | 是   | 回调函数，如果发送无障碍事件失败，则 AsyncCallback中err有数据返回。 |

**错误码：**

| 错误码ID | 错误信息 |
| ------- | -------------------------------- |
| 401  |Parameter error. Possible causes: 1. Mandatory parameters are left unspecified; 2. Incorrect parameter types; 3. Parameter verification failed. |

**主动聚焦示例：**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let eventInfo: accessibility.EventInfo = ({
  type: 'requestFocusForAccessibility',
  bundleName: 'com.example.MyApplication',
  triggerAction: 'common',
  customId: 'click' // 对应待聚焦组件id属性值
});

accessibility.sendAccessibilityEvent(eventInfo, (err: BusinessError) => {
  if (err) {
    console.error(`failed to send event, Code is ${err.code}, message is ${err.message}`);
    return;
  }
  console.info(`Succeeded in send event, eventInfo is ${eventInfo}`);
});
```
**主动播报示例**

```ts
import { accessibility } from '@kit.AccessibilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let eventInfo: accessibility.EventInfo = ({
  type: 'announceForAccessibility',
  bundleName: 'com.example.MyApplication',
  triggerAction: 'common',
  textResourceAnnouncedForAccessibility: '主动播报测试',
});

accessibility.sendAccessibilityEvent(eventInfo, (err: BusinessError) => {
  if (err) {
    console.error(`failed to send event, Code is ${err.code}, message is ${err.message}`);
    return;
  }
  console.info(`Succeeded in send event, eventInfo is ${eventInfo}`);
});
```

## accessibility.getTouchModeSync

getTouchModeSync(): string

查询触摸浏览功能下的单击（不支持）/双击操作模式。

**支持平台：** Android、iOS

**系统能力：**SystemCapability.BarrierFree.Accessibility.Core

**注：**跨平台不支持单击模式

**返回值：**

| 类型        | 说明                                  |
| ----------- | ------------------------------------- |
| string | 表示当前操作模式。<br/>- doubleTouchMode：表示双击操作模式。<br/>- none：表示未开启触摸浏览功能。 |

```ts
import { accessibility } from '@kit.AccessibilityKit';

@Entry
@Component
struct Index {
  aboutToAppear(): void {
    let touchMode: string = accessibility.getTouchModeSync();
    console.info(`current touch mode: ${JSON.stringify(touchMode)}`);
  }

  build() {
    Column() {
    }
  }
}
```

