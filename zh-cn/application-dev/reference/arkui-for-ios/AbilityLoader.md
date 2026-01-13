# AbilityLoader

本模块接口用于在原生侧加载UIAbility，仅适用于如下场景：[以Hap为主体的共享逻辑包开发指南](../../tutorial/how-to-decoupled-UI-and-Logic-on-ios.md)。

## loadAbility

```objective-c
+ (void)loadAbility:(NSString *)bundleName moduleName:(NSString *)moduleName abilityName:(NSString *)abilityName params:(NSString *)params;
```

**描述：**

加载指定的UIAbility。UIAbility加载完成后仅会触发其回调函数[onCreate](../apis/js-apis-app-ability-uiAbility.md#uiabilityoncreate)，不会触发其它生命周期回调函数。

**参数：** 

| 变量名      | 类型       | 描述                    |
| ----------- | :--------- | ----------------------- |
| bundleName  | NSString * | UIAbility所属的应用包名 |
| moduleName  | NSString * | UIAbility所属的模块名   |
| abilityName | NSString * | UIAbility的名称         |
| params      | NSString * | 传递参数                |

**返回值：** 

无

**示例：** 

```objective-c
#import <libarkui_ios/AbilityLoader.h>

[AbilityLoader loadAbility:@"com.example.test" moduleName:@"entry" abilityName:@"EntryAbility" params:@""];
```

## unloadAbility

```objective-c
+ (void)unloadAbility:(NSString *)bundleName moduleName:(NSString *)moduleName abilityName:(NSString *)abilityName;
```

**描述：**

取消已经加载的指定的UIAbility。UIAbility销毁完成后仅会触发其回调函数[onWindowStageDestroy](../apis/js-apis-app-ability-uiAbility.md#uiabilityonwindowstagedestroy)、[onDestroy](../apis/js-apis-app-ability-uiAbility.md#uiabilityondestroy)，不会触发其它生命周期回调函数。

**参数：** 

| 变量名      | 类型       | 描述                    |
| ----------- | :--------- | ----------------------- |
| bundleName  | NSString * | UIAbility所属的应用包名 |
| moduleName  | NSString * | UIAbility所属的模块名   |
| abilityName | NSString * | UIAbility的名称         |

**返回值：** 

无

**示例：** 

```objective-c
#import <libarkui_ios/AbilityLoader.h>

[AbilityLoader unloadAbility:@"com.example.test" moduleName:@"entry" abilityName:@"EntryAbility"];
```