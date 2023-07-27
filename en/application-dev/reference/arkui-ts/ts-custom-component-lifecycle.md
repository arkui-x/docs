# Custom Component Lifecycle

The lifecycle callbacks of a custom component are used to notify users of the lifecycle of the component. These callbacks are private and are invoked by the development framework at a specified time at runtime. They cannot be manually invoked from applications.

>**NOTE**
>
>- The initial APIs of this module are supported since API version 7. Newly added APIs will be marked with a superscript to indicate their earliest API version.
>- Promise and asynchronous callback functions can be used in lifecycle functions, for example, network resource getters and timer setters.


## aboutToAppear

aboutToAppear?(): void

Invoked after a new instance of the custom component is created and before its **build** function is executed. You can change state variables in the **aboutToAppear** function. The change will take effect when you execute the **build** function next time.

Since API version 9, this API is supported in ArkTS widgets.

## aboutToDisappear

aboutToDisappear?(): void

Invoked before the destructor of the custom component is consumed. Do not change state variables in the **aboutToDisappear** function as doing this can cause unexpected errors. For example, the modification of the **@Link** decorated variable may cause unstable application running.

Since API version 9, this API is supported in ArkTS widgets.


## onBackPress

onBackPress?(): void

Invoked when a user clicks the back button. It works only for the custom components decorated by **@Entry**.
