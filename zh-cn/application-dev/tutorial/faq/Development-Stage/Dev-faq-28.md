## ArkUI-X Android应用设置非沉浸式失效问题

### 问题现象

从ArkUI-X SDK 7.0.0.33开始，Android原生侧设置窗口布局为非沉浸式时，布局不避让状态栏与导航栏，组件可能产生与其重叠的情况。

### 问题原因

新版本下默认使能Android全面屏模式和避让区默认避让，避让区信息获取依赖androidx.core接口，需开发者在应用侧显式引入依赖。未引入上述依赖时，设置窗口布局为非沉浸式不会计算绘制避让区。

### 解决办法

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
