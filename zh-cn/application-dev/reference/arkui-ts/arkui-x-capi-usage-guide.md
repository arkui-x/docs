# ArkUI-X应用C API接口使用指导

从OpenHarmony SDK **7.0.0.33**版本开始，ArkUI NDK库（Android的`libace_ndk.so` / iOS的`libace_ndk.xcframework`）由静态库调整为动态库，采用按需加载策略。ArkUI-X Android和iOS应用在使用C API（NDK）时，除引用头文件外，还需按以下章节完成工程配置，否则运行时将因找不到NDK符号而崩溃。

## Android应用适配

在Android应用工程的`CMakeLists.txt`中添加以下配置，将`libace_ndk.so`以动态库方式引入并链接到目标：

```
···
add_library(arkui_ndk SHARED IMPORTED GLOBAL)
set_target_properties(
    arkui_ndk
    PROPERTIES IMPORTED_LOCATION
    ${CMAKE_CURRENT_SOURCE_DIR}/../../../libs/${CMAKE_ANDROID_ARCH_ABI}/libace_ndk.so
)
···
target_link_libraries(entry PUBLIC arkui_android log arkui_ndk)
```

## iOS应用适配

在iOS工程的`project.pbxproj`文件中，按以下5个section依次添加`libace_ndk.xcframework`条目（可参照已有`libarkui_ios.xcframework`的条目结构）：

```
/* Begin PBXBuildFile section，新增两条 */
<UUID_A> /* libace_ndk.xcframework in Embed Frameworks */ = {isa = PBXBuildFile; fileRef = <UUID_C> /* libace_ndk.xcframework */; settings = {ATTRIBUTES = (CodeSignOnCopy, RemoveHeadersOnCopy, ); }; };
<UUID_B> /* libace_ndk.xcframework in Frameworks */ = {isa = PBXBuildFile; fileRef = <UUID_C> /* libace_ndk.xcframework */; };

/* Begin PBXCopyFilesBuildPhase section，Embed Frameworks的files数组追加一行 */
<UUID_A> /* libace_ndk.xcframework in Embed Frameworks */,

/* Begin PBXFileReference section，新增一条 */
<UUID_C> /* libace_ndk.xcframework */ = {isa = PBXFileReference; lastKnownFileType = wrapper.xcframework; name = libace_ndk.xcframework; path = frameworks/libace_ndk.xcframework; sourceTree = "<group>"; };

/* Begin PBXFrameworksBuildPhase section，Frameworks的files数组追加一行 */
<UUID_B> /* libace_ndk.xcframework in Frameworks */,

/* Begin PBXGroup section，Frameworks分组的children数组追加一行 */
<UUID_C> /* libace_ndk.xcframework */,

```

- **UUID说明**：`<UUID_A>`、`<UUID_B>`、`<UUID_C>`需替换为三个不同的24位十六进制字符串（如`A00A58500E4E28A663A9816C`），且在pbxproj文件内全局唯一。
- **Embed说明**：`libace_ndk.xcframework`是动态库（在`ace_engine`的`BUILD.gn`中定义为`ohos_shared_library`），必须同时加入Frameworks（链接）和Embed Frameworks（嵌入）两个阶段，否则运行时会因镜像内无此动态库而崩溃。
