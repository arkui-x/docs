# 窗口子系统变更说明

## cl.window.1 ArkUI-X应用Android原生侧依赖变更说明

**访问级别**

其它

**变更原因**

新版本下默认使能Android全面屏模式和避让区默认避让，避让区信息获取依赖androidx.core接口，需开发者在应用侧显式引入依赖。

**变更影响**

此变更涉及应用适配。

- 变更前：ArkUI-X Android应用设置为非全屏，未依赖androidx.core接口，应用界面内容默认会避开系统UI的显示区域，包括状态栏、导航区域。

- 变更后：ArkUI-X Android应用默认设置为全屏，避让区信息获取需依赖androidx.core接口，需开发者在应用侧显式引入依赖。

**起始 API Level**

不涉及API

**变更发生版本**

从ArkUI-X SDK 7.0.0.33开始。

**变更的接口/组件**

不涉及具体接口/组件

**适配指导**

- 需在Android应用工程的gradle.properties文件中使能AndroidX：

```
android.useAndroidX=true
```

- 需在Android应用的build.gradle文件增加依赖项：

```
dependencies {
    ...
    implementation  'androidx.core:core:1.8.0'
    ...
}
```

## cl.window.2 ArkUI-X应用二次避让问题变更说明

**访问级别**

其它

**变更原因**

新版本下ArkUI提供避让区默认避让能力，不再需要开发者单独处理避让逻辑。

**变更影响**

此变更涉及应用适配。

- 变更前：应用全屏时，默认不避让系统导航栏、状态栏，开发者需在应用中通过Padding等方式处理避让逻辑。

- 变更后：ArkUI提供默认避让能力，开发者应用中若有通过Padding等方式处理避让的逻辑，会引发二次避让现象。

**起始 API Level**

不涉及API

**变更发生版本**

从ArkUI-X SDK 7.0.0.33开始。

**变更的接口/组件**

不涉及具体接口/组件

**适配指导**

- 方案一 开发者可取消原有应用中通过Padding等方式额外设置的避让内容。
  
- 方案二 开发者可在EntryAbility.ets中调用 setWindowLayoutFullScreen(isLayoutFullScreen: boolean): Promise<void> 接口，并且将isLayoutFullScreen设置为true，设置窗口沉浸式布局。
  
```
// EntryAbility.ets
import { UIAbility } from '@kit.AbilityKit';
import { window } from '@kit.ArkUI';

export default class EntryAbility extends UIAbility {
  // ...
  onWindowStageCreate(windowStage: window.WindowStage): void {
    windowStage.loadContent('pages/Index', async (err) => {
      if (err.code) {
        return;
      }

      try {
        const mainWindow: window.Window = windowStage.getMainWindowSync();
        await mainWindow.setWindowLayoutFullScreen(true);
      } catch (e) {
        console.error(`Failed to set status bar to invisible`);
      }
    });
  }
}
// ...
```

## cl.window.3 Android原生侧应用避让问题变更说明

**访问级别**

其它

**变更原因**

新版本下默认设置Android侧页面为全屏模式，其它非ArkUI页面受此影响，需要单独设置避让区。

**变更影响**

此变更涉及应用适配。

- 变更前：ArkUI-X Android应用设置为非全屏，应用内非ArkUI页面内容默认会避开系统UI的显示区域，包括状态栏、导航区域。

- 变更后：ArkUI-X Android应用默认设置为全屏，应用内非ArkUI页面内容可能会与系统UI的显示区域重叠，需要单独设置避让区。

**起始 API Level**

不涉及API

**变更发生版本**

从ArkUI-X SDK 7.0.0.33开始。

**变更的接口/组件**

不涉及具体接口/组件

**适配指导**

- 方案一 在对应页面的java文件内增加以下内容

```
...
import androidx.core.graphics.Insets;
...
protected void onCreate(Bundle savedInstanceState){
    ...
    View rootView = findViewById(R.id.rootLayout);
    ViewCompat.setOnApplyWindowInsetsListener(rootView, new OnApplyWindowInsetsListener() {
        @Override
        public WindowInsetsCompat onApplyWindowInsets(View v, WindowInsetsCompat insets) {
            Insets systemBars = insets.getInsets(
                    WindowInsetsCompat.Type.systemBars()
            );
            v.setPadding(
                    systemBars.left, systemBars.top, systemBars.right, systemBars.bottom
            );
            return insets;
        }
    });
    ViewCompat.requestApplyInsets(rootView);
    ...
}
```
并在对应页面的xml文件内增加页面id
```
<LinearLayout 
    ...
    android:id="@+id/rootLayout"
    ...
>
```

- 方案二 在对应页面的xml文件内增加以下内容

```
<LinearLayout 
    ...
    android:fitsSystemWindows="true"
    ...
>
```

## cl.window.4 ArkUI-X Android应用导航栏、状态栏颜色与背景颜色重叠问题修复指导说明

**访问级别**

其它

**变更原因**

新版本下ArkUI提供默认避让能力，若开发者设置了如"Theme.Light.NoTitleBar"等浅色主题时，避让区域背景颜色可能会与避让区UI颜色相同，导致视觉上难以看清避让区UI信息。

**变更影响**

此变更涉及应用适配。

- 变更前：默认未设置全屏，ArkUI-X Android应用界面在导航栏和状态栏之间进行绘制，绘制区域不重叠。

- 变更后：默认为全屏，ArkUI计算绘制的避让区域背景默认为白色，导航栏和状态栏的字体及符号颜色也为白色，需进行适配以在视觉上产生反差。

**起始 API Level**

不涉及API

**变更发生版本**

从ArkUI-X SDK 7.0.0.33开始。

**变更的接口/组件**

不涉及具体接口/组件

**适配指导**

- 方案一 修改Android侧 app/manifests/AndroidManifest.xml文件，可使导航栏符号变为灰色。

```
<application
    ···
    android:theme="@android:style/Theme.Material.Light.NoActionBar"
    ···
>
```

- 方案二 可在Android侧创建app/src/main/res/values/themes.xml文件，并自定义主题。深色主题下启动应用，则设置"android:windowLightStatusBar"为false。

```
<?xml version="1.0" encoding="utf-8"?>
<resources>

    <style name="myTheme" parent="@android:style/Theme.Material.Light.NoActionBar">
        <item name="android:windowLightStatusBar">true</item>
    </style>
</resources>
```

并且在app/manifests/AndroidManifest.xml中使用自定义主题。
```
<application
    ···
    android:theme="@style/myTheme"
    ···
>
```
