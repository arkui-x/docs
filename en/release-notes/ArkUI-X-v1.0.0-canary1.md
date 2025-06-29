# ArkUI-X 1.0.0 Canary1

## Version Description

This issue is the first official release of ArkUI-X 1.0.0 Canary1. The version mainly packs the following capabilities:

- Basic compiler toolchain, which can generate ArkUI SDKs for iOS and Android to create applications that run on the target platform.
- Basic platform bridging capabilities, including the iOS and Android application loading entry points, lifecycle, event processing, and window system.
- Basic UI component adaptation capabilities, covering common UI components.
- XTS infrastructure, including common UI tests.

### Components

The ArkTS-based declarative development paradigm is supported.

### APIs

ArkUI cross-platform APIs include OpenHarmony APIs and custom extension APIs. You can implement the OpenHarmony APIs and extend custom JS APIs through the [API extension mechanism](../framework-dev/napi/napi-guidelines.md) on Android and iOS. For details about the examples, see [@ohos API Extension on Android](../application-dev/tutorial/how-to-use-napi-on-android.md) and [@ohos API Extension on iOS](../application-dev/tutorial/how-to-use-napi-on-ios.md).

>**NOTE**
>
>The ArkUI-X 1.0.0 Canary1 version is the first preview version for the ArkUI-X project. It only provides implementation for the APIs listed in [OpenHarmony APIs with Cross-Platform Support](../application-dev/reference/apis/README.md).

### Project Build

* Building a project on Android

```
./build.sh --product-name arkui-x --target-os android --ccache
```

> **NOTE**
>
> The build result is stored in the **out** directory. The build output is used for Android application development. For details, see [Building ArkUI Applications on Android](../application-dev/tutorial/how-to-integrate-arkui-into-android.md).

* Building a project on iOS

```
./build.sh --product-name arkui-x --target-os ios --ccache
```

> **NOTE**
>
> The build result is stored in the **out** directory. The build output is used for Android application development. For details, see [Building ArkUI Applications on iOS](../application-dev/tutorial/how-to-integrate-arkui-into-ios.md).

### Application Compiler Toolchain

ACE Tools is a command line (CLI) tool that allows ArkUI-X project developers to build applications. Its functions include development environment check, project creation, building and packaging, and installation and debugging. For details, see [ACE Tools](https://gitcode.com/arkui-x/cli/blob/master/README-EN.md).


## Mapping

**Table 1** Mapping between versions and platforms

| Platform   | OS SDK Version             | Remarks|
| ----------- | ----------------------------------- | ---- |
| OpenHarmony | 4.0 (API Version 10) |  Beta2  |
| Android     | Android 8<sup>+</sup> (API level 26<sup>+</sup>)       | NA   |
| iOS         | iOS 10<sup>+</sup>                             | NA   |

>**NOTE**
>
>The canary1 version is a preview version for trial use. The UI and APIs of this version may be unstable.

## Source Code Acquisition

### Prerequisites

1. Register your account with GitCode.

2. Register an SSH public key for access to GitCode.

3. Install the [git client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [git-lfs](https://gitcode.com/vcs-all-in-one/git-lfs?_from=gitcode_search#downloading), and configure user information.
  
   ```
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. Run the following commands to install the **repo** tool:
  
   ```
   curl -s https://gitcode.com/gitcode-dev/repo/blob/main/repo-py3 > /usr/local/bin/repo  # If you do not have the permission, download the tool to another directory and configure it as an environment variable by running the chmod a+x /usr/local/bin/repo command.
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```


### Acquiring Source Code Using the repo Tool

**Method 1 (recommended)**

Use the **repo** tool to download the source code over SSH. (You must have an SSH public key for access to GitCode.)


```
repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Canary1 --no-repo-verify
repo sync -c
```

**Method 2**

Use the **repo** tool to download the source code over HTTPS.


```
repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Canary1 --no-repo-verify
repo sync -c
```

## Acquiring SDK from Mirrors

**Table 2** Acquiring SDK from Mirrors

| SDK Version                                 | Version| Mirror| SHA-256 Checksum|
| ----------------------------------------- | ------------ | ------------ | ---------------- |
| ArkUI-X SDK package（macOS）  | 1.0.0 Canary1 | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-x64-0.0.9.6-Canary1.zip)   | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-x64-0.0.9.6-Canary1.zip.sha256) |
| ArkUI-X SDK package（macOS-M1）    | 1.0.0 Canary1 | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-arm64-0.0.9.6-Canary1.zip)   | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/darwin/arkui-x-darwin-arm64-0.0.9.6-Canary1.zip.sha256) |
| ArkUI-X SDK package（Windows）    | 1.0.0 Canary1 | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/windows/arkui-x-windows-x64-0.0.9.6-Canary1.zip)   | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/windows/arkui-x-windows-x64-0.0.9.6-Canary1.zip.sha256) |
| ArkUI-X SDK package（Linux）    | 1.0.0 Canary1 | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/linux/arkui-x-linux-x64-0.0.9.6-Canary1.zip)     | [Download](https://repo.huaweicloud.com/arkui-crossplatform/sdk/0.0.9.6/linux/arkui-x-linux-x64-0.0.9.6-Canary1.zip.sha256) |
## Samples

### Samples List

**Table 3** [Samples](https://gitcode.com/arkui-x/samples) list

| Sample     | Description                                                        |
| ------------- | ------------------------------------------------------------ |
| HelloWorld | The HelloWorld sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.|
| Shopping | The Shopping sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.  |
| HealthyDiet | The HealthyDiet sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.|
