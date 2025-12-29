# ArkUI-X框架支持UI与业务解耦，实现不带UI的独立业务逻辑跨平台

## 简介

ArkUI-X框架支持UI与逻辑解耦，实现不带UI的业务逻辑跨平台。开发者在Hap的开发过程中可以不实现UI界面，仅关心业务逻辑的实现。<br>
应用UI使用iOS侧能力实现，通过平台桥接能力调用ArkTS侧Hap包的业务逻辑。从而实现应用UI与业务逻辑解耦的目的。<br>
应注意：本套业务流程仅支持Android、iOS，不支持HarmonyOS。<br>

## 开发流程

- 开发前请务必仔细阅览本文档的[约束与限制](#约束与限制)部分。<br>
- 每一步骤前声明该步骤的所属平台侧。<br>

1. **iOS侧：控制ArkUI-X框架是否加载UI部分**

   应用UI使用iOS侧能力实现，基于减小应用运行内存的目的，可由开发者控制ArkUI-X框架是否加载UI部分。<br>

   - 控制ArkUI-X框架加载UI部分，通过使用接口[launchApplication](#launchApplication)实现。

     ```objective-c
     // .arkui-x/ios/app/AppDelegate.m
     
     - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
     {
         [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
         // 使用接口launchApplication，控制加载UI部分
         [StageApplication launchApplication];
         NativeViewController* mainView = [[NativeViewController alloc] init];
         [self setNavRootVC:mainView];
         return YES;
     }
     ```

   - 控制ArkUI-X框架不加载UI部分，通过使用接口[launchApplicationWithoutUI](#launchApplicationWithoutUI)实现。

     ```objective-c
     // .arkui-x/ios/app/AppDelegate.m
     
     - (BOOL)application:(UIApplication*)application didFinishLaunchingWithOptions:(NSDictionary*)launchOptions
     {
         [StageApplication configModuleWithBundleDirectory:BUNDLE_DIRECTORY];
         // 使用接口launchApplicationWithoutUI，控制不加载UI部分
         [StageApplication launchApplicationWithoutUI];
         NativeViewController* mainView = [[NativeViewController alloc] init];
         [self setNavRootVC:mainView];
         return YES;
     }
     ```

2. **iOS侧：调用 `loadModule`接口加载 HAP 包**

   在合适的时机（如应用启动时）调用 `loadModule`接口加载目标 HAP 包。具体调用时机应根据实际业务场景调整。<br>

   在示例中：通过点击页面的Button，触发加载Hap包。<br>

   ```objective-c
   // NativeViewController.m
   
   - (void)BTN_LoadHap:(UIButton*)sender
   {
       [StageApplication loadModule:@"entry" entryFile:@"./ets/MyModuleLoader.ets"];
   }
   ```
   
4. **ArkTS侧：继承ModuleLoader模块生命周期基类，重载onLoad方法，实现初始化逻辑。**

   实现 `ModuleLoader`接口。当iOS侧调用loadModule触发加载Hap后并完成后，会触发onLoad回调。<br>

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
   
5. **ArkTS侧：在 hap模块对应`build-profile.json5`中配置模块生命周期实现ets文件（示例为MyModuleLoader.ets）的路径，使文件参与编译。**

   需要配置的信息如下，其中路径信息需要开发者修改为实际的路径信息。

   ```json
       "arkOptions": {
         "runtimeOnly": {
           "sources": [
             "./src/main/ets/MyModuleLoader.ets"		// 继承ModuleLoader的实例class所在文件的相对路径
           ]
         }
       }
   ```

   配置完毕后，整体的build-profile.json5文件范例如下。

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

6. **iOS通过平台桥接能力调用 ArkTS Hap中业务逻辑**

   通过[平台桥接机制](../quick-start/platform-bridge-introduction.md)实现iOS侧调用 HAP 包内的 ArkTS 业务逻辑，完成跨语言/跨环境的调用与数据交互。<br>

   示例：当触发加载Hap后，通过点击页面的Button，通过平台桥接调用ArkTS侧方法，并拿到返回数据，通过日志形式输出。<br>

   ```objective-c
   // NativeViewController.m
   
   - (void)BTN_CallMethod:(UIButton*)sender
   {
       @try {
           id data = [self.bridgeObject callMethodSync:@"getDeviceInfo" parameters:nil];
           if (data != nil && [data isKindOfClass:[NSString class]]) {
               NSString* resultString = (NSString*)data;
               if ([resultString containsString:@"[ArkTS]: (Native) call getDeviceInfo by callMethodSync success"]) {
                   sender.backgroundColor = [UIColor colorWithRed:0.2 green:0.8 blue:0.2 alpha:1.0];
               } else {
                   sender.backgroundColor = [UIColor colorWithRed:0.8 green:0.2 blue:0.2 alpha:1.0];
               }
           } else {
               sender.backgroundColor = [UIColor colorWithRed:0.8 green:0.2 blue:0.2 alpha:1.0];
           }
       } @catch (NSException* exception) {
           NSLog(@"[Test][iOS][NativeViewController]:: callMethodSync failed, error is : %@", exception);
           sender.backgroundColor = [UIColor colorWithRed:0.8 green:0.2 blue:0.2 alpha:1.0];
       }
   }
   ```

## 约束与限制

1. iOS侧：应用视图控制器实现需要使用iOS原生提供的**UIViewController**，禁止使用ArkUI-X框架提供的 **<libarkui_ios/StageViewController.h>**。<br>

2. ArkTS侧：需要开发者继承[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)后实现自定义的实例类 " MyModuleLoader "（自定义实例class的类名可以由开发者自行定义，这里方便文档描述使用MyModuleLoader作为自定义实例class类名进行描述）。<br>

   - MyModuleLoader代码所在的实际文件（MyModuleLoader.ets）：文件后缀必须为“ ets ”，**禁止使用其它后缀名**。<br>
   - MyModuleLoader代码所在的实际文件（MyModuleLoader.ets）：文件必须位于工程 `ets`目录下（即 `{应用工程}/{Hap模块}/src/main/ets`）。<br>允许存放在 `ets`目录的**任意子目录**中（例如 `ets/subdir/MyModuleLoader.ets`或 `ets/utils/MyModuleLoader.ets`等均符合要求）。<br>**禁止存放在 `ets`目录之外**。<br>

3. iOS侧：loadModule接口的entryFile参数，该参数是ArkTS侧`MyModuleLoader.ets `的相对路径（相对于工程中ets目录）。<br>

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

### launchApplication

(void)launchApplication

**描述：**

应用初始化ArkUI-X框架，控制加载ArkUI，应用启动占用内存较大。

**参数：** 

无

**返回值：**

无

**示例：**

```objective-c
// .arkui-x/ios/app/AppDelegate.m
[StageApplication launchApplication];
```

### launchApplicationWithoutUI

(void)launchApplicationWithoutUI;

**描述：**

应用初始化ArkUI-X框架，控制不加载ArkUI，应用启动占用内存较小。

**参数：** 

无

**返回值：**

无

**示例：**

```objective-c
// .arkui-x/ios/app/AppDelegate.m
[StageApplication launchApplicationWithoutUI];
```

### loadModule

(void)loadModule:(NSString *)moduleName entryFile:(NSString *)entryFile;

**描述：**

原生侧加载指定的Hap模块，需要与ArkTS侧[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)结合使用。

**参数：** 

| 参数名     | 类型   | 必填 | 说明                                                         |
| ---------- | ------ | ---- | ------------------------------------------------------------ |
| moduleName | String | 是   | Hap模块的模块名称。                                          |
| entryFile  | String | 是   | ArkTS侧Hap模块中自定义的[ModuleLoader](../reference/apis/js-apis-ModuleLoader.md)实现类文件的相对路径（相对于Hap模块中ets目录）。<br>限制：字段必须以“./ets/”开头，后面跟随"MyModuleLoader.ets"的相对路径。更详细的限制描述详见[约束与限制](#约束与限制) |

**返回值：**

无

**示例：**

原生侧：

```objective-c
// 引用头文件
#import <libarkui_ios/StageApplication.h>

// 接口使用范例。
// moduleName : "entry"					   : Hap模块的模块名称为entry
// entryFile  : "./ets/MyModuleLoader.ets" : Hap模块中“MyModuleLoader.ets”的路径
[StageApplication loadModule:@"entry" entryFile:@"./ets/MyModuleLoader.ets"];
```

ArkTS侧：配置Hap信息并实现onLoad()回调。

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

