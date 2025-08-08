# ArkUI-X 跨平台本地数据加载示例参考

**【示例背景】**

在Android平台使用loadData方法，参数传入bundleCodeDir的值，数据资源加载失效，iOS平台正常。

**【示例代码】**

```ts
import { webview } from '@kit.ArkWeb';
import { BusinessError } from '@kit.BasicServicesKit';
import deviceInfo from '@ohos.deviceInfo';

const osFullNameInfo: string = deviceInfo.osFullName;
const platform: string = osFullNameInfo.split(' ')[0];

@Entry
@Component
struct LDWebPage {
  controller: webview.WebviewController = new webview.WebviewController();
  updataContent: string = '<body><div><image src="example.png" alt="image -- end" width="500" height="250"></image></div></body>'

  build() {
    Column() {
      Button('loadData')
        .onClick(() => {
          try {
            let context = this.getUIContext().getHostContext();
            if (platform === 'iOS') {
              this.controller.loadData(this.updataContent, "text/html", "UTF-8", `${context?.bundleCodeDir}/entry/resources/rawfile/`);
            } else if (platform === 'Android') {
              this.controller.loadData(this.updataContent, "text/html", "UTF-8", "file:///android_asset/arkui-x/entry/resources/rawfile/");
            } else {
              // ...
            }
          } catch (error) {
            console.error(`ErrorCode: ${(error as BusinessError).code},  Message: ${(error as BusinessError).message}`);
          }
        })
      Web({ src: 'https://www.example.com', controller: this.controller })
    }
  }
}
```
<b>说明：</b>
loadData方法加载的图片资源（如 example.png）存放在 <b>entry/resources/rawfile</b> 目录下：<br>
![image](../figures/dev-faq-20.png)<br/>