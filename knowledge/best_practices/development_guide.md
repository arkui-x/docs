# ArkUI-X 开发最佳实践

本文档提供 ArkUI-X 跨平台开发的最佳实践指南，涵盖应用开发、框架开发、构建配置、测试调试等各个方面。

## 目录

- [环境配置](#环境配置)
- [构建系统](#构建系统)
- [应用开发最佳实践](#应用开发最佳实践)
- [框架开发最佳实践](#框架开发最佳实践)
- [测试与调试](#测试与调试)
- [性能优化](#性能优化)
- [常见问题](#常见问题)

## 环境配置

### 系统要求

| 平台 | 最低要求 | 推荐配置 |
|-----|---------|---------|
| **Linux** | Ubuntu 18.04 64-bit | Ubuntu 20.04+ |
| **macOS** | macOS 11.6.2 | macOS 12+ |
| **Python** | 3.7.4 | 3.8+ |
| **JDK** | 17.0.10 | 17+ |
| **Shell** | bash | bash (非 dash/sh) |

### Linux 环境配置

**1. 安装依赖**：

```bash
sudo apt-get update
sudo apt-get install binutils git-core gnupg flex bison gperf build-essential \
    zip curl zlib1g-dev gcc-multilib g++-multilib libc6-dev-i386 \
    lib32ncurses5-dev x11proto-core-dev libx11-dev lib32z-dev \
    ccache libgl1-mesa-dev libxml2-utils xsltproc unzip m4
```

**2. 配置 Shell**：

```bash
# 确保 bash 为默认 shell（非 dash/sh）
sudo dpkg-reconfigure dash
# 选择 "No"

# 验证
ls -l /bin/sh
# 应该指向 bash 而非 dash
```

**3. 配置 Java 环境**：

```bash
# 下载 JDK 17.0.10
wget https://repo.huaweicloud.com/openjdk/17.0.10/jdk-17.0.10_linux-x64_bin.tar.gz

# 解压并设置环境变量
export JAVA_HOME=/path/to/java-sdk
export PATH=${JAVA_HOME}/bin:${PATH}

# 添加到 ~/.bashrc
echo 'export JAVA_HOME=/path/to/java-sdk' >> ~/.bashrc
echo 'export PATH=${JAVA_HOME}/bin:${PATH}' >> ~/.bashrc
```

**4. 配置 Android SDK**（如需编译 Android）：

```bash
# 下载 Android SDK Command Line Tools
# 使用 sdkmanager 安装必需的包

./sdkmanager --install "ndk;21.3.6528147" --sdk_root=/path/to-android-sdk
./sdkmanager --install "platforms;android-26" --sdk_root=/path/to-android-sdk
./sdkmanager --install "build-tools;28.0.3" --sdk_root=/path/to-android-sdk

# 设置环境变量
export ANDROID_HOME=/path/to-android-sdk
export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
```

### macOS 环境配置

**1. 安装依赖**：

```bash
# 安装 Homebrew（如果未安装）
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# 安装工具
brew install wget coreutils
```

**2. 安装 Xcode Command Line Tools**：

```bash
xcode-select --install
```

**3. 配置 Java 环境**：

```bash
# 下载 JDK 17.0.10 for macOS
# 设置环境变量
export JAVA_HOME=/Users/username/path-to-java-sdk
export PATH=$JAVA_HOME/bin:$PATH

# 添加到 ~/.zshrc 或 ~/.bash_profile
echo 'export JAVA_HOME=/Users/username/path-to-java-sdk' >> ~/.zshrc
echo 'export PATH=$JAVA_HOME/bin:$PATH' >> ~/.zshrc
```

### 下载预编译工具链

```bash
# 在代码检出或更新后，下载预编译工具链
./build/prebuilts_download.sh --build-arkuix --skip-ssl
```

## 构建系统

### 主要构建命令

**基本构建**：

```bash
# 构建整个项目
./build.sh

# 指定产品
./build.sh --product-name arkui-x

# 指定目标操作系统
./build.sh --product-name arkui-x --target-os android
./build.sh --product-name arkui-x --target-os ios
```

**高级构建选项**：

```bash
# 构建特定组件
./build.sh --product-name arkui-x --target-os android --build-target ace_engine

# 使用自定义 GN 参数
./build.sh --product-name arkui-x --target-os android --gn-args="enable_profiler=true"

# 使用自定义 Ninja 参数
./build.sh --product-name arkui-x --target-os android --ninja-args="-dkeeprsp"

# 调试模式
./build.sh --product-name arkui-x --target-os android --log-level debug

# 显示帮助
./build.sh --help
```

### 构建产物位置

```
out/
└── [product-name]/        # 如 out/arkui-x/
    ├── .gn/              # GN 生成文件
    ├── libs/             # 编译的库文件
    │   ├── *.so         # Android 共享库
    │   └── *.a          # 静态库
    ├── packages/         # 安装包
    │   ├── *.apk        # Android 安装包
    │   └── *.ipa        # iOS 安装包
    └── ...              # 其他输出
```

### 构建最佳实践

**1. 增量构建**：

```bash
# 首次全量构建
./build.sh --product-name arkui-x --target-os android

# 后续增量构建（仅编译修改的文件）
./build.sh --product-name arkui-x --target-os android
```

**2. 并行编译**：

```bash
# 使用多核加速（默认已启用）
# 可通过 GN 参数控制
./build.sh --gn-args="jobs=8"
```

**3. 清理构建**：

```bash
# 清理特定产品的构建输出
rm -rf out/[product-name]/

# 重新构建
./build.sh --product-name arkui-x --target-os android
```

**4. 调试构建**：

```bash
# Debug 构建
./build.sh --product-name arkui-x --target-os android --build-type debug

# Release 构建（默认）
./build.sh --product-name arkui-x --target-os android --build-type release
```

## 应用开发最佳实践

### ArkTS 代码规范

**1. 组件声明**：

```typescript
// ✅ 推荐：使用 @Component 装饰器
@Component
struct MyComponent {
    @State text: string = 'Hello'

    build() {
        Column() {
            Text(this.text)
        }
    }
}

// ❌ 避免：不使用装饰器
class MyComponent {
    text: string = 'Hello'
}
```

**2. 状态管理**：

```typescript
// ✅ 正确：使用 @State 管理内部状态
@Component
struct Counter {
    @State count: number = 0

    build() {
        Button(`Count: ${this.count}`)
            .onClick(() => {
                this.count++
            })
    }
}

// ✅ 正确：使用 @Prop 接收父组件数据
@Component
struct Child {
    @Prop value: number
}

// ✅ 正确：使用 @Link 双向绑定
@Component
struct Parent {
    @State count: number = 0

    build() {
        Child({ value: $count })  // 传递 $count 实现双向绑定
    }
}
```

**3. 生命周期使用**：

```typescript
@Component
struct MyComponent {
    @State data: string = ''

    // 组件即将出现
    aboutToAppear() {
        console.info('Component about to appear')
        this.loadData()
    }

    // 组件即将消失
    aboutToDisappear() {
        console.info('Component about to disappear')
        this.cleanup()
    }

    async loadData() {
        // 加载数据
    }

    cleanup() {
        // 清理资源
    }

    build() {
        Text(this.data)
    }
}
```

### 性能优化建议

**1. 避免不必要的渲染**：

```typescript
// ✅ 正确：使用 @Prop 而非 @State 减少更新
@Component
struct ExpensiveComponent {
    @Prop staticData: string  // 仅父组件更新时更新
    @State localState: string  // 仅本地更新

    build() {
        Text(`${this.staticData} - ${this.localState}`)
    }
}

// ❌ 错误：过度使用 @State
@Component
struct ExpensiveComponent {
    @State staticData: string  // 不必要的响应式
    @State localState: string

    build() {
        Text(`${this.staticData} - ${this.localState}`)
    }
}
```

**2. 使用 LazyForEach**：

```typescript
// ✅ 正确：长列表使用 LazyForEach
@Component
struct ListExample {
    @State data: Array<Item> = []

    build() {
        List() {
            LazyForEach(new MyDataSource(this.data), (item: Item) => {
                ListItem() {
                    Text(item.name)
                }
            })
        }
    }
}

// ❌ 错误：长列表使用 ForEach
@Component
struct ListExample {
    @State data: Array<Item> = []

    build() {
        List() {
            ForEach(this.data, (item: Item) => {
                ListItem() {
                    Text(item.name)
                }
            })
        }
    }
}
```

**3. 合理使用动画**：

```typescript
// ✅ 正确：使用显式动画
@Component
struct AnimatedComponent {
    @State translate: number = 0

    animateTo() {
        animateTo({ duration: 300 }, () => {
            this.translate = 100
        })
    }

    build() {
        Text('Animate Me')
            .translate({ x: this.translate })
    }
}

// ❌ 错误：频繁触发动画
@Component
struct AnimatedComponent {
    @State translate: number = 0

    onScroll() {
        this.translate++  // 每次滚动都触发，性能差
    }
}
```

### 资源管理

**1. 资源文件组织**：

```
entry/src/main/
├── resources/
│   ├── base/
│   │   ├── element/      # 元素资源
│   │   │   └── string.json
│   │   ├── media/        # 媒体资源
│   │   │   ├── icon.png
│   │   │   └── image.jpg
│   │   └── profile/      # 配置文件
│   └── [限定词]/         # 特定配置
│       └── element/
└── ets/
    └── pages/
```

**2. 资源引用**：

```typescript
// ✅ 正确：使用 $r 引用资源
Text($r('app.string.app_name'))
Image($r('app.media.icon'))

// ❌ 错误：硬编码字符串
Text('App Name')
Image('/resources/base/media/icon.png')
```

## 框架开发最佳实践

### 代码规范

**1. 命名约定**：

```cpp
// ✅ 正确：类名使用 PascalCase
class FrameNode {
};

// ✅ 正确：成员变量使用 camelCase，尾随下划线
class FrameNode {
    int patternIndex_;
    std::string id_;
};

// ✅ 正确：成员函数使用 PascalCase
class FrameNode {
    void OnModifyDone();
    void GetPattern();
};

// ✅ 正确：常量使用 kPascalCase
constexpr int kMaxFrameRate = 60;
constexpr size_t kDefaultTimeout = 5000;
```

**2. 智能指针使用**：

```cpp
// ✅ 正确：使用共享所有权
std::shared_ptr<Pattern> pattern_;

// ✅ 正确：独占所有权
std::unique_ptr<RenderNode> renderNode_;

// ✅ 正确：弱引用避免循环依赖
std::weak_ptr<FrameNode> parent_;

// ❌ 错误：裸指针容易内存泄漏
Pattern* pattern_;  // 谁负责释放？
```

**3. 错误处理**：

```cpp
// ✅ 正确：使用可选类型返回
std::optional<Pattern*> GetPattern() {
    if (!pattern_) {
        return std::nullopt;
    }
    return pattern_.get();
}

// 使用
auto pattern = GetPattern();
if (pattern.has_value()) {
    pattern.value()->Process();
}

// ❌ 错误：返回 nullptr 检查麻烦
Pattern* GetPattern() {
    if (!pattern_) {
        return nullptr;
    }
    return pattern_.get();
}
```

### 条件编译

**1. 平台检测**：

```cpp
// ✅ 正确：使用标准宏
#ifdef ANDROID_PLATFORM
    // Android 特定代码
#elif defined(IOS_PLATFORM)
    // iOS 特定代码
#endif

// ✅ 正确：功能检测
#ifdef ENABLE_ROSEN_BACKEND
    // Rosen 渲染代码
#endif
```

**2. 最小化条件编译**：

```cpp
// ✅ 推荐：文件分离
// android/android_impl.cpp
void PlatformSpecificFunction() {
    // Android 实现
}

// ios/ios_impl.mm
void PlatformSpecificFunction() {
    // iOS 实现
}

// ❌ 避免：大块条件编译
// common/impl.cpp
void PlatformSpecificFunction() {
#ifdef ANDROID_PLATFORM
    // 100 行 Android 代码
#elif defined(IOS_PLATFORM)
    // 100 行 iOS 代码
#endif
}
```

### 日志规范

**1. 日志级别使用**：

```cpp
// ✅ 正确：使用适当的日志级别
HILOG_DEBUG("Detailed debug info: %{public}s", data);  // 调试信息
HILOG_INFO("Process started: id=%{public}d", id);      // 一般信息
HILOG_WARN("Resource not found, using default");       // 警告
HILOG_ERROR("Failed to load: %{public}s", error);      // 错误

// ❌ 错误：滥用日志级别
HILOG_ERROR("User clicked button");  // 这不是错误
```

**2. 日志格式**：

```cpp
// ✅ 正确：使用格式化字符串
HILOG_INFO("Processing request: id=%{public}d, user=%{public}s", id, user);

// ❌ 错误：字符串拼接
HILOG_INFO(("Processing request: id=" + std::to_string(id)).c_str());
```

**3. 条件编译日志**：

```cpp
// ✅ 正确：使用日志宏
#ifdef ACE_INSTANCE_LOG
    HILOG_INFO("Instance %{public}d: created", instanceId_);
#endif

// 或使用调试宏
HILOG_DEBUG("Variable value: %{public}d", value);  // Release 构建自动移除
```

## 测试与调试

### XTS 测试

**1. 测试文件组织**：

```
test/xts/
├── android/
│   └── xts_2.0/
│       └── [module_name]/
│           ├── BUILD.gn
│           └── src/
│               └── main/
│                   ├── ets/
│                   │   └── test/
│                   │       └── [module_name].test.ets
│                   └── resources/
└── ios/
    └── xts_2.0/
        └── [module_name]/
```

**2. 编写测试用例**：

```typescript
// ✅ 正确：使用标准测试框架
import { describe, beforeAll, beforeEach, afterEach, afterAll, it, expect } from '@ohos/hypium'

export default function featureTest() {
    describe('MyFeature', () => {
        beforeAll(() => {
            console.info('Test suite setup')
        })

        afterAll(() => {
            console.info('Test suite teardown')
        })

        beforeEach(() => {
            console.info('Test case setup')
        })

        afterEach(() => {
            console.info('Test case teardown')
        })

        it('should pass basic test', 0, () => {
            expect(true).assertEqual(true)
        })

        it('should handle async operations', 0, async () => {
            const result = await asyncOperation()
            expect(result).assertEqual('expected')
        })
    })
}
```

**3. 运行测试**：

```bash
# Android XTS 测试
cd test/xts/android/xts_2.0
./run.sh [module_name]

# 构建特定测试
./build.sh --product-name arkui-x --target-os android --build-target [test_target]
```

### 调试技巧

**1. 日志调试**：

```bash
# 查看 Ace 日志
hilog -T ace

# 查看特定组件日志
hilog -T MyComponent

# 持续查看日志
hilog -T ace | grep "keyword"
```

**2. 构建调试**：

```bash
# 使用详细日志
./build.sh --product-name arkui-x --target-os android --log-level debug

# 查看 Ninja 依赖关系
./build.sh --ninja-args="-dkeeprsp"

# 查看构建时间
./build.sh --ninja-args="-j1 -d stat"
```

**3. Android 调试**：

```bash
# 查看 Android 日志
adb logcat | grep -E "(ace|arkui)"

# 查看 JNI 日志
adb logcat | grep "JNI"

# 查看 Java 崩溃
adb logcat | grep "FATAL"
```

**4. iOS 调试**：

```bash
# 查看 iOS 设备日志
idevicesyslog | grep -i "arkui"

# 使用 Xcode 调试
# 1. 在 Xcode 中打开项目
# 2. 设置断点
# 3. 运行并调试
```

### 常见问题排查

**问题 1: 构建失败 - 找不到头文件**

```bash
# 检查 include_dirs 是否正确
grep -r "missing_header.h" .
# 确认头文件路径

# 检查 BUILD.gn
include_dirs = [
  "interface",
  "//foundation/arkui/napi/native/common",
]
```

**问题 2: 链接错误 - 未定义的引用**

```bash
# 检查 deps 是否完整
grep -r "undefined_symbol" .

# 确认依赖关系
deps = [
  ":common",
  "//foundation/arkui/napi:napi",
]
```

**问题 3: 运行时崩溃**

```bash
# 查看崩溃日志
adb logcat | grep -A 20 "FATAL"

# 检查空指针
# 添加日志确认对象是否为空
if (ptr == nullptr) {
    HILOG_ERROR("Null pointer");
    return;
}

# 检查数组越界
HILOG_DEBUG("Array index: %{public}d, size: %{public}zu", index, size);
```

## 性能优化

### 编译优化

**当前优化状态**：

| 优化项 | 状态 | 影响 |
|-------|-----|------|
| `-Os` | ✅ 已启用 | 优化文件大小 |
| `-ffunction-sections` | ✅ 已启用 | 分离函数 |
| `-fdata-sections` | ✅ 已启用 | 分离数据 |
| `-fvisibility=hidden` | ✅ 已启用 | 符号隐藏 |
| `NDEBUG` | ✅ 已启用 | 禁用断言 |
| `-Wl,--gc-sections` | ✅ 已启用 | 链接时删除未使用代码 |

**潜在优化**：

```bash
# 1. 禁用实例日志（减少 2-5% 大小）
# 在 BUILD.gn 中添加
defines = [ "ACE_INSTANCE_LOG=0" ]

# 2. 启用 LTO（链接时优化）
./build.sh --gn-args="use_lto=true"

# 3. 剥离调试符号
strip --strip-unneeded libace_core.z.so
```

### 运行时优化

**1. 减少布局计算**：

```typescript
// ✅ 正确：避免嵌套布局
Column() {
    Text('Title')
    Text('Content')
}

// ❌ 错误：过度嵌套
Column() {
    Column() {
        Column() {
            Text('Title')
        }
    }
}
```

**2. 使用缓存**：

```cpp
// ✅ 正确：缓存计算结果
class LayoutCache {
private:
    std::optional<Size> cachedSize_;

public:
    Size CalculateSize() {
        if (!cachedSize_.has_value()) {
            cachedSize_ = PerformCalculation();
        }
        return cachedSize_.value();
    }

    void Invalidate() {
        cachedSize_.reset();
    }
};
```

**3. 异步加载**：

```typescript
// ✅ 正确：异步加载资源
async loadImage() {
    try {
        const image = await imageSource.createImage(src)
        this.imagePixelMap = await image.createPixelMap()
    } catch (error) {
        console.error('Failed to load image')
    }
}

// ❌ 错误：同步阻塞
loadImage() {
    const image = imageSource.createImageSync(src)  // 阻塞线程
    this.imagePixelMap = image.createPixelMapSync()
}
```

## 常见问题

### Q: 如何判断使用适配层还是插件层？

**A**:
- **适配层** (`foundation/arkui/ace_engine/adapter/`)：框架能力
  - Window 管理
  - 输入处理
  - 字体渲染
  - 键盘管理

- **插件层** (`plugins/`)：子系统 API
  - 网络请求
  - 文件系统
  - 数据存储
  - 传感器

### Q: 如何添加新的 @ohos API？

**A**: 参考 [插件子系统适配指南](../plugin_adaptation/README.md)：
1. 创建插件目录结构
2. 更新 `plugins/plugin_lib.gni`
3. 更新 `interface/sdk/plugins/api/apiConfig.json`
4. 更新 `build_plugins/sdk/arkui_cross_sdk_description_std.json`
5. 实现平台适配代码
6. 添加 XTS 测试
7. 添加 API 文档

### Q: 如何验证插件是否正确配置？

**A**: 运行以下验证命令：
```bash
# 1. 检查插件注册
grep "plugin_name" plugins/plugin_lib.gni

# 2. 检查 SDK 描述
grep "plugin_name" build_plugins/sdk/arkui_cross_sdk_description_std.json

# 3. 检查 API 配置
grep "ohos.plugin_name" interface/sdk/plugins/api/apiConfig.json

# 4. 验证 JSON 文件
python3 -m json.tool build_plugins/sdk/arkui_cross_sdk_description_std.json
python3 -m json.tool interface/sdk/plugins/api/apiConfig.json

# 5. 构建验证
./build.sh --product-name arkui-x --target-os android
```

### Q: 如何处理平台特有的 API？

**A**: 在平台实现层处理，保持业务逻辑平台无关：
```cpp
// interface/xxx_interface.h - 平台无关
class IXxxStrategy {
    virtual void Execute() = 0;
};

// android/xxx_android.cpp - Android 特有功能
class AndroidXxxStrategy : public IXxxStrategy {
    void Execute() override {
        #ifdef ANDROID_ENABLE_FEATURE_X
            // Android 特有功能
        #endif
    }
};
```

### Q: 如何提高构建速度？

**A**:
1. **使用 ccache**：
   ```bash
   export USE_CCACHE=1
   ```

2. **增量编译**：
   ```bash
   # 仅编译修改的文件
   ./build.sh --product-name arkui-x --target-os android
   ```

3. **并行编译**：
   ```bash
   ./build.sh --gn-args="jobs=$(nproc)"
   ```

4. **使用 SSD**：将源代码放在 SSD 上可显著提高速度

### Q: 如何贡献代码？

**A**:
1. 遵循代码规范
2. 添加单元测试
3. 更新文档
4. 提交 PR 前运行测试
5. 确保所有测试通过

## 参考文档

- [系统架构](../architecture/system_architecture.md)
- [插件子系统适配指南](../plugin_adaptation/README.md)
- [主 CLAUDE.md](../../CLAUDE.md)
- [OpenHarmony 文档](https://docs.openharmony.cn/)
