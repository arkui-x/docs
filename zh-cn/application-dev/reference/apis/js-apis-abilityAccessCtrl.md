# @ohos.abilityAccessCtrl (程序访问控制管理)

程序访问控制提供程序的权限校验能力。

> **说明：**
> 本模块首批接口从API version 8开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。

## 导入模块

```js
import abilityAccessCtrl from '@ohos.abilityAccessCtrl';
```

## abilityAccessCtrl.createAtManager

createAtManager(): AtManager

访问控制管理：获取访问控制模块对象。

**系统能力：** SystemCapability.Security.AccessToken


**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [AtManager](#atmanager) | 获取访问控制模块的实例。 |

**示例：**

```js
let atManager = abilityAccessCtrl.createAtManager();
```

## AtManager

管理访问控制模块的实例。

### checkAccessToken<sup>9+</sup>

checkAccessToken(tokenID: number, permissionName: Permissions): Promise&lt;GrantStatus&gt;

校验应用是否授予权限。使用Promise异步回调。当前跨平台上仅支持校验当前应用的自己的授权状态。

**系统能力：** SystemCapability.Security.AccessToken

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                       |
| -------- | -------------------  | ---- | ------------------------------------------ |
| tokenID   |  number   | 是   | 要校验的目标应用的身份标识。当前跨平台上仅支持校验当前应用的自己的授权状态，tokenid可以填任意非0值。         |
| permissionName | Permissions | 是   | 需要校验的权限名称，合法的权限名取值可在[系统权限定义列表](../../../security/security-permission/permission-list.md)中查询。 |

**返回值：**

| 类型          | 说明                                |
| :------------ | :---------------------------------- |
| Promise&lt;GrantStatus&gt; | Promise对象。返回授权状态结果。 |

**错误码：**

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12100001 | The parameter is invalid. The tokenID is 0, or the string size of permissionName is larger than 256. |

**示例：**

```js
import abilityAccessCtrl from '@ohos.abilityAccessCtrl';
import { BusinessError } from '@ohos.base';

let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
let tokenID: number = 1;

try {
  atManager.checkAccessToken(tokenID, 'ohos.permission.CAMERA').then((data: abilityAccessCtrl.GrantStatus) => {
	console.log(`checkAccessToken success, data->${JSON.stringify(data)}`);
  }).catch((err: BusinessError) => {
	console.log(`checkAccessToken fail, err->${JSON.stringify(err)}`);
  });
} catch(err) {
  console.log(`catch err->${JSON.stringify(err)}`);
}
```

### checkAccessTokenSync<sup>10+</sup>

checkAccessTokenSync(tokenID: number, permissionName: Permissions): GrantStatus;

校验应用是否被授予权限，同步返回结果。当前跨平台上仅支持校验当前应用的自己的授权状态。

**系统能力：** SystemCapability.Security.AccessToken

**参数：**

| 参数名   | 类型                 | 必填 | 说明                                       |
| -------- | -------------------  | ---- | ------------------------------------------ |
| tokenID   |  number   | 是   | 要校验的目标应用的身份标识。当前跨平台上仅支持校验当前应用的自己的授权状态，tokenid可以填任意非0值。           |
| permissionName | Permissions | 是   | 需要校验的权限名称，合法的权限名取值可在[系统权限定义列表](../../../security/security-permission/permission-list.md)中查询。 |

**返回值：**

| 类型          | 说明                                |
| :------------ | :---------------------------------- |
| [GrantStatus](#grantstatus) | 枚举实例，返回授权状态。 |

**错误码：**

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12100001 | The parameter is invalid. The tokenID is 0, or the string size of permissionName is larger than 256. |

**示例：**

```js
let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
let tokenID: number = 1;
let data: abilityAccessCtrl.GrantStatus = atManager.checkAccessTokenSync(tokenID, 'ohos.permission.CAMERA');
console.log(`data->${JSON.stringify(data)}`);
```

### GrantStatus

表示授权状态的枚举。

**系统能力：** SystemCapability.Security.AccessToken

| 名称               |    值 | 说明        |
| ------------------ | ----- | ----------- |
| PERMISSION_DENIED  | -1    | 表示未授权。 |
| PERMISSION_GRANTED | 0     | 表示已授权。 |


### requestPermissionsFromUser<sup>9+</sup>

requestPermissionsFromUser(context: Context, permissionList: Array&lt;Permissions&gt;, requestCallback: AsyncCallback&lt;PermissionRequestResult&gt;) : void;

用于拉起弹框请求用户授权。使用callback异步回调。

**系统能力**: SystemCapability.Security.AccessToken

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| context | Context | 是 | 该参数为请求权限的UIAbility的UIAbilityContext。 |
| permissionList | Array&lt;Permissions&gt; | 是 | 权限名列表，合法的权限名取值可在[系统权限定义列表](../../../security/security-permission/permission-list.md)中查询。 |
| callback | AsyncCallback&lt;[PermissionRequestResult](js-apis-permissionrequestresult.md)&gt; | 是 | 回调函数，返回接口调用是否成功的结果。 |

**错误码：**

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12100001 | The permissionList parameter is invalid. |

**示例：**

  ```js
import abilityAccessCtrl, { Context, PermissionRequestResult } from '@ohos.abilityAccessCtrl';
import { BusinessError } from '@ohos.base';

let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
let context: Context = getContext(this);
try {
  atManager.requestPermissionsFromUser(context, ['ohos.permission.CAMERA'], (err: BusinessError, data: PermissionRequestResult) => {
	if (err) {
	  console.log(`requestPermissionsFromUser err->${JSON.stringify(err)}`);
	} else {
	  console.info('data:' + JSON.stringify(data));
	  console.info('data permissions:' + data.permissions);
	  console.info('data authResults:' + data.authResults);
	}
  });
} catch(err) {
  console.log(`catch err->${JSON.stringify(err)}`);
}
  ```

### requestPermissionsFromUser<sup>9+</sup>

requestPermissionsFromUser(context: Context, permissionList: Array&lt;Permissions&gt;) : Promise&lt;PermissionRequestResult&gt;;

用于拉起弹框请求用户授权。使用promise异步回调。

**系统能力**: SystemCapability.Security.AccessToken

**参数：**

| 参数名 | 类型 | 必填 | 说明 |
| -------- | -------- | -------- | -------- |
| context | Context | 是 | 请求权限的UIAbility的UIAbilityContext。 |
| permissionList | Array&lt;Permissions&gt; | 是 | 需要校验的权限名称，合法的权限名取值可在[系统权限定义列表](../../../security/security-permission/permission-list.md)中查询。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Promise&lt;[PermissionRequestResult](js-apis-permissionrequestresult.md)&gt; | 返回一个Promise，包含接口的结果。 |

**错误码：**

| 错误码ID | 错误信息 |
| -------- | -------- |
| 12100001 | The permissionList parameter is invalid. |

**示例：**

  ```js
import abilityAccessCtrl, { Context, PermissionRequestResult } from '@ohos.abilityAccessCtrl';
import { BusinessError } from '@ohos.base';

let atManager: abilityAccessCtrl.AtManager = abilityAccessCtrl.createAtManager();
let context: Context = getContext(this);
try {
  atManager.requestPermissionsFromUser(context, ['ohos.permission.CAMERA']).then((data: PermissionRequestResult) => {
	console.info('data:' + JSON.stringify(data));
	console.info('data permissions:' + data.permissions);
	console.info('data authResults:' + data.authResults);
  }).catch((err: BusinessError) => {
	console.info('data:' + JSON.stringify(err));
  })
} catch(err) {
  console.log(`catch err->${JSON.stringify(err)}`);
}
  ```
