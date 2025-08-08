# ArkUI-X上如何使用非原生系统字体

两种方式

**非全局，使用fontFamily属性**

在对应页面的aboutToAppear中注册字体，在需要切换字体的组件中使用fontFamily属性。

参考开发者文档：https://docs.openharmony.cn/pages/v5.1/zh-cn/application-dev/reference/apis-arkui/js-apis-font.md

```java
aboutToAppear() {
    // familyName和familySrc都支持系统Resource
    this.getUIContext().getFont().registerFont({
      familyName: $r('app.string.font_name'),
      familySrc: $r('app.string.font_src')
    })

    // familySrc支持RawFile
    this.getUIContext().getFont().registerFont({
      familyName: 'mediumRawFile',
      familySrc: $rawfile('font/medium.ttf')
    })
  }

  build() {
    Column() {
      Text(this.message)
        .align(Alignment.Center)
        .fontSize(20)
        .fontFamily('mediumRawFile') // mediumRawFile：注册自定义字体的名字（$r('app.string.font_name')等已注册字体也能正常使用）
    }.width('100%')
  }

 ```

**全局，使用ApplicationContext.setFont**

在应用入口的Ability中，在onWindowStageCreate()生命周期中通过loadContent方法加载页面之后，先注册字体，再通过getApplicationContext().setFont使字体全局生效。

参考开发者文档：https://docs.openharmony.cn/pages/v5.1/zh-cn/application-dev/reference/apis-ability-kit/js-apis-inner-application-applicationContext.md

```java
export default class EntryAbility extends UIAbility {
  ...
  onWindowStageCreate(windowStage: window.WindowStage): void {
    // Main window is created, set main page for this ability
    hilog.info(DOMAIN, 'testTag', '%{public}s', 'Ability onWindowStageCreate');

    windowStage.loadContent('pages/Index', (err) => {
      if (err.code) {
        hilog.error(DOMAIN, 'testTag', 'Failed to load the content. Cause: %{public}s', JSON.stringify(err));
        return;
      }
      let uiContext :UIContext = windowStage.getMainWindowSync().getUIContext();
      this.setFont(uiContext)
      hilog.info(DOMAIN, 'testTag', 'Succeeded in loading the content.');
    });
  }

  setFont(context:UIContext) {
    context.getFont.registerFont({
      familyName: 'Medium',
      familySrc: $rawfile('font/HarmonyOS_SansSC_Medium.ttf')
    })
    getContext().getApplicationContext().setFont('Medium');
  }
  ...
}

 ```


 注意：需要确认拷贝了libfont.so或libfont.xcframework


 其他：鸿蒙字体的下载路径：https://developer.huawei.com/consumer/cn/doc/design-guides-V1/font-0000001157868583-V1#section136mcpsimp
