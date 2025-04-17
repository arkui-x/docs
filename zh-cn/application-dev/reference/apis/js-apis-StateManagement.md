# @ohos.arkui.StateManagement (状态管理)

状态管理模块提供了应用程序的数据存储能力、UIAbility数据存储能力和应用程序需要的环境状态、工具。

>**说明：**
>
>本模块首批接口从API version 12开始支持，后续版本的新增接口，采用上角标单独标记接口的起始版本。


本文中T和S的含义如下：


| 类型   | 说明                                     |
| ---- | -------------------------------------- |
| T    | Class，number，boolean，string和这些类型的数组形式。 |
| S    | number，boolean，string。                 |


## 导入模块

```ts
import { AppStorageV2,UIUtils} from '@kit.ArkUI';
```

## AppStorageV2

AppStorageV2具体UI使用说明，详见[AppStorageV2(应用全局的UI状态存储)](../../quick-start/arkts-new-appstoragev2.md)。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

### connect

static&nbsp;connect\<T extends object\>( </br >
  &nbsp;&nbsp;&nbsp;&nbsp;type:&nbsp;TypeConstructorWithArgs\<T\>, </br >
  &nbsp;&nbsp;&nbsp;&nbsp;keyOrDefaultCreator?:&nbsp;string&nbsp;|&nbsp;StorageDefaultCreator\<T\>, </br >
  &nbsp;&nbsp;&nbsp;&nbsp;defaultCreator?:&nbsp;StorageDefaultCreator\<T\> </br >
):&nbsp;T&nbsp;|&nbsp;undefined

将键值对数据储存在应用内存中。如果给定的key已经存在于[AppStorageV2](../../quick-start/arkts-new-appstoragev2.md)中，返回对应的值；否则，通过获取默认值的构造器构造默认值，并返回。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型   | 必填 | 说明               |
| -------- | ------ | ---- | ---------------------- |
| type | [TypeConstructorWithArgs\<T\>](#typeconstructorwithargst) | 是   | 指定的类型，若未指定key，则使用type的name作为key。 |
| keyOrDefaultCreator | string&nbsp;\|&nbsp;[StorageDefaultCreator\<T\>](#storagedefaultcreatort) | 否   | 指定的key，或者是获取默认值的构造器。 |
| defaultCreator | StorageDefaultCreator\<T\> | 否   | 获取默认值的构造器。 |

>**说明：**
>
>1、若未指定key，使用第二个参数作为默认构造器；否则使用第三个参数作为默认构造器；
>
>2、确保数据已经存储在AppStorageV2中，可省略默认构造器，获取存储的数据；否则必须指定默认构造器，不指定将导致应用异常；
>
>3、同一个key，connect不同类型的数据会导致应用异常，应用需要确保类型匹配；
>
>4、key建议使用有意义的值，长度不超过255，使用非法字符或空字符的行为是未定义的。

**返回值：**

| 类型                                   | 说明                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| T | 创建或获取AppStorageV2数据成功时，返回数据；否则返回undefined。 |

**示例：**

```ts
import { AppStorageV2 } from '@kit.ArkUI';

@ObservedV2
class SampleClass {
  @Trace p: number = 0;
}

// 将key为SampleClass、value为new SampleClass()对象的键值对存储到内存中，并赋值给as1
const as1: SampleClass | undefined = AppStorageV2.connect(SampleClass, () => new SampleClass());

// 将key为key_as2、value为new SampleClass()对象的键值对存储到内存中，并赋值给as2
const as2: SampleClass = AppStorageV2.connect(SampleClass, 'key_as2', () => new SampleClass())!;

// key为SampleClass已经在AppStorageV2中，将key为SampleClass的值返回给as3
const as3: SampleClass = AppStorageV2.connect(SampleClass) as SampleClass;
```

### remove

static&nbsp;remove\<T\>(keyOrType:&nbsp;string&nbsp;|&nbsp;TypeConstructorWithArgs\<T\>):&nbsp;void

将指定的键值对数据从[AppStorageV2](../../quick-start/arkts-new-appstoragev2.md)里面删除。如果指定的键值不存在于AppStorageV2中，将删除失败。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名   | 类型   | 必填 | 说明               |
| -------- | ------ | ---- | ---------------------- |
| keyOrType | string \| TypeConstructorWithArgs\<T\> | 是   | 需要删除的key；如果指定的是type类型，删除的key为type的name。 |

>**说明：**
>
>删除AppStorageV2中不存在的key会报警告。


**示例：**

<!--code_no_check-->
```ts
// 假设AppStorageV2中存在key为key_as2的键，从AppStorageV2中删除该键值对数据
AppStorageV2.remove('key_as2');

// 假设AppStorageV2中存在key为SampleClass的键，从AppStorageV2中删除该键值对数据
AppStorageV2.remove(SampleClass);

// 假设AppStorageV2中不存在key为key_as1的键，报警告
AppStorageV2.remove('key_as1');
```

### keys

static&nbsp;keys():&nbsp;Array\<string\>

获取[AppStorageV2](../../quick-start/arkts-new-appstoragev2.md)中的所有key。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**返回值：**

| 类型                                   | 说明                                                         |
| -------------------------------------- | ------------------------------------------------------------ |
| Array\<string\> | 所有AppStorageV2中的key。 |

>**说明：**
>
>key在Array中的顺序是无序的，与key插入到AppStorageV2中的顺序无关。

**示例：**

```ts
// 假设AppStorageV2中存在两个key（key_as1、key_as2），返回[key_as1、key_as2]赋值给keys
const keys: Array<string> = AppStorageV2.keys();
```

## UIUtils

UIUtils提供一些方法，用于处理状态管理相关的数据转换。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

### getTarget

static getTarget\<T extends object\>(source: T): T

从状态管理框架包裹的代理对象中获取原始对象。详见[getTarget接口：获取状态管理框架代理前的原始对象](../../quick-start/arkts-new-getTarget.md)。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明     |
| ------ | ---- | ---- | ------------ |
| source | T    | 是   | 数据源对象。 |

**返回值：**

| 类型 | 说明                                             |
| ---- | ------------------------------------------------ |
| T    | 数据源对象去除状态管理框架所加代理后的原始对象。 |

**示例：**

```ts
import { UIUtils } from '@kit.ArkUI';
class NonObservedClass {
  name: string = "Tom";
}
let nonObservedClass: NonObservedClass = new NonObservedClass();
@Entry
@Component
struct Index {
  @State someClass: NonObservedClass = nonObservedClass;
  build() {
    Column() {
      Text(`this.someClass === nonObservedClass: ${this.someClass === nonObservedClass}`) // false
      Text(`UIUtils.getTarget(this.someClass) === nonObservedClass: ${UIUtils.getTarget(this.someClass) ===
        nonObservedClass}`) // true
    }
  }
}
```
### makeObserved

static makeObserved\<T extends object\>(source: T): T

将普通不可观察数据变为可观察数据。详见[makeObserved接口：将非观察数据变为可观察数据](../../quick-start/arkts-new-makeObserved.md)。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明     |
| ------ | ---- | ---- | ------------ |
| source | T    | 是   | 数据源对象。支持非@Observed和@ObserveV2修饰的class，JSON.parse返回的Object和@Sendable修饰的class。</br>支持Array、Map、Set和Date。</br>支持collection.Array, collection.Set和collection.Map。</br>具体使用规则，详见[makeObserved接口：将非观察数据变为可观察数据](../../quick-start/arkts-new-makeObserved.md)。 |

**返回值：**

| 类型 | 说明           |
| ---- | -------------- |
| T    | 可观察的数据。 |

**示例：**

```ts
import { UIUtils } from '@kit.ArkUI';
class NonObservedClass {
  name: string = 'Tom';
}

@Entry
@ComponentV2
struct Index {
  observedClass: NonObservedClass = UIUtils.makeObserved(new NonObservedClass());
  nonObservedClass: NonObservedClass = new NonObservedClass();
  build() {
    Column() {
      Text(`observedClass: ${this.observedClass.name}`)
        .onClick(() => {
          this.observedClass.name = 'Jane'; // 刷新
        })
      Text(`observedClass: ${this.nonObservedClass.name}`)
        .onClick(() => {
          this.nonObservedClass.name = 'Jane'; // 不刷新
        })
    }
  }
}
```

### enableV2Compatibility<sup>18+</sup>

static enableV2Compatibility\<T extends object\>(source: T): T

使V1的状态变量能够在\@ComponentV2中观察，主要应用于状态管理V1、V2混用场景。详见[状态管理V1V2混用文档](../../quick-start/arkts-v1-v2-mixusage.md)。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明     |
| ------ | ---- | ---- | ------------ |
| source | T    | 是   | 数据源，仅支持V1状态数据。 |

**返回值：**

| 类型 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| T    | 如果数据源是V1的状态数据，则返回能够在\@ComponentV2中观察的数据。否则返回数据源本身。 |


**示例：**

```ts
import { UIUtils } from '@kit.ArkUI';

@Observed
class ObservedClass {
  name: string = 'Tom';
}

@Entry
@Component
struct CompV1 {
  @State observedClass: ObservedClass = new ObservedClass();

  build() {
    Column() {
      Text(`@State observedClass: ${this.observedClass.name}`)
        .onClick(() => {
          this.observedClass.name = 'State'; // 刷新
        })
      // 将V1的状态变量使能V2的观察能力
      CompV2({ observedClass: UIUtils.enableV2Compatibility(this.observedClass) })
    }
  }
}

@ComponentV2
struct CompV2 {
  @Param observedClass: ObservedClass = new ObservedClass();

  build() {
    // V1状态变量在使能V2观察能力后，可以在V2观察第一层的变化
    Text(`@Param observedClass: ${this.observedClass.name}`)
      .onClick(() => {
        this.observedClass.name = 'Param'; // 刷新
      })
  }
}
```

### makeV1Observed<sup>18+</sup>
static makeV1Observed\<T extends object\>(source: T): T

将不可观察的对象包装成状态管理V1可观察的对象，其能力等同于@Observed，可初始化@ObjectLink。

该接口可搭配[enableV2Compatibility](#enablev2compatibility18)应用于状态管理V1和V2混用场景，详见[状态管理V1V2混用文档](../../quick-start/arkts-v1-v2-mixusage.md)。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明     |
| ------ | ---- | ---- | ------------ |
| source | T    | 是   | 数据源。支持普通class、Array、Map、Set、Date类型。</br>不支持collections类型和Sendable修饰的class。</br>不支持undefined和null。不支持状态管理V2的数据和[makeObserved](#makeobserved)的返回值。 |

**返回值：**

| 类型 | 说明                                                         |
| ---- | ------------------------------------------------------------ |
| T    | 对于支持的入参类型，返回状态管理V1的观察数据。对于不支持的入参类型，返回数据源对象本身。 |

**示例：**

```ts
import { UIUtils } from '@kit.ArkUI';

class Outer {
  outerValue: string = 'outer';
  inner: Inner;

  constructor(inner: Inner) {
    this.inner = inner;
  }
}

class Inner {
  interValue: string = 'inner';
}

@Entry
@Component
struct Index {
  @State outer: Outer = new Outer(UIUtils.makeV1Observed(new Inner()));

  build() {
    Column() {
      // makeV1Observed的返回值可初始化@ObjectLink
      Child({ inner: this.outer.inner })
    }
    .height('100%')
    .width('100%')
  }
}

@Component
struct Child {
  @ObjectLink inner: Inner;

  build() {
    Text(`${this.inner.interValue}`)
      .onClick(() => {
        this.inner.interValue += '!';
      })
  }
}
```

## StorageDefaultCreator\<T\>

type StorageDefaultCreator\<T\> = () => T

返回默认构造器的函数。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**返回值：**

| 类型 | 说明                                             |
| ---- | ------------------------------------------------ |
| () => T    | 返回默认构造器的函数。 |

## TypeConstructorWithArgs\<T\>

含有任意入参的类构造器。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

### new

new(...args: any): T

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明     |
| ------ | ---- | ---- | ------------ |
| ...args | any    | 是   | 函数入参。   |

**返回值：**

| 类型 | 说明          |
| ---- | ------------- |
| T    | T类型的实例。 |

## TypeConstructor\<T\>

类构造函数。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

### new

new(): T

**返回值：**

| 类型 | 说明          |
| ---- | ------------- |
| T    | T类型的实例。 |

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

## TypeDecorator

type TypeDecorator = \<T\>(type: TypeConstructor\<T\>) => PropertyDecorator

属性装饰器。

**系统能力：** SystemCapability.ArkUI.ArkUI.Full

**支持平台：** Android、iOS

**参数：**

| 参数名 | 类型 | 必填 | 说明     |
| ------ | ---- | ---- | ------------ |
| type | [TypeConstructor\<T\>](#typeconstructort)    | 是   | 标记类属性的类型。   |

**返回值：**

| 类型 | 说明                                             |
| ---- | ------------------------------------------------ |
| PropertyDecorator    | 属性装饰器。 |

