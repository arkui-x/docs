# Xcode升级16版本项目修改项

### 升级LLVM

在[GitHub](https://github.com/llvm/llvm-project/releases/tag/llvmorg-16.0.5)下载16.0.5版本的[clang+llvm-16.0.5-arm64-apple-darwin22.0.tar.xz](https://github.com/llvm/llvm-project/releases/download/llvmorg-16.0.5/clang+llvm-16.0.5-arm64-apple-darwin22.0.tar.xz)，下载完成后解压文件，文件夹名改为llvm，替换prebuilts/clang/ohos/darwin-arm64下的llvm文件（原llvm文件夹可以改名为llvm-14）。

### 修改源码

* 全局搜索`#include <__nullptr>`并注释，目前有5个文件引入了这个头文件（render_context.mm/rs_surface_cpu.mm/rs_surface_gpu.mm/WindowView.mm/virtual_rs_window.mm）。

* plugins/util/plugin/ios/util_plugin.m 
  ```objc
  35 NSStringEncoding result = NSUTF8StringEncoding; // 原为nil
  ```

* plugins/request/core/task/frameworks/ios/dbkit/IosTaskConfig.m
  ```objc
  124 info.tries = 0; // 原为@""
  ```

* foundation/arkui/ace_engine/frameworks/base/log/log_wrapper.h

  ```c++
  273 const char* p = strrchr_(name, separator); // 自定义，否则会报缺少memrchr方法
  ...
  308 private:
      ...
      static char* strrchr_(const char* str, char c) // 自定义的寻找字符位置方法
      {
          if (!str) {
              return nullptr;
          }
          const unsigned char* p = reinterpret_cast<const unsigned char*>(str);
          const unsigned char* last = nullptr;
          const unsigned char uc = static_cast<unsigned char>(c);
          while (*p != '\0') {
              if (*p == uc) {
                  last = p;
              }
              ++p;
          }
          if (uc == '\0') {
              return reinterpret_cast<char*>(const_cast<unsigned char*>(p));
          }
          return reinterpret_cast<char*>(const_cast<unsigned char*>(last));
      }

* build/config/security/BUILD.gn

  ```python
  45 # "-enable-trivial-auto-var-init-zero-knowing-it-will-be-removed-from-clang", # 注释掉这一行，llvm16已废弃

* build/misc/mac/find_sdk.py

  ```python
  77 sdks = [re.findall('^MacOSX([1,2][0,1,2,3,4,5,6]\.\d+)\.sdk$', s) for s in # 添加MacOSX 15

* build_plugins/config/ios/BUILD.gn

  ```python
  134 defines += [ "__IPHONE_OS_VERSION_MIN_REQUIRED=100000" ]
  135 if (defined(Xcode_version) && Xcode_version >= 26) {
        framework_dirs += [ "$sysroot/System/Library/SubFrameworks" ]
      }