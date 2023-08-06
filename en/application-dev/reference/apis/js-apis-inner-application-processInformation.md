# ProcessInformation

The **ProcessInformation** module defines the running information of a process.

> **NOTE**
> 
> The initial APIs of this module are supported since API version 9. Newly added APIs will be marked with a superscript to indicate their earliest API version.

## How to Use

The process information is obtained by calling [getRunningProcessInformation](js-apis-inner-application-applicationContext.md#getRunningProcessInformation) of the **ApplicationContext** module.

```ts

applicationContext.getRunningProcessInformation((error, data) => { 
    if (error) {
        console.error('getRunningProcessInformation fail, error: ${JSON.stringify(error)}');
    } else {
        console.log('getRunningProcessInformation success, data: ${JSON.stringify(data)}');
    }
});
```

## Attributes

**System capability**: SystemCapability.Ability.AbilityRuntime.Core

| Name| Type| Readable| Writable| Description|
| -------- | -------- | -------- | -------- | -------- |
| pid | number | Yes| No| Process ID.|
| processName | string | Yes| No| Process name.|
| bundleNames | Array&lt;string&gt; | Yes| No| Names of all running bundles in the process.|
