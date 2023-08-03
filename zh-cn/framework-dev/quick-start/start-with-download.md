# ArkUI-X代码下载

## ArkUI-X介绍

方舟开发框架（简称：ArkUI），是一套构建OpenHarmony应用界面的UI开发框架，它提供了简洁自然的UI语法与包括UI组件、动画机制、事件交互等在内的UI开发基础设施，以满足应用开发者的可视化界面开发需求。

ArkUI-X，是扩展ArkUI开发框架到多个OS平台，让开发者基于一套主代码，构建出精美的多平台应用。

ArkUI-X主库地址：[https://gitee.com/arkui-x](https://gitee.com/arkui-x)。

## 前提条件

1. 注册码云gitee帐号。

2. 注册码云SSH公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)。

3. 安装[git客户端](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)和[git-lfs](https://gitee.com/vcs-all-in-one/git-lfs?_from=gitee_search#downloading)并配置用户信息。

   ```shell
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. 安装码云repo工具，可以执行如下命令。

   ```shell
   curl -s https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 > /usr/local/bin/repo  #如果没有权限，可下载至其他目录，并将其配置到环境变量中
   chmod a+x /usr/local/bin/repo
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```

## 下载方法

- **ArkUI-X主干代码获取**

  方式一（推荐）：通过repo + ssh下载（需注册公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)）。

  ```shell
  mkdir arkui
  cd arkui
  repo init -u git@gitee.com:arkui-x/manifest.git -b master --no-repo-verify
  repo sync -c --no-tags -j12
  ```

  方式二：通过repo + https下载。

  ```shell
  mkdir arkui
  cd arkui
  repo init -u https://gitee.com/arkui-x/manifest.git -b master --no-repo-verify
  repo sync -c
  repo forall -c 'git lfs pull'
  ```

- **ArkUI-X开发分支代码获取**

  方式一（推荐）：通过repo + ssh下载（需注册公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)）。

  ```shell
  mkdir arkui
  cd arkui
  repo init -u git@gitee.com:arkui-x/manifest.git -b master --no-repo-verify -m arkui-dev.xml
  repo sync -c --no-tags -j12
  ```

  方式二：通过repo + https下载。

  ```shell
  mkdir arkui
  cd arkui
  repo init -u https://gitee.com/arkui-x/manifest.git -b master --no-repo-verify -m arkui-dev.xml
  repo sync -c --no-tags -j12
  ```