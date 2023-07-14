# ArkUI-X Application Manifest

## Introduction

For an ArkUI cross-platform application development project, you must declare page routing and the ArkUI development paradigm in the **manifest.json** file.

## Manifest Internal Structure

The **manifest.json** file mainly consists of the **js** tag. For details, see Table 1.

Table 1 Internal structure of the js tag

| Attribute| Description                                                        | Data Type| Initial Value Allowed              |
| -------- | ------------------------------------------------------------ | -------- | ------------------------ |
| name     | Name of the JS component. The default value is **MainAbility**.   | String  | No                |
| pages    | Route information about all pages in the JS component, including the page path and page name. The value is an array, in which each element represents a page. The first element in the array represents the home page of the JS FA.| String array    | No                |
| window   | Window-related configurations. For details, see Table 2.| Object    | Yes                  |
| mode     | Development mode of the JS component. For details, see Table 3.                            | Object    | Yes (initial value: left empty)      |

Table 2 Internal structure of the window attribute

| Attribute       | Description                                                        | Data Type| Initial Value Allowed             |
| --------------- | ------------------------------------------------------------ | -------- | ----------------------- |
| designWidth     | Baseline width for page design. The size of an element is scaled by the actual device width.| Number    | Yes (initial value: **720px**)  |
| autoDesignWidth | Whether to automatically calculate the baseline width for page design. If it is set to **true**, the **designWidth** attribute becomes invalid, and the baseline width is calculated based on the device width and screen density.| Boolean  | Yes (initial value: **false**)|

Table 3 Internal structure of the mode attribute

| Attribute| Description                | Data Type                           | Initial Value Allowed                 |
| -------- | -------------------- | ----------------------------------- | --------------------------- |
| syntax   | Syntax type of the JS component.| String. The value can be **"hml"** or **"ets"**.         | Yes (initial value: **"hml"**)      |


```json
{
  "js": [
    {
      "mode": {
        "syntax": "ets"
      },
      "name": ".MainAbility",
      "pages": [
        "pages/index/index"
      ],
      "window": {
        "designWidth": 720,
        "autoDesignWidth": false
      }
    }
  ]
}
```
