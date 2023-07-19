# @ohos.abilityAccessCtrl (Application Access Control)

The **abilityAccessCtrl** module provides APIs for application permission management, including authentication, authorization, and revocation.

> **NOTE**
>
> The initial APIs of this module are supported since API version 8. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```js
import abilityAccessCtrl from '@ohos.abilityAccessCtrl'
```

## abilityAccessCtrl.createAtManager

createAtManager(): AtManager

Creates an **AtManager** instance, which is used for application access control.

**System capability**: SystemCapability.Security.AccessToken


**Return value**

| Type| Description|
| -------- | -------- |
| [AtManager](#atmanager) | **AtManager** instance created.|

**Example**

```js
let atManager = abilityAccessCtrl.createAtManager();
```

## AtManager

Provides APIs for application access control.

### checkAccessToken<sup>9+</sup>

checkAccessToken(tokenID: number, permissionName: Permissions): Promise&lt;GrantStatus&gt;

Checks whether a permission is granted to an application. This API uses a promise to return the result. Currently, this API works only for the calling application in the ArkUI-X project.

**System capability**: SystemCapability.Security.AccessToken

**Parameters**
| Name  | Type                | Mandatory| Description                                      |
| -------- | -------------------  | ---- | ------------------------------------------ |
| tokenID   |  number   | Yes  | Application token ID, which cannot be 0. Currently, this API works only for the calling application in the ArkUI-X project. |
| permissionName | Permissions | Yes  | Permission to check. For details about the permissions, see the [Application Permission List](../../../security/security-permission/permission-list.md).|

**Return value**

| Type         | Description                               |
| :------------ | :---------------------------------- |
| Promise&lt;GrantStatus&gt; | Promise used to return the permission grant state.|

**Error codes**

| ID| Error Message|
| -------- | -------- |
| 12100001 | The parameter is invalid. The tokenID is 0, or the permissionName exceeds 256 bytes. |

**Example**

```js
import abilityAccessCtrl from '@ohos.abilityAccessCtrl';

let atManager = abilityAccessCtrl.createAtManager();
let tokenID = 1;
try {
    atManager.checkAccessToken(tokenID, 'ohos.permission.GRANT_SENSITIVE_PERMISSIONS').then((data) => {
        console.log(`checkAccessToken success, data->${JSON.stringify(data)}`);
    }).catch((err) => {
        console.log(`checkAccessToken fail, err->${JSON.stringify(err)}`);
    });
} catch(err) {
    console.log(`catch err->${JSON.stringify(err)}`);
}
```

### checkAccessTokenSync<sup>10+</sup>

checkAccessTokenSync(tokenID: number, permissionName: Permissions): GrantStatus;

Checks whether a permission is granted to an application. This API returns the result synchronously. Currently, this API works only for the calling application in the ArkUI-X project.

**System capability**: SystemCapability.Security.AccessToken

**Parameters**

| Name  | Type                | Mandatory| Description                                      |
| -------- | -------------------  | ---- | ------------------------------------------ |
| tokenID   |  number   | Yes  | Application token ID, which cannot be 0. Currently, this API works only for the calling application in the ArkUI-X project. |
| permissionName | Permissions | Yes  | Permission to check. For details about the permissions, see the [Application Permission List](../../../security/security-permission/permission-list.md).|

**Return value**

| Type         | Description                               |
| :------------ | :---------------------------------- |
| [GrantStatus](#grantstatus) | Permission grant state.|

**Error codes**

| ID| Error Message|
| -------- | -------- |
| 12100001 | The parameter is invalid. The tokenID is 0, or the permissionName exceeds 256 bytes. |

**Example**

```js
let atManager = abilityAccessCtrl.createAtManager();
let tokenID = 1;
let data = atManager.checkAccessTokenSync(tokenID, 'ohos.permission.GRANT_SENSITIVE_PERMISSIONS');
console.log(`data->${JSON.stringify(data)}`);
```

### GrantStatus

Enumerates the permission grant states.

**System capability**: SystemCapability.Security.AccessToken

| Name              |    Value| Description       |
| ------------------ | ----- | ----------- |
| PERMISSION_DENIED  | -1    | Permission denied.|
| PERMISSION_GRANTED | 0     | Permission granted.|

### requestPermissionsFromUser<sup>9+</sup>

requestPermissionsFromUser(context: Context, permissionList: Array&lt;Permissions&gt;, requestCallback: AsyncCallback&lt;PermissionRequestResult&gt;) : void;

Requests user authorization in a dialog box opened by a UIAbility. This API uses an asynchronous callback to return the result.

**System capability**: SystemCapability.Security.AccessToken

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| context | Context | Yes| Context of the UIAbility.|
| permissionList | Array&lt;Permissions&gt; | Yes| Permissions requested. For details about the permissions, see the [Application Permission List](../../../security/security-permission/permission-list.md).|
| callback | AsyncCallback&lt;[PermissionRequestResult](js-apis-permissionrequestresult.md)&gt; | Yes| Callback invoked to return the result.|

**Error codes**

| ID| Error Message|
| -------- | -------- |
| 12100001 | The parameter is invalid. The context is invalid when it does not belong to the application itself. |

**Example**

  ```js
import abilityAccessCtrl from '@ohos.abilityAccessCtrl';
let atManager = abilityAccessCtrl.createAtManager();
try {
    atManager.requestPermissionsFromUser(this.context, ['ohos.permission.CAMERA'], (err, data)=>{
        console.info('data:' + JSON.stringify(data));
        console.info('data permissions:' + data.permissions);
        console.info('data authResults:' + data.authResults);
    });
} catch(err) {
    console.log(`catch err->${JSON.stringify(err)}`);
}
  ```

### requestPermissionsFromUser<sup>9+</sup>

requestPermissionsFromUser(context: Context, permissionList: Array&lt;Permissions&gt;) : Promise&lt;PermissionRequestResult&gt;;

Requests user authorization in a dialog box opened by a UIAbility. This API uses a promise to return the result.

**System capability**: SystemCapability.Security.AccessToken

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| context | Context | Yes| Context of the UIAbility.|
| permissionList | Array&lt;Permissions&gt; | Yes| Permissions requested. For details about the permissions, see the [Application Permission List](../../../security/security-permission/permission-list.md).|

**Return value**

| Type| Description|
| -------- | -------- |
| Promise&lt;[PermissionRequestResult](js-apis-permissionrequestresult.md)&gt; | Promise used to return the result.|

**Error codes**

| ID| Error Message|
| -------- | -------- |
| 12100001 | The parameter is invalid. The context is invalid when it does not belong to the application itself. |

**Example**

  ```js
import abilityAccessCtrl from '@ohos.abilityAccessCtrl';
let atManager = abilityAccessCtrl.createAtManager();
try {
    atManager.requestPermissionsFromUser(this.context, ['ohos.permission.CAMERA']).then((data) => {
        console.info('data:' + JSON.stringify(data));
        console.info('data permissions:' + data.permissions);
        console.info('data authResults:' + data.authResults);
    }).catch((err) => {
        console.info('data:' + JSON.stringify(err));
    })
} catch(err) {
    console.log(`catch err->${JSON.stringify(err)}`);
}
  ```
