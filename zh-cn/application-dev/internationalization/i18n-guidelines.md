# I18n开发指导

本模块提供系统相关的或者增强的国际化能力，包括区域管理、日历等，相关接口为ECMA 402标准中未定义的补充接口。更多接口和使用方式请见[I18N](../reference/apis/js-apis-i18n.md)。

[Intl](intl-guidelines.md)模块提供了ECMA 402标准定义的基础国际化接口，与本模块共同使用可提供完整地国际化支持能力。

## 获取和设置系统国际化相关信息

调用接口访问系统语言、地区、24小时制等国际化信息。

### 接口说明

| 类名        | 接口名称                                     | 描述                    |
| --------- | ---------------------------------------- | --------------------- |
| System | getDisplayCountry(country:string,locale:string,sentenceCase?:boolean):string<sup>9+</sup> | 获取国家的本地化表示。           |
| System | getDisplayLanguage(language:string,locale:string,sentenceCase?:boolean):string<sup>9+</sup> | 获取语言的本地化表示。           |
| System | getSystemLanguage():string<sup>9+</sup>               | 获取系统语言。               |
| System | getSystemRegion():string<sup>9+</sup>                 | 获取系统地区。               |
| System | getSystemLocale():string<sup>9+</sup>                 | 获取系统Locale。           |
| System | is24HourClock():boolean<sup>9+</sup>     | 判断系统时间是否为24小时制。    |
|  | isRTL(locale:string):boolean<sup>9+</sup> | locale对应的语言是否为从右到左语言。 |

### 开发步骤
1. 导入I18n模块。

   ```js
   import I18n from '@ohos.i18n';
   ```

2. 获取系统语言。
   
   调用getSystemLanguage接口获取系统语言。
   
   ```js
   try {
      let language = I18n.System.getSystemLanguage(); // language = "en"
   } catch(error) {
      console.error(`call i18n.System interface failed, error code: ${error.code}, message: ${error.message}`);
   }
   ```

3. 获取系统区域。
   
   调用getSystemRegion接口获取系统国家。

   ```js
   try {
      let region = I18n.System.getSystemRegion(); // region = "CN"
   } catch(error) {
      console.error(`call i18n.System interface failed, error code: ${error.code}, message: ${error.message}`);
   }
   ```

4. 获取系统Locale。

   调用getSystemLocale接口获取系统Locale。

   ```js
   try {
      let locale = I18n.System.getSystemLocale(); // locale = "zh-Hans-CN"
   } catch(error) {
      console.error(`call i18n.System interface failed, error code: ${error.code}, message: ${error.message}`);
   }
   ```

5. 判断Locale的语言是否为从右到左语言。

   调用isRTL接口获取Locale的语言是否为从右到左语言。

   ```js
   try {
      let rtl = I18n.isRTL("zh-CN"); // rtl = false
      rtl = I18n.isRTL("ar"); // rtl = true
   } catch(error) {
      console.error(`call i18n.System interface failed, error code: ${error.code}, message: ${error.message}`);
   }
   ```

6. 获取系统24小时制设置。

   调用is24HourClock接口来判断当前是否打开系统24小时制设置。

   ```js
   try {
      let hourClock = I18n.System.is24HourClock(); // hourClock = true
   } catch(error) {
      console.error(`call i18n.System interface failed, error code: ${error.code}, message: ${error.message}`);
   }
   ```

7. 获取语言的本地化表示。

   调用getDisplayLanguage接口获取某一语言的本地化表示。其中，language表示待本地化显示的语言，locale表示本地化的Locale，sentenceCase结果是否需要首字母大写。

   ```js
   try {
      let language = "en";
      let locale = "zh-CN";
      let sentenceCase = false;
      let localizedLanguage = I18n.System.getDisplayLanguage(language, locale, sentenceCase); // localizedLanguage = "英语"
   } catch(error) {
      console.error(`call i18n.System interface failed, error code: ${error.code}, message: ${error.message}`);
   }
   ```

8. 获取国家的本地化表示。

   调用getDisplayCountry接口获取某一国家的本地化表示。其中，country表示待本地化显示的国家，locale表示本地化的Locale，sentenceCase结果是否需要首字母大写。

   ```js
   try {
      let country = "US";
      let locale = "zh-CN";
      let sentenceCase = false;
      let localizedCountry = I18n.System.getDisplayCountry(country, locale, sentenceCase); // localizedCountry = "美国"
   } catch(error) {
      console.error(`call i18n.System interface failed, error code: ${error.code}, message: ${error.message}`);
   }
   ```

## 获取日历信息

调用日历[Calendar](../reference/apis/js-apis-i18n.md#calendar8)相关接口来获取日历的相关信息，例如获取日历的本地化显示、一周起始日、一年中第一周的最小天数等。

### 接口说明

| 类名        | 接口名称                                     | 描述                    |
| --------- | ---------------------------------------- | --------------------- |
|  | getCalendar(locale:string,type?:string):Calendar<sup>8+</sup> | 获取指定locale和type的日历对象。 |
| Calendar | setTime(date:Date): void<sup>8+</sup>    | 设置日历对象内部的时间日期。        |
| Calendar | setTime(time:number): void<sup>8+</sup>  | 设置日历对象内部的时间日期。        |
| Calendar | set(year:number,month:number,date:number,hour?:number,minute?:number,second?:number): void<sup>8+</sup> | 设置日历对象的年、月、日、时、分、秒。   |
| Calendar | setTimeZone(timezone:string): void<sup>8+</sup> | 设置日历对象的时区。            |
| Calendar | getTimeZone():string<sup>8+</sup>        | 获取日历对象的时区。            |
| Calendar | getFirstDayOfWeek():number<sup>8+</sup>  | 获取日历对象的一周起始日。         |
| Calendar | setFirstDayOfWeek(value:number): void<sup>8+</sup> | 设置日历对象的一周起始日。         |
| Calendar | getMinimalDaysInFirstWeek():number<sup>8+</sup> | 获取一年中第一周的最小天数。        |
| Calendar | setMinimalDaysInFirstWeek(value:number): void<sup>8+</sup> | 设置一年中第一周的最小天数。        |      |
| Calendar | isWeekend(date?:Date):boolean<sup>8+</sup> | 判断给定的日期在日历中是否是周末。     |

### 开发步骤

1. 导入I18n模块。

   ```js
   import I18n from '@ohos.i18n';
   ```

2. 实例化日历对象。

   调用getCalendar接口获取指定locale和type的时区对象（i18n为导入的模块）。其中，type表示合法的日历类型，目前合法的日历类型包括："buddhist", "chinese", "coptic", "ethiopic", "hebrew", "gregory", "indian", "islamic_civil", "islamic_tbla", "islamic_umalqura", "japanese", "persian"。当type没有给出时，采用区域默认的日历类型。

   ```js
   let calendar = I18n.getCalendar("zh-CN", "chinese"); // 创建中文农历日历
   ```

3. 设置日历对象的时间。

     调用setTime接口设置日历对象的时间。setTime接口接收两种类型的参数。一种是传入一个Date对象，另一种是传入一个数值表示从1970.1.1 00:00:00 GMT逝去的毫秒数。

   ```js
   let date1 = new Date();
   calendar.setTime(date1);
   let date2 = 1000;
   calendar.setTime(date2);
   ```

4. 设置日历对象的年、月、日、时、分、秒。

     调用set接口设置日历对象的年、月、日、时、分、秒。

   ```js
   calendar.set(2021, 12, 21, 6, 0, 0);
   ```

5. 设置、获取日历对象的时区。

   调用setTimeZone接口和getTimeZone接口来设置、获取日历对象的时区。其中，setTimeZone接口需要传入一个字符串表示需要设置的时区。

   ```js
   calendar.setTimeZone("Asia/Shanghai");
   let timezone = calendar.getTimeZone();  // timezone = "China Standard Time"
   ```

6. 设置、获取日历对象的一周起始日。

   调用setFirstDayOfWeek接口和getFirstDayOfWeek接口设置、获取日历对象的一周起始日。其中，setFirstDayOfWeek需要传入一个数值表示一周的起始日，1代表周日，7代表周六。

   ```js
   calendar.setFirstDayOfWeek(1);
   let firstDayOfWeek = calendar.getFirstDayOfWeek(); // firstDayOfWeek = 1
   ```

7. 设置、获取日历对象第一周的最小天数。
     调用setMinimalDaysInFirstWeek接口和getMinimalDaysInFirstWeek接口来设置、获取日历对象第一周的最小天数。

   ```js
   calendar.setMinimalDaysInFirstWeek(3);
   let minimalDaysInFirstWeek = calendar.getMinimalDaysInFirstWeek(); // minimalDaysInFirstWeek = 3
   ```

8. 判断某一个日期是否为周末。

   调用isWeekend接口来判断输入的Date是否为周末。

   ```js
   let date = new Date(2022, 12, 12, 12, 12, 12);
   let weekend = calendar.isWeekend(date); // weekend = false
   ```


## 获取时区

调用时区[TimeZone](../reference/apis/js-apis-i18n.md#timezone)相关接口来获取时区的相关信息，例如获取时区的ID、本地化显示、时区偏移量等。

### 接口说明

| 类名        | 接口名称                                     | 描述                             |
| --------- | ---------------------------------------- | ------------------------------ |
|  | getTimeZone(zoneID?: string): TimeZone<sup>7+</sup> | 获取时区对象。                       |
| TimeZone | getID(): string<sup>7+</sup> | 获取时区ID。                      |
| TimeZone | getRawOffset(): number<sup>7+</sup>            | 获取时区对象与UTC时区的偏移量。              |
| TimeZone | getOffset(date?: number): number<sup>7+</sup>              | 获取某一时间点时区对象与UTC时区的偏移量。            |
| TimeZone | getAvailableIDs(): Array<string><sup>9+</sup>               | 获取系统支持的时区ID列表。           |
| TimeZone | getCityDisplayName(cityID: string, locale: string): string<sup>9+</sup>           | 获取时区城市ID的本地化显示。             |
| TimeZone | getTimezoneFromCity(cityID: string): TimeZone<sup>9+</sup> | 获取时区城市ID对应的时区对象。 |

### 开发步骤

1. 导入I18n模块。

   ```js
   import I18n from '@ohos.i18n';
   ```

2. 实例化时区对象，并获取相关时区信息。

   调用getTimeZone接口来获取时区对象。

   ```js
   let timezone = I18n.getTimeZone(); // 使用默认参数可以获取系统时区对象。
   ```

   获取时区ID、时区偏移量、某一时刻的时区偏移量信息。
   
   ```js
   let timezoneID = timezone.getID(); // timezoneID = "Asia/Shanghai"
   let rawOffset = timezone.getRawOffset(); // rawOffset = 28800000
   let offset = timezone.getOffset(new Date().getTime()); // offset = 28800000
   ```

3. 获取系统支持的时区ID。

   调用getAvailableIDs接口获取系统支持的时区ID列表。
   时区ID列表中的时区ID可以作为getTimeZone接口的参数，来创建TimeZone对象。

   ```js
   let timezoneIDs = I18n.TimeZone.getAvailableIDs(); // timezoneIDs = ["America/Adak", ...]，共包含24个时区ID
   ```

## 字符类型判断

调用字符[Unicode](../reference/apis/js-apis-i18n.md#unicode9)相关接口来获取字符的相关信息，例如字符是否是数字、字符是否是空格符等。

### 接口说明

| 类名        | 接口名称                                     | 描述                             |
| --------- | ---------------------------------------- | ------------------------------ |
| Unicode | isDigit(char: string): boolean<sup>9+</sup> | 判断字符是否是数字。                       |
| Unicode | isSpaceChar(char: string): boolean<sup>9+</sup> | 判断字符是否是空格符。                      |
| Unicode | isWhitespace(char: string): boolean<sup>9+</sup>   | 判断字符是否是空白符。                      |
| Unicode | isRTL(char: string): boolean<sup>9+</sup>            | 判断字符是否是从左到右显示的字符。              |
| Unicode | isIdeograph(char: string): boolean<sup>9+</sup>              | 判断字符是否是表意文字。            |
| Unicode | isLetter(char: string): boolean<string><sup>9+</sup>               | 判断字符是否是字母。           |
| Unicode | isLowerCase(char: string): boolean<string><sup>9+</sup>  | 判断字符是否是小写字母。           |
| Unicode | isUpperCase(char: string): boolean<sup>9+</sup>           | 判断字符是否是大写字母。             |
| Unicode | getType(char: string): string<sup>9+</sup> | 获取字符的类型。 |

### 开发步骤

1. 导入I18n模块。

   ```js
   import I18n from '@ohos.i18n';
   ```

2. 判断字符是否具有某种性质。

   判断字符是否是数字。

   ```js
   let isDigit = I18n.Unicode.isDigit("1"); // isDigit = true
   isDigit = I18n.Unicode.isDigit("a"); // isDigit = false
   ```

   判断字符是否是空格符。

   ```js
   let isSpaceChar = I18n.Unicode.isSpaceChar(" "); // isSpaceChar = true
   isSpaceChar = I18n.Unicode.isSpaceChar("\n"); // isSpaceChar = false
   ```

   判断字符是否是空白符。

   ```js
   let isWhitespace = I18n.Unicode.isWhitespace(" "); // isWhitespace = true
   isWhitespace = I18n.Unicode.isWhitespace("\n"); // isWhitespace = true
   ```

   判断字符是否是从左到右书写的文字。

   ```js
   let isRTL = I18n.Unicode.isRTL("مرحبًا"); // isRTL = true，阿拉伯语的文字是从左到右书写的文字
   isRTL = I18n.Unicode.isRTL("a"); // isRTL = false
   ```

   判断字符是否是表意文字。

   ```js
   let isIdeograph = I18n.Unicode.isIdeograph("你好"); // isIdeograph = true
   isIdeograph = I18n.Unicode.isIdeograph("a"); // isIdeograph = false
   ```

   判断字符是否是字母。

   ```js
   let isLetter = I18n.Unicode.isLetter("a"); // isLetter = true
   isLetter = I18n.Unicode.isLetter("."); // isLetter = false
   ```

   判断字符是否是小写字母。

   ```js
   let isLowerCase = I18n.Unicode.isLowerCase("a"); // isLetter = true
   isLowerCase = I18n.Unicode.isLowerCase("A"); // isLetter = false
   ```

   判断字符是否是大写字母。

   ```js
   let isUpperCase = I18n.Unicode.isUpperCase("a"); // isUpperCase = false
   isUpperCase = I18n.Unicode.isUpperCase("A"); // isUpperCase = true
   ```

3. 获取字符的类型。

   调用getType接口获取字符的类型。

   ```js
   let type = I18n.Unicode.getType("a"); // type = U_LOWER_CASE_LETTER
   ```

## 获取日期中年月日的排列顺序

### 接口说明

| 类名        | 接口名称                                     | 描述                             |
| --------- | ---------------------------------------- | ------------------------------ |
| I18NUtil | getDateOrder(locale: string): string<sup>9+</sup> | 判断日期中年月日的排列顺序。                       |

### 开发步骤

1. 导入I18n模块。

   ```js
   import I18n from '@ohos.i18n';
   ```

2. 判断日期的年月日的排序顺序。

   调用getDateOrder接口判断某一Locale的日期中，年月日的排列顺序。
   接口返回一个字符串，由"y"，"L"，"d"三部分组成，分别表示年、月、日，使用中划线进行拼接。例如，"y-L-d"。

   ```js
   let order = I18n.I18NUtil.getDateOrder("zh-CN"); // order = "y-L-d"，表示中文中年月日的顺序为年-月-日。
   ```