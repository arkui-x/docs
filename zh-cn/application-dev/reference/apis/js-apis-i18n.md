# @ohos.i18n (国际化-I18n)

 本模块提供系统相关的或者增强的国际化能力，包括区域管理、电话号码处理、日历等，相关接口为ECMA 402标准中未定义的补充接口。
[Intl模块](js-apis-intl.md)提供了ECMA 402标准定义的基础国际化接口，与本模块共同使用可提供完整地国际化支持能力。

>  **说明：**
>  - 本模块首批接口从API version 7开始支持。后续版本的新增接口，采用上角标单独标记接口的起始版本。
>
>  - I18N模块包含国际化能力增强接口（未在ECMA 402中定义），包括区域管理、电话号码处理、日历等，国际化基础能力请参考[Intl模块](js-apis-intl.md)。
>
>  - 在本项目SDK中，国际化数据暂时仅支持简体中文、香港繁体、台湾繁体、美式英语、藏语、维吾尔语6种语言，其他语言支持待后续版本根据需要补充；使用非支持的语言，可能会回溯到英文特性或者返回空值。


## 导入模块

```ts
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
| sentenceCase | boolean | 否    | 本地化显示文本是否要首字母大写。默认值：true。 |

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
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
      let displayCountry: string = I18n.System.getDisplayCountry("zh-CN", "en-GB"); // displayCountry = "China"
  } catch (error) {
      let err: BusinessError = error as BusinessError;
      console.error(`call System.getDisplayCountry failed, error code: ${err.code}, message: ${err.message}.`);
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
| sentenceCase | boolean | 否    | 本地化显示文本是否要首字母大写。默认值：true。 |

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
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
    let displayLanguage: string = I18n.System.getDisplayLanguage("zh", "en-GB"); // displayLanguage = Chinese
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call System.getDisplayLanguage failed, error code: ${err.code}, message: ${err.message}.`);
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

**示例：**
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
    let systemLanguage: string = I18n.System.getSystemLanguage();  // systemLanguage为当前系统语言
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call System.getSystemLanguage failed, error code: ${err.code}, message: ${err.message}.`);
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

**示例：**
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
    let systemRegion: string = I18n.System.getSystemRegion(); // 获取系统当前地区设置
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call System.getSystemRegion failed, error code: ${err.code}, message: ${err.message}.`);
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

**示例：**
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
    let systemLocale: string = I18n.System.getSystemLocale();  // 获取系统当前Locale
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call System.getSystemLocale failed, error code: ${err.code}, message: ${err.message}.`);
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

**示例：**
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
    let is24HourClock: boolean = I18n.System.is24HourClock();  // 系统24小时开关是否开启
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call System.is24HourClock failed, error code: ${err.code}, message: ${err.message}.`);
  }
  ```


### setAppPreferredLanguage<sup>20+</sup>

static setAppPreferredLanguage(language: string): void

设置应用偏好语言。
设置后，应用将优先加载应用偏好语言对应的资源。
安卓的原生页面需要应用重启才能生效，ios的原生页面需要开发者实现资源映射逻辑。
ArkTs实现的页面需要退回原生页面后重新进入即可生效。
设置偏好语言为'default'后，应用语言将跟随系统语言，需要应用重启生效。

**系统能力：** SystemCapability.Global.I18n

**参数：**

| 参数名      | 类型     | 必填   | 说明    |
| -------- | ------ | ---- | ----- |
| language | string | 是    | 合法的语言ID或'default'。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid. |

**示例：**
  ```ts
  import { BusinessError } from '@kit.BasicServicesKit';

  try {
    i18n.System.setAppPreferredLanguage('zh');
  } catch (error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call System.setAppPreferredLanguage failed, error code: ${err.code}, message: ${err.message}.`);
  }
  ```

**说明：**

在安卓中可以通过在Activity中实现I18NSetAppPreferredLanguage并调用I18NPlugin.setRestartFunc，这样在调用i18n.System.setAppPreferredLanguage接口时可以重启应用：
```java
import ohos.ace.plugin.i18nplugin.I18NPlugin;
import ohos.ace.plugin.i18nplugin.I18NSetAppPreferredLanguage;

public class EntryEntryAbilityActivity extends StageActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
      I18NPlugin.setRestartFunc(new I18NSetAppPreferredLanguage() {
        @Override
        public void restartApp() {
          // 获取应用的启动Intent
          Context context = getApplicationContext();
          PackageManager packageManager = context.getPackageManager();
          Intent launchIntent = packageManager.getLaunchIntentForPackage(context.getPackageName());
          if (launchIntent != null) {
            // 添加标志以清除任务栈并创建新任务
            launchIntent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP);
            launchIntent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            launchIntent.addFlags(Intent.FLAG_ACTIVITY_CLEAR_TASK);
            // 启动主Activity
            startActivity(launchIntent);
            // 结束当前Activity
            finish();
            // 确保进程完全重启（可选，根据需求）
            System.exit(0);
          }
        }
      });
    }
}
```

### getAppPreferredLanguage<sup>9+</sup>

static getAppPreferredLanguage(): string

获取应用偏好语言。

**系统能力：** SystemCapability.Global.I18n

**返回值：**

| 类型     | 说明       |
| ------ | -------- |
| string | 应用偏好语言。 |

**示例：**
  ```ts
  let appPreferredLanguage: string = i18n.System.getAppPreferredLanguage();
  ```

## I18n.isRTL<sup>7+</sup>

isRTL(locale: string): boolean

获取该区域是否为从右至左显示语言。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名    | 类型     | 必填   | 说明      |
| ------ | ------ | ---- | ------- |
| locale | string | 是    | 指定区域ID。 |

**返回值：**

| 类型      | 说明                                       |
| ------- | ---------------------------------------- |
| boolean | true表示该locale从右至左显示语言；false表示该locale从左至右显示语言。 |

**示例：**
  ```ts
  I18n.isRTL("zh-CN");// 中文不是RTL语言，返回false
  I18n.isRTL("ar-EG");// 阿语是RTL语言，返回true
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("en-US", "gregory");
  let date: Date = new Date(2021, 10, 7, 8, 0, 0, 0);
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("en-US", "gregory");
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
| hour   | number | 否    | 设置的小时。默认值：系统小时。 |
| minute | number | 否    | 设置的分钟。默认值：系统分钟。 |
| second | number | 否    | 设置的秒。默认值：系统秒。 |

**示例：**
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
  calendar.setTimeZone("Asia/Shanghai");
  let timezone: string = calendar.getTimeZone(); // timezone = "Asia/Shanghai"
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("en-US", "gregory");
  let firstDayOfWeek: number = calendar.getFirstDayOfWeek(); // firstDayOfWeek = 1
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
  calendar.setFirstDayOfWeek(3);
  let firstDayOfWeek: number = calendar.getFirstDayOfWeek(); // firstDayOfWeek = 3
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
  let minimalDaysInFirstWeek: number = calendar.getMinimalDaysInFirstWeek(); // minimalDaysInFirstWeek = 1
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
  calendar.setMinimalDaysInFirstWeek(3);
  let minimalDaysInFirstWeek: number = calendar.getMinimalDaysInFirstWeek(); // minimalDaysInFirstWeek = 3
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
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 10, 1, 8, 0, 0); // set time to 2021.10.1 08:00:00
  let hourOfDay: number = calendar.get("hour_of_day"); // hourOfDay = 8
  ```


### isWeekend<sup>8+</sup>

isWeekend(date?: Date): boolean

判断指定的日期在日历中是否为周末。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名  | 类型   | 必填   | 说明                                       |
| ---- | ---- | ---- | ---------------------------------------- |
| date | Date | 否    | 指定的日期。若不填，则判断当前日期是否为周末。默认值：系统日期。 |

**返回值：**

| 类型      | 说明                                  |
| ------- | ----------------------------------- |
| boolean | 若判断指定日期为周末时，返回true，否则返回false。 |

**示例：**
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 11, 11, 8, 0, 0); // set time to 2021.11.11 08:00:00
  calendar.isWeekend(); // false
  let date: Date = new Date(2011, 11, 6, 9, 0, 0);
  calendar.isWeekend(date); // true
  ```


### add<sup>11+</sup>

add(field: string, amount: number): void

在日历的给定字段进行加减操作。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名  | 类型   | 必填   | 说明                                       |
| ---- | ---- | ---- | ---------------------------------------- |
| field | string | 是    | 指定进行操作的日历字段，目前支持的field值有&nbsp;year,&nbsp;month,&nbsp;week_of_year,&nbsp;week_of_month,&nbsp;date,&nbsp;day_of_year,&nbsp;day_of_week,&nbsp;day_of_week_in_month,&nbsp;hour,&nbsp;hour_of_day,&nbsp;minute,&nbsp;second,&nbsp;millisecond。 |
| amount | number | 是    | 进行加减操作的具体数值。 |

**错误码：**

以下错误码的详细介绍请参见[ohos.i18n错误码](../errorcodes/errorcode-i18n.md)。

| 错误码ID  | 错误信息                   |
| ------ | ---------------------- |
| 890001 | param value not valid |

**示例：**
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
    let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
    calendar.set(2021, 11, 11, 8, 0, 0); // set time to 2021.11.11 08:00:00
    calendar.add("year", 8); // 2021 + 8
    let year: number = calendar.get("year"); // year = 2029
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call Calendar.add failed, error code: ${err.code}, message: ${err.message}.`);
  }
  ```


### getTimeInMillis<sup>11+</sup>

getTimeInMillis(): number

获取当前日历的UTC毫秒数。

**系统能力**：SystemCapability.Global.I18n

**返回值：**

| 类型      | 说明                                  |
| ------- | ----------------------------------- |
| number | 当前日历的UTC毫秒数。 |

**示例：**
  ```ts
  let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
  calendar.setTime(5000);
  let millisecond: number = calendar.getTimeInMillis(); // millisecond = 5000
  ```


### compareDays<sup>11+</sup>

compareDays(date: Date): number

比较日历和指定日期相差的天数（按毫秒级的精度，不足一天将按一天进行计算）。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名  | 类型   | 必填   | 说明                                       |
| ---- | ---- | ---- | ---------------------------------------- |
| date | Date | 是    | 指定的日期。 |

**返回值：**

| 类型      | 说明                                  |
| ------- | ----------------------------------- |
| number | 相差的天数，正数代表日历时间更早，负数代表日历时间更晚。 |

**示例：**
  ```ts
  import { BusinessError } from '@ohos.base';

  try {
    let calendar: I18n.Calendar = I18n.getCalendar("zh-Hans");
    calendar.setTime(5000);
    let date: Date = new Date(6000);
    let diff: number = calendar.compareDays(date); // diff = 1
  } catch(error) {
    let err: BusinessError = error as BusinessError;
    console.error(`call Calendar.compareDays failed, error code: ${err.code}, message: ${err.message}.`);
  }
  ```


## PhoneNumberFormat<sup>11+</sup>


### constructor<sup>11+</sup>

constructor(country: string, options?: PhoneNumberFormatOptions)

创建电话号码格式化对象。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名     | 类型                                       | 必填   | 说明               |
| ------- | ---------------------------------------- | ---- | ---------------- |
| country | string                                   | 是    | 表示电话号码所属国家或地区代码。 |
| options | [PhoneNumberFormatOptions](#phonenumberformatoptions11) | 否    | 电话号码格式化对象的相关选项。默认值：NATIONAL。  |

**示例：**
  ```ts
  let option: I18n.PhoneNumberFormatOptions = {type: "E164"};
  let phoneNumberFormat: I18n.PhoneNumberFormat = new I18n.PhoneNumberFormat("CN", option);
  ```


### isValidNumber<sup>11+</sup>

isValidNumber(number: string): boolean

判断传入的电话号码格式是否正确。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名    | 类型     | 必填   | 说明        |
| ------ | ------ | ---- | --------- |
| number | string | 是    | 待判断的电话号码。 |

**返回值：**

| 类型      | 说明                                    |
| ------- | ------------------------------------- |
| boolean | 返回true表示电话号码的格式正确，返回false表示电话号码的格式错误。 |

**示例：**
  ```ts
  let phonenumberfmt: I18n.PhoneNumberFormat = new I18n.PhoneNumberFormat("CN");
  let isValidNumber: boolean = phonenumberfmt.isValidNumber("158****2312"); // isValidNumber = true
  ```


### format<sup>11+</sup>

format(number: string): string

对电话号码进行格式化。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名    | 类型     | 必填   | 说明         |
| ------ | ------ | ---- | ---------- |
| number | string | 是    | 待格式化的电话号码。 |

**返回值：**

| 类型     | 说明         |
| ------ | ---------- |
| string | 格式化后的电话号码。 |

**示例：**
  ```ts
  let phonenumberfmt: I18n.PhoneNumberFormat = new I18n.PhoneNumberFormat("CN");
  let formattedPhoneNumber: string = phonenumberfmt.format("158****2312"); // formattedPhoneNumber = "158 **** 2312"
  ```


## PhoneNumberFormatOptions<sup>11+</sup>

表示电话号码格式化对象可设置的属性。

**系统能力**：SystemCapability.Global.I18n

| 名称   | 类型     | 可读   | 可写   | 说明                                       |
| ---- | ------ | ---- | ---- | ---------------------------------------- |
| type | string | 是    | 是    | 表示对电话号码格式化的类型，取值范围："E164",&nbsp;"INTERNATIONAL",&nbsp;"NATIONAL",&nbsp;"RFC3966"。<br>-在API version 8版本，type为必填项。 <br>-API version 9版本开始，type为选填项。|


## I18n.getTimeZone<sup>7+</sup>

getTimeZone(zoneID?: string): TimeZone

获取时区ID对应的时区对象。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名    | 类型     | 必填   | 说明    |
| ------ | ------ | ---- | ----- |
| zondID | string | 否    | 时区ID。默认值：系统时区。 |

**返回值：**

| 类型       | 说明           |
| -------- | ------------ |
| [TimeZone](#timezone) | 时区ID对应的时区对象。 |

**示例：**
  ```ts
  let timezone: I18n.TimeZone = I18n.getTimeZone();
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
  ```ts
  let timezone: I18n.TimeZone = I18n.getTimeZone();
  let timezoneID: string = timezone.getID(); // timezoneID = "Asia/Shanghai"
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
  ```ts
  let timezone: I18n.TimeZone = I18n.getTimeZone();
  let offset: number = timezone.getRawOffset(); // offset = 28800000
  ```


### getOffset

getOffset(date?: number): number

获取某一时刻时区对象表示的时区与UTC时区的偏差。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名    | 类型     | 必填   | 说明     |
| ------ | ------ | ---- | ------ |
| date | number | 否    | 待计算偏差的时刻 |

**返回值：**

| 类型     | 说明                      |
| ------ | ----------------------- |
| number | 某一时刻时区对象表示的时区与UTC时区的偏差。默认值：系统时间。 |

**示例：**
  ```ts
  let timezone: I18n.TimeZone = I18n.getTimeZone();
  let offset: number = timezone.getOffset(1234567890); // offset = 28800000
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
  let ids: Array<string> = I18n.TimeZone.getAvailableIDs();
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
  ```ts
  let isdigit: boolean = I18n.Unicode.isDigit("1");  // isdigit = true
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
  ```ts
  let isspacechar: boolean = I18n.Unicode.isSpaceChar("a");  // isspacechar = false
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
  ```ts
  let iswhitespace: boolean = I18n.Unicode.isWhitespace("a");  // iswhitespace = false
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
  ```ts
  let isrtl: boolean = I18n.Unicode.isRTL("a");  // isrtl = false
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
  ```ts
  let isideograph: boolean = I18n.Unicode.isIdeograph("a");  // isideograph = false
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
  ```ts
  let isletter: boolean = I18n.Unicode.isLetter("a");  // isletter = true
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
  ```ts
  let islowercase: boolean = I18n.Unicode.isLowerCase("a");  // islowercase = true
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
  ```ts
  let isuppercase: boolean = I18n.Unicode.isUpperCase("a");  // isuppercase = false
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
| string | 输入字符的一般类别值。|

一般类别值如下，更详细的介绍可以参考Unicode标准。

| 名称 | 值 | 说明 |
| ---- | -------- | ---------- |
| U_UNASSIGNED | U_UNASSIGNED | 表示未分配和非字符代码点对应类别。 |
| U_GENERAL_OTHER_TYPES | U_GENERAL_OTHER_TYPES | 与 U_UNASSIGNED 相同。 |
| U_UPPERCASE_LETTER | U_UPPERCASE_LETTER | 表示大写字母。 |
| U_LOWERCASE_LETTER | U_LOWERCASE_LETTER | 表示小写字母。  |
| U_TITLECASE_LETTER | U_TITLECASE_LETTER | 表示首字母大写。 |
| U_MODIFIER_LETTER | U_MODIFIER_LETTER | 表示字母修饰符。 |
| U_OTHER_LETTER | U_OTHER_LETTER | 表示其它字母，不属于大写字母、小写字母、首字母大写或修饰符字母的字母。 |
| U_NON_SPACING_MARK | U_NON_SPACING_MARK | 表示非间距标记，如重音符号'，变音符号#。 |
| U_ENCLOSING_MARK | U_ENCLOSING_MARK | 表示封闭标记和能围住其它字符的标记，如圆圈、方框等。 |
| U_COMBINING_SPACING_MARK | U_COMBINING_SPACING_MARK | 表示间距标记，如元音符号[ ]。 |
| U_DECIMAL_DIGIT_NUMBER | U_DECIMAL_DIGIT_NUMBER | 表示十进制数字。 |
| U_LETTER_NUMBER | U_LETTER_NUMBER | 表示字母数字，罗马数字。 |
| U_OTHER_NUMBER | U_OTHER_NUMBER | 表示其它作为加密符号和记号的数字，非阿拉伯数字的数字表示符，如@、#、（1）、①等。 |
| U_SPACE_SEPARATOR | U_SPACE_SEPARATOR | 表示空白分隔符，如空格符、不间断空格、固定宽度的空白符。 |
| U_LINE_SEPARATOR | U_LINE_SEPARATOR | 表示行分隔符。|
| U_PARAGRAPH_SEPARATOR | U_PARAGRAPH_SEPARATOR | 表示段落分割符。 |
| U_CONTROL_CHAR | U_CONTROL_CHAR | 表示控制字符。 |
| U_FORMAT_CHAR | U_FORMAT_CHAR | 表示格式字符。 |
| U_PRIVATE_USE_CHAR | U_PRIVATE_USE_CHAR | 表示私人使用区代码点类别，例如公司 logo。 |
| U_SURROGATE | U_SURROGATE | 表示代理项，在UTF-16中用来表示补充字符的方法。 |
| U_DASH_PUNCTUATION | U_DASH_PUNCTUATION | 表示短划线标点。 |
| U_START_PUNCTUATION | U_START_PUNCTUATION | 表示开始标点，如左括号。 |
| U_END_PUNCTUATION | U_END_PUNCTUATION | 表示结束标点，如右括号。 |
| U_INITIAL_PUNCTUATION | U_INITIAL_PUNCTUATION | 表示前引号，如左双引号、左单引号。 |
| U_FINAL_PUNCTUATION | U_FINAL_PUNCTUATION | 表示后引号，如右双引号、右单引号。 |
| U_CONNECTOR_PUNCTUATION | U_CONNECTOR_PUNCTUATION | 表示连接符标点。 |
| U_OTHER_PUNCTUATION | U_OTHER_PUNCTUATION | 表示其他标点。 |
| U_MATH_SYMBOL | U_MATH_SYMBOL | 表示数学符号。 |
| U_CURRENCY_SYMBOL | U_CURRENCY_SYMBOL | 表示货币符号。 |
| U_MODIFIER_SYMBOL | U_MODIFIER_SYMBOL | 表示修饰符号。 |
| U_OTHER_SYMBOL | U_OTHER_SYMBOL | 表示其它符号。 |

**示例：**
  ```ts
  let type: string = I18n.Unicode.getType("a"); // type = "U_LOWERCASE_LETTER"
  ```


## I18NUtil<sup>9+</sup>

### getDateOrder<sup>9+</sup>

static getDateOrder(locale: string): string

获取该区域日期中年、月、日的排列顺序。

**系统能力**：SystemCapability.Global.I18n

**参数：**

| 参数名    | 类型     | 必填   | 说明                        |
| ------ | ------ | ---- | ------------------------- |
| locale | string | 是    | 格式化时使用的区域参数，如：zh-Hans-CN。 |

**返回值：**

| 类型     | 说明                  |
| ------ | ------------------- |
| string | 返回该区域年、月、日的排列顺序。 |

**示例：**
  ```ts
  let order: string = I18n.I18NUtil.getDateOrder("zh-CN");  // order = "y-L-d"
  ```
