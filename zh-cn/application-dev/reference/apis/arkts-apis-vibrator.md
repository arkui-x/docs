# @ohos.vibrator (振动)

vibrator模块提供控制设备马达振动的能力。包括启动指定时长、预置效果、自定义文件等模式的振动；停止指定时长、预置效果或所有模式的振动。

> **说明：**
> 
> 本模块首批接口从API version 22开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { vibrator } from '@kit.SensorServiceKit';
```

## vibrator.startVibration<sup>22+</sup>

startVibration(effect: VibrateEffect, attribute: VibrateAttribute, callback: AsyncCallback&lt;void&gt;): void

根据指定的振动效果和振动属性触发马达振动。使用callback异步回调。

**需要权限**：ohos.permission.VIBRATE

**原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名    | 类型                                      | 必填 | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| --------- |-----------------------------------------| ---- |:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| effect    | [VibrateEffect](#vibrateeffectsup22sup) | 是   | 马达振动效果，支持四种：1、[VibratePreset](#vibratepresetsup22sup)：按照预置振动效果触发马达振动，适用于交互反馈类的短振场景（如点击长按，滑动，拖拽等），为确保与系统整体振感反馈体验风格一致，推荐使用此接口；2、[VibrateFromFile](#vibratefromfilesup22sup)：按照文件形式定制自定义振动效果触发马达振动，适用于匹配复杂场景效果的交互反馈（如表情包触发的拟真效果、游戏场景/操作反馈）；3、[VibrateTime](#vibratetimesup22sup)：按照指定时长触发马达振动，仅对振动时长进行启动或停止控制，满足基础功能，无法对振动强度、频率等维度进行个性化设置，此种振动调节不够细腻，无法满足精致体验；4、[VibrateFromPattern22+](#vibratefrompatternsup22sup)：按照自定义振动效果触发马达振动。使用场景和VibrateFromFile一致。VibrateFromFile是面向文件中提前定制好的效果，将具体的振动事件以文件描述符形式传递到接口中；VibrateFromPattern提供更加灵活的振动事件排列组合，将振动事件以振动事件数组的形式传递到接口中。 |
| attribute | [VibrateAttribute](#vibrateattributesup22sup)   | 是   | 马达振动属性。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| callback  | AsyncCallback               | 是   | 回调函数。当马达振动成功，err为undefined；否则为错误对象，包含错误码和错误信息。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         |

**错误码**：

以下错误码的详细介绍请参见[振动错误码](../errorcodes/errorcode-vibrator.md)和[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 14600101 | Device operation failed.                                     |

**示例**：

1.按照预置振动效果触发马达振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 查询是否支持'haptic.notice.success'
  vibrator.isSupportEffect('haptic.notice.success', (err: BusinessError, state: boolean) => {
    if (err) {
      console.error(`Failed to query effect. Code: ${err.code}, message: ${err.message}`);
      return;
    }
    console.info('Succeed in querying effect');
    if (state) {
      try {
        vibrator.startVibration({
          type: 'preset',
          effectId: 'haptic.notice.success',
          count: 1,
        }, {
          usage: 'notification' // 根据实际选择类型归属不同的开关管控
        }, (error: BusinessError) => {
          if (error) {
            console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
 		   return;
          }
          console.info('Succeed in starting vibration');
       
        });
      } catch (err) {
        let e: BusinessError = err as BusinessError;
 	   console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
      }
    }
  })
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

2.按照自定义振动配置文件触发马达振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { resourceManager } from '@kit.LocalizationKit';
import { BusinessError } from '@kit.BasicServicesKit';

const fileName: string = 'xxx.json';

@Entry
@Component
struct Index {
  uiContext = this.getUIContext();

  build() {
    Row() {
      Column() {
        Button('alarm-file')
          .onClick(() => {
            let rawFd: resourceManager.RawFileDescriptor | undefined = this.uiContext.getHostContext()?.resourceManager.getRawFdSync(fileName);
            if (rawFd != undefined) {
              try {
                vibrator.startVibration({
                  type: "file",
                  hapticFd: { fd: rawFd.fd, offset: rawFd.offset, length: rawFd.length }
                }, {
                  id: 0,
                  usage: 'alarm' // 根据实际选择类型归属不同的开关管控
                }, (error: BusinessError) => {
                  if (error) {
                    console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
                    return;
                  }
                  console.info('Succeed in starting vibration');
                });
              } catch (err) {
                let e: BusinessError = err as BusinessError;
                console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
              } finally {
                vibrator.stopVibration();
                this.uiContext.getHostContext()?.resourceManager.closeRawFdSync(fileName);
              }
            }
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

3.按照指定时长触发马达振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

try {
  vibrator.startVibration({
    type: 'time',
    duration: 1000,
  }, {
    id: 0,
    usage: 'alarm' // 根据实际选择类型归属不同的开关管控
  }, (error: BusinessError) => {
    if (error) {
      console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
      return;
    }
    console.info('Succeed in starting vibration');
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.startVibration<sup>22+</sup>

startVibration(effect: VibrateEffect, attribute: VibrateAttribute): Promise&lt;void&gt;

根据指定的振动效果和振动属性触发马达振动。使用promise异步回调。

**需要权限**：ohos.permission.VIBRATE

**原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名    | 类型                                   | 必填 | 说明                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| --------- | ------------------------------------- | ---- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| effect    | [VibrateEffect](#vibrateeffectsup22sup)     | 是   | 马达振动效果，支持三种：1、[VibrateTime](#vibratetimesup22sup)：按照预置振动效果触发马达振动，适用于交互反馈类的短振场景（如点击长按，滑动，拖拽等），为确保与系统整体振感反馈体验风格一致，推荐使用此接口；2、[VibratePreset](#vibratepresetsup22sup)：按照文件形式定制自定义振动效果触发马达振动，适用于匹配复杂场景效果的交互反馈（如表情包触发的拟真效果、游戏场景/操作反馈）；3、[VibrateFromFile](#vibratefromfilesup22sup)：按照指定时长触发马达振动，仅对振动时长进行启动或停止控制，满足基础功能，无法对振动强度、频率等维度进行个性化设置，此种振动调节不够细腻，无法满足精致体验；4、[VibrateFromPattern22+](#vibratefrompatternsup22sup)：按照自定义振动效果触发马达振动。使用场景和VibrateFromFile一致。VibrateFromFile是面向文件中提前定制好的效果，将具体的振动事件以文件描述符形式传递到接口中；VibrateFromPattern提供更加灵活的振动事件排列组合，将振动事件以振动事件数组的形式传递到接口中。 |
| attribute | [VibrateAttribute](#vibrateattributesup22sup) | 是   | 马达振动属性。                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |

**返回值**：

| 类型                | 说明                      |
| ------------------- | ------------------------- |
| Promise | 无返回结果的Promise对象。 |

**错误码**：

以下错误码的详细介绍请参见[振动错误码](../errorcodes/errorcode-vibrator.md)和[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |
| 801      | Capability not supported.                                    |
| 14600101 | Device operation failed.                                     |

**示例**：

1.按照预置振动效果触发马达振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 查询是否支持'haptic.notice.success'
  vibrator.isSupportEffect('haptic.notice.success', (err: BusinessError, state: boolean) => {
    if (err) {
      console.error(`Failed to query effect. Code: ${err.code}, message: ${err.message}`);
      return;
    }
    console.info('Succeed in querying effect');
    if (state) {
      try {
        vibrator.startVibration({
          type: 'preset',
          effectId: 'haptic.notice.success',
          count: 1,
        }, {
          usage: 'notification' // 根据实际选择类型归属不同的开关管控
        }, (error: BusinessError) => {
          if (error) {
            console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
 		         return;
          }
          console.info('Succeed in starting vibration');
       
        });
      } catch (err) {
        let e: BusinessError = err as BusinessError;
 	       console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
      }
    }
  })
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

2.按照自定义振动配置文件触发马达振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { resourceManager } from '@kit.LocalizationKit';
import { BusinessError } from '@kit.BasicServicesKit';

const fileName: string = 'xxx.json';

@Entry
@Component
struct Index {
  uiContext = this.getUIContext();

  build() {
    Row() {
      Column() {
        Button('alarm-file')
          .onClick(() => {
            let rawFd: resourceManager.RawFileDescriptor | undefined = this.uiContext.getHostContext()?.resourceManager.getRawFdSync(fileName);
            if (rawFd != undefined) {
              try {
                vibrator.startVibration({
                  type: "file",
                  hapticFd: { fd: rawFd.fd, offset: rawFd.offset, length: rawFd.length }
                }, {
                  id: 0,
                  usage: 'alarm' // 根据实际选择类型归属不同的开关管控
                }, (error: BusinessError) => {
                  if (error) {
                    console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
                    return;
                  }
                  console.info('Succeed in starting vibration');
                });
              } catch (err) {
                let e: BusinessError = err as BusinessError;
                console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
              } finally {
                vibrator.stopVibration();
                this.uiContext.getHostContext()?.resourceManager.closeRawFdSync(fileName);
              }
            }
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

3.按照指定时长触发马达振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

try {
  vibrator.startVibration({
    type: 'time',
    duration: 1000
  }, {
    id: 0,
    usage: 'alarm' // 根据实际选择类型归属不同的开关管控
  }).then(() => {
    console.info('Succeed in starting vibration');
  }, (error: BusinessError) => {
    console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.stopVibration<sup>22+</sup>

stopVibration(stopMode: VibratorStopMode, callback: AsyncCallback&lt;void&gt;): void

按照指定模式停止马达振动。使用callback异步回调。

**需要权限**：ohos.permission.VIBRATE

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型                                            | 必填 | 说明                                                                                                                                                                             |
| -------- |-----------------------------------------------| ---- |--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| stopMode | [VibratorStopMode](#vibratorstopmodesup22sup) | 是   | 指定的停止振动模式，支持两种：VIBRATOR_STOP_MODE_TIME：停止固定时长振动；VIBRATOR_STOP_MODE_PRESET：停止预置振动。此接口无法停止自定义振动，请使用[vibrator.stopVibration22+](#vibratorstopvibrationsup22sup)。 |
| callback | AsyncCallback                     | 是   | 回调函数，当马达停止振动成功，err为undefined，否则为错误对象。                                                                                                                                          |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |

**示例**：

1.停止指定时长振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 按照指定时长振动
  vibrator.startVibration({
    type: 'time',
    duration: 1000,
  }, {
    id: 0,
    usage: 'alarm' // 根据实际选择类型归属不同的开关管控
  }, (error: BusinessError) => {
    if (error) {
      console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
      return;
    }
    console.info('Succeed in starting vibration');
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}

try {
  // 按照VIBRATOR_STOP_MODE_TIME模式停止振动
  vibrator.stopVibration(vibrator.VibratorStopMode.VIBRATOR_STOP_MODE_TIME, (error: BusinessError) => {
    if (error) {
      console.error(`Failed to stop vibration. Code: ${error.code}, message: ${error.message}`);
      return;
    }
    console.info('Succeed in stopping vibration');
  })
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

2.停止预置振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

try {
  // 按照预置效果振动
  vibrator.startVibration({
    type: 'preset',
    effectId: 'haptic.notice.success',
    count: 1,
  }, {
    id: 0,
    usage: 'notification' // 根据实际选择类型归属不同的开关管控
  }, (error: BusinessError) => {
    if (error) {
      console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
      return;
    }
    console.info('Succeed in starting vibration');
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}

try {
  // 按照VIBRATOR_STOP_MODE_PRESET模式停止振动
  vibrator.stopVibration(vibrator.VibratorStopMode.VIBRATOR_STOP_MODE_PRESET, (error: BusinessError) => {
    if (error) {
      console.error(`Failed to stop vibration. Code: ${error.code}, message: ${error.message}`);
      return;
    }
    console.info('Succeed in stopping vibration');
  })
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.stopVibration<sup>22+</sup>

stopVibration(stopMode: VibratorStopMode): Promise&lt;void&gt;

按照指定模式停止马达的振动。使用promise异步回调。

**需要权限**：ohos.permission.VIBRATE

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型                                  | 必填 | 说明                                                                                                                                                                            |
| -------- | ------------------------------------- | ---- |-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| stopMode | [VibratorStopMode](#vibratorstopmodesup22sup) | 是   | 支持停止两种指定的振动模式：VIBRATOR_STOP_MODE_TIME：停止指定时长振动；VIBRATOR_STOP_MODE_PRESET：停止预置振动。此接口无法停止自定义振动，请使用[vibrator.stopVibration22+](#vibratorstopvibrationsup22sup)。 |

**返回值**：

| 类型                | 说明          |
| ------------------- | ------------- |
| Promise | Promise对象。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |

**示例**：

1.停止指定时长振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 按照指定时长振动
  vibrator.startVibration({
    type: 'time',
    duration: 1000,
  }, {
    id: 0,
    usage: 'alarm' // 根据实际选择类型归属不同的开关管控
  }).then(() => {
    console.info('Succeed in starting vibration');
  }, (error: BusinessError) => {
    console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}

try {
  // 按照VIBRATOR_STOP_MODE_TIME模式停止振动
  vibrator.stopVibration(vibrator.VibratorStopMode.VIBRATOR_STOP_MODE_TIME).then(() => {
    console.info('Succeed in stopping vibration');
  }, (error: BusinessError) => {
    console.error(`Failed to stop vibration. Code: ${error.code}, message: ${error.message}`);
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

2.停止预置振动：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

try {
  // 按照预置效果振动
  vibrator.startVibration({
    type: 'preset',
    effectId: 'haptic.notice.success',
    count: 1,
  }, {
    id: 0,
    usage: 'notification' // 根据实际选择类型归属不同的开关管控
  }).then(() => {
    console.info('Succeed in starting vibration');
  }, (error: BusinessError) => {
    console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}

try {
  // 按照VIBRATOR_STOP_MODE_PRESET模式停止振动
  vibrator.stopVibration(vibrator.VibratorStopMode.VIBRATOR_STOP_MODE_PRESET).then(() => {
    console.info('Succeed in stopping vibration');
  }, (error: BusinessError) => {
    console.error(`Failed to stop vibration. Code: ${error.code}, message: ${error.message}`);
  });
} catch (err) {
  let e: BusinessError = err as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.stopVibration<sup>22+</sup>

stopVibration(callback: AsyncCallback&lt;void&gt;): void

停止所有模式的马达振动。使用callback异步回调。

**需要权限**：ohos.permission.VIBRATE

**原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型                      | 必填 | 说明                                                         |
| -------- | ------------------------- | ---- | ------------------------------------------------------------ |
| callback | AsyncCallback | 是   | 回调函数，当马达停止振动成功，err为undefined，否则为错误对象。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息           |
| -------- | ------------------ |
| 201      | Permission denied. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 停止所有模式的马达振动
  vibrator.stopVibration((error: BusinessError) => {
    if (error) {
      console.error(`Failed to stop vibration. Code: ${error.code}, message: ${error.message}`);
      return;
    }
    console.info('Succeed in stopping vibration');
  })
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.stopVibration<sup>22+</sup>

stopVibration(): Promise&lt;void&gt;

停止所有模式的马达振动。使用promise异步回调。

**需要权限**：ohos.permission.VIBRATE

**原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**返回值**：

| 类型                | 说明          |
| ------------------- | ------------- |
| Promise | Promise对象。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息           |
| -------- | ------------------ |
| 201      | Permission denied. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 停止所有模式的马达振动
  vibrator.stopVibration().then(() => {
    console.info('Succeed in stopping vibration');
  }, (error: BusinessError) => {
    console.error(`Failed to stop vibration. Code: ${error.code}, message: ${error.message}`);
  });
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.stopVibration<sup>22+</sup>

stopVibration(param?: VibratorInfoParam): Promise&lt;void&gt;

不传参默认停止本地设备马达的振动。使用promise异步回调。

**需要权限**：ohos.permission.VIBRATE

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型                                      | 必填 | 说明                                |
| -------- |-----------------------------------------| ---- |-----------------------------------|
| param     | [VibratorInfoParam](#vibratorinfoparamsup22sup) | 否   | 指出需要控制的设备和马达信息，默认控制的为本地设备马达 |

**返回值**：

| 类型                | 说明          |
| ------------------- | ------------- |
| Promise | Promise对象。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息           |
| -------- | ------------------ |
| 201      | Permission denied. |
| 14600101 | Device operation failed. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

function vibratorDemo() {
  // 查询所有马达设备信息。
  const vibratorInfoList: vibrator.VibratorInfo[] = vibrator.getVibratorInfoSync();
  // 根据实际业务逻辑获取目标马达, 例如查找本地马达，此处示例仅做展示，开发者需要自行调整筛选逻辑。
  const targetVibrator = vibratorInfoList.find((vibrator: vibrator.VibratorInfo) => {
    return vibrator.isLocalVibrator;
  });
  if (!targetVibrator) {
    return;
  }
  // 调用 vibrator.startVibration 开始振动。
  // ...

  // 使用try catch对可能出现的异常进行捕获。
  try {
    // 根据实际业务场景停止马达振动。
    vibrator.stopVibration({ deviceId: targetVibrator.deviceId, vibratorId: targetVibrator.vibratorId }).then(() => {
      console.info('Succeed in stopping vibration');
    }, (error: BusinessError) => {
      console.error(`Failed to stop vibration. Code: ${error.code}, message: ${error.message}`);
    });
  } catch (error) {
    let e: BusinessError = error as BusinessError;
    console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
  }
}
```

## vibrator.stopVibrationSync<sup>22+</sup>

stopVibrationSync(): void

停止任何形式的马达振动。

**需要权限**：ohos.permission.VIBRATE

**原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**错误码**：

以下错误码的详细介绍请参见[振动错误码](../errorcodes/errorcode-vibrator.md)和[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 201      | Permission denied.       |
| 14600101 | Device operation failed. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 停止任何形式的马达振动
  vibrator.stopVibrationSync()
  console.info('Succeed in stopping vibration');
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.isSupportEffect<sup>22+</sup>

isSupportEffect(effectId: string, callback: AsyncCallback&lt;boolean&gt;): void

查询是否支持传入参数effectId。使用callback异步回调。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型                         | 必填 | 说明                                                        |
| -------- | ---------------------------- | ---- | ----------------------------------------------------------- |
| effectId | string                       | 是   | 待确认的预置振动效果，字符串最大长度64，超出截取64。|
| callback | AsyncCallback | 是   | 回调函数，当返回true则表示支持该effectId，返回false不支持。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 查询是否支持'haptic.notice.success'
  vibrator.isSupportEffect('haptic.notice.success', (err: BusinessError, state: boolean) => {
    if (err) {
      console.error(`Failed to query effect. Code: ${err.code}, message: ${err.message}`);
      return;
    }
    console.info('Succeed in querying effect');
    if (state) {
      try {
        // 使用startVibration需要添加ohos.permission.VIBRATE权限
        vibrator.startVibration({
          type: 'preset',
          effectId: 'haptic.notice.success',
          count: 1,
        }, {
          usage: 'unknown' // 根据实际选择类型归属不同的开关管控
        }, (error: BusinessError) => {
          if (error) {
            console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
          } else {
            console.info('Succeed in starting vibration');
          }
        });
      } catch (error) {
        let e: BusinessError = error as BusinessError;
        console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
      }
    }
  })
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.isSupportEffect<sup>22+</sup>

isSupportEffect(effectId: string): Promise&lt;boolean&gt;

查询是否支持传入参数effectId。使用promise异步回调。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型   | 必填 | 说明                   |
| -------- | ------ | ---- | ---------------------- |
| effectId | string | 是   | 待确认的预置振动效果，字符串最大长度64，超出截取64。 |

**返回值**：

| 类型                   | 说明                                                         |
| ---------------------- | ------------------------------------------------------------ |
| Promise | Promise对象。当返回true则表示支持该effectId，返回false不支持。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 201      | Permission denied.                                           |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 查询是否支持'haptic.notice.success'
  vibrator.isSupportEffect('haptic.notice.success').then((state: boolean) => {
    console.info(`The query result is ${state}`);
    if (state) {
      try {
        vibrator.startVibration({
          type: 'preset',
          effectId: 'haptic.notice.success',
          count: 1,
        }, {
          usage: 'unknown' // 根据实际选择类型归属不同的开关管控
        }).then(() => {
          console.info('Succeed in starting vibration');
        }).catch((error: BusinessError) => {
          console.error(`Failed to start vibration. Code: ${error.code}, message: ${error.message}`);
        });
      } catch (error) {
        let e: BusinessError = error as BusinessError;
        console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
      }
    }
  }, (error: BusinessError) => {
    console.error(`Failed to query effect. Code: ${error.code}, message: ${error.message}`);
  })
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.isSupportEffectSync<sup>22+</sup>

isSupportEffectSync(effectId: string): boolean

查询是否支持预设的振动效果。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型   | 必填 | 说明                   |
| -------- | ------ | ---- | ---------------------- |
| effectId | string | 是   | 待确认的预置振动效果，字符串最大长度64，超出截取64。 |

**返回值**：

| 类型    | 说明                                                        |
| ------- | ----------------------------------------------------------- |
| boolean | 返回对象。当返回true则表示支持该effectId，返回false不支持。 |

**错误码**：

以下错误码的详细介绍请参见[振动错误码](../errorcodes/errorcode-vibrator.md)和[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |
| 14600101 | Device operation failed.                                     |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 查询是否支持预设'haptic.notice.success'
  let ret = vibrator.isSupportEffectSync('haptic.notice.success');
  console.info(`The query result is ${ret}`);
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.getEffectInfoSync<sup>22+</sup>

getEffectInfoSync(effectId: string, param?: VibratorInfoParam): EffectInfo

通过设备ID和可控马达ID获取预置振动效果信息，用于判断该预置振动效果是否受支持。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型                                                         | 必填 | 说明                           |
| -------- | ------------------------------------------------------------ | ---- |------------------------------|
| effectId | string | 是   | 待确认的预置振动效果，字符串最大长度64，超出截取64。 |
| param     | [VibratorInfoParam](#vibratorinfoparamsup22sup)                       | 否   | 指出需要查询的设备和马达信息，默认查询的是本地设备马达。 |

**错误码**：

以下错误码的详细介绍请参见[振动错误码](../errorcodes/errorcode-vibrator.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 14600101 | Device operation failed. |

**返回值**：

| 类型                        | 说明                                                      |
|---------------------------| --------------------------------------------------------- |
| [EffectInfo](#effectinfosup22sup) | 该信息包括指示是否支持该效果。 |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  const effectInfo: vibrator.EffectInfo = vibrator.getEffectInfoSync('haptic.clock.timer', { deviceId: 1, vibratorId: 3});
  console.info(`isEffectSupported: ${effectInfo.isEffectSupported}`);
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## vibrator.getVibratorInfoSync<sup>22+</sup>

getVibratorInfoSync(param?: VibratorInfoParam): Array&lt;VibratorInfo&gt;;

查询本地设备马达信息列表。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS
**参数**：

| 参数名   | 类型                                      | 必填 | 说明                             |
| -------- |-----------------------------------------| ---- |--------------------------------|
| param     | [VibratorInfoParam](#vibratorinfoparamsup22sup) | 否   | 指出需要控制的设备和马达信息,默认查询本地设备马达的信息 |

**返回值**：

| 类型                                         | 说明                                                      |
|--------------------------------------------| --------------------------------------------------------- |
| Array<[VibratorInfo](#vibratorinfosup22sup)> | 马达设备的信息。 |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

try {
  const vibratorInfoList: vibrator.VibratorInfo[] = vibrator.getVibratorInfoSync({ deviceId: 1, vibratorId: 3 });
  console.info(`vibratorInfoList: ${JSON.stringify(vibratorInfoList)}`);
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## VibratorInfoParam<sup>22+</sup>

设备上马达的参数。默认情况下，VibratorInfoParam默认为查询或控制本地马达。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称 | 类型   | 只读 | 可选 | 说明                                                                                                                              |
| ---- | ------ | ---- | ---- |---------------------------------------------------------------------------------------------------------------------------------|
| deviceId    | number | 否   | 是   | 设备的ID：默认值为-1，表示本地设备，API22以后设备ID可以使用[getVibratorInfoSync](#vibratorgetvibratorinfosyncsup22sup)查询。                               |
| vibratorId    | number | 否   | 是   | 马达ID：默认值为0，控制的是该设备的全部马达，API22以后马达ID可以使用[getVibratorInfoSync](#vibratorgetvibratorinfosyncsup22sup)查询。 |

## EffectInfo<sup>22+</sup>

查询的预制效果信息。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称                | 类型      | 只读 | 可选 | 说明                            |
|-------------------|---------|----|----|-------------------------------|
| isEffectSupported | boolean | 是  | 否  | 预制效果是否支持，true表示支持，false表示不支持。 |

## VibratorInfo<sup>22+</sup>

表示查询的马达信息。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称                  | 类型      | 只读 | 可选 | 说明        |
|---------------------|---------|----|----|-----------|
| deviceId            | number  | 是  | 否  | 设备ID。     |
| vibratorId          | number  | 是  | 否  | 马达ID。     |
| deviceName          | string  | 是  | 否  | 设备名称。     |
| isHdHapticSupported | boolean | 是  | 否  | 是否支持高清振动。 |
| isLocalVibrator     | boolean | 是  | 否  | 是否为本地设备。  |

## vibrator.isHdHapticSupported<sup>22+</sup>

isHdHapticSupported(): boolean

查询是否支持高清振动。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

**返回值**：

| 类型    | 说明                                                      |
| ------- | --------------------------------------------------------- |
| boolean | 返回对象，当返回true表示支持高清振动，返回false不支持。。 |

**错误码**：

以下错误码的详细介绍请参见[振动错误码](../errorcodes/errorcode-vibrator.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息                 |
| -------- | ------------------------ |
| 14600101 | Device operation failed. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

// 使用try catch对可能出现的异常进行捕获
try {
  // 查询是否支持高清振动
  let ret = vibrator.isHdHapticSupported();
  console.info(`The query result is ${ret}`);
} catch (error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## VibratorPatternBuilder<sup>22+</sup>

### vibrator('addContinuousEvent')<sup>22+</sup>

addContinuousEvent(time: number, duration: number, options?: ContinuousParam): VibratorPatternBuilder;

添加长振事件的方法成VibratorPattern对象。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

**参数**：

| 参数名   | 类型                                  | 必填 | 说明                                                         |
| -------- |-------------------------------------| ---- | ------------------------------------------------------------ |
| time     | number                              | 是   | 长振事件的起始时间。单位ms，取值范围[0,1800000]区间内所有整数。 |
| duration | number                              | 是   | 长振事件的持续时间。单位ms，取值范围(0,5000]区间内所有整数。 |
| options  | [ContinuousParam](#continuousparamsup22sup) | 否   | 可选参数，可选参数对象。                                     |

**返回值**：

| 类型                   | 说明                                                 |
| ---------------------- | ---------------------------------------------------- |
| VibratorPatternBuilder | 返回已添加连续振动事件的VibratorPatternBuilder对象。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息         |
| -------- | ---------------- |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

let builder = new vibrator.VibratorPatternBuilder();
// 使用try catch对可能出现的异常进行捕获
try {
  let pointsMe: vibrator.VibratorCurvePoint[] = [
      { time: 0, intensity: 0, frequency: -7 },
      { time: 42, intensity: 1, frequency: -6 },
      { time: 128, intensity: 0.94, frequency: -4 },
      { time: 217, intensity: 0.63, frequency: -14 },
      { time: 763, intensity: 0.48, frequency: -14 },
      { time: 1125, intensity: 0.53, frequency: -10 },
      { time: 1503, intensity: 0.42, frequency: -14 },
      { time: 1858, intensity: 0.39, frequency: -14 },
      { time: 2295, intensity: 0.34, frequency: -17 },
      { time: 2448, intensity: 0.21, frequency: -14 },
      { time: 2468, intensity: 0, frequency: -21 }
  ] // VibratorCurvePoint参数最少设置4个，最大设置16个
  let param: vibrator.ContinuousParam = {
      intensity: 97,
      frequency: 34,
      points:pointsMe,
      index: 0
  }
  builder.addContinuousEvent(0, 2468, param);
  console.info(`addContinuousEvent builder is ${builder.build()}`);
} catch(error) {
  let e: BusinessError = error as BusinessError;
  console.error(`Exception. Code ${e.code}`);
}
```

### vibrator('addTransientEvent')<sup>22+</sup>

addTransientEvent(time: number, options?: TransientParam): VibratorPatternBuilder;

添加短振事件的方法成VibratorPattern对象。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

**参数**：

| 参数名  | 类型                                | 必填 | 说明                                                         |
| ------- | --------------------------------- | ---- | ------------------------------------------------------------ |
| time    | number                            | 是   | 短振事件的起始时间。单位ms，取值范围[0,1800000]区间内所有整数。 |
| options | [TransientParam](#transientparamsup22sup) | 否   | 可选参数，可选参数对象。                                     |

**返回值**：

| 类型                   | 说明                                             |
| ---------------------- | ------------------------------------------------ |
| VibratorPatternBuilder | 返回已添加短振事件的VibratorPatternBuilder对象。 |

**错误码**：

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-utils.md)。错误码和错误信息会以异常的形式抛出，调用接口时需要使用try catch对可能出现的异常进行捕获操作。

| 错误码ID | 错误信息         |
| -------- | ---------------- |
| 401      | Parameter error.Possible causes:1. Mandatory parameters are left unspecified;2. Incorrect parameter types;3. Parameter verification failed. |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

let builder = new vibrator.VibratorPatternBuilder();
// 使用try catch对可能出现的异常进行捕获
try {
  let param: vibrator.TransientParam = {
      intensity: 80,
      frequency: 70,
      index: 0
  }
  builder.addTransientEvent(0, param);
  console.info(`addTransientEvent builder is ${builder.build()}`);
} catch(error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

### vibrator('build')<sup>22+</sup>

build(): VibratorPattern;

构造组合短事件或长事件的振动序列的方法。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

**返回值**：

| 类型                                  | 说明                               |
| ----------------------------------- | ---------------------------------- |
| [VibratorPattern](#vibratorpatternsup22sup) | 构造组合短振或长振的振动序列方法。 |

**示例**：

```ts
import { vibrator } from '@kit.SensorServiceKit';
import { BusinessError } from '@kit.BasicServicesKit';

let builder = new vibrator.VibratorPatternBuilder();
try {
  let param: vibrator.TransientParam = {
      intensity: 80,
      frequency: 70,
      index: 0
  }
  builder.addTransientEvent(0, param);
  console.info(`addTransientEvent builder is ${builder.build()}`);
} catch(error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
try {
  vibrator.startVibration({
      type: "pattern",
      pattern: builder.build()
  }, {
    usage: "alarm", // 根据实际选择类型归属不同的开关管控
  }, (error) => {
    if (error) {
      let e: BusinessError = error as BusinessError;
      console.error(`Vibrate fail. Code: ${e.code}, message: ${e.message}`);
    } else {
      console.info(`vibrate success`);
    }
  });
} catch(error) {
  let e: BusinessError = error as BusinessError;
  console.error(`An unexpected error occurred. Code: ${e.code}, message: ${e.message}`);
}
```

## EffectId

预置的振动效果。在调用[vibrator.startVibration22+](#vibratorstartvibrationsup22sup)或[vibrator.stopVibration22+](#vibratorstopvibrationsup22sup-1)接口下发[VibratePreset](#vibratepresetsup22sup)形式振动的时候需要使用此参数类型。此参数值种类多样，'haptic.clock.timer'为其中一种。[HapticFeedback<sup>22+</sup>](#hapticfeedbacksup22sup)展示了几种常用的EffectId值。

> **说明：**
> 
> 由于设备存在多样性，不同的设备可能预置不同的效果，建议使用预置效果前先使用[vibrator.isSupportEffect](#vibratorissupporteffectsup22sup)<sup>22+</sup>接口查询当前设备是否支持该预置效果。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称        | 值                   | 说明                         |
| ----------- | -------------------- | ---------------------------- |
| EFFECT_CLOCK_TIMER | 'haptic.clock.timer' | 描述用户调整计时器时的振动效果。 |

## HapticFeedback<sup>22+</sup>

简单而通用的振动效果。根据各设备的马达器件不同，同一振动效果的频率会有差异，但效果的频率趋向是统一的。这几种振动效果是EffectId参数的具体值，使用方法参考[vibrator.startVibration22+](#vibratorstartvibrationsup22sup)或[vibrator.stopVibration22+](#vibratorstopvibrationsup22sup)接口下发[VibratePreset](#vibratepresetsup22sup)形式振动的示例代码。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称                                | 值                      | 说明                         |
| ----------------------------------- | ----------------------- | ---------------------------- |
| EFFECT_SOFT                         | 'haptic.effect.soft'    | 较松散的振动效果，频率偏低。 |
| EFFECT_HARD                         | 'haptic.effect.hard'    | 较沉重的振动效果，频率居中。 |
| EFFECT_SHARP                        | 'haptic.effect.sharp'   | 较尖锐的振动效果，频率偏高。 |
| EFFECT_NOTICE_SUCCESS22+ | 'haptic.notice.success' | 表达成功通知的振动效果。     |
| EFFECT_NOTICE_FAILURE22+ | 'haptic.notice.fail'    | 表达失败通知的振动效果。     |
| EFFECT_NOTICE_WARNING22+ | 'haptic.notice.warning' | 表达警告通知的振动效果。     |

## VibratorStopMode

停止振动的模式。在调用[vibrator.stopVibration22+](#vibratorstopvibrationsup22sup)或[vibrator.stopVibration22+](#vibratorstopvibrationsup22sup-1)接口时，需要使用此参数类型指定停止的振动模式。停止模式和[VibrateEffect22+](#vibrateeffectsup22sup)中下发的模式为对应关系。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称                      | 值       | 说明                     |
| ------------------------- | -------- | ------------------------ |
| VIBRATOR_STOP_MODE_TIME   | 'time'   | 停止duration模式的振动。 |
| VIBRATOR_STOP_MODE_PRESET | 'preset' | 停止预置EffectId的振动。 |

## VibrateEffect<sup>22+</sup>

马达振动效果，支持以下四种。在调用[vibrator.startVibration22+](#vibratorstartvibrationsup22sup)或[vibrator.startVibration22+](#vibratorstartvibrationsup22sup-1)接口时，此参数的四种类型表示以四种不同的形式触发振动。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 类型                                                              | 说明                                                                |
|-----------------------------------------------------------------|-------------------------------------------------------------------|
| [VibrateTime](#vibratetimesup22sup)                             | 按照指定时长触发马达振动。<br/>**原子化服务API：** 从API Version 22开始，该接口支持在原子化服务中使用。 |
| [VibratePreset](#vibratepresetsup22sup)                         | 按照预置振动类型触发马达振动。                                                   |
| [VibrateFromFile](#vibratefromfilesup22sup)                     | 按照自定义振动配置文件触发马达振动。                                                |
| [VibrateFromPattern<sup>22+</sup>](#vibratefrompatternsup22sup) | 按照自定义振动效果触发马达振动。                                                  |

## VibrateTime<sup>22+</sup>

指定时长振动类型。

**原子化服务API**：从API Version 22开始，该接口在支持原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称     | 类型   | 必填 | 说明                                                        |
| -------- | ------ | ---- | ----------------------------------------------------------- |
| type     | 'time' | 是   | 值为'time'，按照指定时长触发马达振动。                      |
| duration | number | 是   | 马达持续振动时长, 单位ms。取值范围(0,1800000]区间内所有整数 |

## VibratePreset<sup>22+</sup>

预置振动类型。当调用[vibrator.startVibration22+](#vibratorstartvibrationsup22sup)或[vibrator.startVibration22+](#vibratorstartvibrationsup22sup-1)时，[VibrateEffect22+](#vibrateeffectsup22sup)参数的值可以为VibratePreset，表示触发预置振动类型。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称                      | 类型     | 必填 | 说明                                                         |
|-------------------------| -------- | ---- | ------------------------------------------------------------ |
| type                    | 'preset' | 是   | 值为'preset'，按照预置振动效果触发马达振动。                 |
| effectId                | string   | 是   | 预置的振动效果ID，字符串最大长度64，超出截取64。                                           |
| count                   | number   | 否   | 可选参数，振动的重复次数，默认值为1。                        |
| intensity22+ | number   | 否   | 可选参数，振动调节强度，取值范围(0,100]内所有整数，默认值为100。若振动效果不支持强度调节或设备不支持时，则按默认强度振动。 |

## VibrateFromFile<sup>22+</sup>

自定义振动类型。当设备不支持此振动类型时，返回设备不支持错误码。当调用[vibrator.startVibration22+](#vibratorstartvibrationsup22sup)或[vibrator.startVibration22+](#vibratorstartvibrationsup22sup-1)时，[VibrateEffect22+](#vibrateeffectsup22sup)参数的值可以为VibrateFromFile，表示触发自定义振动类型。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称     | 类型                                                            | 只读 | 可选 | 说明                                       |
| -------- |---------------------------------------------------------------| ---- | ---- | ------------------------------------------ |
| type     | 'file'                                                        | 否   | 否   | 值为'file'，按照振动配置文件触发马达振动。 |
| hapticFd | [HapticFileDescriptor](#hapticfiledescriptor10)22+ | 否   | 否   | 振动配置文件的描述符。                     |

## HapticFileDescriptor<sup>22+</sup>

自定义振动配置文件的描述符，必须确认资源文件可用，其参数可通过资源管理API从HAP资源获取。使用场景：振动序列被存储在一个文件中，需要根据偏移量和长度进行振动。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称   | 类型   | 只读 | 可选 | 说明                                                         |
| ------ | ------ | ---- | ---- | ------------------------------------------------------------ |
| fd     | number | 否   | 否   | 资源文件描述符。                                             |
| offset | number | 否   | 是   | 距文件起始位置的偏移量，单位为字节，默认为文件起始位置，不可超出文件有效范围。 |
| length | number | 否   | 是   | 资源长度，单位为字节，默认值为从偏移位置至文件结尾的长度，不可超出文件有效范围。 |

## VibratorEventType<sup>22+</sup>

振动事件类型。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称       | 类型   | 必填 | 说明              |
| ---------- | ------ | ---- | ----------------- |
| CONTINUOUS | number | 是   | 值为0，表示长振。 |
| TRANSIENT  | number | 是   | 值为1，表示短振。 |

## VibratorCurvePoint<sup>22+</sup>

相对事件振动强度的增益。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称      | 类型   | 必填 | 说明                                                         |
| --------- | ------ | ---- | ------------------------------------------------------------ |
| time      | number | 是   | 起始时间偏移。                                               |
| intensity | number | 否   | 可选参数，相对事件振动强度增益，取值范围[0,100%]，省略时默认值为1。 |
| frequency | number | 否   | 可选参数，相对事件振动频率变化，取值范围[-100,100]内所有整数，省略时默认值为0。 |

## VibratorEvent<sup>22+</sup>

振动事件。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称      | 类型                                                             | 必填 | 说明                                                         |
| --------- |----------------------------------------------------------------| ---- | ------------------------------------------------------------ |
| eventType | VibratorEventType                                              | 是   | 振动事件类型。                                               |
| time      | number                                                         | 是   | 振动起始时间，单位ms。取值范围[0,1800000]区间内所有整数。    |
| duration  | number                                                         | 否   | 可选参数，表示振动持续时间，取值范围（0,5000]区间所有整数，短振默认值为48，长振默认值为1000 |
| intensity | number                                                         | 否   | 可选参数，表示振动强度，取值范围[0,100]区间所有整数，省略时默认值为100。 |
| frequency | number                                                         | 否   | 可选参数，表示振动频率，取值范围[0,100]区间内所有整数，省略时默认值为50。 |
| index     | number                                                         | 否   | 可选参数，表示通道编号，默认值为0。        |
| points    | Array<[VibratorCurvePoint](#vibratorcurvepointsuo22sup)> | 否   | 可选参数，表示振动调节曲线数组。                             |

## VibratorPattern<sup>22+</sup>

马达振动序列，每个events代表一个振动事件。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称   | 类型                                                   | 必填 | 说明                                                 |
| ------ |------------------------------------------------------| ---- | ---------------------------------------------------- |
| time   | number                                               | 是   | 振动绝对起始时间。                                   |
| events | Array<[VibratorEvent](#vibratoreventsup22sup)> | 是   | 振动事件数组，build()方法返回的VibratorPattern对象。 |

## ContinuousParam<sup>22+</sup>

连续振动参数。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称      | 类型                 | 必填 | 说明                                                         |
| --------- | -------------------- | ---- | ------------------------------------------------------------ |
| intensity | number               | 否   | 可选参数，表示振动强度，取值范围[0,100]内所有整数，省略时默认值为100。 |
| frequency | number               | 否   | 可选参数，表示振动频率，取值范围[0,100]内所有整数，省略时默认值为50。 |
| points    | [VibratorCurvePoint](#vibratorcurvepointsup22sup)[] | 否   | 可选参数，表示振动调节曲线数组。                             |
| index     | number               | 否   | 可选参数，表示通道编号，默认值为0。                    |

## TransientParam<sup>22+</sup>

瞬态振动参数。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称      | 类型   | 必填 | 说明                                                         |
| --------- | ------ | ---- | ------------------------------------------------------------ |
| intensity | number | 否   | 可选参数，表示振动强度，取值范围[0,100]内所有整数，省略时默认值为100。 |
| frequency | number | 否   | 可选参数，表示振动频率，取值范围[0,100]内所有整数，省略时默认值为50。 |
| index     | number | 否   | 可选参数，表示通道编号，默认值为0。                    |

## VibrateFromPattern<sup>22+</sup>

自定义振动效果触发马达振动。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称    | 类型            | 必填 | 说明                                                 |
| ------- | --------------- | ---- | ---------------------------------------------------- |
| type    | 'pattern'       | 是   | 值为“pattern”，根据组合模式触发电机振动。            |
| pattern | VibratorPattern | 是   | 振动事件数组，build()方法返回的VibratorPattern对象。 |

## VibrateAttribute<sup>22+</sup>

马达振动属性。

**原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

| 名称                     | 类型                      | 必填 | 说明                                                                                                                                                                                    |
|------------------------|-------------------------| ---- |---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| id                     | number                  | 否   | 马达ID， 默认值为0。                                                                                                                                                                          |
| deviceId22+ | number                  | 否   | 设备ID，默认值为-1，表示本地设备，API22以后设备ID可以使用[getVibratorInfoSync](#vibratorgetvibratorinfosyncsup22sup)查询。 **原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。 |
| usage                  | [Usage](#usagesup22sup) | 是   | 马达振动的使用场景。默认值为'unknown'，取值范围只允许在[Usage](#usagesup22sup)提供的类型中选取。                                                                                                                      |

## Usage<sup>22+</sup>

type Usage = 'unknown' | 'alarm' | 'ring' | 'notification' | 'communication' | 'touch' | 'media' | 'physicalFeedback' | 'simulateReality'

振动使用场景。

**原子化服务API**：从API Version 22开始，该接口支持在原子化服务中使用。

**系统能力**：SystemCapability.Sensors.MiscDevice

**支持平台**：Android、iOS

<!--RP1-->

| 类型               | 说明                                                    |
| ------------------ | ------------------------------------------------------- |
| 'unknown'          | 没有明确使用场景，最低优先级，值固定为'unknown'字符串。 |
| 'alarm'            | 用于警报场景，值固定为'alarm'字符串。                   |
| 'ring'             | 用于铃声场景，值固定为'ring'字符串。                    |
| 'notification'     | 用于通知场景，值固定为'notification'字符串。            |
| 'communication'    | 用于通信场景，值固定为'communication'字符串。           |
| 'touch'            | 用于触摸场景，值固定为'touch'字符串。                   |
| 'media'            | 用于多媒体场景，值固定为'media'字符串。                 |
| 'physicalFeedback' | 用于物理反馈场景，值固定为'physicalFeedback'字符串。    |
| 'simulateReality'  | 用于模拟现实场景，值固定为'simulateReality'字符串。     |

<!--RP1End-->

