# 如何把鸿蒙工程结构改造为跨平台工程结构

当前ArkUI-X基于Ace Tools，为开发者提供了一键式自动迁移的命令，具体使用方式请参考：

**<u>第一步：获取最新的Ace Tools工具</u>**

**方案1：集成最新的ArkUI-X SDK版本（**推荐使用**）**

因为该功能当前属于内部公测阶段，请下载使用内部最新的ArkUI-X SDK中的Ace Tools，下载链接：[Windows](https://cidownload.openharmony.cn/version/Master_Version/linux_x86_arkui_sdk/20250513_093045/version-Master_Version-linux_x86_arkui_sdk-20250513_093045-linux_x86_arkui_sdk.tar.gz)，[Mac-Arm](https://cidownload.openharmony.cn/version/Master_Version/ArkUI-X/20250513103157/arkui_x_darwin_sdk.tar.gz)，[Mac-X86](https://cidownload.openharmony.cn/version/Master_Version/ArkUI-X/20250513102413/arkui_x_darwin_sdk.tar.gz)；

手动替换SDK请参考：[如何手动替换ArkUI-X SDK](../Development-Stage/Dev-faq-1.md)

**方案2：手动拉取Ace Tools源码仓代码（如果上述SDK包有问题，逃生方案，如果还报错，请直接新建issue反馈问题）**

```shell
// 1、拉cli仓代码
git clone https://gitcode.com/arkui-x/cli.git

// 2、配置Ace环境变量

// macos
export PATH=/Users/arkuix/cli/bin:$PATH

// windows
// 可在桌面工具栏搜索框键入"环境变量"，然后选择编辑系统环境变量，进行环境变量配置
```

**<u>第二步：Ace Tools配置生效</u>**

需要先配置Ace Tools环境，详情参考：[Ace Tools环境配置](../Pre-research-Stage/Pre-faq-7.md)

**<u>第三步：执行命令进行改造</u>**

配置完成后，使用ace modify命令可以完成工程级和指定模块的工程目录结构改造，参考：[命令说明](https://gitcode.com/arkui-x/cli#ace-modify)。