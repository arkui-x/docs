# PermissionRequestResult

权限请求结果对象，在调用[requestPermissionsFromUser](js-apis-abilityAccessCtrl.md#requestpermissionsfromuser9)申请权限时返回此对象表明此次权限申请的结果。

> **说明：**
> 
> 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。  
> 本模块接口仅可在Stage模型下使用。

## 属性

**系统能力**：以下各项对应的系统能力均为SystemCapability.Security.AccessToken

| 名称 | 类型 | 只读 | 可选 | 说明 | Android平台 | iOS平台 |
| -------- | -------- | -------- | -------- | -------- | -------- | -------- |
| permissions | Array&lt;string&gt; | 是 | 否 | 用户传入的权限。<br> **原子化服务API**：从API version 18开始，该接口支持在原子化服务中使用。 | 支持 | 支持 |
| authResults | Array&lt;number&gt; | 是 | 否 | 相应请求权限的结果：<br>- -1：未授权，表示权限已设置，无需弹窗，需要用户在"设置"中修改。<br>- 0：已授权。<br>- 2：未授权，表示请求无效，可能原因有：<br>  -未在设置文件中声明目标权限。<br>  -权限名非法。<br> **原子化服务API**：从API version 18开始，该接口支持在原子化服务中使用。 | 支持 | 支持 |
| errorReasons<sup>18+</sup> | Array&lt;number&gt; | 是 | 是 | 申请相应权限的返回值说明：<br>0：本次申请合法。<br>1：权限名非法。<br>2：权限未声明。<br>3：不满足对应权限的申请条件。<br>4：用户未同意隐私声明。<br>5：该权限不支持通过权限弹窗进行申请。<br>6：该权限已被系统策略强制管控，无法通过权限弹窗申请。<br>12：服务异常。<br> **原子化服务API**：从API version 18开始，该接口支持在原子化服务中使用。 | 不支持 | 不支持 |
## 使用说明

通过atManager实例来获取。

**示例：**
```ts
import { abilityAccessCtrl, Context, PermissionRequestResult, common } from '@kit.AbilityKit';
import { BusinessError } from '@kit.BasicServicesKit';

let atManager = abilityAccessCtrl.createAtManager();
try {
  let context: Context = this.getUIContext().getHostContext() as common.UIAbilityContext;
  atManager.requestPermissionsFromUser(context, ["ohos.permission.CAMERA"]).then((data) => {
    console.info('data:' + JSON.stringify(data));
    console.info('data permissions:' + data.permissions);
    console.info('data authResults:' + data.authResults);
  }).catch((err: BusinessError) => {
      console.error("data:" + JSON.stringify(err));
  })
} catch(err) {
  console.error(`catch err->${JSON.stringify(err)}`);
}
```