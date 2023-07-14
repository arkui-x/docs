# Quick Start Guide

## Overview

ACE Tools is a command line tool that allows developers to build applications runnable across the OpenHarmony, Android, and iOS platforms. Its functions include development environment check, project creation, building and packaging, and installation and debugging.

## Installing the Environment

Before using the CLI tool to create a project, check the local development environment.

1. NodeJS

   Run the `node -v` command to check the local Node.js version. If no Node.js version exists, [download and install a latest stable version](https://nodejs.org/en/download/). A version later than 14.19.1 is recommended.

2. OpenHarmony SDK

   An OpenHarmony SDK is required for HAP building. If no OpenHarmony SDK exists, [download and install one] (https://developer.harmonyos.com/en/develop/deveco-studio) depending on your system.

   **Recommended environment variable configuration for the SDK:**

   [Linux]

   ```shell
   // Configure environment variables.
   export OpenHarmony_HOME=/home/usrername/path-to-ohsdk
   export PATH=${OpenHarmony_HOME}/toolchains/versioncode:${PATH}
   ```

   [Mac]

   ```shell
   // Configure environment variables.
   export OpenHarmony_HOME=/Users/usrername/path-to-ohsdk
   export PATH=$OpenHarmony_HOME/toolchains/versioncode:$PATH
   ```

   [Windows]

   ```shell
   // Configure environment variables.
   set OpenHarmony_HOME=/Users/usrername/path-to-ohsdk
   set PATH=%PATH%;%OpenHarmony_HOME%/toolchains/versioncode
   ```

3. Android SDK

   An Android SDK is required for APK building. You can access the Android SDK through Android Studio. If you do not have Android Studio, [download and install one](https://developer.android.com/studio).

   **Recommended environment variable configuration for the SDK:**

   [Linux]

   ```shell
   // Configure environment variables.
   export ANDROID_HOME=/home/usrername/path-to-android-sdk
   export PATH=${ANDROID_HOME}/tools:${ANDROID_HOME}/tools/bin:${ANDROID_HOME}/build-tools/28.0.3:${ANDROID_HOME}/platform-tools:${PATH}
   ```

   [Mac]

   ```shell
   // Configure environment variables.
   export ANDROID_HOME=/Users/usrername/path-to-android-sdk
   export PATH=$ANDROID_HOME/tools:$ANDROID_HOME/tools/bin:$ANDROID_HOME/build-tools/28.0.3:$ANDROID_HOME/platform-tools:$PATH
   ```

   [Windows]

   ```shell
   // Configure environment variables.
   set ANDROID_HOME=/home/usrername/path-to-android-sdk
   set PATH=%PATH%;%ANDROID_HOME%/tools;%ANDROID_HOME%/tools/bin;%ANDROID_HOME%/build-tools/28.0.3;%ANDROID_HOME%/platform-tools
   ```

4. iOS SDK

   - Xcode and Command Line Tools for Xcode

   You can download Xcode and Command Line Tools for Xcode from Apple Store. Alternatively, you can run the following command to install the Command Line Tools:

   ```shell
   xcode-select --install
   ```

   - libimobiledevice

   ```shell
   brew install libimobiledevice
   ```

   For details, see [Libimobiledevice Installation](https://libimobiledevice.org).

   - ios-deploy

   ```shell
   brew install ios-deploy
   ```

   For details, see [ios-deploy Installation] (https://github.com/ios-control/ios-deploy).

## Installing Dependencies

Modify the npm source. Go to the user directory and add the following content to the **.npmrc** file:

```shell
@ohos:registry=https://repo.harmonyos.com/npm/
registry=https://repo.huaweicloud.com/repository/npm/
```

You can run the installation command in the toolchain's **cli** directory to install the CLI dependency package.

```shell
cd ace_tools/cli
npm install
npm install . -g
```

## How to Use

### Creating an Application

#### 1. Check the development environment.

   ```shell
   ace check
   ```

Run the `ace check` command to check the local development environment. Mandatory items must pass the check. Otherwise, you are not allowed to continue the subsequent operations.

Note: The development environment check focuses on the default installation and download paths of the SDK and IDE. If the SDK is downloaded through SDK Manager, the system checks whether the default environment variables **ANDROID_HOME** and **OpenHarmony_HOME** are properly configured.

#### 2. Check the device connection.

   ```shell
   ace devices
   ```

The command output contains a list of currently connected devices and device IDs. Take a record of the information as you need to specify a device ID as the input parameter of subsequent commands.

Note: This command has been integrated in `ace check` and can be skipped.

#### 3. Configure the development environment path.

   ```shell
   ace config
   ```

If the IDE and SDK are not downloaded to or installed in the default paths, you can run this command to change the paths.

#### 4. Create a project.

   The following describes how to create a project named **demo**:

   ```shell
   ace create project
   ? Please enter the project name: demo
   ? Please enter the packages (com.example.demo):com.example.demo
   ? Please enter the ACE version (1: declarative paradigm, 2: web-like paradigm): 1
   ```

Run the `ace create project` command (**project** can be omitted). Then, enter the project name **demo** and press **Enter** to retain the default bundle name. Enter **1** to create an ArkUI declarative paradigm-based application project.

A project named **demo** is thus created.

The key structure of the project is as follows:

```shell
demo/
├── android		// Android project for the cross-platform application
│   ├── app
│   │   ├── libs
│   │   └── src
│   │       ├── androidTest
│   │       ├── main
│   │       │   ├── AndroidManifest.xml
│   │       │   ├── assets	// Resource files generated from building of the cross-platform application
│   │       │   ├── java
│   │       │   │   └── com
│   │       │   │       └── example
│   │       │   │           └── demo
│   │       │   │               ├── MainActivity.java	// Inherited from the AceActivity base class provided by ArkUI
│   │       │   │               └── MyApplication.java	// Inherited from the AceApplication base class provided by ArkUI
│   │       │   └── res
│   │       └── test
│   └── settings.gradle
├── ios		// iOS project for the cross-platform application
│   ├── etsapp
│   │   ├── AppDelegate.h
│   │   ├── AppDelegate.mm	// Instantiate AceViewController and load the ArkUI page.
│   │   ├── Info.plist
│   │   └── main.m
│   ├── etsapp.xcodeproj
│   ├── frameworks
│   └── js
│   └── res
├── ohos	// OpenHarmony project for the cross-platform application
│   ├── build-profile.json5
│   ├── entry
│   │   └── src
│   │       └── main
│   │           ├── config.json
│   │           └── resources
└── source	// Source code for the cross-platform application
    └── entry
        └── src
            ├── main
            │   ├── ets
            │   │   └── MainAbility
            │   │       ├── app.ets
            │   │       ├── manifest.json	// Project configuration 
            │   │       └── pages
            │   │           └── index
            │   │               └── index.ets
            │   └── resources
            └── ohosTest

```

### Editing Code

After the project is created, you can develop code in the **source** directory of the project.

### Building the Project

Start building the **demo** project. This includes three parts: HAP, APK, and app.

   ```shell
   cd demo
   ```

1. Build the HAP. By default, this applies to all modules.

   ```shell
   ace build hap
   ```

   An HAP file is generated for each module. The default path is **demo/ohos/entry/build/default/outputs/default/**.

2. Build the HAP for the specified modules.

   ```shell
   ace build hap --target moduleName
   ace build hap --target "moduleName1 moduleName2 ..."
   ```

   An HAP file is generated for each specified module. The default path is **demo/ohos/moduleName/build/default/outputs/default/**.

   Note: In this version, you need to build the entry module first if you are building the HAP for specified modules. If you want to add multiple modules, add them to end of the **--target** parameter. If you do so, use quotation marks to include each target module, and separate each two of them with a space.

3. Build the APK. By default, this applies to the **app** module.

   ```shell
   ace build apk
   ```

   An APK file is generated. The default path is **demo/android/app/build/outputs/apk/debug/**.

4. Build the APK for the specified module.

   ```shell
   ace build apk --target moduleName
   ```

   An APK file is generated. The default path is **demo/android/app/build/outputs/apk/debug/**.

5. Build the app. By default, this applies to the **app** module.

   ```shell
   ace build app
   ```
   
   An app file is generated. The default path is **demo/ios/build/outputs/app/**.
   
6. Build the app for the specified module.

   ```shell
   ace build app --target moduleName
   ```

   An app file is generated. The default path is **demo/ios/build/outputs/app/**.

### Installing and Uninstalling the Application

Go to the demo project directory.

   ```shell
cd demo
   ```

1. Install the HAP.

   ```shell
   ace install hap
   ```

2. Install the HAP on the specified device.

   ```shell
   ace install hap -d deviceId
   ```

3. Install the APK.

   ```shell
   ace install apk
   ```

4. Install the APK on the specified device.

   ```shell
   ace install apk -d deviceId
   ```

5. Install the app.

   ```shell
   ace install app
   ```

6. Install the app on the specified device.

   ```shell
   ace install app -d deviceId
   ```

7. Uninstalling the HAP

   ```shell
   ace uninstall hap --bundle bundleName
   ```

8. Uninstall the HAP on the specified device.

   ```shell
   ace uninstall hap --bundle bundleName -d deviceId
   ```

9. Uninstall the APK.

   ```shell
   ace uninstall apk --bundle bundleName
   ```

10. Uninstall the APK on the specified device.

    ```shell
    ace uninstall apk --bundle bundleName -d deviceId
    ```

11. Uninstall the app.

    ```shell
    ace uninstall app --bundle bundleName
    ```

12. Uninstall the app on the specified device.

    ```shell
    ace uninstall app --bundle bundleName -d deviceId
    ```

###  Running the Application

1. Run the HAP.

   ```shell
   ace run hap
   ```

2. Run the HAP on the specified device.

   ```shell
   ace run hap -d deviceId
   ```

3. Run the APK.

   ```shell
   ace run apk
   ```

4. Run the APK on the specified device.

   ```shell
   ace run apk -d deviceId
   ```

5. Run the app.

   ```shell
   ace run app
   ```

6. Run the app on the specified device.

   ```shell
   ace run app -d deviceId
   ```

### Clearing Build Results

Clear build results of the HAP, APK, and app.

  ```shell
ace clean
  ```

### Generating Log Files

Log information of running applications is displayed continuously.

1. Display the HAP log.

  ```shell
ace log hap
  ```

2. Display the log for the HAP running on the specified device.

  ```shell
ace log hap -d deviceId
  ```

3. Display the APK log.

  ```shell
ace log apk
  ```

4. Display the log for the APK running on the specified device.

  ```shell
ace log apk -d deviceId
  ```

5. Display the app log.

  ```shell
ace log app
  ```

6. Display the log for the app running on the specified device.

  ```shell
ace log app -d deviceId
  ```

### Displaying Help Information

Display the help information about supported commands.

  ```shell
ace help
  ```

Display the help information about the specified command.

```shell
ace build --help
```

# Reference

- [CLI User Guide](https://gitee.com/arkui-x/cli/blob/master/README-EN.md)