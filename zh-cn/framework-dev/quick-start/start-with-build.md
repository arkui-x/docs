# ArkUI-X编译

- 使用build.sh编译脚本进行编译，编译脚本常用选项

  ```shell
  --product-name    # 必须  编译的产品名称，如：arkui-x
  --target-os       # 必须  编译的跨平台目标，如：android或ios
  --target-cpu      # 可选  指定target侧CPU架构，如：arm或arm64
  --build-target    # 可选  指定编译目标，可以指定多个
  --gn-args         # 可选  gn参数，支持指定多个
  --ninja-args      # 可选  ninja参数，如：--ninja-args=-dkeeprsp
  --log-level       # 可选  指定log等级，如：info或debug
  --help, -h        # 可选  命令行help辅助命令
  ```

- 新下载代码或更新代码时，需要下载或更新预编译工具链，命令如下
  ```
  ./build/prebuilts_download.sh --build-arkuix
  ```
- 编译命令示例
  - 查看编译脚本支持的选项
  ```
  ./build.sh -h
  ```

  - ArkUI-X Android平台编译命令：

  ```shell
  ./build.sh --product-name arkui-x --target-os android
  ```

  - ArkUI-X iOS平台编译命令：

  ```shell
  ./build.sh --product-name arkui-x --target-os ios
  ```