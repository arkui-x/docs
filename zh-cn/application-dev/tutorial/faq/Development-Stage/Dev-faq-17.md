# ArkUI-X 解决useNormalizedOHMUrl设置为true时Android、iOS应用闪退或白屏

**【问题现象】**

工程中build-profile.json5文件useNormalizedOHMUrl设置为true，安装的APP在Android和iOS端闪退或白屏。

**【问题原因】**

useNormalizedOHMUrl设置为true时，开发者构建app，并在Android和iOS端安装app，由于IDE没有自动拷贝pkgContextInfo.json文件，导致框架异常。

**【解决方法】**

在工程构建Hap后，按下图所示找到entry-default-unsigned.hap

![image](../figures/dev-faq-17-1.png)<br/>

将文件中的pkgContextInfo.json文件拷贝对应目录下，重新构建app。

目录位置：

> Android：.arkui-x/android/app/src/main/assets/arkui-x/entry

> iOS：.arkui-x/ios/arkui-x/entry

**【注意事项】**

DevEco Studio 5.1.1 已默认集成 hvigor-ohos-arkui-x-plugin 4.19.5，其最低兼容版本为 4.19.2。

如果您正在打开或维护旧工程，请先升级至 4.19.2 及以上版本(4.20.2 为特殊版本未适配该功能)，以确保确保该问题解决。

hvigor-arkui-x-plugin 版本号查看位置如下图所示：

![image](../figures/dev-faq-17-2.png)<br/>

**【解决计划】**

该问题已在DevEco Studio 5.1.1版本中修改解决。请下载更新 [DevEco Studio 5.1.1版本](https://developer.huawei.com/consumer/cn/download/deveco-studio)。