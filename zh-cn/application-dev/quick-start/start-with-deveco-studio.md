# ArkUI-X之初体验

## 开发跨平台应用


### 创建工程

在DevEco Studio中导入ArkUI-X Sample，快速创建跨平台工程。

1. 点击File > New > Import > Import Sample。
   ![import_sample](figures/import_sample.png)

2. 在左上方的下拉框中选择OpenHarmony，选择ArkUI-X/HelloWorld，点击**Next**。

3. 在工程配置页面，填写Project name和Project location，点击**Finish**，等待Sample工程导入完成。

### 编译构建生成跨平台应用

DevEco Studio可打包生成不同平台的应用包。

在主菜单栏，单击**Build &gt; Build Hap(s)/APP(s) &gt; Build APP(s)**。
   ![zh-cn_image_0000001580152768](figures/zh-cn_image_0000001580152768.png)

编译后的ArkTS代码、资源和平台胶水代码已生成到Android和iOS应用工程中，后续安装、运行和调试请使用Android Studio和Xcode，也可使用[ACE Tools](start-with-ace-tools.md#应用运行)。