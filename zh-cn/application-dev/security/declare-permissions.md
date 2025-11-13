
## 声明权限

应用在申请权限时，需在项目的配置文件中逐个声明所需权限，否则无法获取授权。其中，当前支持跨平台的权限可在[系统权限定义列表](../../security/permission-list.md)中查询。


#### ArkUI:
应用必须在module.json5配置文件的requestPermissions标签中声明权限。

示例如下：

``` JSON5
{
  "module": {
    // ···
    "requestPermissions": [
      {
        "name": "ohos.permission.CAMERA",
        "reason": "use for camera",
        "usedScene": {
          "abilities": [
            "FormAbility"
          ],
          "when": "inuse"
        }
      }
    ]
    // ···
  }
}
```

#### Android:
在在工程根目录下.arkui-x/android/app/src/main/AndroidManifest.xml的uses-permission字段中加入指定权限。

示例如下：

```
    <uses-permission android:name="android.permission.CAMERA" />
```

#### iOS
在工程根目录下.arkui-x/ios/app/Info.plist文件中添加对应权限声明。

示例如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>NSCameraUsageDescription</key>
    <string>需要您的同意, APP 才能访问相机</string>
</dict>
</plist>
```
