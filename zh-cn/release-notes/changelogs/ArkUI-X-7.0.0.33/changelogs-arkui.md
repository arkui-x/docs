# ArkUI子系统变更说明

## ArkUI-X Android和iOS应用NDK动态库按需加载变更

**访问级别**

其他

**变更原因**

新版本下ArkUI中NDK库改为动态库，采用按需加载策略。

**变更影响**

该变更为不兼容变更。

此变更涉及应用适配。

变更前：ArkUI-X Android应用使用C API方法时仅需引用头文件；ArkUI-X iOS应用使用C API方法时仅需引用头文件。

变更后：ArkUI-X Android应用使用C API方法时需要引用头文件，并在CMakeLists.txt文件中进行适配；ArkUI-X iOS应用使用C API方法时需要引用头文件，并在工程的project.pbxproj文件中进行适配。

**起始 API Level**

不涉及

**变更发生版本**

从ArkUI-X SDK 7.0.0.33开始。

**变更的接口/组件**

不涉及具体接口/组件

**适配指导**

- Android：需在Android应用工程的CMakeLists.txt文件中添加以下配置：

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

- iOS：需在iOS工程的project.pbxproj，按以下5个section依次添加libace_ndk.xcframework条目（可参照已有libarkui_ios.xcframework的条目结构）：

```
/* Begin PBXBuildFile section，新增两条 */
<UUID_A> /* libace_ndk.xcframework in Embed Frameworks */ = {isa = PBXBuildFile; fileRef = <UUID_C> /* libace_ndk.xcframework */; settings = {ATTRIBUTES = (CodeSignOnCopy, RemoveHeadersOnCopy, ); }; };
<UUID_B> /* libace_ndk.xcframework in Frameworks */ = {isa = PBXBuildFile; fileRef = <UUID_C> /* libace_ndk.xcframework */; };

/* Begin PBXCopyFilesBuildPhase section，Embed Frameworks的files数组追加一行 */
<UUID_A> /* libace_ndk.xcframework in Embed Frameworks */,

/* Begin PBXFileReference section，新增一条 */
<UUID_C> /* libace_ndk.xcframework */ = {isa = PBXFileReference; lastKnownFileType = wrapper.xcframework; name = libace_ndk.xcframework; path = "$(ARKUIX_SDK_HOME)/26.0.0/arkui-x/plugins/api/xcframeworkios-release/libace_ndk.xcframework"; sourceTree = "<group>"; };

/* Begin PBXFrameworksBuildPhase section，Frameworks的files数组追加一行 */
<UUID_B> /* libace_ndk.xcframework in Frameworks */,

/* Begin PBXGroup section，Frameworks分组的children数组追加一行 */
<UUID_C> /* libace_ndk.xcframework */,

```

说明：`<UUID_A>`、`<UUID_B>`、`<UUID_C>`需替换为三个不同的24位十六进制字符串（如`A00A58500E4E28A663A9816C`），且在pbxproj文件内全局唯一。`libace_ndk.xcframework`是动态库（在`ace_engine`的BUILD.gn中定义为`ohos_shared_library`），必须同时加入Frameworks（链接）和Embed Frameworks（嵌入）两个阶段，否则运行时会因镜像内无此动态库而崩溃。`$(ARKUIX_SDK_HOME)`为ArkUI-X SDK安装路径，`26.0.0`为启用的ArkUI-X SDK版本号，需根据开发环境实际情况进行修改。
