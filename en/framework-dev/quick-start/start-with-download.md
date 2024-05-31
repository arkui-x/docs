# Downloading ArkUI-X Code

## ArkUI-X Overview

ArkUI is a UI development framework for building OpenHarmony applications. It provides simple and natural UI syntax and UI development infrastructure including UI components, animation mechanisms, and event interaction, to meet the visualized GUI development requirements of application developers.

ArkUI-X extends the ArkUI development framework to multiple OS platforms, which allows you develop multi-platform applications based on a set of main code.

ArkUI-X master repository: [https://gitee.com/arkui-x](https://gitee.com/arkui-x)

## Prerequisites

1. You have registered an account with Gitee.

2. You have registered an SSH public key for access to Gitee.

3. You have installed the [Git client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [git-lfs](https://gitee.com/vcs-all-in-one/git-lfs?_from=gitee_search#downloading), and configured basic user information.

   ```shell
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. You have installed the **repo** tool.

   ```shell
   curl -s https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 > /usr/local/bin/repo  # If you do not have the access permission to this directory, download the tool to any other accessible directory and configure the directory to the environment variable.
   chmod a+x /usr/local/bin/repo
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```

## How to Obtain

- **Obtaining the ArkUI-X Master Code**

  Method 1 (recommended): Use the **repo** tool to download the source code over SSH. (You must have registered an SSH public key for access to Gitee.)

  ```shell
  mkdir arkui
  cd arkui
  repo init -u git@gitee.com:arkui-x/manifest.git -b master --no-repo-verify
  repo sync -c --no-tags -j12
  ```

  Method 2: Use the **repo** tool to download the source code over HTTPS.

  ```shell
  mkdir arkui
  cd arkui
  repo init -u https://gitee.com/arkui-x/manifest.git -b master --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```

- **Obtaining the ArkUI-X Release Code**

  Method 1 (recommended): Use the **repo** tool to download the source code over SSH. (You must have registered an SSH public key for access to Gitee.)

  ```shell
  mkdir arkui
  cd arkui
  repo init -u git@gitee.com:arkui-x/manifest.git -b master --no-repo-verify -m arkui-dev.xml
  repo sync -c --no-tags -j12
  ```

  Method 2: Use the **repo** tool to download the source code over HTTPS.

  ```shell
  mkdir arkui
  cd arkui
  repo init -u https://gitee.com/arkui-x/manifest.git -b master --no-repo-verify -m arkui-dev.xml
  repo sync -c --no-tags -j12
  ```
