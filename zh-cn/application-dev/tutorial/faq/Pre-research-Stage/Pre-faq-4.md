# ArkUI-X是否支持应用跨平台和鸿蒙的混合开发

<img src="../figures/Pre-faq-4.png" width="600">

跨平台工程中支持跨平台模块和鸿蒙模块共存，Build Hap(s)情况下，所有的模块都会进行鸿蒙态编译，不会区分是否是跨平台模块；而Build APP(s)情况下，同样会先进行所有模块的鸿蒙态编译，但**只有标记了是跨平台模块的，才会最终打包编译到安卓或者iOS的最终应用形态。**

1）应用代码集成ArkUI-X框架底座，以安卓为例，重写Application、继承StageActivity等，参考[通过Stage模型开发Android端应用指南](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/start-with-ability-on-android.md)，[通过Stage模型开发iOS端应用指南](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/start-with-ability-on-ios.md)

2）使用DevEco Studio正常开发ets代码，写鸿蒙原生应用

3）在HarmonyOS Next设备上正常签名、安装、调试

4）使用DevEco Studio->Build APP(s)拷贝跨平台模块产物和SDK中依赖项，参考[**Android应用工程集成ArkUI-X SDK**](./pre-faq-2.md)，[**Android应用工程集成ArkUI-X应用编译产物**](./pre-faq-2.md)

5）在Android、iOS应用工程整体打包编译，在对应设备上开发联调