# 应用权限列表

以下展示的是在跨平台上支持的权限列表。

## ohos.permission.CAMERA

允许应用使用相机拍摄照片和录制视频。

**Android平台**：支持，对应权限为android.permission.CAMERA，权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**iOS平台**：支持，对应标签关键字为NSCameraUsageDescription，权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。

**授权方式**：user_grant

## ohos.permission.MICROPHONE

允许应用使用麦克风。

**Android平台**：支持，对应权限为android.permission.RECORD_AUDIO，权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**iOS平台**：支持，对应标签关键字为NSMicrophoneUsageDescription，权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。

**授权方式**：user_grant

## ohos.permission.LOCATION

允许应用获取设备精确位置信息。

**Android平台**：支持，对应权限为android.permission.ACCESS_FINE_LOCATION，权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。注意：部分Android 12版本上，targetSdkVersion不低于Android 12（API 31）的应用单独申请该权限时请求可能被系统忽略（不弹框），此时可降级申请ohos.permission.APPROXIMATELY_LOCATION（对应android.permission.ACCESS_COARSE_LOCATION，不受该限制）。

**iOS平台**：支持，对应标签关键字为NSLocationWhenInUseUsageDescription，权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。

**授权方式**：user_grant

## ohos.permission.APPROXIMATELY_LOCATION

允许应用获取设备模糊位置信息。

**Android平台**：支持，对应权限为android.permission.ACCESS_COARSE_LOCATION，权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**iOS平台**：支持，对应标签关键字为NSLocationWhenInUseUsageDescription，权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。定位授权后即视为已授权；如用户在系统设置中关闭“精确位置”开关，ohos.permission.LOCATION返回未授权而该权限仍为已授权，两者配合可实现降级。

**授权方式**：user_grant

## ohos.permission.ACCESS_BLUETOOTH

允许应用接入蓝牙并使用蓝牙能力，例如配对、连接外围设备等。

**Android平台**：支持，对应权限与应用的生效版本相关（取设备系统版本与应用targetSdkVersion的较小值），权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

- 生效版本低于Android 12（API 31）：对应权限为android.permission.BLUETOOTH、android.permission.BLUETOOTH_ADMIN、android.permission.ACCESS_FINE_LOCATION与android.permission.ACCESS_COARSE_LOCATION，需声明这四个权限，该版本下蓝牙扫描与发现依赖定位权限。
- 生效版本为Android 12（API 31）及以上：对应权限为android.permission.BLUETOOTH_ADVERTISE、android.permission.BLUETOOTH_CONNECT与android.permission.BLUETOOTH_SCAN，需声明这三个权限，建议同时为android.permission.BLUETOOTH与android.permission.BLUETOOTH_ADMIN声明android:maxSdkVersion="30"。该版本下如使用经典蓝牙发现（startBtDiscovery），仍需单独申请定位权限。

**iOS平台**：不支持。

**授权方式**：user_grant

## ohos.permission.READ_IMAGEVIDEO

允许读取用户公共目录的图片或视频文件。

**Android平台**：支持，对应权限与应用的生效版本相关（取设备系统版本与应用targetSdkVersion的较小值），权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

- 生效版本低于Android 13（API 33）：对应权限为android.permission.READ_EXTERNAL_STORAGE，声明该权限即可。
- 生效版本为Android 13（API 33）及以上：对应权限为android.permission.READ_MEDIA_IMAGES与android.permission.READ_MEDIA_VIDEO，需声明这两个权限；若应用仍需支持Android 12（API 32）及以下设备，需同时声明android.permission.READ_EXTERNAL_STORAGE并配android:maxSdkVersion="32"（READ_MEDIA_*权限在API 33以下不存在，低版本设备上运行时仍申请该权限，未声明则申请失败）。

**iOS平台**：支持，对应标签关键字为NSPhotoLibraryUsageDescription，权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。

**授权方式**：user_grant

## ohos.permission.WRITE_IMAGEVIDEO

允许修改用户公共目录的图片或视频文件。

**Android平台**：支持，对应权限为android.permission.WRITE_EXTERNAL_STORAGE，权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。注意：当应用的生效版本（取设备系统版本与应用targetSdkVersion的较小值）不低于Android 11（API 30）时，该权限不再提供任何访问能力，申请时系统不弹框并直接返回未授权（authResults为-1）。

**iOS平台**：不支持。

**授权方式**：user_grant
