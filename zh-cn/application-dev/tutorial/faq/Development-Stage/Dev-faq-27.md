## ArkUI-X使用C API方法编译失败

### 问题现象
在使用ArkUI-X SDK 7.0.0.33及以上版本的ArkUI-X应用中使用了C API方法，构建时报错导致编译失败。不同平台的报错形式不同：

- Android：构建apk时有`ld.lld: error: undefined symbol`形式的报错。
- iOS：在Xcode中构建时有`Undefined symbol`或`Undefined symbols for architecture arm64`形式的报错。

### 问题原因

此问题的核心原因是在ArkUI-X SDK 7.0.0.33及以上版本中，ArkUI中NDK库改为动态库，采用按需加载策略，如需使用C API方法必须分别在Android和iOS应用工程中修改对应配置。

### 解决办法

#### Android

在Android应用工程的CMakeLists.txt文件中添加以下配置：

- 文件路径：.arkui-x/android/app/src/main/cpp/CMakeLists.txt

```
...
if(${CMAKE_ANDROID_ARCH_ABI} STREQUAL "arm64-v8a")
    set(ARKUI_ABI_DIR "android-arm64-release")
elseif(${CMAKE_ANDROID_ARCH_ABI} STREQUAL "armeabi-v7a")
    set(ARKUI_ABI_DIR "android-arm-release")
elseif(${CMAKE_ANDROID_ARCH_ABI} STREQUAL "x86_64")
    set(ARKUI_ABI_DIR "android-x86_64")
else()
    message(FATAL_ERROR "Unsupported ABI: ${CMAKE_ANDROID_ARCH_ABI}")
endif()

add_library(arkui_ndk SHARED IMPORTED GLOBAL)
set_target_properties(
    arkui_ndk
    PROPERTIES IMPORTED_LOCATION
    $ENV{ARKUIX_SDK_HOME}/26.0.0/arkui-x/plugins/api/lib/arkui/${ARKUI_ABI_DIR}/libace_ndk.so
)
...
target_link_libraries(entry PUBLIC arkui_android log arkui_ndk)
```

说明：`$ENV{ARKUIX_SDK_HOME}`为ArkUI-X SDK安装路径，`26.0.0`为启用的ArkUI-X SDK版本号，需根据开发环境实际情况进行修改。

#### iOS

iOS端通过将`libace_ndk.xcframework`加入Xcode工程，把C API对应的NDK符号引入工程。具体步骤如下：

- 编辑iOS工程的`project.pbxproj`，按以下5个section依次添加`libace_ndk.xcframework`条目（可参照已有`libarkui_ios.xcframework`的条目结构）：

  - 文件路径：.arkui-x/ios/app.xcodeproj/project.pbxproj

  ```
  /* Begin PBXBuildFile section，新增两条 */
  <UUID_A> /* libace_ndk.xcframework in Embed Frameworks */ = {isa = PBXBuildFile; fileRef = <UUID_C> /* libace_ndk.xcframework */; settings = {ATTRIBUTES = (CodeSignOnCopy, RemoveHeadersOnCopy, ); }; };
  <UUID_B> /* libace_ndk.xcframework in Frameworks */ = {isa = PBXBuildFile; fileRef = <UUID_C> /* libace_ndk.xcframework */; };

  /* Begin PBXCopyFilesBuildPhase section，Embed Frameworks的files数组追加一行 */
  <UUID_A> /* libace_ndk.xcframework in Embed Frameworks */,

  /* Begin PBXFileReference section，新增一条 */
  <UUID_C> /* libace_ndk.xcframework */ = {isa = PBXFileReference; lastKnownFileType = wrapper.xcframework; name = libace_ndk.xcframework; path = "$(ARKUIX_SDK_HOME)/26.0.0/arkui-x/plugins/api/xcframework/ios-release/libace_ndk.xcframework"; sourceTree = "<group>"; };

  /* Begin PBXFrameworksBuildPhase section，Frameworks的files数组追加一行 */
  <UUID_B> /* libace_ndk.xcframework in Frameworks */,

  /* Begin PBXGroup section，Frameworks分组的children数组追加一行 */
  <UUID_C> /* libace_ndk.xcframework */,
  ```

  说明：`<UUID_A>`、`<UUID_B>`、`<UUID_C>`需替换为三个不同的24位十六进制字符串（如`A00A58500E4E28A663A9816C`），且在pbxproj文件内全局唯一。`libace_ndk.xcframework`是动态库（在`ace_engine`的BUILD.gn中定义为`ohos_shared_library`），必须同时加入Frameworks（链接）和Embed Frameworks（嵌入）两个阶段，否则运行时会因镜像内无此动态库而崩溃。`$(ARKUIX_SDK_HOME)`为ArkUI-X SDK安装路径，`26.0.0`为启用的ArkUI-X SDK版本号，需根据开发环境实际情况进行修改。

- 在Xcode中执行Product → Clean Build Folder（Shift+Cmd+K）后重新编译。
