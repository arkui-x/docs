# 应用权限列表

以下展示的是在跨平台上支持的权限列表。

## ohos.permission.CAMERA

允许应用使用相机拍摄照片和录制视频。

**Android平台**：支持，对应权限为android.permission.CAMERA, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**IOS平台**：支持，对应标签关键字为NSCameraUsageDescription, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。

**授权方式**：user_grant

## ohos.permission.MICROPHONE

允许应用使用麦克风。

**Android平台**：支持，对应权限为android.permission.RECORD_AUDIO, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**IOS平台**：支持，对应标签关键字为NSMicrophoneUsageDescription, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。

**授权方式**：user_grant

## ohos.permission.APPROXIMATELY_LOCATION

允许应用获取设备模糊位置信息。

**Android平台**：支持，对应权限为android.permission.ACCESS_COARSE_LOCATION, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**IOS平台**：不支持。

**授权方式**：user_grant

## ohos.permission.ACCESS_BLUETOOTH

允许应用接入蓝牙并使用蓝牙能力，例如配对、连接外围设备等。

**Android平台**：支持，对应权限列表如下, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

android.permission.BLUETOOTH

android.permission.BLUETOOTH_ADMIN

android.permission.BLUETOOTH_ADVERTISE

android.permission.BLUETOOTH_CONNECT

android.permission.BLUETOOTH_SCAN

android.permission.ACCESS_FINE_LOCATION

android.permission.ACCESS_COARSE_LOCATION

**IOS平台**：不支持。

**授权方式**：user_grant

## ohos.permission.READ_IMAGEVIDEO

允许读取用户公共目录的图片或视频文件。

**Android平台**：支持，对应权限为android.permission.READ_EXTERNAL_STORAGE, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**IOS平台**：支持，对应标签关键字为NSPhotoLibraryUsageDescription, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#ios)。


**授权方式**：user_grant

## ohos.permission.WRITE_IMAGEVIDEO

允许修改用户公共目录的图片或视频文件。

**Android平台**：支持，对应权限为android.permission.WRITE_EXTERNAL_STORAGE, 权限声明方式参考[权限声明指导](../security/declare-permissions.md#android)。

**IOS平台**：不支持。

**授权方式**：user_grant
