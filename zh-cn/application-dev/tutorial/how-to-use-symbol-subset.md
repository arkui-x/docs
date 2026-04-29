# hwSymbol文件按需加载开发指导

## 简介

本文档用于指导在 ArkUI-X 工程中启用 hwSymbol文件按需加载能力。构建阶段会根据项目实际使用的 `symbol` 自动裁剪，降低包体中字体资源大小。

## 能力说明

hwSymbol文件按需加载流程会读取项目 `symbol` 配置，并结合 SDK 配置生成最小可用子集，输出以下文件：

- `HMSymbolVF.ttf`
- `hm_symbol_config_next.json`

## 前置条件

- macOS需要11.6.2及以上版本，Windows需要Windows 10版本，Ubuntu需要18.04及以上版本。
- 需配置ACE Tools环境，配置可参考：[ACE Tools环境配置指导](./how-to-configure-dev-environment.md)
- 如果已配置 ACE Tools 环境，请更新 ACE Tools 代码，并执行 npm install 下载新增的依赖包。

**使用前请确认：**

1. 当前工程为 ArkUI-X 工程。
2. 工程中已经正确安装并配置 ArkUI-X SDK。
3. 工程中已配置 `resources-config.json`，并声明实际使用的 `symbol`。

## 配置方式

#### 配置文件路径

`.arkui-x/resources-config.json`

若以上路径不存在，hwSymbol 文件的按需加载会被跳过，此时可手动添加该文件。

#### resources-config.json 文件示例

`resources-config.json` 需要包含 `symbol` 字段，声明项目实际使用的 symbol 资源：

```json
{
  "symbol": [
    "sys.symbol.ohos_wifi",
    "sys.symbol.heart",
    "sys.symbol.location"
  ]
}
```

> "symbol"字段仅支持配置系统预置 symbol 资源名，若引用非系统 symbol 资源，显示结果可能异常。<br/>

## 输出结果位置

不同构建目标下，输出目录如下。

### Android

- 应用工程：`.arkui-x/android/app/src/main/assets/arkui-x/systemres/fonts`
- Library 工程：`.arkui-x/android/library/src/main/assets/arkui-x/systemres/fonts`

### iOS

- `.arkui-x/ios/arkui-x/systemres/fonts`

### bundle

- `.arkui-x/build/ace_assets/systemres/fonts`

各目标目录下均会生成：

- `HMSymbolVF.ttf`
- `hm_symbol_config_next.json`

## 注意事项

目前只有使用 ACE Tools 进行构建，才支持 symbol 资源的按需加载。<br/>

使用配置文件中未声明的系统 symbol，图标位置显示为缺字图标，而不是空白。<br/>

> 效果图如下: 
>
> <img src="figures/unspecified-symbol-effect-diagram.png" 
>      alt="unspecified-symbol-effect-diagram" 
>      style="zoom:50%; float: left; margin-right: 15px; margin-bottom: 10px;">

当 symbol 资源按需加载失败时，将未裁剪原文件放入对应目录，不会影响 symbol 的显示。<br/>

