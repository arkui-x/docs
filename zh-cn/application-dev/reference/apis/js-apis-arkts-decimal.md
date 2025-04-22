# @arkts.math.Decimal (高精度数学库Decimal)

Decimal用于提供高精度数学库，主要用于提供高精度浮点运算能力。

> **说明：**
>
> 本模块首批接口从API version 18开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。


## 导入模块

```ts
import { Decimal } from '@kit.ArkTS';
```

## Value

type Value = string | number | Decimal

表示用于构建Decimal的参数类型。

取值类型为下列类型中的并集。

**支持平台：** Android 、iOS

| 类型                | 说明                           | **Android平台** | **iOS平台** |
| ------------------- | ------------------------------ | --------------- | ----------- |
| string              | 表示值类型为字符，可取任意值。 | 支持            | 支持        |
| number              | 表示值类型为数字，可取任意值。 | 支持            | 支持        |
| [Decimal](#decimal) | 表示值类型为Decimal类型。      | 支持            | 支持        |

## Rounding

type Rounding = 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8

表示可设置的舍入类型。

取值类型为下列类型中的并集。

**支持平台：** Android 、iOS

| 类型 | 说明                                                         | **Android平台** | **iOS平台** |
| ---- | ------------------------------------------------------------ | --------------- | ----------- |
| 0    | 向远离零的方向舍入。与[Decimal.ROUND_UP](#常量)一致。        | 支持            | 支持        |
| 1    | 向靠近零的方向舍入。与[Decimal.ROUND_DOWN](#常量)一致。      | 支持            | 支持        |
| 2    | 向正无穷方向舍入。与[Decimal.ROUND_CEILING](#常量)一致。     | 支持            | 支持        |
| 3    | 向负无穷方向舍入。与[Decimal.ROUND_FLOOR](#常量)一致。       | 支持            | 支持        |
| 4    | 向最近的邻值舍入。如果距离相等，则远离零方向舍入。与[Decimal.ROUND_HALF_UP](#常量)一致。 | 支持            | 支持        |
| 5    | 向最近的邻值舍入。如果距离相等，则靠近零方向舍入。与[Decimal.ROUND_HALF_DOWN](#常量)一致。 | 支持            | 支持        |
| 6    | 向最近的邻值舍入。如果距离相等，则向偶数邻值舍入。与[Decimal.ROUND_HALF_EVEN](#常量)一致。 | 支持            | 支持        |
| 7    | 向最近的邻值舍入。如果距离相等，则向正无穷方向舍入。与[Decimal.ROUND_HALF_CEILING](#常量)一致。 | 支持            | 支持        |
| 8    | 向最近的邻值舍入。如果距离相等，则向负无穷方向舍入。与[Decimal.ROUND_HALF_FLOOR](#常量)一致。 | 支持            | 支持        |

## Modulo

type Modulo = Rounding | 9

表示可设置的取模方法舍入类型。

取值类型为下列类型中的并集。

**支持平台：** Android 、iOS

| 类型                   | 说明                                                         | **Android平台**                                            | **iOS平台**                                                |
| ---------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Rounding](#rounding) | 模运算下的舍入类型。与[Rounding](#常量)表示的舍入模式相同。  | 支持 | 支持 |
| 9                      | 余模运算下，余数始终为正。欧几里得除法。与[Decimal.EUCLIDEAN](#常量)一致。 | 支持 | 支持 |

## DecimalConfig

用于设置Decimal的配置属性，可使用[Decimal.set](#set)方法进行配置。

**支持平台：** Android 、iOS

### 属性

| 名称      | 类型                   | 只读 | 必填 | 说明                                                         | **Android平台**                                            | **iOS平台**                                                |
| --------- | ---------------------- | ---- | ---- | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| precision | number                 | 否   | 否   | 运算结果的最大有效位数，取值范围为[1, 1e9]，默认值为20。     | 支持   | 支持   |
| rounding  | [Rounding](#rounding) | 否   | 否   | 舍入模式，取值范围为0到8的整数，默认值为4。                  | 支持                | 支持                |
| toExpNeg  | number                 | 否   | 否   | 指数表示法的负指数值的极限值，若Decimal的负指数小于等于该值时，使用科学计数法表示，[toString](#tostring)方法中使用，取值范围为[-9e15, 0]，默认值为-7。 | 支持 | 支持 |
| toExpPos  | number                 | 否   | 否   | 指数表示法的正指数值的极限值，若Decimal的正指数大于等于该值时，使用科学计数法表示，[toString](#tostring)方法中使用，取值范围为[0, 9e15]，默认值为21。 | 支持 | 支持 |
| minE      | number                 | 否   | 否   | 负指数极限，若Decimal的指数值小于该值，会下溢到零，取值范围为[-9e15, 0]，默认值为-9e15。 | 支持 | 支持 |
| maxE      | number                 | 否   | 否   | 正指数极限，若Decimal的指数值大于该值，会溢出至无穷大，取值范围为[0, 9e15]，默认值为9e15。 | 支持 | 支持 |
| crypto    | boolean                | 否   | 否   | 确定是否使用加密安全伪随机数生成的值，默认值为false。该能力不支持使用，报错的错误码为：10200061  | 支持 | 支持 |
| modulo    | [Modulo](#modulo)      | 否   | 否   | 模计算时使用的舍入模式，取值范围为0到9的整数，默认值为1。    | 支持  | 支持  |
| defaults  | boolean                | 否   | 否   | 表示未指定的属性是否被设置为默认值，true表示使用默认值，false表示不使用默认值，默认值为true。 | 支持 | 支持 |

## Decimal

任意精度的Decimal类型。

**支持平台：** Android 、iOS

### 属性

| 名称 | 类型     | 只读 | 必填 | 说明                                      | Android平台 | iOS平台 |
| ---- | -------- | ---- | ---- | ----------------------------------------- | ----------- | ------- |
| d    | number[] | 是   | 是   | digits：表示Decimal数整数部分和小数部分。 | 支持        | 支持    |
| e    | number   | 是   | 是   | exponent：表示Decimal数十进制指数的数目。 | 支持        | 支持    |
| s    | number   | 是   | 是   | sign：表示Decimal数的符号位。             | 支持        | 支持    |

### 常量

| 名称               | 类型   | 值   | 说明                                                         | Android平台 | iOS平台 |
| ------------------ | ------ | ---- | ------------------------------------------------------------ | ----------- | ------- |
| ROUND_UP           | number | 0    | 向远离零的方向舍入。模运算下，如果被除数为负，则余数为正，否则为负。 | 支持        | 支持    |
| ROUND_DOWN         | number | 1    | 向靠近零的方向舍入。模运算下，余数与被除数的符号相同，使用截断除法。 | 支持        | 支持    |
| ROUND_CEILING      | number | 2    | 向正无穷方向舍入。                                           | 支持        | 支持    |
| ROUND_FLOOR        | number | 3    | 向负无穷方向舍入。模运算下，余数与除数的符号相同。           | 支持        | 支持    |
| ROUND_HALF_UP      | number | 4    | 向最近的邻值舍入。如果距离相等，则远离零方向舍入。           | 支持        | 支持    |
| ROUND_HALF_DOWN    | number | 5    | 向最近的邻值舍入。如果距离相等，则靠近零方向舍入。           | 支持        | 支持    |
| ROUND_HALF_EVEN    | number | 6    | 向最近的邻值舍入。如果距离相等，则向偶数邻值舍入。模运算下，IEEE 754 求余函数。 | 支持        | 支持    |
| ROUND_HALF_CEILING | number | 7    | 向最近的邻值舍入。如果距离相等，则向正无穷方向舍入。         | 支持        | 支持    |
| ROUND_HALF_FLOOR   | number | 8    | 向最近的邻值舍入。如果距离相等，则向负无穷方向舍入。         | 支持        | 支持    |
| EUCLIDEAN          | number | 9    | 模运算下，余数始终为正。使用欧几里得除法：q = sign(x) * floor(a / abs(x))。 | 支持        | 支持    |

### constructor

constructor(n: Value)

Decimal的构造函数。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                    | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ----------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 构造Decimal时的初始值。 | 支持        | 支持    |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(5);
console.info("test Decimal constructor:" + data.toString()); // 'test Decimal constructor:5'
```

### abs

abs(): Decimal

返回一个新的Decimal对象，其值为该Decimal的绝对值。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                |
| ------------------- | ----------------------------------- |
| [Decimal](#decimal) | 返回绝对值运算后的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(-0.5).abs();
console.info("test Decimal abs:" + data.toString()); // 'test Decimal abs:0.5'
```

### floor

floor(): Decimal

返回一个新的Decimal对象，其值为该Decimal向负无穷方向舍入得到的结果。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回舍入之后的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(1.8).floor();
console.info("test Decimal floor:" + data.toString()); // 'test Decimal floor:1'
```

### ceil

ceil(): Decimal

返回一个新的Decimal对象，其值为该Decimal向正无穷方向舍入得到的结果。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回舍入之后的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(1.8).ceil();
console.info("test Decimal ceil:" + data.toString()); // 'test Decimal ceil:2'
```

### trunc

trunc(): Decimal

返回一个新的Decimal，其值是将此Decimal截断为整数部分。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回截断之后的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(2.5).trunc();
console.info("test Decimal trunc:" + data.toString()); // 'test Decimal trunc:2'
```

### clamp

clamp(min: Value, max: Value): Decimal

返回一个值为将该Decimal的值限制在min到max范围内的Decimal对象。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                     | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ------------------------ | ----------- | ------- |
| min    | [Value](#value) | 是   | 限制的最小值。包含该值。 | 支持        | 支持    |
| max    | [Value](#value) | 是   | 限制的最大值。包含该值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回符合范围内的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200001 | The value of 'min' is out of range.                          |

**示例：**

```ts
let data: Decimal = new Decimal(10.1).clamp(0, 10);
console.info("test Decimal clamp:" + data.toString()); // 'test Decimal clamp:10'
```

### add

add(n: Value): Decimal;

返回一个新的Decimal，其值为该Decimal的值加上n。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 加法运算的加数。 | 支持        | 支持    |

**返回值**：

| 类型    | 说明                            |
| ------- | ------------------------------- |
| [Decimal](#decimal) | 返回加法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(0.5).add(0.5);
console.info("test Decimal add:" + data.toString()); // 'test Decimal add:1'
```

### sub

sub(n: Value): Decimal

返回一个新的Decimal，其值为此Decimal的值减去n。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 减法运算的减数。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                            |
| ------- | ------------------------------- |
| [Decimal](#decimal) | 返回减法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(1).sub(0.5);
console.info("test Decimal sub:" + data.toString()); // 'test Decimal sub:0.5'
```

### mul

mul(n: Value): Decimal

返回一个新的Decimal，其值为Decimal乘以n。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 乘法运算的乘数。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                            |
| ------- | ------------------------------- |
| [Decimal](#decimal) | 返回乘法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(1).mul(0.5);
console.info("test Decimal mul:" + data.toString()); // 'test Decimal mul:0.5'
```

### div

div(n: Value): Decimal

返回一个新的Decimal，其值是此Decimal的值除以n。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 除法运算的除数。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                            |
| ------- | ------------------------------- |
| [Decimal](#decimal) | 返回除法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(1).div(0.5);
console.info("test Decimal div:" + data.toString()); // 'test Decimal div:2'
```

### mod

mod(n: Value): Decimal

返回一个新的Decimal，其值是该Decimal除以n的模。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 取模运算的除数。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                            |
| ------- | ------------------------------- |
| [Decimal](#decimal) | 返回取模运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(2).mod(1);
console.info("test Decimal mod:" + data.toString()); // 'test Decimal mod:0'
```

### sqrt

sqrt(): Decimal

返回一个新的Decimal，其值为该Decimal的平方根。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                              |
| ------- | --------------------------------- |
| [Decimal](#decimal) | 返回平方根运算后的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(3).sqrt();
console.info("test Decimal sqrt:" + data.toString()); // 'test Decimal sqrt:1.7320508075688772935'
```

### cbrt

cbrt(): Decimal

返回一个新的Decimal，其值是此Decimal值的立方根。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                              |
| ------- | --------------------------------- |
| [Decimal](#decimal) | 返回立方根运算后的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(3).cbrt();
console.info("test Decimal cbrt:" + data.toString()); // 'test Decimal cbrt:1.4422495703074083823'
```

### pow

pow(n: Value): Decimal

返回一个新的Decimal，它的值是这个Decimal的值的n次幂。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 幂运算的幂的值。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                          |
| ------- | ----------------------------- |
| [Decimal](#decimal) | 返回幂运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = new Decimal(3).pow(-2);
console.info("test Decimal pow:" + data.toString()); // 'test Decimal pow:0.11111111111111111111'
```

### exp

exp(): Decimal

返回一个新的Decimal，其值是该Decimal值的自然指数。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 返回值                                |
| ------------------- | ------------------------------------- |
| [Decimal](#decimal) | 返回自然指数运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(2).exp();
console.info("test Decimal exp:" + data.toString()); // 'test Decimal exp:7.3890560989306502272'
```

### log

log(n: Value): Decimal

返回一个值，以n为底的指定的对数运算的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明               | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ------------------ | ----------- | ------- |
| n      | [Value](#value) | 是   | 对数计算的底数值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回对数运算后的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = new Decimal(2).log(256);
console.info("test Decimal log:" + data.toString()); // 'test Decimal log:0.125'
```

### ln

ln(): Decimal

返回一个新的Decimal，其值是此Decimal值的自然对数。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                  |
| ------------------- | ------------------------------------- |
| [Decimal](#decimal) | 返回自然对数运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(1.23e+30).ln();
console.info("test Decimal ln:" + data.toString()); // 'test Decimal ln:69.284566959205696648'
```

### cos

cos(): Decimal

返回一个新的Decimal，其值是此Decimal的余弦值。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回计算余弦值的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(-0.25).cos();
console.info("test Decimal cos:" + data.toString()); // 'test Decimal cos:0.96891242171064478414'
```

### sin

sin(): Decimal

返回一个新的Decimal，其值是此Decimal的正弦值。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回计算正弦值的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(0.75).sin();
console.info("test Decimal sin:" + data.toString()); // 'test Decimal sin:0.68163876002333416673'
```

### tan

tan(): Decimal

返回一个新的Decimal，其值是此Decimal的正切值。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回计算正切值的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(0.75).tan();
console.info("test Decimal tan:" + data.toString()); // 'test Decimal tan:0.93159645994407246117'
```

### cosh

cosh(): Decimal

返回一个新的Decimal，其值是此Decimal的双曲余弦值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                |
| ------- | ----------------------------------- |
| [Decimal](#decimal) | 返回计算双曲余弦值的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(0.5).cosh();
console.info("test Decimal cosh:" + data.toString()); // 'test Decimal cosh:1.1276259652063807852'
```

### sinh

sinh(): Decimal

返回一个新的Decimal，其值是此Decimal的双曲正弦值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                |
| ------- | ----------------------------------- |
| [Decimal](#decimal) | 返回计算双曲正弦值的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(0.5).sinh();
console.info("test Decimal sinh:" + data.toString()); // 'test Decimal sinh:0.52109530549374736162'
```

### tanh

tanh(): Decimal

返回一个新的Decimal，其值是此Decimal的双曲正切值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                |
| ------- | ----------------------------------- |
| [Decimal](#decimal) | 返回计算双曲正切值的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(0.5).tanh();
console.info("test Decimal tanh:" + data.toString()); // 'test Decimal tanh:0.4621171572600097585'
```

### acos

acos(): Decimal

返回一个新的Decimal，其值是此Decimal的反余弦值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                              |
| ------- | --------------------------------- |
| [Decimal](#decimal) | 返回计算反余弦值的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(0.5).acos();
console.info("test Decimal acos:" + data.toString()); // 'test Decimal acos:1.0471975511965977462'
```

### asin

asin(): Decimal

返回一个新的Decimal，其值为此Decimal的反正弦值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                              |
| ------- | --------------------------------- |
| [Decimal](#decimal) | 返回计算反正弦值的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(0.75).asin();
console.info("test Decimal asin:" + data.toString()); // 'test Decimal asin:0.84806207898148100805'
```

### atan

atan(): Decimal

返回一个新的Decimal，其值是此Decimal的反正切值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                              |
| ------- | --------------------------------- |
| [Decimal](#decimal) | 返回计算反正切值的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(0.75).atan();
console.info("test Decimal atan:" + data.toString()); // 'test Decimal atan:0.6435011087932843868'
```

### acosh

acosh(): Decimal

返回一个新的Decimal，其值是此Decimal值的双曲余弦的倒数。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回计算双曲余弦的倒数值的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(50).acosh();
console.info("test Decimal acosh:" + data.toString()); // 'test Decimal acosh:4.6050701709847571595'
```

### asinh

asinh(): Decimal

返回一个新的Decimal，其值是此Decimal值的双曲正弦的倒数。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回计算双曲正弦的倒数值的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(50).asinh();
console.info("test Decimal asinh:" + data.toString()); // 'test Decimal asinh:4.6052701709914238266'
```

### atanh

atanh(): Decimal

返回一个新的Decimal，其值是此Decimal值的双曲正切的倒数。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回计算双曲正切的倒数值的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                  |
| -------- | ------------------------- |
| 10200060 | Precision limit exceeded. |

**示例：**

```ts
let data: Decimal = new Decimal(0.75).atanh();
console.info("test Decimal atanh:" + data.toString()); // 'test Decimal atanh:0.97295507452765665255'
```

### comparedTo

comparedTo(n: Value): number

Decimal的比较方法。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                  | Android平台 | iOS平台 |
| ------ | --------------- | ---- | --------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 待比较的值或Decimal。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                                                         |
| ------ | ------------------------------------------------------------ |
| number | 返回该Decimal与n的比较结果：<br>1:该Decimal大于比较值。<br/>-1:该Decimal小于比较值。<br/>0:该Decimal等于比较值。<br/>NaN:该Decimal与比较值有一个值为NaN。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(Infinity);
let data1: Decimal = new Decimal(5);
let data2: number = data.comparedTo(data1);
console.info("test Decimal comparedTo:" + data2); // 'test Decimal comparedTo:1'

let data3: number = data1.comparedTo(10.5);
console.info("test Decimal comparedTo:" + data3); // 'test Decimal comparedTo:-1'
```

### equals

equals(n: Value): boolean

返回该Decimal是否等于比较值。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                  | Android平台 | iOS平台 |
| ------ | --------------- | ---- | --------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 待比较的值或Decimal。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                                             |
| ------- | ------------------------------------------------ |
| boolean | true表示该Decimal与比较值相等，其余情况为false。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(0);
let data1: boolean = data.equals('1e-324');
console.info("test Decimal equals:" + data1); // 'test Decimal equals:false'
```

### greaterThan

greaterThan(n: Value): boolean

返回该Decimal是否大于比较值。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                  | Android平台 | iOS平台 |
| ------ | --------------- | ---- | --------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 待比较的值或Decimal。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                                           |
| ------- | ---------------------------------------------- |
| boolean | true表示该Decimal大于比较值，其余情况为false。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(0.1);
let data1: boolean = data.greaterThan(new Decimal(0.3).sub(0.2)); 
console.info("test Decimal greaterThan:" + data1); // 'test Decimal greaterThan:false'
```

### greaterThanOrEqualTo

greaterThanOrEqualTo(n: Value): boolean

返回该Decimal是否大于等于比较值。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                  | Android平台 | iOS平台 |
| ------ | --------------- | ---- | --------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 待比较的值或Decimal。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                                               |
| ------- | -------------------------------------------------- |
| boolean | true表示该Decimal大于等于比较值，其余情况为false。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(0.3).sub(0.2);
let data1: boolean = data.greaterThanOrEqualTo(0.1);
console.info("test Decimal greaterThanOrEqualTo:" + data1); // 'test Decimal greaterThanOrEqualTo:true'
```

### lessThan

lessThan(n: Value): boolean

返回该Decimal是否小于比较值。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                  | Android平台 | iOS平台 |
| ------ | --------------- | ---- | --------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 待比较的值或Decimal。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                                           |
| ------- | ---------------------------------------------- |
| boolean | true表示该Decimal小于比较值，其余情况为false。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(0.3).sub(0.2);
let data1: boolean = data.lessThan(0.1)
console.info("test Decimal lessThan:" + data1); // 'test Decimal lessThan:false'
```

### lessThanOrEqualTo

lessThanOrEqualTo(n: Value): boolean

返回该Decimal是否小于等于比较值。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                  | Android平台 | iOS平台 |
| ------ | --------------- | ---- | --------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 待比较的值或Decimal。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                                               |
| ------- | -------------------------------------------------- |
| boolean | true表示该Decimal小于等于比较值，其余情况为false。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(0.1);
let data1: boolean = data.lessThanOrEqualTo(new Decimal(0.3).sub(0.2))
console.info("test Decimal lessThanOrEqualTo:" + data1); // 'test Decimal lessThanOrEqualTo:true'
```

### isFinite

isFinite(): boolean

返回该Decimal是否为有限值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                         |
| ------- | -------------------------------------------- |
| boolean | true表示该Decimal为有限值，其余情况为false。 |

**示例：**

```ts
let data: Decimal = new Decimal(1);
let data1: boolean = data.isFinite();
console.info("test Decimal isFinite:" + data1); // 'test Decimal isFinite:true'
```

### isInteger

isInteger(): boolean

返回该Decimal是否为整数。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                       |
| ------- | ------------------------------------------ |
| boolean | true表示该Decimal为整数，其余情况为false。 |

**示例：**

```ts
let data: Decimal = new Decimal(123.456);
let data1: boolean = data.isInteger();
console.info("test Decimal isInteger:" + data1); // 'test Decimal isInteger:false'
```

### isNaN

isNaN(): boolean

返回该Decimal是否为无效值。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                      |
| ------- | ----------------------------------------- |
| boolean | true表示该Decimal为NaN，其余情况为false。 |

**示例：**

```ts
let data: Decimal = new Decimal(NaN);
let data1: boolean = data.isNaN();
console.info("test Decimal isNaN:" + data1); // 'test Decimal isNaN:true'
```

### isNegative

isNegative(): boolean

返回该Decimal是否为负数。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                       |
| ------- | ------------------------------------------ |
| boolean | true表示该Decimal为负数，其余情况为false。 |

**示例：**

```ts
let data: Decimal = new Decimal(-5);
let data1: boolean = data.isNegative();
console.info("test Decimal isNegative:" + data1); // 'test Decimal isNegative:true'

let data2: Decimal = new Decimal(-0);
let data3: boolean = data.isNegative();
console.info("test Decimal isNegative:" + data3); // 'test Decimal isNegative:true'
```

### isPositive

isPositive(): boolean

返回该Decimal是否为正数。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                       |
| ------- | ------------------------------------------ |
| boolean | true表示该Decimal为正数，其余情况为false。 |

**示例：**

```ts
let data: Decimal = new Decimal(5);
let data1: boolean = data.isPositive();
console.info("test Decimal isPositive:" + data1); // 'test Decimal isPositive:true'

let data2: Decimal = new Decimal(0);
let data3: boolean = data.isPositive();
console.info("test Decimal isPositive:" + data3); // 'test Decimal isPositive:true'
```

### isZero

isZero(): boolean

返回该Decimal是否为0或是-0。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                          |
| ------- | --------------------------------------------- |
| boolean | true表示该Decimal为0或是-0，其余情况为false。 |

**示例：**

```ts
let data: Decimal = new Decimal(0);
let data1: boolean = data.isZero();
console.info("test Decimal isZero:" + data1.toString()); // 'test Decimal isZero:true'
```

### dividedToIntegerBy

dividedToIntegerBy(n: Value): Decimal

返回该Decimal除以n后获得的整数部分。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 除法的除数值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                                         |
| ------------------- | ------------------------------------------------------------ |
| [Decimal](#decimal) | 返回一个新的Decimal，其值是将该Decimal的值除以n值的整数部分。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = new Decimal(5);
let data1: Decimal = new Decimal(3);
let data2: Decimal = data.dividedToIntegerBy(data1);
console.info("test Decimal dividedToIntegerBy:" + data2.toString()); // 'test Decimal dividedToIntegerBy:1'
```

### negate

negate(): Decimal

Decimal取反。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                             |
| ------------------- | ------------------------------------------------ |
| [Decimal](#decimal) | 返回一个新的Decimal，其值为该Decimal的值乘以-1。 |

**示例：**

```ts
let data: Decimal = new Decimal(1.8);
let data1: Decimal = data.negate();
console.info("test Decimal negate:" + data1.toString()); // 'test Decimal negate:-1.8'
```

### toBinary

toBinary(): string

转换为二进制表示的字符串。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 返回二进制表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------- |
| 10200001 | The value of 'significantDigits' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toBinary();
console.info("test Decimal toBinary:" + data1); // 'test Decimal toBinary:0b100000000'
```

### toBinary

toBinary(significantDigits: number): string

转换为二进制表示的字符串，可按照significantDigits设置有效数字。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型   | 必填 | 说明                                             | Android平台 | iOS平台 |
| ----------------- | ------ | ---- | ------------------------------------------------ | ----------- | ------- |
| significantDigits | number | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 返回二进制表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------- |
| 10200001 | The value of 'significantDigits' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toBinary(1);
console.info("test Decimal toBinary:" + data1); // 'test Decimal toBinary:0b1p+8'
```

### toBinary

toBinary(significantDigits: number, rounding: Rounding): string

转换为二进制表示的字符串，可按照significantDigits设置有效数字，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ----------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| significantDigits | number                | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。          | 支持        | 支持    |
| rounding          | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 返回二进制表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 10200001 | The value of 'significantDigits \| rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toBinary(1, Decimal.ROUND_HALF_UP);
console.info("test Decimal toBinary:" + data1); // 'test Decimal toBinary:0b1p+8'
```

### toOctal

toOctal(): string

转换为八进制表示的字符串。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 返回八进制表示的字符串。 |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toOctal();
console.info("test Decimal toOctal:" + data1); // 'test Decimal toOctal:0o400'
```

### toOctal

toOctal(significantDigits: number): string

转换为八进制表示的字符串，可按照significantDigits设置有效数字。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型   | 必填 | 说明                                             | Android平台 | iOS平台 |
| ----------------- | ------ | ---- | ------------------------------------------------ | ----------- | ------- |
| significantDigits | number | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 返回八进制表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------- |
| 10200001 | The value of 'significantDigits' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toOctal(1);
console.info("test Decimal toOctal:" + data1); // 'test Decimal toOctal:0o1p+8'
```

### toOctal

toOctal(significantDigits: number, rounding: Rounding): string

转换为八进制表示的字符串，可按照significantDigits设置有效数字，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ----------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| significantDigits | number                | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。          | 支持        | 支持    |
| rounding          | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                     |
| ------ | ------------------------ |
| string | 返回八进制表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 10200001 | The value of 'significantDigits \| rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toOctal(1, Decimal.ROUND_HALF_UP);
console.info("test Decimal toOctal:" + data1); // 'test Decimal toOctal:0o1p+8'
```

### toHexadecimal

toHexadecimal(): string

转换为十六进制表示的字符串。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                       |
| ------ | -------------------------- |
| string | 返回十六进制表示的字符串。 |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toHexadecimal();
console.info("test Decimal toHexadecimal:" + data1); // 'test Decimal toHexadecimal:0x100'
```

### toHexadecimal

toHexadecimal(significantDigits: number): string

转换为十六进制表示的字符串，可按照significantDigits设置有效数字。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型   | 必填 | 说明                                             | Android平台 | iOS平台 |
| ----------------- | ------ | ---- | ------------------------------------------------ | ----------- | ------- |
| significantDigits | number | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                       |
| ------ | -------------------------- |
| string | 返回十六进制表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------- |
| 10200001 | The value of 'significantDigits' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toHexadecimal(1);
console.info("test Decimal toHexadecimal:" + data1); // 'test Decimal toHexadecimal:0x1p+8'
```

### toHexadecimal

toHexadecimal(significantDigits: number, rounding: Rounding): string

转换为十六进制表示的字符串，可按照significantDigits设置有效数字，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ----------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| significantDigits | number                | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。          | 支持        | 支持    |
| rounding          | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                       |
| ------ | -------------------------- |
| string | 返回十六进制表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 10200001 | The value of 'significantDigits \| rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(256);
let data1: string = data.toHexadecimal(1, Decimal.ROUND_HALF_UP);
console.info("test Decimal toHexadecimal:" + data1); // 'test Decimal toHexadecimal:0x1p+8'
```

### toDecimalPlaces

toDecimalPlaces(): Decimal

返回一个保留小数点后指定位数的Decimal对象，不进行小数的取舍。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回一个新的Decimal，保留小数点后指定位数。 |

**示例：**

```ts
let data: Decimal = new Decimal(12.34567);
let data1: Decimal = data.toDecimalPlaces();
console.info("test Decimal toDecimalPlaces:" + data1.toString()); // 'test Decimal toDecimalPlaces:12.34567'
```

### toDecimalPlaces

toDecimalPlaces(decimalPlaces: number): Decimal

返回一个保留小数点后指定位数的Decimal对象，可按照decimalPlaces设置小数位数。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名        | 类型   | 必填 | 说明                                                     | Android平台 | iOS平台 |
| ------------- | ------ | ---- | -------------------------------------------------------- | ----------- | ------- |
| decimalPlaces | number | 是   | 转换时保留的小数点后有效位数，取值范围为[0, 1e9]的整数。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回一个新的Decimal，保留小数点后指定位数。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                      |
| -------- | --------------------------------------------- |
| 10200001 | The value of 'decimalPlaces' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(9876.54321);
let data1: Decimal = data.toDecimalPlaces(3);
console.info("test Decimal toDecimalPlaces:" + data1.toString()); // 'test Decimal toDecimalPlaces:9876.543'
```

### toDecimalPlaces

toDecimalPlaces(decimalPlaces: number, rounding: Rounding): Decimal

返回一个保留小数点后指定位数的Decimal对象，可按照decimalPlaces设置小数位数，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名        | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| decimalPlaces | number                | 是   | 转换时保留的小数点后有效位数，取值范围为[0, 1e9]的整数。  | 支持        | 支持    |
| rounding      | [Rounding](#rounding) | 是   | 转换时使用的舍入模式。取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回一个新的Decimal，保留小数点后指定位数。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                  |
| -------- | --------------------------------------------------------- |
| 10200001 | The value of 'decimalPlaces \| rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(9876.54321);
let data1: Decimal = data.toDecimalPlaces(1, 0);
console.info("test Decimal toDecimalPlaces:" + data1.toString()); // 'test Decimal toDecimalPlaces:9876.6'
data1 = data.toDecimalPlaces(1, Decimal.ROUND_DOWN) // data1：'9876.5'
```

### toExponential

toExponential(): string

转换为按照指数表示法显示的字符串，不进行小数的取舍。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                             |
| ------ | -------------------------------- |
| string | 返回按照指数表示法显示的字符串。 |

**示例：**

```ts
let data: Decimal = new Decimal(45.6);
let data1: string = data.toExponential();
console.info("test Decimal toExponential:" + data1); // 'test Decimal toExponential:4.56e+1'
```

### toExponential

toExponential(decimalPlaces: number): string

转换为按照指数表示法显示的字符串，可按照decimalPlaces设置小数位数。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名        | 类型   | 必填 | 说明                                                     | Android平台 | iOS平台 |
| ------------- | ------ | ---- | -------------------------------------------------------- | ----------- | ------- |
| decimalPlaces | number | 是   | 转换时保留的小数点后有效位数，取值范围为[0, 1e9]的整数。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                             |
| ------ | -------------------------------- |
| string | 返回按照指数表示法显示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                      |
| -------- | --------------------------------------------- |
| 10200001 | The value of 'decimalPlaces' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(45.6);
let data1: string = data.toExponential(0);
console.info("test Decimal toExponential:" + data1); // 'test Decimal toExponential:5e+1'
data1 = data.toExponential(1) // data1：'4.6e+1'
data1 = data.toExponential(3) // data1：'4.560e+1'
```

### toExponential

toExponential(decimalPlaces: number, rounding: Rounding): string

转换为按照指数表示法显示的字符串，可按照decimalPlaces设置小数位数，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名        | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| decimalPlaces | number                | 是   | 转换时保留的小数点后有效位数，取值范围为[0, 1e9]的整数。  | 支持        | 支持    |
| rounding      | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                             |
| ------ | -------------------------------- |
| string | 返回按照指数表示法显示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                  |
| -------- | --------------------------------------------------------- |
| 10200001 | The value of 'decimalPlaces \| rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(45.6);
let data1 = data.toExponential(1, Decimal.ROUND_DOWN)
console.info("test Decimal toExponential:" + data1); // 'test Decimal toExponential:4.5e+1'
```

### toFixed

toFixed(): string

转换为十进制定点模式表示的字符串，不进行小数的取舍。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                             |
| ------------------- | ------------------------------------------------ |
| [Decimal](#decimal) | 返回按照正常模式（十进制定点模式）表示的字符串。 |

**示例：**

```ts
let data: Decimal = new Decimal(3.456);
let data1: string = data.toFixed();
console.info("test Decimal toFixed:" + data1); // 'test Decimal toFixed:3.456'
```

### toFixed

toFixed(decimalPlaces: number): string

转换为十进制定点模式表示的字符串，可按照decimalPlaces设置小数位数。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名        | 类型   | 必填 | 说明                                                     | Android平台 | iOS平台 |
| ------------- | ------ | ---- | -------------------------------------------------------- | ----------- | ------- |
| decimalPlaces | number | 是   | 转换时保留的小数点后有效位数，取值范围为[0, 1e9]的整数。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                             |
| ------------------- | ------------------------------------------------ |
| [Decimal](#decimal) | 返回按照正常模式（十进制定点模式）表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                      |
| -------- | --------------------------------------------- |
| 10200001 | The value of 'decimalPlaces' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(3.456);
let data1: string = data.toFixed(0)
console.info("test Decimal toFixed:" + data1); // 'test Decimal toFixed:3'
data1 = data.toFixed(2) // data1：'3.46'
data1 = data.toFixed(5) // data1：'3.45600'
```

### toFixed

toFixed(decimalPlaces: number, rounding: Rounding): string

转换为十进制定点模式表示的字符串，可按照decimalPlaces设置小数位数，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名        | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| decimalPlaces | number                | 是   | 转换时保留的小数点后有效位数，取值范围为[0, 1e9]的整数。  | 支持        | 支持    |
| rounding      | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                             |
| ------------------- | ------------------------------------------------ |
| [Decimal](#decimal) | 返回按照正常模式（十进制定点模式）表示的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                  |
| -------- | --------------------------------------------------------- |
| 10200001 | The value of 'decimalPlaces \| rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(3.456);
let data1: string = data.toFixed(2, Decimal.ROUND_DOWN);
console.info("test Decimal toFixed:" + data1); // b：'test Decimal toFixed:3.45'
```

### toFraction

toFraction(): Decimal[]

转换为分数表示的数。

**支持平台：** Android 、iOS

**返回值：**

| 类型                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| [Decimal](#decimal)[] | 返回一个Decimal数组，该数组长度固定为2，其值表示为具有整数分子和整数分母的简单分数。且分子在前，分母在后。 |

**示例：**

```ts
let data: Decimal = new Decimal(1.75);
let data1: Decimal[] = data.toFraction();
console.info("test Decimal toFraction:" + data1.toString()); // 'test Decimal toFraction:7,4'
```

### toFraction

toFraction(max_denominator: Value): Decimal[]

转换为分数表示的数，可以通过max_denominator设置最大分母值。

**支持平台：** Android 、iOS

**参数：**

| 参数名          | 类型            | 必填 | 说明                     | Android平台 | iOS平台 |
| --------------- | --------------- | ---- | ------------------------ | ----------- | ------- |
| max_denominator | [Value](#value) | 是   | 分母的最大值。包含该值。 | 支持        | 支持    |

**返回值：**

| 类型                  | 说明                                                         |
| --------------------- | ------------------------------------------------------------ |
| [Decimal](#decimal)[] | 返回一个Decimal数组，该数组长度固定为2，其值表示为具有整数分子和整数分母的简单分数。且分子在前，分母在后。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let pi: Decimal = new Decimal('3.14159265358')
let data1 = pi.toFraction() // data1：'157079632679,50000000000'
data1 = pi.toFraction(100000) // data1：'312689, 99532'
data1 = pi.toFraction(10000) // data1：'355, 113'
data1 = pi.toFraction(100) // data1：'311, 99'
data1 = pi.toFraction(10) // data1：'22, 7'
data1 = pi.toFraction(1) // data1：'3, 1'
```

### toNearest

toNearest(n: Value): Decimal

返回一个新的Decimal，该Decimal为指定值乘以一个倍数后与原Decimal最接近的值。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 参考的指定值。 | 支持        | 支持    |

**返回值：**

| 类型    | 说明                                        |
| ------- | ------------------------------------------- |
| Decimal | 返回一个新的Decimal，指定值最接近的倍数值。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 10200001 | The value of 'rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(1.39);
let data1: Decimal = data.toNearest(0.25);
console.info("test Decimal toNearest:" + data1.toString()); // 'test Decimal toNearest:1.5'
```

### toNearest

toNearest(n: Value, rounding: Rounding): Decimal

返回一个新的Decimal，该Decimal为指定值乘以一个倍数后与原Decimal最接近的值，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名   | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| -------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| n        | [Value](#value)       | 是   | 参考的指定值。                                            | 支持        | 支持    |
| rounding | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回一个新的Decimal，指定值最接近的倍数值。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                 |
| -------- | ---------------------------------------- |
| 10200001 | The value of 'rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(9.499)
let data1 = data.toNearest(0.5, Decimal.ROUND_UP) // data1：'9.5'
data1 = data.toNearest(0.5, Decimal.ROUND_DOWN) // data1：'9'
```

### toPrecision

toPrecision(): string

转换为字符串。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                              |
| ------ | --------------------------------- |
| string | 返回一个表示Decimal对象的字符串。 |

**示例：**

```ts
let data: Decimal = new Decimal(45.6);
let data1: string = data.toPrecision();
console.info("test Decimal toPrecision:" + data1); // 'test Decimal toPrecision:45.6'
```

### toPrecision

toPrecision(significantDigits: number): string

转换为字符串，可按照significantDigits设置有效数字。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型   | 必填 | 说明                   | Android平台 | iOS平台 |
| ----------------- | ------ | ---- | ---------------------- | ----------- | ------- |
| significantDigits | number | 是   | 转换时保留的有效数字。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                              |
| ------ | --------------------------------- |
| string | 返回一个表示Decimal对象的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------- |
| 10200001 | The value of 'significantDigits' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(45.6);
let data1: string = data.toPrecision(1);
console.info("test Decimal toPrecision:" + data1); // 'test Decimal toPrecision:5e+1'
data1 = data.toPrecision(5); // data1：'45.600'
```

### toPrecision

toPrecision(significantDigits: number, rounding: Rounding): string

转换为字符串，可按照significantDigits设置有效数字，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ----------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| significantDigits | number                | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。          | 支持        | 支持    |
| rounding          | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                              |
| ------ | --------------------------------- |
| string | 返回一个表示Decimal对象的字符串。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 10200001 | The value of 'significantDigits \|  rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(45.6);
let data1: string = data.toPrecision(2, Decimal.ROUND_UP) // data1：'46'
data1 = data.toPrecision(2, Decimal.ROUND_DOWN) // data1：'45'
```

### toSignificantDigits

toSignificantDigits(): Decimal

返回一个按照保留有效数字的转换的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**返回值：**

| 类型    | 说明                                    |
| ------- | --------------------------------------- |
| [Decimal](#decimal) | 返回一个保留有效数字后的Decimal对象实例。 |

**示例：**

```ts
let data: Decimal = new Decimal(987.654321);
let data1: Decimal = data.toSignificantDigits();
console.info("test Decimal toSignificantDigits:" + data1.toString()); // 'test Decimal toSignificantDigits:987.654321'
```

### toSignificantDigits

toSignificantDigits(significantDigits: number): Decimal

返回一个按照保留有效数字的转换的Decimal对象，可按照significantDigits设置有效数字。

使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型   | 必填 | 说明                   | Android平台 | iOS平台 |
| ----------------- | ------ | ---- | ---------------------- | ----------- | ------- |
| significantDigits | number | 是   | 转换时保留的有效数字。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                      |
| ------------------- | ----------------------------------------- |
| [Decimal](#decimal) | 返回一个保留有效数字后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                          |
| -------- | ------------------------------------------------- |
| 10200001 | The value of 'significantDigits' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(987.654321);
let data1: Decimal = data.toSignificantDigits(6);
console.info("test Decimal toSignificantDigits:" + data1.toString()); // 'test Decimal toSignificantDigits:987.654'
```

### toSignificantDigits

toSignificantDigits(significantDigits: number, rounding: Rounding): Decimal

返回一个按照保留有效数字的转换的Decimal对象，可按照significantDigits设置有效数字，可按照rounding设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型                  | 必填 | 说明                                                      | Android平台 | iOS平台 |
| ----------------- | --------------------- | ---- | --------------------------------------------------------- | ----------- | ------- |
| significantDigits | number                | 是   | 转换时保留的有效数字，取值范围为[1, 1e9]的整数。          | 支持        | 支持    |
| rounding          | [Rounding](#rounding) | 是   | 转换时使用的舍入模式，取值范围参考[Rounding](#rounding)。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                      |
| ------------------- | ----------------------------------------- |
| [Decimal](#decimal) | 返回一个保留有效数字后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 10200001 | The value of 'significantDigits \|  rounding' is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(987.654321);
let data1: Decimal = data.toSignificantDigits(6, Decimal.ROUND_UP);
console.info("test Decimal toSignificantDigits:" + data1.toString()); // 'test Decimal toSignificantDigits:987.655'
```

### toNumber

toNumber(): number

转换为number类型的值。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                            |
| ------ | ------------------------------- |
| number | 返回一个表示Decimal的number值。 |

**示例：**

```ts
let data: Decimal = new Decimal(456.789);
let data1: number = data.toNumber();
console.info("test Decimal toNumber:" + data1.toString()); // 'test Decimal toNumber:456.789'
```

### toString

toString(): string

返回一个字符串，表示此 Decimal 的值，如果此 Decimal 的正指数等于或大于[toExpPos](#decimalconfig)，或负指数等于或小于[toExpNeg](#decimalconfig)，则将返回指数表示法。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                          |
| ------ | ----------------------------- |
| string | 返回一个表示Decimal的字符串。 |

**示例：**

```ts
let data: Decimal = new Decimal(750000);
let data1: string = data.toString();
console.info("test Decimal toString:" + data1); // 'test Decimal toString:750000'

Decimal.set({ toExpPos: 5 })
data1 = data.toString() // data1:'7.5e+5'

let data2: Decimal = new Decimal(0.000000123)
console.info("test Decimal toString:" + data2.toString()); // 'test Decimal toString:1.23e-7'

Decimal.set({ toExpNeg: -7 })
data1 = data2.toString() // data1:'1.23e-7'
```

### valueOf

valueOf(): string

返回一个字符串，表示此 Decimal 的值，负零包含减号。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                          |
| ------ | ----------------------------- |
| string | 返回一个表示Decimal的字符串。 |

**示例：**

```ts
let data: Decimal = new Decimal(-0);
let data1: string = data.valueOf();
console.info("test Decimal valueOf:" + data1); // 'test Decimal valueOf:-0'
```

### decimalPlaces

decimalPlaces(): number

返回Decimal对象的小数位数。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                        |
| ------ | --------------------------- |
| number | 返回Decimal对象的小数位数。 |

**示例：**

```ts
let data: Decimal = new Decimal(1.234);
let data1: number = data.decimalPlaces();
console.info("test Decimal decimalPlaces:" + data1); // 'test Decimal decimalPlaces:3'
```

### precision

precision(): number

返回Decimal对象的有效数字位数。

**支持平台：** Android 、iOS

**返回值：**

| 类型   | 说明                        |
| ------ | --------------------------- |
| number | 返回Decimal对象的有效位数。 |

**示例：**

```ts
let data: Decimal = new Decimal(1.234);
let data1: number = data.precision();
console.info("test Decimal precision:" + data1); // 'test Decimal precision:4'
```

### precision

precision(includeZeros: boolean | number): number

返回Decimal对象的有效数字位数，通过includeZeros判断是否计算整数部分的尾随零。

**支持平台：** Android 、iOS

**参数：**

| 参数名       | 类型    | 必填 | 说明                                                         | Android平台 | iOS平台 |
| ------------ | ------- | ---- | ------------------------------------------------------------ | ----------- | ------- |
| includeZeros | boolean | 是   | 是否计算整数部分尾随零。true表示计算整数部分尾随零，false表示不计算整数部分尾随零。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                        |
| ------ | --------------------------- |
| number | 返回Decimal对象的有效位数。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                   |
| -------- | ------------------------------------------ |
| 10200001 | The value of includeZeros is out of range. |

**示例：**

```ts
let data: Decimal = new Decimal(987000);
let data1: number = data.precision();
console.info("test Decimal precision:" + data1); // 'test Decimal precision:3'
data1 = data.precision(true) // data1:'6'
```

### abs

static abs(n: Value): Decimal

返回一个新的Decimal对象，Decimal的值为参数n的绝对值。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 取绝对值的参数。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                   |
| ------------------- | -------------------------------------- |
| [Decimal](#decimal) | 返回一个值为参数n的绝对值的Decimal。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.abs(-0.5);
console.info("test Decimal abs:" + data.toString()); // 'test Decimal abs:0.5'
```

### floor

static floor(n: Value): Decimal

返回一个新的Decimal对象，其值为该Decimal向负无穷方向舍入得到的结果。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要舍入的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回舍入之后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.floor(1.8);
console.info("test Decimal floor:" + data.toString()); // 'test Decimal floor:1'
```

### ceil

static ceil(n: Value): Decimal

返回一个新的Decimal对象，其值为该Decimal向正无穷方向舍入得到的结果。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要舍入的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回舍入之后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.ceil(1.8);
console.info("test Decimal ceil:" + data.toString()); // 'test Decimal ceil:2'
```

### trunc

static trunc(n: Value): Decimal

返回一个新的Decimal，其值是将此Decimal截断为整数部分。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要截断的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回截断之后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.trunc(2.5);
console.info("test Decimal trunc:" + data.toString()); // 'test Decimal trunc:2'
```

### clamp

static clamp(n: Value, min: Value, max: Value): Decimal

返回一个值为将该Decimal的值限制在min到max范围内的Decimal对象。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                     | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ------------------------ | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要被限制的值。         | 支持        | 支持    |
| min    | [Value](#value) | 是   | 限制的最小值。包含该值。 | 支持        | 支持    |
| max    | [Value](#value) | 是   | 限制的最大值。包含该值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回符合范围的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200001 | The value of 'min' is out of range.                          |

**示例：**

```ts
let data: Decimal = Decimal.clamp(10.1, 0, 10);
console.info("test Decimal clamp:" + data.toString()); // 'test Decimal clamp:10'
```

### add

static add(x: Value, y: Value): Decimal

返回一个值为x加y的和的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明               | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ------------------ | ----------- | ------- |
| x      | [Value](#value) | 是   | 加法的一个加数。   | 支持        | 支持    |
| y      | [Value](#value) | 是   | 加法的另一个加数。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回加法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.add(0.5, 0.5);
console.info("test Decimal add:" + data.toString()); // 'test Decimal add:1'
```

### sum

static sum(...n: Value[]): Decimal

返回一个值为数组的和的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型              | 必填 | 说明         | Android平台 | iOS平台 |
| ------ | ----------------- | ---- | ------------ | ----------- | ------- |
| n      | [Value](#value)[] | 是   | 加数的序列。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回加法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.sum(0.5, 0.5);
console.info("test Decimal sum:" + data.toString()); // 'test Decimal sum:1'
```

### sub

static sub(x: Value, y: Value): Decimal

返回一个值为x减y的差的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| x      | [Value](#value) | 是   | 减法的被减数。 | 支持        | 支持    |
| y      | [Value](#value) | 是   | 减法的减数。   | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回减法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.sub(1, 0.5);
console.info("test Decimal sub:" + data.toString()); // 'test Decimal sub:0.5'
```

### mul

static mul(x: Value, y: Value): Decimal

返回一个值为x乘以y的积的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| x      | [Value](#value) | 是   | 乘法的被乘数。 | 支持        | 支持    |
| y      | [Value](#value) | 是   | 乘法的乘数。   | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回乘法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.mul(1, 0.5);
console.info("test Decimal mul:" + data.toString()); // 'test Decimal mul:0.5'
```

### div

static div(x: Value, y: Value): Decimal

返回一个值为x除以y的商的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| x      | [Value](#value) | 是   | 除法的被除数。 | 支持        | 支持    |
| y      | [Value](#value) | 是   | 除法的除数。   | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回除法运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |


**示例：**

```ts
let data: Decimal = Decimal.div(1, 0.5);
console.info("test Decimal div:" + data.toString()); // 'test Decimal div:2'
```

### mod

static mod(x: Value, y: Value): Decimal

返回一个新的Decimal对象，其值是x除以y的模。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明               | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ------------------ | ----------- | ------- |
| x      | [Value](#value) | 是   | 模除运算的被除数。 | 支持        | 支持    |
| y      | [Value](#value) | 是   | 模除运算的除数。   | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回模除运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.mod(2, 1);
console.info("test Decimal mod:" + data.toString()); // 'test Decimal mod:0'
```

### sqrt

static sqrt(n: Value): Decimal

返回一个值为n的平方根的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 取平方根的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                |
| ------------------- | ----------------------------------- |
| [Decimal](#decimal) | 返回平方根运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.sqrt(3);
console.info("test Decimal sqrt:" + data.toString()); // 'test Decimal sqrt:1.7320508075688772935'
```

### cbrt

static cbrt(n: Value): Decimal

返回一个值为n的立方根的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 取立方根的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                |
| ------------------- | ----------------------------------- |
| [Decimal](#decimal) | 返回立方根运算后的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.cbrt(3);
console.info("test Decimal cbrt:" + data.toString()); // 'test Decimal cbrt:1.4422495703074083823'
```

### pow

static pow(base: Value, exponent: Value): Decimal

返回一个值为base的exponent次幂的Decimal对象，按照[DecimalConfig.precision](#decimalconfig)设置有效位数，按照[DecimalConfig.rounding](#decimalconfig)设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名   | 类型            | 必填 | 说明               | Android平台 | iOS平台 |
| -------- | --------------- | ---- | ------------------ | ----------- | ------- |
| base     | [Value](#value) | 是   | 幂运算的底数的值。 | 支持        | 支持    |
| exponent | [Value](#value) | 是   | 幂运算的幂的值。   | 支持        | 支持    |

**返回值：**

| 类型                | 说明                            |
| ------------------- | ------------------------------- |
| [Decimal](#decimal) | 返回幂运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.pow(3, -2);
console.info("test Decimal pow:" + data.toString()); // 'test Decimal pow:0.11111111111111111111'
```

### exp

static exp(n: Value): Decimal

返回一个值为n的自然指数的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                 | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求自然指数的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                  |
| ------------------- | ------------------------------------- |
| [Decimal](#decimal) | 返回自然指数运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.exp(2);
console.info("test Decimal exp:" + data.toString()); // 'test Decimal exp:7.3890560989306502272'
```

### log

static log(n: Value, base: Value): Decimal

返回一个值为以base为底n的对数的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 对数运算的真数。 | 支持        | 支持    |
| base   | [Value](#value) | 是   | 对数运算的底。   | 支持        | 支持    |

**返回值：**

| 类型                | 说明                              |
| ------------------- | --------------------------------- |
| [Decimal](#decimal) | 返回对数运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.log(2, 256);
console.info("test Decimal log:" + data.toString()); // 'test Decimal log:0.125'
```

### ln

static ln(n: Value): Decimal

返回一个值为n的自然对数的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 对数运算的真数。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                  |
| ------------------- | ------------------------------------- |
| [Decimal](#decimal) | 返回自然对数运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.ln(1.23e+30);
console.info("test Decimal ln:" + data.toString()); // 'test Decimal ln:69.284566959205696648'
```

### log2

static log2(n: Value): Decimal

返回一个值为以2为底n的对数的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 对数运算的真数。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                       |
| ------------------- | ------------------------------------------ |
| [Decimal](#decimal) | 返回以2为底的对数运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.log2(4);
console.info("test Decimal log2:" + data.toString()); // 'test Decimal log2:2'
```

### log10

static log10(n: Value): Decimal

返回一个值为以10为底n的对数的Decimal对象。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 对数运算的真数。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                        |
| ------------------- | ------------------------------------------- |
| [Decimal](#decimal) | 返回以10为底的对数运算后的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.log10(10000);
console.info("test Decimal log10:" + data.toString()); // 'test Decimal log10:4'
```

### cos

static cos(n: Value): Decimal

返回一个新的Decimal，其值是n的余弦值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 要求余弦值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                   |
| ------------------- | -------------------------------------- |
| [Decimal](#decimal) | 返回n的余弦值对应的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.cos(-0.25);
console.info("test Decimal cos:" + data.toString()); // 'test Decimal cos:0.96891242171064478414'
```

### sin

static sin(n: Value): Decimal

返回一个新的Decimal，其值是n的正弦值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 要求正弦值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                   |
| ------------------- | -------------------------------------- |
| [Decimal](#decimal) | 返回n的正弦值对应的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.sin(0.75);
console.info("test Decimal sin:" + data.toString()); // 'test Decimal sin:0.68163876002333416673'
```

### tan

static tan(n: Value): Decimal

返回一个新的Decimal，其值是n的正切值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明             | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 要求正切值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                   |
| ------------------- | -------------------------------------- |
| [Decimal](#decimal) | 返回n的正切值对应的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.tan(0.75);
console.info("test Decimal tan:" + data.toString()); // 'test Decimal tan:0.93159645994407246117'
```

### cosh

static cosh(n: Value): Decimal

返回一个新的Decimal，其值是n的双曲余弦值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                   | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求双曲余弦值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                       |
| ------------------- | ------------------------------------------ |
| [Decimal](#decimal) | 返回n的双曲余弦值对应的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.cosh(0.5);
console.info("test Decimal cosh:" + data.toString()); // 'test Decimal cosh:1.1276259652063807852'
```

### sinh

static sinh(n: Value): Decimal

返回一个新的Decimal，其值是n的双曲正弦值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                   | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求双曲正弦值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                       |
| ------------------- | ------------------------------------------ |
| [Decimal](#decimal) | 返回n的双曲正弦值对应的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.sinh(0.5);
console.info("test Decimal sinh:" + data.toString()); // 'test Decimal sinh:0.52109530549374736162'
```

### tanh

static tanh(n: Value): Decimal

返回一个新的Decimal，其值是n的双曲正切值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                   | Android平台 | iOS平台 |
| ------ | --------------- | ---- | ---------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求双曲正切值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                       |
| ------------------- | ------------------------------------------ |
| [Decimal](#decimal) | 返回n的双曲正切值对应的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.tanh(0.5);
console.info("test Decimal tanh:" + data.toString()); // 'test Decimal tanh:0.4621171572600097585'
```

### acos

static acos(n: Value): Decimal

返回一个新的Decimal，其值是n的反余弦值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                 | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求反余弦值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                   |
| ------------------- | -------------------------------------- |
| [Decimal](#decimal) | 返回n的反余弦值对应的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.acos(0.5);
console.info("test Decimal acos:" + data.toString()); // 'test Decimal acos:1.0471975511965977462'
```

### asin

static asin(n: Value): Decimal

返回一个新的Decimal，其值是n的反正弦值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                 | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求反正弦值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                   |
| ------------------- | -------------------------------------- |
| [Decimal](#decimal) | 返回n的反正弦值对应的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.asin(0.75);
console.info("test Decimal asin:" + data.toString()); // 'test Decimal asin:0.84806207898148100805'
```

### atan

static atan(n: Value): Decimal

返回一个新的Decimal，其值是n的反正切值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                 | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求反正切值的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                   |
| ------------------- | -------------------------------------- |
| [Decimal](#decimal) | 返回n的反正切值对应的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.atan(0.75);
console.info("test Decimal atan:" + data.toString()); // 'test Decimal atan:0.6435011087932843868'
```

### acosh

static acosh(n: Value): Decimal

返回一个新的Decimal，其值是n的双曲余弦值的倒数。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                       | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求双曲余弦的倒数的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                           |
| ------------------- | ---------------------------------------------- |
| [Decimal](#decimal) | 返回n的双曲余弦的倒数对应的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.acosh(50);
console.info("test Decimal acosh:" + data.toString()); // 'test Decimal acosh:4.6050701709847571595'
```

### asinh

static asinh(n: Value): Decimal

返回一个新的Decimal，其值是n的双曲正弦值的倒数。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                       | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求双曲正弦的倒数的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                           |
| ------------------- | ---------------------------------------------- |
| [Decimal](#decimal) | 返回n的双曲正弦的倒数对应的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.asinh(50);
console.info("test Decimal asinh:" + data.toString()); // 'test Decimal asinh:4.6052701709914238266'
```

### atanh

static atanh(n: Value): Decimal

返回一个新的Decimal，其值是n的双曲正切值的倒数。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                       | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要求双曲正切的倒数的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                           |
| ------------------- | ---------------------------------------------- |
| [Decimal](#decimal) | 返回n的双曲正切的倒数对应的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.atanh(0.75);
console.info("test Decimal atanh:" + data.toString()); // 'test Decimal atanh:0.97295507452765665255'
```

### atan2

static atan2(y: Value, x: Value): Decimal

返回一个新的Decimal，其值是为-π到π范围内的y/x反正切值。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| y      | [Value](#value) | 是   | 除法的被除数。 | 支持        | 支持    |
| x      | [Value](#value) | 是   | 除法的除数。   | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                                       |
| ------------------- | ---------------------------------------------------------- |
| [Decimal](#decimal) | 返回-pi 到 pi 范围内的"y/x"反正切值对应的Decimal对象实例。 |

**错误码**：

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200060 | Precision limit exceeded.                                    |

**示例：**

```ts
let data: Decimal = Decimal.atan2(2, 3);
console.info("test Decimal atan2:" + data.toString()); // 'test Decimal atan2:0.58800260354756755125'
```

### hypot

static hypot(...n: Value[]): Decimal

返回一个新的Decimal，其值为参数平方和的平方根。

使用[DecimalConfig.precision](#decimalconfig)的值进行有效数字的保留，使用[DecimalConfig.rounding](#decimalconfig)的值设置舍入模式。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型              | 必填 | 说明                 | Android平台 | iOS平台 |
| ------ | ----------------- | ---- | -------------------- | ----------- | ------- |
| n      | [Value](#value)[] | 是   | 需要求平方和的序列。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                              |
| ------------------- | ------------------------------------------------- |
| [Decimal](#decimal) | 返回值为所有参数平方和的平方根的Decimal对象实例。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.hypot(2, 3, 4);
console.info("test Decimal hypot:" + data.toString()); // 'test Decimal hypot:5.3851648071345040313'
```

### max

static max(...n: Value[]): Decimal

返回一个值为所有参数中最大值的Decimal对象。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型              | 必填 | 说明                 | Android平台 | iOS平台 |
| ------ | ----------------- | ---- | -------------------- | ----------- | ------- |
| n      | [Value](#value)[] | 是   | 需要求最大值的序列。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                      |
| ------------------- | ----------------------------------------- |
| [Decimal](#decimal) | 返回所有参数中的最大值的Decimal对象实例。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.max(2, 3, 4);
console.info("test Decimal max:" + data.toString()); // 'test Decimal max:4'
```

### min

static min(...n: Value[]): Decimal

返回一个值为所有参数中最小值的Decimal对象。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明                 | Android平台        | iOS平台            |
| ------ | --------------- | ---- | -------------------- | -------------------- | -------------------- |
| n      | [Value](#value)[] | 是   | 需要求最小值的序列。 | 支持 | 支持 |

**返回值：**

| 类型                | 说明                                      |
| ------------------- | ----------------------------------------- |
| [Decimal](#decimal) | 返回所有参数中的最小值的Decimal对象实例。 |

**错误码**：

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: Decimal = Decimal.min(2, 3, 4);
console.info("test Decimal min:" + data.toString()); // 'test Decimal min:2'
```

### random

static random(): Decimal

返回一个值为大于等于0小于1的随机值的Decimal对象。

**支持平台：** Android 、iOS

**返回值：**

| 类型                | 说明                                      |
| ------------------- | ----------------------------------------- |
| [Decimal](#decimal) | 大于等于0小于1的随机值的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息            |
| -------- | ------------------- |
| 10200061 | Crypto unavailable. |

**示例：**

```ts
let data: Decimal = Decimal.random();
```

### random

static random(significantDigits: number): Decimal

返回一个值为大于等于0小于1的随机值的Decimal对象，随机值保留significantDigits位有效数字。

**支持平台：** Android 、iOS

**参数：**

| 参数名            | 类型   | 必填 | 说明                   | Android平台 | iOS平台 |
| ----------------- | ------ | ---- | ---------------------- | ----------- | ------- |
| significantDigits | number | 是   | 随机值保留的有效数字。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                      |
| ------------------- | ----------------------------------------- |
| [Decimal](#decimal) | 大于等于0小于1的随机值的Decimal对象实例。 |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200061 | Crypto unavailable.                                          |

**示例：**

```ts
let data: Decimal = Decimal.random(20);
```

### sign

static sign(n: Value): number

根据参数的值进行判断返回对应的值：当n>0返回1，当n<0返回-1，当n==0返回0，当n==-0返回-0，否则返回NaN。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要判断的值。 | 支持        | 支持    |

**返回值：**

| 类型   | 说明                               |
| ------ | ---------------------------------- |
| number | 根据参数的值进行判断返回对应的值。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let data: number = Decimal.sign(2);
console.info("test Decimal sign:" + data); // 'test Decimal sign:1'
```

### round

static round(n: Value): Decimal

返回一个新的Decimal，其值是使用[DecimalConfig.rounding](#decimalconfig)模式舍入为整数的n。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型            | 必填 | 说明           | Android平台 | iOS平台 |
| ------ | --------------- | ---- | -------------- | ----------- | ------- |
| n      | [Value](#value) | 是   | 需要舍入的值。 | 支持        | 支持    |

**返回值：**

| 类型                | 说明                                      |
| ------------------- | ----------------------------------------- |
| [Decimal](#decimal) | 返回舍入之后的整数对应的Decimal对象实例。 |

**错误码：**

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |

**示例：**

```ts
let x = 3.3333333333333;
let data = Decimal.round(x);
console.info("test Decimal round:" + data.toString()); // 'test Decimal round:3'
```

### set

static set(object: DecimalConfig):void

用于设置Decimal的配置属性，通过set设置的属性是全局生效的。

**支持平台：** Android 、iOS

**参数：**

| 参数名 | 类型                            | 必填 | 说明                 | Android平台 | iOS平台 |
| ------ | ------------------------------- | ---- | -------------------- | ----------- | ------- |
| object | [DecimalConfig](#decimalconfig) | 是   | 需要配置的属性集合。 | 支持        | 支持    |

**错误码：**

以下错误码的详细介绍请参见[语言基础类库错误码](../errorcodes/errorcode-utils.md)。

| 错误码ID | 错误信息                                                     |
| -------- | ------------------------------------------------------------ |
| 401      | Parameter error. Possible causes:<br/>1. Incorrect parameter types;<br/>2. Parameter verification failed. |
| 10200001 | The value of 'DecimalConfig.properties' is out of range.     |
| 10200061 | Crypto unavailable.                                          |

**示例1：**

```ts
let data : Decimal = new Decimal(1.2345678901234567);
Decimal.set({
    precision: 5,
    rounding: 4,
    toExpNeg: -7,
    toExpPos: 7,
    maxE: 9e15,
    minE: -9e15,
    modulo: 1,
    crypto: false
})
let data1 : Decimal = data.add(0.5);
console.info("test Decimal set:" + data1.toString()); // "test Decimal set:1.7346"
// 将配置属性全部设置为默认值
Decimal.set({ defaults: true })
let data2 : Decimal = data.add(0.5);
console.info("test Decimal set:" + data2.toString()); // "test Decimal set:1.7345678901234567"
// 最大有效位数设置为10，其余配置属性设置为默认值
Decimal.set({ precision: 10, defaults: true })
let data3 : Decimal = data.add(0.5);
console.info("test Decimal set:" + data3.toString()); // "test Decimal set:1.73456789"

// toExpNeg和toExpPos的用法
Decimal.set({ toExpNeg: -7 })
let x0 : Decimal = new Decimal(0.00000123) // x0:'0.00000123'
let x1 : Decimal = new Decimal(0.000000123) // x1:'1.23e-7'

Decimal.set({ toExpPos: 2 })
let y0 : Decimal = new Decimal(12.3) // y0:'12.3'
let y1 : Decimal = new Decimal(123) // y1:'1.23e+2'

// 所有数据均使用科学计数法表示
Decimal.set({ toExpPos: 0 })

// minE和maxE的用法
Decimal.set({ minE: -500 })
let a0 : Decimal = new Decimal('1e-500') // a0:'1e-500'
let a1 : Decimal = new Decimal('9.9e-501') // a1:'0e0'

Decimal.set({ minE: -3 })
let b0 : Decimal = new Decimal(0.001) // b0:'0.001'
let b1 : Decimal = new Decimal(0.0001) // b1:'0e0'

Decimal.set({ maxE: 500 })
let c0 : Decimal = new Decimal('9.999e500') // c0:'9.999e+500'
let c1 : Decimal = new Decimal('1e501') // c1:'Infinity'

Decimal.set({ maxE: 4 })
let d0 : Decimal = new Decimal(99999) // d0:'9.9999e+4'
let d1 : Decimal = new Decimal(100000) // d1:'Infinity'
```

**示例2：**

```ts
// /entry/src/main/ets/pages/test.ets
export function test(){
  let data : Decimal = new Decimal(1.2345678901234567);
  Decimal.set({
    precision: 5,
    rounding: 0,
    toExpNeg: -7,
    toExpPos: 7,
    maxE: 9e15,
    minE: -9e15,
    modulo: 1,
    crypto: false
  })
  let data1 : Decimal = data.add(0.5);
  console.info("test Decimal set:" + data1.toString()); // 'test Decimal set:1.7346'
}
```

```ts
// /entry/src/main/ets/pages/Index.ets
import {test} from './test'

let data : Decimal = new Decimal(1.2345678901234567);
Decimal.set({
  precision: 6,
  rounding: 1,
  toExpNeg: -7,
  toExpPos: 7,
  maxE: 9e15,
  minE: -9e15,
  modulo: 1,
  crypto: false
})
let data1 : Decimal = data.add(0.5);
console.info("test Decimal set:" + data1.toString()); // 'test Decimal set:1.73456'
test();
data1 = data1.add(0); // data1:'1.7346'
console.info("test Decimal set:" + data1.toString()); // 'test Decimal set:1.7346'
```