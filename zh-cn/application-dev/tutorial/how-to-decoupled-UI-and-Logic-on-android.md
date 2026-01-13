# 以Hap为主体的共享逻辑包开发指南

## 简介

ArkUI-X框架支持UI与逻辑解耦，实现不带UI的业务逻辑跨平台。开发者在Hap的开发过程中可以不实现UI界面，仅关心业务逻辑的实现。<br>
应用UI使用Android侧能力实现，通过平台桥接能力调用ArkTS侧Hap包的业务逻辑。从而实现应用UI与业务逻辑解耦的目的。<br>
应注意：本套业务流程仅支持Android、iOS，不支持HarmonyOS。<br>

## 方案对比和适用场景分析

|            | 默认方式加载Hap                                              | 通过[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)加载Hap | 通过[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)加载Hap |
| ---------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| UIAbility  | 创建UIAbility组件<br/>根据[UIAbility规则](../reference/apis/js-apis-app-ability-uiAbility.md)会触发所有生命周期函数 | 不创建UIAbility<br/>不会触发UIAbility的任何生命周期函数      | 创建UIAbility组件<br/>调用[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)时仅会触发UIAbility的onCreate()<br/>调用[unloadAbility](../reference/arkui-for-android/AbilityLoader.md#unloadability)时仅会触发UIAbility的onWindowStageDestroy()和onDestroy() |
| UI界面     | 可使用ArkTS侧与UI相关的组件和API                             | 不可使用ArkTS侧与UI相关的组件和API                           | 不可使用ArkTS侧与UI相关的组件和API                           |
| API限制    | 全量API均可使用，无限制                                      | 涉及下列内容的API将无法使用<br/>1.涉及使用UIAbility组件的上下文信息(context)，比如[@ohos.data.preferences](../reference/apis/js-apis-data-preferences.md#data_preferencesgetpreferences)。<br/>2.涉及UI组件相关的API | 涉及下列内容的API将无法使用<br/>1.涉及UI组件相关的API |
| 特殊配置项 | 无                                                           | 详见[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md) | 无                                                           |
| 重复性     | 已加载的UIAbility通过startAbility加载本身时，会创建新的UIAbility，即可重复创建 | 重复调用loadModule时，每一次调用都会触发[onLoad()](../reference/apis/js-apis-ModuleLoader.md#moduleloaderonload)回调 | 重复调用loadAbility加载相同的UIAbility时，仅第一次加载时会触发onCreate()，后续接口调用将不会生效，即不可重复创建<br>unloadAbility同理 |
| 内存 | <img src="figures/decoupled-UI-and-Logic-android-1.png" width="350" height="800"/> | <img src="figures/decoupled-UI-and-Logic-android-2.png" width="350" height="800"/> | <img src="figures/decoupled-UI-and-Logic-android-3.png" width="350" height="800"/> |

**注1**：内存结果受多种因素影响，以上数值仅供参考，内存结果使用Android adb调试命令得出。<br>

**注2**："API限制"中API指[OpenHarmony接口定义跨平台支持列表](../reference/apis/README.md)。<br>

综上所述：判断以下条件<br>

1. 应用使用Android系统能力实现UI界面。<br>
2. ArkTS生成的Hap逻辑中，只涉及使用API接口，不涉及使用UIAbility组件、UI组件等内容。<br>
3. ArkTS侧Hap中的自定义函数逻辑可通过平台桥接功能在Android侧调用到。<br>
4. ArkTS侧使用的API接口**是否**需要用到UIAbility组件的上下文信息。<br>

当满足上述所有条件且第4点为**否**时；可采用方案：通过[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)加载Hap。<br>

当满足上述所有条件且第4点为**是**时；可采用方案：通过[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)加载Hap。<br>

上述条件任一不满足时；可采用方案：默认方式加载Hap。<br>

## 开发流程

- 开发前请务必仔细阅览本文档的[约束与限制](#约束与限制)部分。<br>
- 每一步骤前声明该步骤的所属平台侧。<br>

### (一)通过loadModule加载Hap

1. **Android侧：控制ArkUI-X框架是否加载UI部分**

   应用UI使用Android侧能力实现，基于减小应用运行内存的目的，可由开发者控制ArkUI-X框架是否加载UI部分。<br>

   当前StageApplication初始化有继承StageApplication和创建StageApplicationDelegate实例2种方式。对应是否加载UI部分的方法如下。<br>

   推荐使用方式：**继承StageApplication**。<br>

   - 其一：使用[StageApplication](../reference/arkui-for-android/StageApplication.md)作为Android应用的入口基类。<br>

     通过Override [onShouldLoadUI](../reference/arkui-for-android/StageApplication.md#onshouldloadui)接口，控制函数返回值来决定ArkUI-X框架是否加载UI部分。<br>

     ```java
     // MyStageApplication.java

     import ohos.stage.ability.adapter.StageApplication;

     public class MyStageApplication extends StageApplication {
         @Override
         public boolean onShouldLoadUI() {
             // 函数返回值false，表示不加载UI部分。
             return false;
         }
     }
     ```
     
   - 其二：使用**android.app.Application**作为Android应用的入口基类。<br>
   
     初始化[StageApplicationDelegate](../reference/arkui-for-android/StageApplicationDelegate.md)对象，并调用[initApplication](../reference/arkui-for-android/StageApplicationDelegate.md#initapplication)初始化ArkUI-X框架，通过第二个函数入参决定ArkUI-X框架是否加载UI部分。<br>
     
     ```java
     // MyApplication.java

     import android.app.Application;
     import ohos.stage.ability.adapter.StageApplicationDelegate;

     public class MyApplication extends Application {
         @Override
         public void onCreate() {
             StageApplicationDelegate stageApplicationDelegate = new StageApplicationDelegate();
             // 第二个函数入参为false，表示不加载UI部分。
             stageApplicationDelegate.initApplication(this, false);
             super.onCreate();
         }
     }
     ```
   

2. **Android侧：调用[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)接口加载 HAP 包**

   在合适的时机（如应用启动时）调用[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)接口加载目标 HAP 包。具体调用时机应根据实际业务场景调整。<br>

   ```java
   // NativeActivity.java
   import android.app.Activity;
   import android.os.Bundle;
   import ohos.stage.ability.adapter.StageApplicationDelegate;
   
   public class NativeActivity extends Activity {
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           StageApplicationDelegate.loadModule("entry", "./ets/MyModuleLoader.ets");
       }
   }
   ```
  

3. **ArkTS侧：继承[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)，重载[onLoad()](../reference/apis/js-apis-ModuleLoader.md#moduleloaderonload)方法，实现初始化逻辑。**

   实现[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)接口。当Android侧调用[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)触发加载Hap并完成后，会触发[onLoad()](../reference/apis/js-apis-ModuleLoader.md#moduleloaderonload)回调。<br>

   示例：<br>

   ```tsx
   // MyModuleLoader.ets
   // 自定义class MyModuleLoader 继承ModuleLoader，覆写onLoad方法，实现业务逻辑。
   // 示例代码中实现了初始化ArkTS侧Bridge对象。
   
   import ModuleLoader from '@arkui-x.ModuleLoader'
   import Bridge from '@arkui-x.bridge'
   
   export default class MyModuleLoader extends ModuleLoader {
     onLoad(): void {
       console.info('MyModuleLoader onLoad start')
       Bridge.createBridge('BridgeObject', Bridge.BridgeType.JSON_TYPE)
       console.info('MyModuleLoader onLoad end')
     }
   }
   ```
   

4. **ArkTS侧：在 hap模块对应`build-profile.json5`中配置模块生命周期实现ets文件（示例为MyModuleLoader.ets）的路径，使文件参与编译。**

   需要配置的信息如下，其中路径信息需要开发者修改为实际的路径信息。<br>

   ```json
   "arkOptions": {
       "runtimeOnly": {
           "sources": [
               "./src/main/ets/MyModuleLoader.ets"		// 继承ModuleLoader的实例class所在文件的相对路径
           ]
       }
   }
   ```

   配置完毕后，整体的build-profile.json5文件范例如下。<br>

   ```json
   // ${工程}/entry/build-profile.json5
   // 配置" MyModuleLoader.ets "的信息
   
   {
     "apiType": "stageMode",
     "buildOption": {
       "resOptions": {
         "copyCodeResource": {
           "enable": false
         }
       },
       "arkOptions": {
         "runtimeOnly": {
           "sources": [
             "./src/main/ets/MyModuleLoader.ets"
           ]
         }
       }
     },
     "buildOptionSet": [
       {
         "name": "release",
         "arkOptions": {
           "obfuscation": {
             "ruleOptions": {
               "enable": false,
               "files": [
                 "./obfuscation-rules.txt"
               ]
             }
           }
         }
       }
     ],
     "targets": [
       {
         "name": "default"
       }
     ]
   }
   ```

5. **Android侧：Android通过平台桥接能力调用 ArkTS Hap中业务逻辑**

   通过[平台桥接机制](../quick-start/platform-bridge-introduction.md)实现Android侧调用 HAP 包内的 ArkTS 业务逻辑，完成跨语言/跨环境的调用与数据交互。<br>

   ```java
   // NativeActivity.java
   import android.app.Activity;
   import android.os.Bundle;
   import android.util.Log;
   
   import ohos.stage.ability.adapter.StageApplicationDelegate;
   
   public class NativeActivity extends Activity {
       private static final String TAG = "LOG";
   
       private static final String LOG_TAG = "[Test][JAVA][NativeActivity]:: ";
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           Log.e(TAG, LOG_TAG + "onCreate");
           super.onCreate(savedInstanceState);
           setContentView(R.layout.native_page);
   
           findViewById(R.id.BTN_LoadHap).setOnClickListener(v -> {
               // 添加按钮点击事件，在点击按钮后，触发加载Hap：entry。
               StageApplicationDelegate.loadModule("entry", "./ets/MyModuleLoader.ets");
           });
   
           findViewById(R.id.BTN_CallMethod).setOnClickListener(v -> {
               // 当触发加载Hap后，通过点击页面的Button，通过平台桥接调用ArkTS侧方法，并拿到返回数据，通过日志形式输出。
               try {
                   BridgeUtil object = BridgeUtil.getInstance();
                   if (object == null) {
                       Log.e(TAG, LOG_TAG + "BridgeUtil object is null");
                       return;
                   }
                   Object data = object.callMethodSync("getDeviceInfo");
                   Log.i(TAG, LOG_TAG + "DeviceInfo is " + data);
               } catch (Exception e) {
                   Log.e(TAG, LOG_TAG + "callMethodSync failed, error is :", e);
               }
           });
       }
   }
   ```
   

### (二)通过loadAbility加载Hap

ArkTS侧部分API在调用时需要用到UIAbility组件的上下文信息，而通过[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)接口加载hap时，框架仅初始化了运行时，加载了Hap字节码，不会触发UIAbility的创建流程，因此此时的应用环境中UIAbility组件的上下文信息为空，导致这部分API将无法使用。为了解决此问题，提供第二种方案：通过[AbilityLoader](../reference/arkui-for-android/AbilityLoader.md)接口[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)加载Hap，此方案会触发UIAbility的创建流程。由此开发者可顺利使用此前限制的API。<br>

1. **Android侧：控制ArkUI-X框架是否加载UI部分**

   应用UI使用Android侧能力实现，基于减小应用运行内存的目的，可由开发者控制ArkUI-X框架是否加载UI部分。<br>

   当前StageApplication初始化有继承StageApplication和创建StageApplicationDelegate实例2种方式。对应是否加载UI部分的方法如下。<br>

   推荐使用方式：**继承StageApplication**。<br>

   - 其一：使用[StageApplication](../reference/arkui-for-android/StageApplication.md)作为Android应用的入口基类。<br>

     通过Override [onShouldLoadUI](../reference/arkui-for-android/StageApplication.md#onshouldloadui)接口，控制函数返回值来决定ArkUI-X框架是否加载UI部分。<br>

     ```java
     // MyStageApplication.java

     import ohos.stage.ability.adapter.StageApplication;

     public class MyStageApplication extends StageApplication {
         @Override
         public boolean onShouldLoadUI() {
             // 函数返回值false，表示不加载UI部分。
             return false;
         }
     }
     ```

   - 其二：使用**android.app.Application**作为Android应用的入口基类。<br>

     初始化[StageApplicationDelegate](../reference/arkui-for-android/StageApplicationDelegate.md)对象，并调用[initApplication](../reference/arkui-for-android/StageApplicationDelegate.md#initapplication)初始化ArkUI-X框架，通过第二个函数入参决定ArkUI-X框架是否加载UI部分。<br>

     ```java
     // MyApplication.java

     import android.app.Application;
     import ohos.stage.ability.adapter.StageApplicationDelegate;

     public class MyApplication extends Application {
         @Override
         public void onCreate() {
             StageApplicationDelegate stageApplicationDelegate = new StageApplicationDelegate();
             // 第二个函数入参为false，表示不加载UI部分。
             stageApplicationDelegate.initApplication(this, false);
             super.onCreate();
         }
     }
     ```

2. **Android侧：通过[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)控制加载UIAbility**

   在合适的时机（如应用启动时）调用[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)接口加载目标HAP包。具体调用时机应根据实际业务场景调整。<br>

   ```java
   // NativeActivity.java
   
   import android.app.Activity;
   import android.os.Bundle;
   import ohos.stage.ability.adapter.AbilityLoader;
   
   public class NativeActivity extends Activity {
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           super.onCreate(savedInstanceState);
           AbilityLoader.loadAbility("com.example.hspui", "entry", "EntryAbility", "");
       }
   }
   ```

3. **ArkTS侧：在UIAbility组件onCreate()中实现初始化逻辑。**

   当Android侧调用[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)触发加载Hap后并完成后，会触发UIAbility的onCreate()回调。<br>

   示例：<br>

   ```tsx
   // ets/entryability/EntryAbility.ets
   
   import { AbilityConstant, UIAbility, Want } from '@kit.AbilityKit';
   import Bridge from '@arkui-x.bridge'
   
   const LOG_TAG: string = '[Test][ArkTS][entry:EntryAbility]:: '
   
   export default class EntryAbility extends UIAbility {
     onCreate(want: Want, launchParam: AbilityConstant.LaunchParam): void {
       console.info(LOG_TAG + 'Ability onCreate');
       Bridge.createBridge('BridgeObject', Bridge.BridgeType.JSON_TYPE)
     }
   }
   ```

4. **Android侧：Android通过平台桥接能力调用 ArkTS Hap中业务逻辑**

   通过[平台桥接机制](../quick-start/platform-bridge-introduction.md)实现Android侧调用 HAP 包内的 ArkTS 业务逻辑，完成跨语言/跨环境的调用与数据交互。<br>

   ```java
   // NativeActivity.java
   
   import android.app.Activity;
   import android.os.Bundle;
   import android.util.Log;
   import ohos.stage.ability.adapter.AbilityLoader;
   
   public class NativeActivity extends Activity {
       private static final String TAG = "LOG";
   
       private static final String LOG_TAG = "[Test][JAVA][NativeActivity]:: ";
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           Log.e(TAG, LOG_TAG + "onCreate");
           super.onCreate(savedInstanceState);
           setContentView(R.layout.native_page);
   
           findViewById(R.id.BTN_LoadHap).setOnClickListener(v -> {
               // 添加按钮点击事件，在点击按钮后，触发加载Hap：entry。
               AbilityLoader.loadAbility("com.example.hspui", "entry", "EntryAbility", "");
           });
   
           findViewById(R.id.BTN_CallMethod).setOnClickListener(v -> {
               // 当触发加载Hap后，通过点击页面的Button，通过平台桥接调用ArkTS侧方法，并拿到返回数据，通过日志形式输出。
               try {
                   BridgeUtil object = BridgeUtil.getInstance();
                   if (object == null) {
                       Log.e(TAG, LOG_TAG + "BridgeUtil object is null");
                       return;
                   }
                   Object data = object.callMethodSync("getDeviceInfo");
                   Log.i(TAG, LOG_TAG + "DeviceInfo is " + data);
               } catch (Exception e) {
                   Log.e(TAG, LOG_TAG + "callMethodSync failed, error is :", e);
               }
           });
       }
   }
   ```

## 约束与限制

1. Android侧：Activity需要使用Android原生提供的**android.app.Activity**，禁止使用ArkUI-X框架提供的**ohos.stage.ability.adapter.StageActivity**。<br>

2. ArkTS侧：如使用接口[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)，需要开发者在ArkTS侧继承[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)后实现自定义的实例类 "MyModuleLoader"（自定义实例class的类名可以由开发者自行定义，这里方便文档描述使用MyModuleLoader作为自定义实例class类名进行描述）。<br>

   - MyModuleLoader代码所在的实际文件（MyModuleLoader.ets）：文件后缀必须为"ets"，**禁止使用其它后缀名**。<br>
   - MyModuleLoader代码所在的实际文件（MyModuleLoader.ets）：文件必须位于工程 `ets`目录下（即 `{应用工程}/{Hap模块}/src/main/ets`）。<br>允许存放在 `ets`目录的**任意子目录**中（例如 `ets/subdir/MyModuleLoader.ets`或 `ets/utils/MyModuleLoader.ets`等均符合要求）。<br>**禁止存放在 `ets`目录之外**。<br>

3. Android侧：[loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)接口的entryFile参数，该参数是ArkTS侧`MyModuleLoader.ets`的相对路径（相对于工程中ets目录）。<br>

   - 要求入参的字符串字段必须以"./ets"作为字段开头。后面跟随ArkTS侧`MyModuleLoader.ets`的相对路径。<br>

     举例：<br>

     - `MyModuleLoader.ets`的路径如下： `{应用工程}/{Hap模块}/src/main/ets/MyModuleLoader.ets` ，则entryFile参数为`./ets/MyModuleLoader.ets`。<br>
     - `MyModuleLoader.ets`的路径如下： `{应用工程}/{Hap模块}/src/main/ets/utils/MyModuleLoader/MyModuleLoader.ets` ，则entryFile参数为`./ets/utils/MyModuleLoader/MyModuleLoader.ets`。<br>

4. [loadModule](../reference/arkui-for-android/StageApplicationDelegate.md#loadmodule)接口、[loadAbility](../reference/arkui-for-android/AbilityLoader.md#loadability)接口目前仅支持加载Hap包，不支持加载Hsp包、Har包。<br>

## 实践参考

提供完整的[Sample示例](https://gitcode.com/arkui-x/samples/tree/master/SuperFeature/DecoupledUIAndLogic)，供开发者参考。<br>

