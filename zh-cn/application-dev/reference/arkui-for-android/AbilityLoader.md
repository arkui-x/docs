# AbilityLoader

本模块接口用于在原生侧加载UIAbility，仅适用于如下场景：[以Hap为主体的共享逻辑包开发指南](../../tutorial/how-to-decoupled-UI-and-Logic-on-android.md)。

## loadAbility

```java
/**
 * Load ability.
 *
 * @param bundleName  The bundle name.
 * @param moduleName  The module name.
 * @param abilityName The ability name.
 * @param params      the want params.
 */
static public void loadAbility(String bundleName, String moduleName, String abilityName, String params);
```

**描述：**

加载指定的UIAbility。UIAbility加载完成后仅会触发其回调函数[onCreate](../apis/js-apis-app-ability-uiAbility.md#uiabilityoncreate)，不会触发其它生命周期回调函数。

**参数：** 

| 变量名      | 类型   | 描述                    |
| ----------- | :----- | ----------------------- |
| bundleName  | String | UIAbility所属的应用包名 |
| moduleName  | String | UIAbility所属的模块名   |
| abilityName | String | UIAbility的名称         |
| params      | String | 传递参数                |

**返回值：** 

无

**示例：** 

```java
import ohos.stage.ability.adapter.AbilityLoader;

AbilityLoader.loadAbility("com.example.test", "entry", "EntryAbility", "");
```

## unloadAbility

```java
/**
 * Unload ability.
 *
 * @param bundleName  The bundle name.
 * @param moduleName  The module name.
 * @param abilityName The ability name.
 */
static public void unloadAbility(String bundleName, String moduleName, String abilityName);
```

**描述：**

取消已经加载的指定的UIAbility。UIAbility销毁完成后仅会触发其回调函数[onWindowStageDestroy](../apis/js-apis-app-ability-uiAbility.md#uiabilityonwindowstagedestroy)、[onDestroy](../apis/js-apis-app-ability-uiAbility.md#uiabilityondestroy)，不会触发其它生命周期回调函数。

**参数：** 

| 变量名      | 类型   | 描述                    |
| ----------- | :----- | ----------------------- |
| bundleName  | String | UIAbility所属的应用包名 |
| moduleName  | String | UIAbility所属的模块名   |
| abilityName | String | UIAbility的名称         |

**返回值：** 

无

**示例：** 

```java
import ohos.stage.ability.adapter.AbilityLoader;

AbilityLoader.unloadAbility("com.example.test", "entry", "EntryAbility");
```