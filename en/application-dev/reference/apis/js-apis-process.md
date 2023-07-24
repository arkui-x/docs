# @ohos.process (Obtaining Process Information)

> **NOTE**
>
> The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.


## Modules to Import

```
import process from '@ohos.process';
```


## Attributes

**System capability**: SystemCapability.Utils.Lang

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| uid | number | Yes| No| User identifier (UID) of the process.|
| pid | number | Yes| No| Process ID (PID) of the process.|
| tid<sup>8+</sup> | number | Yes| No| Thread ID (TID) of the thread.|


## EventListener

**System capability**: SystemCapability.Utils.Lang

| Name| Description|
| -------- | -------- |
| EventListener&nbsp;=&nbsp;(evt: &nbsp;Object)&nbsp;=&gt;&nbsp;void | Event to store.|


## process.isIsolatedProcess<sup>8+</sup>

isIsolatedProcess(): boolean

Checks whether this process is isolated.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the process is isolated; returns **false** otherwise.|

**Example**

```js
let result = process.isIsolatedProcess();
```


## process.is64Bit<sup>8+</sup>

is64Bit(): boolean

Checks whether this process is running in a 64-bit environment.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the process is running in a 64-bit environment; returns **false** otherwise.|

**Example**

```js
let result = process.is64Bit();
```


## process.getStartRealtime<sup>8+</sup>

getStartRealtime(): number

Obtains the duration, in milliseconds, from the time the system starts to the time the process starts.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number | Duration obtained, in millisecond.|

**Example**

```js
let realtime = process.getStartRealtime();
```

## process.getPastCpuTime<sup>8+</sup>

getPastCpuTime(): number

Obtains the CPU time (in milliseconds) from the time the process starts to the current time.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number | CPU time obtained, in millisecond.|

**Example**

```js
let result = process.getPastCpuTime() ;
```


## process.abort

abort(): void

Aborts a process and generates a core file. This method will cause a process to exit immediately. Exercise caution when using this method.

**System capability**: SystemCapability.Utils.Lang

**Example**

```js
process.abort();
```


## process.uptime

uptime(): number

Obtains the running time of this process.

**System capability**: SystemCapability.Utils.Lang

**Return value**

| Type| Description|
| -------- | -------- |
| number | Running time of the process, in seconds.|

**Example**

```js
let time = process.uptime();
```


## ProcessManager<sup>9+</sup>	

Provides APIs for throwing exceptions during the addition of a process.

A **ProcessManager** object is obtained through its own constructor.

### isAppUid<sup>9+</sup>

isAppUid(v: number): boolean

Checks whether a UID belongs to this application.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| v | number | Yes| UID.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the UID belongs to the application; returns **false** otherwise.|

**Example**

```js
let pro = new process.ProcessManager();
let result = pro.isAppUid(688);
```


### getUidForName<sup>9+</sup>

getUidForName(v: string): number

Obtains the process UID based on the process name.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| v | string | Yes| Name of a process.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Process UID.|

**Example**

```js
let pro = new process.ProcessManager();
let pres = pro .getUidForName("tool");
```


### getThreadPriority<sup>9+</sup>

getThreadPriority(v: number): number

Obtains the thread priority based on the specified TID.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| v | number | Yes| TID.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | Priority of the thread.|

**Example**

```js
let pro = new process.ProcessManager();
let tid = process.tid;
let pres = pro.getThreadPriority(tid);
```


### getSystemConfig<sup>9+</sup>

getSystemConfig(name: number): number

Obtains the system configuration.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | number | Yes| System configuration parameter name.|

**Return value**

| Type| Description|
| -------- | -------- |
| number | System configuration obtained.|

**Example**

```js
let pro = new process.ProcessManager();
let _SC_ARG_MAX = 0;
let pres = pro.getSystemConfig(_SC_ARG_MAX);
```


### getEnvironmentVar<sup>9+</sup>

getEnvironmentVar(name: string): string

Obtains the value of an environment variable.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| name | string | Yes| Environment variable name.|

**Return value**

| Type| Description|
| -------- | -------- |
| string | Value of the environment variable.|

**Example**

```js
let pro = new process.ProcessManager();
let pres = pro.getEnvironmentVar("PATH");
```


### exit<sup>9+</sup>

exit(code: number): void

Terminates this process.

Exercise caution when using this API. After this API is called, the application exits. If the input parameter is not 0, data loss or exceptions may occur.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| code | number | Yes| Exit code of the process.|

**Example**

```js
let pro = new process.ProcessManager();
pro.exit(0);
```


### kill<sup>9+</sup>

kill(signal: number, pid: number): boolean

Sends a signal to the specified process to terminate it.

**System capability**: SystemCapability.Utils.Lang

**Parameters**

| Name| Type| Mandatory| Description|
| -------- | -------- | -------- | -------- |
| pid | number | Yes| PID of the process, to which the signal will be sent.|
| signal | number | Yes| Signal to send.|

**Return value**

| Type| Description|
| -------- | -------- |
| boolean | Returns **true** if the signal is sent successfully; returns **false** otherwise.|

**Example**

```js
let pro = new process.ProcessManager();
let pres = process.pid;
let result = pro.kill(28, pres);
```
