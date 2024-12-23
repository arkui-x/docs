# Downloading ArkUI-X Code

## ArkUI-X Overview

ArkUI is a UI development framework for building OpenHarmony applications. It provides simple and natural UI syntax and UI development infrastructure including UI components, animation mechanisms, and event interaction, to meet the visualized GUI development requirements of application developers.

ArkUI-X extends the ArkUI development framework to multiple OS platforms, which allows you develop multi-platform applications based on a set of main code.

ArkUI-X master repository: [https://gitcode.com/arkui-x](https://gitcode.com/arkui-x)

## Prerequisites

1. You have registered an account with GitCode.

2. You have registered an SSH public key for access to GitCode.

3. You have installed the [Git client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [git-lfs](https://gitcode.com/vcs-all-in-one/git-lfs?_from=gitcode_search#downloading), and configured basic user information.

   ```shell
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. You have installed the **repo** tool.

   ```shell
   curl -s https://https://gitcode.com/gitcode-dev/repo/blob/main/repo-py3 > /usr/local/bin/repo  # If you do not have the access permission to this directory, download the tool to any other accessible directory and configure the directory to the environment variable.
   chmod a+x /usr/local/bin/repo
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```

## How to Obtain

- **Obtaining the ArkUI-X master code**

  Method 1 (recommended): Use the **repo** tool to download the source code over SSH. (You must have registered an SSH public key for access to GitCode.)

  ```shell
  repo init -u git@gitcode.com:arkui-x/manifest.git -b master --no-repo-verify
  repo sync -c --no-tags -j12
  repo forall -c 'git lfs pull'
  ```

  Method 2: Use the **repo** tool to download the source code over HTTPS.

  ```shell
  repo init -u https://gitcode.com/arkui-x/manifest.git -b master --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```

- **Obtaining the ArkUI-X 1.0.0 release code**

  Method 1 (recommended): Use the **repo** tool to download the source code over SSH. (You must have registered an SSH public key for access to GitCode.)

  Obtain the source code from the version branch. You can obtain the latest source code of the version branch, which includes the code that has been incorporated into the branch up until the time you run the following commands:
  ```shell
  repo init -u git@gitcode.com:arkui-x/manifest.git -b ArkUI-X-1.0.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```
  
  Obtain the source code from the version tag, which is the same as that released with the version.
  ```shell
  repo init -u git@gitcode.com:arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```

  Method 2: Use the **repo** tool to download the source code over HTTPS.

  Obtain the source code from the version branch. You can obtain the latest source code of the version branch, which includes the code that has been incorporated into the branch up until the time you run the following commands:
  ```shell
  repo init -u https://gitcode.com/arkui-x/manifest.git -b ArkUI-X-1.0.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```
  
  Obtain the source code from the version tag, which is the same as that released with the version.
  ```shell
  repo init -u https://gitcode.com/arkui-x/manifest.git -b refs/tags/ArkUI-X-v1.0.0-Release --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```
