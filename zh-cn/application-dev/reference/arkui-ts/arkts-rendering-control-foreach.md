# ForEach：循环渲染


ForEach基于数组类型数据执行循环渲染。

> **说明：**
>
> 从API version 9开始，该接口支持在ArkTS卡片中使用。

## 接口描述


```ts
ForEach(
  arr: Array,
  itemGenerator: (item: Array, index?: number) => void,
  keyGenerator?: (item: Array, index?: number): string => string
)
```


| 参数名           | 参数类型                                     | 必填   | 参数描述                                     |
| ------------- | ---------------------------------------- | ---- | ---------------------------------------- |
| arr           | Array                                    | 是    | 必须是数组，允许设置为空数组，空数组场景下将不会创建子组件。同时允许设置返回值为数组类型的函数，例如arr.slice(1,&nbsp;3)，设置的函数不得改变包括数组本身在内的任何状态变量，如Array.splice、Array.sort或Array.reverse这些改变原数组的函数。 |
| itemGenerator | (item:&nbsp;any,&nbsp;index?:&nbsp;number)&nbsp;=&gt;&nbsp;void | 是    | 生成子组件的lambda函数，为数组中的每一个数据项创建一个或多个子组件，单个子组件或子组件列表必须包括在大括号“{...}”中。<br/>**说明：**<br/>-&nbsp;子组件的类型必须是ForEach的父容器组件所允许的（例如，只有当ForEach父级为List组件时，才允许ListItem子组件）。<br/>-&nbsp;允许子类构造函数返回if或另一个ForEach。ForEach可以在if内的任意位置。<br/>-&nbsp;可选index参数如在函数体中使用，则必须仅在函数签名中指定。 |
| keyGenerator  | (item:&nbsp;any,&nbsp;index?:&nbsp;number)&nbsp;=&gt;&nbsp;string | 否    | 匿名函数，用于给数组中的每一个数据项生成唯一且固定的键值。键值生成器的功能是可选的，但是，为了使开发框架能够更好地识别数组更改，提高性能，建议提供。如将数组反向时，如果没有提供键值生成器，则ForEach中的所有节点都将重建。<br/>**说明：**<br/>-&nbsp;同一数组中的不同项绝对不能计算出相同的ID。<br/>-&nbsp;如果未使用index参数，则项在数组中的位置变动不得改变项的键值。如果使用了index参数，则当项在数组中的位置有变动时，键值必须更改。<br/>-&nbsp;当某个项目被新项替换（值不同）时，被替换的项键值和新项的键值必须不同。<br/>-&nbsp;在构造函数中使用index参数时，键值生成函数也必须使用该参数。<br>-&nbsp;键值生成函数不允许改变任何组件状态。 |


## 使用限制

- ForEach必须在容器组件内使用。

- 生成的子组件应当是允许包含在ForEach父容器组件中的子组件。

- 允许子组件生成器函数中包含if/else条件渲染，同时也允许ForEach包含在if/else条件渲染语句中。

- itemGenerator函数的调用顺序不一定和数组中的数据项相同，在开发过程中不要假设itemGenerator和keyGenerator函数是否执行及其执行顺序。例如，以下示例可能无法正确运行：

    ```ts
    let obj: Object
    ForEach(anArray.map((item1: Object, index1: number): Object => {
        obj.i = index1 + 1
        obj.data = item1
        return obj;
      }),
    (item: string) => Text(`${item.i}. item.data.label`),
    (item: string): string => {
        return item.data.id.toString()
    })
    ```


## 开发建议

- 建议开发者不要假设项构造函数的执行顺序。执行顺序可能不能是数组中项的排列顺序。

- 不要假设数组项是否是初始渲染。ForEach的初始渲染在\@Component首次渲染时构建所有数组项。后续框架版本中可能会将此行为更改为延迟加载模式。

- 使用 index参数对UI更新性能有严重的负面影响，请尽量避免。

- 如果项构造函数中使用index参数，则项索引函数中也必须使用该参数。否则，如果项索引函数未使用index参数，ForEach在生成实际的键值时，框架也会把index考虑进来，默认将index拼接在后面。


## 使用场景

项目索引函数为每个数组项创建唯一且持久的键值，ArkUI框架通过此键值确定数组中的项是否有变化，只要键值相同，数组项的值就假定不变，但其索引位置可能会更改。此机制的运行前提是不同的数组项不能有相同的键值。

使用计算出的ID，框架可以对添加、删除和保留的数组项加以区分：

1. 框架将删除已删除数组项的UI组件。

2. 框架仅对新添加的数组项执行项构造函数。

3. 框架不会为保留的数组项执行项构造函数。如果数组中的项索引已更改，框架将仅根据新顺序移动其UI组件，但不会更新该UI组件。

建议使用项目索引函数，但这是可选的。生成的ID必须是唯一的，这意味着不能为数组中的不同项计算出相同的ID。即使两个数组项具有相同的值，其ID也必须不同。

如果数组项值更改，则ID必须更改。
如前所述，id生成函数是可选的。以下是不带项索引函数的ForEach：

  ```ts
let list: Object
ForEach(this.arr,
    (item: Object): string => {
      list.label = item.toString();
      CounterView(list)
    }
  )
  ```


### ForEach中使用可选index参数示例

可以在构造函数和ID生成函数中使用可选的index参数。

必须正确构造ID生成函数。当在项构造函数中使用index参数时，ID生成函数也必须使用index参数，以生成唯一ID和给定源数组项的ID。当数组项在数组中的索引位置发生变化时，其ID会发生变化。

此示例还说明了index参数会造成显著性能下降。即使项在源数组中移动而不做修改，因为索引发生改变，依赖该数组项的UI仍然需要重新渲染。例如，使用索引排序时，数组只需要将ForEach未修改的子UI节点移动到正确的位置，这对于框架来说是一个轻量级操作。而使用索引时，所有子UI节点都需要重新构建，这操作负担要重得多。
