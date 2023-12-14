# \@Prop装饰器：父子单向同步


\@Prop装饰的变量可以和父组件建立单向的同步关系。\@Prop装饰的变量是可变的，但是变化不会同步回其父组件。


> **说明：**
>
> 从API version 9开始，该装饰器支持在ArkTS卡片中使用。


## 概述

\@Prop装饰的变量和父组件建立单向的同步关系：

- \@Prop变量允许在本地修改，但修改后的变化不会同步回父组件。

- 当数据源更改时，\@Prop装饰的变量都会更新，并且会覆盖本地所有更改。因此，数值的同步是父组件到子组件（所属组件)，子组件数值的变化不会同步到父组件。


## 装饰器使用规则说明

| \@Prop变量装饰器 | 说明                                       |
| ----------- | ---------------------------------------- |
| 装饰器参数       | 无                                        |
| 同步类型        | 单向同步：对父组件状态变量值的修改，将同步给子组件\@Prop装饰的变量，子组件\@Prop变量的修改不会同步到父组件的状态变量上。嵌套类型的场景请参考[观察变化](#观察变化)。 |
| 允许装饰的变量类型   | Objec、class、string、number、boolean、enum类型，以及这些类型的数组。<br/>不支持any，不支持简单类型和复杂类型的联合类型，不允许使用undefined和null。<br/>支持Date类型。<br/>支持类型的场景请参考[观察变化](#观察变化)。<br/>必须指定类型。<br/>**说明** ：<br/>不支持Length、ResourceStr、ResourceColor类型，Length，ResourceStr、ResourceColor为简单类型和复杂类型的联合类型。<br/>在父组件中，传递给\@Prop装饰的值不能为undefined或者null，反例如下所示。<br/>CompA&nbsp;({&nbsp;aProp:&nbsp;undefined&nbsp;})<br/>CompA&nbsp;({&nbsp;aProp:&nbsp;null&nbsp;})<br/>\@Prop和[数据源](arkts-state-management-overview.md#基本概念)类型需要相同，有以下三种情况：<br/>-&nbsp;\@Prop装饰的变量和\@State以及其他装饰器同步时双方的类型必须相同，示例请参考[父组件@State到子组件@Prop简单数据类型同步](#父组件state到子组件prop简单数据类型同步)。<br/>-&nbsp;\@Prop装饰的变量和\@State以及其他装饰器装饰的数组的项同步时 ，\@Prop的类型需要和\@State装饰的数组的数组项相同，比如\@Prop&nbsp;:&nbsp;T和\@State&nbsp;:&nbsp;Array&lt;T&gt;，示例请参考[父组件@State数组中的项到子组件@Prop简单数据类型同步](#父组件state数组项到子组件prop简单数据类型同步)；<br/>-&nbsp;当父组件状态变量为Object或者class时，\@Prop装饰的变量和父组件状态变量的属性类型相同，示例请参考[从父组件中的@State类对象属性到@Prop简单类型的同步](#从父组件中的state类对象属性到prop简单类型的同步)。 |
| 被装饰变量的初始值   | 允许本地初始化。                                 |


## 变量的传递/访问规则说明

| 传递/访问     | 说明                                       |
| --------- | ---------------------------------------- |
| 从父组件初始化   | 如果本地有初始化，则是可选的。没有的话，则必选，支持父组件中的常规变量、\@State、\@Link、\@Prop、\@Provide、\@Consume、\@ObjectLink、\@StorageLink、\@StorageProp、\@LocalStorageLink和\@LocalStorageProp去初始化子组件中的\@Prop变量。 |
| 用于初始化子组件  | \@Prop支持去初始化子组件中的常规变量、\@State、\@Link、\@Prop、\@Provide。 |
| 是否支持组件外访问 | \@Prop装饰的变量是私有的，只能在组件内访问。                |


  **图1** 初始化规则图示  


![zh-cn_image_0000001552972029](figures/zh-cn_image_0000001552972029.png)


## 观察变化和行为表现


### 观察变化

\@Prop装饰的数据可以观察到以下变化。

- 当装饰的类型是允许的类型，即Object、class、string、number、boolean、enum类型都可以观察到的赋值变化。

  ```ts
  // 简单类型
  @Prop count: number;
  // 赋值的变化可以被观察到
  this.count = 1;
  // 复杂类型
  @Prop count: Model;
  // 可以观察到赋值的变化
  this.title = new Model('Hi');
  ```

当装饰的类型是Object或者class复杂类型时，可以观察到第一层的属性的变化，属性即Object.keys(observedObject)返回的所有属性；

```
class ClassA {
  public value: string;
  constructor(value: string) {
    this.value = value;
  }
}
class Model {
  public value: string;
  public a: ClassA;
  constructor(value: string, a: ClassA) {
    this.value = value;
    this.a = a;
  }
}

@Prop title: Model;
// 可以观察到第一层的变化
this.title.value = 'Hi'
// 观察不到第二层的变化
this.title.a.value = 'ArkUi' 
```

对于嵌套场景，如果装饰的class是被\@Observed装饰的，可以观察到class属性的变化。

```
@Observed
class ClassA {
  public value: string;
  constructor(value: string) {
    this.value = value;
  }
}
class Model {
  public value: string;
  public a: ClassA;
  constructor(value: string, a: ClassA) {
    this.value = value;
    this.a = a;
  }
}
@Prop title: Model;
// 可以观察到第一层的变化
this.title.value = 'Hi'
// 可以观察到ClassA属性的变化，因为ClassA被@Observed装饰this.title.a.value = 'ArkUi'
```

当装饰的类型是数组的时候，可以观察到数组本身的赋值、添加、删除和更新。

```
// @State装饰的对象为数组时
@Prop title: string[]
// 数组自身的赋值可以观察到
this.title = ['1']
// 数组项的赋值可以观察到
this.title[0] = '2'
// 删除数组项可以观察到
this.title.pop()
// 新增数组项可以观察到
this.title.push('3')
```

对于\@State和\@Prop的同步场景：

- 使用父组件中\@State变量的值初始化子组件中的\@Prop变量。当\@State变量变化时，该变量值也会同步更新至\@Prop变量。
- \@Prop装饰的变量的修改不会影响其数据源\@State装饰变量的值。
- 除了\@State，数据源也可以用\@Link或\@Prop装饰，对\@Prop的同步机制是相同的。
- 数据源和\@Prop变量的类型需要相同，\@Prop允许简单类型和class类型。

- 当装饰的对象是Date时，可以观察到Date整体的赋值，同时可通过调用Date的接口`setFullYear`, `setMonth`, `setDate`, `setHours`, `setMinutes`, `setSeconds`, `setMilliseconds`, `setTime`, `setUTCFullYear`, `setUTCMonth`, `setUTCDate`, `setUTCHours`, `setUTCMinutes`, `setUTCSeconds`, `setUTCMilliseconds` 更新Date的属性。

```ts
@Component
struct DateComponent {
  @Prop selectedDate: Date;

  build() {
    Column() {
      Button('child update the new date')
        .margin(10)
        .onClick(() => {
          this.selectedDate = new Date('2023-09-09')
        })
      Button(`child increase the year by 1`).onClick(() => {
        this.selectedDate.setFullYear(this.selectedDate.getFullYear() + 1)
      })
      DatePicker({
        start: new Date('1970-1-1'),
        end: new Date('2100-1-1'),
        selected: this.selectedDate
      })
    }
  }
}

@Entry
@Component
struct ParentComponent {
  @State parentSelectedDate: Date = new Date('2021-08-08');

  build() {
    Column() {
      Button('parent update the new date')
        .margin(10)
        .onClick(() => {
          this.parentSelectedDate = new Date('2023-07-07')
        })
      Button('parent increase the day by 1')
        .margin(10)
        .onClick(() => {
          this.parentSelectedDate.setDate(this.parentSelectedDate.getDate() + 1)
        })
      DatePicker({
        start: new Date('1970-1-1'),
        end: new Date('2100-1-1'),
        selected: this.parentSelectedDate
      })

      DateComponent({selectedDate:this.parentSelectedDate})
    }

  }
}
```

### 框架行为

要理解\@Prop变量值初始化和更新机制，有必要了解父组件和拥有\@Prop变量的子组件初始渲染和更新流程。

1. 初始渲染：
   1. 执行父组件的build()函数将创建子组件的新实例，将数据源传递给子组件；
   2. 初始化子组件\@Prop装饰的变量。

2. 更新：
   1. 子组件\@Prop更新时，更新仅停留在当前子组件，不会同步回父组件；
   2. 当父组件的数据源更新时，子组件的\@Prop装饰的变量将被来自父组件的数据源重置，所有\@Prop装饰的本地的修改将被父组件的更新覆盖。


## 使用场景


### 父组件\@State到子组件\@Prop简单数据类型同步


以下示例是\@State到子组件\@Prop简单数据同步，父组件ParentComponent的状态变量countDownStartValue初始化子组件CountDownComponent中\@Prop装饰的count，点击“Try again”，count的修改仅保留在CountDownComponent 不会同步给父组件CountDownComponent。


ParentComponent的状态变量countDownStartValue的变化将重置CountDownComponent的count。



```ts
@Component
struct CountDownComponent {
  @Prop count: number;
  costOfOneAttempt: number = 1;

  build() {
    Column() {
      if (this.count > 0) {
        Text(`You have ${this.count} Nuggets left`)
      } else {
        Text('Game over!')
      }
      // @Prop装饰的变量不会同步给父组件
      Button(`Try again`).onClick(() => {
        this.count -= this.costOfOneAttempt;
      })
    }
  }
}

@Entry
@Component
struct ParentComponent {
  @State countDownStartValue: number = 10;

  build() {
    Column() {
      Text(`Grant ${this.countDownStartValue} nuggets to play.`)
      // 父组件的数据源的修改会同步给子组件
      Button(`+1 - Nuggets in New Game`).onClick(() => {
        this.countDownStartValue += 1;
      })
      // 父组件的修改会同步给子组件
      Button(`-1  - Nuggets in New Game`).onClick(() => {
        this.countDownStartValue -= 1;
      })

      CountDownComponent({ count: this.countDownStartValue, costOfOneAttempt: 2 })
    }
  }
}
```


在上面的示例中：


1. CountDownComponent子组件首次创建时其\@Prop装饰的count变量将从父组件\@State装饰的countDownStartValue变量初始化；

2. 按“+1”或“-1”按钮时，父组件的\@State装饰的countDownStartValue值会变化，这将触发父组件重新渲染，在父组件重新渲染过程中会刷新使用countDownStartValue状态变量的UI组件并单向同步更新CountDownComponent子组件中的count值；

3. 更新count状态变量值也会触发CountDownComponent的重新渲染，在重新渲染过程中，评估使用count状态变量的if语句条件（this.count &gt; 0），并执行true分支中的使用count状态变量的UI组件相关描述来更新Text组件的UI显示；

4. 当按下子组件CountDownComponent的“Try again”按钮时，其\@Prop变量count将被更改，但是count值的更改不会影响父组件的countDownStartValue值；

5. 父组件的countDownStartValue值会变化时，父组件的修改将覆盖掉子组件CountDownComponent中count本地的修改。


### 父组件\@State数组项到子组件\@Prop简单数据类型同步


父组件中\@State如果装饰的数组，其数组项也可以初始化\@Prop。以下示例中父组件Index中\@State装饰的数组arr，将其数组项初始化子组件Child中\@Prop装饰的value。



```ts
@Component
struct Child {
  @Prop value: number;

  build() {
    Text(`${this.value}`)
      .fontSize(50)
      .onClick(()=>{this.value++})
  }
}

@Entry
@Component
struct Index {
  @State arr: number[] = [1,2,3];

  build() {
    Row() {
      Column() {
        Child({value: this.arr[0]})
        Child({value: this.arr[1]})
        Child({value: this.arr[2]})

        Divider().height(5)

        ForEach(this.arr, 
          item => {
            Child({value: item})
          }, 
          item => item.toString()
        )
        Text('replace entire arr')
        .fontSize(50)
        .onClick(()=>{
          // 两个数组都包含项“3”。
          this.arr = this.arr[0] == 1 ? [3,4,5] : [1,2,3];
        })
      }
    }
  }
}
```


初始渲染创建6个子组件实例，每个\@Prop装饰的变量初始化都在本地拷贝了一份数组项。子组件onclick事件处理程序会更改局部变量值。


假设我们点击了多次，所有变量的本地取值都是“7”。



```
7
7
7
----
7
7
7
```


单击replace entire arr后，屏幕将显示以下信息。



```
3
4
5
----
7
4
5
```


- 在子组件Child中做的所有的修改都不会同步回父组件Index组件，所以即使6个组件显示都为7，但在父组件Index中，this.arr保存的值依旧是[1,2,3]。

- 点击replace entire arr，this.arr[0] == 1成立，将this.arr赋值为[3, 4, 5]；

- 因为this.arr[0]已更改，Child({value: this.arr[0]})组件将this.arr[0]更新同步到实例\@Prop装饰的变量。Child({value: this.arr[1]})和Child({value: this.arr[2]})的情况也类似。


- this.arr的更改触发ForEach更新，this.arr更新的前后都有数值为3的数组项：[3, 4, 5] 和[1, 2, 3]。根据diff机制，数组项“3”将被保留，删除“1”和“2”的数组项，添加为“4”和“5”的数组项。这就意味着，数组项“3”的组件不会重新生成，而是将其移动到第一位。所以“3”对应的组件不会更新，此时“3”对应的组件数值为“7”，ForEach最终的渲染结果是“7”，“4”，“5”。


### 从父组件中的\@State类对象属性到\@Prop简单类型的同步

如果图书馆有一本图书和两位用户，每位用户都可以将图书标记为已读，此标记行为不会影响其它读者用户。从代码角度讲，对\@Prop图书对象的本地更改不会同步给图书馆组件中的\@State图书对象。

在此示例中，图书类可以使用\@Observed装饰器，但不是必须的，只有在嵌套结构时需要此装饰器。这一点我们会在[从父组件中的\@State数组项到\@Prop class类型的同步](#从父组件中的\@State数组项到\@Prop class类型的同步)说明。


```ts
class Book {
  public title: string;
  public pages: number;
  public readIt: boolean = false;

  constructor(title: string, pages: number) {
    this.title = title;
    this.pages = pages;
  }
}

@Component
struct ReaderComp {
  @Prop book: Book;

  build() {
    Row() {
      Text(this.book.title)
      Text(`...has${this.book.pages} pages!`)
      Text(`...${this.book.readIt ? "I have read" : 'I have not read it'}`)
        .onClick(() => this.book.readIt = true)
    }
  }
}

@Entry
@Component
struct Library {
  @State book: Book = new Book('100 secrets of C++', 765);

  build() {
    Column() {
      ReaderComp({ book: this.book })
      ReaderComp({ book: this.book })
    }
  }
}
```

### 从父组件中的\@State数组项到\@Prop class类型的同步

在下面的示例中，更改了\@State 修饰的allBooks数组中Book对象上的属性，但点击“Mark read for everyone”无反应。这是因为该属性是第二层的嵌套属性，\@State装饰器只能观察到第一层属性，不会观察到此属性更改，所以框架不会更新ReaderComp。

```
let nextId: number = 1;

// @Observed
class Book {
  public id: number;
  public title: string;
  public pages: number;
  public readIt: boolean = false;

  constructor(title: string, pages: number) {
    this.id = nextId++;
    this.title = title;
    this.pages = pages;
  }
}

@Component
struct ReaderComp {
  @Prop book: Book;

  build() {
    Row() {
      Text(this.book.title)
      Text(`...has${this.book.pages} pages!`)
      Text(`...${this.book.readIt ? "I have read" : 'I have not read it'}`)
        .onClick(() => this.book.readIt = true)
    }
  }
}

@Entry
@Component
struct Library {
  @State allBooks: Book[] = [new Book("100 secrets of C++", 765), new Book("Effective C++", 651), new Book("The C++ programming language", 1765)];

  build() {
    Column() {
      Text('library`s all time favorite')
      ReaderComp({ book: this.allBooks[2] })
      Divider()
      Text('Books on loaan to a reader')
      ForEach(this.allBooks, book => {
        ReaderComp({ book: book })
      },
        book => book.id)
      Button('Add new')
        .onClick(() => {
          this.allBooks.push(new Book("The C++ Standard Library", 512));
        })
      Button('Remove first book')
        .onClick(() => {
          this.allBooks.shift();
        })
      Button("Mark read for everyone")
        .onClick(() => {
          this.allBooks.forEach((book) => book.readIt = true)
        })
    }
  }
}
```

 需要使用\@Observed装饰class Book，Book的属性将被观察。 需要注意的是，\@Prop在子组件装饰的状态变量和父组件的数据源是单向同步关系，即ReaderComp中的\@Prop book的修改不会同步给父组件Library。而父组件只会在数值有更新的时候（和上一次状态的对比），才会触发UI的重新渲染。

```
@Observed
class Book {
  public id: number;
  public title: string;
  public pages: number;
  public readIt: boolean = false;

  constructor(title: string, pages: number) {
    this.id = nextId++;
    this.title = title;
    this.pages = pages;
  }
}
```

\@Observed装饰的类的实例会被不透明的代理对象包装，此代理可以检测到包装对象内的所有属性更改。如果发生这种情况，此时，代理通知\@Prop，\@Prop对象值被更新。

### \@Prop本地初始化不和父组件同步

为了支持\@Component装饰的组件复用场景，\@Prop支持本地初始化，这样可以让\@Prop是否与父组件建立同步关系变得可选。当且仅当\@Prop有本地初始化时，从父组件向子组件传递\@Prop的数据源才是可选的。

下面的示例中，子组件包含两个\@Prop变量：

- \@Prop customCounter没有本地初始化，所以需要父组件提供数据源去初始化\@Prop，并当父组件的数据源变化时，\@Prop也将被更新；

- \@Prop customCounter2有本地初始化，在这种情况下，\@Prop依旧允许但非强制父组件同步数据源给\@Prop。


```ts
@Component
struct MyComponent {
  @Prop customCounter: number;
  @Prop customCounter2: number = 5;

  build() {
    Column() {
      Row() {
        Text(`From Main: ${this.customCounter}`).width(90).height(40).fontColor('#FF0010')
      }

      Row() {
        Button('Click to change locally !').width(480).height(60).margin({ top: 10 })
          .onClick(() => {
            this.customCounter2++
          })
      }.height(100).width(480)

      Row() {
        Text(`Custom Local: ${this.customCounter2}`).width(90).height(40).fontColor('#FF0010')
      }
    }
  }
}

@Entry
@Component
struct MainProgram {
  @State mainCounter: number = 10;

  build() {
    Column() {
      Row() {
        Column() {
          Button('Click to change number').width(480).height(60).margin({ top: 10, bottom: 10 })
            .onClick(() => {
              this.mainCounter++
            })
        }
      }

      Row() {
        Column()
        // customCounter必须从父组件初始化，因为MyComponent的customCounter成员变量缺少本地初始化；此处，customCounter2可以不做初始化。
        MyComponent({ customCounter: this.mainCounter })
        // customCounter2也可以从父组件初始化，父组件初始化的值会覆盖子组件customCounter2的本地初始化的值
        MyComponent({ customCounter: this.mainCounter, customCounter2: this.mainCounter })
      }.width('40%')
    }
  }
}
```
<!--no_check-->