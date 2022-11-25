# ArkUI-X 0.1.0 Beta

## Version Description

This issue is the first official release of ArkUI-X 0.1.0 Beta. The version mainly packs the following capabilities:

- Basic compiler toolchain, which can generate ArkUI SDKs for iOS/Android to create applications that run on the target platform.
- Basic platform bridging capabilities, including the iOS/Android application loading entry points, lifecycle, event processing, and window system.
- Basic UI component adaptation capabilities, covering common UI components (for details, see [ArkUI-X App Samples](https://gitee.com/arkui-x/samples)).
- XTS infrastructure, including common UI tests.

### ArkUI Components

The ArkTS-based declarative development paradigm is supported.

### APIs

ArkUI-X APIs include OpenHarmony APIs and custom extension APIs. You can implement the OpenHarmony APIs and extend JS APIs through the [API extension mechanism](../framework-dev/napi/napi-guidelines.md) on Android and iOS. For details about the examples, see [@ohos API Extension on Android](../contribute/tutorial/how-to-use-napi-on-Android.md) and [@ohos API Extension on iOS](../contribute/tutorial/how-to-use-napi-on-iOS.md).

>**NOTE**
>
>The Beta version currently does not implement OpenHarmony APIs.

### Project Build

* Building a project on Android

```
./build.sh --product-name arkui-cross --target-os android --ccache
```

> **NOTE**
>
> The build result is stored in the **out/arkui-cross** directory. The build output is used for Android application development. For details, see [Building ArkUI Applications on Android](../contribute/tutorial/how-to-build-Android-app.md).

* Building a project on iOS

```
./build.sh --product-name arkui-cross --target-os ios --ccache
```

> **NOTE**
>
> The build result is stored in the **out/arkui-cross** directory. The build output is used for iOS application development. For details, see [Building ArkUI Applications on iOS](../contribute/tutorial/how-to-build-iOS-app.md).

### Application Compiler Toolchain

ArkUI-X Command Line Tools is a command line (CLI) tool that allows developers to build applications runnable across the OpenHarmony, Android, and iOS platforms. Its functions include development environment check, project creation, building and packaging, and installation and debugging. For details, see [Quick Start Guide](https://gitee.com/arkui-x/cli/blob/master/README-EN.md).


## Mapping

**Table 1** Mapping between versions and platforms

| Platform   | OS SDK Version             | Remarks|
| ----------- | ----------------------------------- | ---- |
| OpenHarmony | 3.1 Release (API Version 8 Release) | NA   |
| Android     | Quince Tart 10 (API level 29)       | NA   |
| iOS         | iOS 10                              | NA   |

>**NOTE**
>
>The Beta version is a preliminary release open only to specific developers. It does not promise API stability and may require tolerance of instability.

## Source Code Acquisition


### Prerequisites

1. Register your account with Gitee.

2. Register an SSH public key for access to Gitee.

3. Install the [git client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [git-lfs](https://gitee.com/vcs-all-in-one/git-lfs?_from=gitee_search#downloading), and configure user information.
  
   ```
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. Run the following commands to install the **repo** tool:
  
   ```
   curl -s https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 > /usr/local/bin/repo  # If you do not have the permission, download the tool to another directory and configure it as an environment variable by running the chmod a+x /usr/local/bin/repo command.
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```


### Acquiring Source Code Using the repo Tool

**Method 1 (recommended)**

Use the **repo** tool to download the source code over SSH. (You must have an SSH public key for access to Gitee.)


```
repo init -u git@gitee.com:arkui-x/manifest.git -b refs/tags/ArkUI-0.1.0-Beta --no-repo-verify
repo sync -c
```

**Method 2**

Use the **repo** tool to download the source code over HTTPS.


```
repo init -u https://gitee.com/arkui-x/manifest.git -b refs/tags/ArkUI-0.1.0-Beta --no-repo-verify
repo sync -c
```

### Acquiring Source Code from Mirrors

**Table 2** Mirrors for acquiring source code and SDKs

| Source Code                                 | Version| Mirror| SHA-256 Checksum|
| ----------------------------------------- | ------------ | ------------ | ---------------- |
| Full code (supporting OpenHarmony, Android, and iOS)| 0.1.0 Beta    | [Site]()    | [Download]()|
| ArkUI-X SDK (Android)     | 0.1.0 Beta    | [Site]()    | [Download]()|
| ArkUI-X SDK (iOS)         | 0.1.0 Beta    | [Site]()    | [Download]()|

## Samples

### Samples List

**Table 3** [Samples](https://gitee.com/arkui-x/samples) list

| Sample     | Description                                                        |
| ------------- | ------------------------------------------------------------ |
| HelloWorld | The HelloWorld sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.|
| Shopping | The Shopping sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.  |
| HealthyDiet | The HealthyDiet sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.|
