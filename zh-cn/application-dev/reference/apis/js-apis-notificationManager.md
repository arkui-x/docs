# @ohos.notificationManager (NotificationManager模块)

本模块提供通知管理的能力，包括发布、取消发布通知，创建、获取、移除通知渠道，获取通知的使能状态、角标使能状态，获取通知的相关信息等。

> **说明：**
>
> 本模块首批接口从API version 12开始支持跨平台。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```ts
import { notificationManager } from '@kit.NotificationKit';
```

## notificationManager.publish

publish(request: NotificationRequest, callback: AsyncCallback\<void\>): void

发布通知。使用callback异步回调。

如果新发布通知与已发布通知的ID相同，则新通知将取代原有通知。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名     | 类型                                        | 必填 | 说明                                        |
| -------- | ------------------------------------------- | ---- | ------------------------------------------- |
| request  | [NotificationRequest](js-apis-inner-notification-notificationRequest.md#notificationrequest) | 是   | 用于设置要发布通知的内容和相关配置信息。 |
| callback | AsyncCallback\<void\>                       | 是   | 发布通知的回调方法。                        |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                                              |
| -------- | ---------------------------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.    | 
| 1600001  | Internal error.                                      |
| 1600002  | Marshalling or unmarshalling error.                  |
| 1600003  | Failed to connect to the service.                    |
| 1600004  | Notification disabled.                               |
| 1600005  | Notification slot disabled.                          |
| 1600007  | The notification does not exist.                     |
| 1600009  | The notification sending frequency reaches the upper limit.            |
| 1600012  | No memory space.                                     |
| 1600014  | No permission.                                       |
| 1600015  | The current notification status does not support duplicate configurations. |
| 1600016  | The notification version for this update is too low. |
| 2300007  | Network unreachable.                                 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

//publish回调
let publishCallback = (err: BusinessError): void => {
    if (err) {
        console.error(`publish failed, code is ${err.code}, message is ${err.message}`);
    } else {
        console.info("publish success");
    }
}
//通知Request对象
let notificationRequest: notificationManager.NotificationRequest = {
    id: 1,
    content: {
        notificationContentType: notificationManager.ContentType.NOTIFICATION_CONTENT_BASIC_TEXT,
        normal: {
            title: "test_title",
            text: "test_text"
        }
    }
};
notificationManager.publish(notificationRequest, publishCallback);
```

## notificationManager.publish

publish(request: NotificationRequest): Promise\<void\>

发布通知。使用Promise异步回调。

如果新发布通知与已发布通知的ID相同，则新通知将取代原有通知。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名     | 类型                                        | 必填 | 说明                                        |
| -------- | ------------------------------------------- | ---- | ------------------------------------------- |
| request  | [NotificationRequest](js-apis-inner-notification-notificationRequest.md#notificationrequest) | 是   | 用于设置要发布通知的内容和相关配置信息。 |

**返回值：**

| 类型     | 说明 | 
| ------- |--|
| Promise\<void\> | 无返回结果的Promise对象。 | 

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                                              |
| -------- | ---------------------------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.    | 
| 1600001  | Internal error.                                      |
| 1600002  | Marshalling or unmarshalling error.                  |
| 1600003  | Failed to connect to the service.                    |
| 1600004  | Notification disabled.                               |
| 1600005  | Notification slot disabled.                          |
| 1600007  | The notification does not exist.                     |
| 1600009  | The notification sending frequency reaches the upper limit.            |
| 1600012  | No memory space.                                     |
| 1600014  | No permission.                                       |
| 1600015  | The current notification status does not support duplicate configurations. |
| 1600016  | The notification version for this update is too low. |
| 2300007  | Network unreachable.                                 |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

// 通知Request对象
let notificationRequest: notificationManager.NotificationRequest = {
    id: 1,
    content: {
        notificationContentType: notificationManager.ContentType.NOTIFICATION_CONTENT_BASIC_TEXT,
        normal: {
            title: "test_title",
            text: "test_text"
        }
    }
};
notificationManager.publish(notificationRequest).then(() => {
	console.info("publish success");
}).catch((err: BusinessError) => {
    console.error(`publish fail: ${JSON.stringify(err)}`);
});

```

## notificationManager.cancel

cancel(id: number, callback: AsyncCallback\<void\>): void

取消与指定通知ID相匹配的已发布通知。使用callback异步回调。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名     | 类型                  | 必填 | 说明                 |
| -------- | --------------------- | ---- | -------------------- |
| id       | number                | 是   | 通知ID。               |
| callback | AsyncCallback\<void\> | 是   | 表示被指定通知的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |
| 1600007  | The notification does not exist.      |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

// cancel回调
let cancelCallback = (err: BusinessError): void => {
    if (err) {
        console.error(`cancel failed, code is ${err.code}, message is ${err.message}`);
    } else {
        console.info("cancel success");
    }
}
notificationManager.cancel(0, cancelCallback);
```

## notificationManager.cancelAll

cancelAll(callback: AsyncCallback\<void\>): void

取消当前应用所有已发布的通知。使用callback异步回调。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名     | 类型                  | 必填 | 说明                 |
| -------- | --------------------- | ---- | -------------------- |
| callback | AsyncCallback\<void\> | 是   | 表示被指定通知的回调方法。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

// cancel回调
let cancelAllCallback = (err: BusinessError): void => {
    if (err) {
        console.error(`cancelAll failed, code is ${err.code}, message is ${err.message}`);
    } else {
        console.info("cancelAll success");
    }
}
notificationManager.cancelAll(cancelAllCallback);
```

## notificationManager.cancelAll

cancelAll(): Promise\<void\>

取消当前应用所有已发布的通知。使用Promise异步回调。

**系统能力**：SystemCapability.Notification.Notification

**返回值：**

| 类型     | 说明        | 
| ------- |-----------|
| Promise\<void\> | 无返回结果的Promise对象。 | 

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

notificationManager.cancelAll().then(() => {
	console.info("cancelAll success");
}).catch((err: BusinessError) => {
    console.error(`cancelAll fail: ${JSON.stringify(err)}`);
});
```

## notificationManager.isNotificationEnabled

isNotificationEnabled(callback: AsyncCallback\<boolean\>): void

获取通知使能状态。使用callback异步回调。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名     | 类型                  | 必填 | 说明                     |
| -------- | --------------------- | ---- | ------------------------ |
| callback | AsyncCallback\<boolean\> | 是   | 获取通知使能状态回调函数（true：使能，false：禁止）。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                                  |
| -------- | ---------------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      |
| 1600001  | Internal error.                          |
| 1600002  | Marshalling or unmarshalling error.      |
| 1600003  | Failed to connect to the service.               |
| 1600008  | The user does not exist.                   |
| 17700001 | The specified bundle name was not found. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let isNotificationEnabledCallback = (err: BusinessError, data: boolean): void => {
    if (err) {
        console.error(`isNotificationEnabled failed, code is ${err.code}, message is ${err.message}`);
    } else {
        console.info(`isNotificationEnabled success, data is ${JSON.stringify(data)}`);
    }
}

notificationManager.isNotificationEnabled(isNotificationEnabledCallback);
```

## notificationManager.isNotificationEnabled

isNotificationEnabled(): Promise\<boolean\>

获取通知使能状态。使用Promise异步回调。

**系统能力**：SystemCapability.Notification.Notification

**返回值：**

| 类型                                                        | 说明                                                         |
| ----------------------------------------------------------- | ------------------------------------------------------------ |
| Promise\<boolean\> | 以Promise形式返回获取通知使能状态的结果。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      |
| 1600001  | Internal error.                          |
| 1600002  | Marshalling or unmarshalling error.      |
| 1600003  | Failed to connect to the service.               |
| 1600008  | The user does not exist.                   |
| 17700001 | The specified bundle name was not found. |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

notificationManager.isNotificationEnabled().then((data: boolean) => {
	console.info("isNotificationEnabled success, data: " + JSON.stringify(data));
}).catch((err: BusinessError) => {
    console.error(`isNotificationEnabled fail: ${JSON.stringify(err)}`);
});
```

## notificationManager.setBadgeNumber

setBadgeNumber(badgeNumber: number): Promise\<void\>

设定角标个数，在应用的桌面图标上呈现。使用Promise异步回调。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名      | 类型   | 必填 | 说明       |
| ----------- | ------ | ---- | ---------- |
| badgeNumber | number | 是   | 角标个数。 |

**返回值：**

| 类型      | 说明        | 
|---------|-----------|
| Promise\<void\> | 无返回结果的Promise对象。 | 

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |
| 1600012  | No memory space.                          |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let badgeNumber: number = 10;

notificationManager.setBadgeNumber(badgeNumber).then(() => {
	console.info("setBadgeNumber success");
}).catch((err: BusinessError) => {
    console.error(`setBadgeNumber fail: ${JSON.stringify(err)}`);
});
```

## notificationManager.setBadgeNumber

setBadgeNumber(badgeNumber: number, callback: AsyncCallback\<void\>): void

设定角标个数，在应用的桌面图标上呈现。使用callback异步回调。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名      | 类型                  | 必填 | 说明               |
| ----------- | --------------------- | ---- | ------------------ |
| badgeNumber | number                | 是   | 角标个数。         |
| callback    | AsyncCallback\<void\> | 是   | 设定角标回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |
| 1600012  | No memory space.                          |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let setBadgeNumberCallback = (err: BusinessError): void => {
    if (err) {
        console.error(`setBadgeNumber failed code is ${err.code}, message is ${err.message}`);
    } else {
        console.info("setBadgeNumber success");
    }
}
let badgeNumber: number = 10;
notificationManager.setBadgeNumber(badgeNumber, setBadgeNumberCallback);
```

## notificationManager.requestEnableNotification<sup>(deprecated)</sup>

requestEnableNotification(callback: AsyncCallback\<void\>): void

应用请求通知使能。使用callback异步回调。

> **说明：**
>
> 从API version 12开始不再维护，建议使用有context入参的[requestEnableNotification](#notificationmanagerrequestenablenotification)代替。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名   | 类型                     | 必填 | 说明                       |
| -------- | ------------------------ | ---- | -------------------------- |
| callback | AsyncCallback\<void\> | 是   | 应用请求通知使能的回调函数。 |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |
| 1600004  | Notification disabled.          |
| 1600013  | A notification dialog box is already displayed.           |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

let requestEnableNotificationCallback = (err: BusinessError): void => {
    if (err) {
        console.error(`requestEnableNotification failed, code is ${err.code}, message is ${err.message}`);
    } else {
        console.info("requestEnableNotification success");
    }
};
notificationManager.requestEnableNotification(requestEnableNotificationCallback);
```

## notificationManager.requestEnableNotification<sup>(deprecated)</sup>

requestEnableNotification(): Promise\<void\>

应用请求通知使能。使用Promise异步回调。

> **说明：**
>
> 从API version 12开始不再维护，建议使用有context入参的[requestEnableNotification](#notificationmanagerrequestenablenotification-1)代替。

**系统能力**：SystemCapability.Notification.Notification

**返回值：**

| 类型      | 说明        | 
|---------|-----------|
| Promise\<void\> | 无返回结果的Promise对象。 | 

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |
| 1600004  | Notification disabled.          |
| 1600013  | A notification dialog box is already displayed.           |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';

notificationManager.requestEnableNotification().then(() => {
    console.info("requestEnableNotification success");
}).catch((err: BusinessError) => {
    console.error(`requestEnableNotification fail: ${JSON.stringify(err)}`);
});
```

## notificationManager.requestEnableNotification

requestEnableNotification(context: UIAbilityContext, callback: AsyncCallback\<void\>): void

应用请求通知使能模态弹窗。使用callback异步回调。

**模型约束**：此接口仅可在Stage模型下使用。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名   | 类型                     | 必填 | 说明                 |
| -------- | ------------------------ | ---- |--------------------|
| context | [UIAbilityContext](js-apis-inner-application-uiAbilityContext.md) | 是   | 通知弹窗绑定Ability的上下文。 |
| callback | AsyncCallback\<void\> | 是   | 应用请求通知使能的回调函数。     |

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      |
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |
| 1600004  | Notification disabled.          |
| 1600013  | A notification dialog box is already displayed.           |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { UIAbility } from '@kit.AbilityKit';
import { window } from '@kit.ArkUI';
import { hilog } from '@kit.PerformanceAnalysisKit';

class MyAbility extends UIAbility {
    onWindowStageCreate(windowStage: window.WindowStage) {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err, data) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
      let requestEnableNotificationCallback = (err: BusinessError): void => {
        if (err) {
            console.error(`requestEnableNotification failed, code is ${err.code}, message is ${err.message}`);
        } else {
            console.info("requestEnableNotification success");
        }
      };
      notificationManager.requestEnableNotification(this.context, requestEnableNotificationCallback);
    });
  }
}
```

## notificationManager.requestEnableNotification

requestEnableNotification(context: UIAbilityContext): Promise\<void\>

应用请求通知使能模态弹窗。使用Promise异步回调。

**模型约束**：此接口仅可在Stage模型下使用。

**系统能力**：SystemCapability.Notification.Notification

**参数：**

| 参数名   | 类型                     | 必填 | 说明                 |
| -------- | ------------------------ | ---- |--------------------|
| context | [UIAbilityContext](js-apis-inner-application-uiAbilityContext.md) | 是   | 通知弹窗绑定Ability的上下文。 |

**返回值：**

| 类型      | 说明        | 
|---------|-----------|
| Promise\<void\> | 无返回结果的Promise对象。 | 

**错误码：**

以下错误码的详细介绍请参见[通用错误码](../errorcodes/errorcode-universal.md)和[通知错误码](../errorcodes/errorcode-notification.md)。

| 错误码ID | 错误信息                            |
| -------- | ----------------------------------- |
| 401     | Parameter error. Possible causes: 1. Mandatory parameters are left unspecified. 2. Incorrect parameter types. 3.Parameter verification failed.      | 
| 1600001  | Internal error.                     |
| 1600002  | Marshalling or unmarshalling error. |
| 1600003  | Failed to connect to the service.          |
| 1600004  | Notification disabled.          |
| 1600013  | A notification dialog box is already displayed.           |

**示例：**

```ts
import { BusinessError } from '@kit.BasicServicesKit';
import { UIAbility } from '@kit.AbilityKit';
import { window } from '@kit.ArkUI';
import { hilog } from '@kit.PerformanceAnalysisKit';

class MyAbility extends UIAbility {
    onWindowStageCreate(windowStage: window.WindowStage) {
    hilog.info(0x0000, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err, data) => {
      if (err.code) {
        hilog.error(0x0000, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err) ?? '');
        return;
      }
      hilog.info(0x0000, 'testTag', 'Succeeded in loading the content. Data: %{public}s', JSON.stringify(data) ?? '');
      notificationManager.requestEnableNotification(this.context).then(() => {
        console.info("requestEnableNotification success");
      }).catch((err: BusinessError) => {
        console.error(`requestEnableNotification fail: ${JSON.stringify(err)}`);
      });
    });
  }
}
```

## ContentType

**系统能力**：以下各项对应的系统能力均为SystemCapability.Notification.Notification

| 名称                              | 值          | 说明               |
| --------------------------------- | ----------- |------------------|
| NOTIFICATION_CONTENT_BASIC_TEXT   | 0          | 普通类型通知。          |
| NOTIFICATION_CONTENT_LONG_TEXT    | 1          | 长文本类型通知。         |
| NOTIFICATION_CONTENT_MULTILINE    | 4          | 多行文本类型通知。        |
