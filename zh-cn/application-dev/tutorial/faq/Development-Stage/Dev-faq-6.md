# ArkUI-X(Android)如何设置沉浸式效果

**【问题描述】**
在ArkTS侧，组件设置expandSafeArea属性，实际安卓运行不能达到沉浸式效果

**【解决方案】**
在Ability的onWindowStageCreate方法中，使用setWindowLayoutFullScreen方法达到沉浸式效果，代码示例：

```typescript
  let windowClass: window.Window | undefined = undefined;
  windowStage.getMainWindow((err: BusinessError, data) => {
    const errCode: number = err.code;
    if (errCode) {
      console.error(`Failed to obtain the main window. Cause code: ${err.code}, message: ${err.message}`);
      return;
    }
    windowClass = data;
    let isLayoutFullScreen = true;
    try {
      let promise = windowClass.setWindowLayoutFullScreen(isLayoutFullScreen);
      promise.then(() => {
        console.info('Succeeded in setting the window layout to full-screen mode.');
      }).catch((err: BusinessError) => {
        console.error(`Failed to set the window layout to full-screen mode. Cause code: ${err.code}, message: ${err.message}`);
      });
    } catch (exception) {
      console.error(`Failed to set the window layout to full-screen mode. Cause code: ${exception.code}, message: ${exception.message}`);
    }
  });
```

