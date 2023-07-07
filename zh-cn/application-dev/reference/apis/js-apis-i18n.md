# @ohos.i18n (国际化-I18n)

 本模块提供系统相关的或者增强的国际化能力，包括区域管理、日历等，相关接口为ECMA 402标准中未定义的补充接口。
[Intl模块](js-apis-intl.md)提供了ECMA 402标准定义的基础国际化接口，与本模块共同使用可提供完整地国际化支持能力。 

>  **说明：**
>  - 本模块首批接口从API version 7开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
>  - I18N模块包含国际化能力增强接口（未在ECMA 402中定义），包括区域管理、日历等，国际化基础能力请参考[Intl模块](js-apis-intl.md)。


## 导入模块

```js
import I18n from '@ohos.i18n';
```


## System<sup>9+</sup>

### getDisplayCountry<sup>9+</sup>

static getDisplayCountry(country: string, locale: string, sentenceCase?: boolean): string

获取指定国家的本地化显示文本。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名          | 类型      | 必填   | 说明               |
| ------------ | ------- | ---- | ---------------- |
| country      | string  | 是    | 指定国家。            |
| locale       | string  | 是    | 显示指定国家的区域ID。     |
| sentenceCase | boolean | 否    | 本地化显示文本是否要首字母大写。 |

**返回值：** 

| 类型     | 说明            |
| ------ | ------------- |
| string | 指定国家的本地化显示文本。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid |

**示例：** 
  ```js
  try {
    let displayCountry = I18n.System.getDisplayCountry("zh-CN", "en-GB"); // displayCountry = "China"
  } catch(error) {
    console.error(`call System.getDisplayCountry failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getDisplayLanguage<sup>9+</sup>

static getDisplayLanguage(language: string, locale: string, sentenceCase?: boolean): string

获取指定语言的本地化显示文本。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名          | 类型      | 必填   | 说明               |
| ------------ | ------- | ---- | ---------------- |
| language     | string  | 是    | 指定语言。            |
| locale       | string  | 是    | 显示指定语言的区域ID。     |
| sentenceCase | boolean | 否    | 本地化显示文本是否要首字母大写。 |

**返回值：** 

| 类型     | 说明            |
| ------ | ------------- |
| string | 指定语言的本地化显示文本。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid |

**示例：** 
  ```js
  try {
    let displayLanguage = I18n.System.getDisplayLanguage("zh", "en-GB"); // displayLanguage = Chinese
  } catch(error) {
    console.error(`call System.getDisplayLanguage failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getSystemLanguage<sup>9+</sup>

static getSystemLanguage(): string

获取系统语言。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明      |
| ------ | ------- |
| string | 系统语言ID。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid |

**示例：** 
  ```js
  try {
    let systemLanguage = I18n.System.getSystemLanguage();  // systemLanguage为当前系统语言
  } catch(error) {
    console.error(`call System.getSystemLanguage failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getSystemRegion<sup>9+</sup>

static getSystemRegion(): string

获取系统地区。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明      |
| ------ | ------- |
| string | 系统地区ID。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid |

**示例：** 
  ```js
  try {
    let systemRegion = I18n.System.getSystemRegion(); // 获取系统当前地区设置
  } catch(error) {
    console.error(`call System.getSystemRegion failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getSystemLocale<sup>9+</sup>

static getSystemLocale(): string

获取系统区域。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明      |
| ------ | ------- |
| string | 系统区域ID。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid |

**示例：** 
  ```js
  try {
    let systemLocale = I18n.System.getSystemLocale();  // 获取系统当前Locale
  } catch(error) {
    console.error(`call System.getSystemLocale failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### is24HourClock<sup>9+</sup>

static is24HourClock(): boolean

判断系统时间是否为24小时制。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型      | 说明                                       |
| ------- | ---------------------------------------- |
| boolean | 返回true，表示系统24小时开关开启；返回false，表示系统24小时开关关闭。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid |

**示例：** 
  ```js
  try {
    let is24HourClock = I18n.System.is24HourClock();  // 系统24小时开关是否开启
  } catch(error) {
    console.error(`call System.is24HourClock failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```


## I18n.getCalendar<sup>8+</sup>

getCalendar(locale: string, type? : string): Calendar

获取日历对象。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名    | 类型     | 必填   | 说明                                       |
| ------ | ------ | ---- | ---------------------------------------- |
| locale | string | 是    | 合法的locale值，例如zh-Hans-CN。                 |
| type   | string | 否    | 合法的日历类型，目前合法的类型有buddhist,&nbsp;chinese,&nbsp;coptic,&nbsp;ethiopic,&nbsp;hebrew,&nbsp;gregory,&nbsp;indian,&nbsp;islamic_civil,&nbsp;islamic_tbla,&nbsp;islamic_umalqura,&nbsp;japanese,&nbsp;persian。当type没有给出时，采用区域默认的日历类型。 |

**返回值：** 

| 类型                     | 说明    |
| ---------------------- | ----- |
| [Calendar](#calendar8) | 日历对象。 |

**示例：** 
  ```js
  I18n.getCalendar("zh-Hans", "chinese"); // 获取中国农历日历对象
  ```


## Calendar<sup>8+</sup>


### setTime<sup>8+</sup>

setTime(date: Date): void

设置日历对象内部的时间日期。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型   | 必填   | 说明                |
| ---- | ---- | ---- | ----------------- |
| date | Date | 是    | 将要设置的日历对象的内部时间日期。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("en-US", "gregory");
  let date = new Date(2021, 10, 7, 8, 0, 0, 0);
  calendar.setTime(date);
  ```


### setTime<sup>8+</sup>

setTime(time: number): void

设置日历对象内部的时间日期, time为从1970.1.1 00:00:00 GMT逝去的毫秒数。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明                                       |
| ---- | ------ | ---- | ---------------------------------------- |
| time | number | 是    | time为从1970.1.1&nbsp;00:00:00&nbsp;GMT逝去的毫秒数。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("en-US", "gregory");
  calendar.setTime(10540800000);
  ```


### set<sup>8+</sup>

set(year: number, month: number, date:number, hour?: number, minute?: number, second?: number): void

设置日历对象的年、月、日、时、分、秒。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名    | 类型     | 必填   | 说明     |
| ------ | ------ | ---- | ------ |
| year   | number | 是    | 设置的年。  |
| month  | number | 是    | 设置的月。  |
| date   | number | 是    | 设置的日。  |
| hour   | number | 否    | 设置的小时。 |
| minute | number | 否    | 设置的分钟。 |
| second | number | 否    | 设置的秒。  |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 10, 1, 8, 0, 0); // set time to 2021.10.1 08:00:00
  ```


### setTimeZone<sup>8+</sup>

setTimeZone(timezone: string): void

设置日历对象的时区。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名      | 类型     | 必填   | 说明                        |
| -------- | ------ | ---- | ------------------------- |
| timezone | string | 是    | 设置的时区id，如“Asia/Shanghai”。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setTimeZone("Asia/Shanghai");
  ```


### getTimeZone<sup>8+</sup>

getTimeZone(): string

获取日历对象的时区。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明         |
| ------ | ---------- |
| string | 日历对象的时区id。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setTimeZone("Asia/Shanghai");
  let timezone = calendar.getTimeZone(); // timezone = "Asia/Shanghai"
  ```


### getFirstDayOfWeek<sup>8+</sup>

getFirstDayOfWeek(): number

获取日历对象的一周起始日。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明                    |
| ------ | --------------------- |
| number | 获取一周的起始日，1代表周日，7代表周六。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("en-US", "gregory");
  let firstDayOfWeek = calendar.getFirstDayOfWeek(); // firstDayOfWeek = 1
  ```


### setFirstDayOfWeek<sup>8+</sup>

setFirstDayOfWeek(value: number): void

设置每一周的起始日。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名   | 类型     | 必填   | 说明                    |
| ----- | ------ | ---- | --------------------- |
| value | number | 是    | 设置一周的起始日，1代表周日，7代表周六。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setFirstDayOfWeek(3);
  let firstDayOfWeek = calendar.getFirstDayOfWeek(); // firstDayOfWeek = 3
  ```


### getMinimalDaysInFirstWeek<sup>8+</sup>

getMinimalDaysInFirstWeek(): number

获取一年中第一周的最小天数。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明           |
| ------ | ------------ |
| number | 一年中第一周的最小天数。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  let minimalDaysInFirstWeek = calendar.getMinimalDaysInFirstWeek(); // minimalDaysInFirstWeek = 1
  ```


### setMinimalDaysInFirstWeek<sup>8+</sup>

setMinimalDaysInFirstWeek(value: number): void

设置一年中第一周的最小天数。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名   | 类型     | 必填   | 说明           |
| ----- | ------ | ---- | ------------ |
| value | number | 是    | 一年中第一周的最小天数。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setMinimalDaysInFirstWeek(3);
  let minimalDaysInFirstWeek = calendar.getMinimalDaysInFirstWeek(); // minimalDaysInFirstWeek = 3
  ```


### get<sup>8+</sup>

get(field: string): number

获取日历对象中与field相关联的值。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名   | 类型     | 必填   | 说明                                       |
| ----- | ------ | ---- | ---------------------------------------- |
| field | string | 是    | 通过field来获取日历对象相应的值。目前支持的field值有&nbsp;era,&nbsp;year,&nbsp;month,&nbsp;week_of_year,&nbsp;week_of_month,&nbsp;date,&nbsp;day_of_year,&nbsp;day_of_week,&nbsp;day_of_week_in_month,&nbsp;hour,&nbsp;hour_of_day,&nbsp;minute,&nbsp;second,&nbsp;millisecond,&nbsp;zone_offset,&nbsp;dst_offset,&nbsp;year_woy,&nbsp;dow_local,&nbsp;extended_year,&nbsp;julian_day,&nbsp;milliseconds_in_day,&nbsp;is_leap_month。 |

**返回值：** 

| 类型     | 说明                                       |
| ------ | ---------------------------------------- |
| number | 与field相关联的值，如当前Calendar对象的内部日期的年份为1990，get("year")返回1990。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 10, 1, 8, 0, 0); // set time to 2021.10.1 08:00:00
  let hourOfDay = calendar.get("hour_of_day"); // hourOfDay = 8
  ```


### isWeekend<sup>8+</sup>

isWeekend(date?: Date): boolean

判断给定的日期是否在日历中是周末。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型   | 必填   | 说明                                       |
| ---- | ---- | ---- | ---------------------------------------- |
| date | Date | 否    | 判断日期在日历中是否是周末。如果不传日期参数，则判断当前日期是否为周末。 |

**返回值：** 

| 类型      | 说明                                  |
| ------- | ----------------------------------- |
| boolean | 当所判断的日期为周末时，返回&nbsp;true，否则返回false。 |

**示例：** 
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 11, 11, 8, 0, 0); // set time to 2021.11.11 08:00:00
  calendar.isWeekend(); // false
  let date = new Date(2011, 11, 6, 9, 0, 0);
  calendar.isWeekend(date); // true
  ```


## I18n.getTimeZone<sup>7+</sup>

getTimeZone(zoneID?: string): TimeZone

获取时区ID对应的时区对象。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名    | 类型     | 必填   | 说明    |
| ------ | ------ | ---- | ----- |
| zondID | string | 否    | 时区ID。 |

**返回值：** 

| 类型       | 说明           |
| -------- | ------------ |
| TimeZone | 时区ID对应的时区对象。 |

**示例：** 
  ```js
  let timezone = I18n.getTimeZone();
  ```


## TimeZone


### getID

getID(): string

获取时区对象的ID。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明           |
| ------ | ------------ |
| string | 时区对象对应的时区ID。 |

**示例：** 
  ```js
  let timezone = I18n.getTimeZone();
  let timezoneID = timezone.getID(); // timezoneID = "Asia/Shanghai"
  ```


### getRawOffset

getRawOffset(): number

获取时区对象表示的时区与UTC时区的偏差。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明                  |
| ------ | ------------------- |
| number | 时区对象表示的时区与UTC时区的偏差。 |

**示例：** 
  ```js
  let timezone = I18n.getTimeZone();
  let offset = timezone.getRawOffset(); // offset = 28800000
  ```


### getOffset

getOffset(date?: number): number

获取某一时刻时区对象表示的时区与UTC时区的偏差。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型     | 说明                      |
| ------ | ----------------------- |
| number | 某一时刻时区对象表示的时区与UTC时区的偏差。 |

**示例：** 
  ```js
  let timezone = I18n.getTimeZone();
  let offset = timezone.getOffset(1234567890); // offset = 28800000
  ```


### getAvailableIDs<sup>9+</sup>

static getAvailableIDs(): Array&lt;string&gt;

获取系统支持的时区ID。

**系统能力**：SystemCapability.Global.I18n

**返回值：** 

| 类型                  | 说明          |
| ------------------- | ----------- |
| Array&lt;string&gt; | 系统支持的时区ID列表 |

**示例：** 
  ```ts
  // ids = ["America/Adak", "America/Anchorage", "America/Bogota", "America/Denver", "America/Los_Angeles", "America/Montevideo", "America/Santiago", "America/Sao_Paulo", "Asia/Ashgabat", "Asia/Hovd", "Asia/Jerusalem", "Asia/Magadan", "Asia/Omsk", "Asia/Shanghai", "Asia/Tokyo", "Asia/Yerevan", "Atlantic/Cape_Verde", "Australia/Lord_Howe", "Europe/Dublin", "Europe/London", "Europe/Moscow", "Pacific/Auckland", "Pacific/Easter", "Pacific/Pago-Pago"], 当前共支持24个时区
  let ids = I18n.TimeZone.getAvailableIDs();
  ```


## Unicode<sup>9+</sup>


### isDigit<sup>9+</sup>

static isDigit(char: string): boolean

判断字符串char是否是数字。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                   |
| ------- | ------------------------------------ |
| boolean | 返回true表示输入的字符是数字，返回false表示输入的字符不是数字。 |

**示例：** 
  ```js
  let isdigit = I18n.Unicode.isDigit("1");  // isdigit = true
  ```


### isSpaceChar<sup>9+</sup>

static isSpaceChar(char: string): boolean

判断字符串char是否是空格符。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                     |
| ------- | -------------------------------------- |
| boolean | 返回true表示输入的字符是空格符，返回false表示输入的字符不是空格符。 |

**示例：** 
  ```js
  let isspacechar = I18n.Unicode.isSpaceChar("a");  // isspacechar = false
  ```


### isWhitespace<sup>9+</sup>

static isWhitespace(char: string): boolean

判断字符串char是否是空白符。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                     |
| ------- | -------------------------------------- |
| boolean | 返回true表示输入的字符是空白符，返回false表示输入的字符不是空白符。 |

**示例：** 
  ```js
  let iswhitespace = I18n.Unicode.isWhitespace("a");  // iswhitespace = false
  ```


### isRTL<sup>9+</sup>

static isRTL(char: string): boolean

判断字符串char是否是从右到左语言的字符。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                       |
| ------- | ---------------------------------------- |
| boolean | 返回true表示输入的字符是从右到左语言的字符，返回false表示输入的字符不是从右到左语言的字符。 |

**示例：** 
  ```js
  let isrtl = I18n.Unicode.isRTL("a");  // isrtl = false
  ```


### isIdeograph<sup>9+</sup>

static isIdeograph(char: string): boolean

判断字符串char是否是表意文字。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                       |
| ------- | ---------------------------------------- |
| boolean | 返回true表示输入的字符是表意文字，返回false表示输入的字符不是表意文字。 |

**示例：** 
  ```js
  let isideograph = I18n.Unicode.isIdeograph("a");  // isideograph = false
  ```


### isLetter<sup>9+</sup>

static isLetter(char: string): boolean

判断字符串char是否是字母。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                   |
| ------- | ------------------------------------ |
| boolean | 返回true表示输入的字符是字母，返回false表示输入的字符不是字母。 |

**示例：** 
  ```js
  let isletter = I18n.Unicode.isLetter("a");  // isletter = true
  ```


### isLowerCase<sup>9+</sup>

static isLowerCase(char: string): boolean

判断字符串char是否是小写字母。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                       |
| ------- | ---------------------------------------- |
| boolean | 返回true表示输入的字符是小写字母，返回false表示输入的字符不是小写字母。 |

**示例：** 
  ```js
  let islowercase = I18n.Unicode.isLowerCase("a");  // islowercase = true
  ```


### isUpperCase<sup>9+</sup>

static isUpperCase(char: string): boolean

判断字符串char是否是大写字母。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型      | 说明                                       |
| ------- | ---------------------------------------- |
| boolean | 返回true表示输入的字符是大写字母，返回false表示输入的字符不是大写字母。 |

**示例：** 
  ```js
  let isuppercase = I18n.Unicode.isUpperCase("a");  // isuppercase = false
  ```


### getType<sup>9+</sup>

static getType(char: string): string

获取输入字符串的一般类别值。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名  | 类型     | 必填   | 说明    |
| ---- | ------ | ---- | ----- |
| char | string | 是    | 输入字符。 |

**返回值：** 

| 类型     | 说明          |
| ------ | ----------- |
| string | 输入字符的一般类别值。 |

**示例：** 
  ```js
  let type = I18n.Unicode.getType("a"); // type = "U_LOWERCASE_LETTER"
  ```


## I18NUtil<sup>9+</sup>

### getDateOrder<sup>9+</sup>

static getDateOrder(locale: string): string

获取某一区域的日期的年、月、日排列顺序。

**系统能力**：SystemCapability.Global.I18n

**参数：** 

| 参数名    | 类型     | 必填   | 说明                        |
| ------ | ------ | ---- | ------------------------- |
| locale | string | 是    | 格式化时使用的区域参数，如：zh-Hans-CN。 |

**返回值：** 

| 类型     | 说明                  |
| ------ | ------------------- |
| string | 返回某一区域的日期的年、月、日排列顺序 |

**示例：** 
  ```js
  let order = I18n.I18NUtil.getDateOrder("zh-CN");  // order = "y-L-d"
  ```
