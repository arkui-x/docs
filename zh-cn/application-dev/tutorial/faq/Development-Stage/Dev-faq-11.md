# 稳定性问题自排查指南

## 应用闪退问题

**问题一：找不到native方法实现**

**关键报错日志：**

```shell
java.lang.UnsatisfiedLinkError: No implementation found for ...
```

**具体示例：**

```shell
java.lang.UnsatisfiedLinkError: No implementation found for void ohos.stage.ability.adapter.StageActivityDelegate.nativeAttachStageActivity(java.lang.String, ohos.stage.ability.adapter.StageActivity) (tried Java_ohos_stage_ability_adapter_StageActivityDelegate_nativeAttachStageActivity and Java_ohos_stage_ability_adapter_StageActivityDelegate_nativeAttachStageActivity__Ljava_lang_String_2Lohos_stage_ability_adapter_StageActivity_2)
at ohos.stage.ability.adapter.StageActivityDelegate.nativeAttachStageActivity(Native Method)
at ohos.stage.ability.adapter.StageActivityDelegate.attachStageActivity(StageActivityDelegate.java:47)
at ohos.stage.ability.adapter.StageActivity.onCreate(StageActivity.java:106)
```
**【问题原因】**

运行时找不到对应的C++实现方法，上述这个示例日志中，StageActivity启动时，调用nativeAttachStageActivity时异常，未找到对应的Native实现(.so或者.xcframework)导致的Crash。
问题原因是在StageActivity页面拉起时，未进行ArkUI-X跨平台框架初始化。


**【解决办法】**

ArkUI-X跨平台初始化的方法：
以Android为例：
方法1：应用Application继承ArkUI-X框架提供的StageApplication：
```Java
public class MyApplication extends StageApplication
```
方法2：
在Application初始化阶段或者在拉起StageActivity之前调用如下方法，初始化ArkUI-X跨平台框架：
```C++
StageApplicationDelegate appDelegate = new StageApplicationDelegate();
appDelegate.initApplication(this);
```



**问题二：依赖预置hsp的模块不支持跨平台**

**关键报错日志：**

```shell
resolveBufferCallback get hsp buffer failed...
```

**具体示例：**

```shell
Abort message: '[default] [LoadJSPandaFile:106] resolveBufferCallback get hsp buffer failed, hsp path:/data/storage/el1/bundle/com.huawei.hmsapp.hiai.hsp/interactivelivenessHsp/interactivelivenessHsp/ets/modules.abc, errorMsg:modulePath:'
```

**解决办法：**

这个异常是在ArkTS代码中import { interactiveLiveness } from "@kit.VisionKit"，会加载interactiveLiveness API实现的hsp，该API属于HarmonyOS闭源SDK，不支持跨平台，可以参考[部分API不支持跨平台场景的解决](./Dev-faq-10.md)

**问题三：严格模式下跨平台漏拷贝pkgContextinfo.json**

**关键报错日志**：
```shell
Throw error: Cannot find module ‘xxx’，which is application Entry Point
```

**具体示例：**

```shell
D ArkCompiler: [default] Throw error: Cannot find module 'com.LanHaiBank.huawei/Cross_platform/ets/arkuixability/ArkUiXAbility' , which is application Entry Point
D ArkCompiler: [default] 
E ArkCompiler: [default] Cannot execute module buffer file 'Cross_platform/ets/arkuixability/ArkUiXAbility.abc
```

**解决办法：**

将跨平台module A编译生成的hap包，先改成.zip格式，再解压zip包，将压缩包中的pkgContextinfo.json文件拷贝到.arkui-x/android/app/src/main/assets/arkui-x/A对应目录下。

![image](../figures/pkgContextinfo.png)


**问题四：abc镜像文件不匹配导致推包crash**

**关键报错日志**：

```shell
Unable to open file 'xxx' with abc file version 12.0.1.0. Maximum supported abc file version on the current system image is 12.0.0.0.
Please upgrade the system image or use former version of SDK tools to generate abc files
```

**问题原因**：

使用高API Version的HarmonyOS NEXT SDK编译生成了abc文件，但ArkUI-X SDK集成的方舟镜像.so库版本较低，所以无法进行高版本abc文件的解析，常见于应用的abc文件编译和arkui-x SDK分开集成的场景。

**解决办法**：

要保证使用的ArkUI-X SDK的API Version大于等于HarmonyOS NEXT SDK的API Version。可以通过升级最新IDE版本，使用当前已商发的最新ArkUI-X版本：
https://developer.huawei.com/consumer/cn/download/

## 应用白屏问题

#### **问题一：模块运行异常**

**关键日志：**
```shell
ArkCompiler: [default] Throw error: Cannot read property ...
```

**具体示例：**
**场景一：**未集成对应的动态库

```shell
08-14 08:55:20.603  4354  4354 W ArkCompiler: [default] GetNativeModuleValue:185 GetNativeModuleValueByIndex: currentModule /data/storage/el1/bundle/entry/ets/modules.abc, find requireModule @ohos:intl failed
08-14 08:55:20.603  4354  4354 D ArkCompiler: [default] Throw error: Cannot read property NumberFormat of undefined
08-14 08:55:20.603  4354  4354 D ArkCompiler: [default] at CategoryPage (entry/src/main/ets/pages/CategoryPage.ets:47:37)
08-14 08:55:20.603  4354  4354 D ArkCompiler:at anonymous (entry/src/main/ets/pages/CategoryPage.ets:429:26)
08-14 08:55:20.603  4354  4354 E ArkCompiler: [default] Call:2170 occur exception need return
```

**问题原因：**
.arkui-x/android/app/libs目录下缺少库libintl.so

**解决办法：**
查看应用是否是通过DevEco Studio编译实现的.abc生成和.so自动拷贝，如果是，IDE没帮忙拷贝需要的二进制库，请[社区提issue](https://gitcode.com/org/arkui-x/issues)，并提供环境信息及复现Demo告知SDK，临时规避方案可以从ArkUI-X SDK目录中拷贝libintl.so到.arkui-x/android/app/libs


**场景二：**插件化场景，被依赖的so库无法指定到沙箱目录加载，需要在apk预置

```shell
09-12 19:20:28.133 6265 6265 W NAPI : [native_module_manager.cpp(LoadModuleLibrary)] dlopen failed: dlopen failed: library "libcrypto_openssl.so" not found
09-12 19:20:28.133 6265 6265 W ArkCompiler: [default] GetNativeModuleValue:185 GetNativeModuleValueByIndex: currentModule /data/storage/el1/bundle/arkuix/ets/modules.abc, find requireModule @ohos:util failed
09-12 19:20:28.133 6265 6265 D ArkCompiler: [default] Throw error: Cannot read property LRUCache of undefined
```

**问题原因：**
ArkTS代码中使用了类似“import XXX from '@ohos.util.xxx'”，libutil.so作为该ohos api一级依赖的二进制库支持动态化下载，然而libcrypto_openssl.so被libutil.so依赖，libcrypto_openssl.so这种二级依赖的二进制库必须预置在apk中，具体动态化细节使用可以参考：[跨平台动态化开发指南](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/dynamic-introduction.md)

**解决办法：**
libcrypto_openssl.so作为二级依赖的二进制库需要预置到apk中，不支持动态下载。

#### **问题二：应用ArkTS代码未做异常保护**

**具体示例：**
例如：在aboutToappear方法中使用了未赋值的对象test[0]

```typescript
aboutToAppear(): void {
 let my=this.test[0].toString();
}
```

具体报错日志如下：
```shell
08-14 10:38:17.485 25500 25500 D ArkCompiler: [default] Throw error: Cannot read property toString of undefined
08-14 10:38:17.485 25500 25500 D ArkCompiler: [default] at aboutToAppear (entry/src/main/ets/pages/CategoryPage.ets:138:31)
08-14 10:38:17.485 25500 25500 E ArkCompiler: [default] Call:2170 occur exception need return
08-14 10:38:17.485 25500 25500 E ArkCompiler: [default] Pending exception before ExecutePendingJob called, in line:3518, exception details as follows:
08-14 10:38:17.485 25500 25500 E ArkCompiler: [default] TypeError: Cannot read property toString of undefined
```

**问题原因：**
在ability生命周期方法中，应用代码未做异常保护，会导致白屏

**解决办法：**
应用ArkTS代码需要做异常保护


#### **问题三：Android P以上环境web组件加载失败**

**具体示例：**
web组件加载失败，也会导致白屏。

具体报错日志如下：
```shell
06-07 10:55:36.858 1759 1759 E Ace  : [ace_resource_register.cpp(CreateResource)-(1)] AceResourceRegister CreateResource: has exception
06-07 10:55:36.859 1759 1759 W System.err: java.lang.RuntimeException: Using WebView from more than one process at once with the same data directory is not supported. https://crbug.com/558377 : Current process XXX (pid 1759), lock owner  unknown
06-07 10:55:36.859 1759 1759 W System.err: at org.chromium.android_webview.AwDataDirLock.b(HwWebview-12.0.4.307.4397:27)
06-07 10:55:36.860 1759 1759 W System.err: at org.chromium.android_webview.AwBrowserProcess.k(HwWebview-12.0.4.307.4397:5)
```

**问题原因：**
Android P以上手机，不可多进程使用同一个目录webView，需要为不同进程webView设置不同目录

**解决办法：**在应用android代码中设置不同目录

```java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.P) {
    String processName = getProcessName(this);
    String packageName = this.getPackageName();
    if (!packageName.equals(processName)) {
        WebView.setDataDirectorySuffix(processName);
    }
}
```

#### **问题四：apk构建异常，assets漏拷贝对应模块**

apk（非动态化加载）构建时，assets目录下没有拷贝对应的模块文件。

**具体报错日志：**

```shell
2024-09-03 16:06:44.827 2901-2901 Ace  com.huawei.hmos.world I [app_main.cpp(72)] AppMain schedule launch application.
2024-09-03 16:06:44.827 2901-2901 Ace  com.huawei.hmos.world I [stage_asset_provider.cpp(101)-(-1:-1:undefined)] Get module json buffer list
2024-09-03 16:06:44.827 2901-2901 Ace  com.huawei.hmos.world I [app_main.cpp(82)] module list size: 0
2024-09-03 16:06:44.827 2901-2901 Ace  com.huawei.hmos.world E [bundle_container.cpp(132)] bundleInfo_ is nullptr
2024-09-03 16:06:44.827 2901-2901 Ace  com.huawei.hmos.world I [application.cpp(41)] Application::SetApplicationContext
2024-09-03 16:06:44.827 2901-2901 Ace  com.huawei.hmos.world E [app_main.cpp(190)] applicationInfo or bundleInfo is nullptr.
```

**问题原因：**
编译完成的.arkui-x/android/app/src/main/assets目录下需要包含所有标记成跨平台模块的hap、hsp产物文件夹，主要包括modules.abc和模块资源文件，应用侧需要自排查是否已经全部生成

**解决办法：**
先删除.arkui-x/android/app/src/main/assets目录下全部内容，执行Clean Project动作，用DevEco Studio或ACE Tools重新构建apk或者iOS应用


#### **问题五：不支持跨平台，标签误标**

**关键日志**

例1：
ArkCompiler: [default] [GetNativeOrCjsExports:50] Load native module failed, so is xxx
ArkCompiler: [default] Throw error: the requested module 'xxx' does not provide an export name 'xxx' which imported by 'xxx'

例2：
ArkCompiler: [default] ReferenceError: xxx is not defined

**问题原因：**
一些组件或API误标了跨平台标签，实际未适配无法使用。

**解决办法**：
请在[社区提issue](https://gitcode.com/org/arkui-x/issues)，基于以下模板反馈问题，SDK内部会即使查看确认并回复。
【示例代码】
【ArkUI-X SDK版本】
【报错日志】



  
