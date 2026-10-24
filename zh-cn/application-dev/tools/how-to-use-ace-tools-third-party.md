# ArkUI-X 三方件（HAR）开发与集成指导

> 本文档介绍 ArkUI-X 三方件（HAR）的开发与集成全流程，涵盖以下内容：
> - 三方件类型选型（`package` / `ffi` / `plugin`）与平台配置
> - 使用 `ace create` 创建三方件工程
> - 使用 `ace build plugin` 构建 HAR 包
> - 使用 `ace install` 将 HAR 集成到消费工程
> - 编写 Index.ets 导入并调用三方件方法
> - 使用 `ace run hap/apk/ios` 安装到真机验证
> - 单平台包跨端导入白屏的已知问题与规避
> - 已集成包的手动卸载方法

---

## 0. 前置准备

环境与工具链准备请参考官方文档：[《使用 ArkUI-X CLI 工具开始》](https://gitcode.com/arkui-x/docs/blob/master/zh-cn/application-dev/quick-start/start-with-ace-tools.md)

环境自检：`ace check -v`

---

## 1. 三方件类型与选型

三种三方件形态对比：

| 形态 | `type` 字段 | 原生代码形式 | 适用场景 |
|---|---|---|---|
| **Package** | `package` | 无，纯 ArkTS | UI 组件库、工具函数库 |
| **FFI** | `ffi` | 预编译 C++（AAR + xcframework + .so） | MQTT、加密、图像处理等 C 库 |
| **Plugin** | `plugin` | 预编译 C++ & Java/Kotlin（AAR + xcframework + .so） | 藍牙、相机、传感器等平台 API |

> 区分：**Package=纯 TS 逻辑；FFI=C++ 纯计算（平台无关）；Plugin=C++ 调平台能力（含 Java/Kotlin 桥）**。仅 package 无 `.so` 依赖、三端零成本；ffi/plugin 需各端原生产物。

### 平台（`crossplatform-config.json5` 的 `platform`）
- `['android']` — 仅 Android（+ HarmonyOS 原生）
- `['iOS']` — 仅 iOS（+ HarmonyOS 原生）
- `['android','iOS']`（创建时选 Default/both）— 三端

> 单平台包（android-only / ios-only）只能在该平台被导入调用，跨平台导入会白屏（见第 8 节）。package 不受此限。

---

## 2. 创建三方件工程

```bash
ace create <输出目录> -t <ffi|plugin|package>
```

> 以下以创建一个**双平台（android + iOS）的 ffi 三方件 `my_ffi_both`** 为例，对应 lib 模块名为 `my_ffi_bothlib`。plugin / package 类型的创建流程相同，仅需将 `-t` 参数改为 `plugin` 或 `package`。

交互式问答（命令行逐项提示，输入对应值回车）：

```
$ ace create my_ffi_both -t ffi
? Enter the project name(my_ffi_both): my_ffi_both
? Enter the bundleName (com.example.my_ffi_both): 
? Enter the runtimeOS (1: OpenHarmony; 2: HarmonyOS): 1
? Please select the Compile SDK (1: 10; ...; 16: 26.0.0; 17: 26.0.1): 16
? Please select the project platform (1: Default(Android, iOS); 2: Android; 3: iOS): 1
? Please select the Framework integration approach (1: Native Manual; 2: CocoaPods): 1
Signing iOS app for device deployment using developer identity: "Apple Development: ..."

Project created. Target directory: /path/to/my_ffi_both.
In order to run your app, type:
    $ cd /path/to/my_ffi_both
    $ ace run
Your app code is in /path/to/my_ffi_both/entry.
```

> 说明：`Framework integration approach` 仅在 iOS/both 平台且 macOS 上出现；android-only 工程无此项。

创建后结构（以 `my_ffi_bothlib` 为 lib 模块）：
```
my_ffi_both/
├── my_ffi_bothlib/                 # HAR 库模块
│   ├── build-profile.json5           # 构建配置（含原生编译选项）
│   ├── Index.ets                     # 对外导出（re-export 自 src/main/ets/Index）
│   ├── oh-package.json5
│   └── src/main/
│       ├── ets/Index.ets             # import nativeModule from 'lib<module>.so'
│       ├── cpp/                      # CMakeLists.txt + *.cpp + types/（ffi/plugin）
│       └── module.json5
├── entry/                            # 自带示例 entry（用于自测）
├── .arkui-x/                         # android/ios 工程壳 + crossplatform-config.json5
└── build-profile.json5
```
---

## 3. 自身编译运行验证

在三方件工程自身先验证三端可跑（entry 已自带示例页面）：

```bash
cd my_ffi_both
ace run hap      # 鸿蒙真机
ace run apk      # Android 真机
ace run ios      # iOS 真机/模拟器
```

- package/ffi/plugin 在自身工程内，entry 导入的是本工程 lib，三端原生均已链接，应正常。

---

## 4. 构建 HAR（ace build plugin）

```bash
cd my_ffi_both
ace build plugin
```

执行过程（核心输出）：
```
Building a HAR file...
> hvigor Finished :my_ffi_bothlib:default@BuildNativeWithNinja... after 126 ms
> hvigor BUILD SUCCESSFUL in 1 s 430 ms
HAR file built successfully.
Auto-discovered HAR module: my_ffi_bothlib
Detected plugin type: ffi
Building Android AAR for my_ffi_bothlib...
  AAR stripped libarkui_android.so (x2): 43.3MB -> 0.2MB
  Android: my_ffi_bothlib-release.aar (185KB)
Building iOS xcframework via xcodebuild...
Cross-platform plugin HAR built: /path/to/my_ffi_both/build/outputs/my_ffi_bothlib.har
  Type: ffi  Module: my_ffi_bothlib
```

产出：`<工程>/build/outputs/my_ffi_bothlib.har`

HAR 结构：
```
my_ffi_bothlib.har
├── crossplatform-config.json5
├── android/my_ffi_bothlib-release.aar          # 平台含 android 时
├── ios/my_ffi_bothlib.xcframework/             # 平台含 iOS 时
└── package/
    ├── libs/arm64-v8a/libmy_ffi_bothlib.so     # 鸿蒙原生
    ├── libs/arm64-v8a/libc++_shared.so   # C++ 运行时（hvigor 自动随包）
    ├── Index.ets / oh-package.json5 / src/main/...
    └── META-INF/MANIFEST.MF
```

---

## 5. 消费工程：集成 HAR

### 5.1 创建普通应用工程
```bash
ace create consumer_app -t app
# platform 选 1 (Default: android, ios) 以支持三端
```

### 5.2 安装 HAR
```bash
cd consumer_app
ace install /Users/.../my_ffi_both/build/outputs/my_ffi_bothlib.har
```

> `<HAR 绝对路径>`：需传入 HAR 文件的绝对路径（如 `/Users/arkui-x/.../my_ffi_both/build/outputs/my_ffi_bothlib.har`），相对路径可能导致解包失败。

执行过程（完整输出）：
```
Extracting HAR: my_ffi_bothlib.har
Installing: my_ffi_bothlib@1.0.0 (type: ffi)
  oh-package.json5: added 'my_ffi_bothlib': 'file:../.arkui-x/plugins/my_ffi_bothlib'
  HarmonyOS: ohpm install resolved my_ffi_bothlib -> entry/oh_modules/
  Android: my_ffi_bothlib-release.aar -> app/libs/
  iOS: my_ffi_bothlib.xcframework -> frameworks/
  iOS: registered in project.pbxproj
my_ffi_bothlib@1.0.0 installed.
  HarmonyOS: oh_modules/my_ffi_bothlib/
  Android: AAR installed
```

`ace install` 自动完成：
- 解包 HAR 到 `.arkui-x/plugins/my_ffi_bothlib/`（含 `.so`、aar、xcframework）
- `entry/oh-package.json5` 加依赖 `"my_ffi_bothlib": "file:../.arkui-x/plugins/my_ffi_bothlib"`
- `ohpm install` 解析到 `entry/oh_modules/my_ffi_bothlib`
- Android：aar 拷到 `.arkui-x/android/app/libs/`，`build.gradle` 加 `implementation files(...)`
- iOS：xcframework 拷到 `.arkui-x/ios/frameworks/`，`project.pbxproj` 注册

---

## 6. 消费工程：编写 Index.ets 导入并测试

`consumer_app/entry/src/main/ets/pages/Index.ets`：

```typescript
import { add, greet } from 'my_ffi_bothlib'   // 导入包的方法

@Entry
@Component
struct Index {
  @State result: string = ''

  build() {
    Column() {
      Text(this.result).fontSize(20).margin(20)
      Button('调用')
        .onClick(() => {
          this.result = `add(3,5)=${add(3, 5)}\n${greet('ArkUI-X')}`
        })
    }.width('100%').height('100%')
  }
}
```

> 多包集成时注意**命名冲突**，用别名：`import { add as ffiA_add } from '...'`。

---

## 7. 运行安装到手机

```bash
cd consumer_app
ace run hap      # 鸿蒙
ace run apk      # Android
ace run ios      # iOS
```

`ace run <平台>` = 构建 + 安装 + 启动。

---

## 8. 已知问题：单平台包跨端导入白屏

### 现象

单平台三方件工程（`platform: ['android']` 或 `['iOS']` 的 ffi/plugin）生成的 HAR 包在消费工程中被导入后，在**非目标平台**运行时：**编译通过，启动白屏**。

| 三方件（platform） | 消费工程运行 | 结果 |
|---|---|---|
| `my_ffi_android`（android） | `ace run ios` | 编译正常，启动**白屏** |
| `my_ffi_ios`（iOS） | `ace run apk` | 编译正常，启动**白屏** |
| `my_plugin_android`（android） | `ace run ios` | 编译正常，启动**白屏** |
| `my_plugin_ios`（iOS） | `ace run apk` | 编译正常，启动**白屏** |

> package（纯 ETS）包无此问题，三端均可导入。both 平台的 ffi/plugin 也无此问题（三端原生均已链接）。

### 根因

ffi/plugin 包的 `Index.ets` 顶层存在 NAPI 原生导入：
```typescript
import nativeModule from 'libmy_ffi_android.so'   // ← 模块加载时执行
export const add = nativeModule.add                 // ← 立即访问 .add
```

在非目标平台运行时，该包未链接对应原生产物（android-only 包无 iOS xcframework；ios-only 包无 Android aar），导致：

```
import nativeModule from 'libxxx.so'
  → 运行时 requireNapi('libxxx.so') 未找到原生产物
  → nativeModule = undefined
  → 顶层 nativeModule.add 抛 TypeError: Cannot read property add of undefined
  → 模块加载阶段（func_main_0）失败，早于 aboutToAppear / try-catch
  → 页面加载失败 → 白屏
```

Android logcat 关键日志：
```
E ArkCompiler: Error: Load Page Failed: pages/Index, probably caused by reasons as follows:
1. there is a js error in target page;
E Ace: Failed to create reference for global.requireNapi
```

### 规避

**方案一（推荐）：跨端消费使用 `both` 包**

`platform: ['android','iOS']` 的 ffi/plugin 三端原生均链接，无条件导入即正常。三端通用的消费工程应优先集成 both 包。

**方案二：按平台选择性导入**

在消费工程中，仅导入当前平台支持的原生包：

| 运行平台 | 可导入 | 不可导入 |
|---|---|---|
| Android | android-only + both | ~~ios-only~~ |
| iOS | ios-only + both | ~~android-only~~ |
| HarmonyOS | 三类均可 | —（原生 `.so` 随 HAR 提供） |

> 单平台包仅用于目标平台的定制能力封装，不要无条件跨端导入。若需单份代码三端通用，需按平台条件加载（动态 import 或平台特定源码目录）。

> 此为 ArkUI-X 跨端 HAR 原生分发的已知限制：平台过滤只裁原生产物（aar/xcframework），不剔除 ETS；顶层 `.so` 导入在未链接平台解析为 undefined 致白屏。

---

## 9. 卸载/移除已集成的包（手动）

`ace` 没有移除消费工程已集成的 HAR 包的命令。移除一个已集成包需手动删除以下内容（以 `my_ffi_bothlib` 为包名）：

### 9.1 三方件本体
- 删除 `.arkui-x/plugins/my_ffi_bothlib/`（整目录，含 `.so`/aar/xcframework）

### 9.2 oh_modules 符号链接
- 删除 `entry/oh_modules/my_ffi_bothlib`（符号链接）
- 如根目录 `oh_modules/my_ffi_bothlib` 也存在，一并删除

### 9.3 依赖声明
- `entry/oh-package.json5`：删除 `"my_ffi_bothlib": "file:../.arkui-x/plugins/my_ffi_bothlib"` 这一行
- 删除 `entry/oh-package-lock.json5` 与根 `oh-package-lock.json5`（或重新执行 `ohpm install` 让其重新生成）

### 9.4 Android 侧
- `.arkui-x/android/app/build.gradle`：删除 `implementation files('libs/my_ffi_bothlib-release.aar')` 这一行
- 删除 `.arkui-x/android/app/libs/my_ffi_bothlib-release.aar`

### 9.5 iOS 侧
- 删除 `.arkui-x/ios/frameworks/my_ffi_bothlib.xcframework/`（整目录）
- `.arkui-x/ios/app.xcodeproj/project.pbxproj`：删除该 framework 的引用行（搜索 `my_ffi_bothlib` 关键字，移除 `PBXFileReference` / `PBXBuildFile` / `Frameworks` build phase 中的相关条目）

### 9.6 清理后
- 删除工程构建产物：`entry/build/`、`build/`、`.arkui-x/android/app/build/`、`.arkui-x/ios/build/`（或重新 `ace build`）
- 重新 `ohpm install` 刷新依赖锁

> 移除完成后，Index.ets 中对该包的 `import` 语句也要一并删除，否则编译报 `Cannot find module`。

---

## 10. 小结

| 场景 | 推荐做法 |
|---|---|
| 纯 ETS 逻辑库，三端通用 | 用 `package` 类型 |
| 跨端原生能力，三端都要 | 用 `ffi`/`plugin` + platform=both |
| 单平台能力 | 用 `ffi`/`plugin` + platform=android 或 iOS，**仅在目标平台消费工程导入** |
| 消费工程三端跑同一份代码 | 只导入 both 包 + package 包；单平台包按平台条件加载 |
