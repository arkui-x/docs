# ArkTS通过UIContext获取组件能力，运行态功能不可用

**问题描述**
参考如下ArkTS写法来使用PromptAction功能，安卓和iOS上运行态弹窗功能不可用：
```typescript
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct Index {
  build() {
    Row() {
      Column() {
        Button()
          .onClick(() => {
            let uiContext = this.getUIContext();
            let promptAction = uiContext.getPromptAction();
            try {
              promptAction.showToast({
                message: 'Message Info',
                duration: 2000
              });
            } catch (error) {
              let message = (error as BusinessError).message;
              let code = (error as BusinessError).code;
              console.error(`showToast args error code is ${code}, message is ${message}`);
            };
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```


**具体报错日志**
```shell
[native_module_manager.cpp(EmplaceModuleLib)] modulekey is 'promptAction'

[native_module_manager.cpp(FindNativeModuleByDisk)] try to load abc module path: /private/var/containers/Bundle/Application/98628FE0-A19E-42AE-AEC7-EE3B79EA9508/app.app/arkui-x/systemres/abc/promptaction.abc

[native_module_manager.cpp(GetFileBuffer)] /private/var/containers/Bundle/Application/98628FE0-A19E-42AE-AEC7-EE3B79EA9508/app.app/arkui-x/systemres/abc/promptaction.abc is not existed.

[native_module_manager.cpp(FindNativeModuleByDisk)] First attempt: 
Second attempt: 
try to load abc file from /private/var/containers/Bundle/Application/98628FE0-A19E-42AE-AEC7-EE3B79EA9508/app.app/arkui-x/systemres/abc/promptaction.abc failed

[native_module_manager.cpp(LoadNativeModule)] load native module failed

[ark_native_engine.cpp(operator())] First attempt: 
Second attempt: 
try to load abc file from /private/var/containers/Bundle/Application/98628FE0-A19E-42AE-AEC7-EE3B79EA9508/app.app/arkui-x/systemres/abc/promptaction.abc failed

[ERROR] [js_console_log.cpp(103)] showToast args error code is undefined, message is Cannot read property showToast of undefined
```

**解决办法**
uiContext可以获取很多的组件能力并使用，当前跨平台的拷贝依赖库机制暂无法支持该使用方式，所以会漏拷贝库导致功能不可用，应用侧可以通过显式import组件能力的方式来规避解决该问题，以promptAction为例：

```typescript
import { PromptAction } from '@kit.ArkUI';
import { BusinessError } from '@kit.BasicServicesKit';

@Entry
@Component
struct Index {
  build() {
    Row() {
      Column() {
        Button()
          .onClick(() => {
            let uiContext = this.getUIContext();
            let promptAction = uiContext.getPromptAction();
            try {
              promptAction.showToast({
                message: 'Message Info',
                duration: 2000
              });
            } catch (error) {
              let message = (error as BusinessError).message;
              let code = (error as BusinessError).code;
              console.error(`showToast args error code is ${code}, message is ${message}`);
            };
          })
      }
      .width('100%')
    }
    .height('100%')
  }
}
```

