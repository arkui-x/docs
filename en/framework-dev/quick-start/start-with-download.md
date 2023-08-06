# Downloading ArkUI-X Project Code

## ArkUI-X Project

ArkUI is a UI development framework for building OpenHarmony applications. It provides simple and natural UI syntax and UI development infrastructure including UI components, animation mechanisms, and event interaction, to meet the visualized GUI development requirements of application developers.

The ArkUI-X project extends the ArkUI development framework to multiple OS platforms, enabling developers to develop multi-platform applications based on a set of main code.

[ArkUI-X project master repository](https://gitee.com/arkui-x)

## Prerequisites

1. Register your account with Gitee.

2. Register an SSH public key for access to Gitee. For details, see [Gitee Support Center](https://gitee.com/help/articles/4191).

3. Install the [Git client](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) and [git-lfs](https://gitee.com/vcs-all-in-one/git-lfs?_from=gitee_search#downloading), and configure basic user information.

   ```shell
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. Run the following commands to install the **repo** tool:

   ```shell
   curl -s https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 > /usr/local/bin/repo  # If you do not have the access permission to this directory, download the tool to any other accessible directory and configure the directory to the environment variable.
   chmod a+x /usr/local/bin/repo
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```

## How to Obtain

- **Obtaining the ArkUI-X Project Master Code**

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

- **Obtaining the ArkUI-X Project Branch Code**

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
