# Context

Context模块提供了ability或application的上下文的能力，包括访问特定应用程序的资源等。

> **说明：**
>
>  - 本模块首批接口从API version 9开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>  - 本模块接口仅可在Stage模型下使用。

## 导入模块

```ts
import common from '@ohos.app.ability.common';
```

## 属性

**系统能力**：以下各项对应的系统能力均为SystemCapability.Ability.AbilityRuntime.Core

| 名称          | 类型     | 可读   | 可写   | 说明      |
| ----------- | ------ | ---- | ---- | ------- |
| resourceManager     | resmgr.[ResourceManager](js-apis-resource-manager.md) | 是    | 否    | 资源管理对象。   |
| applicationInfo | [ApplicationInfo](js-apis-bundleManager-applicationInfo.md) | 是    | 否    | 当前应用程序的信息。 |
| cacheDir | string | 是    | 否    | 缓存目录。 |
| tempDir | string | 是    | 否    | 临时目录。 |
| filesDir | string | 是    | 否    | 文件目录。 |
| databaseDir | string | 是    | 否    | 数据库目录。 |
| preferencesDir | string | 是    | 否    | preferences目录。 |
| bundleCodeDir | string | 是    | 否    | 安装包目录。不能拼接路径访问资源文件，请使用[资源管理接口](js-apis-resource-manager.md)访问资源。 |

## Context.createModuleContext

createModuleContext(moduleName: string): Context;

根据模块名创建上下文。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**参数：**

| 名称       | 类型                     | 必填   | 说明            |
| -------- | ---------------------- | ---- | ------------- |
| moduleName | string | 是    | 模块名。 |

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| Context | 模块的上下文。 |

**示例：**

```ts
let moduleContext: common.Context;
try {
    moduleContext = this.context.createModuleContext('entry');
} catch (error) {
    console.error('createModuleContext failed, error.code: ${error.code}, error.message: ${error.message}');
}
```

## Context.getApplicationContext

getApplicationContext(): ApplicationContext;

获取本应用的应用上下文。

**系统能力**：SystemCapability.Ability.AbilityRuntime.Core

**返回值：**

| 类型 | 说明 |
| -------- | -------- |
| [ApplicationContext](js-apis-inner-application-applicationContext.md) | 应用上下文Context。 |

**示例：**

```ts
let applicationContext: common.Context;
try {
    applicationContext = this.context.getApplicationContext();
} catch (error) {
    console.error('getApplicationContext failed, error.code: ${error.code}, error.message: ${error.message}');
}
```

