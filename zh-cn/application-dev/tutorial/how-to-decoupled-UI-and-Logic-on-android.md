# ArkUI-X框架支持UI与业务解耦，实现不带UI的独立业务逻辑跨平台

## 简介

ArkUI-X框架支持加载特殊形式的ArkTS侧Hap包，在该Hap中，开发者可以不实现UI界面，而只关心业务逻辑的开发实现。<br>
应用UI使用Android侧能力实现，通过平台桥接能力调用ArkTS侧Hap包的业务逻辑。从而实现应用UI与业务逻辑解耦的目的。<br>

## 开发流程

- 开发前请务必仔细阅览本文档的[约束与限制](#约束与限制)部分。<br>
- 每一步骤前声明该步骤的所属平台侧。<br>

1. **Android侧：控制ArkUI-X框架是否加载UI部分**

   应用UI使用Android侧能力实现，基于减小应用运行内存的目的，可由开发者控制ArkUI-X框架是否加载UI部分。<br>

   当前StageApplication初始化有继承StageApplication和创建StageApplicationDelegate实例2种方式。对应是否加载UI部分的方法如下。<br>

   推荐使用方式：**继承StageApplication**。<br>

   - 其一：使用[StageApplication](../reference/arkui-for-android/StageApplication.md)作为Android应用的入口基类。<br>

     通过Override [onShouldLoadUI](#onShouldLoadUI)接口，控制函数返回值来决定ArkUI-X框架是否加载UI部分。<br>

     ```java
     // MyStageApplication.java
     public class MyStageApplication extends StageApplication {
         @Override
         public void onCreate() {
             super.onCreate();
         }
     
         @Override
         public boolean onShouldLoadUI() {
             // 函数返回值false，表示不加载UI部分。
             return false;
         }
     }
     ```

   - 其二：使用**android.app.Application**作为Android应用的入口基类。<br>

     初始化[StageApplicationDelegate](../reference/arkui-for-android/StageApplicationDelegate.md)对象，并调用[initApplication](#initApplication)初始化ArkUI-X框架，通过第二个函数入参决定ArkUI-X框架是否加载UI部分。<br>

     ```java
     // MyApplication.java
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

2. **Android侧：调用 `loadModule`接口加载 HAP 包**

   在合适的时机（如应用启动时）调用 `loadModule`接口加载目标 HAP 包。具体调用时机应根据实际业务场景调整。<br>

   在示例中：通过点击页面的Button，触发加载Hap包。<br>

   ```java
   // NativeActivity.java
   
   package com.example.hspui;
   
   import android.app.Activity;
   import android.graphics.Color;
   import android.graphics.drawable.GradientDrawable;
   import android.os.Bundle;
   import android.util.Log;
   
   import ohos.stage.ability.adapter.StageApplicationDelegate;
   
   public class NativeActivity extends Activity {
       private static final String TAG = "LOG";
   
       private static final String LOG_TAG = "[Test][JAVA][NativeActivity]:: ";
   
       private void showCaseRes(int id, boolean res) {
           GradientDrawable greenBg = new GradientDrawable();
           if (res) {
               greenBg.setColor(Color.GREEN);
           } else {
               greenBg.setColor(Color.RED);
           }
           findViewById(id).setBackground(greenBg);
       }
   
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           Log.e(TAG, LOG_TAG + "onCreate");
           super.onCreate(savedInstanceState);
           setContentView(R.layout.native_page);
   
           findViewById(R.id.BTN_LoadHap).setOnClickListener(v -> {
               // 添加按钮点击事件，在点击按钮后，触发加载Hap：entry
               StageApplicationDelegate.loadModule("entry", "./ets/MyModuleLoader.ets");
           });
   
           findViewById(R.id.BTN_CallMethod).setOnClickListener(v -> {
               try {
                   BridgeUtil object = BridgeUtil.getInstance();
                   if (object == null) {
                       Log.e(TAG, LOG_TAG + "BridgeUtil object is null");
                       return;
                   }
                   Object data = object.callMethodSync("getDeviceInfo");
                   Log.i(TAG, LOG_TAG + "DeviceInfo is " + data);
                   this.showCaseRes(R.id.BTN_CallMethod, data.toString().contains(
                       "[ArkTS]: (Native) call getDeviceInfo by callMethodSync success"));
               } catch (Exception e) {
                   Log.e(TAG, LOG_TAG + "callMethodSync failed, error is :", e);
                   this.showCaseRes(R.id.BTN_CallMethod, false);
               }
           });
       }
   }
   ```

4. **ArkTS侧：继承ModuleLoader模块生命周期基类，重载onLoad方法，实现初始化逻辑。**

   实现 `ModuleLoader`接口。当Android侧调用loadModule触发加载Hap后并完成后，会触发onLoad回调。<br>

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
   
5. **在 hap模块对应`build-profile.json5`中配置模块生命周期实现ets文件（示例为MyModuleLoader.ets）的路径，使文件参与编译。**

   ```diff
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
   +    "arkOptions": {
   +      "runtimeOnly": {
   +        "sources": [
   +          "./src/main/ets/MyModuleLoader.ets"  // 继承ModuleLoader的实例class所在文件的相对路径
   +        ]
   +      }
   +    }
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

6. **Android通过平台桥接能力调用 ArkTS Hap中业务逻辑**

   通过[平台桥接机制](../quick-start/platform-bridge-introduction.md)实现Android侧调用 HAP 包内的 ArkTS 业务逻辑，完成跨语言/跨环境的调用与数据交互。<br>

   示例：当触发加载Hap后，通过点击页面的Button，通过平台桥接调用ArkTS侧方法，并拿到返回数据，通过日志形式输出。<br>

   ```java
   // NativeActivity.java
   
   package com.example.hspui;
   
   import android.app.Activity;
   import android.graphics.Color;
   import android.graphics.drawable.GradientDrawable;
   import android.os.Bundle;
   import android.util.Log;
   
   import ohos.stage.ability.adapter.StageApplicationDelegate;
   
   public class NativeActivity extends Activity {
       private static final String TAG = "LOG";
   
       private static final String LOG_TAG = "[Test][JAVA][NativeActivity]:: ";
   
       private void showCaseRes(int id, boolean res) {
           GradientDrawable greenBg = new GradientDrawable();
           if (res) {
               greenBg.setColor(Color.GREEN);
           } else {
               greenBg.setColor(Color.RED);
           }
           findViewById(id).setBackground(greenBg);
       }
   
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
                   this.showCaseRes(R.id.BTN_CallMethod, data.toString().contains(
                       "[ArkTS]: (Native) call getDeviceInfo by callMethodSync success"));
               } catch (Exception e) {
                   Log.e(TAG, LOG_TAG + "callMethodSync failed, error is :", e);
                   this.showCaseRes(R.id.BTN_CallMethod, false);
               }
           });
       }
   }
   ```

## 约束与限制

1. Android侧：Activity需要使用Android原生提供的**android.app.Activity**，禁止使用ArkUI-X框架提供的**ohos.stage.ability.adapter.StageActivity**。<br>

2. ArkTS侧：需要开发者继承[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)后实现自定义的实例类 " MyModuleLoader "（自定义实例class的类名可以由开发者自行定义，这里方便文档描述使用MyModuleLoader作为自定义实例class类名进行描述）。<br>

   - MyModuleLoader代码所在的实际文件（MyModuleLoader.ets）：文件后缀必须为“ ets ”，**禁止使用其它后缀名**。<br>
   - MyModuleLoader代码所在的实际文件（MyModuleLoader.ets）：文件必须位于工程 `ets`目录下（即 `{应用工程}/{Hap模块}/src/main/ets`）。<br>允许存放在 `ets`目录的**任意子目录**中（例如 `ets/subdir/MyModuleLoader.ets`或 `ets/utils/MyModuleLoader.ets`等均符合要求）。<br>**禁止存放在 `ets`目录之外**。<br>

3. Android侧：loadModule接口的entryFile参数，该参数是ArkTS侧`MyModuleLoader.ets `的相对路径（相对于工程中ets目录）。<br>

   - 要求入参的字符串字段必须以“ ./ets ”作为字段开头。后面跟随ArkTS侧`MyModuleLoader.ets `的相对路径。<br>

     举例：<br>

     - `MyModuleLoader.ets `的路径如下： `{应用工程}/{Hap模块}/src/main/ets/MyModuleLoader.ets` ，则entryFile参数为`./ets/MyModuleLoader.ets`。<br>
     - `MyModuleLoader.ets `的路径如下： `{应用工程}/{Hap模块}/src/main/ets/utils/MyModuleLoader/MyModuleLoader.ets` ，则entryFile参数为`./ets/utils/MyModuleLoader/MyModuleLoader.ets`。<br>

4. loadModule接口目前仅支持加载Hap包，不支持加载Hsp包、Har包。<br>

## 接口说明

> **说明：**
>
> 本模块首批接口从API version 23 开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
> 本模块接口仅可在Stage模型下使用。

### onShouldLoadUI

public boolean onShouldLoadUI()

**描述：**

控制ArkUI-X框架是否加载ArkUI，如不加载，应用启动占用内存降低。<br>
禁止在函数中实现无关逻辑，开发者应保证函数返回值为确定的boolean值（true或false）。

**参数：** 

无

**返回值：**

| 类型    | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| boolean | 函数返回true表示需要加载ArkUI，函数返回false表示不需要加载ArkUI。 |

**示例：**

```java
// MyStageApplication.java
public class MyStageApplication extends StageApplication {
    @Override
    public void onCreate() {
        super.onCreate();
    }

    @Override
    public boolean onShouldLoadUI() {
        return false;
    }
}
```

### initApplication

public void initApplication(Application application, boolean shouldLoadUI)

**描述：**

应用初始化ArkUI-X框架，控制是否加载ArkUI，如不加载，应用启动占用内存降低。

**参数：** 

| 参数名       | 类型        | 必填 | 说明                                              |
| ------------ | ----------- | ---- | ------------------------------------------------- |
| application  | Application | 是   | 应用的全局管理者。                                |
| shouldLoadUI | boolean     | 是   | true表示需要加载ArkUI，false表示不需要加载ArkUI。 |

**返回值：**

无

**示例：**

```java
// MyApplication.java
public class MyApplication extends Application {
    @Override
    public void onCreate() {
        StageApplicationDelegate stageApplicationDelegate = new StageApplicationDelegate();
        stageApplicationDelegate.initApplication(this, false);
        super.onCreate();
    }
}
```

### loadModule

public static void loadModule(String moduleName, String entryFile)

**描述：**

Android侧加载指定的Hap模块，需要与ArkTS侧[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)结合使用。

**参数：** 

| 参数名     | 类型   | 必填 | 说明                                                         |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| moduleName | String | 是   | Hap模块的模块名称。                                          |
| entryFile  | String | 是   | ArkTS侧Hap模块中自定义的[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)实现类文件的相对路径（相对于Hap模块中ets目录）。<br>限制：字段必须以“./ets/”开头，后面跟随"MyModuleLoader.ets"的相对路径。更详细的限制描述详见[约束与限制](#约束与限制) |

**返回值：**

**示例：**

Android侧：

```java
// 引用头文件
import ohos.stage.ability.adapter.StageApplicationDelegate;

// 接口使用范例。
// moduleName : "entry"					   : Hap模块的模块名称为entry
// entryFile  : "./ets/MyModuleLoader.ets" : Hap模块中“MyModuleLoader.ets”的路径
StageApplicationDelegate.loadModule("entry", "./ets/MyModuleLoader.ets");
```

ArkTS侧：配置Hap信息并实现onLoad()回调。

```diff
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
+    "arkOptions": {
+      "runtimeOnly": {
+        "sources": [
+          "./src/main/ets/MyModuleLoader.ets"  // 继承ModuleLoader的实例class所在文件的相对路径
+        ]
+      }
+    }
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

```tsx
// MyModuleLoader.ets
import ModuleLoader from '@arkui-x.ModuleLoader'

export default class MyModuleLoader extends ModuleLoader {
  onLoad(): void {
    console.info('MyModuleLoader onLoad')
    // 开发者业务逻辑
  }
}
```

## 实践参考

提供完整的[Sample示例](https://gitcode.com/arkui-x/samples/tree/master/SuperFeature/DecoupledUIAndLogic)，供开发者参考。

