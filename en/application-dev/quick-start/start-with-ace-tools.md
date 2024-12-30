# ACE Tools Quick Start

## Overview

ACE Tools is a command line (CLI) tool that allows ArkUI-X application developers to build applications on the OpenHarmony, HarmonyOS, Android, and iOS platforms. Its functions include development environment check, project creation, building and packaging, and installation and debugging.

## Environment Setup

**Prerequisites**: Ubuntu 18.04 or later, macOS 11.6.2 or later, or Windows 10

1. Configure Node.js.

   Node.js is required for running ACE Tools and the OpenHarmony SDK. You are advised to download Node.js 14.19.1 to 16.19.1. To check the local Node.js version, run the **node -v** command. If no Node.js is installed or the installed one does not meet the version requirement, [download](https://nodejs.org/en/download/) and install a stable version and configure it to environment variables.

2. Configure the Java environment.

   The Java environment is required for packaging Android, OpenHarmony, and HarmonyOS applications. You are advised to download JDK11.0.2 or later. You can download it [here](https://repo.huaweicloud.com/openjdk/). The following are recommended environment variable configurations:

   [macOS]

   ```shell
   // Configure environment variables.
   export JAVA_HOME=/path-to-java-sdk
   export PATH=$JAVA_HOME/bin:$PATH
   ```

   [Windows]

   ```shell
   // Configure environment variables.
   set JAVA_HOME=/path-to-java-sdk
   set PATH=%PATH%;%JAVA_HOME%/bin
   ```

3. Configure the ohpm environment.

   ohpm, short for OpenHarmony Package Manager, is a command-line tool for managing third-party libraries in OpenHarmony application projects. Check the ohpm installation path in DevEco Studio under **File** > **Settings** > **Build, Execution, Deployment** > **Ohpm** and configure it to environment variables.

4. Configure the ArkUI-X SDK environment.

   Check the download path of the ArkUI-X SDK in DevEco Studio under **File** > **Settings** > **ArkUI-X** (**Preferences** > **ArkUI-X** for macOS) and configure it to environment variables. The following are recommended environment variable configurations:

   [macOS]

   ```shell
   // Configure environment variables.
   export ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   ```

   [Windows]

   ```shell
   // Configure environment variables.
   set ARKUIX_SDK_HOME=/path-to-arkui-x-sdk
   ```

## Command Execution

   - Modify the npm source by going to the user directory and adding the following content to the .npmrc file:

   ```shell
   @ohos:registry=https://repo.harmonyos.com/npm/
   registry=https://repo.huaweicloud.com/repository/npm/
   ```

   - Run the following global commands to install ACE Tools:

   ```shell
   cd arkui-x/toolchains/ace_tools    // Access the directory where ACE Tools is located based on the ArkUI-X SDK download path.
   npm install
   npm install . -g
   ```

## How to Use

### Checking the Development Environment

   ```shell
   ace check
   ```

Run the **ace check** command to check whether the local development environment is complete for the ArkUI-X application.

Note: The development environment check mainly checks the IDE and SDKs in the default installation and download paths. If the displayed information is inconsistent with the actual situation, run the **ace config** command to specify the actual IDE installation path and SDK download paths.

### Creating an Application

   The following example creates the **demo** project in the stage model:

   ```shell
   ace create project
   ? Please enter the project name: demo
   ? Please enter the bundle name (com.example.demo):com.example.demo
   ? Please enter the system (1: OpenHarmony, 2: HarmonyOS): 1
   ? Please enter the project type (1: Application, 2: Library): 1
   ? Please enter the template (1: Empty Ability, 2: Native C++): 1 // Create an empty Ability or native C++ project.
   ```

Run the **ace create project** command and enter the project name **demo**.

### Running Your Application

* Installing and running your application on an Android device

```shell
cd demo
ace run apk
```

* Installing and running your application on an iOS device

```shell
cd demo
ace run app
```

* Installing and running your application on an OpenHarmony device

```shell
cd demo
ace run hap
```

After the preceding commands are executed, the application is built, packaged, and installed to the target device.

## Reference

- [ACE Tools](https://gitcode.com/arkui-x/cli/blob/ArkUI-X-5.0.2-Release/README-EN.md)
- [Ubuntu Environment Configuration](https://gitcode.com/arkui-x/docs/blob/ArkUI-X-5.0.2-Release/en/application-dev/tutorial/how-to-configure-dev-environment.md)

<!--no_check-->
