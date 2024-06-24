# Checking Path Validity



## Overview

This topic describes how to check the validity of a path configured in **config**. It provides the following information:

- Methods for checking path validity
- Mappings between configuration items and environment variables
- Path related configuration items

## Methods for Checking Path Validity

####  SDK


| Configuration Item         | Check Method                                                    |
| ----------------- | ------------------------------------------------------------ |
| --android-sdk     | 1. Check whether the configured path exists.<br>2. Check whether the configured path is available. (In Windows, the path cannot be a drive letter. In macOS and Linux, the path cannot be the system root directory.)<br>3. Check whether the **build-tools** and **platform-tools** folders exist in the configured path.|
| --arkui-x-sdk     | 1. Check whether the configured path exists.<br>2. Check whether the configured path is available. (In Windows, the path cannot be a drive letter. In macOS and Linux, the path cannot be the system root directory.)<br>3. Check whether the **versioncode** folder exists in the configured path.<br>4. Check whether the **versioncode** folder contains the **arkui-x** folder.<br>5. Check whether the **licenses** folder exists in the configured path.|
| --harmonyos-sdk   | 1. Check whether the configured path exists.<br>2. Check whether the configured path is available. (In Windows, the path cannot be a drive letter. In macOS and Linux, the path cannot be the system root directory.)<br>New version:<br>1. Check whether the name of any folder in the configured path contains **HarmonyOS**.<br>2. Check whether the **sdk-pkg.json** file exists in a folder of the configured path.<br>3. Check whether the **licenses** folder exists in the configured path.<br>If a folder whose name contains **HarmonyOS** does not exist in the path or the **sdk-pkg.json** file does not exist in the folder, follow the method used in the old version.<br>Old version:<br>1. Check whether the **hmscore** and **openharmony** folders exist in the configured path.<br>2. Check whether the **openharmony** folder contains the **versioncode** folder.<br>3. Check whether the **versioncode** folder contains the **toolchains** folder.<br>4. Check whether the **licenses** folder exists in the configured path.|
| --openharmony-sdk | 1. Check whether the configured path exists.<br>2. Check whether the configured path is available. (In Windows, the path cannot be a drive letter. In macOS and Linux, the path cannot be the system root directory.)<br>3. Check whether the **versioncode** folder exists in the configured path.<br>4. Check whether the **versioncode** folder contains the **toolchains** folder.<br>5. Check whether the **licenses** folder exists in the configured path.|

####  DevEco Studio


| Configuration Item             | Check Method                                                    |
| --------------------- | ------------------------------------------------------------ |
| --android-studio-path | 1. Check whether the configured path exists.<br>2. For Windows, check whether the executable file **studio.exe** or **studio64.exe** exists in the **bin** directory or in the configured path.<br>For Linux, check whether the executable file **studio.sh** or **studio64.sh** exists in the **bin** directory or in the configured path.<br>For macOS, check whether the executable file **studio** or **studio64** exists in the **Contents/MacOS** directory or in the configured path.|
| --deveco-studio-path  | 1. Check whether the path exists.<br>2. For Windows, check whether the executable file **devecostudio.exe** or **devecostudio64.exe** exists in the **bin** directory or in the configured path.<br>For macOS, check whether the executable file **devecostudio** or **devecostudio64** exists in the **Contents/MacOS** directory or in the configured path.|

####  CMD


| Configuration Item    | Check Method                                                    |
| ------------ | ------------------------------------------------------------ |
| --java-sdk   | 1. Check whether the configured path exists.<br>2. For Windows, check whether the executable file **java.exe** exists in the configured path. For Linux or macOS, check whether the executable file **java** exists in the **bin** directory.|
| --nodejs-dir | 1. Check whether the configured path exists.<br>2. For Windows, check whether the executable file **node.exe** exists in the configured path. For Linux or macOS, check whether the executable file **node** exists in the **bin** directory.|
| --ohpm-dir   | 1. Check whether the configured path exists.<br>2. Check whether the executable file **ohpm** exists in the **bin** directory.|
| --build-dir  | 1. Check whether the configured path exists.                                          |

## Mappings Between Configuration Items and Environment Variables

| Configuration Item             | Environment Variable      |
| --------------------- | ---------------- |
| --android-sdk         | ANDROID_HOME     |
| --arkui-x-sdk         | ARKUIX_SDK_HOME  |
| --harmonyos-sdk       | HarmonyOS_HOME   |
| --openharmony-sdk     | OpenHarmony_HOME |
| --java-sdk            | JAVA_HOME        |
| --android-studio-path | Android Studio   |
| --deveco-studio-path  | DevEco Studio    |
| --nodejs-dir          | NODE_HOME        |
| --ohpm-dir            | OHPM_HOME        |

## Path Related Configuration Items

- --java-sdk

- --nodejs-dir

- --ohpm-dir
