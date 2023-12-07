# ForEach: Rendering of Repeated Content


**ForEach** enables repeated content based on array-type data.

> **NOTE**
>
> Since API version 9, this API is supported in ArkTS widgets.

## API Description


```ts
ForEach(
  arr: Array,
  itemGenerator: (item: Array, index?: number) => void,
  keyGenerator?: (item: Array, index?: number): string => string
)
```


| Name          | Type                                    | Mandatory  | Description                                    |
| ------------- | ---------------------------------------- | ---- | ---------------------------------------- |
| arr           | Array                                    | Yes   | An array, which can be empty, in which case no child component is created. The functions that return array-type values are also allowed, for example, **arr.slice (1, 3)**. The set functions cannot change any state variables including the array itself, such as **Array.splice**, **Array.sort**, and **Array.reverse**.|
| itemGenerator | (item: any, index?: number) =&gt; void | Yes   | A lambda function used to generate one or more child components for each data item in an array. Each component and its child component list must be contained in parentheses.<br>**NOTE**<br>- The type of the child component must be the one allowed inside the parent container component of **ForEach**. For example, a **\<ListItem>** child component is allowed only when the parent container component of **ForEach** is **\<List>**.<br>- The child build function is allowed to return an **if** or another **ForEach**. **ForEach** can be placed inside **if**.<br>- The optional **index** parameter should only be specified in the function signature if used in its body.|
| keyGenerator  | (item: any, index?: number) =&gt; string | No   | An anonymous function used to generate a unique and fixed key value for each data item in an array. This key-value generator is optional. However, for performance reasons, it is strongly recommended that the key-value generator be provided, so that the development framework can better identify array changes. For example, if no key-value generator is provided, a reverse of an array will result in rebuilding of all nodes in **ForEach**.<br>**NOTE**<br>- Two items inside the same array must never work out the same ID.<br>- If **index** is not used, an item's ID must not change when the item's position within the array changes. However, if **index** is used, then the ID must change when the item is moved within the array.<br>- When an item is replaced by a new one (with a different value), the ID of the replaced and the ID of the new item must be different.<br>- When **index** is used in the build function, it should also be used in the ID generation function.<br>- The ID generation function is not allowed to mutate any component state.|


## Restrictions

- **ForEach** must be used in container components.

- The type of the child component must be the one allowed inside the parent container component of **ForEach**. 

- The **itemGenerator** function can contain an **if/else** statement, and an **if/else** statement can contain **ForEach**.

- The call sequence of **itemGenerator** functions may be different from that of the data items in the array. During the development, do not assume whether or when the **itemGenerator** and **keyGenerator** functions are executed. For example, the following example may not run properly:



## Development Guidelines

- Make no assumption on the order of item build functions. The execution order may not be the order of items inside the array.

- Make no assumption either when items are built the first time. Currently initial render of **ForEach** builds all array items when the \@Component decorated component is rendered at the first time, but future framework versions might change this behaviour to a more lazy behaviour.

- Using the **index** parameter has severe negative impact on the UI update performance. Avoid using this parameter whenever possible.

- If the **index** parameter is used in the item generator function, it must also be used in the item index function. Otherwise, the framework counts in the index when generating the ID. By default, the index is concatenated to the end of the ID.

**MainView** has an \@State decorated array of numbers. Adding, deleting, and replacing array items are observed mutation events. Whenever one of these events occurs, **ForEach** in **MainView** is updated.

The item index function creates a unique and persistent ID for each array item. The ArkUI framework uses this ID to determine whether an item in the array changes. As long as the ID is the same, the item value is assumed to remain unchanged, but its index position may have changed. For this mechanism to work, different array items cannot have the same ID.

Using the item ID obtained through computation, the framework can distinguish newly created, removed, and retained array items.

1. The framework removes UI components for a removed array item.

2. The framework executes the item build function only for newly added array items.

3. The framework does not execute the item build function for retained array items. If the item index within the array has changed, the framework will just move its UI components according to the new order, but will not update that UI components.

The item index function is recommended, but optional. The generated IDs must be unique. This means that the same ID must not be computed for any two items within the array. The ID must be different even if the two array items have the same value.

If the array item value changes, the ID must be changed.
As mentioned earlier, the ID generation function is optional. The following example shows **ForEach** without the item index function: