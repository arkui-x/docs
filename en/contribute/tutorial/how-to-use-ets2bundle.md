# Using eTS Loader

eTS Loader is a webpack loader used to compile and transform the code developed using the ArkTS-based declarative development paradigm. It can bundle eTS and TS resources as JS modules. It can also implement UI layouts, styles, events, and page logic processing, verify syntax, and report syntax errors.

#### Directory Structure

* compiler: source code of the compilation and transformation tool
* test: unit test cases
* .eslintrc.js: ESLint configuration
* babel.config.js: Babel configuration
* main.js: main compilation entry
* package.json: eTS Loader version and dependencies
* tsconfig.json: compilation project configuration
* webpack.config.js: packaging tool script configuration

#### Setting Up the Environment

Download [Node.js](https://nodejs.org/en/download/) for your platform and install it or configure environment variables. Run the following commands to check the node environment:

```
> npm -v
  6.14.6
> node -v
  v12.18.4
```

*Note: The displayed version is subject to the version you installed.*

#### Installing eTS Loader

Accessing the **compiler** Directory

###### (Optional) Configuring npm

```
> npm config set registry http://registry.npm.taobao.org
> npm config set strict-ssl false
> npm cache clean -f
```

###### Installing Dependencies

```
> npm install
```

#### Testing eTS Loader

```
> npm run build
> npm run compile
```

The preceding commands build the **sample** project in the **compiler** directory. The build result is stored in the **sample\build** directory.

#### Creating a Project

Run the following command to create a project:

```
npm run create [projectName]
```

*Note: If projectName is not specified, the default project name HelloEts is used.*

###### Creating a foo Project

```
npm run create foo
```

The directory structure of the **foo** project is as follows:

```
foo
├── pages
│   └── index.ets
├── app.ets
└── manifest.json
```

###### Adding a foo Project Page

Create a **bar.ets** page file in the **pages** directory, and add the **bar.ets** page path to the **pages** tag in the **manifest.json** file. Example:

```
"pages": [
  "pages/index",
  "pages/bar"
]
```

The content of the **bar.ets** page is as follows:

```
@Entry
@Component
struct BarPage {
  @State textColor:string = "red"
  build() {
    Column() {
      Text("Hello ArkUI-X")
        .fontColor(this.textColor)
        .fontSize(50)
        .onClick(() => {
          this.textColor = "blue"
        })
    }
  }
}
```

###### Compiling the foo Project

```
npm run compile foo
```

After the compilation is complete, the JS bundle files corresponding to the page eTS file and **app.ets** file in the **foo** project are generated.

# Configuring eTS Loader

You can customize the project directory, project build output directory, **manifest.json** file directory, build mode, compiler type, and ability type for eTS Loader. The configuration options have been used in **DevEco Studio** to support building of OpenHarmony application projects.

#### Customizing the Project Directory and Project Build Output Directory

eTS Loader allows you to set the project directory and project build output directory by using environment variables.

- **aceModuleRoot**: specifies the root directory of the project.
- **aceModuleBuild**: specifies the project out output directory.

Example:

[Windows]

```
> set aceModuleRoot=path\to\your\arkui\project
> set aceModuleBuild=path\to\your\arkui\build
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux or macOS]

```
> export aceModuleRoot=path/to/your/arkui/project
> export aceModuleBuild=path/to/your/arkui/build
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```

#### Customizing the manifest.json File Directory

eTS Loader allows you to set the **manifest.json** file directory using environment variables.

Example:

[Windows]

```
> set aceManifestPath=path\to\your\arkui\project\manifest.json
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux or macOS]

```
> export aceManifestPath=path/to/your/arkui/project/manifest.json
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```

#### Selecting a Build Mode

eTS loader allows you to select a build mode, **release** or **debug**. The build mode is specified by the **buildMode** parameter, and the default value is **debug**.

Example:

[Windows]

```
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js -env buildMode=release
```

[Linux or macOS]

```
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js -env buildMode=release
```

#### Configuring the Compiler Type

eTS Loader allows you to enable the ArkCompiler using a command line with the **compilerType** parameter set to **ark**.

Example:

[Windows]

```
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js -env compilerType=ark
```

[Linux or macOS]

```
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js -env compilerType=ark
```

#### Configuring the Ability Type

eTS Loader allows you to set **abilityType**, which specifies the type of abilities that can be parsed, by using environment variables.

- **page**: Page abilities can be parsed.

- **form**: Widgets can be parsed.

Example:

[Windows]

```
> set abilityType=page
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux or macOS]

```
> export abilityType=page
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```

#### Configuring Incremental Build

 eTS Loader allows you to configure incremental build using environment variables. In watch mode, the webpack service polls the status of all source code files involved in the build and if the file status changes, triggers increment build.

Example:

[Windows]

```
> set watchMode=true
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux or macOS]

```
> export watchMode=true
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```

#### Configuring the Cache Mechanism

eTS Loader allows you to set the cache mechanism by using environment variables. During secondary build, only the files whose file status changes are built. Compared with incremental build, the webpack service does not need to reside in the background, reducing memory and CPU usage.

Example:

[Windows]

```
> set cachePath=path\to\your\cachePath
> node .\node_modules\webpack\bin\webpack.js --config webpack.config.js
```

[Linux or macOS]

```
> export cachePath=path\to\your\cachePath
> node ./node_modules/webpack/bin/webpack.js --config webpack.config.js
```