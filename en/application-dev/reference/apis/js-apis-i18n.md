# @ohos.i18n (Internationalization)

 This module provides system-related or enhanced I18N capabilities, such as locale management, phone number formatting, and calendar, through supplementary I18N APIs that are not defined in ECMA 402.
The [Intl](js-apis-intl.md) module provides basic I18N capabilities through the standard I18N APIs defined in ECMA 402. It works with the I18N module to provide a complete suite of I18N capabilities.

>  **NOTE**
>  - The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>
>  - This module provides system-related or enhanced I18N capabilities, such as locale management, phone number formatting, and calendar, through supplementary I18N APIs that are not defined in ECMA 402. For details about the basic I18N capabilities, see [Intl](js-apis-intl.md).


## Modules to Import

```js
import I18n from '@ohos.i18n';
```


## System<sup>9+</sup>

### getDisplayCountry<sup>9+</sup>

static getDisplayCountry(country: string, locale: string, sentenceCase?: boolean): string

Obtains the localized script for the specified country.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name         | Type     | Mandatory  | Description              |
| ------------ | ------- | ---- | ---------------- |
| country      | string  | Yes   | Specified country.           |
| locale       | string  | Yes   | Locale ID.    |
| sentenceCase | boolean | No   | Whether to use sentence case for the localized script. The default value is **true**.|

**Return value**

| Type    | Description           |
| ------ | ------------- |
| string | Localized script for the specified country.|

**Error codes**

For details about the error codes, see [I18N Error Codes](../errorcodes/errorcode-i18n.md).

| ID | Error Message                  |
| ------ | ---------------------- |
| 890001 | param value not valid |

**Example**
  ```js
  try {
    let displayCountry = I18n.System.getDisplayCountry("zh-CN", "en-GB"); // displayCountry = "China"
  } catch(error) {
    console.error(`call System.getDisplayCountry failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getDisplayLanguage<sup>9+</sup>

static getDisplayLanguage(language: string, locale: string, sentenceCase?: boolean): string

Obtains the localized script for the specified language.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name         | Type     | Mandatory  | Description              |
| ------------ | ------- | ---- | ---------------- |
| language     | string  | Yes   | Specified language.           |
| locale       | string  | Yes   | Locale ID.    |
| sentenceCase | boolean | No   | Whether to use sentence case for the localized script. The default value is **true**.|

**Return value**

| Type    | Description           |
| ------ | ------------- |
| string | Localized script for the specified language.|

**Error codes**

For details about the error codes, see [I18N Error Codes](../errorcodes/errorcode-i18n.md).

| ID | Error Message                  |
| ------ | ---------------------- |
| 890001 | param value not valid |

**Example**
  ```js
  try {
    let displayLanguage = I18n.System.getDisplayLanguage("zh", "en-GB"); // displayLanguage = Chinese
  } catch(error) {
    console.error(`call System.getDisplayLanguage failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getSystemLanguage<sup>9+</sup>

static getSystemLanguage(): string

Obtains the system language.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description     |
| ------ | ------- |
| string | System language ID.|

**Error codes**

For details about the error codes, see [I18N Error Codes](../errorcodes/errorcode-i18n.md).

| ID | Error Message                  |
| ------ | ---------------------- |
| 890001 | param value not valid |

**Example**
  ```js
  try {
    let systemLanguage = I18n.System.getSystemLanguage(); // systemLanguage indicates the current system language.
  } catch(error) {
    console.error(`call System.getSystemLanguage failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getSystemRegion<sup>9+</sup>

static getSystemRegion(): string

Obtains the system region.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description     |
| ------ | ------- |
| string | System region ID.|

**Error codes**

For details about the error codes, see [I18N Error Codes](../errorcodes/errorcode-i18n.md).

| ID | Error Message                  |
| ------ | ---------------------- |
| 890001 | param value not valid |

**Example**
  ```js
  try {
    let systemRegion = I18n.System.getSystemRegion(); // Obtain the current system region.
  } catch(error) {
    console.error(`call System.getSystemRegion failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### getSystemLocale<sup>9+</sup>

static getSystemLocale(): string

Obtains the system locale.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description     |
| ------ | ------- |
| string | System locale ID.|

**Error codes**

For details about the error codes, see [I18N Error Codes](../errorcodes/errorcode-i18n.md).

| ID | Error Message                  |
| ------ | ---------------------- |
| 890001 | param value not valid |

**Example**
  ```js
  try {
    let systemLocale = I18n.System.getSystemLocale(); // Obtain the current system locale.
  } catch(error) {
    console.error(`call System.getSystemLocale failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

### is24HourClock<sup>9+</sup>

static is24HourClock(): boolean

Checks whether the 24-hour clock is used.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type     | Description                                      |
| ------- | ---------------------------------------- |
| boolean | Returns **true** if the 24-hour clock is used; returns **false** otherwise.|

**Error codes**

For details about the error codes, see [I18N Error Codes](../errorcodes/errorcode-i18n.md).

| ID | Error Message                  |
| ------ | ---------------------- |
| 890001 | param value not valid |

**Example**
  ```js
  try {
    let is24HourClock = I18n.System.is24HourClock(); // Check whether the 24-hour clock is enabled.
  } catch(error) {
    console.error(`call System.is24HourClock failed, error code: ${error.code}, message: ${error.message}.`);
  }
  ```

## I18n.isRTL<sup>7+</sup>

isRTL(locale: string): boolean

Checks whether a locale uses an RTL language.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name   | Type    | Mandatory  | Description     |
| ------ | ------ | ---- | ------- |
| locale | string | Yes   | Locale ID.|

**Return value**

| Type     | Description                                      |
| ------- | ---------------------------------------- |
| boolean | Returns **true** if the localized script is displayed from right to left; returns **false** otherwise.|

**Example**
  ```js
  i18n.isRTL("zh-CN");// Since Chinese is not written from right to left, false is returned.
  i18n.isRTL("ar-EG");// Since Arabic is written from right to left, true is returned.
  ```


## I18n.getCalendar<sup>8+</sup>

getCalendar(locale: string, type? : string): Calendar

Obtains a **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name   | Type    | Mandatory  | Description                                      |
| ------ | ------ | ---- | ---------------------------------------- |
| locale | string | Yes   | Valid locale value, for example, **zh-Hans-CN**.                |
| type   | string | No   | Valid calendar type. Currently, the valid types are as follows: **buddhist**, **chinese**, **coptic**, **ethiopic**, **hebrew**, **gregory**, **indian**, **islamic\_civil**, **islamic\_tbla**, **islamic\_umalqura**, **japanese**, and **persian**. The default value is the default calendar type of the locale.|

**Return value**

| Type                    | Description   |
| ---------------------- | ----- |
| [Calendar](#calendar8) | **Calendar** object.|

**Example**
  ```js
  I18n.getCalendar("zh-Hans", "chinese"); // Obtain the Calendar object for the Chinese lunar calendar.
  ```


## Calendar<sup>8+</sup>


### setTime<sup>8+</sup>

setTime(date: Date): void

Sets the date for this **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type  | Mandatory  | Description               |
| ---- | ---- | ---- | ----------------- |
| date | Date | Yes   | Date to be set for the **Calendar** object.|

**Example**
  ```js
  let calendar = I18n.getCalendar("en-US", "gregory");
  let date = new Date(2021, 10, 7, 8, 0, 0, 0);
  calendar.setTime(date);
  ```


### setTime<sup>8+</sup>

setTime(time: number): void

Sets the date and time for this **Calendar** object. The value is represented by the number of milliseconds that have elapsed since the Unix epoch (00:00:00 UTC on January 1, 1970).

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description                                      |
| ---- | ------ | ---- | ---------------------------------------- |
| time | number | Yes   | Number of milliseconds that have elapsed since the Unix epoch.|

**Example**
  ```js
  let calendar = I18n.getCalendar("en-US", "gregory");
  calendar.setTime(10540800000);
  ```


### set<sup>8+</sup>

set(year: number, month: number, date:number, hour?: number, minute?: number, second?: number): void

Sets the year, month, day, hour, minute, and second for this **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name   | Type    | Mandatory  | Description    |
| ------ | ------ | ---- | ------ |
| year   | number | Yes   | Year to set. |
| month  | number | Yes   | Month to set. |
| date   | number | Yes   | Day to set. |
| hour   | number | No   | Hour to set. The default value is the system hour.|
| minute | number | No   | Minute to set. The default value is the system minute.|
| second | number | No   | Second to set. The default value is the system second.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 10, 1, 8, 0, 0); // set time to 2021.10.1 08:00:00
  ```


### setTimeZone<sup>8+</sup>

setTimeZone(timezone: string): void

Sets the time zone of this **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name     | Type    | Mandatory  | Description                       |
| -------- | ------ | ---- | ------------------------- |
| timezone | string | Yes   | Time zone, for example, **Asia/Shanghai**.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setTimeZone("Asia/Shanghai");
  ```


### getTimeZone<sup>8+</sup>

getTimeZone(): string

Obtains the time zone of this **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description        |
| ------ | ---------- |
| string | Time zone of the **Calendar** object.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setTimeZone("Asia/Shanghai");
  let timezone = calendar.getTimeZone(); // timezone = "Asia/Shanghai"
  ```


### getFirstDayOfWeek<sup>8+</sup>

getFirstDayOfWeek(): number

Obtains the start day of a week for this **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description                   |
| ------ | --------------------- |
| number | Start day of a week. The value **1** indicates Sunday, and the value **7** indicates Saturday.|

**Example**
  ```js
  let calendar = I18n.getCalendar("en-US", "gregory");
  let firstDayOfWeek = calendar.getFirstDayOfWeek(); // firstDayOfWeek = 1
  ```


### setFirstDayOfWeek<sup>8+</sup>

setFirstDayOfWeek(value: number): void

Sets the start day of a week for this **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name  | Type    | Mandatory  | Description                   |
| ----- | ------ | ---- | --------------------- |
| value | number | Yes   | Start day of a week. The value **1** indicates Sunday, and the value **7** indicates Saturday.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setFirstDayOfWeek(3);
  let firstDayOfWeek = calendar.getFirstDayOfWeek(); // firstDayOfWeek = 3
  ```


### getMinimalDaysInFirstWeek<sup>8+</sup>

getMinimalDaysInFirstWeek(): number

Obtains the minimum number of days in the first week of a year.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description          |
| ------ | ------------ |
| number | Minimum number of days in the first week of a year.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  let minimalDaysInFirstWeek = calendar.getMinimalDaysInFirstWeek(); // minimalDaysInFirstWeek = 1
  ```


### setMinimalDaysInFirstWeek<sup>8+</sup>

setMinimalDaysInFirstWeek(value: number): void

Sets the minimum number of days in the first week of a year.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name  | Type    | Mandatory  | Description          |
| ----- | ------ | ---- | ------------ |
| value | number | Yes   | Minimum number of days in the first week of a year.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.setMinimalDaysInFirstWeek(3);
  let minimalDaysInFirstWeek = calendar.getMinimalDaysInFirstWeek(); // minimalDaysInFirstWeek = 3
  ```


### get<sup>8+</sup>

get(field: string): number

Obtains the value of the specified field in the **Calendar** object.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name  | Type    | Mandatory  | Description                                      |
| ----- | ------ | ---- | ---------------------------------------- |
| field | string | Yes   | Value of the specified field in the **Calendar** object. Currently, a valid field can be any of the following: **era**, **year**, **month**, **week\_of\_year**, **week\_of\_month**, **date**, **day\_of\_year**, **day\_of\_week**, **day\_of\_week\_in\_month**, **hour**, **hour\_of\_day**, **minute**, **second**, **millisecond**, **zone\_offset**, **dst\_offset**, **year\_woy**, **dow\_local**, **extended\_year**, **julian\_day**, **milliseconds\_in\_day**, **is\_leap\_month**.|

**Return value**

| Type    | Description                                      |
| ------ | ---------------------------------------- |
| number | Value of the specified field. For example, if the year in the internal date of this **Calendar** object is **1990**, the **get("year")** function will return **1990**.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 10, 1, 8, 0, 0); // set time to 2021.10.1 08:00:00
  let hourOfDay = calendar.get("hour_of_day"); // hourOfDay = 8
  ```

### isWeekend<sup>8+</sup>

isWeekend(date?: Date): boolean

Checks whether a given date is a weekend in the calendar.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type  | Mandatory  | Description                                      |
| ---- | ---- | ---- | ---------------------------------------- |
| date | Date | No   | Specified date. If this parameter is left empty, the system checks whether the current date is a weekend. The default value is the system date.|

**Return value**

| Type     | Description                                 |
| ------- | ----------------------------------- |
| boolean | Returns **true** if the specified date is a weekend; returns **false** otherwise.|

**Example**
  ```js
  let calendar = I18n.getCalendar("zh-Hans");
  calendar.set(2021, 11, 11, 8, 0, 0); // set time to 2021.11.11 08:00:00
  calendar.isWeekend(); // false
  let date = new Date(2011, 11, 6, 9, 0, 0);
  calendar.isWeekend(date); // true
  ```

## I18n.getTimeZone<sup>7+</sup>

getTimeZone(zoneID?: string): TimeZone

Obtains the **TimeZone** object corresponding to the specified time zone ID.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name   | Type    | Mandatory  | Description   |
| ------ | ------ | ---- | ----- |
| zondID | string | No   | Time zone ID. The default value is the system time zone.|

**Return value**

| Type      | Description          |
| -------- | ------------ |
| TimeZone | **TimeZone** object corresponding to the time zone ID.|

**Example**
  ```js
  let timezone = I18n.getTimeZone();
  ```


## TimeZone


### getID

getID(): string

Obtains the ID of the specified **TimeZone** object.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description          |
| ------ | ------------ |
| string | Time zone ID corresponding to the **TimeZone** object.|

**Example**
  ```js
  let timezone = I18n.getTimeZone();
  let timezoneID = timezone.getID(); // timezoneID = "Asia/Shanghai"
  ```

### getRawOffset

getRawOffset(): number

Obtains the offset between the time zone represented by a **TimeZone** object and the UTC time zone.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description                 |
| ------ | ------------------- |
| number | Offset between the time zone represented by the **TimeZone** object and the UTC time zone.|

**Example**
  ```js
  let timezone = I18n.getTimeZone();
  let offset = timezone.getRawOffset(); // offset = 28800000
  ```


### getOffset

getOffset(date?: number): number

Obtains the offset between the time zone represented by a **TimeZone** object and the UTC time zone at a certain time point.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type    | Description                     |
| ------ | ----------------------- |
| number | Offset between the time zone represented by the **TimeZone** object and the UTC time zone at a certain time point. The default value is the system time zone.|

**Example**
  ```js
  let timezone = I18n.getTimeZone();
  let offset = timezone.getOffset(1234567890); // offset = 28800000
  ```


### getAvailableIDs<sup>9+</sup>

static getAvailableIDs(): Array&lt;string&gt;

Obtains the list of time zone IDs supported by the system.

**System capability**: SystemCapability.Global.I18n

**Return value**

| Type                 | Description         |
| ------------------- | ----------- |
| Array&lt;string&gt; | List of time zone IDs supported by the system.|

**Example**
  ```ts
  // ids = ["America/Adak", "America/Anchorage", "America/Bogota", "America/Denver", "America/Los_Angeles", "America/Montevideo", "America/Santiago", "America/Sao_Paulo", "Asia/Ashgabat", "Asia/Hovd", "Asia/Jerusalem", "Asia/Magadan", "Asia/Omsk", "Asia/Shanghai", "Asia/Tokyo", "Asia/Yerevan", "Atlantic/Cape_Verde", "Australia/Lord_Howe", "Europe/Dublin", "Europe/London", "Europe/Moscow", "Pacific/Auckland", "Pacific/Easter", "Pacific/Pago-Pago"], 24 time zones supported in total
  let ids = I18n.TimeZone.getAvailableIDs();
  ```

## Unicode<sup>9+</sup>


### isDigit<sup>9+</sup>

static isDigit(char: string): boolean

Checks whether the input character string is composed of digits.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                  |
| ------- | ------------------------------------ |
| boolean | Returns **true** if the input character is a digit; returns **false** otherwise.|

**Example**
  ```js
  let isdigit = I18n.Unicode.isDigit("1");  // isdigit = true
  ```


### isSpaceChar<sup>9+</sup>

static isSpaceChar(char: string): boolean

Checks whether the input character is a space.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                    |
| ------- | -------------------------------------- |
| boolean | Returns **true** if the input character is a space; returns **false** otherwise.|

**Example**
  ```js
  let isspacechar = I18n.Unicode.isSpaceChar("a");  // isspacechar = false
  ```


### isWhitespace<sup>9+</sup>

static isWhitespace(char: string): boolean

Checks whether the input character is a white space.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                    |
| ------- | -------------------------------------- |
| boolean | Returns **true** if the input character is a white space; returns **false** otherwise.|

**Example**
  ```js
  let iswhitespace = I18n.Unicode.isWhitespace("a");  // iswhitespace = false
  ```


### isRTL<sup>9+</sup>

static isRTL(char: string): boolean

Checks whether the input character is of the right to left (RTL) language.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                      |
| ------- | ---------------------------------------- |
| boolean | Returns **true** if the input character is of the RTL language; returns **false** otherwise.|

**Example**
  ```js
  let isrtl = I18n.Unicode.isRTL("a");  // isrtl = false
  ```


### isIdeograph<sup>9+</sup>

static isIdeograph(char: string): boolean

Checks whether the input character is an ideographic character.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                      |
| ------- | ---------------------------------------- |
| boolean | Returns **true** if the input character is an ideographic character; returns **false** otherwise.|

**Example**
  ```js
  let isideograph = I18n.Unicode.isIdeograph("a");  // isideograph = false
  ```


### isLetter<sup>9+</sup>

static isLetter(char: string): boolean

Checks whether the input character is a letter.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                  |
| ------- | ------------------------------------ |
| boolean | Returns **true** if the input character is a letter; returns **false** otherwise.|

**Example**
  ```js
  let isletter = I18n.Unicode.isLetter("a");  // isletter = true
  ```


### isLowerCase<sup>9+</sup>

static isLowerCase(char: string): boolean

Checks whether the input character is a lowercase letter.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                      |
| ------- | ---------------------------------------- |
| boolean | Returns **true** if the input character is a lowercase letter; returns **false** otherwise.|

**Example**
  ```js
  let islowercase = I18n.Unicode.isLowerCase("a");  // islowercase = true
  ```


### isUpperCase<sup>9+</sup>

static isUpperCase(char: string): boolean

Checks whether the input character is an uppercase letter.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type     | Description                                      |
| ------- | ---------------------------------------- |
| boolean | Returns **true** if the input character is an uppercase letter; returns **false** otherwise.|

**Example**
  ```js
  let isuppercase = I18n.Unicode.isUpperCase("a");  // isuppercase = false
  ```


### getType<sup>9+</sup>

static getType(char: string): string

Obtains the type of the input character string.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name | Type    | Mandatory  | Description   |
| ---- | ------ | ---- | ----- |
| char | string | Yes   | Input character.|

**Return value**

| Type    | Description         |
| ------ | ----------- |
| string | Type of the input character.|

**Example**
  ```js
  let type = I18n.Unicode.getType("a"); // type = "U_LOWERCASE_LETTER"
  ```


## I18NUtil<sup>9+</sup>


### getDateOrder<sup>9+</sup>

static getDateOrder(locale: string): string

Obtains the sequence of the year, month, and day in the specified locale.

**System capability**: SystemCapability.Global.I18n

**Parameters**

| Name   | Type    | Mandatory  | Description                       |
| ------ | ------ | ---- | ------------------------- |
| locale | string | Yes   | Locale used for formatting, for example, **zh-Hans-CN**.|

**Return value**

| Type    | Description                 |
| ------ | ------------------- |
| string | Sequence of the year, month, and day in the locale.|

**Example**
  ```js
  let order = I18n.I18NUtil.getDateOrder("zh-CN");  // order = "y-L-d"
  ```