# 快速入门

ACE Loader是一个用于ArkUI类Web范式语法编译转换的webpack loader，可以完成hml/css/js JsBundle化、UI布局、样式、事件和页面逻辑处理，并可以进行语法验证，同时拥有完善的语法报错提示能力。

#### 目录介绍

* compiler：编译转换工具源码
* test：单元测试用例
* .eslintrc.js：eslint配置规则
* babel.config.js：babel配置信息
* main.product.js：编译主入口
* package.json：eTs loader版本及依赖相关信息
* webpack.rich.config.js：打包工具脚本配置信息

#### 环境搭建

根据不同平台[下载Node.js](https://nodejs.org/zh-cn/download/)，并进行安装或环境变量配置。可通过运行如下命令判断Node环境：

```
> npm -v
  6.14.6
> node -v
  v12.18.4
```

*注释：显示版本以自己安装的版本为准*

#### 安装ACE Loader

进入compiler目录

###### 配置npm(可选)

```
> npm config set registry http://registry.npm.taobao.org
> npm config set strict-ssl false
> npm cache clean -f
```

###### 安装依赖

```
> npm install
```

#### 测试ACE Loader

```
> npm run build
> npm run rich
```

上述命令会编译compiler目录下的sample/rich工程，编译结果在sample/rich/build目录。

# ACE Loader配置

ACE Loader为支持复杂工程和实际项目需要，可自定义配置工程目录、工程编译输出目录、编译模式、Manifest路径、解析类型和预览模式。配置选项已用于**DevEco Studio**，支持OpenHarmony应用工程编译构建。

#### 自定义工程目录

ACE Loader支持通过环境变量设置工程路径和工程编译输出路径，比如：

- aceModuleRoot：用于指定工程根目录路径。
- aceModuleBuild：用于指定工程编译输出路径。

示例：

[Windows平台]

```
> set aceModuleRoot=path\to\your\arkui\project
> set aceModuleBuild=path\to\your\arkui\build
> node .\node_modules\webpack\bin\webpack.js --config webpack.rich.config.js
```

[Linux|Mac平台]

```
> export aceModuleRoot=path/to/your/arkui/project
> export aceModuleBuild=path/to/your/arkui/build
> node ./node_modules/webpack/bin/webpack.js --config webpack.rich.config.js
```

#### 自定义清单文件路径

ACE Loader支持通过环境变量设置manifest.json路径。

示例：

[Windows]

```
> set aceManifestPath=path\to\your\arkui\project\manifest.json
> node .\node_modules\webpack\bin\webpack.js --config webpack.rich.config.js
```

[Linux|Mac]

```
> export aceManifestPath=path/to/your/arkui/project/manifest.json
> node ./node_modules/webpack/bin/webpack.js --config webpack.rich.config.js
```

#### 选择编译模式

ACE Loader支持命令行参数选择编译模式，参数buildMode可选值：release和debug，默认为debug。

示例：

[Windows]

```
> node .\node_modules\webpack\bin\webpack.js --config webpack.rich.config.js -env buildMode=release
```

[Linux|Mac]

```
> node ./node_modules/webpack/bin/webpack.js --config webpack.rich.config.js -env buildMode=release
```

#### 配置编译类型

ACE Loader支持命令行参数开启Ark方舟编译，参数compilerType值：ark。

示例：

[Windows]

```
> node .\node_modules\webpack\bin\webpack.js --config webpack.rich.config.js -env compilerType=ark
```

[Linux|Mac]

```
> node ./node_modules/webpack/bin/webpack.js --config webpack.rich.config.js -env compilerType=ark
```

#### 配置解析类型

ACE Loader支持通过环境变量设置abilityType类型，用于设置解析类型。

- page：用于指定解析类型为页面。

- form：用于指定解析类型为卡片。

示例：

[Windows平台]

```
> set abilityType=page
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux|Mac平台]

```
> export abilityType=page
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```

#### 配置增量编译

 ACE Loader支持通过环境变量设置增量编译，即webpack 监听watch模式，watch模式会轮询所有参与编译的源码文件状态，文件状态发生改变后会自动触发增量编译。

示例：

[Windows平台]

```
> set watchMode=true
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux|Mac平台]

```
> export watchMode=true
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```

#### 配置缓存机制

 ACE Loader支持通过环境变量设置缓存机制，二次编译时，仅编译文件状态发生改变的文件。相比增量编译的watch模式，webpack服务不需要常驻后台，节省内存空间和CPU占用。

示例：

[Windows平台]

```
> set cachePath=path\to\your\cachePath
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux|Mac平台]

```
> export cachePath=path/to/your/cachePath
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```

