# ArkUI-X 1.0.0 Alpha

## Version Description

This issue is the first official release of ArkUI-X 1.0.0 Alpha. The version mainly packs the following capabilities:

- Basic compiler toolchain, which can generate ArkUI SDKs for iOS and Android to create applications that run on the target platform.
- Basic platform bridging capabilities, including the iOS and Android application loading entry points, lifecycle, event processing, and window system.
- Basic UI component adaptation capabilities, covering common UI components.
- XTS infrastructure, including common UI tests.

### Components

The ArkTS-based declarative development paradigm is supported.

### APIs

ArkUI cross-platform APIs include OpenHarmony APIs and custom extension APIs. You can implement the OpenHarmony APIs and extend custom JS APIs through the [API extension mechanism](../framework-dev/napi/napi-guidelines.md) on Android and iOS. For details about the examples, see [@ohos API Extension on Android](../contribute/tutorial/how-to-use-napi-on-Android.md) and [@ohos API Extension on iOS](../contribute/tutorial/how-to-use-napi-on-iOS.md).

>**NOTE**
>
>The ArkUI-X 1.0.0 Alpha version is the first preview version for the ArkUI-X project. It only provides implementation for the APIs listed in [OpenHarmony APIs with Cross-Platform Support](../application-dev/reference/apis/readme.md).

### Project Build

* Building a project on Android

```
./build.sh --product-name arkui-cross --target-os android --ccache
```

> **NOTE**
>
> The build result is stored in the **out** directory. The build output is used for Android application development. For details, see [Building ArkUI Applications on Android](../contribute/tutorial/how-to-build-Android-app.md).

* Building a project on iOS

```
./build.sh --product-name arkui-cross --target-os ios --ccache
```

> **NOTE**
>
> The build result is stored in the **out** directory. The build output is used for Android application development. For details, see [Building ArkUI Applications on iOS](../contribute/tutorial/how-to-build-iOS-app.md).

### Application Compiler Toolchain

ACE Tools is a command line (CLI) tool that allows ArkUI-X project developers to build applications. Its functions include development environment check, project creation, building and packaging, and installation and debugging. For details, see [ACE Tools](https://gitee.com/arkui-x/cli/blob/master/README-EN.md).


## Mapping

**Table 1** Mapping between versions and platforms

| Platform   | OS SDK Version             | Remarks|
| ----------- | ----------------------------------- | ---- |
| OpenHarmony | OpenHarmony 3.2 Beta (API level 9) | NA   |
| Android     | Quince Tart 10 (API level 29)       | NA   |
| iOS         | iOS 10                              | NA   |

>**NOTE**
>
>The alpha version is a preview version for trial use. The UI and APIs of this version may be unstable.

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
repo init -u git@gitee.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-1.0.0-Alpha --no-repo-verify
repo sync -c
```

**Method 2**

Use the **repo** tool to download the source code over HTTPS.


```
repo init -u https://gitee.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-1.0.0-Alpha --no-repo-verify
repo sync -c
```

### Acquiring Source Code from Mirrors

**Table 2** Mirrors for acquiring source code and SDKs

| Source Code                                 | Version| Mirror| SHA-256 Checksum|
| ----------------------------------------- | ------------ | ------------ | ---------------- |
| ArkUI-X full code| 1.0.0 Alpha    | [Site]()    | [Download]()|
| ArkUI-X SDK | 1.0.0 Alpha    | [Site]()    | [Download]()|

## Samples

### Samples List

**Table 3** [Samples](https://gitee.com/arkui-x/samples) list

| Sample     | Description                                                        |
| ------------- | ------------------------------------------------------------ |
| HelloWorld | The HelloWorld sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.|
| Shopping | The Shopping sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.  |
| HealthyDiet | The HealthyDiet sample app is created using ArkUI-X Command Line Tools and runs across Android, iOS, and OpenHarmony platforms.|
