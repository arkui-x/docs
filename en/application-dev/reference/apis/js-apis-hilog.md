# @ohos.hilog (HiLog)

The **hilog** module allows your applications or services to output logs based on the specified type, level, and format string. Such logs help you learn the running status of applications and better debug programs.

> **NOTE**<br>
> The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## Modules to Import

```js
import hilog from '@ohos.hilog';
```

**System capability**: SystemCapability.HiviewDFX.HiLog


## LogLevel

Enumerates the log levels.

**System capability**: SystemCapability.HiviewDFX.HiLog

| Name | Value| Description                                                        |
| ----- | ------ | ------------------------------------------------------------ |
| DEBUG | 3      | Log level used to record more detailed process information than INFO logs to help developers analyze service processes and locate faults.|
| INFO  | 4      | Log level used to record key service process nodes and exceptions that occur during service running,<br>for example, no network signal or login failure.<br>These logs should be recorded by the dominant module in the service to avoid repeated logging conducted by multiple invoked modules or low-level functions.|
| WARN  | 5      | Log level used to record severe, unexpected faults that have little impact on users and can be rectified by the programs themselves or through simple operations.|
| ERROR | 6      | Log level used to record program or functional errors that affect the normal running or use of the functionality and can be fixed at a high cost, for example, by resetting data.|
| FATAL | 7      | Log level used to record program or functionality crashes that cannot be rectified.              |

## hilog.debug

debug(domain: number, tag: string, format: string, ...args: any[]) : void

Prints DEBUG logs.

DEBUG logs are not recorded in official versions by default. They are available in debug versions or in official versions with the debug function enabled.

**System capability**: SystemCapability.HiviewDFX.HiLog

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| domain | number | Yes  | Service domain of logs. The value ranges from **0x0** to **0xFFFF**. You can define the value within your application as required.|
| tag    | string | Yes  | Log tag in the string format. You are advised to use this parameter to identify a particular service behavior or the class holding the ongoing method.|
| format | string | Yes  | Format string used to output logs in a specified format. It can contain several parameters, where the parameter type and privacy identifier are mandatory.<br>Parameters labeled **{public}** are public data and are displayed in plaintext; parameters labeled **{private}** (default value) are private data and are filtered by **<private>**.|
| args   | any[]  | Yes  | Variable-length parameter list corresponding to the format string. The number and type of parameters must map to the identifier in the format string.|

**Example**

This example is used to output a DEBUG log with the format string being `"%{public}s World %{private}d"`. The variable `%{public}s` is a plaintext string, and the variable `%{private}d` is a private integer.

```js
hilog.debug(0x0001, "testTag", "%{public}s World %{private}d", "hello", 3);
```

If `"hello"` is filled in `%{public}s` and `3` in `%{private}d`, the output log is as follows:

```
08-05 12:21:47.579  2695-2703/com.example.myapplication D 00001/testTag: hello World <private>
```

## hilog.info

info(domain: number, tag: string, format: string, ...args: any[]) : void

Prints INFO logs.

**System capability**: SystemCapability.HiviewDFX.HiLog

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| domain | number | Yes  | Service domain of logs. The value ranges from **0x0** to **0xFFFF**. You can define the value within your application as required.|
| tag    | string | Yes  | Log tag in the string format. You are advised to use this parameter to identify a particular service behavior or the class holding the ongoing method.|
| format | string | Yes  | Format string used to output logs in a specified format. It can contain several parameters, where the parameter type and privacy identifier are mandatory.<br>Parameters labeled **{public}** are public data and are displayed in plaintext; parameters labeled **{private}** (default value) are private data and are filtered by **<private>**.|
| args   | any[]  | Yes  | Variable-length parameter list corresponding to the format string. The number and type of parameters must map to the identifier in the format string.|

**Example**

This example is used to output an INFO log with the format string being `"%{public}s World %{private}d"`. The variable `%{public}s` is a plaintext string, and the variable `%{private}d` is a private integer.

```js
hilog.info(0x0001, "testTag", "%{public}s World %{private}d", "hello", 3);
```

If `"hello"` is filled in `%{public}s` and `3` in `%{private}d`, the output log is as follows:

```
08-05 12:21:47.579  2695-2703/com.example.myapplication I 00001/testTag: hello World <private>
```

## hilog.warn

warn(domain: number, tag: string, format: string, ...args: any[]) : void

Prints WARN logs.

**System capability**: SystemCapability.HiviewDFX.HiLog

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| domain | number | Yes  | Service domain of logs. The value ranges from **0x0** to **0xFFFF**. You can define the value within your application as required.|
| tag    | string | Yes  | Log tag in the string format. You are advised to use this parameter to identify a particular service behavior or the class holding the ongoing method.|
| format | string | Yes  | Format string used to output logs in a specified format. It can contain several parameters, where the parameter type and privacy identifier are mandatory.<br>Parameters labeled **{public}** are public data and are displayed in plaintext; parameters labeled **{private}** (default value) are private data and are filtered by **<private>**.|
| args   | any[]  | Yes  | Variable-length parameter list corresponding to the format string. The number and type of parameters must map to the identifier in the format string.|

**Example**

This example is used to output a WARN log with the format string being `"%{public}s World %{private}d"`. The variable `%{public}s` is a plaintext string, and the variable `%{private}d` is a private integer.

```js
hilog.warn(0x0001, "testTag", "%{public}s World %{private}d", "hello", 3);
```

If `"hello"` is filled in `%{public}s` and `3` in `%{private}d`, the output log is as follows:

```
08-05 12:21:47.579  2695-2703/com.example.myapplication W 00001/testTag: hello World <private>
```

## hilog.error

error(domain: number, tag: string, format: string, ...args: any[]) : void

Prints ERROR logs.

**System capability**: SystemCapability.HiviewDFX.HiLog

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| domain | number | Yes  | Service domain of logs. The value ranges from **0x0** to **0xFFFF**. You can define the value within your application as required.|
| tag    | string | Yes  | Log tag in the string format. You are advised to use this parameter to identify a particular service behavior or the class holding the ongoing method.|
| format | string | Yes  | Format string used to output logs in a specified format. It can contain several parameters, where the parameter type and privacy identifier are mandatory.<br>Parameters labeled **{public}** are public data and are displayed in plaintext; parameters labeled **{private}** (default value) are private data and are filtered by **<private>**.|
| args   | any[]  | Yes  | Variable-length parameter list corresponding to the format string. The number and type of parameters must map to the identifier in the format string.|

**Example**

This example is used to output an ERROR log with the format string being `"%{public}s World %{private}d"`. The variable `%{public}s` is a plaintext string, and the variable `%{private}d` is a private integer.

```js
hilog.error(0x0001, "testTag", "%{public}s World %{private}d", "hello", 3);
```

If `"hello"` is filled in `%{public}s` and `3` in `%{private}d`, the output log is as follows:

```
08-05 12:21:47.579  2695-2703/com.example.myapplication E 00001/testTag: hello World <private>
```

## hilog.fatal

fatal(domain: number, tag: string, format: string, ...args: any[]) : void

Prints FATAL logs.

**System capability**: SystemCapability.HiviewDFX.HiLog

**Parameters**

| Name| Type  | Mandatory| Description                                                        |
| ------ | ------ | ---- | ------------------------------------------------------------ |
| domain | number | Yes  | Service domain of logs. The value ranges from **0x0** to **0xFFFF**. You can define the value within your application as required.|
| tag    | string | Yes  | Log tag in the string format. You are advised to use this parameter to identify a particular service behavior or the class holding the ongoing method.|
| format | string | Yes  | Format string used to output logs in a specified format. It can contain several parameters, where the parameter type and privacy identifier are mandatory.<br>Parameters labeled **{public}** are public data and are displayed in plaintext; parameters labeled **{private}** (default value) are private data and are filtered by **<private>**.|
| args   | any[]  | Yes  | Variable-length parameter list corresponding to the format string. The number and type of parameters must map to the identifier in the format string.|

**Example**

This example is used to output a FATAL log with the format string being `"%{public}s World %{private}d"`. The variable `%{public}s` is a plaintext string, and the variable `%{private}d` is a private integer.

```js
hilog.fatal(0x0001, "testTag", "%{public}s World %{private}d", "hello", 3);
```

If `"hello"` is filled in `%{public}s` and `3` in `%{private}d`, the output log is as follows:

```
08-05 12:21:47.579  2695-2703/com.example.myapplication F 00001/testTag: hello World <private>
```