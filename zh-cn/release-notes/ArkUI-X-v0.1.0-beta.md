# ArkUI-X 0.1.0 Beta

## 版本概述

首次发布ArkUI-X 0.1.0 Beta版本，主要能力范围包括：

- 基础的编译工具链，能够生成iOS/Android的SDK，编译出相应的应用并运行在目标平台上。
- 基础的平台桥接能力，包括iOS/Android的应用加载入口，生命周期，事件处理，窗口系统等。
- 基础的UI组件适配能力，包括常用的UI组件。
- XTS基础设施，包括常用的UI测试。

### 组件参考

开发范式支持基于ArkTS的声明式开发范式。

### 接口范围

ArkUI跨平台接口包含OpenHarmony接口和自定义扩展接口，在Android和iOS平台通过[API扩展机制](../framework-dev/napi/napi-guidelines.md)扩展JS接口和实现OpenHarmony接口定义。比如：[Android平台接口实现示例](../contribute/tutorial/how-to-use-napi-on-Android.md)和[iOS平台接口实现示例](../contribute/tutorial/how-to-use-napi-on-iOS.md)。

>说明：Beta版本为跨平台项目首次发布的预览版本，暂不提供OpenHarmony接口定义的跨平台实现。

### 项目编译

* ArkUI支持Android平台

```
./build.sh --product-name arkui-cross --target-os android --ccache
```

> 说明：编译结果输出在**out**目录下，编译输出件用于ArkUI跨平台Android侧应用开发，[输出件列表]()和[使用指南](../contribute/tutorial/how-to-build-Android-app.md)。

* ArkUI支持iOS平台

```
./build.sh --product-name arkui-cross --target-os ios --ccache
```

> 说明：编译结果输出在**out**目录下，编译输出件用于ArkUI跨平台iOS侧应用开发，[输出件列表]()和[使用指南](../contribute/tutorial/how-to-build-iOS-app.md)。

### 应用构建工具链

ACE Tools，是一套为ArkUI-X开发者提供的命令行工具，包括开发环境检查，新建项目，编译打包，安装调试。[详情参见](https://gitee.com/arkui-x/cli/blob/master/README.md)。


## 配套关系

  **表1** 版本软件和平台配套关系

| 目标平台    | 项目编译使用OS SDK版本              | 备注 |
| ----------- | ----------------------------------- | ---- |
| OpenHarmony | OpenHarmony 3.2 Beta (API level 9) | NA   |
| Android     | Quince Tart 10 (API level 29)       | NA   |
| iOS         | iOS 10                              | NA   |

>说明：Beta版本为面向特定开发者发布的早期预览版本，不承诺UI和API稳定性。

## 源码获取

### 前提条件

1. 注册码云gitee账号。

2. 注册码云SSH公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)。

3. 安装[git客户端](https://gitee.com/link?target=https%3A%2F%2Fgit-scm.com%2Fbook%2Fzh%2Fv2%2F%25E8%25B5%25B7%25E6%25AD%25A5-%25E5%25AE%2589%25E8%25A3%2585-Git)和[git-lfs](https://gitee.com/vcs-all-in-one/git-lfs?_from=gitee_search#downloading)并配置用户信息。
  
   ```
   git config --global user.name "yourname"
   git config --global user.email "your-email-address"
   git config --global credential.helper store
   ```

4. 安装码云repo工具，可以执行如下命令。
  
   ```
   curl -s https://gitee.com/oschina/repo/raw/fork_flow/repo-py3 > /usr/local/bin/repo  #如果没有权限，可下载至其他目录，并将其配置到环境变量中chmod a+x /usr/local/bin/repo
   pip3 install -i https://repo.huaweicloud.com/repository/pypi/simple requests
   ```


### 通过repo获取

**方式一（推荐）**

通过repo + ssh 下载（需注册公钥，请参考[码云帮助中心](https://gitee.com/help/articles/4191)）。


```
repo init -u git@gitee.com:arkui-x/manifest.git -b refs/tags/ArkUI-0.1.0-Beta --no-repo-verify
repo sync -c
```

**方式二**

通过repo + https 下载。


```
repo init -u https://gitee.com/arkui-x/manifest.git -b refs/tags/ArkUI-0.1.0-Beta --no-repo-verify
repo sync -c
```

### 从镜像站点获取

**表2** 获取源码和SDK路径

| 版本源码                                  | **版本信息** | **下载站点** | **SHA256校验码** |
| ----------------------------------------- | ------------ | ------------ | ---------------- |
| ArkUI-X全量代码 | 0.1.0 Beta    | [站点]()     | [SHA256校验码]() |
| ArkUI-X SDK包  | 0.1.0 Beta    | [站点]()     | [SHA256校验码]() |

## Samples

### Samples列表

**表5** [Samples](https://gitee.com/arkui-x/samples)列表

| 项目名称      | 简介                                                         |
| ------------- | ------------------------------------------------------------ |
| HelloWorld | 使用ACE Tools命令行工具生成的HellWorld应用工程示例，支持编译Android、iOS和OpenHarmony应用。 |
| Shopping | 使用ACE Tools命令行工具生成的仿购物应用工程示例，支持编译Android、iOS和OpenHarmony应用。   |
| HealthyDiet | 使用ACE Tools命令行工具生成的健康饮食应用工程示例，支持编译Android、iOS和OpenHarmony应用。|