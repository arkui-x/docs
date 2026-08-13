# ArkUI子系统变更说明

## cl.arkui.1 ArkUI-X Android和iOS应用NDK动态库按需加载变更说明

**访问级别**

其它

**变更原因**

新版本下ArkUI中NDK库改为动态库，采用按需加载策略。

**变更影响**

此变更涉及应用适配。

- 变更前：ArkUI-X Android应用使用C API方法时仅需引用头文件；ArkUI-X iOS应用使用C API方法时仅需引用头文件。

- 变更后：ArkUI-X Android应用使用C API方法时需要引用头文件，并在CMakeLists.txt文件中进行适配；ArkUI-X iOS应用使用C API方法时需要引用头文件，并在工程的project.pbxproj文件中进行适配。

**起始 API Level**

不涉及API

**变更发生版本**

从ArkUI-X SDK 7.0.0.33开始。

**变更的接口/组件**

不涉及具体接口/组件

**适配指导**

- Android：需在Android应用工程的CMakeLists.txt文件中添加以下配置：

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

- iOS：需在iOS工程的project.pbxproj，按以下5个section依次添加libace_ndk.xcframework条目（可参照已有libarkui_ios.xcframework的条目结构）：

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

说明：`<UUID_A>`、`<UUID_B>`、`<UUID_C>`需替换为三个不同的24位十六进制字符串（如`A00A58500E4E28A663A9816C`），且在pbxproj文件内全局唯一。`libace_ndk.xcframework`是动态库（在`ace_engine`的BUILD.gn中定义为`ohos_shared_library`），必须同时加入Frameworks（链接）和Embed Frameworks（嵌入）两个阶段，否则运行时会因镜像内无此动态库而崩溃。
