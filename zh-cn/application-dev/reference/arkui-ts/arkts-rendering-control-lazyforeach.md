# LazyForEach：数据懒加载


LazyForEach从提供的数据源中按需迭代数据，并在每次迭代过程中创建相应的组件。当在滚动容器中使用了LazyForEach，框架会根据滚动容器可视区域按需创建组件，当组件滑出可视区域外时，框架会进行组件销毁回收以降低内存占用。


## 接口描述

**参数：**


| 参数名        | 参数类型                                | 必填 | 参数描述                                                     |
| ------------- | --------------------------------------- | ---- | ------------------------------------------------------------ |
| dataSource    | [IDataSource](#idatasource类型说明)     | 是   | LazyForEach数据源，需要开发者实现相关接口。                  |
| itemGenerator | (item:&nbsp;any)&nbsp;=&gt;&nbsp;void   | 是   | 子组件生成函数，为数组中的每一个数据项创建一个子组件。<br/>**说明：**<br/>itemGenerator的函数体必须使用大括号{...}。itemGenerator每次迭代只能并且必须生成一个子组件。itemGenerator中可以使用if语句，但是必须保证if语句每个分支都会创建一个相同类型的子组件。itemGenerator中不允许使用ForEach和LazyForEach语句。 |
| keyGenerator  | (item:&nbsp;any)&nbsp;=&gt;&nbsp;string | 否   | 键值生成函数，用于给数据源中的每一个数据项生成唯一且固定的键值。当数据项在数组中的位置更改时，其键值不得更改，当数组中的数据项被新项替换时，被替换项的键值和新项的键值必须不同。键值生成器的功能是可选的，但是，为了使开发框架能够更好地识别数组更改，提高性能，建议提供。如将数组反向时，如果没有提供键值生成器，则LazyForEach中的所有节点都将重建。<br/>**说明：**<br/>数据源中的每一个数据项生成的键值不能重复。 |

## IDataSource类型说明

| 接口声明                                                     | 参数类型                                          | 说明                                                        |
| ------------------------------------------------------------ | ------------------------------------------------- | ----------------------------------------------------------- |
| totalCount():&nbsp;number                                    | -                                                 | 获得数据总数。                                              |
| getData(index:&nbsp;number):&nbsp;any                        | number                                            | 获取索引值index对应的数据。<br/>index：获取数据对应的索引值。 |
| registerDataChangeListener(listener:[DataChangeListener](#datachangelistener类型说明)):&nbsp;void | [DataChangeListener](#datachangelistener类型说明) | 注册数据改变的监听器。<br/>listener：数据变化监听器         |
| unregisterDataChangeListener(listener:[DataChangeListener](#datachangelistener类型说明)):&nbsp;void | [DataChangeListener](#datachangelistener类型说明) | 注销数据改变的监听器。<br/>listener：数据变化监听器         |

## DataChangeListener类型说明

| 接口声明                                                     | 参数类型                               | 说明                                                         |
| ------------------------------------------------------------ | -------------------------------------- | ------------------------------------------------------------ |
| onDataReloaded():&nbsp;void                                  | -                                      | 通知组件重新加载所有数据。                                   |
| onDataAdd(index:&nbsp;number):&nbsp;void<sup>8+</sup>        | number                                 | 通知组件index的位置有数据添加。<br/>index：数据添加位置的索引值。 |
| onDataMove(from:&nbsp;number,&nbsp;to:&nbsp;number):&nbsp;void<sup>8+</sup> | from:&nbsp;number,<br/>to:&nbsp;number | 通知组件数据有移动。<br/>from:&nbsp;数据移动起始位置，to:&nbsp;数据移动目标位置。<br/>**说明：**<br/>数据移动前后键值要保持不变，如果键值有变化，应使用删除数据和新增数据接口。 |
| onDataDelete(index: number):void<sup>8+</sup>                | number                                 | 通知组件删除index位置的数据并刷新LazyForEach的展示内容。<br/>index：数据删除位置的索引值。<br/>**说明：** <br/>需要保证dataSource中的对应数据已经在调用onDataDelete前删除，否则页面渲染将出现未定义的行为。 |
| onDataChange(index:&nbsp;number):&nbsp;void<sup>8+</sup>     | number                                 | 通知组件index的位置有数据有变化。<br/>index：数据变化位置的索引值。 |


## 使用限制

- LazyForEach必须在容器组件内使用，仅有[List](ts-container-list.md)、[Grid](ts-container-grid.md)、[Swiper](ts-container-swiper.md)以及[WaterFlow](ts-container-waterflow.md)组件支持数据懒加载（可配置cachedCount属性，即只加载可视部分以及其前后少量数据用于缓冲），其他组件仍然是一次性加载所有的数据。

- LazyForEach在每次迭代中，必须创建且只允许创建一个子组件。

- 生成的子组件必须是允许包含在LazyForEach父容器组件中的子组件。

- 允许LazyForEach包含在if/else条件渲染语句中，也允许LazyForEach中出现if/else条件渲染语句。

- 键值生成器必须针对每个数据生成唯一的值，如果键值相同，将导致键值相同的UI组件被框架忽略，从而无法在父容器内显示。

- LazyForEach必须使用DataChangeListener对象来进行更新，第一个参数dataSource使用状态变量时，状态变量改变不会触发LazyForEach的UI刷新。

- 为了高性能渲染，通过DataChangeListener对象的onDataChange方法来更新UI时，需要生成不同于原来的键值来触发组件刷新。

- itemGenerator函数的调用顺序不一定和数据源中的数据项相同，在开发过程中不要假设itemGenerator和keyGenerator函数是否执行及其执行顺序。例如，以下示例可能无法正常运行：


  ```ts
  LazyForEach(dataSource, 
    (item: Object) => Text(`${item.i}. item.data.label`),
    (item: Object): string => item.data.id.toString())
  ```
