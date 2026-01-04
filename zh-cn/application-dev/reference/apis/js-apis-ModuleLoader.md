# @arkui-x.ModuleLoader.d.ts 

ModuleLoader是独立于UIAbility的回调，仅在原生侧使用loadModule接口加载ArkTS Hap包的场景下触发。

> **说明：**
>
> 本模块首批接口从API version 23开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```javaScript
import ModuleLoader from '@arkui-x.ModuleLoader'
```
## ModuleLoader.onLoad

onLoad(): void;

**描述：**

原生侧调用loadModule接口加载指定Hap，系统会触发该回调，开发者可在该回调中执行初始化逻辑（如初始化Bridge等）。<br>
Android侧loadModule使用详见[文档](../../tutorial/how-to-decoupled-UI-and-Logic-on-android.md#loadmodule)。iOS侧loadModule使用详见[文档](../../tutorial/how-to-decoupled-UI-and-Logic-on-ios.md#loadmodule)。

**限制：**

- 要求文本后缀为ets。
- 需要在Hap所属的模块级build-profile.json5中配置路径信息，使文件参与编译。详细见示例。

**参数：** 

无

**返回值：**

无

**示例：**

```tsx
// MyModuleLoader.ets
// 限制：文件后缀必须为“ets”
import ModuleLoader from '@arkui-x.ModuleLoader'

export default class MyModuleLoader extends ModuleLoader {
  onLoad(): void {
    console.info('MyModuleLoader onLoad')
    // 开发者业务逻辑
  }
}
```

```json
// ${工程}/{Hap}/build-profile.json5
// 在Hap所属的模块级build-profile.json5中配置" MyModuleLoader.ets "的信息

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
